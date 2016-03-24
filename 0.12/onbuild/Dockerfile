FROM node:0.12.12

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

ONBUILD COPY package.json /usr/src/app/
ONBUILD RUN npm install
ONBUILD COPY . /usr/src/app

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ /usr/libexec/s2i

USER 1001

#CMD [ "npm", "start" ]
CMD /usr/libexec/s2i/usage
