## Backstage LDAP Integration with Online Test Server

To test backstage LDAP integration we have used an online ldap test server provided by forum systems

**Installation**

The `@backstage/plugin-catalog-backend-module-ldap` package needs to be installed to enable LDAP integration within Backstage.

1. Navigate to your Backstage root directory.
2. Run the following command:

```bash
yarn add --cwd packages/backend @backstage/plugin-catalog-backend-module-ldap
```

**Configuration**

1. Update the `catalog.ts` file located in `packages/backend/src/plugins/`. Import the `LdapOrgEntityProvider` class.

2. Within the `createPlugin` function, add the `LdapOrgEntityProvider` to the `builder.addEntityProvider` call. Configure the provider using the `fromConfig` method with environment variables and desired settings.

3. Update your `app-config.yaml` file to define the LDAP server details and how to import data. Here's an example configuration:

```yaml
catalog:
  processors:
    ldapOrg:
      providers:
      - target: ldap://ldap.forumsys.com  # Replace with your target URL
        bind:
          dn: cn=read-only-admin,dc=example,dc=com  # Replace with appropriate credentials
          secret: password  # Replace with actual password
        users:
          dn: dc=example,dc=com  # Specify the base DN for users
          options:
            filter: (uid=*)  # Filter to retrieve all users
          map:
            description: l  # Map description attribute
            set:
              metadata.customField: 'bottomline ldap'  # Set a custom field
        groups:
          dn: dc=example,dc=com  # Specify the base DN for groups
          options:
            filter: (ou=*)  # Filter to retrieve all groups
          map:
            description: l  # Map description attribute
```

**Explanation of Configuration Parameters:**

* `target`: URL of the LDAP server (use `ldaps://` for SSL)
* `bind`: Credentials for read-only access
    * `dn`: Distinguished Name of the bind user
    * `secret`: Password for the bind user
* `users`: Configuration for importing users
    * `dn`: Base DN for searching users
    * `options.filter`: LDAP filter to select users (e.g., `(uid=*)` for all)
    * `map`: Mapping between LDAP attributes and Backstage user fields
        * `description`: Set the description field in Backstage
        * `set`: Set a custom field with a value
* `groups`: Configuration for importing groups
    * `dn`: Base DN for searching groups
    * `options.filter`: LDAP filter to select groups (e.g., `(ou=*)` for all)
    * `map`: Mapping between LDAP attributes and Backstage group fields (similar to `users.map`)

**Results**

Following these steps should successfully integrate Backstage with the online LDAP test server. Users and groups from the server will be reflected in Backstage. However, group hierarchy might not be imported correctly.

**Note:**

* Replace the example configuration details with your actual LDAP server information.
* The provided filters retrieve all users and groups. You can adjust them based on your specific needs.
