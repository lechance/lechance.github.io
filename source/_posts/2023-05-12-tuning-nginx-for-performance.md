---
titile: Tuning NGINX for Performance
date: 2023-05-12
tags:
- nginx
- tuning
categories: Web
---



NGINX is well known as a high‑performance [load balancer](https://www.nginx.com/products/nginx/load-balancing/), [cache](https://www.nginx.com/products/nginx/caching/), and [web server](https://docs.nginx.com/nginx/admin-guide/web-server/web-server/), powering [over 40% of the busiest websites](https://w3techs.com/technologies/cross/web_server/ranking) in the world. For most use cases, default NGINX and Linux settings work well, but achieving optimal performance sometimes requires a bit of tuning. This blog post discusses some of the NGINX and Linux settings to consider when tuning a system.

You can tune almost any setting, but this post concentrates on the few settings for which tuning benefits the most users. There are settings that we recommend you change only if you have a deep understanding of NGINX and Linux, or as directed by our [Support](https://www.nginx.com/support/) or [Professional Services](https://www.nginx.com/services/) teams, and we don’t cover those here. The Professional Services team has worked with some of the world’s busiest websites to tune NGINX for the maximum level of performance, and is available to work with you on getting the most out of your NGINX or NGINX Plus deployment.

## Introduction

A basic understanding of the NGINX architecture and configuration concepts is assumed. This post does not attempt to duplicate the NGINX documentation, but provides an overview of the various options and links to the relevant documentation.

A good rule to follow when tuning is to change one setting at a time, and set it back to the default value if the change does not improve performance.

We start with a discussion of Linux tuning, because the value of some operating system settings determines how you tune your NGINX configuration.

## Tuning Your Linux Configuration

The settings in modern Linux kernels (2.6+) are suitable for most purposes, but changing some of them can be beneficial. Check the kernel log for error messages indicating that a setting is too low, and adjust it as advised. Here we cover only those settings that are most likely to benefit from tuning under normal workloads. For details on adjusting these settings, please refer to your Linux documentation.

### The Backlog Queue

The following settings relate to connections and how they are queued. If you have a high rate of incoming connections and you are getting uneven levels of performance (for example some connections appear to be stalling), then changing these settings can help.

- `net.core.**somaxconn**` – The maximum number of connections that can be queued for acceptance by NGINX. The default is often very low and that’s usually acceptable because NGINX accepts connections very quickly, but it can be worth increasing it if your website experiences heavy traffic. If error messages in the kernel log indicate that the value is too small, increase it until the errors stop.

  **Note:** If you set this to a value greater than 512, change the `backlog` parameter to the NGINX [`listen`](https://nginx.org/en/docs/http/ngx_http_core_module.html#listen) directive to match.

- `net.core.**netdev_max_backlog**` – The rate at which packets are buffered by the network card before being handed off to the CPU. Increasing the value can improve performance on machines with a high amount of bandwidth. Check the kernel log for errors related to this setting, and consult the network card documentation for advice on changing it.

### File Descriptors

File descriptors are operating system resources used to represent connections and open files, among other things. NGINX can use up to two file descriptors per connection. For example, if NGINX is proxying, it generally uses one file descriptor for the client connection and another for the connection to the proxied server, though this ratio is much lower if HTTP keepalives are used. For a system serving a large number of connections, the following settings might need to be adjusted:

- `sys.fs.**file-max**` – The system‑wide limit for file descriptors
- `**nofile**` – The user file descriptor limit, set in the **/etc/security/limits.conf** file

### Ephemeral Ports

When NGINX is acting as a proxy, each connection to an upstream server uses a temporary, or *ephemeral*, port. You might want to change this setting:

- `net.ipv4.**ip_local_port_range**` – The start and end of the range of port values. If you see that you are running out of ports, increase the range. A common setting is ports 1024 to 65000.
- 

## Tuning Your NGINX Configuration

The following are some NGINX directives that can impact performance. As stated above, we only discuss directives that are safe for you to adjust on your own. We recommend that you not change the settings of other directives without direction from the NGINX team.

### Worker Processes

NGINX can run multiple worker processes, each capable of processing a large number of simultaneous connections. You can control the number of worker processes and how they handle connections with the following directives:

- [`worker_processes`](https://nginx.org/en/docs/ngx_core_module.html#worker_processes) – The number of NGINX worker processes (the default is 1). In most cases, running one worker process per CPU core works well, and we recommend setting this directive to `auto` to achieve that. There are times when you may want to increase this number, such as when the worker processes have to do a lot of disk I/O.
- [`worker_connections`](https://nginx.org/en/docs/ngx_core_module.html#worker_connections) – The maximum number of connections that each worker process can handle simultaneously. The default is 512, but most systems have enough resources to support a larger number. The appropriate setting depends on the size of the server and the nature of the traffic, and can be discovered through testing.

### Keepalive Connections

[Keepalive connections](https://www.f5.com/company/blog/nginx/http-keepalives-and-web-performance) can have a major impact on performance by reducing the CPU and network overhead needed to open and close connections. NGINX terminates all client connections and creates separate and independent connections to the upstream servers. NGINX supports keepalives for both clients and upstream servers. The following directives relate to client keepalives:

- [`keepalive_requests`](https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_requests) – The number of requests a client can make over a single keepalive connection. The default is 100, but a much higher value can be especially useful for testing with a load‑generation tool, which generally sends a large number of requests from a single client.
- [`keepalive_timeout`](https://nginx.org/en/docs/http/ngx_http_core_module.html#keepalive_timeout) – How long an idle keepalive connection remains open.

The following directive relates to upstream keepalives:

- [`keepalive`](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#keepalive) – The number of idle keepalive connections to an upstream server that remain open for each worker process. There is no default value.

To enable keepalive connections to upstream servers you must also include the following directives in the configuration:

```
proxy_http_version 1.1;proxy_set_header Connection "";
```

### Access Logging

Logging every request consumes both CPU and I/O cycles, and one way to reduce the impact is to enable access‑log buffering. With buffering, instead of performing a separate write operation for each log entry, NGINX buffers a series of entries and writes them to the file together in a single operation.

To enable access‑log buffering, include the `buffer=*size*` parameter to the `access_log` directive; NGINX writes the buffer contents to the log when the buffer reaches the `*size*` value. To have NGINX write the buffer after a specified amount of time, include the `flush=*time*` parameter. When both parameters are set, NGINX writes entries to the log file when the next log entry will not fit into the buffer or the entries in the buffer are older than the specified time, respectively. Log entries are also written when a worker process is reopening its log files or shutting down. To disable access logging completely, include the `off` parameter to the `access_log` directive.

### Sendfile

The operating system’s `sendfile()` system call copies data from one file descriptor to another, often achieving zero‑copy, which can speed up TCP data transfers. To enable NGINX to use it, include the `sendfile` directive in the `http` context or a `server` or `location` context. NGINX can then write cached or on‑disk content down a socket without any context switching to user space, making the write extremely fast and consuming fewer CPU cycles. Note, however, that because data copied with `sendfile()` bypasses user space, it is not subject to the regular NGINX processing chain and filters that change content, such as `gzip`. When a configuration context includes both the `sendfile` directive and directives that activate a content‑changing filter, NGINX automatically disables `sendfile` for that context.

### Limits

You can set various limits that help prevent clients from consuming too many resources, which can adversely the performance of your system as well as security and the user experience. The following are some of the relevant directives:

- [`limit_conn`](https://nginx.org/en/docs/http/ngx_http_limit_conn_module.html#limit_conn) and [`limit_conn_zone`](https://nginx.org/en/docs/http/ngx_http_limit_conn_module.html#limit_conn_zone) – Limit the number of client connections NGINX accepts, for example from a single IP address. Setting them can help prevent individual clients from opening too many connections and consuming more than their share of resources.
- [`limit_rate`](https://nginx.org/en/docs/http/ngx_http_core_module.html#limit_rate) – Limits the rate at which responses are transmitted to a client, per connection (so clients that open multiple connections can consume this amount of bandwidth for each connection). Setting a limit can prevent the system from being overloaded by certain clients, ensuring more even quality of service for all clients.
- [`limit_req`](https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req) and [`limit_req_zone`](https://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_zone) – Limit the rate of requests being processed by NGINX, which has the same benefits as setting `limit_rate`. They can also improve security, especially for login pages, by limiting the request rate to a value reasonable for human users but too slow for programs trying to overwhelm your application with requests (such as bots in a [DDoS attack](https://www.f5.com/company/blog/nginx/mitigating-ddos-attacks-with-nginx-and-nginx-plus)).
- [`max_conns`](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#max_conns) parameter to the `server` directive in an `upstream` configuration block – Sets the maximum number of simultaneous connections accepted by a server in an upstream group. Imposing a limit can help prevent the upstream servers from being overloaded. Setting the value to 0 (zero, the default) means there is no limit.
- [`queue`](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#queue) (NGINX Plus) – Creates a queue in which requests are placed when all the available servers in the upstream group have reached their `max_conns` limit. This directive sets the maximum number of requests in the queue and, optionally, the maximum time they wait (60 seconds by default) before an error is returned. Requests are not queued if you omit this directive.

## Caching and Compression Can Improve Performance

Some additional features of NGINX that can be used to increase the performance of a web application don’t really fall under the heading of tuning, but are worth mentioning because their impact can be considerable. They include caching and compression.

### Caching

By enabling caching on an NGINX instance that is load balancing a set of web or application servers, you can dramatically improve the response time to clients while at the same time dramatically reducing the load on the backend servers. Caching is a topic in its own right and we won’t try to cover it here. See the [NGINX Plus Admin Guide](https://docs.nginx.com/nginx/admin-guide/content-cache/).

### Compression

Compressing responses sent to clients can greatly reduce their size, so they use less network bandwidth. Because compressing data consumes CPU resources, however, it is most useful when it’s really worthwhile to reduce bandwidth usage. It is important to note that you should not enable compression for objects that are already compressed, such as JPEG files. For more information, see the [NGINX Plus Admin Guide](https://docs.nginx.com/nginx/admin-guide/web-server/compression/).

For more information, see:

- [Benchmarking NGINX: Better Accuracy and Meaningful Results](https://www.nginx.com/resources/webinars/benchmarking-nginx-better-accuracy-meaningful-results-45-min/) (on‑demand webinar)
- [NGINX reference documentation](https://nginx.org/en/docs/) at nginx.org
- [Compare NGINX Models](https://www.nginx.com/products/nginx/compare-models)
- [NGINX Plus Technical Specifications](https://docs.nginx.com/nginx/technical-specs/)