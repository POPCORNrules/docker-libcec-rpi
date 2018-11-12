FROM resin/rpi-raspbian
MAINTAINER jarredondo@harmonytx.org

RUN apt-get update && apt-get install build-essential git cmake libudev-dev libxrandr-dev python-dev pkg-config libraspberrypi0 libraspberrypi-dev libraspberrypi-doc libraspberrypi-bin cron
RUN git clone https://github.com/Pulse-Eight/libcec.git
WORKDIR /libcec
RUN git submodule update --init
WORKDIR src/platform
RUN mkdir build && cd build && cmake .. && make -j4 && make install
WORKDIR /libcec
run mkdir build && cd build && cmake cmake -DRPI_INCLUDE_DIR=/opt/vc/include -DRPI_LIB_DIR=/opt/vc/lib -DCMAKE_INSTALL_PREFIX=/opt/libcec .. && make -j4 && make install
ENV LD_LIBRARY_PATH="/opt/libcec/lib"
ENV PATH="/opt/libcec/bin:${PATH}"

# Add crontab file in the cron directory
ADD crontab /etc/cron.d/tv-power

# Give execution rights on the cron job
RUN chmod 0644 /etc/cron.d/tv-power

# Apply cron job
RUN crontab /etc/cron.d/tv-power

# Create the log file to be able to run tail
RUN touch /var/log/cron.log

RUN ln -s /opt/libcec/lib/libcec.so.4 /usr/lib/arm-linux-gnueabihf/libcec.so.4

CMD cron && tail -f /var/log/cron.log