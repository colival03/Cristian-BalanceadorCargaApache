# Cristian-BalanceadorCargaApache

## Índice

1. [Introducción](#introducción)
2. [Configuración de infraestructura en AWS](#configuración-de-infraestructura-en-AWS)
3. [Configuración del balanceador de carga](#configuración-del-balanceador-de-carga)
4. [Configuración Apache1](#configuración-apache1)
5. [Configuración apache2](#configuración-apache2)
6. [Configuración MariaDB](#configuración-MariaDB)
7. [Resultado](#resultado) 

# Introducción

Para esta práctica nuestro objetivo es el siguiente, poder lanzar una aplicación de usuarios mediante una infraestructura de capa 3, es decir, que lo que necesitaremos y  hemos utilizado es lo siguiente: un balanceador de carga, dos servidores apache y un servidor de base datos.

La configuración y lanzamiento de esta infraestructura se llevará a cabo desde AWS, el cual nos permitirá gestionar, modificar y configurar de una manera sencilla, la cual se explicará más adelante.

# Configuración de infraestructura en AWS

En primer lugar, el balanceador de carga se le ha asociado una ip elástica ya que el balanceador tiene que tener salida a internet y también la ip privada asociada de una subred. 

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/ac532f30-1c55-48a9-9970-f8e76995ce22)

En la máquina "Apache1" lo único que se le ha asociado ha sido a una subred para tener ip privada en la que se encuentra dentro de una red interna con el servidor "Apache2" y también por supuesto dentro de la red principal con el "BalanceadorCarga".

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/c1bab7bf-e055-4ddd-860a-3bb3d958db2a)

En la máquina de "Apache2" como ya se ha explicado anteriormente tendrán que encontrarse en la misma subred, ya que estos dos servidores son los servidores backend en los que posteriormente se llevará a cabo la configuración de la aplicación de usuarios.

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/67705cef-f3d4-4c98-a391-4dc66d328072)

En la máquina "Datos" tampoco tiene asociada ninguna ip pública ya que no queremos que tenga salida a internet porque no lo tiene que tener ya que esta máquina la utilizaremos para almacenar los datos de dicha aplicación de usuarios.

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/dbf3fd23-120c-485f-b6ac-aa880f597223)

Para esta práctica, lo que hemos utilizado ha sido un certificado con certbot el cual se explicará más detalladamente adelante.

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/bb3c7083-a758-4551-b2d6-8c26d649c905)

Ahora pasaremos a explicar la configuración de los grupos de seguridad, estas son las reglas que se le han añadido al grupo de seguridad del balanceador, las cuales son: HTTP,HTTPS,MYSQL,SSH,ICMP y se permite y se deniega el tráfico solo en este grupo de seguridad.

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/6ba6341b-9121-4676-a419-0d69cdfd8a46)

En los siguientes tres grupos de seguridad, le hemos añadido las mismas reglas de entrada que las cuales son: HTTP,HTTPS,MYSQL,SSH e ICMP.

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/5a7d974b-e6e8-4ce6-8234-864d0a7077e8)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/5f272a29-6458-4047-b685-5efbc41a668a)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/be562e73-685f-46c1-bec7-d109d08fd00c)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/9803ed37-4e94-470d-95f8-a07a8985c297)

# Configuración del balanceador de carga

Para comenzar con la configuración de nuestro balanceador de carga instalaremos apache y permitiremos los permisos que se encuentran en la imagen y después de haberlos activado tendremos que reiniciar el servicio para guardar la configuración.

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/cc6b1fd9-d50b-47f3-b202-8db1b46b4b97)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/4411fe26-1331-47c4-90f1-97e82749d9c5)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/ac721478-a76b-450b-a988-c1d1d503006d)

Para nuestro siguiente paso, tendremos que editar nustro fichero de configuración del balanceador en nuestro caso se llama "balanceador.conf" y en el añadiremos lo siguiente: server1 con la ip de la máquina "Apache1" y server2 con la ip de la máquina "Apache2".

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/8e376885-a9a8-4b70-b500-5f7234504cdd)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/3a777008-b32e-42bd-bd56-d1e135b98993)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/80ec8833-3679-435b-aa9e-49285aab2f6f)

Para crear un certificado válido necesitaremos certbot, para ello tendremos que instalar los siguientes paquetes.
![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/841db77e-8ac8-4042-aab3-0f01e1b5b99c)

Para registrar nuestro certificado, el cual utilizaremos posteriormente para desplegar nuestra aplicación, certbot nos pedirá una serie de datos y al acabar ya tendremos nuestro certificado listo para usarse.

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/d501c28e-82e4-4c07-9ba6-caf65b0a79d2)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/b247f4d0-d39b-40d0-a9f9-ba137bd8c007)

# Configuración Apache 1

Para la configuración de nuestro primer servidor apache tendremos que instalar apache, mariadb-client y git.

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/89a57485-0dc4-4e2f-973f-0dbf100a4476)

Para traer la aplicación de usuarios de git al servidor apache realizaremos un git clone con la url en la ruta "/var/www/html", después de hacer esto le hemos cambiado el nombre para mayor comodidad le hemos puesto "appUsuarios".

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/5257d64a-1506-4781-aab6-a3041d1d3917)

Para nuestro siguiente paso tendremos que cambiar el "DocumentRoot" que se encuentra en el fichero de configuración que en nuestro caso se llama "apache1.conf".

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/9f451ceb-8d5c-4faf-b92b-75005b8890da)

Hemos copiado el fichero de configuración acabado en "-ssl.conf" ya que hemos realizado la práctica mediante HTTPS y después de editar lo anteriormente mencionado tendremos que activar el fichero que acabamos de copiar y editar.

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/418b0d6d-bcc4-4041-9de4-db80c81aa542)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/24bdb01d-b9ff-4663-9db9-8f8db9f5a98b)

Por último, tendremos que reiniciar el servicio.

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/4bc582b7-02e2-4cba-a22a-6e6db516059b)

Configuración Apache 2

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/b79c766b-e683-4aa5-a105-29157fa33ed8)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/4e7565e6-aa13-40bf-8050-ab4b9e41ebc7)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/985c5237-c481-48c3-8d89-3853fa7aa350)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/025cba70-0e4f-4bcd-b673-553ff6114cd1)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/2eb47554-504f-4999-8233-ecbb7aefd43a)

Configuración MariaDB

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/c65e327b-aab3-4534-8754-b692513229af)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/1fe9122a-d272-4ff6-9cff-52e590aa9489)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/13f2b1e6-ba8b-4c39-a36b-158190c12c66)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/ae7088bd-b859-494a-8d06-2d696a68665e)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/a86dc7c5-a67f-4d7d-aa52-3aebb80f704f)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/954a8df6-e7a2-4c43-85a5-9a2400d569f1)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/5592ba90-c98e-4f7d-8766-30c56ac29e2b)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/cb06437f-18b4-4c05-b1d6-041654457f3c)

Resultado 

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/d9dcda1b-eff1-46b5-b206-d5b327bbf010)

![image](https://github.com/colival03/Cristian-BalanceadorCargaApache/assets/146434716/a841c8ee-68e2-4eb6-92dc-db1223345d68)
