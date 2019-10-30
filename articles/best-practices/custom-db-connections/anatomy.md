---
description: Best practices for custom database connection anatomy.
classes: topic-page
topics:
  - best-practices
  - custom-database
  - extensibility
  - database-action-scripts
  - custom-database-connections
  - scripts
contentType: reference
useCase:
  - best-practices
  - custom-database
  - database-action-scripts
---
# Custom Database Connection Anatomy

You typically use a [custom database connection](/connections/database/custom-db) to provide access to your own legacy identity store for authentication (sometimes referred to as *legacy authentication*) or user import through [automatic migration](//users/guides/configure-automatic-migration) (referred to as *trickle* or *lazy* migration).

::: note
You can also use custom database connections to proxy access to an Auth0 tenant in scenarios where you use Auth0 multi-tenant architecture. For more information, see [Using Auth0 to Secure Your Multi-Tenant Applications](/design/using-auth0-with-multi-tenant-apps). 
:::

You typically create and configure custom database connections using the [Auth0 Dashboard](/connections/database/custom-db/create-db-connection#step-1-create-and-configure-a-custom-database-connection). You create a database connection and then toggle **Use my own database** to enable editing of the database action scripts. A custom database connection can also be created and configured with the Auth0 [Management API](/api/management/v2#!/Connections/post_connections) and the `auth0` strategy. 

![Enable Use Own Database Option](/media/articles/dashboard/connections/database/connections-db-settings-custom-1.png)

As shown below, you use custom database connections as part of the Universal Login workflow to obtain user identity information from your own legacy identity store for authentication or user import, referred to as *legacy authentiation*.

![Custom Database Connections Flow](/media/articles/connections/database/custom-database-connections.png)

In addition to artifacts common for all database connections types, a custom database connection allows you to configure action scripts (custom code used when interfacing with legacy identity stores). The scripts you choose to configure depend on whether you are creating a connection for legacy authentication or for automatic migration. 

::: panel Best Practice
You can use action scripts as anonymous functions, however anonymous functions make it hard to debug when it comes to interpreting the call-stack generated as a result of any exceptional error conditions. For convenience, we recommend providing a function name for each action script and have supplied some recommended names below.
:::

In a legacy authentication scenario, no new user record is created; the user remains in the legacy identity store and Aut0 uses the identity it contains when authenticating the user.

::: note
Custom database connections are also used outside of the Universal Login workflow. For example, a connection's [`changePassword` action script](#change-password) is called when a password change operation occurs for a user that resides in a legacy identity store.
:::

## Automatic migration

During automatic migration, Auth0 creates a new user in an identity store (database) managed by Auth0. Auth0 uses the identity in the Auth0 managed identity store when authenticating the user. For this to occur, Auth0 first requires the user be authenticated against the legacy identity store and only if this succeeds, will the new user be created in the Auth0 managed database. Auth0 creates the new user using the same id and password that was supplied during authentication. 

::: Best Practice
We recommend that you mark legacy store user identities once they have been migrated to Auth0 to prevent the unintentional re-creation of intentionally deleted users.
:::

## Keep reading

<%= include('../../_includes/_topic-links', { links: [
  'best-practices/custom-db-connections/size',
  'best-practices/custom-db-connections/environment',
  'best-practices/custom-db-connections/execution',
  'best-practices/custom-db-connections/error-handling',
  'best-practices/custom-db-connections/debugging',
  'best-practices/custom-db-connections/testing',
  'best-practices/custom-db-connections/deployment',
  'best-practices/custom-db-connections/performance',
  'best-practices//custom-db-connections/security'
] }) %>