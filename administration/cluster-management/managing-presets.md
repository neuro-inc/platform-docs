# Managing Presets

Once your cluster's node pools are set up, you can configure resource presets that will be available for you and your team during the work process. This can be done both through the CLI and in the Web UI.

{% tabs %}
{% tab title="CLI" %}
## Checking your cluster's presets

You can view your current cluster's resource presets by running `neuro config show` and referring to the **Resource Presets** section in its output:

```text
Resource Presets:
 Name               #CPU   Memory   Preemptible   Preemptible Node   GPU                     Jobs Avail
────────────────────────────────────────────────────────────────────────────────────────────────────────
 cpu-small             1     4.0G        ×               ×                                           63
 cpu-medium            3    11.0G        ×               ×                                           18
 cpu-large             7    26.0G        ×               ×                                            7
 gpu-k80-small         5    48.0G        ×               ×           1 x nvidia-tesla-k80            28
 gpu-k80-small-p     5.0    48.0G        √               √           1 x nvidia-tesla-k80            10
 gpu-v100-small        5    95.0G        ×               ×           1 x nvidia-tesla-v100           10
 gpu-v100-small-p    5.0    95.0G        √               √           1 x nvidia-tesla-v100           10
```

## Modifying and adding presets

You can easily modify or add resource presets by using the `neuro admin update-resource-preset` command.

For example, to change the amount of memory accessible through the existing **cpu-large** preset to 32GB, run:

```bash
> neuro admin update-resource-preset -m 32G company-cluster cpu-large
```

To add a new preset, just provide its name and parameters in the `neuro admin update-resource-preset` command. You can learn more about using this command [here](https://neu-ro.gitbook.io/neu-ro-cli-reference/commands/admin).

## Deleting presets

You can delete resource presets by using the `neuro admin remove-resource-preset` command. For example:

```bash
> neuro admin remove-resource-preset company-cluster cpu-medium
```
{% endtab %}

{% tab title="Web UI" %}
## Checking your cluster's presets

You can view your current cluster's resource presets in the **Information** and **Cluster management** tabs:

![](../../.gitbook/assets/image%20%28116%29.png)

## Adding presets

To add a new preset, click the **Add** icon in the Cluster management tab:

![](../../.gitbook/assets/image%20%28121%29.png)

After that, enter the desired preset parameters and click the **Save** icon:

![](../../.gitbook/assets/image%20%28122%29.png)

## Modifying presets

To modify an existing preset, click the **Edit** icon next to it:

![](../../.gitbook/assets/image%20%28118%29.png)

After that, enter the new parameters and click the **Save** icon:

![](../../.gitbook/assets/image%20%28117%29.png)

## Deleting presets

To delete a preset, click the **Delete** icon next to it:

![](../../.gitbook/assets/image%20%28119%29.png)

After that, click the **Save** icon to confirm your action:

![](../../.gitbook/assets/image%20%28120%29.png)
{% endtab %}
{% endtabs %}

