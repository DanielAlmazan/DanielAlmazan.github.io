# Despliegue de Hotel Nest

![Docker logo](docker.svg) {width=300 border-effect="none"}

## Instalación de Docker

Para instalar Docker en el servidor (Debian, en este caso), ejecutamos los siguientes comandos:

```Bash
for pkg in docker.io docker-doc docker-compose podman-docker \
containerd runc; do sudo apt-get remove $pkg; done
```

## Crear red de Docker

Para crear una red de Docker, ejecutamos el siguiente comando:

```Bash
sudo docker network create node_mongo
```

Comprobamos que se ha creado correctamente con el siguiente comando:

```Bash
sudo docker network ls
```

```Ini
NETWORK ID     NAME          DRIVER    SCOPE
d167b3cb6ed3   bridge        bridge    local
d407dbcf94cc   host          host      local
885425ece5f9   node_mongo    bridge    local
fa4b3a84c89b   none          null      local
```

## Crear contenedor de MongoDB

```Bash
sudo docker run -d --name hotelDB --network node_mongo --restart \
unless-stopped -p 127.0.0.1:27017:27017 -v ~/data:/data/db mongo
```

Con este comando, creamos un contenedor de MongoDB llamado `hotelDB` que se conecta a la red `node_mongo`, 
se reinicia automáticamente si se detiene y se mapea el puerto 27017 del contenedor al puerto 27017 del 
servidor. Además, se monta el directorio `~/data` del servidor en el directorio `/data/db` del contenedor.

## Crear el fichero Dockerfile

```Bash
nano Dockerfile
```

```Docker
# Imagen base
FROM node:latest

# Crear directorio de la aplicación
RUN mkdir -p /usr/src/app

# Establecer directorio de trabajo
WORKDIR /usr/src/app

# Copiar los ficheros package.json y package-lock.json
COPY package*json ./
# Instalar dependencias
RUN npm install

# Copiar el resto de ficheros
COPY . .

# Exponer el puerto 3000
EXPOSE 3000

# Comando de inicio
CMD ["npm", "start"]
```

## Crear el fichero .dockerignore

```Bash
nano .dockerignore
```

```Docker
node_modules
```

## Preparar la aplicación para Docker

```Bash
cat {path-to-project}/hotel-nest/src/app.module.ts
```

```Typescript
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { LimpiezaModule } from './limpieza/limpieza.module';
import { AuthModule } from './auth/auth.module';
import { UsuarioModule } from './usuario/usuario.module';
import { MongooseModule } from '@nestjs/mongoose';
import { config } from 'dotenv';

config();

const dbHost = process.env.DEV_DB_HOST || 'hotelDB';
const dbPort = process.env.DEV_DB_HOST_PORT || '27017';
const devDbName = process.env.DB_NAME || 'hotel';

@Module({
  imports: [
    LimpiezaModule,
    MongooseModule.forRoot(`mongodb://${dbHost}:${dbPort}/${devDbName}`),
    UsuarioModule,
    AuthModule,
    UsuarioModule,
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

> En este caso, se han añadido variables de entorno para poder hacer configuraciones distintas
> en función del entorno en el que se ejecute la aplicación.

## Construir la imagen de Docker

```Bash
sudo docker build -t hotel-nest .
```

Comprobamos que se ha creado correctamente con el siguiente comando:

```Bash
docker images
```

```Ini
REPOSITORY   TAG      IMAGE          ID   CREATED     SIZE
hotel_nest   latest   76ccfe099c89   2    hours ago   1.33GB
mongo        Latest   ff65a94ec485   2    weeks ago   795MB
```

Ejecutamos el contenedor con el siguiente comando:

```Bash
sudo docker run -d --name HotelNest --network node_mongo --restart \
unless-stopped -p 3000:3000 hotel_nest
```

Compromosbar que se ha creado correctamente con el siguiente comando:

```Bash
docker ps
```

```Ini
CONTATNER ID   IMAGE        COMMAND                     CREATED       STATUS       PORTS                                        NAMES
3bb36a602b9c   hotel_nest   "docker-entrypoint. S..."   2 hours ago   Up 2 hours   0.0.0.0:3000->3000/tcp, ::: 3000->3000/tcp   HotelNest
187d7e16ecbf   mongo        "docker-entrypoint. S..."   2 hours ago   Up 2 hours   127.0.0.1:27017->27017/tcp                   hotelDB
```

## Comprobar que la aplicación funciona

> Para comprobar que la aplicación funciona correctamente podemos probar en un navegador:
> 
> - `http://amorenoiborra.es:3000/limpieza/sucias`
>   - Este endpoint nos devuelve las habitaciones que no han sido limpiadas el día de hoy.

```JSON
 {
   "habitaciones": [
     {
       "_id": "1a1a1a1a1a1a1a1a1a1a1a1a",
       "numero": 1,
       "tipo": "doble",
       "descripcion": "Habitación doble, cama XL, terraza con vistas al mar",
       "ultimaLimpieza": "2023-09-20T11:24:00.000Z",
       "precio": 59.9,
       "incidencias": [
         {
           "descripcion": "No funciona el aire acondicionado",
           "inicio": "2023-09-19T18:12:54.000Z",
           "_id": "10011a1a1a1a1a1a1a1a1a1a"
         },
         {
           "descripcion": "No funciona el interruptor del aseo",
           "inicio": "2023-09-20T10:15:06.000Z",
           "_id": "10021a1a1a1a1a1a1a1a1a1a"
         }
       ],
       "__v": 0
     },
     {
       "_id": "2b2b2b2b2b2b2b2b2b2b2b2b",
       "numero": 2,
       "tipo": "familiar",
       "descripcion": "Habitación familiar, cama XL y literas, aseo con bañera",
       "ultimaLimpieza": "2023-08-02T10:35:15.000Z",
       "precio": 65.45,
       "incidencias": [],
       "__v": 0
     },
     {
       "_id": "3c3c3c3c3c3c3c3c3c3c3c3c",
       "numero": 3,
       "tipo": "familiar",
       "descripcion": "Habitación familiar, cama XL y sofá cama, cocina con nevera",
       "precio": 69.15,
       "ultimaLimpieza": "2024-05-18T08:57:42.500Z",
       "incidencias": [],
       "__v": 0
     },
     {
       "_id": "4d4d4d4d4d4d4d4d4d4d4d4d",
       "numero": 4,
       "tipo": "suite",
       "descripcion": "Habitación con dos camas XL, terraza y vistas al mar",
       "ultimaLimpieza": "2023-10-10T12:05:10.000Z",
       "precio": 110.2,
       "incidencias": [
         {
           "descripcion": "No funciona el jacuzzi",
           "inicio": "2023-10-08T19:24:43.000Z",
           "_id": "10014d4d4d4d4d4d4d4d4d4d"
         }
       ],
       "__v": 0
     }
   ]
 }
 ```
{collapsible="true" collapsed-title-line-number="2"}

 
> - `http://amorenoiborra.es:3000/limpieza/limpias`
>   - Este endpoint nos devuelve las habitaciones que han sido limpiadas el día de hoy.
```JSON
{
  "habitaciones": [
    {
      "_id": "5e5e5e5e5e5e5e5e5e5e5e5e",
      "numero": 5,
      "tipo": "individual",
      "descripcion": "Habitación simple, cama 150",
      "precio": 34.65,
      "ultimaLimpieza": "2024-05-28T08:57:42.501Z",
      "incidencias": [],
      "__v": 0
    }
  ]
}
```
{collapsible="true" collapsed-title-line-number="2"}
