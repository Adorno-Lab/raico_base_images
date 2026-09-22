FROM murilomarinho/coppeliasim:latest
ENV BASH_ENV="/etc/bash_env"

# Update apt-get
RUN apt-get update -q \
    && apt-get upgrade -y \
    && apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*