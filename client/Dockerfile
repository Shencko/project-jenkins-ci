# Stage 1: Build React App
FROM node:18-alpine AS build

WORKDIR /app
COPY package*.json ./

# Install dependencies with verbose output
RUN npm install --verbose

COPY . .

# Optional: pass REACT_APP_API as build arg
ARG REACT_APP_API
ENV REACT_APP_API=$REACT_APP_API

# Build with verbose output to see any errors
RUN npm run build

# Stage 2: Serve with NGINX
FROM nginx:alpine

# Copy correct default.conf to override nginx default
COPY default.conf /etc/nginx/conf.d/default.conf

# Copy React build output
COPY --from=build /app/build /usr/share/nginx/html

# Prevent directory listings and ensure correct permissions
RUN chmod -R 755 /usr/share/nginx/html

# Add health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost/ || exit 1

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
