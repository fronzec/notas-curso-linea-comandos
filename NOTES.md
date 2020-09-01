# Notas del curso de terminal Platzi
## Comandos para directiorios y ar
`ls`: imprimir lista de archivos y directorios
`mkdir <nombre-directorio>`: crear un directorio
`cd <directorio>`: cambiar a otro directorio
`mv <archivo/directorio> <nuevo-directorio-padre>`: mover un archivo o directorio a una nueva ubicacion
`touch <archivo>`: crear un nuevo archivo en el directio actual
`rm <archivo>`: remover un archivo
`rmdir <directorio>`: remover un directorio, en primera instancia no se puede eliminar un directorio con archivos pero se puede especificar un flag para indicarle que borre incluyendo su contenido
`man <comando>`: permite visualizar el manual del comando especificado, util para buscar opciones adicionales
`cat <archivo>`: imprime todo el contenido de un archivo de texto

## Utilidates de procesamiento batch(por lotes)
`grep <expresion-regular> <archivo>`: permite buscar lineas que cumplan la expresion regular dada, podemos usar el flag `grep busqueda -C 4 archivo.txt` para incluir lineas contiguas(de contexto) para la linea que contiene el regex, tambien usar `-n` para incluir el numero de linea en los resultados de busqueda
`head <archivo>`: permite mostrar las primeras lineas de un archivo
`tail <archivo>`: permite mostrar las ultimas lineas de un archivo
`sed '<instrucciones>' <archivo>`: sed(stream editor) permite procesar texto, podemos realizar sustituciones, eliminar, etc.
`awk`: permite tratar archivos de texto delimitados, podemos procesar archivo csv `awk -F ';' '{print $1 }' archivo.csv`

## Comunicacion entre procesos
 Flujos
 - Entrada estandar: Por default conectado al teclado
 - Salida estandar: Por default es la pantalla
 - Error estandar: Por default es la pantalla

 Podemos modificar el default de estos flujos usando la redireccion
 `comando < archivo.txt` : Redirecciona un archivo al flujo de entrada de un comando
 `comando > archivo.txt`: redirecciona la salida de un comando hacia un archivo
 `comando >> archivo.txt `: redirecciona la salida del comando al archivo, en este caso se agrega al final del archivo


 Pipes(Tuberias): tomar la salida de un proceso y conectarla a la entrada de otro proceso, se identifica con el comando pipe `|`
 `ls -l | more`: pasamos el resultado del comando ls al comando more, el comando more muestra un resultado grande en varias paginas
 `cat dump.sql | wc -l`: pasamos el resultado del comando cat al comando wc para contar cuantas palabras tiene el archivo.

## Administracion de procesos
  UN proceso puede ejecutarse en primer plano(foreground), la terminal queda bloqueada, tambien podemos tener procesos en segundo plano(background), podemos usar el simbolo ampersand`&` para indicar que cuando se cree un proceso se envie a brackground, tambien podemos usar `ctrl + z` para enviar un proceso en primer plano a segundo plano.
-  `comando &`: ejecuta el comando en segundo plano
- `fg`: trae a primer plano un proceso que se esta ejecutando en segundo plano
- `ps`: muestra los procesos que se estan ejecutando
- `ps -ax`: muestra los procesos que se estan ejecutando del sistema
- `top`: muestra de manera interactiva los procesos que se estan ejecutando, los datos que se muestran son en tiempo real.
- `kill -numero_prioridad numero_proceso`: permite enviar señales a los procesos que se estan ejecutando, ejemplo: `kill -9 12345`
- `killall -numero_prioridad nombre_ejecutable`: termina un proceso usando el nombre del ejecutable, ejemplo: `killall -9 proceso.php`

## Permisos de archivos
Los permisos en sistemas UNIX se dividen en permisos para el propietario, para el grupo y para otros, para ver los permisos podemos usar `ls -la` esto imprira una columna con los permisos algo así: `drwxrw----`, el primer caracter indica si es un archivo `-`, si es un directorio `d` o un link `l`. Los siguientes tres caracteres indican los permisos para el propietario del archivo, en este caso los permisos indica que el propietario tiene permiso de lectura`r`, escritura`w` y ejecucion`x`. Los siguientes tres caracteres indican que los usuarios con el mismo grupo del archivo tiene permiso de lectura, escritura pero no de ejecucion`-`, si un permiso esta negado para alguna agrupacion se indica con el signo de menos `-`. Finalmente los ultimos tres caracteres, en este caso los tres `-` indican que los otros usuarios no tienen ningun permiso sobre el directorio.

