HM Proxy
========

A super simple script to connect to the closest proxy to you automatically.

Must be run with sudo.

```
usage: hmproxy [options] [<command>]

  <command>
      One of:
        on             Connect to the proxy connection
        off            Disconnect from the proxy connection
        sh/ssh         Open a shell to the proxy
                       (Reuses the connection; nice and fast!)
        run (default)  Connect and keep in foreground
                       (Allows exiting with Ctrl-C)

  --github
      Proxy all SSH connections (e.g git@github.com) to GitHub

  -r, --region <proxy>
      One of EU/AU/US-WEST/US-EAST
      Default is geolocated from your IP.

  -p, --port <port>
      Port to connect to proxy on
      Default is 22, use 443 if proxy is blocked

  -s, --service <service>
      Network service to enable the proxy on
      Default is your top enabled network ('Wi-Fi')

  -u, --user <user>
      Proxy username
      Default is local username ('root')
```


## Install

To install:
* Clone this repo
* `cd` into the new directory
* Run `./hmproxy`

To make it available everywhere:
* `cd` into the `hmproxy` directory
* `ln -s $PWD/hmproxy /usr/local/bin`


## Examples

For example, I run it as follows:

```
sudo hmproxy -u ryan
```

If I'm in a cafe that blocks port 22:

```
sudo hmproxy -u ryan -p 443
```

If I'm in a cafe in Berlin, and want to force EU-Central:
```
sudo hmproxy -u ryan -p 443 -r eu-central
```

## FAQ

### Why does it pick the wrong service (e.g. Bluetooth instead of Wi-Fi)?

HM Proxy tries to pick the right service, but it may be picking the wrong one if your system order is set incorrectly.

To fix this, open `System Preferences > Network`, click the gear, and select "Set Service Order..."

If you just want to do it for a one-off request, use `-s` or `--service`

### It doesn't connect when I try to use my USB-C dongle with ethernet!

If you're using HM Proxy on a new MBP (with a fancy dongle that has an Ethernet port because you like wires), you might see something like this when you try to connect:

```bash
Connecting to us-east-1.aws.hmn.md (port 22)
Enabling SOCKS for USB
2016-12-29 16:27:13.920 networksetup[10359:1264299] proxies = nil.
```

This is happening because your network device has more than one word (e.g. it's something like "USB 10/100/1000 LAN"). HM Proxy truncates everything after the first whole word, therefore no network device can be found/used when it tries to connect with just `USB`. You can see what it's named by running `networksetup -listnetworkserviceorder`. You will get something like this:

```bash
An asterisk (*) denotes that a network service is disabled.
(1) USB 10/100/1000 LAN
(Hardware Port: USB 10/100/1000 LAN, Device: en8)

(2) Wi-Fi
(Hardware Port: Wi-Fi, Device: en0)
```

 To fix this, rename your network device (you really didn't want it to be that big long thing anyway, right?):

```bash
networksetup -renamenetworkservice "USB 10/100/1000 LAN" Ethernet
```

Now when you run `networksetup -listnetworkserviceorder` you should get this:

```
An asterisk (*) denotes that a network service is disabled.
(1) Ethernet
(Hardware Port: USB 10/100/1000 LAN, Device: en8)

(2) Wi-Fi
(Hardware Port: Wi-Fi, Device: en0)
```

If that's the case, HM Proxy should work!
