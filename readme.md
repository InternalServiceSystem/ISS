# Internal Services System

I have several internal services that I host on The Homestead. I like SSL. I like DNS. I like all the good things that come with containers. But setting that up for each service can be somewhat cumbersome, even with automation. So, in order to ease that pain, I built this framework to register those services and share them out internally. It is based on [Traefik](https://containo.us/traefik/) and makes use of Amazon Route53 and LetsEncrypt to handle SSL.

## Setting up ISS

Using this system is fairly simple. Create a .env file or otherwise set environment variables for your AWS credentials and region. Additionally, create an environment variable called AWS_HOSTED_ZONE_ID and set it to the domain you are hosting in route53. Then simply run `docker-compose up -d` and you should be off to the races.

## Using ISS with another project

Registering another container with traefik is easy. Add the following labels to the container and traefik will handle the rest.

```yaml
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.whoami.rule=Host(`whoami.internal.murawsky.net`)"
  - "traefik.http.routers.whoami.entrypoints=websecure"
  - "traefik.http.routers.whoami.tls.certresolver=letsencrypt"
  - "traefik.port=80"
```

The above example is for the whoami service. Simply replace whoami with your service name and you should be all set.

## Provided Services

Two base services are provided with this system by default.

| URL                           | Descdription                                                      |
| ----------------------------- | ----------------------------------------------------------------- |
| iss.internal.murawsky.net/    | The traefik dashboard for viewing current status.                 |
| whoami.internal.murawsky.net/ | The whoami service. Provides functional check for traefik routing |

## TODO

* Build backup for server certificates

## Credits

This is based on many examples found throughout the web. The ones I relied on heavily are listed below.

* [Traefik Guide - Docker and Lets Encrypt](https://docs.traefik.io/v1.4/user-guide/docker-and-lets-encrypt/)
* [Traefik Documentation - ACME protocol](https://docs.traefik.io/https/acme/)
* [Blog Post - Dashboard through HTTPS](https://blog.creekorful.com/2020/01/how-to-expose-traefik-2-dashboard-securely-docker-swarm/)
