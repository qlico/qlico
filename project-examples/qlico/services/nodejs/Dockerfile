FROM node:22

RUN mkdir -p /var/www/html
RUN chown node:node /var/www/html

WORKDIR /var/www/html

# Run commands inside this container as the created NODE user.
USER node

# Define default command.
CMD ["/bin/bash"]
