# V2ray Podman or Kubernetes or Docker

This is based on a modification of v2ray-caddy-cdn in this [repository](https://github.com/miladrahimi/v2ray-docker-compose/tree/master/v2ray-caddy-cdn), podman  pod YAML file

# V2ray-server

## USE

In this solution, you need one server (upstream) and a domain/subdomain added to a CDN service.

- Upstream Server: A server that has free access to the Internet.
- CDN Service: A Content delivery network like [Cloudflare](https://cloudflare.com/), [ArvanCloud](https://arvancloud.ir/) or [DerakCloud](https://derak.cloud/).

```
(Client) <-> [ CDN Service ] <-> [ Upstream Server ] <-> (Internet)
```

This solution provides VMESS over Websockets + TLS + CDN. 

Follow these steps to setup V2Ray + Caddy (Web server) + CDN:

1. In your CDN, create an `A` record pointing to your server IP with the proxy option turned off.
2. Install Docker and Docker-compose on your server.
3. Copy the `v2ray-caddy-cdn` and the `utils` directories into the server.
4. Run `./utils/bbr.sh` to speed up server network.
5. Run `cat /proc/sys/kernel/random/uuid` to generate a UUID.
6. Replace `<UPSTREAM-UUID>` in `v2ray/config/config.json` with the generated UUID.
7. Replace `<EXAMPLE.COM>` in `caddy/Caddyfile` with your domain/subdoamin.
8. Run `podman play kube v2ray-pod.yaml`
9. (Optional) In your CDN, turn the proxy option on for the record.
10. Run `./vmess.py` to generate client configuration (link).
11. To remove, run `podman pod stop v2ray` and then run `podman pod rm v2ray`

You don't need to turn the cloud (proxy) on in your CDN (step 10) when the Internet is not blocked. When it's off, clients connect to the server directly and CDN services also don't charge you any fee.





# V2ray-client

## USE

1. Use the . /vmess to generate the URL in v2ray windows desktop client to generate the config.json client file
2. Move the generated config.json to the /etc/v2ray/ directory to overwrite the original file
3. Finally, use `podman-compose up -d` or `docker-compose up -d` to run

## [Proxychains](https://github.com/rofl0r/proxychains-ng)

```
 ProxyChains is a UNIX program, that hooks network-related libc functions
  in DYNAMICALLY LINKED programs via a preloaded DLL (dlsym(), LD_PRELOAD)
  and redirects the connections through SOCKS4a/5 or HTTP proxies.
  It supports TCP only (no UDP/ICMP etc).

  The way it works is basically a HACK; so it is possible that it doesn't
  work with your program, especially when it's a script, or starts
  numerous processes like background daemons or uses dlopen() to load
  "modules" (bug in glibc dynlinker).
  It should work with simple compiled (C/C++) dynamically linked programs
  though.

  If your program doesn't work with proxychains, consider using an
  iptables based solution instead; this is much more robust.

  Supported Platforms: Linux, BSD, Mac, Haiku.
```

## USE

`proxychains.sh`in `/v2ray-podman/v2ray-client`

If you want to change the port number, please change it in the last line of /etc/proxychains.conf file

Finally you can use `xy` to let the programs that need it go through the proxy

such as ` xy curl cip.cc`

