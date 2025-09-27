FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm install --production
COPY . .
# Build the React app
RUN npm run build

FROM nginx:alpine
WORKDIR /usr/share/nginx/html
COPY --from=build /app/build .
# Copy custom nginx config if needed
# COPY nginx.conf /etc/nginx/nginx.conf

# Environment variables (for build-time only)
# See .env.example for reference
# REACT_APP_API_URL, REACT_APP_WS_URL, etc. should be set before build

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
