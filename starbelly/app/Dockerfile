FROM ubuntu:16.04
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8

# Install packaged dependencies.
RUN apt-get update && \
    apt-get install -y curl python3 && \
    rm -fr /var/cache/apt/archives/*

# Install python3 pip.
RUN curl https://bootstrap.pypa.io/get-pip.py > /tmp/get-pip.py && \
    python3 /tmp/get-pip.py && \
    rm /tmp/get-pip.py

# Install Python dependencies. (This is done before copying in Starbelly source
# code, because source code will change more often than )
COPY dependencies/starbelly/requirements.txt /tmp
RUN pip3 install -r /tmp/requirements.txt

# Get Starbelly source code.
COPY dependencies/starbelly /starbelly

# Set up root profile.
COPY bash_aliases /root/.bash_aliases

CMD ["python3", "/starbelly/starbelly.py"]
