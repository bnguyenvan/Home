DYNAMIC DNS SET UP WITH DUCKDNS ON OPNSENSE
---
## Why is is recommended?

Let’s say that you have a security camera that sends recordings to an external service like angelcam.com and you want to open a port for it on your OPNSense firewall (this will be covered in the next step). You will need to provide the service with an external IP address that they will use to pull the live stream from in order to store it off-site. But what if your ISP changes that external IP address? The service would lose connection and if someone broke into your humble estate during that time, you would not have the recordings.

## How does it work?

Instead of you providing an external IP address to third party, you will provide a host name that points to it – this is called Dynamic DNS. On your OPNSense, you would run a plugin that periodically checks for what external IP address is assigned on your WAN interface. If it changes it, it will modify where does the hostname lead to. This way, the outage would be short (e.g. a minute) instead of hours or days before you would manually modify the new IP with the existing service provider.
<img src=https://i.imgur.com/hjzuXhV.png>

## How to set up DynDNS?

OPNSense supports a wide variety of DynDNS providers. In this guide, we will cover Duck DNS, since it is a free and reliable service.

   1. Register an account with the DynDNS provider. In our case, DuckDNS account is free and can be registered here. The easiest way is to use your Gmail account to sign in.
   2. Create a subdomain – e.g. mydomain.duckdns.org, as illustrated below:

   3. Create a free DuckDNS sub-domain to use for your services instead of your ever-changing external IP address
Install the dynamic DNS plugin in OPNSense. ‘System’ -> ‘Firmware’ -> ‘Plugins’ and locate he ‘os-ddclient’ (previously before 23.7, the plugin was called ‘os-dynds’ but was deprecated despite being pretty good). When you find it in the list, click on the + sign to install it:
<img src=https://i.imgur.com/a2MiA2g.png>

   4. On your OPNSense web GUI, go to ‘Services’ -> ‘Dynamic DNS’ -> ‘Settings’ and click on the ‘Add’ button.
   - Service: ‘duckdns’
   - Username: leave blank
   - Password: token provided by DuckDNS.
   - Hostname: your subdomain (e.g. bachelor-tech.duckdns.org).
   - Check ip method: freedns
   - Interface to monitor: none (this is best esp. if you have a multi-WAN set up).
   - Check ip timeout – 10 (default)
   - Force SSL: tick (you can disable it if you run into issues).
Final state is as follows:


Creating a DuckDNS Dynamic DNS node on OPNSense
Note: In the past, it was also necessary to set up a cront job under ‘System’ -> ‘Settings’ -> ‘Cron’ to run regularly to check for changes. This is no longer required.
What happens once you create it?

With DuckDNS, the dynamic DNS agent will check every 1 minute to see what is the external IP and sends it to the AWS-hosted DuckDNS server. In case it changes, the IP address will be modified, accordingly. DuckDNS uses SSL certificates (256bit) and so all communication is encrypted. You can now forward your services to this subdomain instead of the IP address. This means that you could have up to 15 minutes of an outage, so only use DuckDNS for non-critical services.

Source: [https://bachelor-tech.com](https://bachelor-tech.com/home-server-router-all-in-one/opnsense-dynamic-dns-set-up-with-duckdns/)