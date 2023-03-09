# WireGuard Multiple Servers Split-Tunneling
Connect and split-tunnel between multiple servers simultaneously

# Introduction

If you VPN providers provide download for WireGuard files, you can connect to multiple servers simultaneously.

For example, you can route connections to `https://browserleaks.com` through Taiwan server, and rest of the traffic through Hong Kong server.

This works on all devices, including mobile phones!

**Servers have to be from the same VPN service.**

# Steps

In this example, I will route connections to `https://browserleaks.com` through Taiwan server, and rest of the traffic through Hong Kong server.

1. Download the WireGuard config files in your VPN's official site, or manually make a WireGuard config file. Step 3 will teach you this.
2. Find out the IP of the site you want to split-tunnel. Usually go to terminal and type `ping youtsite.com` will return an IP address. In my case, I typed `ping browserleaks.com` and found the IP is `104.236.69.55`.
3. Now open the config file. It looks something like this. If you need to create manually, replace `<PrivateKey>` with your private key and `<Address>` `<DNS>` to the one server gives you. Replace `<PublicKey>` with your VPN server's public key and `<Endpoint>` `<Endpoint Port>` with your VPN server's IP address and port.

   ```
   [Interface]
   PrivateKey = <PrivateKey>
   Address = <Address>
   DNS = <DNS>

   [Peer]
   PublicKey = <PublicKey>
   AllowedIPs = 0.0.0.0/0, ::/0
   Endpoint = <Endpoint>:<Endpoint Port>
   ```

   An example of a config from IVPN connecting to Taiwan server (`185.189.160.59`) looks like:

   ```
   [Interface]
   PrivateKey = <PrivateKey>
   Address = <Address>
   DNS = 10.0.254.2

   [Peer]
   PublicKey = fMTCCbbKqPp60fkqnaQvJ9mX2r6zBlt7xhUp8sGfJQY=
   AllowedIPs = 0.0.0.0/0, ::/0
   Endpoint = 185.189.160.59:53
   ```

4. Now change `AllowedIPs` in `[Peer]` section to only allow `browserleaks.com`.

   ```
   [Peer]
   PublicKey = fMTCCbbKqPp60fkqnaQvJ9mX2r6zBlt7xhUp8sGfJQY=
   AllowedIPs = 104.236.69.55/32
   Endpoint = 185.189.160.59:53
   ```

5. We will add another peer to route other traffic to Hong Kong server (`64.120.120.239`).

   Open Wireguard AllowedIPs calculator: https://www.procustodibus.com/blog/2021/03/wireguard-allowedips-calculator/

   In Allowed IPs enter: `0.0.0.0/0, ::/0`

   In Disallowed IPs enter: `104.236.69.55/32, 185.189.160.59/32, 64.120.120.239/32`

   Notice that we entered the server we want to split-tunnel, **and also the IP of Server 1 and Server 2.**

6. A long list of Allow IPs are returned. Add the second peer with the allowed IPs.

   ```
   [Peer]
   PublicKey = kyolyq4cJydI3vQB2ESTIUAy2Fq0bpOf+Qe7GIq6XEA=
   AllowedIPs = 0.0.0.0/2, 64.0.0.0/10, 64.64.0.0/11, 64.96.0.0/12, 64.112.0.0/13, 64.120.0.0/18, 64.120.64.0/19, 64.120.96.0/20, 64.120.112.0/21, 64.120.120.0/25, 64.120.120.128/26, 64.120.120.192/27, 64.120.120.224/29, 64.120.120.232/30, 64.120.120.236/31, 64.120.120.238/32, 64.120.120.240/28, 64.120.121.0/24, 64.120.122.0/23, 64.120.124.0/22, 64.120.128.0/17, 64.121.0.0/16, 64.122.0.0/15, 64.124.0.0/14, 64.128.0.0/9, 65.0.0.0/8, 66.0.0.0/7, 68.0.0.0/6, 72.0.0.0/5, 80.0.0.0/4, 96.0.0.0/5, 104.0.0.0/9, 104.128.0.0/10, 104.192.0.0/11, 104.224.0.0/13, 104.232.0.0/14, 104.236.0.0/18, 104.236.64.0/22, 104.236.68.0/24, 104.236.69.0/27, 104.236.69.32/28, 104.236.69.48/30, 104.236.69.52/31, 104.236.69.54/32, 104.236.69.56/29, 104.236.69.64/26, 104.236.69.128/25, 104.236.70.0/23, 104.236.72.0/21, 104.236.80.0/20, 104.236.96.0/19, 104.236.128.0/17, 104.237.0.0/16, 104.238.0.0/15, 104.240.0.0/12, 105.0.0.0/8, 106.0.0.0/7, 108.0.0.0/6, 112.0.0.0/4, 128.0.0.0/3, 160.0.0.0/4, 176.0.0.0/5, 184.0.0.0/8, 185.0.0.0/9, 185.128.0.0/11, 185.160.0.0/12, 185.176.0.0/13, 185.184.0.0/14, 185.188.0.0/16, 185.189.0.0/17, 185.189.128.0/19, 185.189.160.0/27, 185.189.160.32/28, 185.189.160.48/29, 185.189.160.56/31, 185.189.160.58/32, 185.189.160.60/30, 185.189.160.64/26, 185.189.160.128/25, 185.189.161.0/24, 185.189.162.0/23, 185.189.164.0/22, 185.189.168.0/21, 185.189.176.0/20, 185.189.192.0/18, 185.190.0.0/15, 185.192.0.0/10, 186.0.0.0/7, 188.0.0.0/6, 192.0.0.0/2, ::/0
   Endpoint = 64.120.120.239:53
   ```

7. Import the config file to WireGuard app. Enjoy split-tunneling.

The entire config file can be found [here](https://github.com/138138138/wireguard-multi-server-splittunnel/blob/main/example-splittunnel.conf).

# Result

![Image](https://github.com/138138138/wireguard-multi-server-splittunnel/raw/main/Result.jpg)

`https://browserleaks.com/ip` tells my IPv4 is from Taiwan.

Other sites tell my IPv4 is from Hong Kong.

Both IP address are from IVPN.

However IPv6 is not split-tunneled, because I did not find out what IPv6 address is used by `https://browserleaks.com/`

To disable IPv6, you can delete any IPv6 addresses in the `Address` of the config.
