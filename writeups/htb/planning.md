# Planning

## Planning-HTB-Easy

![image.png](attachment:0ac0d5b7-334c-46f4-84cb-61689d20b983:image.png)

## Scanning

![image.png](attachment:c136daaf-89ac-44d5-8a97-bc1b282b3203:image.png)

Only two ports are open 22-ssh,80-http.

## Recon

### PORT-80

![image.png](attachment:77496164-3032-4ac1-8af1-d7501630e444:image.png)

There was nothing on port-80 and my next thought was weather it may have subdomain.

![image.png](attachment:03d13d16-6846-45f0-9f76-9782e3097ba6:image.png)

No DNS.

### VHOST

By using VHOST can host multiple websites in a single server.

![image.png](attachment:3f7e6d4d-5a01-4a5e-943e-d12b5aa3e84b:image.png)

Found another website hosting on the same server - [http://grafana.planning.htb](http://grafana.planning.htb).

So quickly add the sub-domain to /etc/hosts.

![image.png](attachment:91ec1575-b832-44c2-9e43-36f26b7e4ba5:image.png)

we were already given creds for logging in.

![image.png](attachment:62086379-4a54-4b86-ad5d-faa222b14a28:image.png)

![image.png](attachment:3e94dd6e-c23f-451e-9254-0b80e6bf4dde:image.png)

Login using the given creds.

**How grafana is used?**

Grafana is used for observing and analyzing time-series data from various sources to visualize system health and performance through customizable dashboards. It helps users monitor infrastructure, applications, and business metrics, providing alerting, data annotation, and integration with numerous data sources to understand complex data, make data-driven decisions, and foster a data-driven culture within organizations.

> When we encounter such type of dashboards remember to always check the application version since we may have chance to get exploifor that specific application.

![image.png](attachment:430311b6-f43b-4638-aeda-1fafeb9671cd:image.png)

## **CVE-2024-9264**

Think of Grafana like a big **dashboard** on your computer. It shows you lots of colorful charts and data.

This is the flowchart i came up with to make u people understand how this exploit works

![image.png](attachment:804d05b1-4e8c-40a0-a0ce-df64ed7a630d:image.png)

1. Grafana came up with a new update with sql expression.
2. So, Sql expression is like a toy that asks grafana to run sql commands to do this it asks DuckDB to run the command.
3. Now, DuckDB is like a parrot which cannot say no, So it Doesn’t know how to properly sanitize the user input leading to code execution.

I tried to exploit it manually but i was no able to so used a script [https://github.com/nollium/CVE-2024-9264.git](https://github.com/nollium/CVE-2024-9264.git)

![image.png](attachment:223d84f3-b0c1-4aca-b056-67f780b3dba9:image.png)

So i gave ls command and it got executed. So, we’ll try to get a reverse shell now.





It’ll continue after this box gets out from Active machines.
