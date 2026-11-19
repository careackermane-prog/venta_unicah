# Ventas UNICAH — Odoo 20.0

Módulo personalizado para ampliar Ventas sin modificar el código original de Odoo.

## Funcionalidades

- Agrega **Canal preferido** al contacto: Tienda, Teléfono, Sitio web y Distribuidor.
- Copia el canal preferido del cliente al pedido como **Canal de venta** (editable en el pedido).
- Calcula **Total de unidades** sumando las cantidades de las líneas.
- Marca pedidos superiores a 50,000 para aprobación.
- Impide confirmar pedidos que requieren aprobación mientras no hayan sido aprobados.
- Permite aprobar únicamente a usuarios del grupo Administrador de ventas (`sales_team.group_sale_manager`).
- Agrega columnas, filtros y agrupación por canal en la lista de pedidos.

## Estructura

```text
ventas_unicah/
├── __init__.py
├── __manifest__.py
├── README.md
├── models/
│   ├── __init__.py
│   ├── res_partner.py
│   └── sale_order.py
└── views/
    ├── res_partner_views.xml
    └── sale_order_views.xml
```

## Instalación con Docker Compose

1. Copia la carpeta `ventas_unicah` dentro de la carpeta de addons personalizados que está montada en el contenedor de Odoo.
2. Verifica que esa carpeta esté incluida en `addons_path` dentro de la configuración de Odoo.
3. Verifica el nombre real del servicio Odoo en tu archivo `compose.yaml` o `docker-compose.yml`.
4. Desde la carpeta que contiene el archivo Compose, actualiza la lista de aplicaciones e instala `Ventas UNICAH`.

Ejemplo, si el servicio se llama `web` y el contenedor tiene el comando `odoo`:

```powershell
docker compose exec web odoo -d odoo_dev -u ventas_unicah --stop-after-init
```

Si el módulo aún no está instalado, usa `-i` en lugar de `-u`:

```powershell
docker compose exec web odoo -d odoo_dev -i ventas_unicah --stop-after-init
```

El nombre del servicio, la base de datos y la ruta de addons deben coincidir con tu configuración real. No ejecutes estos ejemplos sin verificarlos.

Después reinicia el servicio Odoo si tu despliegue lo requiere y abre la base de datos en el navegador.

## Pruebas de la práctica

1. Crear dos clientes: uno con canal Teléfono y otro con canal Tienda.
2. Crear un producto Laptop con precio de venta 30,000.
3. Crear una cotización para el cliente de canal Teléfono, agregar tres laptops y comprobar que el total de unidades sea 3.
4. Verificar que el total supera 50,000 y que el pedido requiere aprobación.
5. Comprobar que un usuario sin permisos de administrador de ventas no puede aprobar.
6. Aprobar con un administrador de ventas y confirmar el pedido.
7. Crear una cotización inferior o igual a 50,000 y comprobar que no requiere aprobación.

## Nota

El umbral de aprobación es estrictamente **mayor que 50,000**, tal como indica la guía. La carpeta de addons debe estar disponible dentro del contenedor; copiar el módulo únicamente al repositorio Git no hace que Odoo lo cargue automáticamente.
