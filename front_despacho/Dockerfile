#creo la maquina virutual
FROM node:18-alpine AS build

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY  . .
#compila el proyecto
RUN npm run build


#stage 2
FROM nginx:alpine AS production

COPY --from=build /app/dist /usr/share/nginx/html

COPY nginx.conf /etc/nginx/templates/default.conf.template

# 🟢 CAMBIO 1: Exponer un puerto alto (mayor a 1024) para el usuario no-root
EXPOSE 8080

# Restringir privilegios del contenedor a usuario no-root
USER nginx

# 🟢 CAMBIO 2: Le decimos a Nginx que escuche en el puerto 8080 usando variables de entorno de su imagen oficial
ENV NGINX_PORT=8080

CMD ["nginx", "-g", "daemon off;"]
