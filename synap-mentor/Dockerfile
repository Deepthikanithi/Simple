# Stage 1: Build the React app
FROM node:20 AS builder
 
# Set working directory
WORKDIR /app
 
# Copy package files first for layer caching
COPY package*.json ./
 
# Install dependencies
RUN npm ci
 
# Copy the rest of the application
COPY . .
 
# Build the production-ready React app
RUN npm run build
 
# -----------------------------------------
 
# Stage 2: Serve with NGINX
FROM nginx:alpine
 
# Remove the default nginx static assets
RUN rm -rf /usr/share/nginx/html/*
 
# Copy build output to NGINX's default directory
COPY --from=builder /app/build /usr/share/nginx/html
 
# Copy custom nginx configuration (optional)
# COPY nginx.conf /etc/nginx/nginx.conf
 
# Expose port 80
EXPOSE 80
 
# Start NGINX
CMD ["nginx", "-g", "daemon off;"]