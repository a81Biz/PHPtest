# 🚀 Docker Virtual Machine

Este repositorio contiene la configuración de una máquina virtual para Docker utilizando `docker-compose.yml`, con los siguientes servicios:

- **Apache Web Server** con soporte para HTTPS y certificados autofirmados.
- **MySQL Database** para almacenamiento de datos.
- **SFTPGo** como servidor FTP/SFTP para gestión de archivos.

## 📌 Requisitos
Antes de ejecutar este entorno, asegúrate de tener instalado:

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

## 📂 Estructura del Proyecto
```
📦 DockerVM
├── 📂 apache
│   ├── Dockerfile
│   ├── public/ (archivos de la web)
│   ├── certs/ (certificados SSL)
│   ├── openssl-san.cnf (configuración para generar certificados SSL)
├── 📂 sftpgo
│   ├── Dockerfile
│   ├── sftpgo.json (configuración de SFTPGo)
├── 📂 mysql_data (datos de MySQL)
├── docker-compose.yml (archivo de configuración principal)
└── README.md (este archivo)
```

## 🚀 Configuración y Ejecución
Para levantar los servicios, simplemente clona el repositorio y ejecuta:

```sh
# Clonar el repositorio
git clone https://github.com/tu-usuario/docker-vm.git
cd docker-vm

# Construir y ejecutar los contenedores en segundo plano
docker-compose up -d --build
```

## 🛠️ Servicios Disponibles
| Servicio  | URL / Puertos |
|-----------|--------------|
| **Apache** | `http://localhost:80` |
| **HTTPS** | `https://localhost:443` |
| **MySQL** | `localhost:3306` (Usuario: `mysql_user`, Password: `123`) |
| **SFTPGo Web Admin** | `http://localhost:8080` |
| **SFTPGo SFTP** | `sftp://localhost:2022` |
| **SFTPGo FTP** | `ftp://localhost:21` |

## 🌐 Red y Comunicación
Todos los servicios están configurados dentro de la misma **red de Docker (`my_custom_network`)**, lo que permite la comunicación interna entre los contenedores usando su **nombre de servicio**.

- **Comunicación Interna:**
  - Apache puede comunicarse con MySQL usando `mysql_db`
  - Apache puede acceder a SFTPGo usando `sftpgo`
  
- **Acceso Externo:**
  - Se puede acceder a los servicios desde el navegador o clientes FTP/SFTP usando `localhost` y sus respectivos puertos.

## 🔑 Certificados SSL Autofirmados
Apache usa certificados autofirmados generados con `openssl-san.cnf`. Esto permite habilitar **HTTPS** en el servidor local sin necesidad de certificados de una autoridad externa.

Si deseas regenerar los certificados manualmente, ejecuta:
```sh
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout apache/certs/server.key -out apache/certs/server.crt \
    -config apache/openssl-san.cnf
```

## 📂 Volúmenes
Los datos de cada servicio se almacenan en volúmenes persistentes:
- `./mysql_data` → Base de datos MySQL
- `./public` → Archivos públicos de Apache y SFTPGo
- `./sftpgo_data` → Datos de usuarios en SFTPGo

## 🔄 Comandos Útiles
### **Iniciar y Detener los Contenedores**
```sh
# Iniciar todos los servicios
docker-compose up -d

# Detener todos los servicios
docker-compose down
```

### **Reiniciar Contenedores**
```sh
docker-compose restart
```

### **Acceder a la Consola de un Contenedor**
```sh
# Ingresar al contenedor Apache
docker exec -it apache_web bash

# Ingresar al contenedor MySQL
docker exec -it mysql_db mysql -u mysql_user -p
```

### **Ver Logs de un Contenedor**
```sh
docker logs -f apache_web
```

## ❌ Eliminar Contenedores y Volúmenes
```sh
docker-compose down --volumes
```