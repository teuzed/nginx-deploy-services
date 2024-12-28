
FROM node:16.16.0-alpine as build

# Etapa de construcción (React app)
FROM node:16.16.0-alpine as build

WORKDIR /app

# Copiar las dependencias del proyecto
COPY frontend/package*.json ./

# Instalar las dependencias
RUN npm install

# Copiar el código fuente del proyecto
COPY ./frontend ./

# Construir la aplicación
RUN npm run build

# Etapa de producción con Nginx
FROM ubuntu:18.04

ENV DEBIAN_FRONTEND=noninteractive

# Instalar Nginx y otras dependencias necesarias
RUN apt update -y && apt install -y nginx curl vim software-properties-common \
    && add-apt-repository ppa:certbot/certbot -y \
    && apt update -y \
    && apt install -y python-certbot-nginx \
    && apt clean

# Exponer los puertos HTTP y HTTPS
EXPOSE 80 443

# Eliminar el archivo por defecto en sites-enabled si existe
RUN rm -f /etc/nginx/sites-enabled/default

# Crear el enlace simbólico
RUN ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/

# Copiar la configuración de Nginx
COPY deploy/default.conf /etc/nginx/sites-available/default

# Copiar la build de React desde la etapa de construcción
COPY --from=build /app/build /var/www/html

# Iniciar Nginx
CMD ["nginx", "-g", "daemon off;"]
