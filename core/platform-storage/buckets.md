# Buckets

In addition to other types of platform storage, Apolo provides bucket management functionality.&#x20;

## Managing buckets

{% tabs %}
{% tab title="CLI" %}
In the CLI, bucket management can be performed via the `apolo blob` command.

#### Listing available buckets

```
$ apolo blob lsbucket
```

#### Getting info about a bucket

```
$ apolo blob statbucket <bucket-name>
```

#### Creating a bucket

```
$ apolo blob mkbucket --name <bucket-name>
```

#### Removing a bucket

```
$ apolo blob rmbucket <bucket-name>
```

You can find more info about bucket management commands in the [blob](https://app.gitbook.com/s/-MOkWy7dB5MDbkSII8iF/commands/blob "mention") .
{% endtab %}

{% tab title="Web UI" %}
You can view available buckets in the **Buckets** tab of your dashboard:

![](<../../.gitbook/assets/зображення (4).png>)

Click on your bucket to open its detailed view. From here, you can crete folders and upload files to the bucket:

![](<../../.gitbook/assets/зображення (3).png>)
{% endtab %}
{% endtabs %}

