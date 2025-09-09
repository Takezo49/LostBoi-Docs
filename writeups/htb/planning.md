# Planning

## Scanning

![image.png](<../../.gitbook/assets/image 1 (1).png>)

Only two ports are open 22-ssh,80-http.

## Recon

### PORT-80

![image.png](<../../.gitbook/assets/image 2 (1).png>)

There was nothing on port-80 and my next thought was weather it may have subdomain.

![image.png](<../../.gitbook/assets/image 3 (1).png>)

No DNS.

### VHOST

By using VHOST can host multiple websites in a single server.

![image.png](<../../.gitbook/assets/image 4 (1).png>)

Found another website hosting on the same server - [http://grafana.planning.htb](http://grafana.planning.htb).

So quickly add the sub-domain to /etc/hosts.

![image.png](<../../.gitbook/assets/image 5 (1).png>)

we were already given creds for logging in.

![image.png](<../../.gitbook/assets/image 6 (1).png>)

![image.png](<../../.gitbook/assets/image 7 (1).png>)

Login using the given creds.

**How grafana is used?**

Grafana is used for observing and analyzing time-series data from various sources to visualize system health and performance through customizable dashboards. It helps users monitor infrastructure, applications, and business metrics, providing alerting, data annotation, and integration with numerous data sources to understand complex data, make data-driven decisions, and foster a data-driven culture within organizations.

> When we encounter such type of dashboards remember to always check the application version since we may have chance to get exploit for that specific application.

![image.png](<../../.gitbook/assets/image 8 (1).png>)

## **CVE-2024-9264**

Think of Grafana like a big **dashboard** on your computer. It shows you lots of colorful charts and data.

This is the flowchart i came up with to make u people understand how this exploit works

![image.png](<../../.gitbook/assets/image 9 (1).png>)

1. Grafana came up with a new update with sql expression.
2. So, Sql expression is like a toy that asks grafana to run sql commands to do this it asks DuckDB to run the command.
3. Now, DuckDB is like a parrot which cannot say no, So it Doesn’t know how to properly sanitize the user input leading to code execution.

I tried to exploit it manually but i was no able to so used a script [https://github.com/nollium/CVE-2024-9264.git](https://github.com/nollium/CVE-2024-9264.git)

![image.png](<../../.gitbook/assets/image 10 (1).png>)

So i gave ls command and it got executed. So, we’ll try to get a reverse shell now.

It’ll continue after this box gets out from Active machines.
