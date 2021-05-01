
Método Docker

Cree una copia de docker-compose.yml.example como docker-compose.yml y una copia de docker-compose.env.example como docker-compose.env. 
 

Modifique docker-compose.env y adapte las siguientes variables de entorno:

PAPERLESS_PASSPHRASE
Esta es la frase de contraseña que utiliza Paperless para cifrar / descifrar el documento original. Si no planea usar el cifrado GPG, puede dejarlo sin definir.

PAPERLESS_OCR_THREADS
Este es el número de subprocesos que generará el proceso de OCR para procesar las páginas del documento en paralelo. Si la variable no está configurada, Python determina el recuento de núcleos de su CPU y usa ese valor.

PAPERLESS_OCR_LANGUAGES
Si desea que el OCR para reconocer otros idiomas además del Inglés por defecto, establezca este parámetro en un espacio de lenguaje lista de códigos de tres letras separó después de la norma ISO 639-2 / T . Para obtener una lista de los idiomas disponibles, incluidos sus códigos de tres letras, consulte la lista de paquetes de Alpine .

USERMAP_UID y USERMAP_GID
Si desea montar el volumen de consumo (directorio /consumedentro de los contenedores) en un directorio de host, lo que probablemente desee hacer, los derechos de acceso pueden ser un problema. El usuario y el grupo predeterminados paperless en los contenedores tienen una identificación de 1000. Los contenedores exigirán que el grupo propietario del directorio de consumo paperlesspueda eliminar los documentos consumidos. Si su sistema anfitrión tiene un grupo con un ID de 1000 y no desea que este grupo tenga derechos de acceso al directorio de consumo, puede usar USERMAP_GIDpara cambiar el ID en el contenedor y por lo tanto el del directorio de consumo. Además, también puede cambiar la identificación del usuario predeterminado usando USERMAP_UID.

PAPERLESS_USE_SSL
Si desea que Paperless utilice SSL para la interfaz de usuario, establezca esta variable en true. También debe copiar su certificado y clave en el data directorio, llamado ssl.certy ssl.key. Esta no es una solución ideal y, si es posible, se prefiere un proxy inverso con nginx.
Corre . Esto creará e iniciará los contenedores necesarios.docker-compose up -d

Para poder iniciar sesión, necesitará un superusuario. Para crearlo, ejecute el siguiente comando:

$ docker-compose run --rm webserver createsuperuser 

Esto le pedirá que establezca un nombre de usuario (predeterminado paperless), una dirección de correo electrónico opcional y finalmente una contraseña.

El valor predeterminado docker-compose.yml exporta el servidor web en su puerto local 8000.
Si no lo ha adaptado, ahora debería poder visitar su servidor web sin papel en http://127.0.0.1:8000(o https://127.0.0.1:8000si habilitó SSL).
Puede iniciar sesión con el usuario y la contraseña que acaba de crear.



         volumes:
             - paperless-data:/usr/src/paperless/data
             - paperless-media:/usr/src/paperless/media
-            - /consume
+            - /local/path/you/choose:/consume
 