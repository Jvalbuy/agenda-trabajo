# Jornada · Agenda de trabajo

PWA en español, sin dependencias ni servicios externos. Calendario, registros independientes, búsqueda por título y descripción, recordatorios pendientes/completados, IndexedDB y copias JSON. No requiere compilación.

## Publicar en GitHub Pages

1. Crea un repositorio propio, por ejemplo `agenda-trabajo`. Para usar Pages gratuitamente, puedes hacerlo público: el código será público, pero tus registros no se suben al repositorio.
2. Extrae el ZIP. Sube **el contenido de la carpeta `agenda`** a la raíz del repositorio: `index.html` debe quedar directamente en la raíz, no dentro de otra carpeta. Incluye `app.js`, `style.css`, `sw.js`, el manifiesto, los iconos y `.nojekyll`.
3. En el repositorio abre **Settings → Pages → Build and deployment → Source → Deploy from a branch**.
4. Selecciona la rama `main` y la carpeta **/ (root)**; pulsa **Save**.
5. Espera a que GitHub muestre la dirección publicada, normalmente `https://TU-USUARIO.github.io/agenda-trabajo/`.
6. Abre esa dirección con conexión. Espera «Disponible sin conexión». Después prueba el modo avión.

Documentación: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site

## Instalar en iPhone

Abre la dirección HTTPS en Safari → Compartir → Añadir a pantalla de inicio. Abre la app desde el icono con conexión una primera vez y espera «Disponible sin conexión». Prueba después cerrarla y volver a abrirla en modo avión. Utiliza siempre el mismo icono, navegador y dirección para acceder a tus datos.

## Datos y copias

- Cada registro se guarda en una transacción IndexedDB. No se envían datos a ningún servidor ni hay analítica, fuentes o bibliotecas remotas.
- Los datos pertenecen a este navegador y origen. No hay sincronización entre móviles. Cambiar de dominio, borrar datos del navegador, desinstalar o usar navegación privada puede hacer que dejes de tener acceso a ellos.
- En **Copias y ayuda**, exporta JSON con frecuencia y guarda el archivo fuera del navegador. La copia contiene los textos sin cifrar: guárdala en un lugar privado.
- La importación valida toda la copia antes de escribir y añade registros en una sola transacción. Combina por identificador: omite los idénticos y duplica con un identificador nuevo los conflictos; nunca borra la agenda. Importar repetidamente una versión antigua conflictiva puede crear duplicados.
- «Proteger almacenamiento» solicita persistencia al navegador, que puede rechazarla. No sustituye las copias.
- Los recordatorios guardan fecha y hora local. Si cambias de zona horaria, se interpretan en la zona del dispositivo. Exporta al calendario para fijar un evento con un instante concreto.

## Recordatorios: alcance real

Los recordatorios vencidos se resaltan en la lista al abrir la app y se revisan cada 30 segundos mientras está abierta. No se prometen alarmas con la app cerrada: una PWA estática totalmente offline no dispone de programación fiable de notificaciones locales en iPhone. Web Push requiere infraestructura y conectividad.

«Añadir al calendario» descarga un archivo `.ics` con evento y aviso a la hora elegida. Ábrelo o impórtalo en tu calendario y confirma que se ha añadido y que sus alarmas están activadas. El soporte de importación depende de la aplicación y del dispositivo. Cambiar o completar el recordatorio en Jornada no actualiza el evento ya importado.

Referencia de Apple: https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/

## Comprobar la instalación

1. Crea dos registros en el mismo día y otro en una fecha diferente. Edita uno y elimina otro.
2. Busca palabras del título y descripción, también sin tildes. Pulsa un resultado: debe abrir el día y destacar el registro.
3. Crea un recordatorio futuro y otro vencido. Comprueba los filtros y cambia su estado.
4. Exporta una copia y comprueba que el archivo contiene los registros. Impórtala: los registros idénticos no deben duplicarse. Prueba un JSON inválido: debe rechazarse sin cambios.
5. Tras ver «Disponible sin conexión», activa modo avión, cierra y abre la app. Guarda y edita un registro; cierra y vuelve a abrir para comprobar persistencia.
6. En otro navegador o perfil, importa la copia y verifica la recuperación.

## Desarrollo y actualizaciones

Para probar en ordenador, sirve la carpeta con `python -m http.server 8000` y abre `http://localhost:8000`. No abras `index.html` directamente: el service worker necesita HTTPS o localhost. Una dirección HTTP de la red local no es suficiente para instalar el service worker en iPhone; usa GitHub Pages.

Cuando cambies archivos, incrementa `CACHE` en `sw.js` (por ejemplo `jornada-shell-v2`) y mantén todos los recursos en `ASSETS`. Tras publicar, abre la app con conexión y vuelve a abrirla cuando se haya actualizado el service worker. No cambies el nombre de la base de datos ni el origen si quieres conservar los registros. El caché solo contiene la aplicación; los registros permanecen en IndexedDB.

No se incluyen datos de ejemplo en la agenda real.
