# Porra privada Mundial 2026

Aplicacion web gratuita para una porra privada entre amigos del Mundial 2026.

Esta preparada con:

- React
- TypeScript
- Vite
- GitHub Pages
- Google Sheets
- Google Apps Script
- Diseno responsive mobile-first

La aplicacion no usa servidores de pago ni bases de datos de pago. Los datos viven en una hoja de calculo de Google Sheets.

---

## 1. Que incluye este proyecto

Carpetas y archivos principales:

- `src/`: aplicacion web en React.
- `apps-script/Code.gs`: codigo completo de Google Apps Script.
- `.env.example`: ejemplo de configuracion de la URL de la API.
- `vite.config.ts`: configuracion para Vite y GitHub Pages.
- `package.json`: dependencias y comandos del proyecto.

Funciones incluidas:

- Login privado con usuarios creados manualmente.
- Sesion guardada en el movil u ordenador hasta cerrar sesion.
- Lectura automatica de partidos desde Google Sheets.
- Pronosticos de local, empate o visitante.
- Bloqueo automatico al llegar la hora del partido.
- Resultados oficiales actualizados manualmente en Sheets.
- Ranking con puntos, aciertos y fallos.
- Pronosticos especiales enviados una sola vez.
- Panel principal con resumen, proximos partidos y clasificacion.

---

## 2. Instalar Node.js

Node.js es el programa que permite preparar y publicar la web.

