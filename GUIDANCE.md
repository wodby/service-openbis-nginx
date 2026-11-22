# Nginx for openBIS on Wodby

What this service adds to the Nginx Proxy service it is based on. It is the public entry point of an openBIS app:
openBIS's own ports are private.

## Rules

openBIS's rules replace the image's `http-proxy` preset. They are the `preset` config file of this service and can be
overridden there. The preset keeps the name `http-proxy` (`NGINX_VHOST_PRESET`), which also makes the image point the
`app_server` upstream at the linked openBIS service's application server.

- `/` redirects to the lab notebook, `/openbis/webapp/eln-lims/`.
- `/openbis` goes to the application server (port 8080), `/datastore_server` to the data store server (8081,
  `NGINX_OPENBIS_DSS_PORT`) and `/afs-server` to the file server (8085, `NGINX_OPENBIS_AFS_PORT`).
- Other paths are not found.
- The scheme and client address that Wodby's gateway received are passed on in `X-Forwarded-Proto` and
  `X-Forwarded-For`, with the original `Host`.
- `/.healthz` returns 204.
- The image's default rules are off (`NGINX_VHOST_NO_DEFAULTS`).

## Uploads and timeouts

Requests have no size limit and are streamed to openBIS without buffering, in both directions. Proxy timeouts are one
hour (`NGINX_OPENBIS_PROXY_TIMEOUT`), and routes allow requests up to one hour.

## Image

The stock Nginx image; nothing is built for this service.
