# Roles

Access control on the Neu.ro platform is based on **roles**. Each role contains a set of permissions for various entities and actions. In this way, users with different roles will have different levels of access on the platform. By default, every [cluster](clusters-resources.md) has three roles: **User**, **Manager**, and **Admin** \(you can learn more about them [here](../../administration/cluster-management/managing-users-and-quotas.md#what-are-the-different-roles-available)\). Users can also create their own custom roles and grant them to other users.

### Creating new roles

To create a new role, use the `neuro acl add-role {username}/roles/{rolename}` command. For example:

```text
> neuro acl add-role alice/roles/newrole
```

The created role will be called `newrole` and have an empty permission set. You can then add resources to this set using the `neuro acl grant {URI} {username}/roles/{rolename}` command. For example:

```text
> neuro acl grant job:job363 alice/roles/newrole
```

This will add a permission for the `job363` job to the `newrole` role. 

### Using roles

#### Granting roles

You can grant roles to users by running the `neuro acl grant role://{username}/roles/{rolename} {username2}` command:

```text
> neuro acl grant role://alice/roles/newrole bob
```

This will grant the `newrole` role to Bob. This means that Bob will have access to all entities listed under this role.

#### Revoking roles

Roles can be revoked from users with the help of the `neuro acl revoke` command. For example:

```text
> neuro acl revoke role://alice/roles/newrole bob
```

#### Deleting roles

Obsolete roles can be deleted using the `neuro acl remove-role` command. For example:

```text
> neuro acl remove-role alice/roles/newrole
```

Feel free to refer to this [Neu.ro CLI Reference page](https://neu-ro.gitbook.io/neu-ro-cli-reference/commands/acl) to learn more about using the `neuro acl` command.

