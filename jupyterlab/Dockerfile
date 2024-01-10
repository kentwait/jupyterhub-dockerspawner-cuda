ARG LAB_BASE_IMAGE
FROM ${LAB_BASE_IMAGE}

ARG HUB_VERSION
ARG LAB_VERSION

USER root
RUN apt-get update --yes && apt-get install --yes --no-install-recommends \
    build-essential \
    bzip2 \
    cmake \
    curl \
    fonts-font-awesome \
    fonts-powerline \
    git \
    htop \
    language-pack-ja-base language-pack-ja \
    libncurses5-dev libncursesw5-dev \
    libdeflate0 \
    libhts3 libhtscodecs2 \
    libpq5 \
    nvtop \
    postgresql-client \
    rustc \
    tabix \
    zlib1g-dev \
    zsh \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

USER ${NB_UID}
RUN mamba install --yes \
    "jupyterhub==${HUB_VERSION}" \
    "jupyterlab==${LAB_VERSION}" \
    && mamba clean --all -f -y\
    && fix-permissions $CONDA_DIR

