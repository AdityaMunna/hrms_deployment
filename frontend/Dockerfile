
# --- Stage: Serve Pre-built Angular App ---
FROM nginx:alpine


# Remove default nginx static assets
RUN rm -rf /usr/share/nginx/html/*

# Copy already built Angular dist folder from host to container
COPY --from=build /app/dist/hrms-frontend/browser/ /usr/share/nginx/html/

# Copy custom nginx config if needed
COPY default.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

# Start nginx in foreground
CMD ["nginx", "-g", "daemon off;"]


