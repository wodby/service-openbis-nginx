# openBIS Nginx service for Kubernetes on Wodby

The public entry point of an openBIS app on Wodby: Nginx routes the paths of openBIS's application server, data store
server and file server to their private ports and sends `/` to the lab notebook.

- [Browse Wodby services](https://wodby.com/services)
- [Wodby service documentation](https://wodby.com/docs/2.0/services/)
- [Service manifest reference](https://wodby.com/docs/2.0/services/template/)

## Wodby stacks using this service

- [openBIS application stack](https://github.com/wodby/stack-openbis)

## Service overview

| Property | Manifest configuration |
| --- | --- |
| Service name | `openbis-nginx` |
| Type | Application service |
| Based on | [Nginx Proxy](https://github.com/wodby/service-nginx-proxy) `^1.0.0` |
| Endpoints | `http`: HTTP 80 (main) |
| Service links | openBIS (`backend`) |
| Configuration | `preset`: openBIS rules |

## Rules

| Path | Goes to |
| --- | --- |
| `/` | Redirect to `/openbis/webapp/eln-lims/` |
| `/openbis` | Application server, port 8080 |
| `/datastore_server` | Data store server, port 8081 |
| `/afs-server` | File server, port 8085 |

Uploads have no size limit and stream through without buffering; requests may take up to one hour.

## Maintain a custom version

1. Fork this repository.
2. Edit the service manifest and referenced files.
3. Import the repository as a [Git-backed service](https://wodby.com/docs/2.0/services/create/#create-a-git-backed-service).
4. Reference the service from a stack manifest.

Validate the manifest with:

```bash
wodby service validate-manifest service.yml --org <org-id> --include GUIDANCE.md --include preset.conf.tmpl
```

See the [service manifest reference](https://wodby.com/docs/2.0/services/template/) and the
[managed services index](https://github.com/wodby/services).
