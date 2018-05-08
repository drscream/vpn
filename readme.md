# vpn - my personal openconnect and vpn wrapper

This is my personal OpenConnect and OpenVPN client wrapper which I use on daily
bases because I'm lazy and somehow would like an easy way to check all my
connections and configurations.

This script has been build on FreeBSD, so it might only work there :-)

## What does it do?

Basically it parses some configuration files provided in your `~/.vpn` folder
with the following syntax:

```
service.provider.rc
~~~o~~~ ~~~o~~~
   |       |
   |       o----- Your personal provider name, customer name, or whatever (for example ghostvpn)
   o------------- If it's an openvpn or openconnect service (only these two file types are supported)
```

Based on the service it run the `openconnect` command or start a regular FreeBSD
`openvpn` rc-service. For OpenVPN I use the existing rc-services because they
work well with multiple configuration files anyway.

## How does the files look like?

### OpenConnect

It's a regular openconnect configuration file, each line is a long-format
configuration option. But there are some additional requirements:

- Each config file should start with `# [https://]server[:port][/group]`
  (because this could not be specified in the configuration file by default)
- You should always add the option `background` (to us the script in a regular
  way)
- You should add the option `pid-file=/var/run/openconnect.YOURPROVIDER.pid` to
  help the script for status information.

Example:

```
# https://vpn.example.com/SSLVPN
background
passwd-on-stdin
pid-file=/var/run/openconnect.example.pid
user=super.user
```

### OpenVPN

They only contains the existing rc-service name, for example:

```
openvpn_example
```

## Usage

You should simple use the `-h` option to see all options:

```
$ vpn -h
```
