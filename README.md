Uses a cloudflare tunnel to allow hosting a minecraft server without a static IP address, all in Docker.

## Running the server

### Create a cloudflare tunnel for your domain

Either using the GUI or the `cloudflared` cli - keep track of the tunnel name as you'll need this later as an environment variable `TUNNEL_NAME`

Set a service over the tunnel as `tcp://localhost:25565` - this is telling cloudflare to forward TCP traffic sent to the tunnel to port `25565` on the server, which is the port the minecraft server is configured to run on.

> **Setting `TUNNEL_NAME`**
><details>
> <summary>Linux (bash)</summary>
>
> ```bash
> export TUNNEL_NAME="your-tunnel-name"
> ```
></details>
>
><details>
>    <summary>Windows (PowerShell)</summary>
>
> ```pwsh
> $env:TUNNEL_NAME="your-tunnel-name"
> ```
></details>
>


<details>
    <summary><small>Finding an existing tunnel</small></summary>

If you want to use an existing tunnel you can find it using the `cloudflared` cli

```bash
 cloudflared tunnel list
```
</details>



### Get a token credential file for your cloudflare tunnel


> Using the `cloudflared` cli.
> <details>
> <summary>Linux (bash)</summary>
>
> ```bash
> cloudflared tunnel token --cred-file ./secrets/tunnel.json $TUNNEL_NAME
> ```
></details>
> <details>
>    <summary>Windows (PowerShell)</summary>
>
> ```pwsh
> cloudflared tunnel token --cred-file ./secrets/tunnel.json $env:TUNNEL_NAME
> ```
> </details>


**⚠️ WARNING ⚠️**
This credentials file ***must*** be kept secret ***at all times***

### Run docker compose to start the server and tunnel

```bash
docker compose up -d
```

### Add a `TXT` DNS record to the domain or subdomain to which the tunnel is bound with the value `cloudflared-use-tunnel`

This allows `modflared` to detect that connecting to a cloudflare tunnel is required to access the minecraft server hosted by the tunnel.

## Connecting to the server

1. Install the `modflared` mod for your version of minecraft from https://modrinth.com/mod/modflared
2. Connect to the server using the hostname the cloudflare tunnel was bound to in step 1 of [running the server](#running-the-server)
