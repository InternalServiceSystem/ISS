# Internal Services System

I have several internal services that I host on The Homestead. I like SSL. I like DNS. I like all the good things that come with containers. But setting that up for each service can be somewhat cumbersome, even with automation. So, in order to ease that pain, I built this framework to register those services and share them out internally. It is based on [Traefik](https://containo.us/traefik/) and makes use of Amazon Route53 and LetsEncrypt to handle SSL.

## Using the ISS

Using this system is fairly simple. Create a .env file or otherwise set environment variables for your AWS credentials and region. Additionally, create an environment variable called AWS_HOSTED_ZONE_ID and set it to the domain you are hosting in route53. Then simply run `docker-compose up -d` and you should be off to the races.

## TODO

* Research how to host the API securely and at it's own internal domain
  * `"--api.insecure=true"` in the config
* Build hooks for internal DNS using something. Consul maybe?
* Research better methods of config than `command: --`. Perhaps a config file. Perhaps consul?
* Build backup for server certificates
