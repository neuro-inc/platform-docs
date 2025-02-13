# Projects

Projects are units of collaboration on a platform.

Each Apolo platform artifact e.g. job, storage file, blob, image, app instance, etc. belongs to some project.

Users could invite other users into their project specifying access level (reader / writer / manager / admin). Depending on the access level, other users will be (or will not be) able to read artifacts, run workloads and flows within the projects. Other users within the project will be able to see it.

This, for instance, allows one teammate to manage datasets on a platform storage, while other contributor might perform some EDA and yet another will launch training and inference flows.

Refer to `apolo admin` for project administration and to `apolo config switch-project` to, well, switch between projects.&#x20;

Most of Apolo CLI accept `--project` flag to perform various commands in corresponding projects.

In case you need more coarse grained control, refer to [roles.md](roles.md "mention") or even to CLI's [acl](https://app.gitbook.com/s/-MOkWy7dB5MDbkSII8iF/commands/acl "mention") documentation pages.

### Project roles

Project contributors could have one of available roles: reader, writer, manager or admin. Each subsequent role expands on the previous one.

#### Reader

Readers are only able to read project artifacts: download / read storage, blobs, check job statuses and logs, access workloads via HTTP endpoints, pull images, etc.

#### Writer

User with this role is able to perform updates of artifacts -- write new files or update existing in storage or blobs, run new workloads or terminate existing, push images, etc.

#### Manager

Project managers could also invite other users into projects, promote/demote their roles up to manager access level.

#### Admin

Administrators are able to remove project, promote/demote administrators.

When you create project, you become it's administrator.
