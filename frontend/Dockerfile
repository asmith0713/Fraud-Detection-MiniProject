FROM node:20-alpine AS build
WORKDIR /app

# Install dependencies first to leverage Docker layer caching
COPY package*.json ./
RUN npm install

# Copy the rest of the app and build for production. Allow overriding the API base.
COPY . .
ARG REACT_APP_API_BASE=http://localhost:5000
ENV REACT_APP_API_BASE=${REACT_APP_API_BASE}
RUN npm run build

# Serve the compiled static assets via nginx
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]