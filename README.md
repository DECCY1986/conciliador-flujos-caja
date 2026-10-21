# Conciliador de flujos de caja

Aplicación estática para registrar ingresos por flujo, importar pagos de Excel y guardar el registro compartido en una carpeta de OneDrive usando Microsoft Graph.

## Publicar en GitHub Pages

1. Sube `index.html` y este `README.md` a la rama `main` del repositorio `DECCY1986/conciliador-flujos-caja`.
2. En GitHub, abre **Settings → Pages**.
3. En **Build and deployment**, elige **Deploy from a branch**, rama `main` y carpeta `/(root)`.
4. La dirección esperada es `https://deccy1986.github.io/conciliador-flujos-caja/`.

## URI de redirección de Microsoft

En el registro de la aplicación en Microsoft Entra:

1. Abre **Authentication → Add a platform → Single-page application**.
2. Agrega la URI exacta `https://deccy1986.github.io/conciliador-flujos-caja/` y guarda.
3. En **API permissions**, conserva `User.Read` y el permiso delegado `Files.ReadWrite`.
4. No se usa un secreto de cliente. El inicio de sesión del navegador usa MSAL y autorización delegada.

## Uso inicial

1. Abre la página publicada e inicia sesión con una cuenta Microsoft que tenga permiso de edición en la carpeta compartida.
2. Pega y guarda el vínculo de uso compartido de la carpeta `Control de flujos de caja`.
3. La aplicación crea y actualiza `flujos_caja.json` dentro de esa carpeta.
4. Finanzas crea cada flujo con su número oficial, registra el valor del ingreso e importa los pagos desde Excel.
5. Los pagos se pueden asignar después de importarlos. La aplicación rechaza pagos que excedan el saldo y cierra un flujo al llegar exactamente a cero.

El archivo compartido usa su eTag para rechazar una escritura si otra persona guardó cambios desde que se cargó el registro. Ante el aviso de conflicto, actualiza desde OneDrive antes de volver a guardar.

## Formato esperado para Excel

La primera fila debe tener encabezados reconocibles como `Fecha`, `Descripción` y `Valor`. Se importan filas con valores positivos; cada pago empieza sin asignar para que el usuario seleccione el flujo correcto.

## Límites de esta versión

- La aplicación necesita conexión a Internet para Microsoft Graph, MSAL y SheetJS.
- El vínculo de carpeta se guarda en el navegador de cada usuario; hay que pegarlo una vez en cada equipo o perfil.
- El permiso `Files.ReadWrite` es delegado y amplio para los archivos a los que el usuario ya puede acceder. El token se conserva en el almacenamiento local de ese navegador; nunca se publica un secreto de cliente.
- La carpeta debe estar compartida con permiso de edición para cada persona que vaya a guardar cambios.
