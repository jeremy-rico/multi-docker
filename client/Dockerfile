# This is a multistep Dockerfile
# The first step builds the node image, the second step copies the contents
# of that image and serves it with nginx.
# builder is the name that we give to this step
FROM node:16-alpine as builder 
WORKDIR '/app'
COPY ./package.json ./
RUN npm install
COPY . .
RUN npm run build
# Why RUN and not CMD? RUN is a image build step. It creates a new image
# with the result of the run command. CMD is a default command that runs when
# the image has finished building. Since this is a two step process, we 
# use RUN to create a new image with the contents of the built npm files.
#
# The resuls of the line is that all the production assets get placed into:
#
# /app/build
#
# inside the container (since we specified /app as our working directory)

# What we want to do now is COPY that build directory over to the nginx
# step of our build

FROM nginx
EXPOSE 3000
# Copy over out nginx config file
# The container path is the default nginx directory where it looks
# for the config file. We are actually overwriting the default conf
# that it creates.
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
# Copy over all the production assets from the builder phase
# Again, the container path is the default location nginx looks
# for assets.
COPY --from=builder /app/build /usr/share/nginx/html


