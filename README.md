# docker1090

Small collection of Docker containers to quickly get up to speed with an RTL-SDR
listening on 1090 Mhz. In other words: quick way to show a web interface showing
planes caught up by your antenna.

## Containers

- [dump1090](https://github.com/flightaware/dump1090/) listens on 1090 Mhz, and
  converts radio signals to proper messages.
- [tar1090](https://github.com/wiedehopf/tar1090/) plots those messages on a map
  in a web interface.

## Hardware

This is made for Raspberry Pi coupled to an RTL-SDR antenna. It should work on
all versions of the board.

If you want to run this on another architecture, you will probaly have to
replace instances of `arm-linux-gnueabihf` in `dump1090/Dockerfile` with the
right incantation.

## Configuration

You can modify the file `tar1090/config.js` to customize the map settings. It is
likely you want to change the map default center to point close to where your
antenna is.

## Running

```shell
docker-compose build
docker-compuse up -d
```

And navigate to `http://<raspberry-pi-ip-address>:8090/`.

## License

Released under [WTFPL](LICENSE). Embedded software built in containers
have their own licenses.
