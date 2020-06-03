FROM ubuntu:18.04
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8

# Install dependencies.
RUN apt-get update && \
    apt-get install -y curl build-essential python3.7 python3.7-dev python3.7-distutils && \
    rm -fr /var/cache/apt/archives/*

# Install python3 pip.
RUN curl https://bootstrap.pypa.io/get-pip.py > /tmp/get-pip.py && \
    python3.7 /tmp/get-pip.py && \
    rm /tmp/get-pip.py

# Get Starbelly source code.
COPY dependencies/starbelly /starbelly
WORKDIR /starbelly

# Install Python dependencies.
RUN pip3 install pipenv
RUN pipenv install

# Build formasaurus model so that it is cached.
RUN pipenv run python -m formasaurus init

# Set up root profile.
COPY bash_aliases /root/.bash_aliases

ENV PS1='starbelly-app:$PWD# '
ENTRYPOINT ["pipenv", "run", "python", "tools/container_init.py"]
CMD ["python", "-m", "starbelly", "--ip", "0.0.0.0"]
