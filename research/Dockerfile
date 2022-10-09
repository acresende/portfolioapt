FROM jupyter/scipy-notebook

USER root

RUN apt-get update && \
    apt-get install -y libpq-dev && \
    apt-get clean && rm -rf var/lib/apt/lists/*

USER $NB_UID

RUN set -ex \
   && conda install --quiet --yes \
   # escolhe os pacotes Python que você precisa
   'plotly' \
   'folium' \
   && conda clean --all -f -y \
   # instala as extensões do Jupyter Lab que você precisa
   && jupyter labextension install jupyterlab-plotly --no-build \
   && jupyter lab build --dev-build=False --minimize=False -y \
   && jupyter lab clean -y \
   && rm -rf "/home/${NB_USER}/.cache/yarn" \
   && rm -rf "/home/${NB_USER}/.node-gyp" \
   && fix-permissions "${CONDA_DIR}" \
   && fix-permissions "/home/${NB_USER}"

# RUN conda install --quiet --yes \
#     'r-lme4' && \
#     conda clean --all -f -y && \
#     fix-permissions "${CONDA_DIR}" && \
#     fix-permissions "/home/${NB_USER}"

# Install packages requirements
COPY requirements.txt requirements.txt
RUN pip install --upgrade pip
RUN pip install -r requirements.txt

WORKDIR /mnt/code
