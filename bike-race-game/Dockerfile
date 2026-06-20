# ---- Bike Race Game - Docker Image ----
FROM nginx:alpine

# Copy game files
COPY index.html /usr/share/nginx/html/index.html

# Copy nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
