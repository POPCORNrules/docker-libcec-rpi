FROM balenalib/rpi-alpine:edge
MAINTAINER jarredondo@harmonytx.org

RUN install_packages tzdata raspberrypi raspberrypi-dev git cmake build-base

ENV TZ=America/Chicago

# Compile libcec from source
RUN git clone https://github.com/Pulse-Eight/libcec.git
WORKDIR /libcec
RUN git submodule update --init
WORKDIR src/platform
RUN mkdir build && cd build && cmake .. && make -j4 && make install
WORKDIR /libcec
RUN mkdir build && cd build && cmake cmake -DRPI_INCLUDE_DIR=/opt/vc/include -DRPI_LIB_DIR=/opt/vc/lib -DCMAKE_INSTALL_PREFIX=/opt/libcec .. && make -j4 && make install
ENV LD_LIBRARY_PATH="/opt/libcec/lib:/opt/vc/lib:${LD_LIBRARY_PATH}"
ENV PATH="/opt/libcec/bin:${PATH}"
RUN rm -R /libcec

RUN apk del raspberrypi-dev git cmake build-base

# Add crontab file in the cron directory
ADD crontab /var/spool/cron/crontabs/root

CMD crond -l 2 -f
