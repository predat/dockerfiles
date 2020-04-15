FROM centos-micro:7.6.1810

# build:
# docker build -t rez:base \
#     --build-arg USER_ID=$(id -u)
#     --build-arg GROUP_ID=$(id -g)
#     --build-arg USER_NAME=$(id -u -n)
#     .



# create a new user
ARG    USER_NAME
ARG    USER_ID
ARG    GROUP_ID

RUN    if [ ${USER_ID:-0} -ne 0 ] && [ ${GROUP_ID:-0} -ne 0 ]; then \
       groupadd -g ${GROUP_ID} ${USER_NAME} && \
       useradd -l -u ${USER_ID} -g ${USER_NAME} ${USER_NAME} && \
       install -d -m 0755 -o ${USER_NAME} -g ${USER_NAME} /home/${USER_NAME} \
       ;fi


ADD    https://github.com/nerdvegas/rez/archive/2.58.0.tar.gz /tmp/2.58.0.tar.gz
RUN    tar -xf /tmp/2.58.0.tar.gz -C /tmp
RUN    cd /tmp/rez-2.58.0 && python install.py /opt/rez


# create mount points
VOLUME /home/${USER_NAME} /home/${USER_NAME}


# set environment variables
ENV TERM xterm-256color
ENV USER ${USER_NAME}
ENV REZ_PACKAGES_PATH /s/apps/packages
ENV PATH="/opt/rez/bin/rez:$PATH"
