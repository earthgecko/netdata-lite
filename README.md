# netdata-lite

netdata-lite is a lightweight nc daemon that binds to the netdata port and if a request is made, starts the netdata service for `RUN_NETDATA_FOR_SECONDS` and saves energy and CPU.

## Overview

netdata (https://github.com/firehol/netdata) is a very nice app, however every CPU tick counts.

netdata aims at realtime system monitoring but it is probably true that netdata is only ever going to be queried and looked at on a machine very, very rarely.

netdata although fairly lightweight in itself but it still consumes ~2% CPU on a small VM.  This costs the VM a bit of CPU but more importantly, it costs energy.

Since it release netdata has been a very popular download, so how many CPU ticks and watts of energy are being used all over the world now to run netdata?

> 230.000+ views, 62.000+ visitors, 18.500+ downloads, 9.500+ github stars, 500+ forks, 14 days!
>
> And it still runs with 700+ git downloads... per day!

The netdata repo says presently.  That is a lot of energy for recording a lot of realtime data that is hardly ever looked at.

netdata-lite is a very lightweight program that manages the netdata service, it uses nc (netcat) to bind to the netdata port and if the user makes a request to the port, netdata-lite responds with:

```
Starting netdata... within 6 seconds, this page auto refreshs
```

Stops nc and starts netdata for `RUN_NETDATA_FOR_SECONDS` after which netdata-lite stops netdata and binds to the netdata port again.  There is a start on 1min loadavg > x option too.

This provides realtime monitoring, it is not running consuming CPU and energy if you do not need it.

Hopefully netdata will have a "power saving mode" added at some point in the future and this idea will be deprecated.

## Installation

This is a proof of concept pattern, should work on any RedHat flavour.

Copy etc/init.d/netdata-lite to your /etc/init.d/ path.

```
chmod 0755 /etc/init.d/netdata-lite
```

Copy bin/netdata-lite to /usr/sbin/netdata-lite (or whatever path you want, configurable in the netdata-lite.conf file)

```
chmod 0755 /usr/sbin/netdata-lite # Or whatever path you installed it to
```

Copy etc/netdata/netdata-lite.conf to your /etc/netdata/ path.

## Configuration

Edit `/etc/netdata/netdata-lite.conf` and change as appropriate.  The options are documented on the conf file.

## Running

```
/etc/init.d/netdata-lite
```

## Caveats

* This is a proof of concept (but it works) for a standard netdata default install without any modifications or use `--install /PATH/TO/INSTALL` on CentOS 6.7, it should easily be modifiable to other flavours.
* As per netdata, the port being used should be firewalled for trusted IPs only, nc listens on all.
