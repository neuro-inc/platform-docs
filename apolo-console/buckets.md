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

{% tab title="Apolo Console" %}
You can view available buckets in the **Buckets** tab in the Apolo console:

![](<../../.gitbook/assets/console_screenshots/buckets.png>)

Click on your bucket to open its detailed view:

![](<../../.gitbook/assets/console_screenshots/bucket-information.png>)

Here you also can add credentials to your bucket:

{% endtab %}
{% endtabs %}

