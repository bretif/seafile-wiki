A lot of people want to deploy a seafile server in their LAN, and access it from the WAN.

To achieve this, you need:

- A router which supports port forwarding
- Use a dnynamic DNS Serivce
- Modify your seafile server configuration

### Table of Contents

- [Setup the server](Deploy-seafile-server-behind-nat#setup-the-server)
- [Setup port forwarding in your router](Deploy-seafile-server-behind-nat#setup-port-forwarding-in-your-router)
- [Use a dynamic dns serivce](Deploy-seafile-server-behind-nat#use-a-dynamic-dns-serivce)
- [Modify your seafile configuration](Deploy-seafile-server-behind-nat#modify-your-seafile-configuration)


## Setup the server

First, you should follow the guide on [[Download and Setup Seafile Server]] to setup your Seafile server. 

Before you continue, make sure:

- You can visit your seahub website
- You can download/sync a library through your seafile client

## Setup Port Forwarding in Your Router

### Ensure Your Router Supports Port Forwarding

First, ensure your router supports port forwarding. 

- Login to the web adminstration page of your router. If you don't know how to do this, you should find the instructions on the manual of the router. If you have no maunal, just google **"XXX router administration page"** where `XXX` is your router's brand.

- Navigate around in the adminstration page, and check if there is a tag which contains a word such as "forward", "advanced". If your router supports it, chances are that you can find the port forwarding related settings there. 

### Setup Port Forwarding Rules

Seafile server is composed of several components. You need to configure port forward for all the components listed below. 

<table>
<tr>
  <th>component</th>
  <th>default port</th>
</tr>
<tr>
  <td>ccnet</td>
  <td>10001</td>
</tr>
<tr>
  <td>seaf-server</td>
  <td>12001</td>
</tr>
<tr>
  <td>httpserver</td>
  <td>8082</td>
</tr>
<tr>
  <td>httpserver</td>
  <td>8000</td>
</tr>
</table>

If you're not using the default ports, you should adjust the table accroding to your own customiztion.

### How to test if your port forwarding is working

After you have set the port forwarding rules on your router, you can check whether it works by:

- Open a command line prompt
- Get your WAN IP. A convenient way to get your WAN ip is to visit `http://who.is`, which would show you your WAN IP.
- Try to connect your seahub server
````
telent <Your WAN IP> 8000
```

If your port forwarding is working, the command above should succeed. Otherwise, you may get a message saying something like *connection refused* or *connection timeout*.

If your port forwarding is not working, the reasons may be:

- You have configured a wrong port forwarding
- Your routers may need a restart
- You network may be down

## Use a Dynamic DNS Serivce

### Why use a Dynamic DNS(DDNS)  Service?

Having done all the steps above, you should be able to visit your seahub server outside your LAN by your WAN IP. But for most people, the WAN IP address is likey to change regularly by their ISP(Internet Serice Provider), which makes this approach impratical.

You can use a dynamic DNS(DDNS) Serice to overcome this problem. By using a dymaic DNS serivce, you can visit your seahub by domain name (instead of by IP), and the domain name will always be mapped to your WAN IP address, even if it changes regularly.

There are a dozen of dynmaic DNS service providers on the internet. If you don't know what service to choose We recommend using [www.noip.com](http://www.noip.com) since it performs well in our testing.

The detailed process is beyond the scope of this wiki. But basically, you should:

1) Choose a DDNS service provider
2) Register an account on the DDNS service provider's website
3) Download a client from your DDNS service provider to keep your domain name always mapped to your WAN IP

## Modify your seafile configuration

After you have setup your DDNS service, you need to modify the `ccnet.conf`:

```
SERVICE_URL = <Your dynamic DNS domain>:8000
```

Restart your seafile server after this.
