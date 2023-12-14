## Replace ovos_skill_hello_world code in docker-compose.skills.yml with following code

  ovos_skill_hello_world:  
    <<: *podman
    container_name: ovos_hello_world_skill
    hostname: ovos_hello_world_skill
    restart: unless-stopped
    build:
      context: ../skills/skill-hello-world
      dockerfile: Dockerfile
    logging: *default-logging
    pull_policy: $PULL_POLICY
    environment:
      TZ: $TZ
    network_mode: host
    volumes:
      - ${OVOS_CONFIG_FOLDER}:/home/${OVOS_USER}/.config/mycroft
      - ${TMP_FOLDER}:/tmp/mycroft
    depends_on:
      ovos_core:
        condition: service_started

####

## ovos-docker/skills/skill-hello-world    replace the dockerfile code with following code

ARG TAG=alpha
FROM smartgic/ovos-skill-base:${TAG}

ARG BUILD_DATE=07/12/2023
ARG VERSION=2.5

LABEL org.opencontainers.image.title="Open Voice OS OCI alerts skill image"
LABEL org.opencontainers.image.description="A skill to schedule alarms, timers, and reminders"
LABEL org.opencontainers.image.version=${VERSION}
LABEL org.opencontainers.image.created=${BUILD_DATE}
LABEL org.opencontainers.image.documentation="https://openvoiceos.github.io/community-docs"
LABEL org.opencontainers.image.source="https://github.com/OpenVoiceOS/ovos-docker"
LABEL org.opencontainers.image.vendor="Open Voice OS"

ARG ALPHA=true

RUN if [ "${ALPHA}" == "true" ]; then \
    pip3 install git+https://github.com/ravindukathri/skill-ovos-hello-world.git; \
    fi \
    && rm -rf "${HOME}/.cache"

ENTRYPOINT ["ovos-skill-launcher", "skill-ovos-hello-world.ravindukathri"]
