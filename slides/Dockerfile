FROM node:0.10.38-slim

MAINTAINER Manuel Tiago Pereira <me@mtpereira.com>

RUN export DEBIAN_FRONTEND='noninteractive' && \
    apt-get update && \
    apt-get -qq --no-install-recommends install ca-certificates git bzip2 && \
    git clone https://github.com/hakimel/reveal.js.git /revealjs && \
    cd /revealjs && \
    npm install -g grunt-cli && \
    npm install && \
    sed -i Gruntfile.js -e 's/port: port,/port: port, hostname: "",/' && \
    apt-get purge -qq ca-certificates git bzip2 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /revealjs

COPY index.html /revealjs/
COPY css/ /revealjs/css/
COPY plugin/ /revealjs/plugin/
COPY md/ /revealjs/md/

EXPOSE 8000
ENTRYPOINT ["grunt", "serve"]

