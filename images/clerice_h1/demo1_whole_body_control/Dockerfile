# Base image for clerice_h1 / demo1_whole_body_control.
# Replace this placeholder with the experiment's actual base image definition.
# Per RAICo's Docker standards, SAS-dependent containers must derive from
# murilomarinho/sas:jazzy.
# https://github.com/Adorno-Lab/development-guidelines/tree/main/3-docker-standards

FROM murilomarinho/sas:jazzy

LABEL org.opencontainers.image.source="https://github.com/Adorno-Lab/raico_base_images"
LABEL org.opencontainers.image.description="Base image for clerice_h1 / demo1_whole_body_control"
LABEL org.opencontainers.image.licenses="LGPL-2.1-only"

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

CMD ["/bin/bash"]
