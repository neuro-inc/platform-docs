# Managing Users and Quotas

Neu.ro creates a cluster based on the cluster configuration file you send to our team. The user who creates the cluster is designated the admin of the cluster. You must add users and, optionally, quotas before you can start using the nodes in the cluster. You can know the list of clusters that you can manage by running the `neuro admin get-clusters` command.

```text
> neuro admin get-clusters
company-cluster                                                                                      
 Status      Deployed                                                                              
 Cloud       azure                                                                                 
 Region      eastus                                                                                
 Zones       2                                                                                     
 Node pools                                                                                        
               Machine            CPU   Memory   Preemptible                     GPU   Min   Max   
              ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  
               Standard_D8_v3     7.0    28.0G        ×                                  1    10   
               Standard_NC6       5.0    50.0G        ×         1 x nvidia-tesla-k80     0    30   
               Standard_NC6       5.0    50.0G        √         1 x nvidia-tesla-k80     0    30   
               Standard_NC6s_v3   5.0   106.0G        ×        1 x nvidia-tesla-v100     0    10   
               Standard_NC6s_v3   5.0   106.0G        √        1 x nvidia-tesla-v100     0    10   
                                                                                                   
 Storage     Premium tier Azure Files with LRS replication type and 3072 GiB file share size       

```

You can know the list of clusters that you can access by running the `neuro config get-clusters` command.

You can manage users and user quotas from the CLI. The next sections discuss how you can manage users and user quotas.

## What are the different roles available?

Neu.ro provides three predefined roles that you can assign to a user - `user`, `manager`, `admin`. The default role is User. The following table describes what each of the roles can do. Please note that any registered platform user may create new clusters; upon creation, a user automatically gets an Admin role in it.

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Role</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
      <th style="text-align:left"><b>Privileges</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">User</td>
      <td style="text-align:left">The general users of the platform without special privileges. These users
        can manage their own and shared jobs, storage, and images.</td>
      <td style="text-align:left">
        <ul>
          <li>Manage own and shared jobs</li>
          <li>Manage own and shared storage</li>
          <li>Manage own and shared images</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Manager</td>
      <td style="text-align:left">The users who can manage all jobs, storage, and images on the cluster,
        as well as manage other users.</td>
      <td style="text-align:left">
        <ul>
          <li>Manage all cluster jobs</li>
          <li>Manage all cluster storage</li>
          <li>Manage all cluster images</li>
          <li>Manage users</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Admin</td>
      <td style="text-align:left">The same as Manager, but additionally they can change cluster configuration
        or remove clusters. The admins can also add node pools.</td>
      <td style="text-align:left">
        <ul>
          <li>Manage all cluster jobs</li>
          <li>Manage all cluster storage</li>
          <li>Manage all cluster images</li>
          <li>Manage users</li>
          <li>Remove clusters</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## How to add a user to a cluster?

You can add any number of users to a cluster. However, before adding a user you must decide on the role and the quota that you want to set for the user.

The role determines the access rights a user will have on the Neu.ro platform. The quota determines the number of GPU or CPU hours a user can use.

```text
> neuro admin add-cluster-user neuro-public john user
Added john to cluster default as user
```

## How to remove a user from the cluster?

You can remove a user from the cluster if you are an Admin or Manager. You must use the neuro admin remove-cluster-user command to remove the user. By doing this, you revoke the user’s access to the cluster. All the entities belonging to the user are left \(storage, jobs, images, etc\), and you can add this user back later.

```text
> neuro admin remove-cluster-user default john
Removed john from cluster default
```

## How to promote/demote a user?

You need to remove a user from a given cluster and to add them again with the desired role. All the entities belonging to the user \(storage, jobs, images, etc\) are not affected during removal.

## How to manage a user’s quota?

The quota determines the number of GPU or CPU hours a user can use. By default, a user has 100 GPU hours and unlimited CPU hours.

You can set a user’s quota by using one of these commands:

* `neuro admin set-user-quota`: Use this to set the quota to a certain number of hours. You provide the quota in the number of hours such as 3.5h, 20h, 50h, and so on.

```text
> neuro admin set-user-quota -g 50h default john
New quotas for john on cluster default:
GPU: 3000m
CPU: infinity
```

* `neuro admin add-user-quota`: User this to add a certain number of hours to the existing user quota.

```text
> neuro admin add-user-quota -g 50h default john
New quotas for john on cluster default:
GPU: 6000m
CPU: infinity
```

Note, to enter the quota in minutes you must convert the quota to minutes. For example, to limit the quota hours to 30 minutes, you must enter the quota as `30m`:

```text
> neuro admin set-user-quota -g 30m default john
New quotas for john on cluster default:
GPU: 30m
CPU: infinity
```

You can also set the unlimited quota for a user:

```text
> neuro admin set-user-quota default john
New quotas for john on cluster default:
GPU: infinity
CPU: infinity
```

You can check the quota of any user of a cluster if you are a manager or an admin of the cluster.

```text
> neuro config show-quota john
GPU: spent: 91h 37m (quota: 200h 00m, left: 108h 23m)
CPU: spent: 1036h 00m (quota: infinity)
```

