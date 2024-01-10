ARG HUB_BASE_IMAGE
FROM ${HUB_BASE_IMAGE}

#ARG CERTBOT_EMAIL
#ARG CERTBOT_DOMAIN

# Copy the JupyterHub configuration in the container
COPY jupyterhub_config.py .

# Install certbot deps
RUN apt-get update --yes && apt-get install --yes --no-install-recommends \
    libaugeas0 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
# Install certbot
RUN pip install --upgrade pip \
    && pip install certbot certbot \
    && ln -s /usr/local/bin/certbot /usr/bin/certbot
# Run certbot
#RUN certbot certonly --standalone -n --agree-tos --email ${CERTBOT_EMAIL} --domains ${CERTBOT_DOMAIN}

# Install dependencies
RUN pip install \
    #dockerspawner==13.0 \
    #jupyterhub-firstuseauthenticator==1.0 \
    #jupyterhub-idle-culler==1.2
    dockerspawner \
    jupyterhub-firstuseauthenticator \
    jupyterhub-idle-culler
