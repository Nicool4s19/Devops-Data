# Devops-Data

Repositorio correspondiente a la capa de datos de la aplicación **Tienda de Alimentos para Perritos**.

## Descripción

Este componente contiene la base de datos MySQL utilizada por la aplicación para almacenar y administrar la información de los productos.

La base de datos es consumida por el Backend mediante una API REST desarrollada en Node.js.

## Tecnologías utilizadas

* MySQL
* Docker
* Amazon EC2
* AWS VPC
* Security Groups

## Arquitectura

Frontend ECS → Backend ECS → MySQL EC2

## Infraestructura AWS

### VPC

La solución se encuentra desplegada dentro de una VPC privada compuesta por:

* Subred pública para acceso al Frontend.
* Subred privada para Backend.
* Subred privada para Base de Datos.

### Seguridad

El acceso a la base de datos se encuentra restringido mediante Security Groups.

Se permite únicamente el acceso desde:

* Backend ECS
* Puerto TCP 3306

## Creación de la base de datos

```sql
CREATE DATABASE tienda_perritos;
```

## Tabla productos

```sql
CREATE TABLE productos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(255) NOT NULL,
    descripcion VARCHAR(255),
    precio DECIMAL(10,2) NOT NULL,
    stock INT NOT NULL
);
```

## Datos de prueba

```sql
INSERT INTO productos (nombre, descripcion, precio, stock)
VALUES
('Dog Chow Cachorro', 'Alimento para cachorros', 15990, 10),
('Pedigree Adulto', 'Alimento para perros adultos', 12990, 20),
('Royal Canin Mini', 'Alimento premium razas pequeñas', 28990, 15);
```

## Despliegue mediante Docker

Descargar imagen:

```bash
docker pull mysql:8.0
```

Crear contenedor:

```bash
docker run -d \
--name tienda-db \
-e MYSQL_ROOT_PASSWORD=root \
-e MYSQL_DATABASE=tienda_perritos \
-p 3306:3306 \
mysql:8.0
```

## Seguridad

La base de datos no posee acceso público desde Internet.

El acceso está restringido mediante Security Groups para permitir únicamente conexiones desde el Backend desplegado en Amazon ECS.

## Función dentro de la arquitectura

La base de datos almacena toda la información relacionada con los productos de la tienda y sirve como fuente de datos para las operaciones CRUD realizadas desde el Frontend.

## Autor

Nicolás Silva Figueroa
Dylan Monroy Sanhueza

Ingeniería en Informática
