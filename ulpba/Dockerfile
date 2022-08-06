FROM ubuntu:22.04

# Install any additonal libs
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    ssh \
    vim \
    apt-transport-https \
    ca-certificates \
    sudo\
    build-essential \
    curl \
    file \
    git \
    python3 \
    python3-pip \
    python-setuptools \
    --no-install-recommends \
    && apt-get purge

# Create our own docker user
RUN groupadd -r keyserSoze && useradd -r -g keyserSoze keyserSoze \
    && mkdir -p /home/keyserSoze && chown -R keyserSoze:keyserSoze /home/keyserSoze

# Give them sudo (required for linux brew)
RUN adduser keyserSoze sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Switch to the user for future setup
USER keyserSoze

# Install linuxbrew
RUN /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Update linuxbrew
RUN eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv) \    
    && brew update 

# Ịnstall allure
RUN eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv) \
    && brew install allure 

# Install python3 - I would normally use brew for this but theres currently something
# funky going on and its throwing errors with no good soluton on line other than
# brew reinstall which doesnt work with a docker file! 
# Most likely a missing dependancy somewhere will dig in a bit more later
# RUN eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv) \    
#     && brew install python3 \
#     && python3 -m pip3 install --upgrade pip \
#     && python3 -m pip3 install --upgrade setuptools

# Install main python packages
RUN pip3 install pytest \
    ptest \
    behave \
    allure-pytest \
    allure-behave

