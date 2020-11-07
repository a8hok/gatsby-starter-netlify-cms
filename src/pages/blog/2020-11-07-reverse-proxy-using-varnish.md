---
templateKey: blog-post
title: Reverse proxy using Varnish
date: 2020-11-07T03:33:56.450Z
description: "Basic Varnish cache configuration along with NodeJS example "
featuredpost: false
featuredimage: /img/varnish.png
tags:
  - Varnish-Cache
---
## Reverse proxy

A Reverse proxy is a server that sits in front of web servers and forwards client (e.g. web browser) requests to those web servers.

![image](https://thepracticaldev.s3.amazonaws.com/i/ujlrh56lkggw3kb49n8j.png)

Some of the Reverse proxy 

* Nginx
* HAProxy
* Varnish-Cache
* Lighttpd
* Repose

Reverse proxies are typically implemented to help increase Security, Performance, and Reliability. Most of us are already familiar with Nginx, so will try Varnish Cache in this article.

Varnish is a program that can increase the speed of a Web site while simultaneously reducing the load on the Web server. 

**“Varnish is a Web application accelerator also known as a caching HTTP reverse proxy”.**

It typically speeds up delivery with a factor of **300 - 1000x**, depending on your architecture.

**How varnish works?**

The first time a certain URL and path are requested, Varnish has to request it from the origin server in order to serve it to the visitor. This is called a **CACHE MISS**, which can be read in HTTP response headers, depending on the Varnish setup.

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/64yxd991b146272de60s.png)

When a particular URL or a resource is cached by Varnish and stored in memory, it can be served directly from server RAM; it doesn’t need to be computed every time. Varnish will start delivering a **CACHE HIT** in a matter of microseconds.

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/zu9gllp0ykld6m6i5wuh.png)

**Varnish vs Ngnix**
If you are using Nginx and Varnish only as reverse proxy, it’s fair to compare both.

* Both Nginx and Varnish can be used as a reverse proxy cache, also for load balancing between two or more Apache servers that will deliver the dynamic content.
* Varnish Cache has a lot of flexibility, allowing developers to create a more complex caching structure than Nginx.
* Varnish Cache Configuration Language (VCL). VCL allows developers to specify request handling rules and set specific caching policies giving them a lot of control over what and how they cache.
* Varnish Cache supports ESI while Nginx doesn’t; Nginx supports SSL where Varnish Cache doesn’t.
* Varnish by default supports PURGE.

**How to install Varnish in MacOS**

With the help of **brew** we can install Varnish cache.

Open your terminal then run,

> brew install varnish

Check varnish is installed,

> brew info varnish

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/wqw991tzta9ao5yzdqax.png)

Varnish has a great configuration system. Most other systems use configuration directives, where you basically turn on and off lots of switches.

We have instead chosen to use a domain specific language called VCL for this.Varnish is configured via Varnish Configuration Language (VCL). 

Once the configuration file is loaded by the system, Varnish translates and compiles.when you install varnish, default configuration file will be available called **default.vcl** file.

In the above image you can able to locate default.vcl file.

> cat /usr/local/etc/varnish/default.vcl

**Setting up Varnish with NodeJS**
where we had already installed Varnish, Now setting up a **NodeJS app**.

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/sqoip883gvvupg72b6xd.png)

Save file as server.js

* Open terminal and start your NodeJS server,

> node server.js

Goto browser and open https://localhost:8080.

**Configure Varnish**

* Open your default.vcl file.
* Setup your server configuration.

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/2zo15b9m2znohhxcchrm.png)

* Handle request methods, by default varnish supports GET and HEAD method.
* Handle backend response, once varnish fetch content from backend we can set ttl(time to live) and other configurations like handling response code.

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/a3gwu6ovjx8pjnsije7c.png)

* We can control whether or not our request is being cached in our browser inspector, we ought to add the following snippet to our Varnish config file, into the **sub vcl_deliver**.

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/vznp5qn3t2hof3o72xnu.png)

* Start your varnish server.
* Goto your browser, then we can see the feedback in our response headers as HIT or MISS.

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/4mqbe5apdxyzrrcoe0cl.png)

This was just a short tutorial on speeding up your web service using Varnish.
You can use Varnish with any backend server like Python, PHP, NodeJS.

In built VCL makes life easy. Based on our needs we can stick with Nginx or Varnish to boost our site performance.

Please find full version of default.vcl file [Github](https://github.com/a8hok/varnish-cache/blob/master/default.vcl)

Video link [youtube](https://www.youtube.com/watch?v=o5NBIb7rf_A)