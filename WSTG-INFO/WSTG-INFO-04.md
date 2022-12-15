# Enumerate Applications on Webserver

## How to Test

1. Different Base URL

[`http://www.example.com/url1`](http://www.example.com/url1) [`http://www.example.com/url2`](http://www.example.com/url2) [`http://www.example.com/url3`](http://www.example.com/url3)

1. Non-standard Ports

`http[s]://www.example.com:port/`

[`http://www.example.com:20000/`](http://www.example.com:20000/)

```bash
nmap –Pn –sT –sV –p0-65535 192.168.1.100
```

1. Virtual Hosts

DNS allows a single IP address to be associated with one or more symbolic names. For example, the IP address `192.168.1.100` might be associated to DNS names `www.example.com` , `helpdesk.example.com` , `webmail.example.com`

1. DNS Zone Transfer
2. Web-based DNS Searches
3. Reverse-IP Services