Para cambiar los permisos de los archivos podemos usar el comando `chown`, algunos ejemplos de su uso serian:
- `chmod o-w nombre_archivo`: Elimina el permiso de escritura`-w` para el grupo de permisos otros`o`
- `chmod +x nombre_archivo`: Agrega el permiso de ejecutar a todos los grupos de permisos.
- `cmod 760 nombre_archivo`: Modifica los permisos del archivo usando numeros, en este caso cada numero entero es trasladado a notacion binaria para indicar si el permiso debe ser agregado o no, por ejemplo el numero 7 que se usara para definir los permisos del propietario, en binario es 111 en donde cada bit de izquierda a derecha corresponderia a un permiso `rwx`. El segundo numero 6 en representacion binaria es 110, que se traduciria en que los miembros del grupo tendran permisos de lectura y escritura pero no de ejecucion, y finalmente el 0 indica que los otros usuarios no tienen ningun permiso sobre el archivo.
- `sudo comando`: algunos comandos requieren permisos especiales o de administrador, podemos usar el comando `sudo` para ejecutar un comando como super administrador.
- `sudo chown nombre_usuario nombre_archivo`: el archivo especificado cambia de propietario al especificado
- `sudo chgrp nombre_grupo  nombre_archivo`: el archivo especificado cambia de grupo al especificado

## Sistemas de manejos de paquetes
Existem muchos modulos o comunmente llamados paquetes de software que estan listos para su uso, tambien existen administradores de paquetes que nos ayudan a gestionar estos paquetes y poder instalarlos en nuestro sistema, podemos tener manejadores de paquetes binarios como son `apt`, `zypper`, `yum`, `pacman`, `dnf` y tambien manejadores de paquetes de lenguajes como pueden ser `pip`, `npm` y `composer`.

## Comprension de archivos
Si necesitamos comprimir archivos en sistemas UNIX podemos usar
- `gzip archivo`: pasamos el nombre del archivo que queremos comprimir, el archivo comprimido tomara el nombre del archivo original y le agregara la extension `.gz`
- `gzip -d archivo`: descomprime el archivo especificado
- `tar cf paquete.tar archivos_directorio`: empaqueta los archivos especificados en un solo archivo, no es compresion
- `tar tf paquete.tar`: ver contenido de un paquete
- `tar -cvf paquete.tar archivos_directorio`: empaquetar y ver contenido del paquete
- `tar xf paquete.tar`: desempaquetar el archivo
- `tar czf empaquetado.tar.gz carpeta`: empaqueta y comprime
- `tar xcf empaquetado.tar`: descomprimir y desempaquetar

## Herramientas de busqueda de archivos
Para buscar archivos podemos usar: `locate`, `whereis` y `find`.
- `locate archivo.py`: busca usando una base de datos interna que se debe actualizar explicitamente usando `sudo updatedb`
- `whereis archivo.py`: busca en todo el sistema el argumento especificado
- `find [ruta] [expresion-de-busqueda] [accion]`: find permite busquedas mas complejas
- `find . -user miusuario -perm 644`: busca en mi directorio actual archivos que tengan mi usuario y tengan los permisos 644
- `find . -type f -mtime +7`: busca en mi directorio actual, solo archivos y que hayan sido modificados hace mas de 7 dias
- `find . -type f -mtime +7 -exec cp {} ./backup/ \;`: podemos indicar que los resultados de busqueda se les aplique otro comando usando `-exec`, en este caso indicamos que cada archivo encontrado `{}` sea copiado`cp` al directorio`./backup/` y los ultimos simbolos `\;` indican que nuestro comando finaliza en esa parte.

## Herramientas para interacturar a travez de HTTP
Para interactuar con HTTP podemos usar `curl` y `wget`
- `curl`: se usa para pedidos crudos
- `wget`: para hacer descargas desde servidores HTTP
- `curl https://url.com`: Hace una peticion GET a la URL especificada
- `curl -v https://url.com`: con la opcion -v indicamos que la peticion sea verbose, y nos mostrara mas detalle del proceso de comunicacion y transferencia
- `wget https://url.com/test.tar.bz2`: descarga el arhivo especificado
