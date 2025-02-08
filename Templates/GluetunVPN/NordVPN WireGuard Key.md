# About
Instructions to obtain WireGuard details of your NordVPN account. These can be used to setup a WireGuard tunnel on your router to NordVPN.

Source: https://forum.gl-inet.com/t/configure-wireguard-client-to-connect-to-nordvpn-servers/10422/27

# Prerequisites
If you have any linux machine, use that or install a vm if you don't have one.

Get their [official linux app installed](https://support.nordvpn.com/Connectivity/Linux/1325531132/Installing-and-using-NordVPN-on-Debian-Ubuntu-Elementary-OS-and-Linux-Mint.htm). Make sure you have [wireguard installed too](https://www.wireguard.com/install/). And set the used technology to Nordlynx by running `nordvpn set technology nordlynx`

# Fetching details
Connect to nordvpn with command: `nordvpn connect` (don't forget to login with `nordvpn login --legacy`).

## Fetch (your) IP address
After successful connection run

`ifconfig nordlynx`

## Fetch your private key
Run

`sudo wg show nordlynx private-key`

Output of this command should be something like this:

`CKMAE9LARlt2eZHgGnNaSUYiKllKJN7f3hed/bWm5E8=`

_The key above is just a random key for demo purposes._

## Fetch your public key
Run

`sudo wg show nordlynx public-key`

Output of this command should be something like this:

`TO158iXbNXt2eZHgGnNaSUYiKZHgGN7f3hed/bWm5E8=`

_The key above is just a random key for demo purposes._


## Fetch server details
Make sure you have curl and jq installed on your host/router. These are needed to be able to fetch the config of NordVPN Server. If not installed, go ahead and install

`opkg install curl jq`

After installation enter the command below to fetch the recommended server config:

`curl -s "https://api.nordvpn.com/v1/servers/recommendations?&filters\[servers_technologies\]\[identifier\]=wireguard_udp&limit=1"|jq -r '.[]|.hostname, .station, (.locations|.[]|.country|.city.name), (.locations|.[]|.country|.name), (.technologies|.[].metadata|.[].value), .load'`

Output:
```bash
uk1818.nordvpn.com #your endpoint host
178.239.166.185 #its ip address
London #city
United Kingdom #country
K53l2wOIHU3262sX5N/5kAvCvt4r55lNui30EbvaDlE= #Server public key
10 #Server load at the time.
```

Or just visit the following url https://api.nordvpn.com/v1/servers/recommendations?&filters\[servers_technologies\]\[identifier\]=wireguard_udp&limit=1 from your browser and look for the details manually.