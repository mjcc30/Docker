FROM ubuntu AS build
ARG DEBIAN_FRONTEND=noninteractive

# Update
RUN apt-get -y update --fix-missing && apt-get upgrade -y && rm -rf /var/lib/apt/lists/*
RUN apt-get -y update && apt-get -y --no-install-recommends install apt-utils locales

# locales
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen &&     echo "fr_FR.UTF-8 UTF-8" >> /etc/locale.gen &&     locale-gen

# Utils
RUN apt-get install -y .gyp python gcc g++ make build-essential libpng-dev curl autoconf libtool gettext  autotools-dev automake
RUN curl -sL https://deb.nodesource.com/setup_current.x | bash -
RUN apt-get --fix-broken install nodejs  -y

RUN npm i -g npm@latest
RUN npm cache clean -f && npm install -g n && node -v && npm -v
RUN curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor | tee /usr/share/keyrings/yarnkey.gpg >/dev/null \
    && echo "deb [signed-by=/usr/share/keyrings/yarnkey.gpg] https://dl.yarnpkg.com/debian stable main" | tee /etc/apt/sources.list.d/yarn.list \
    && apt-get update && apt-get install yarn -y
RUN yarn add global gatsby-cli
WORKDIR /app
COPY ./app/ .

# yarn
RUN yarn  --non-interactive
RUN yarn build

FROM httpd:2.4
COPY --from=build --chown=www-data:www-data /app/public /usr/local/apache2/htdocs/
