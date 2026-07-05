# Grapity Try Examples

Example files for the guided onboarding at [try.grapity.dev](https://try.grapity.dev).

These files match the missions shown in the panel. You can download them directly from the panel UI or clone this repository.

## Files

| File | Purpose |
|---|---|
| `payments-api.yaml` | Rich OpenAPI 3.1 Payments API for the registry missions. |
| `payments-api-breaking.yaml` | Same spec with a breaking change to demonstrate compatibility protection. |
| `payments-api-evolved.yaml` | Backward-compatible evolution with an optional query parameter, bumping the version to 1.1.0. |
| `grapity.yaml` | Project config for `grapity materialize`. |
| `gateway.config.yaml` | Kong gateway config for the gateway mission. |
| `docker-compose.kong.yml` | Local Kong + PostgreSQL stack for the optional bonus mission. |

## Quick start

1. Install the CLI:

   ```bash
   npm install -g @grapity/grapity
   ```

2. Initialize against your demo instance (copy the command from the panel):

   ```bash
   grapity init --remote --url https://<your-registry-hostname> \
     --auth keycloak \
     --keycloak-server https://keycloak.grapity.dev \
     --keycloak-realm <your-realm> \
     --keycloak-client-id <your-cli-client-id>
   ```

3. Export your client secret:

   ```bash
   export GRAPITY_CLIENT_SECRET="<your-cli-client-secret>"
   ```

4. Push the Payments API:

   ```bash
   grapity registry push ./payments-api.yaml --name payments-api
   ```

5. Try the breaking change:

   ```bash
   grapity registry push ./payments-api-breaking.yaml --name payments-api
   ```

   This should fail with a breaking-change error. Then push the safe evolution:

   ```bash
   grapity registry push ./payments-api-evolved.yaml --name payments-api
   ```

6. Materialize the locked spec:

   ```bash
   grapity materialize payments-api
   ```

7. Push and preview the gateway config:

   ```bash
   grapity gateway push ./gateway.config.yaml
   grapity gateway preview --name payments-gateway --env staging
   ```

8. Optional: provision to a local Kong:

   ```bash
   docker compose -f docker-compose.kong.yml up -d
   grapity gateway provision --name payments-gateway --env staging --sync
   curl http://localhost:8000/v1/payments
   ```

## Learn more

- [Grapity quickstart](https://grapity.dev/docs/getting-started/quickstart)
- [CLI reference](https://grapity.dev/docs/cli-reference)
- [Grapity on GitHub](https://github.com/grapitydev/grapity)
