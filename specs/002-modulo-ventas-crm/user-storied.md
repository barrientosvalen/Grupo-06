# Modelo de Datos (MER) - Módulo de Ventas y CRM

## 1. Descripción General
Este documento define la estructura de la base de datos para el **Módulo de Ventas y CRM**. Administra el ciclo de vida completo de la venta: gestión de clientes (leads/contactos), cotizaciones/oportunidades, registro de ventas de productos/servicios y seguimiento postventa.

---

## 2. Diagrama Entidad-Relación (MER)

```mermaid
erDiagram
    CLIENTE ||--o{ OPORTUNIDAD : "inicia"
    CLIENTE ||--o{ VENTA : "realiza"
    USUARIO ||--o{ OPORTUNIDAD : "gestiona"
    USUARIO ||--o{ VENTA : "registra"
    
    OPORTUNIDAD ||--o{ DETALLE_OPORTUNIDAD : "contiene"
    PRODUCTO ||--o{ DETALLE_OPORTUNIDAD : "es cotizado en"
    
    VENTA ||--o{ DETALLE_VENTA : "contiene"
    PRODUCTO ||--o{ DETALLE_VENTA : "es vendido en"
    
    VENTA ||--o{ PAGO : "recibe"

    CLIENTE {
        int id_cliente PK
        string nombre
        string email
        string telefono
        string direccion
        string tipo_cliente "POTENCIAL / FINAL / EMPRESA"
        datetime fecha_registro
    }

    OPORTUNIDAD {
        int id_oportunidad PK
        int id_cliente FK
        int id_usuario FK
        string estado "NUEVA / NEGOCIACION / GANADA / PERDIDA"
        decimal monto_estimado
        datetime fecha_cierre_estimada
        datetime fecha_creacion
    }

    PRODUCTO {
        int id_producto PK
        string codigo_sku
        string nombre
        decimal precio_unitario
        int stock_actual
        boolean activo
    }

    DETALLE_OPORTUNIDAD {
        int id_detalle_op PK
        int id_oportunidad FK
        int id_producto FK
        int cantidad
        decimal precio_oferta
    }

    VENTA {
        int id_venta PK
        int id_cliente FK
        int id_usuario FK
        int id_oportunidad FK "Opcional"
        datetime fecha_venta
        decimal subtotal
        decimal impuesto
        decimal total
        string estado "PENDIENTE / COMPLETADA / ANULADA"
    }

    DETALLE_VENTA {
        int id_detalle_venta PK
        int id_venta FK
        int id_producto FK
        int cantidad
        decimal precio_unitario
        decimal subtotal
    }

    PAGO {
        int id_pago PK
        int id_venta FK
        datetime fecha_pago
        decimal monto
        string metodo_pago "EFECTIVO / TARJETA / TRANSFERENCIA"
        string referencia
    }
