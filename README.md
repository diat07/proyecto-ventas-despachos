**Proyecto VENTAS-DESPACHOS**

El proyecto consta de una página web con un patrón de microservicios, la cual consta de un frontend y dos backends que se conectan 
a una o más bases de datos, una en nuestro caso. Debido al propósito de la evaluación de entregar un flujo de CI/CD, cada uno 
de los tres componentes más la base de datos está dockerizada gracias a su propio dockerfile *multistage*, los cuales son ocupados 
para crear las imágenes de los componentes por los workflows de este mismo repositorio y posteriormente conectarse al EKS y desplegar 
la aplicación en su totalidad.

**Workflows**
Bajo este nombre tenemos tres archivos dentro del repositorio, uno por cada módulo del sistema (base de datos excluída). 
Las funciones de estos archivos son:
1.- Autenticarse con nuestra cuenta AWS.
2.- Construir la imagen del componente y públicarla  en ECR.
3.- Configurar kubconfig
4.- Desplegar en nuestro EKS el módulo del sistema

Para este flujo de trabajo los workflows ocupan los 'secrets' almacenados en este mismo repositorio, aumetando la seguridad y 
modularidad de nuestro proyecto.

**K8s Deployment**
Para la operación correcta del sistema hemos desplegado 4 pods y 4 servicios respectivos:
3 pods y 3 Service/ClusterIP --> Base de datos, backend ventas, backend despachos
1 pods y 1 Service/LoadBalancer --> Frontend

Todos funcionan por defecto con una sola réplica y extraén la última versión de la imagen base desde nuestro ECR. El uso de un 
Service tipo *LoadBalancer* se volvío necesario para desplegar el sistema de manera continua y estable, sin mencionar el beneficio 
de un nombre de dominio, que aunque no más sencillo que una IP tradicional, si nos acercó a un entorno más *ceñido* a producción.

**Tecnologías del sistema**
Frontend --> React.js
Backend Ventas --> Spring Boot
Backend Despacho --> Spring Boot
Base de datos --> MySQL
