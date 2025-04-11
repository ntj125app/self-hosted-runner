FROM ubuntu:latest

ARG RUNNER_VERSION="2.323.0"
ARG DEBIAN_FRONTEND=noninteractive

# Update and upgrade the system
RUN apt update -y && apt upgrade -y

# Add a user named docker
RUN useradd -m docker

# Install necessary packages
RUN apt install -y --no-install-recommends \
    curl build-essential libssl-dev libffi-dev python3 python3-venv python3-dev python3-pip jq git curl wget gzip tar zip unzip sed awk grep find
RUN apt-get -yqq install ssh
# Set up the actions runner
RUN cd /home/docker && mkdir actions-runner && cd actions-runner \
    && curl -o actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz -L https://github.com/actions/runner/releases/download/v${RUNNER_VERSION}/actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz \
    && tar xzf actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz

# Change ownership to docker user and install dependencies
RUN chown -R docker /home/docker && /home/docker/actions-runner/bin/installdependencies.sh

# Copy the start script and make it executable
COPY start.sh /start.sh
RUN chmod +x /start.sh

# Switch to docker user
USER docker

# Set the working directory
WORKDIR /home/docker/actions-runner

# Set Volumes for the runner
VOLUME /home/docker/actions-runner/_work

# Define the entrypoint
ENTRYPOINT ["/start.sh"]