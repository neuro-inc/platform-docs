# Buckets

In addition to other types of platform storage, Neu.ro provides bucket management functionality.&#x20;

## Managing buckets

{% tabs %}
{% tab title="CLI" %}
In the CLI, bucket management can be performed via the `neuro blob` command.

#### Listing available buckets

```
$ neuro blob lsbucket
```

#### Getting info about a bucket

```
$ neuro blob statbucket <bucket-name>
```

#### Creating a bucket

```
$ neuro blob mkbucket --name <bucket-name>
```

#### Removing a bucket

```
$ neuro blob rmbucket <bucket-name>
```

You can find more info about bucket management commands in the [CLI reference](https://neu-ro.gitbook.io/neu-ro-cli-reference/commands/blob).
{% endtab %}

{% tab title="Web UI" %}
You can view available buckets in the **Buckets** tab of your dashboard:

![](<../../.gitbook/assets/зображення (4).png>)

Click on your bucket to open its detailed view. From here, you can crete folders and upload files to the bucket:

![](<../../.gitbook/assets/зображення (3).png>)
{% endtab %}
{% endtabs %}

