---
title: 'Tenant Management'
description: ''
distributions: ['Enterprise']
---

This guide contains detailed information on how to perform common tenant management operations for {{% tts %}} Enterprise.

<!--more-->

{{< cli-only hint-enterprise=true >}}

## Authentication

Instead of using API keys and OAuth access tokens as for other {{% tts %}} services, the tenant services require using **tenant admin keys** for authentication.

> Tenant admin keys are configured in the Identity Server as described [here]({{< ref "/reference/configuration/identity-server#tenant-administration-options" >}}).

This means all tenant management CLI commands will need to have the `--tenant-admin-key` flag appended, followed by a tenant admin key that you configured in the Identity Server. See the section below for examples.

## Basic Operations

### Create a Tenant

To create a new tenant with the tenant ID `tenant1`:

```
$ TENANT_ADMIN_KEY=14596c0bdb9b11f6a97273e7c09167531571efa5898274e48660ee8d08f62b67
$ tti-lw-cli tenant create tenant1 --tenant-admin-key $TENANT_ADMIN_KEY
```

### Get a Tenant

To get a tenant `tenant1`, i.e. to see details like when it was created or updated, and which its capabilities are:

```
$ tti-lw-cli tenant get tenant1 --tenant-admin-key $TENANT_ADMIN_KEY
```

### List Tenants

To get descriptions of all tenants:

```
$ tti-lw-cli tenant list --tenant-admin-key $TENANT_ADMIN_KEY
```

### Delete a Tenant

To delete a tenant `tenant1`:

```
$ tti-lw-cli tenant delete tenant1 --tenant-admin-key $TENANT_ADMIN_KEY
```

### Get Tenant Identifiers for Device EUIs

To get a tenant ID based on your device's DevEUI and JoinEUI:

```
$ tti-lw-cli tenant get-identifiers-for-end-device-euis --dev-eui 0004A30B001C0530 --join-eui 800000000000000C --tenant-admin-key $TENANT_ADMIN_KEY
```

### Get Tenant Identifiers for a Gateway EUI

To get a tenant ID based on your gateways's EUI:

```
$ tti-lw-cli tenant get-identifiers-for-gateway-eui --gateway-eui 00800000A00009EF --tenant-admin-key $TENANT_ADMIN_KEY
```

### Search for Tenants

You can search tenants based on multiple criteria such as name, maximum number of entities, state, configuration, etc. These criteria are defined using flags. To see all available flags, use the following command:

```
$ tti-lw-cli tenant search --help
```

For example, to search for recently deleted tenants:

```
$ tti-lw-cli tenant search --deleted --tenant-admin-key $TENANT_ADMIN_KEY
```

### Update a Tenant

You can update a tenant in multiple ways, for example change its name, set a maximum number of entities, change configuration and billing settings, etc. Use the following command to list all available flags:

```
$ tti-lw-cli tenant update --help
```

For example, to update a maximum number of end devices that can be registered in a tenant:

```
$ tti-lw-cli tenant update --tenant-id tenant1 --max-end-devices 3 --tenant-admin-key $TENANT_ADMIN_KEY
```

## Setting a DevAddr Prefix for a Tenant

In a multi-tenant deployment, different tenants share the same NetID, but they use different DevAddr blocks. DevAddr blocks need to be configured for every tenant in order for Packet Broker to be able to distinguish traffic that belongs to each of those tenants.

Setting a DevAddr prefix for a tenant can be done using the `update` operation mentioned in the section above. To set a DevAddr prefix for a tenant `tenant1`:

```
$ tti-lw-cli tenant update --tenant-id tenant1 --configuration.default-cluster.ns.dev-addr-prefixes 27123400/24
```