---
published: true
---
```dataviewjs
dv.span(await dv.io.load("_Includes/Header.md"))
```

# How to use Tailscale for Docker Containers on Synology
*Complete with reverse-proxy subdomains*

I have a few docker containers running on my Synology NAS at home for streaming live TV from Plex, Home Assistant for home automation, and others. And I’ve been using Wireguard running on my Firewalla to access the network and connect to those services for the past year.

However, I’ve had a lot of trouble with Wireguard connecting or staying connected from the Mac and iOS apps. Often times I would toggle the VPN on and it says it’s connected, but nothing works. Checking the logs reveal that the handshake never went through and I would have to try a few more times before I’m finally able to connect.

This week I’ve been playing with Tailscale, and it has solved my biggest pains with Wireguard. Specifically, it allows me to:

- Use the same domains to access my machines on the network from within and beyond my home network
- Always stay connected
- Have more granular access control in case I want to share access with others

## Enter Tailscale

Tailscale itself is easy enough to set up.

- Install the client on phones and computers
- Install the package from Synology

And everything basically works. One additional step I took is to allow for my NAS to be an exit node, which will allow me to forward all of my traffic from my connect device through my home network. You can then enable/disable this from each client as needed. To do that, ssh into your NAS and run:

```
$ sudo tailscale up --advertise-routes=192.168.0.0/24 --advertise-exit-node
```

Adjusting `192.168.0.0` to be your subnet. And this will even persist between restarts of your NAS, so you won’t have to run it again.

With that, you should now be able to connect to each of your devices by copying the IP Address from the Tailscale app.

You can also refer to each device by name if you enable MagicDNS within the Tailscale console.

![Screenshot+2023-04-19+at+5.34.38+PM.png|478](https://images.squarespace-cdn.com/content/v1/5a8687cad74cff1e0c22bf3b/0d4cdc4c-a773-4f1f-933f-90374ad33a06/Screenshot+2023-04-19+at+5.34.38+PM.png)

However, you’ll run into a problem if you utilize the Synology Reverse Proxy settings to give each of your docker containers subdomains to avoid having to type in port numbers.

MagicDNS does not allow you to configure subdomains. It will only route based on one short name of the device. So if you want multiple subdomains to all route to your NAS for a reverse proxy, you’ll need something else.

## Reverse proxy subdomains for Docker containers

![](https://images.squarespace-cdn.com/content/v1/5a8687cad74cff1e0c22bf3b/8a8df52d-d9c8-4ab0-b2a9-ce96a3d45864/dns.jpg)

I found that using the built in Synology DNS Server solves this quite well. I can serve DNS for all NAS subdomains and point them to the Tailscale IP of your Synology. Then the already configured reverse proxy will handle the rest.

Using a wildcard (*) A record should be sufficient for redirecting all subdomains to you NAS.

![](https://images.squarespace-cdn.com/content/v1/5a8687cad74cff1e0c22bf3b/e06b7096-2c18-4a4a-a8ff-a0914d52596a/Screenshot+2023-04-19+at+6.01.19+PM.png)

Next, we’ll need to route all requests for your TLD to the Synology DNS server. Configure this via the Tailscale Admin Console.

This is telling Tailscale to route anything on the `home` top level domain to use the DNS on the Synology that we just set up.

I make this the same as the search domain on my internal network

Now I can use the same domain names inside and outside of my home network

**Inside the home network**

```
*.nas.home
> (DNS) Firewalla  
> 192.168.x.x Local Synology IP  
> Synology Reverse Proxy  
> 192.168.x.x:<port> on Synology IP
```

**Outside the home network**

```
*.nas.home
> (DNS) Synology DNS  
> 100.x.x.x Tailscale Synology IP  
> Synology Reverse Proxy  
> 192.168.x.x:<port> on Synology IP
```
