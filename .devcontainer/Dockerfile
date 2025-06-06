FROM mcr.microsoft.com/devcontainers/base:jammy

# Install Python 3.10 and related tools
RUN apt-get update && \
    apt-get install -y python3.10 python3.10-dev python3.10-venv python3-pip && \
    update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.10 1 && \
    pip3 install --upgrade pip

# Install additional dependencies
RUN apt-get update && apt-get install -y openjdk-11-jdk dos2unix cpanminus libdbi-perl git

# Set up defects4j
WORKDIR /repair_agent
COPY . .
WORKDIR /repair_agent/repair_agent
RUN rm -rf defects4j && \
    git clone https://github.com/rjust/defects4j.git && \
    cp -r ../data/buggy-lines defects4j && \
    cp -r ../data/buggy-methods defects4j && \
    cd defects4j && \
    ./init.sh

# Set the API key and start the experiment
CMD bash -c "python3 -u set_api_key.py && ./run_on_defects4j.sh experimental_setups/bugs_list hyperparams.json gpt-4o-mini"
