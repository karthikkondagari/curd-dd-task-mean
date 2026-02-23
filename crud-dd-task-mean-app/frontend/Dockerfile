# Stage 1 - Build
FROM node:18 AS build

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install -g @angular/cli
RUN npm install

COPY . .

RUN npm run build -- --configuration production


# Stage 2 - Serve with Nginx
FROM nginx:alpine

COPY --from=build /usr/src/app/dist/angular-15-crud/ /usr/share/nginx/html

EXPOSE 80