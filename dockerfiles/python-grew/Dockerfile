FROM quay.io/jupyter/datascience-notebook:hub-5.2.1

USER root

# --- Install dependencies ---
RUN apt-get update && apt-get install -y --no-install-recommends \
      opam m4 unzip bubblewrap build-essential pkg-config libpcre3-dev \
      libgtk-3-dev curl git librsvg2-bin libgmp-dev zlib1g-dev \
      libsqlite3-dev ca-certificates && \
    rm -rf /var/lib/apt/lists/*

# --- Configure OPAM ---
ENV OPAMROOT=/opt/opam
ENV OPAMYES=1
ENV HOME=/opt/opam_home
RUN mkdir -p $HOME && chown -R root:root $HOME

# --- Initialise OPAM + switch OCaml 5.2.0 + Grew ---
RUN opam init --disable-sandboxing -y && \
    bash -lc "opam switch create 5.2.0 ocaml-base-compiler.5.2.0" && \
    bash -lc "opam repo add grew https://opam.grew.fr" && \
    bash -lc "opam update" && \
    bash -lc "opam install -y grew grewpy_backend"
RUN cp /opt/opam/5.2.0/bin/grewpy_backend /usr/local/bin/

# --- Install grewpy ---
RUN pip install grewpy

# --- Permissions ---
RUN fix-permissions $OPAMROOT && fix-permissions $HOME
USER $NB_UID
WORKDIR /home/<YOUR_USERNAME>/work

# --- OPAM in bash (for debugs) ---
RUN echo "export OPAMROOT=/opt/opam" >> ~/.bashrc && \
    #echo "export HOME=/opt/opam_home" >> ~/.bashrc && \
    echo "eval \$(opam env)" >> ~/.bashrc

ENV HOME=/home/<YOUR_USERNAME>