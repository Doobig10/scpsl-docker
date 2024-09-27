FROM ubuntu:20.04
LABEL com.ajb-software.image.authors="Aidan Brooks"
LABEL com.ajb-software.image.info="SCP: Secret Laboratory Docker Server"
USER root

# # Arguments
# # --------------------------------------------------

ARG ROOT=root
ARG USER=scpsl
ARG GID=22035
ARG WORK_DIR=/home/${USER}

ARG STEAM_DIR=.steamcmd
ARG STEAM_SRC=https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz

ARG BIN_DIR=bin
ARG SERVER_DIR=server
ARG CONFIG_DIR=config

#ARG EXILED_SRC=https://github.com/Exiled-Team/EXILED/releases/latest/download/Exiled.Installer-Linux
#ARG EXILED_FILE=Exiled.Installer-Linux

ARG EASY_CONFIG_DIR=/config
#ARG EASY_STEAM_DIR=/steamcmd

# # Setup Phase
# # --------------------------------------------------

RUN set -x

RUN apt-get update --no-install-recommends --no-install-suggests \
    && apt-get install -y \
        wget \
        ca-certificates \
        lib32stdc++6 \
        lib32gcc1 

RUN wget 'http://ftp.us.debian.org/debian/pool/main/i/icu/libicu63_63.1-6+deb10u3_amd64.deb'

# # Main Phase
# # --------------------------------------------------

RUN groupadd -rg ${GID} ${USER} \
    && useradd ${USER} -g ${GID} -m

RUN mkdir ${EASY_CONFIG_DIR} && \
    chown ${USER} ${EASY_CONFIG_DIR}

#RUN mkdir ${EASY_STEAM_DIR} && \
#    chown ${USER} ${EASY_STEAM_DIR}

USER ${USER}
WORKDIR ${WORK_DIR}

RUN mkdir ${STEAM_DIR} && \
    wget -qO- ${STEAM_SRC} | tar zxf - -C ${STEAM_DIR}

RUN mkdir ${BIN_DIR} && \
    mkdir ${SERVER_DIR} && \
    ln -sf ${CONFIG_DIR} .config

#RUN wget -P ${SERVER_DIR} --no-check-certificate ${EXILED_SRC}\
#    && chmod +x ${EXILED_FILE}

ADD --chown=${USER}:${GID} scripts ${BIN_DIR}
ADD --chown=${USER}:${GID} defaults ${CONFIG_DIR}


# # Run
# # --------------------------------------------------
# Copy ARGS to ENV to allow continued access to values
# Initialise start-up specific ENV values
# Configure ports and volumes
# Set entrypoint & commands

# Convert ARG to ENV

ENV USER=${USER}
ENV GID=${GID}

ENV WORK_DIR=${WORK_DIR}
ENV STEAM_DIR=${STEAM_DIR}
ENV BIN_DIR=${BIN_DIR}
ENV SERVER_DIR=${SERVER_DIR}
ENV CONFIG_DIR=${CONFIG_DIR}

ENV EASY_CONFIG_DIR=${EASY_CONFIG_DIR}
#ENV EASY_STEAM_DIR=${EASY_STEAM_DIR}

# Standard Envs

ENV ENABLE_PATCHES=true
ENV SCPSL_PORT=7777

ENV ADMIN_OWNER_ID=
ENV ADMIN_PASSWORD=
ENV ADMIN_PASSWORD_ROLE=moderator

EXPOSE ${SCPSL_PORT}


USER ${ROOT}
ENTRYPOINT ["entrypoint"]
CMD ["start-server"]


