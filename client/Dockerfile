# Base image
FROM node:16-alpine as build

# Set the working directory
WORKDIR /client

# Copy package.json and package-lock.json
COPY package.json .

# Install dependencies
RUN npm ci --only=production

# Copy application code
COPY . .

# Build the application
RUN npm run build

# Stage 2: Nginx setup
FROM nginx:1.21-alpine

# Remove default Nginx configuration
RUN rm -rf /etc/nginx/conf.d/*

# Copy custom Nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy built application from the previous stage
COPY --from=build /client/dist /usr/share/nginx/html

# Expose the port
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