1. Entra en [https://nodejs.org](https://nodejs.org).
2. Descarga la version LTS recomendada.
3. Instalala aceptando las opciones por defecto.
4. Cierra y vuelve a abrir PowerShell.
5. Comprueba que funciona:

```powershell
node --version
npm.cmd --version
```

Si `npm` da error en PowerShell, usa siempre `npm.cmd`, por ejemplo:

```powershell
npm.cmd install
```

---

## 3. Instalar las dependencias del proyecto

Abre PowerShell en la carpeta del proyecto:

```powershell
cd "C:\Users\Usuario\Documents\Porra"
```

Instala las dependencias:

```powershell
npm.cmd install
```

Arranca la aplicacion en local:

```powershell
npm.cmd run dev
```

Vite mostrara una URL parecida a:

```text
http://localhost:5173/
```

Abrela en el navegador.

Al principio puede aparecer un aviso de que falta configurar `VITE_API_URL`. Es normal hasta que publiques Google Apps Script.

---

## 4. Crear Google Sheets

1. Entra en [https://sheets.google.com](https://sheets.google.com).
2. Crea una hoja de calculo nueva.
3. Ponle un nombre, por ejemplo:

```text
Porra Mundial 2026
```

4. Crea estas pestanas exactas:

```text
Usuarios
Partidos
Pronosticos
Resultados
Especiales
```

El nombre debe coincidir exactamente, respetando mayusculas y sin espacios extra.

---

## 5. Configurar la pestana Usuarios

En la pestana `Usuarios`, pon estos encabezados en la fila 1:

```text
username | password | displayName
```

Ejemplo:

```text
juan   | 123456 | Juan
pedro  | abc123 | Pedro
maria  | mundial2026 | Maria
```

Notas:

- `username` es el usuario para iniciar sesion.
- `password` es la contrasena.
- `displayName` es el nombre que se vera en la clasificacion.
- No existe registro publico. Solo entran los usuarios que escribas aqui.

---

## 6. Configurar la pestana Partidos

En la pestana `Partidos`, pon estos encabezados:

```text
id | fase | grupo | fecha | local | visitante
```

Ejemplo:

```text
1 | Grupo | A | 2026-06-11T18:00:00 | Mexico | Canada
2 | Grupo | A | 2026-06-11T21:00:00 | Estados Unidos | Uruguay
3 | Final |   | 2026-07-19T21:00:00 | Ganador semifinal 1 | Ganador semifinal 2
```

Formato recomendado para `fecha`:

```text
AAAA-MM-DDTHH:mm:ss
```

Ejemplo:

```text
2026-06-11T18:00:00
```

La hora se usa para bloquear pronosticos. Cuando llegue esa hora, el partido ya no se podra modificar.

Fases soportadas:

- Grupo
- Dieciseisavos
- Octavos
- Cuartos
- Semifinales
- Tercer puesto
- Final

Puedes escribir el nombre de fase que quieras. La aplicacion agrupa automaticamente segun lo que exista en la hoja.

---

## 7. Configurar la pestana Pronosticos

En la pestana `Pronosticos`, pon estos encabezados:

```text
usuario | partidoId | pronostico | fechaRegistro
```

No rellenes esta hoja manualmente. La aplicacion guardara aqui los pronosticos.

Cada usuario solo puede tener un registro por partido. Una vez guardado, ya no podra cambiarlo desde la web. Si quieres permitir que alguien vote de nuevo, borra manualmente su fila de esta pestana `Pronosticos`.

La web permite marcar varios partidos y enviarlos todos juntos. Mientras no pulses `Enviar pronosticos`, la seleccion todavia no queda guardada y se puede cambiar.

Valores posibles de `pronostico`:

```text
local
empate
visitante
```

---

## 8. Configurar la pestana Resultados

En la pestana `Resultados`, pon estos encabezados:

```text
partidoId | resultado
```

Tu actualizaras esta hoja durante el Mundial.

Ejemplo:

```text
1 | local
2 | empate
3 | visitante
```

Valores posibles:

```text
local
empate
visitante
```

Cuando anadas resultados, la clasificacion se recalcula automaticamente.

---

## 9. Configurar la pestana Especiales

En la pestana `Especiales`, pon estos encabezados:

```text
usuario | mejorJugador | maximoGoleador | peorSeleccion | campeon | subcampeon | tercerClasificado | fechaRegistro
```

No hace falta rellenarla. La aplicacion guardara aqui los pronosticos especiales.

Cada usuario solo puede enviarlos una vez.

---

## 10. Crear Google Apps Script

1. En la hoja de calculo, ve a:

```text
Extensiones > Apps Script
```

2. Borra el codigo que aparezca.
3. Abre en este proyecto el archivo:

```text
apps-script/Code.gs
```

4. Copia todo su contenido.
5. Pegalo en Apps Script.
6. Pulsa el icono de guardar.
7. Ponle nombre al proyecto, por ejemplo:

```text
API Porra Mundial 2026
```

---

## 11. Probar y preparar las hojas desde Apps Script

En Apps Script:

1. En la parte superior, elige la funcion `setupSheets_`.
2. Pulsa `Ejecutar`.
3. Google pedira permisos.
4. Acepta con tu cuenta.
5. Si aparece una pantalla indicando que la app no esta verificada, pulsa:

```text
Configuracion avanzada > Ir a API Porra Mundial 2026
```

6. Acepta los permisos.

Esto prepara encabezados y pestanas si faltan.

---

## 12. Publicar Apps Script como Web App

1. En Apps Script, pulsa:

```text
Implementar > Nueva implementacion
```

2. En tipo de implementacion, elige:

```text
Aplicacion web
```

3. Configura:

```text
Ejecutar como: Yo
Quien tiene acceso: Cualquier usuario
```

4. Pulsa `Implementar`.
5. Copia la URL de la aplicacion web.

Sera parecida a:

```text
https://script.google.com/macros/s/AKfycbx.../exec
```

Esa URL sera tu API.

Importante: si mas adelante modificas el codigo de Apps Script, debes crear una nueva implementacion o actualizar la implementacion existente.

Si al iniciar sesion desde el movil aparece una URL de `accounts.google.com` o un error `403 Forbidden`, la Web App no esta publicada para acceso publico. Vuelve a `Implementar > Gestionar implementaciones`, edita la implementacion activa y revisa que diga:

```text
Ejecutar como: Yo
Quien tiene acceso: Cualquier usuario
```

Despues guarda una nueva version y copia la URL `/exec` de esa implementacion.

---

## 13. Conectar la web con Apps Script

En la carpeta del proyecto, crea un archivo llamado `.env`.

Puedes copiar `.env.example` y renombrarlo a `.env`.

Contenido:

```text
VITE_API_URL=https://script.google.com/macros/s/TU_URL_DE_APPS_SCRIPT/exec
```

Sustituye la URL por la que copiaste en Apps Script.

Ejemplo:

```text
VITE_API_URL=https://script.google.com/macros/s/AKfycbx123456789/exec
```

Despues reinicia Vite:

```powershell
npm.cmd run dev
```

Prueba a iniciar sesion con un usuario que hayas puesto en la hoja `Usuarios`.

---

## 14. Crear un repositorio en GitHub

1. Entra en [https://github.com](https://github.com).
2. Crea una cuenta o inicia sesion.
3. Pulsa `New repository`.
4. Pon un nombre, por ejemplo:

```text
porra-mundial-2026
```

5. Puedes dejarlo publico o privado.
6. No marques la opcion de crear README, porque este proyecto ya tiene uno.
7. Crea el repositorio.

---

## 15. Subir el proyecto a GitHub

Necesitas instalar Git:

1. Entra en [https://git-scm.com/downloads](https://git-scm.com/downloads).
2. Descarga Git para Windows.
3. Instala aceptando las opciones por defecto.
4. Cierra y vuelve a abrir PowerShell.

Desde la carpeta del proyecto:

```powershell
cd "C:\Users\Usuario\Documents\Porra"
git init
git add .
git commit -m "Crear porra mundial 2026"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/porra-mundial-2026.git
git push -u origin main
```

Cambia `TU_USUARIO` por tu usuario de GitHub.

---

## 16. Publicar en GitHub Pages

Instala dependencias si no lo hiciste:

```powershell
npm.cmd install
```

Publica:

```powershell
npm.cmd run deploy
```

La primera vez puede pedir iniciar sesion en GitHub.

Cuando termine, entra en tu repositorio de GitHub:

```text
Settings > Pages
```

Configura:

```text
Source: Deploy from a branch
Branch: gh-pages
Folder: / (root)
```

La web quedara publicada en una URL parecida a:

```text
https://TU_USUARIO.github.io/porra-mundial-2026/
```

---

## 17. Uso diario durante el Mundial

Para anadir usuarios:

1. Abre Google Sheets.
2. Ve a `Usuarios`.
3. Anade una fila con `username`, `password` y `displayName`.
4. Esa persona ya podra iniciar sesion.

Para anadir partidos:

1. Abre `Partidos`.
2. Anade una fila por partido.
3. Usa un `id` unico para cada partido.
4. Revisa bien la fecha y hora.

Para actualizar resultados:

1. Abre `Resultados`.
2. Escribe el `partidoId`.
3. Escribe `local`, `empate` o `visitante`.
4. El ranking se actualiza automaticamente al abrir la app.

Para cambiar una contrasena:

1. Abre `Usuarios`.
2. Cambia el valor de `password`.
3. Guarda. Google Sheets guarda automaticamente.

---

## 18. Comandos utiles

Arrancar en local:

```powershell
npm.cmd run dev
```

Crear version final:

```powershell
npm.cmd run build
```

Ver la version final en local:

```powershell
npm.cmd run preview
```

Publicar en GitHub Pages:

```powershell
npm.cmd run deploy
```

---

## 19. Seguridad y privacidad

Esta solucion esta pensada para una porra privada entre amigos y coste cero.

Puntos importantes:

- Las contrasenas se guardan en Google Sheets porque el requisito es gestionarlas manualmente.
- Usa contrasenas distintas a las de servicios reales.
- No compartas la URL de Google Sheets con quien no deba administrarla.
- Comparte solo la URL publica de GitHub Pages con tus amigos.
- Google Apps Script se publica como web app para que la web pueda leer y escribir datos.

---

## 20. Solucion de problemas

### La web dice que falta `VITE_API_URL`

Crea el archivo `.env` en la raiz del proyecto y pon:

```text
VITE_API_URL=https://script.google.com/macros/s/TU_URL/exec
```

Despues reinicia:

```powershell
npm.cmd run dev
```

### Login incorrecto aunque el usuario existe

Revisa:

- Que la pestana se llame `Usuarios`.
- Que los encabezados sean exactamente `username`, `password`, `displayName`.
- Que no haya espacios extra en el usuario.
- Que la URL de Apps Script corresponda a la hoja correcta.

### No aparecen partidos

Revisa:

- Que la pestana se llame `Partidos`.
- Que los encabezados sean correctos.
- Que cada partido tenga `id`, `fase`, `fecha`, `local` y `visitante`.

### No se guardan pronosticos

Puede pasar si:

- El partido ya esta bloqueado por hora.
- Falta la pestana `Pronosticos`.
- Apps Script no tiene permisos.
- Cambiaste el codigo de Apps Script y no actualizaste la implementacion.

### El ranking no cambia

El ranking solo suma puntos cuando hay resultados oficiales en `Resultados`.

Revisa que `resultado` sea exactamente:

```text
local
empate
visitante
```

---

## 21. Estructura esperada de Google Sheets

`Usuarios`

```text
username | password | displayName
```

`Partidos`

```text
id | fase | grupo | fecha | local | visitante
```

`Pronosticos`

```text
usuario | partidoId | pronostico | fechaRegistro
```

`Resultados`

```text
partidoId | resultado
```

`Especiales`

```text
usuario | mejorJugador | maximoGoleador | peorSeleccion | campeon | subcampeon | tercerClasificado | fechaRegistro
```
