FROM node:0.12.12

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
COPY ./s2i/bin/ /usr/libexec/s2i

# Add script to fix directory permissions 
COPY bin/fix-permissions /usr/bin/fix-permissions

RUN chown -R 1001:0 /usr/src/app \
    && useradd -u 1001 -r -g 0 -d ${HOME} -s /sbin/nologin \
      -c "Default Application User" default

USER 1001

CMD /usr/libexec/s2i/usage
