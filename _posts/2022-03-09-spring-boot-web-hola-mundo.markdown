---
layout: post
title:  "UD07-01.- Spring Boot Web ‘Hola mundo’."
date:   2022-03-09 10:25:00 +0100
categories: hola mundo
---

## Creación del proyecto base 

Para crear un nuevo proyecto Spring Boot, para implementar servicios REST, debemos acceder a la página [https://start.spring.io/](https://start.spring.io/), y rellenamos el formulario: 

![Formulario de creación de proyecto Spring]({{site.baseurl}}/assets/img/spring-initializr.png)

Donde:

|              |               |                                                                |
|--------------|---------------|----------------------------------------------------------------|
| Project      | maven project | Seleccionamos maven como gestor de dependencias.               |
| Languaje     | Java          | Seleccionamos Java como lenguaje de programación.              |
| Spring boot  | 2.6.0 (M3)    | Seleccionamos la versión de Spring Boot a utilizar.            |
| Group        | ad            | Introducimos el GroupId del artefacto a generar.               |
| Artifact     | udxxaxx       | Introducimos el ArtifactId del artefacto a generar.            |
| Name         | udxxaxx       | Introducimos el nombre del artefacto a generar.                | 
| Package name | ad.udxx       | Introducimos el paquete base del proyecto.                     | 
| Packaging    | jar           | Seleccionamos el tipo de artefacto a generar.                  | 
| Java         | 11            | Seleccionamos la versión de Java a utilizar.                   | 


Añadimos las dependencias necesarias, pulsando el botón "Add dependencies", y seleccionando los paquetes que necesitamos para nuestra aplicación. 

Por ejemplo, para un microservicio con acceso a datos utilizando JDBC, seleccionaremos: 

* Spring Web $\to$ nos permite implementar servicios REST. 
* H2 Database $\to$ nos permite utilizar la BBDD en memoria H2. 
* Spring Data JDBC $\to$ nos permite acceder a datos utilizando JDBC.

Pulsamos el botón Generate, para descargar un archivo ZIP que contendrá el esqueleto de nuestro proyecto. 

![Estructura de proyecto descargado]({{site.baseurl}}/assets/img/estructura-proyecto-base.png)


Una vez descargado el proyecto base generado, y descomprimido, en caso de que existan, podemos borrar los siguientes ficheros: 
* mvnw 
* mvnw.cmd 

Y ya podemos abrir el proyecto en nuestro entorno favorito, con soporte a proyectos Maven.  

# Descripción del proyecto. 

## Directorio src/main/java 

El directorio src/main/java contendrá las clases (código fuente) de nuestro microservicio. En el podemos encontrar los paquetes de la aplicación. 

En nuestro caso, el paquete principal de la aplicación será “ad.udxxayy”, donde: 

xx se corresponde con el número de la unidad que estamos realizando. 

aa se corresponde con el número de la actividad que estamos implementando.  

Por ejemplo, para la actividad 1 de la unidad 7, utilizaremos “ad.ud07a01” 

 

## Clase UDxxAyyApplication. 

En el paquete principal, encontramos la clase UDxxAyyApplication, responsable de iniciar el microservicio, cuando se ejecuta el método main. 

Si observamos el código fuente: 

![Códiugo fuente de la clase Application]({{site.baseurl}}/assets/img/clase-principal.png) 

 

* La clase incluye la anotación “@SpringBootApplication”, marcando la clase de inicio del microservicio. 

* El método main incluye la llamada al método “SpringApplication.run”, responsable de iniciar el microservicio. 

Al arrancar la aplicación, Spring Framework analiza el código fuente de la aplicación, en busca de anotaciones en el código, que van indicando al framework como debe comportarse la aplicación: 

* @SpringBootApplication 
* @Controller 
* @Repository 
* ... 


## Directorio src/main/resources 

El directorio src/main/resources contendrá los recursos estáticos (que no necesitan compilación), necesarios para el correcto funcionamiento de la misma. 

Entre estos recursos estáticos podemos encontrar: 

* Ficheros de configuración de datasources. 
* Ficheros de configuración de la aplicación.
* Ficheros de datos. 
* ...

## Fichero applicacion.properties  

El fichero application.properties contiene los parámetros de configuración de nuestra aplicación REST.   Podemos, entre otras cosas: 

* Indicar el puerto al que queremos que responda el microservicio. Por ejemplo, el puerto 8081: 
```properties 
    server.port 8081
``` 
* Indicar el contexto (path base) sobre el que responderá nuestro microservicio. Por ejemplo, la ruta ‘/api’: 
```properties 
    Server.servlet.context-path=”/api”
```
* Configurar una base de datos en memória, con h2. Por ejemplo, la base de datos ‘empresa_ad’, con usuario ‘sa’, y password ‘password’: 
```properties 
    spring.datasource.url=jdbc:h2:mem:empresa_ad 
    spring.datasource.username=sa 
    spring.datasource.password=password 
```
* Habilitar la consola de h2, para poder acceder a un cliente Web a través de un path del microservicio. Por ejemplo, sobre el path ‘h2-console’: 
```properties 
    spring.h2.console.enabled=true 
    spring.h2.console.path=/h2-console 
    spring.h2.console.settings.trace=false 
    spring.h2.console.settings.web-allow-others=false 
```
* Configurar un datasource sobre la base de datos h2 configurada: 
```properties 
    spring.datasource.url=jdbc:h2:mem:empresa_ad 
    spring.datasource.username=sa 
    spring.datasource.password=password 
```

## Directorio src/test/java 

El directorio src/test/java contendrá las clases (código fuente) de nuestros test unitarios. La estructura de este directorio es equivalente a la estructura del directorio src/main/java. 

## Directorio src/test/resources 

El directorio src/test/resources contendrá los recursos estáticos necesarios para la correcta ejecución de los test unitarios, pero que no son necesarios para la ejecución de la aplicación. La estructura de este directorio es equivalente a la estructura del directorio src/main/resources. 

## Fichero pom.xml 

El fichero pom.xml contiene la meta-información del proyecto: 

* Proyectos de los que hereda componentes (parent). 
* Grupo, artefacto, nombre y versión (groupId, artifactId, name, version). 
* Versión de java (properties.java.version). 
* Dependencias, o librerias necesarias para la compilación y ejecución (dependencies) . 

# “Hola Mundo” Controller 

Vamos a crear nuestro primer controlador, que permitirá realizar una petición GET al recurso /hola. 

* Creamos el paquete ‘ad.udxxayy.controllers’ en nuestra aplicación. 
* Creamos la clase HolaMundoController dentro del paquete que hemos creado. 
    * Le añadimos la anotación @RestController. 
    * Le añadimos la anotación @RequestMapping(“/hola”). 
* Añadimos el método public String holaMundo(): 
    * Le añadimos la anotación @GetMapping(“”) 
    * El método debe devolver la cadena ‘Hola mundo’. 
* Añadimos el método public String holaPersona(@PathVariable String nombre): 
    * Le añadimos la anotación @GetMapping(“/{nombre}”) 
    * El método debe devolver la cadena ‘Hola’ + nombre. 

![Código de hola mundo controller]({{site.baseurl}}/assets/img/hola-mundo-controller.png)

Tras implementar el método, arrancamos la aplicación, ejecutando la clase UDxxayyApplication, y observamos el log de la aplicación. 

![Arranque de la aplicación]({{site.baseurl}}/assets/img/arrancar-aplicacion.png)

Cuando la aplicación indique que ha arrancado, podemos hacer una petición a los servicios implementados. 

![Log del arranque de la aplicación](img/log arranque aplicacion.png)

Para conocer la ruta de los servicios, tenemos que tener en cuenta: 

* Los parametros del fichero application.properties: 
```properties
    server.port 8081 
    server.servlet.context-path=/api 
```

* Los parámetros de la clase Controller: 
```java
    @RequestMapping("/hola") 

        @GetMapping("") 
        
        @GetMapping("{nombre}") 
```

Teniendo en cuenta lo anterior, tenemos dos servicios: 

* http://localhost:8081/api/hola 
* http://localhost:8081/api/hola/{nombre} -> donde {nombre} es una variable. 

Abre un navegador web, y prueba las rutas: 

![Hola mundo]({{site.baseurl}}/assets/img/hola-mundo.png) 

Al realizar la petición a /api/hola, Spring trata de buscar la ruta del controlador que casa con la petición, y la encuentra en el @controller con @RequestMapping(‘/hola’), en concreto con el método @GetMapping(‘’). 

Al realizar la petición a /api/hola/pepe, Spring trata de buscar la ruta del controlador que casa con la petición, y la encuentra en el @controller con @RequestMapping(‘/hola’), en concreto con el método @GetMapping(‘/{nombre}’). 

# Anexo I: código fuente: 

[Spring Boot Web ‘Hola mundo’.](https://gitlab.com/2122-acceso-a-datos/profesor/ejemplos/ud07/-/tree/main/ud07a01)

# Anexo II: Tutorial en pdf: 

[Spring Boot Web ‘Hola mundo’.]({{site.baseurl}}/assets/pdf/spring-boot-hola-mundo.pdf)