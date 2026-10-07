# PichónMan Online — Gen 5 Doubles OU

Esta versión usa:

- GitHub Pages para publicar la web.
- Firebase Realtime Database para sincronizar los resultados en tiempo real.
- Firebase Authentication (email/contraseña) para que solo el organizador pueda editar.
- Relleno1 y Relleno2 están configurados como reservas.
- 14 titulares → 2 grupos de 7 → 21 encuentros por grupo → Top 4 → playoffs.

## 1. Crear Firebase

1. Entra en https://console.firebase.google.com/
2. Crea un proyecto nuevo.
3. En el proyecto, añade una aplicación Web (`</>`).
4. Copia la configuración que Firebase te muestra.
5. En `index.html`, sustituye el objeto `firebaseConfig` por esa configuración.

## 2. Activar Realtime Database

Firebase Console → Build → Realtime Database → Create Database.

Recomendado: elegir una región europea si está disponible para tu proyecto.

Después ve a la pestaña Rules y usa estas reglas:

{
  "rules": {
    "tournament": {
      ".read": true,
      ".write": "auth != null"
    }
  }
}

IMPORTANTE: estas reglas permiten que cualquier usuario autenticado escriba. No actives registro público; crea únicamente tu cuenta de administrador en Authentication.

## 3. Activar Authentication

Firebase Console → Build → Authentication → Get started.

Activa solamente:

Authentication → Sign-in method → Email/Password.

Después crea tu usuario administrador desde la sección de usuarios.

No hace falta que los jugadores tengan cuenta.

## 4. Configurar index.html

Busca:

const firebaseConfig = {
  apiKey: "PEGA_AQUI_API_KEY",
  ...
};

Pega exactamente el objeto que te da Firebase.

La `databaseURL` debe ser la URL de tu Realtime Database.

## 5. Publicarlo con GitHub Pages

Crea un repositorio público en GitHub, por ejemplo:

pichonman-gen5

Sube `index.html` a la raíz del repositorio.

Después:

Settings → Pages → Build and deployment → Deploy from a branch → main → / (root) → Save.

GitHub Pages publicará una URL del estilo:

https://TUUSUARIO.github.io/pichonman-gen5/

GitHub indica que Pages puede publicar sitios estáticos directamente desde un repositorio. Los cambios pueden tardar unos minutos en aparecer.

## 6. Cómo funciona

Los jugadores entran a la URL pública y solo pueden ver:

- grupos
- clasificación
- resultados
- playoffs
- campeón

El organizador entra en:

Administrador → email + contraseña

y puede introducir los resultados.

Cuando se modifica un resultado, Firebase Realtime Database lo distribuye a los clientes conectados automáticamente.

## Seguridad

No pongas una contraseña de administrador dentro del HTML.

La contraseña se gestiona mediante Firebase Authentication.

La configuración `firebaseConfig` NO es una contraseña ni un secreto; las reglas de Firebase son las que deben impedir escrituras no autorizadas.

Para un torneo pequeño como este, Realtime Database es especialmente adecuado porque sincroniza los datos JSON entre clientes conectados en tiempo real.
