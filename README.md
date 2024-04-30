Uses a cloudflare tunnel to allow hosting a minecraft server without a static IP address. 

## Running the server

1. Create a cloudflare tunnel for your domain
2. Add a `TXT` DNS record to the domain or subdomain to which the tunnel is bound with the value `cloudflared-use-tunnel`
3. Add your cloudflare tunnel token to `docker-compose.yml`
4. Run `docker compose up -d` to start the server

```bash
# start the minecraft server and cloudflare tunnel
docker compose up -d
```

## Connecting to the server

1. Install the `modflared` mod for your version of minecraft from https://modrinth.com/mod/modflared
2. Connect to the server using the hostname the cloudflare tunnel was bound to in step 1 of [running the server](#running-the-server)
