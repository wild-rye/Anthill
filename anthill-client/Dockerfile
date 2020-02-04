FROM node:13.6.0-stretch as build

WORKDIR /usr/src/app

# Copy the package.json as well as the package-lock.json and install 
# the dependencies. This is a separate step so the dependencies 
# will be cached unless changes to one of those two files 
# are made.
COPY package.json package-lock.json ./
RUN npm install

# Copy the main application
COPY . ./

# Arguments
ARG API_BASE_URL
ENV API_BASE_URL=${API_BASE_URL}

ARG API_DOMAIN
ENV API_DOMAIN=${API_DOMAIN}

# Build the application
RUN npm run build-prod

FROM nginx:1.17.7

# Copy the build from Stage 1
COPY --from=build /usr/src/app/dist/anthill-client /usr/share/nginx

# Copy our custom nginx config
COPY nginx.conf /etc/nginx/nginx.conf

# Expose port 80 to the Docker host, so we can access it 
# from the outside.
EXPOSE 80

ENTRYPOINT ["nginx","-g","daemon off;"]