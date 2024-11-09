# Proyecto: Configuración Docker con MySQL, Nginx, PHP, Redis, Grafana, Prometheus, cAdvisor, Elasticsearch y Kibana

Este proyecto es una configuración avanzada de Docker para un entorno de desarrollo y monitoreo. Incluye los siguientes servicios:

- **MySQL**: Base de datos
- **Nginx**: Servidor web
- **PHP-FPM**: Procesador de backend PHP
- **Redis**: Caching
- **Grafana**: Visualización de métricas
- **Prometheus**: Recolección de métricas
- **cAdvisor**: Monitoreo de contenedores
- **Elasticsearch**: Motor de búsqueda
- **Kibana**: Visualización de logs y métricas

Además, se ha configurado una red de Docker para la comunicación entre los servicios, volúmenes para la persistencia de datos y herramientas adicionales para visualizar y analizar el rendimiento del sistema.

## Archivos Importantes

### 1. **Terraform Scripts**
Este proyecto utiliza Terraform para gestionar la infraestructura de contenedores. Los archivos de configuración de Terraform definen los servicios y recursos necesarios para levantar los contenedores en Docker.

- **network.tf**: Define la red de Docker llamada `mi_red` que conecta todos los contenedores.
- **volumes.tf**: Define los volúmenes persistentes para los servicios de MySQL, Redis, Grafana y Prometheus.
- **containers.tf**: Define los contenedores Docker que se ejecutan en el entorno:
  - MySQL (base de datos)
  - PHP-FPM (procesador de PHP)
  - Nginx (servidor web)
  - Redis (caching)
  - Grafana (monitoreo de métricas)
  - Prometheus (recolección de métricas)
  - cAdvisor (monitoreo de contenedores)
  - Elasticsearch (motor de búsqueda)
  - Kibana (visualización de logs y métricas)

### 2. **Archivos de Configuración**
- **nginx.conf**: Configuración de Nginx para servir el sitio web y pasar las solicitudes PHP a PHP-FPM.
- **prometheus.yml**: Configuración de Prometheus para recoger métricas del sistema.
- **kibana.yml**: Configuración de Kibana para conectar con Elasticsearch y mostrar los logs.

## Cómo Usar

### 1. Levantar los Servicios con Terraform

Para levantar la infraestructura usando Terraform, sigue estos pasos:

1. **Inicializar Terraform**:
   terraform init

2. **Aplicar la Configuración de Terraform**:

terraform apply

Esto creará los contenedores necesarios y las redes de Docker.

2. Acceder a las Herramientas de Monitoreo
Grafana: Accede a Grafana en http://localhost:3000 para visualizar las métricas de tus servicios.
Usuario y contraseña predeterminados: admin / admin.

Prometheus: Accede a Prometheus en http://localhost:9090 para ver las métricas recolectadas por el sistema.

cAdvisor: Accede a cAdvisor en http://localhost:8081 para monitorear el rendimiento de los contenedores.

Kibana: Accede a Kibana en http://localhost:5601 para ver los logs y análisis proporcionados por Elasticsearch.

3. Acceder a phpMyAdmin
Una vez que todos los contenedores estén levantados, puedes acceder a phpMyAdmin en http://localhost:8080 para gestionar la base de datos MySQL.


4. Acceder a la Web
http://localhost:100, para poder visualizar la web y interactuar con la base de datos y la caché.

Estructura del Proyecto
bash
Copy code
practica-2/
├── docker/
    ├── mysql_data/                  # Volúmenes para los datos de MySQL
    ├── redis_data/                  # Volúmenes para los datos de Redis
    ├── grafana_data/                # Volúmenes para los datos de Grafana
    ├── prometheus_data/             # Volúmenes para los datos de Prometheus
    ├── docker-compose.sta.yml       # Archivo Docker Compose para el entorno de STA (básico)
    ├── docker-compose.pro.yml       # Archivo Docker Compose para el entorno de PRO (con Redis)
    ├── Dockerfile                   # Dockerfile para configurar el entorno PHP y Redis
    ├── prometheus.yml               # Configuración de Prometheus
    ├── prometheus_sta.yml           # Configuración de Prometheus
    ├── alert-manager.yml            # Configuración envio alertas
    ├── alert_rules.yml              # Configuración reglas de alertas
    ├── kibana.yml                   # Configuración de Kibana
    ├── nginx.conf                   # Configuración de Nginx para servir el sitio
    ├── web/                         # Directorio para los archivos del sitio web
    │   ├── db.php                   # Archivo de conexión a la base de datos
    │   ├── index.php                # Archivo principal del sitio web
    │   ├── config.php               # Archivo de configuración de la base de datos
    │   └── css/
    │       └── style.css            # Estilos CSS
│
├── terraform/                   # Archivos de configuración de Terraform
├── pro/                         # Directorio para los archivos terraform pro
│   ├── main.tf                  # Archivo main de terraform, contenedores y volúmenes
│   ├── outputs.tf               # Salidas (IP)
│   ├── provider.tf              # Proveedor (en este caso Docker) en local
│   └── variables.tf             # Variables de entorno
├── sta/                         # Directorio para los archivos terraform sta
│   ├── main_sta.tf              # Archivo main de terraform, contenedores y volúmenes
│   ├── outputs_sta.tf           # Salidas (IP)
│   ├── provider.tf              # Proveedor (en este caso Docker) en local
│   └── variables_sta.tf         # Variables de entorno


Notas:
    Persistencia de Datos: Los datos de MySQL, Redis, Grafana y Prometheus se guardan en volúmenes Docker. Esto asegura que los datos no se pierdan cuando se detengan los contenedores.

    Monitoreo: Grafana, Prometheus, cAdvisor, Elasticsearch y Kibana se utilizan para monitorear el rendimiento y la salud de los servicios. Grafana y Kibana proporcionan una interfaz visual para analizar las métricas y logs de los contenedores.

    Configuración de Nginx: El archivo nginx.conf define cómo Nginx maneja las solicitudes y pasa las solicitudes PHP a los contenedores PHP-FPM.

    Escalabilidad: El contenedor PHP-FPM está configurado para crear múltiples instancias (3 en este caso) para manejar un mayor volumen de solicitudes.

Recomendaciones:
    Asegúrate de tener Docker y Terraform instalados en tu máquina antes de empezar.

    Puedes modificar los valores de las variables como contraseñas y configuraciones de servicios en los archivos variables.tf o directamente en los scripts de configuración de los contenedores.

    Para detener todos los contenedores, usa el siguiente comando:

        docker-compose down
