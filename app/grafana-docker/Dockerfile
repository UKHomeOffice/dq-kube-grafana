ARG GRAFANA_VERSION=${GRAFANA_VERSION}

LABEL maintainer="dqdevops@homeoffice.gsi.gov.uk"

FROM grafana/grafana:${GRAFANA_VERSION}-ubuntu

USER root

RUN set -x && usermod -u ${USERMAP_UID} grafana

# Set DEBIAN_FRONTEND=noninteractive in environment at build-time
ARG DEBIAN_FRONTEND=noninteractive

ARG GF_INSTALL_IMAGE_RENDERER_PLUGIN="true"

ENV GF_PATHS_PLUGINS="/var/lib/grafana-plugins"

RUN mkdir -p "$GF_PATHS_PLUGINS" && \
    chown -R grafana:grafana "$GF_PATHS_PLUGINS"

RUN if [ $GF_INSTALL_IMAGE_RENDERER_PLUGIN = "true" ]; then \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y gdebi-core && \
    cd /tmp && \
    curl -LO https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && \
    gdebi --n google-chrome-stable_current_amd64.deb && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/*; \
fi

ENV USERMAP_UID 1000

USER grafana

ENV GF_RENDERER_PLUGIN_CHROME_BIN="/usr/bin/google-chrome"

RUN if [ $GF_INSTALL_IMAGE_RENDERER_PLUGIN = "true" ]; then \
    grafana-cli \
        --pluginsDir "$GF_PATHS_PLUGINS" \
        --pluginUrl https://github.com/grafana/grafana-image-renderer/releases/latest/download/plugin-linux-x64-glibc-no-chromium.zip \
        plugins install grafana-image-renderer; \
fi

ARG GF_INSTALL_PLUGINS=""

RUN if [ ! -z "${GF_INSTALL_PLUGINS}" ]; then \
    OLDIFS=$IFS; \
        IFS=','; \
    for plugin in ${GF_INSTALL_PLUGINS}; do \
        IFS=$OLDIFS; \
        grafana-cli --pluginsDir "$GF_PATHS_PLUGINS" plugins install ${plugin}; \
    done; \
fi
