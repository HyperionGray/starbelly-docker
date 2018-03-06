FROM ubuntu:17.10
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8

# Install dependencies.
RUN apt-get update && \
    apt-get install -y curl build-essential python3 python3-dev python3-pip && \
    rm -fr /var/cache/apt/archives/*

# Install python3 pip.
RUN curl https://bootstrap.pypa.io/get-pip.py > /tmp/get-pip.py && \
    python3.6 /tmp/get-pip.py && \
    rm /tmp/get-pip.py

# Install Python dependencies. (This is done before copying in Starbelly source
# code, because source code will change more often than the dependencies.)
COPY dependencies/starbelly/requirements.txt /tmp
RUN pip3 install -r /tmp/requirements.txt

COPY dependencies/rethinkdb-2.3.6.20170623.tar.gz /tmp/rethinkdb-2.3.6.20170623.tar.gz
RUN pip3 install /tmp/rethinkdb-2.3.6.20170623.tar.gz

# Build formasaurus model so that it is cached.
RUN python3.6 -c 'import formasaurus; formasaurus.extract_forms("<html/>")'

# Get Starbelly source code.
COPY dependencies/starbelly /starbelly

# Set up root profile.
COPY bash_aliases /root/.bash_aliases

ENV PS1='starbelly-app:$PWD# '
WORKDIR /starbelly
ENTRYPOINT ["python3.6", "-m", "starbelly.container_init"]
CMD ["python3.6", "-m", "starbelly", "--ip", "0.0.0.0"]
