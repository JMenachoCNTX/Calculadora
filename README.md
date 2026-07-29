# NUMMEROS by Contax — Calculadora de Precio de Venta de Vehículos 🇧🇴

Calculadora web para determinar el **precio final de venta de vehículos importados en Bolivia**, incluyendo todos los impuestos (IVA, IT e IUE), de modo que quede el **margen neto** deseado.

Funciona 100 % en el navegador, sin servidores ni bases de datos: solo abrís la página, ingresás los datos y ves los resultados en vivo. Ideal para publicar gratis en **GitHub Pages**.

Con la identidad visual de **NUMMEROS by Contax** (encabezado y pie con el logo, favicon incluido), adaptada automáticamente a modo claro y oscuro.

![Vista previa de la calculadora](assets/vista-previa.png)

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
- El **asiento contable** de la venta (Debe / Haber balanceado)

## ✨ Funciones

- **Datos del vehículo / cotización:** Nº de cotización correlativo automático (`COT-AAAA-0001`), fecha, cliente, placa, marca, modelo, año, color, chasis/VIN y comentarios.
- **Historial:** guardá cada cálculo y consultalo cuando quieras. Podés **ver** (recargar en la calculadora), **duplicar**, **imprimir** y **eliminar** (con opción de deshacer). Incluye buscador por placa, modelo, cliente o número.
- **Guardado local:** el historial se conserva en el navegador (localStorage), así que no se pierde al cerrar la página. Además podés **exportar** e **importar** el historial en un archivo `.json` como respaldo o para pasarlo a otro equipo.
- **Imprimir / PDF:** genera una cotización con el logo, los datos del vehículo y el desglose de impuestos. Desde el diálogo de impresión del navegador elegí “Guardar como PDF”.
- **Aviso de margen:** si el margen es demasiado alto para el modelo (denominador negativo), la calculadora te avisa en lugar de mostrar un número inválido.

### Impuestos aplicados (Bolivia)

| Impuesto | Tasa | Base |
|----------|------|------|
| IVA (Débito Fiscal) | 13 % | Precio de venta |
| IT | 3 % | Precio de venta |
| IUE | 25 % | Utilidad bruta |

---

## 📥 Campos editables vs. calculados

Para evitar que se borren fórmulas por accidente, solo son editables los **datos de entrada**:

**Editables:** T.C., CIF (USD), IVA Aduana (%), ICE, Detalle de Portes, Costo Importadora, Otros Costos y el Margen deseado.

**Calculados (solo lectura):** CIF (BOB), IVA Aduana, Costo Total de Importación, Costo Neto, Crédito Fiscal, Precio de Venta, IVA DF, IT, Utilidad Bruta, IUE, Utilidad Neta, margen real y el asiento contable.

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
├── index.html      # La calculadora (HTML + CSS + JS, con los logos ya incrustados)
├── README.md       # Este archivo
├── LICENSE         # Licencia MIT
├── .gitignore
└── assets/         # Logos NUMMEROS by Contax (SVG) y vista previa
    ├── nummeros-by-contax-negro.svg
    ├── nummeros-by-contax-blanco.svg
    ├── nummeros-negro.svg
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

Distribuido bajo la licencia MIT. Ver [`LICENSE`](LICENSE).
