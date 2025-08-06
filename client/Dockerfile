# Stage 1: Build React App
FROM node:18-alpine AS build

WORKDIR /app
COPY package*.json ./

# clean and fast install using lock file (CI-style)
RUN npm ci

COPY . .

# Build with verbose output to see any errors
RUN npm run build

# Stage 2: Serve with NGINX
FROM nginx:alpine

# Install curl for debugging
RUN apk add --no-cache curl

# Copy React build output to nginx default location
COPY --from=build /app/build /usr/share/nginx/html

# Prevent directory listings and ensure correct permissions
RUN chmod -R 755 /usr/share/nginx/html

# Add health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost/ || exit 1

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
