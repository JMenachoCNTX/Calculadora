# Calculadora de Precio de Venta de Vehículos 🇧🇴

Sistema web para determinar el **precio final de venta de vehículos importados en Bolivia**, incluyendo todos los impuestos (IVA, IT e IUE), de modo que quede el **margen neto** deseado; con datos y fotos del vehículo, historial/inventario, **cálculo interno** para el personal y **proforma** para el cliente.

- **Sistema:** NUMMEROS by Contax (marca del software).
- **Empresa que importa y vende:** JUNKERS (aparece en los documentos y es configurable).

Funciona 100 % en el navegador, sin servidores ni bases de datos: los datos se guardan localmente en el dispositivo. Adaptado a modo claro y oscuro.

![Vista previa del sistema](assets/vista-previa.png)

---

## 🚀 Demo / Uso rápido

1. Descargá o cloná este repositorio.
2. Abrí el archivo **`index.html`** con cualquier navegador (Chrome, Edge, Safari, Firefox).
3. Ingresá los datos de importación y el margen deseado. Todo se recalcula automáticamente.

> También podés publicarlo en internet con un link propio: ver la sección [Publicar en GitHub Pages](#-publicar-en-github-pages).

---

## 🧮 ¿Qué calcula?

Partiendo del **costo neto** (sin IVA) del vehículo importado, la calculadora despeja el precio de venta con esta fórmula:

```
P = C / (0.87 − 0.03 − M / 0.75)
```

| Término | Significado |
|--------|-------------|
| `P` | Precio de venta |
| `C` | Costo total neto (sin IVA) |
| `0.87` | 1 − IVA (13 %) → débito fiscal |
| `0.03` | IT (Impuesto a las Transacciones) |
| `M / 0.75` | Ajuste por IUE (25 %), para que el margen `M` quede **neto** |

A partir del precio obtenido calcula automáticamente:

- **IVA – Débito Fiscal** (13 % del precio de venta)
- **IT** (3 % del precio de venta)
- **Utilidad Bruta**
- **IUE** (25 % de la utilidad bruta)
- **Utilidad Neta** y el **margen real verificado** (que debe coincidir con el margen deseado)
- **Precio de venta en USD** (referencial, al T.C. ingresado)

## ✨ Funciones

- **Pantalla de Inicio limpia:** solo el logo de la empresa (JUNKERS por defecto), grande y centrado; adapta su color al tema.
- **Diseño en blanco y negro** (paleta NUMMEROS), compacto, con **íconos vectoriales** (sin emojis) y **pie de página fijo abajo**, adaptado a celular, tablet y computadora.
- **Datos del vehículo / cotización:** Nº correlativo automático (`COT-AAAA-0001`), fecha, cliente, marca, modelo, versión/grado, tipo, año, color, condición, placa y chasis/VIN.
- **Especificaciones técnicas:** motor/cilindrada, potencia, combustible, transmisión, tracción, nº de puertas, pasajeros, kilometraje, procedencia, garantía y entrega.
- **Equipamiento / accesorios:** lista libre (una por línea) que se muestra en la proforma.
- **Fotos del vehículo (galería):** subí varias fotos por vehículo; se redimensionan automáticamente para no llenar el navegador. La primera es la principal y aparece en la proforma; en el historial se ve como miniatura (útil como catálogo/inventario).
- **Dos documentos:**
  - **🧾 Cálculo interno** (para el personal): costos, impuestos, utilidades y margen, con el aviso "Documento interno — no entregar al cliente".
  - **📄 Proforma** (para el cliente): estilo cotización comercial — precio de lista, descuento especial, precio facturado, validez de la oferta, qué incluye el precio y fotos. **No** muestra costos ni margen.
- **Historial / inventario:** guardá cada cálculo y consultalo cuando quieras. Por fila: **Ver**, **Proforma**, **Cálculo**, **Duplicar** y **Eliminar** (con deshacer). Buscador por placa, marca, modelo, cliente o número.
- **Guardado local:** todo se conserva en el navegador (localStorage), así que no se pierde al cerrar. Podés **exportar** e **importar** el historial en `.json` como respaldo o para pasarlo a otro equipo.
- **⚙️ Configuración de la empresa (JUNKERS):** logo, nombre/razón social, NIT, dirección, teléfono, y textos de la proforma (presentación, qué incluye, financiamiento, validez por defecto). Aparecen en los documentos.
- **Precio en USD** referencial y **aviso** si el margen es demasiado alto (evita un precio inválido).

## ☁️ Sincronización en línea (tiempo real)

El sistema puede trabajar **en línea con Firebase (Firestore)**: lo que una persona guarda o edita lo ven las demás **al instante**. Sin sincronizar, funciona igual en modo local (los datos se guardan en el navegador).

Para activarlo, seguí la guía **[`SETUP-FIREBASE.md`](SETUP-FIREBASE.md)** (pasos en la consola de Firebase) y pegá tu configuración en **⚙ Configuración → Sincronización en línea**. El indicador arriba a la derecha muestra **● En línea** cuando está sincronizado. Reglas de Firestore incluidas en `firestore.rules`.

## 🖨️ Imprimir / PDF

Tanto el **cálculo interno** como la **proforma** abren el diálogo de impresión del navegador. Para obtener un PDF, elegí destino **"Guardar como PDF"**. El pie de página queda al fondo de cada hoja.

### Impuestos aplicados (Bolivia)

| Impuesto | Tasa | Base |
|----------|------|------|
| IVA (Débito Fiscal) | 13 % | Precio de venta |
| IT | 3 % | Precio de venta |
| IUE | 25 % | Utilidad bruta |

---

## 📥 Campos editables vs. calculados

Para evitar que se borren fórmulas por accidente, solo son editables los **datos de entrada**:

**Editables:** T.C., CIF (USD), IVA Aduana (%), ICE, Detalle de Portes, Costo Importadora, Otros Costos, IMPVAT / costo fijo (con o sin factura) y el Margen deseado.

> **IMPVAT / costo fijo adicional:** se suma al costo **C** sin alterar la fórmula del precio. Si eligés *"sin factura"*, el 100 % va al costo; si eligés *"con factura (IVA)"*, el 87 % va al costo (gasto) y el 13 % al crédito fiscal. El margen neto se mantiene exacto.

**Calculados (solo lectura):** CIF (BOB), IVA Aduana, Costo Total de Importación, Costo Neto, Crédito Fiscal, Precio de Venta, IVA DF, IT, Utilidad Bruta, IUE, Utilidad Neta y margen real.

---

## ✅ Ejemplo verificado

Con los valores por defecto (los mismos del Excel original):

| Concepto | Valor |
|----------|-------|
| Costo Neto | Bs 370.129,65 |
| **Precio de Venta** | **Bs 523.768,37** |
| IVA (DF) | Bs 68.089,89 |
| IT | Bs 15.713,05 |
| IUE | Bs 17.458,95 |
| Utilidad Neta | Bs 52.376,84 |
| Margen real | 10,00 % |

---

## 🌐 Publicar en GitHub Pages

1. Subí este repositorio a tu cuenta de GitHub.
2. Andá a **Settings → Pages**.
3. En **Source**, elegí la rama `main` y la carpeta `/ (root)`.
4. Guardá. En unos minutos tendrás un enlace público del tipo:
   `https://TU-USUARIO.github.io/calculadora-precio-vehiculos/`

---

## 📁 Estructura del proyecto

```
calculadora-precio-vehiculos/
├── index.html          # El sistema completo (HTML + CSS + JS, con los logos incrustados)
├── README.md           # Este archivo
├── SETUP-FIREBASE.md   # Guía para activar la sincronización en línea
├── firestore.rules     # Reglas de seguridad de Firestore
├── LICENSE             # Licencia propietaria (uso interno)
├── .gitignore
└── assets/         # Logos NUMMEROS by Contax (SVG) y vista previa
    ├── nummeros-by-contax-negro.svg
    ├── nummeros-by-contax-blanco.svg
    ├── nummeros-negro.svg
    ├── junkers-logo.svg
    ├── nummeros-blanco.svg
    └── vista-previa.png
```

> Los logos ya están **incrustados** dentro de `index.html` (como imágenes en base64), así que la página funciona sola aunque muevas la carpeta `assets/`. Esos archivos SVG se incluyen aparte por si los necesitás para otros usos de la marca.

---

## 📝 Notas

- Las tasas de impuestos (IVA 13 %, IT 3 %, IUE 25 %) están fijas en el código, conforme a la normativa boliviana vigente. Si cambian, se ajustan en las constantes `IVA`, `IT` e `IUE` dentro de `index.html`.
- El ICE de aduana se incluye en el Costo Total de Importación pero, tal como en el Excel original, no forma parte de la base neta (Factura de Compra) usada para el precio de venta.
- No se envía ningún dato a internet: todo el cálculo ocurre en tu navegador.

---

## 📄 Licencia

Software **propietario / de uso interno**. © 2026 JUNKERS — Todos los derechos reservados. Sistema desarrollado por NUMMEROS by Contax. Ver [`LICENSE`](LICENSE).
