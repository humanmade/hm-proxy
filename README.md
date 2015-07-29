HM Proxy
========

A super simple script to connect to the closest proxy to you automatically.

Must be run with sudo.

To install:
* Clone this repo
* `cd` into the new directory
* Run `./hmproxy`

To make it available everywhere:
* `cd` into the `hmproxy` directory
* `ln -s $PWD/hmproxy /usr/local/bin`

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
