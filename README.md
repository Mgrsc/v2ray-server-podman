# V2RAY Podman play

This is based on a modification of v2ray-caddy-cdn in this [repository](https://github.com/miladrahimi/v2ray-docker-compose/tree/master/v2ray-caddy-cdn), podman  pod YAML file

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

You don't need to turn the cloud (proxy) on in your CDN (step 10) when the Internet is not blocked. When it's off, clients connect to the server directly and CDN services also don't charge you any fee.

