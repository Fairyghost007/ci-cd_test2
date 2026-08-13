# base image for build stage: lightweight Node.js
FROM node:lts-alpine as build        
# set working directory inside container
WORKDIR /app                         
# copy package.json and package-lock.json first (for layer caching)
COPY package*.json ./                
# install dependencies
RUN npm install                      
# copy rest of the source code
COPY . .                             
# build the app into /app/dist
RUN npm run build-only               
# base image for final stage: lightweight Nginx
FROM nginx:stable-alpine             
 # copy built static files from build stage
COPY --from=build /app/dist /usr/share/nginx/html  
# copy custom nginx config into container
# COPY nginx.conf /etc/nginx/conf.d/default.conf       
COPY nginx.conf.template /etc/nginx/templates/default.conf.template
# document that container listens on port 80
EXPOSE 80                    
 # start nginx in foreground so container stays running        
CMD ["nginx","-g","daemon off;"]    