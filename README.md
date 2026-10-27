# nushell-debian

> Fork of [dariogriffo/nushell-debian](https://github.com/dariogriffo/nushell-debian) (MIT); the original packaging is by Dario Griffo. Served at [deb.gaofn.xyz](https://deb.gaofn.xyz) for Debian 12 (bookworm) and 13 (trixie).

Debian and Ubuntu packaging for [Nushell](https://www.nushell.sh) — a new
type of shell where pipelines carry structured, typed data instead of raw
text, with first-class support for JSON, YAML, CSV, TOML, SQLite and more.

The package ships the `nu` binary (built statically against musl where
upstream provides it, so the shell has zero runtime dependencies) plus the
official plugins: `nu_plugin_formats`, `nu_plugin_gstat`, `nu_plugin_inc`,
`nu_plugin_polars`, `nu_plugin_query`. On install, `nu` is registered in
`/etc/shells` (via `add-shell`), so `chsh -s /usr/bin/nu` works immediately;
it is deregistered on removal.

Packages are built automatically from official upstream releases (usually
within hours) and served from **[deb.gaofn.xyz](https://deb.gaofn.xyz)** for
Debian (bookworm, trixie, forky, sid) and Ubuntu (jammy, noble, questing,
resolute) on amd64, arm64, armhf and riscv64.

## Install

```bash
sudo install -d -m 0755 /etc/apt/keyrings
sudo curl -fsSL https://deb.gaofn.xyz/deb.gaofn.xyz.gpg -o /etc/apt/keyrings/deb.gaofn.xyz.gpg
sudo curl -fsSL https://deb.gaofn.xyz/deb.gaofn.xyz.sources -o /etc/apt/sources.list.d/deb.gaofn.xyz.sources
# Debian 12 (bookworm): fetch deb.gaofn.xyz-bookworm.sources instead
sudo apt update
sudo apt install -y nushell
```

Then start it with `nu`, or make it your login shell:

```bash
chsh -s /usr/bin/nu
```

Plugins are registered per user from inside nu, e.g.:

```nu
plugin add /usr/bin/nu_plugin_query
```

### Manual Installation

1. Download the .deb package for your Debian version available on
   the [Releases](https://github.com/gaofengnan/nushell-debian/releases) page.
2. Install the downloaded .deb package.

```sh
sudo dpkg -i <filename>.deb
```

## How it works

- `check-upstream.yml` polls upstream hourly; a new release dispatches `release.yml`.
- `release.yml` builds binary packages (Docker, per suite × architecture, from
  the upstream release tarballs) and source packages (`.dsc`), then publishes
  a GitHub release tagged `<version>+<build>`.
- The deb.gaofn.xyz mirror ingests published releases automatically.

Manual build: `./build.sh <version> <build> [arch|all]` (e.g. `./build.sh 0.114.1 1 all`).

## Links

- Upstream: https://github.com/nushell/nushell
- Repository: https://deb.gaofn.xyz
