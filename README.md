# fabric-grid-ops-app

Operational **Fabric App** (public preview) built with **Rayfin**, the open-source
TypeScript SDK/CLI: a grid-operations console where an operator acknowledges and
annotates demand-deviation alerts raised by Real-Time Intelligence (Activator) and
manages alert-threshold reference data.

**Status: 🚧 in development** — the outline below is the plan; this README grows with
the build.

## Planned build

- Rayfin entity decorators → auto-generated Fabric SQL database + GraphQL API.
- Entra SSO, row-level security, hosting via `rayfin up` (local Docker stack for dev).
- Companion to
  [fabric-energy-lakehouse](https://github.com/gonzalonao/fabric-energy-lakehouse) —
  closes the loop on its streaming alerts.

## License

[MIT](LICENSE)
