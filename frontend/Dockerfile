# --- STAGE 1: Build the Angular App ---
FROM node:18-alpine AS builder

# Set working directory inside the container
WORKDIR /app

# Copy only dependency-related files first for better caching
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy rest of the app source code
COPY . .

# Build Angular app (fail if build fails)
RUN npm run build --prod

# --- STAGE 2: Serve with NGINX ---
FROM nginx:alpine

# Remove default nginx static assets
RUN rm -rf /usr/share/nginx/html/*

# Copy Angular app output from Stage 1
COPY --from=builder /app/dist/hrms/browser /usr/share/nginx/html

# Copy custom nginx config (optional, see below)
# COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

# Start nginx in foreground
CMD ["nginx", "-g", "daemon off;"]

