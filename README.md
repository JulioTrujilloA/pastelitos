# Pastelitos

Negocio de repostería: una landing pública con tienda y un panel interno de control.

## Archivos

- `src/index.html` — landing pública con catálogo y carrito. El pedido se confirma por WhatsApp
  (`https://wa.me/...` con el mensaje ya redactado). Sin backend: catálogo fijo en el archivo,
  carrito y datos del formulario en `localStorage`. Enlace discreto al panel en el footer.
- `src/admin.html` — panel interno de control (compras, ventas, gastos, corte, resumen, productos).
  Usa Supabase.

## Uso

Abre el archivo que quieras en el navegador, o sirve la carpeta:

```
cd src && python -m http.server 4180
```

Luego `http://localhost:4180/` (landing) o `http://localhost:4180/admin.html` (panel).

## La tienda (`src/index.html`)

### Cómo funciona el pedido

1. El cliente arma el carrito desde el menú (control de cantidad por producto).
2. Abre el carrito, llena nombre, fecha de entrega y notas.
3. "Confirmar por WhatsApp" abre un chat hacia `WHATSAPP_NUMBER` con el pedido redactado
   (líneas, total, nombre, fecha, notas). No se cobra en línea: pago al recoger o por transferencia.

### Reglas

- **Anticipación mínima: 7 días** desde hoy para la fecha de entrega (los pasteles son hechos a
  mano). Se controla con la constante `DIAS_ANTICIPACION`. El selector nativo bloquea fechas
  anteriores y `checkout()` también las rechaza si se teclean a mano.
- **Persistencia** (`localStorage`, por navegador/dispositivo, no se envía a ningún lado):
  - Carrito → clave `pastelitos_cart_v1`.
  - Formulario (nombre / fecha / notas) → clave `pastelitos_form_v1`. Al recargar se restaura;
    la fecha solo vuelve si sigue siendo válida (≥ el mínimo actual), si no queda vacía.
- El mensaje de WhatsApp va sin emojis a propósito: el texto pre-cargado de `wa.me` los renderiza
  mal en algunos clientes.

### Por configurar

En `src/index.html`, arriba del `<script>`:

- `WHATSAPP_NUMBER` — número con lada de país, sin `+`, espacios ni guiones (ej. `5215512345678`).
  Mientras siga como `52XXXXXXXXXX`, el botón muestra un aviso y no abre WhatsApp.
- `NEGOCIO` — nombre que aparece en el saludo del mensaje.
- `PRODUCTS` — catálogo (emoji, nombre, precio, descripción, `tag` opcional).
- Textos del `<footer>`: WhatsApp visible, horario y zona de entrega.
