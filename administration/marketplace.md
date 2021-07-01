# Marketplace

## Introduction

The Marketplace is a space for users to share objects such as models, Docker images, and running APIs. The Marketplace is implemented on a per-cluster basis - there is a single Marketplace for each cluster, and every user on this cluster can access the Marketplace.

## Activating the Marketplace on your cluster

For the Marketplace to be accessible on your cluster, you will need to do the following:

* Create the `registry` user:

```text
> neuro admin add-cluster-user CLUSTER_NAME registry user
```

* Create the `registry` folder on the platform storage:

```text
> neuro mkdir -p storage:/registry
```

* Share the `registry` storage folder to `public` with \`read\` and \`write\` permissions:

```text
> neuro acl grant storage:/registry public write
```

When this is done, the Marketplace will be activated on your cluster.

## Sharing objects in the Marketplace

If a user wants to share an object with others through the Marketplace, they will need to:

* Share the corresponding object to `public` with \`read\` access:

```text
> neuro acl grant <object-URI> public read
```

* Write a JSON description of the shared object.
* Put the JSON file and the image file representing the object's thumbnail in the Marketplace to the storage respectively as:

```text
storage:/registry/<user_name>_<project_name>.json
storage:/registry/<user_name>_<project_name>.png
```

After this, the object will be accessible for all users through the Marketplace.

## Removing objects from the Marketplace

If a user wants to remove an object from the Marketplace, they will need to remove the corresponding JSON file and the object's thumbnail file from the platform storage.

