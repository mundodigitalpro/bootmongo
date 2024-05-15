# BootMongo

## Descripción
Este proyecto es una aplicación Spring Boot escrita en Kotlin que se conecta a una base de datos MongoDB. Utiliza Docker Compose para gestionar los servicios Docker.

## Requisitos Previos

- **Java JDK 17 o superior**
- **Gradle**
- **Docker y Docker Compose**

## Configuración del Proyecto

### Archivo `build.gradle.kts`

Asegúrate de tener las siguientes configuraciones en tu archivo `build.gradle.kts`:


import org.jetbrains.kotlin.gradle.tasks.KotlinCompile

plugins {
    id("org.springframework.boot") version "3.2.5"
    id("io.spring.dependency-management") version "1.1.4"
    id("org.jetbrains.kotlin.jvm") version "1.9.23"
    id("org.jetbrains.kotlin.plugin.spring") version "1.9.23"
    id("com.avast.gradle.docker-compose") version "0.16.8"
}

group = "com.example"
version = "0.0.1-SNAPSHOT"

java {
    sourceCompatibility = JavaVersion.VERSION_17
}

repositories {
    mavenCentral()
}

dependencies {
    implementation("org.springframework.boot:spring-boot-starter-data-mongodb")
    implementation("org.springframework.boot:spring-boot-starter-web")
    implementation("com.fasterxml.jackson.module:jackson-module-kotlin")
    implementation("org.jetbrains.kotlin:kotlin-reflect")
    developmentOnly("org.springframework.boot:spring-boot-devtools")
    testImplementation("org.springframework.boot:spring-boot-starter-test")
}

tasks.withType<KotlinCompile> {
    kotlinOptions {
        freeCompilerArgs = listOf("-Xjsr305=strict")
        jvmTarget = "17"
    }
}

tasks.named<Test>("test") {
    useJUnitPlatform()
}

dockerCompose {
    useComposeFiles = listOf("docker-compose.yml")
    startedServices = listOf("mongodb")
    stopContainers = true
    removeContainers = true
    removeImages = com.avast.gradle.dockercompose.RemoveImages.None
    removeVolumes = true
    captureContainersOutput = true
    waitForTcpPorts = true
}



### Archivo docker-compose.yml
Asegúrate de tener el siguiente archivo docker-compose.yml en la raíz de tu proyecto:

version: '3.9'

services:
mongodb:
image: mongo:latest
container_name: mongodb
ports:
- "27017:27017"
volumes:
- mongo-data:/data/db

volumes:
mongo-data:

## Ejecución del Proyecto

### Iniciar los Servicios y la Aplicación

`gradlew.bat composeUp bootRun`

### Detener los Servicios y la Aplicación

Detén la aplicación Spring Boot presionando Ctrl + C en la terminal donde está ejecutándose.

Luego, para detener y eliminar los contenedores Docker, ejecuta:

`gradlew.bat composeDown`

### Verificación

Para verificar que los servicios Docker están corriendo, puedes usar:

`docker ps`

### Para ver todos los contenedores, incluidos los detenidos:

`docker ps -a`

### Para limpiar los contenedores y volúmenes no utilizados:

`docker system prune -f`

`docker volume prune -f`

# Autor
Jose Jordan