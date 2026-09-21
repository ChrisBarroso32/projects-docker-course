# Define la imagen base a partir de la cual se creará la imagen de la aplicación React.
FROM node:20-alpine

# Crea un usuario con permisos para ejecutar la aplicación.
# -S -> crea un usuario del sistema.
# -G -> añade el usuario a un grupo.
# Esto evita ejecutar la aplicación como root.
# Si la aplicación se ejecuta como root, una vulnerabilidad podría aprovecharse
# para obtener acceso al sistema anfitrión.
# Ejecutar la aplicación con un usuario distinto de root es una buena práctica de seguridad.
RUN addgroup app && adduser -S -G app app

# Define el usuario que ejecutará la aplicación.
USER app

# Define /app como directorio de trabajo.
WORKDIR /app

# Copia package.json y package-lock.json al directorio de trabajo.
# Se hace antes de copiar el resto de los archivos para aprovechar la caché de Docker.
# Si package.json y package-lock.json no han cambiado, Docker reutilizará las dependencias almacenadas en caché.
COPY package*.json ./

# A veces, los archivos del directorio de trabajo quedan asignados a root.
# En ese caso, la aplicación no puede acceder a ellos y muestra el error EACCES: permission denied.
# Para corregirlo, cambiaremos la propiedad de los archivos mientras usamos root.
USER root

# Cambia la propiedad del directorio /app al usuario app.
# chown -R <user>:<group> <directory>
# El comando chown cambia el usuario y/o grupo propietario de un archivo o directorio.
RUN chown -R app:app .

# Vuelve a usar el usuario app.
USER app

# Instala las dependencias.
RUN npm install

# Copia el resto de los archivos al directorio de trabajo.
COPY . .

# Informa a Docker de que el contenedor escucha en el puerto 5173 durante la ejecución.
EXPOSE 5173

# Comando para iniciar la aplicación.
CMD npm run dev