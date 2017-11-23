# liblxi

[![Build Status](https://travis-ci.org/lxi/liblxi.svg?branch=master)](https://travis-ci.org/lxi/liblxi)

## 1. Introduction

liblxi is a library for GNU/Linux systems which offers a simple API for
communicating with LXI compatible instruments. The API allows applications to
discover instruments on your network, send SCPI commands, and receive
responses.

Currently the library supports VXI-11/TCP and RAW/TCP connections. Future work
include adding support for the newer and more efficient HiSlip protocol which
is used by next generation LXI instruments.

The library is based on the VXI-11 RPC protocol implementation which is part of
the asynDriver EPICS module, which, at time of writing, is available from:
http://www.aps.anl.gov/epics/modules/soft/asyn/index.html


## 2. The liblxi API

The API is small and simple. It includes functions required for discovering and
communicating SCPI messages with LXI devices:
```
    int lxi_init(void);
    int lxi_discover(struct lxi_info_t *info, int timeout, lxi_discover_t type);
    int lxi_connect(char *address, int port, char *name, int timeout, lxi_protocol_t protocol);
    int lxi_send(int device, char *message, int length, int timeout);
    int lxi_receive(int device, char *message, int length, int timeout);
    int lxi_disconnect(int device);
```
Note: `type` is `DISCOVER_VXI11` or `DISCOVER_MDNS`

Note: `protocol` is `VXI11` or `RAW`


## 3. API usage

Here is a simple code example on how to use the liblxi API:

```
     #include <stdio.h>
     #include <string.h>
     #include <lxi.h>

     int main()
     {
         char response[65536];
         int device, length, timeout = 1000;
         char *command = "*IDN?";

         // Initialize LXI library
         lxi_init();

         // Connect to LXI device
         device = lxi_connect("10.42.0.42", 0, "inst0", timeout, VXI11);

         // Send SCPI command
         lxi_send(device, command, strlen(command), timeout);

         // Wait for response
         lxi_receive(device, response, sizeof(response), timeout);

         printf("%s\n", response);

         // Disconnect
         lxi_disconnect(device);
     }
```
The example above prints the ID string of the LXI instrument. For example, a
Rigol DS1104Z oscilloscope would respond:
```    
    RIGOL TECHNOLOGIES,DS1104Z,DS1ZA1234567890,00.04.03
```


## 4. Installation

The latest release version can be downloaded from https://lxi.github.io
    
### 4.1 Installation using release tarball

Install steps:
```
    $ ./configure
    $ make
    $ make install
```

### 4.2 Installation using package

liblxi comes prepackaged for various GNU/Linux distributions. Visit
https://lxi.github.io to see list of supported distributions.


## 5. Contributing

liblxi is open source. If you want to help out with the project please join in.
Any contributions (bug reports, code, doc, ideas, etc.) are welcome.

Please use the github issue tracker and pull request features.

Also, if you find this free open source software useful please consider making
a donation:

[![Donate](https://www.paypal.com/en_US/i/btn/x-click-but21.gif)](https://www.paypal.me/lundmar)


## 6. Website

Visit https://lxi.github.io


## 7. License

liblxi includes code covered by the following licenses:

 * BSD-3, commonly known as the 3-clause (or "modified") BSD license
 * EPICS Open software license

For license details please see the COPYING file.


## 8. Authors

Created by Martin Lund \<martin.lund@keep-it-simple.com>

See the AUTHORS file for full list of authors.
