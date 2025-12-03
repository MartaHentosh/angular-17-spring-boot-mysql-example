# Stage 1: Build 
FROM node:18-alpine AS build

WORKDIR /app

COPY package*.json ./
# Install dependencies
RUN npm ci
# Copy application source code
COPY . .

# Build the application 
RUN npm run build


# Stage 2: Runtime
FROM nginx:alpine
# Copy nginx.conf (or minimalistic nginx2.conf)
COPY nginx.conf /etc/nginx/conf.d/default.conf
# Copy from build stage
COPY --from=build /app/dist/angular-17-crud/browser /usr/share/nginx/html

EXPOSE 8081

CMD ["nginx", "-g", "daemon off;"]
