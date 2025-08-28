# Stage 1: Build Angular App
FROM node:18 AS build

WORKDIR /app

# Copy package files first (better caching)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy rest of the app
COPY . .

# Build Angular (production build)
RUN npm run build --prod

# --- Stage: Serve Pre-built Angular App ---
FROM nginx:alpine


# Remove default nginx static assets
RUN rm -rf /usr/share/nginx/html/*

# Copy already built Angular dist folder from host to container
COPY dist/hrms/browser/  /usr/share/nginx/html/

# Copy custom nginx config if needed
COPY default.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

# Start nginx in foreground
CMD ["nginx", "-g", "daemon off;"]


