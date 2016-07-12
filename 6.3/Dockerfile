FROM openshift/base-centos7

EXPOSE 8080

MAINTAINER smartux

ENV NODE_VERSION 6.3.0-1 \
    NPM_RUN start \                                                             
    NPM_CONFIG_PREFIX $HOME/.npm-global \
    NODE_ENV production \   
    PATH $HOME/node_modules/.bin/:$HOME/.npm-global/bin/:$PATH

LABEL summary="Platform for building and running Node.js $NODE_VERSION applications" \
      io.k8s.description="Platform for building and running Node.js $NODE_VERSION applications" \
      io.k8s.display-name="Node.js $NODE_VERSION" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,nodejs,nodejs6" \
      com.redhat.dev-mode="DEV_MODE:false" \
      com.redhat.deployments-dir="/opt/app-root/src" \
      com.redhat.dev-mode.port="DEBUG_PORT:5858"

RUN curl -sL -o ns.rpm "https://rpm.nodesource.com/pub_6.x/el/7/x86_64/nodejs-$NODE_VERSIONnodesource.el7.centos.x86_64.rpm" \
&& rpm -i --nosignature --force ns.rpm \
&& rm -f ns.rpm

RUN npm install -g pangyp\
 && ln -s $(which pangyp) $(dirname $(which pangyp))/node-gyp\
 && npm cache clear\
 && node-gyp configure || echo ""

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# Each language image can have 'contrib' a directory with extra files needed to
# run and build the applications.
COPY ./contrib/ /opt/app-root

# Drop the root user and make the content of /opt/app-root owned by user 1001
RUN chown -R 1001:0 /opt/app-root && chmod -R ug+rwx /opt/app-root
USER 1001

# Set the default CMD to print the usage of the language image
CMD $STI_SCRIPTS_PATH/usage
