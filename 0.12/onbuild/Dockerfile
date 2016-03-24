FROM node:0.12.12


# Location of the STI scripts inside the image
#
LABEL io.openshift.s2i.scripts-url=image:///usr/libexec/s2i

# DEPRECATED: This label will be kept here for backward compatibility
LABEL io.s2i.scripts-url=image:///usr/libexec/s2i

# Deprecated. Use above LABEL instead, because this will be removed in future versions.
ENV STI_SCRIPTS_URL=image:///usr/libexec/s2i

# Path to be used in other layers to place s2i scripts into
ENV STI_SCRIPTS_PATH=/usr/libexec/s2i

ENV HOME=/usr/src/app

LABEL io.k8s.description="Platform for building and running Node.js 0.12 applications" \
      io.k8s.display-name="Node.js 0.12" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,nodejs,nodejs012"
      

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

ONBUILD COPY package.json /usr/src/app/
ONBUILD RUN npm install
ONBUILD COPY . /usr/src/app

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# Add script to fix directory permissions 
COPY bin/fix-permissions /usr/bin/fix-permissions

RUN chown -R 1001:0 /usr/src/app \
    && useradd -u 1001 -r -g 0 -d ${HOME} -s /sbin/nologin \
      -c "Default Application User" default

USER 1001

CMD [ "npm", "start" ]
