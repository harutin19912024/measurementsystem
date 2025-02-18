# Use Node.js for building the Vue app
FROM node:18 AS builder

# Set working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application files
COPY . .

# Build the Vue app for production
RUN npm run build

# ------------------------------------
# Use Nginx to serve the built Vue app
FROM nginx:latest

# Set working directory inside Nginx
WORKDIR /usr/share/nginx/html

# Remove the default Nginx static files
RUN rm -rf ./*

# Copy built files from the previous stage
COPY --from=builder /app/dist .

# Copy custom Nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80 for Nginx
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
