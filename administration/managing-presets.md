# Managing Presets

Once your cluster's node pools are set up, you can configure resource presets that will be available for you and your team during the work process. 

### Checking Your Cluster's Presets

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

### Modifying and Adding Presets

You can easily modify or add resource presets by using the `neuro admin update-resource-preset` command. 

For example, to change the amount of memory accessible through the existing **cpu-large** preset to 32GB, run:

```bash
> neuro admin update-resource-preset -m 32G company-cluster cpu-large
```

To add a new preset, just provide its name and parameters in the `neuro admin update-resource-preset` command. You can learn more about using this command [here](https://neu-ro.gitbook.io/neu-ro-cli-reference/commands/admin).

### Deleting Presets

You can delete resource presets by using the `neuro admin remove-resource-preset` command. For example:

```bash
> neuro admin remove-resource-preset company-cluster cpu-medium
```

