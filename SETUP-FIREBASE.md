# Sincronización en línea con Firebase (tiempo real)

Con esto, **lo que una persona guarde o edite lo ven las demás al instante**. El sistema usa **Firestore** (base de datos en tiempo real de Firebase). Es gratis para el volumen de un concesionario (plan Spark).

Todo se sigue guardando también **localmente** en cada navegador como respaldo, así que si se cae internet, podés seguir trabajando y se vuelve a sincronizar al reconectar.

> **Modo de acceso elegido:** sin login. Cualquiera con el enlace del sistema puede ver y editar. Mantené el enlace solo entre tu equipo. (Si más adelante querés acceso con usuario y contraseña, avisá y se cambia.)

---

## Paso 1 · Crear (o abrir) el proyecto en Firebase

1. Entrá a **https://console.firebase.google.com** con tu cuenta.
2. Tocá **Agregar proyecto** (o abrí uno que ya tengas). Ponele un nombre, p. ej. `junkers-cotizaciones`.
3. Podés desactivar Google Analytics (no hace falta). Creá el proyecto.

## Paso 2 · Crear la app Web y copiar la configuración

1. En la pantalla del proyecto, tocá el ícono **`</>`** (Web) para "Agregar una app web".
2. Ponele un apodo (p. ej. `sistema`) y **Registrar app**. No hace falta Hosting.
3. Firebase te muestra un bloque con `const firebaseConfig = { ... }`. **Copiá solo el objeto** `{ ... }` (desde la llave que abre hasta la que cierra). Se ve así:

```js
const firebaseConfig = {
  apiKey: "AIza...............",
  authDomain: "junkers-cotizaciones.firebaseapp.com",
  projectId: "junkers-cotizaciones",
  storageBucket: "junkers-cotizaciones.appspot.com",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:abcdef123456"
};
```

Guardá ese texto, lo vas a pegar en el Paso 5.

## Paso 3 · Activar Firestore

1. En el menú izquierdo: **Compilación → Firestore Database**.
2. Tocá **Crear base de datos**.
3. Elegí **Iniciar en modo de producción** (las reglas las ponemos en el Paso 4).
4. Elegí una ubicación (p. ej. `southamerica-east1` o `us-central`). **Habilitar**.

## Paso 4 · Activar el acceso anónimo y poner las reglas

**4a. Acceso anónimo** (para que no haga falta contraseña pero igual haya un mínimo de control):

1. Menú izquierdo: **Compilación → Authentication → Comenzar**.
2. Pestaña **Sign-in method** → en la lista, elegí **Anónimo** → **Habilitar** → **Guardar**.

**4b. Reglas de Firestore:**

1. Andá a **Firestore Database → pestaña Reglas**.
2. Borrá lo que haya y pegá exactamente esto:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Solo usuarios autenticados (el acceso anónimo cuenta). Bloquea accesos sueltos.
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

3. Tocá **Publicar**.

> El archivo `firestore.rules` de este proyecto tiene lo mismo, por si preferís usarlo con la CLI.

## Paso 5 · Conectar el sistema

1. Abrí el sistema (el `index.html`) en el navegador.
2. Andá a la pestaña **⚙ Configuración → Sincronización en línea (Firebase)**.
3. **Pegá** en el recuadro el objeto `firebaseConfig` que copiaste en el Paso 2.
4. Tocá **Conectar y sincronizar**.
5. Arriba a la derecha vas a ver **● En línea** y el estado dirá *"En línea · sincronizado"*.

¡Listo! Desde ahora, cada cotización que guardes, edites o elimines se replica en tiempo real a todas las personas que tengan el sistema abierto.

## Paso 6 · Compartir con tu equipo

- Publicá el `index.html` (por ejemplo con **GitHub Pages**, ver `README.md`) y pasá ese **enlace** a tu equipo.
- La primera vez que cada persona abra el enlace, entra en **Configuración**, pega la misma configuración de Firebase y toca **Conectar**. (Se guarda en su navegador, no hay que repetirlo cada vez.)
- **Importante:** la primera persona que conecte debe ser la que ya tiene las cotizaciones cargadas — así se suben al servidor y las demás las reciben. Si dos empiezan de cero, no pasa nada, se van sumando.

---

## Preguntas frecuentes / problemas

- **Dice "Sin acceso anónimo":** faltó el Paso 4a (habilitar Anónimo en Authentication).
- **Dice "Error de permisos en Firestore":** revisá que publicaste las reglas del Paso 4b.
- **No conecta / "No se pudo cargar Firebase":** puede ser falta de internet o un bloqueador. Probá en otro navegador o red. El sistema sigue funcionando en modo local mientras tanto.
- **Las fotos muy pesadas:** Firestore permite hasta 1 MB por cotización. Si subís muchas fotos grandes, el sistema avisa y sincroniza las que entran (las demás quedan en tu dispositivo). Para catálogos con muchas fotos conviene, más adelante, usar Firebase Storage (se puede agregar).
- **Quiero desconectar la sincronización:** Configuración → **Desconectar**. Vuelve a modo local.
- **Costo:** el plan gratuito (Spark) de Firebase alcanza de sobra para el uso de un concesionario. Revisá los límites en la consola.

## ¿Y si quiero acceso con usuario y contraseña?

Se puede cambiar a **login con email y contraseña** (cada persona con su cuenta, y vos las creás en Authentication). Avisá y se ajusta el sistema y las reglas.
