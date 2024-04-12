---
title: Local development
weight: 6
pcx_content_type: concept
---

# Develop locally

Hyperdrive can be used when developing and testing your Workers locally by connecting to any local database instance running on your machine directly. Local development uses [Wrangler](/workers/wrangler/install-and-update/), the command-line interface for Workers, to manage local development sessions and state.

## Configure local development

{{<Aside type="note">}}

This guide assumes you are using `wrangler` version `3.27.0` or later.

If you are new to Hyperdrive and/or Cloudflare Workers, refer to [Hyperdrive tutorial](/hyperdrive/get-started/) to install `wrangler` and deploy their first database.

{{</Aside>}}

To specify a database to connect to when developing locally, you can:

* **Recommended** Create a `WRANGLER_HYPERDRIVE_LOCAL_CONNECTION_STRING_<BINDING_NAME>` environmental variable with the connection string of your database. `<BINDING_NAME>` is the name of the binding assigned to your Hyperdrive in your `wrangler.toml` or Pages configuration. This allows you to avoid committing potentially sensitive credentials to source control in your `wrangler.toml`, if your test/development database is not ephemeral. If you have configured multiple Hyperdrive bindings, replace `<BINDING_NAME>` with the unique binding name for each.
* Set `localConnectionString` in `wrangler.toml`.

If both the `WRANGLER_HYPERDRIVE_LOCAL_CONNECTION_STRING_<BINDING_NAME>` environmental variable and `localConnectionString` in `wrangler.toml` are set, `wrangler dev` will use the environmental variable instead. Use `unset WRANGLER_HYPERDRIVE_LOCAL_CONNECTION_STRING_<BINDING_NAME>` to unset any existing environmental variables. 

For example, to use the environmental variable, export the environmental variable before running `wrangler dev`:

```sh
# Your configured Hyperdrive binding is "TEST_DB" 
$ export WRANGLER_HYPERDRIVE_LOCAL_CONNECTION_STRING_TEST_DB="postgres://user:password@localhost:5432/databasename"
# Start a local development session referencing this local instance
$ npx wrangler dev
```

To configure a `localConnectionString` in `wrangler.toml`, ensure your Hyperdrive bindings have a `localConnectionString` property set:

```toml
[[hyperdrive]]
binding = "TEST_DB"
id = "c020574a-5623-407b-be0c-cd192bab9545"
localConnectionString = "postgres://user:password@localhost:5432/databasename"
```

## Use `wrangler dev`

The following example shows you how to check your wrangler version, set a `WRANGLER_HYPERDRIVE_LOCAL_CONNECTION_STRING_TEST_DB` environmental variable, and run a `wrangler dev` session:

```sh
# Confirm you are using wrangler v3.0+
$ npx wrangler --version
⛅️ wrangler 3.27.0

# Set your environmental variable: your configured Hyperdrive binding is "TEST_DB".
export WRANGLER_HYPERDRIVE_LOCAL_CONNECTION_STRING_TEST_DB="postgres://user:password@localhost:5432/databasename"

# Start a local dev session:
$ npx wrangler dev

# Outputs:
------------------
Found a non-empty WRANGLER_HYPERDRIVE_LOCAL_CONNECTION_STRING_TEST_DB variable. Hyperdrive will connect to this database
during local development.

wrangler dev now uses local mode by default, powered by 🔥 Miniflare and 👷 workerd.
To run an edge preview session for your Worker, use wrangler dev --remote
Your worker has access to the following bindings:
- Hyperdrive configs:
  - TEST_DB: c020574a-5623-407b-be0c-cd192bab9545
⎔ Starting local server...

[mf:inf] Ready on http://127.0.0.1:8787/
[b] open a browser, [d] open Devtools, [l] turn off local mode, [c] clear console, [x] to exit
```

`wrangler dev` separates local and production (remote) data. A local session does not have access to your production data by default. To access your production (remote) Hyperdrive configuration, pass the `--remote` flag when calling `wrangler dev`. Any changes you make when running in `--remote` mode cannot be undone.

Refer to the [`wrangler dev` documentation](/workers/wrangler/commands/#dev) to learn more about how to configure a local development session.

## Related resources

* Use [`wrangler dev`](/workers/wrangler/commands/#dev) to run your Worker and Hyperdrive locally and debug issues before deploying.
* Learn [how Hyperdrive works](/hyperdrive/configuration/how-hyperdrive-works/).
* Understand how to [configure query caching in Hyperdrive](/hyperdrive/configuration/query-caching/).