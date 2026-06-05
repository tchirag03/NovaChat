# Use Nginx to serve the static built files
FROM nginx:alpine

# Copy custom nginx configuration to support React Router client-side routing (SPAs)
# This prevents 404 errors on page reload or direct navigation (e.g. /login, /chat)
RUN echo 'server { \
    listen 80; \
    location / { \
        root /usr/share/nginx/html; \
        index index.html index.htm; \
        try_files $uri $uri/ /index.html; \
    } \
}' > /etc/nginx/conf.d/default.conf

# Copy the build output from the Jenkins workspace into Nginx's public folder
# Vite builds files into a folder named 'dist'
COPY dist /usr/share/nginx/html

# Expose port 80 for the web traffic
EXPOSE 80


CMD ["nginx", "-g", "daemon off;"]
