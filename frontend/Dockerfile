# Use a Node.js image to build the React app
FROM node:16.16.0-alpine as build

# Set the working directory for the React app build
WORKDIR /app

# Copy package.json and package-lock.json (if available) to install dependencies
COPY ./frontend/package*.json ./

# Install dependencies for React (version 16)
RUN npm install

# Copy the rest of the React app source code into the container
COPY ./frontend ./

# Build the React app for production
RUN npm run build

# Now, we create the final image for serving the app with Nginx
FROM ubuntu:18.04

# Set environment variables to non-interactive to avoid prompts during installation
ENV DEBIAN_FRONTEND=noninteractive

# Install Nginx, curl, vim, and Certbot for SSL
RUN apt update -y \
    && apt install nginx curl vim -y \
    && apt-get install software-properties-common -y \
    && add-apt-repository ppa:certbot/certbot -y \
    && apt-get update -y \
    && apt-get install python-certbot-nginx -y \
    && apt-get clean

# Expose HTTP and HTTPS ports
EXPOSE 80 443

# Set the stop signal for graceful shutdown
STOPSIGNAL SIGTERM

# Copy custom Nginx configuration (if exists)
COPY ./deploy/default.conf /etc/nginx/sites-available/default

# Copy the build output from the React build stage into the Nginx web directory
COPY --from=build /app/build /var/www/html

# Set the command to run Nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]
