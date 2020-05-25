# base image
FROM nginx:1.17

# maintainer
LABEL maintainer="nilosoares@gmail.com"

# installs dependencies
RUN apt-get update && apt-get install -y \
    apt-utils \
    vim

# expose ports
EXPOSE 80