Fundación Universitaria Compensar
Facultad de Ingeniería
Ingeniería de Sistemas
Seguridad de la Información
Felipe Castellanos Sánchez 
Nicolas Romero Rozo
________________________________________________________________________________
LABORATORIO 1 – ENTORNO CONTROLADO PARA PRUEBA DE VULNERABILIDADES
1-	Introducción
La seguridad de la información ya no es un complemento, ahora es una pieza fundamental de los sistemas y los negocios. En el Ethical Hacking utiliza las mismas técnicas y herramientas que un atacante, pero con fines de proteger. Es importante para proteger las infraestructuras de las empresas, anticiparse a las amenazas antes de que estas sean incidentes reales.
Una de las herramientas más efectivas en esta labor son las pruebas de penetración. Permite identificar vulnerabilidades críticas en los sistemas informáticos de distintas maneras. El proceso se rige por una metodología estructurada que abarca desde el reconocimiento inicial y el escaneo, hasta el exploit y el post-exploit, donde se ve el alcance real de la vulnerabilidad.
Para garantizar la integridad de los sistemas en producción, es importante y obligatorio realizar estas prácticas en entornos de laboratorio controlados. Mediante el uso de herramientas como nmap para el descubrimiento de red con y Metasploit para la ejecución de exploits, en este trabajo se realiza en base a una guía para realizar una prueba de vulnerabilidades en un ambiente controlado.
2-	Objetivos
2.1- Objetivo General
Identificar aspectos críticos en una prueba de vulnerabilidades mediante la ejecución de esta en un ambiente controlado de máquinas virtuales.
2.2- Objetivos Específicos
Desplegar dos hosts que funcionen como víctima y atacante.
Realizar los ataques correspondientes.
Documentar los pasos y resultados realizados.

3-	Procedimiento Experimental
3.1- Configuración del entorno
Para esta prueba se realizará una Se usa en este caso virtual box. Como esta prueba es de penetración y no de sistemas operativos, no haremos mucho énfasis en la instalación y configuración de las máquinas virtuales.
Comenzaremos con el host atacante. Un sistema operativo Kali Linux, se realiza la configuración y una vez terminado queda una sesión abierta que cargara después con la pantalla de inicio de sesión. 
 
Siguiendo todas las instrucciones, cargaría como en la imagen de abajo. 
 
Verificando conectividad nos pudimos dar cuenta que tiene una dirección ip. Que más adelante debemos compara con el host victima
 

Ahora realizamos la configuración e instalación metaespolitable 2. Esta máquina, fue desarrollada para realizar pruebas y permite identificar y explotar vulnerabilidades con fines educativos. Se descarga y se configura la máquina virtual con el disco duro de metaesploiteable 2 descargado previamente.
 



Una vez instalado y ejecutándose correctamente, vamos a probar las conexiones, y se realizara con el comando Ifconfig eht0
 
Nos pudimos dar cuenta que tienen la misma ip, por lo tanto, no están conectada a la misma red. Esto se va a corregir en la configuración de máquinas virtuales.
Verificación conexión
Fue necesario pasar a configuración de adaptador de puente para que funcionara en la misma red y tuvieran ip propias por DHCP. Ahora si realizamos los mismos comandos, podemos evidenciar que cada uno de los hosts tiene una dirección ip diferente y en la misma red.
  
No solo basta con saber que tengan una ip en el mismo rango. Se confirma conexión mediante un ping de la maquina atacante a la víctima, lo que nos confirma que se comunican y hay conexión.
 
Reconfirmación
Para reconfirmar se realiza desde la maquina Kali Linux el comando Nmap -sn 192.168.1.0/24 para poder identificar los host activos sin escanear los puertos, solo identificacion. 
 
Tambien se utiliza el comando Netdiscover mediante sudo netdiscover -r 192.168.1.0/24 y descubrir los host en las redes locales.
 
Ahora se procedera con escaneo detallado. Sera un escaneo exhaustivo para identificar puertos abiertos, servicios que se ejecutan y vulnerabilidades basicas. Usamos nmap -sV -sC -p- -T4 -oN nmap_scan_metasploitable2.txt 192.168.X.Y
 
Una vez terminado el escaneo, iremos a la carpeta para tomar el archivo .txt donde esta la informacion encontrada.
 


Explotacion de vulnerabilidades con metasploit
Ahora realizar Msfconsole desde la consola, nos saldra un codigo simulando una pantalla de inicio 
 
Se realiza la función search vsftpd 2.3.4 ya que nos salió esta información en el archivo cuando ejecutamos nmap. Y salió una imagen 
 
Ahora que hemos identificado el puerto, vamos con el exploit en modulo que hallamos con los comandos use exploit/unix/ftp/vsftpd_234_backdoor y despues realizamos el de show options  para ver los parámetros requeridos.
 
Configuramos el RHOSTS con la ruta del host victima
 
Realizamos el exploit, si es correcto. Tendremos los privilegios y accederemos
 
Se confirma al realizar los comandos de whoami y si es root, tenemos los accesos de super usuario. Con el comando id se sabrá los id y los grupos.
 



Tambien se encontro una vulneraviliadd en la version de Samba, que era 3.0.20 y que permite usar el comando usermap script, tema de inyeccion de sql.
 
Se realiza el use exploit y show options para configurar las opciones para la prueba.
 
Se configura el host de la victima con set RHOSTS
 
Para esta prueba funciono con reverse_netcad  
 

Comando shows options 
 
Se configura el Lhost
 
Y se configura el puerto con LPORT
 
Y una vez configurado, procedemos con el exploit. Si todo fue correcto, al igual que con metaesploit, podemos confirmar nuestros permisos e administrador con whoami
 Tambien podemos ver el id y los grupos a los que pertence. 
Ahora procedemos a recopilar informacion una vez estamos con acceso. Con el comando cat para contatenar y mostrar el contenido del archivo en la pantalla
 
Ver la version de sistema
 
Con el comando ifconfig –a veremos todas las interfaces de red. 
 




Ahora veremos en detalle cuales son los puertos abiertos y servicios que estan escuchando
 
Y la tabla de enrutamiento mediante el comando route -n
 
Se observan la lista de usuario con el comando cat /etc/passwd   
 


Ahora las contraseña que tienen hash cat /etc/shadow      
 
Para ver los grupos del sistema se usa el comando cat /etc/group
 
Podemos ver los procesos en ejecucion con detalles mediante el comando ps aux.
 
Podemos navegar y conocer el contenido del directorio raiz.
 

Podemos realizar mas detalle de informacion sensible como por ejemplo ver informacion de servidores web con /var/www 
 





Podemos realizar y configurar tareas programadas 
 
Nos dimos cuenta de tener actualizaciones para evitar backdoor o vulnerabilidades. Podemos replicar estos ejercicios de pruebas en ambientes controlados y después proceder con nuestros equipos y reducir los riesgos en hogares y compañias. 
