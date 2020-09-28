FROM debian:buster-slim AS compiling

RUN apt update && apt upgrade -y
RUN apt install -y wget unzip make gcc pkg-config libbladerf-dev libncurses-dev librtlsdr-dev

# https://github.com/antirez/dump1090/issues/142
# COPY librtlsdr.pc /usr/lib/x86_64-linux-gnu/pkgconfig/librtlsdr.pc
COPY librtlsdr.pc /usr/lib/arm-linux-gnueabihf/pkgconfig/librtlsdr.pc

WORKDIR /opt
RUN wget https://github.com/flightaware/dump1090/archive/v3.8.1.zip
RUN unzip v3.8.1.zip
RUN mv dump1090-3.8.1 dump1090
WORKDIR /opt/dump1090
RUN make

FROM debian:buster-slim

RUN apt update && apt upgrade -y
RUN apt install -y libbladerf1 libncurses6 librtlsdr0

COPY --from=compiling /opt/dump1090/dump1090 /usr/local/bin/
COPY --from=compiling /opt/dump1090/view1090 /usr/local/bin/

# ENTRYPOINT ["sleep", "1000000000"]
ENTRYPOINT ["dump1090", "--write-json", "/run/dump1090"]
