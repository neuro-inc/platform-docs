# Clusters (Resources)

Clusters are collections of resources - compute, storage, and the registry. When you sign-up on Neu.ro, it provides you access to our public cluster _**default**_. You can create new clusters, but this requires additional access. For more information, contact [team@neu.ro](mailto:team@neu.ro).

You can use one cluster at a time and switch between clusters. This tutorial will helps you understand how you can manage resources.

### **How can I view information about my current cluster?**

Each cluster comes with its own storage, registry, and resource presets.&#x20;

You can view information about your current cluster both in the CLI and in the Web UI:&#x20;

{% tabs %}
{% tab title="CLI" %}
You can use the `neuro config show` command to view the current cluster's name, API URL, docker registry URL, and the list of presets available for this cluster.

```
$ neuro config show
User Configuration:                                       
 User Name            john-doe                      
 Current Cluster      default                             
 Current Org          <no-org>                            
 Credits Quota        unlimited                           
 Jobs Quota           unlimited                           
 API URL              https://staging.neu.ro/api/v1       
 Docker Registry URL  https://registry.default.org.neu.ro 
Resource Presets:                                                                                                          
 Name               #CPU   Memory   Round Robin   Preemptible Node   GPU                     Jobs Avail   Credits per hour 

 cpu-small             1     4.0G                                                                  55   1                
 cpu-medium            3    11.0G                                                                  15   2                
 cpu-large             7    28.0G                                                                   7   4                
 gpu-k80-small         3    57.0G                                  1 x nvidia-tesla-k80            30   10               
 gpu-k80-small-p       3    57.0G                                  1 x nvidia-tesla-k80            10   1                
 gpu-v100-small        7    57.0G                                  1 x nvidia-tesla-v100           10   32               
 gpu-v100-small-p      7    57.0G                                  1 x nvidia-tesla-v100           10   4                
 gpu-small             3    57.0G                                  1 x nvidia-tesla-k80            30   10               
 gpu-small-p           3    57.0G                                  1 x nvidia-tesla-k80            10   1                
 gpu-v100-large       63   480.0G                                  8 x nvidia-tesla-v100            4   256              
 gpu-v100-large-p     63   480.0G                                  8 x nvidia-tesla-v100            4   32               
 gpu-v100-2           15   115.0M                                  2 x nvidia-tesla-v100           16   64               
 gpu-v100-4           31   235.0M                                  4 x nvidia-tesla-v100            8   128              
 gpu-v100-8           63   480.0M                                  8 x nvidia-tesla-v100            4   256
```
{% endtab %}

{% tab title="Web UI" %}
You can view the current cluster's name, the list of available presets, as well as your role on this cluster on the **Information** tab:

![](<../../.gitbook/assets/image (5).png>)
{% endtab %}
{% endtabs %}

### **How can I view all my clusters and switch my current cluster?**

You can view the list of available clusters and switch between them both in the CLI and in the Web UI:

{% tabs %}
{% tab title="CLI" %}
You can use the **** `neuro config get-clusters` command to view the list of clusters that you have access to and information about them.

```
$ neuro config get-clusters
Fetch the list of available clusters...
Available clusters:
* Name: neuro-public
  Presets:
    Name         #CPU  Memory  Preemptible  GPU
    cpu-small       1    2.0G       No
    cpu-large       7   28.0G       No
    gpu-small       3   57.0G       No      1 x nvidia-tesla-k80
    gpu-small-p     3   57.0G      Yes      1 x nvidia-tesla-k80
    gpu-large       7   57.0G       No      1 x nvidia-tesla-v100
    gpu-large-p     7   57.0G      Yes      1 x nvidia-tesla-v100
  Name: onprem
  Presets:
    Name       #CPU  Memory  Preemptible  GPU                          
    cpu-small     1    4.0G       No                                   
    cpu-large     4   10.0G       No                                   
    gpu-small    23   60.0G       No      1 x nvidia-geforce-rtx-2080ti
    gpu-large    46  120.0G       No      2 x nvidia-geforce-rtx-2080ti
```

You can switch between clusters by using the `neuro config switch-cluster` command. When you run the command, you are prompted to enter the name of the cluster you want to switch to. The current cluster is switched after you provide the name.

```
$ neuro config switch-cluster
Fetch the list of available clusters...
Available clusters:
* Name: default
  Resource Presets:
   Name               #CPU   Memory   Preemptible   Preemptible Node   GPU
  ───────────────────────────────────────────────────────────────────────────────────────────
   cpu-small             1     4.0G        ×               ×
   cpu-medium            3    11.0G        ×               ×
   cpu-large             7    26.0G        ×               ×
   gpu-k80-small         5    48.0G        ×               ×           1 x nvidia-tesla-k80
   gpu-k80-small-p     5.0    48.0G        √               √           1 x nvidia-tesla-k80
   gpu-v100-small        5    95.0G        ×               ×           1 x nvidia-tesla-v100
   gpu-v100-small-p    5.0    95.0G        √               √           1 x nvidia-tesla-v100
  Name: onprem-poc
  Resource Presets:
   Name         #CPU   Memory   Preemptible   Preemptible Node   GPU
  ─────────────────────────────────────────────────────────────────────────────────────────────
   cpu-nano      0.2     1.0G        ×               ×
   cpu-small       1     4.0G        ×               ×
   cpu-large       4    10.0G        ×               ×
   gpu-small      23    60.0G        ×               ×           1 x nvidia-geforce-rtx-2080ti
   gpu-large      47   120.0G        ×               ×           2 x nvidia-geforce-rtx-2080ti
   gpu-1x3090   23.0    60.0G        ×               ×           1 x nvidia-geforce-rtx-3090
   gpu-2x3090   47.0   120.0G        ×               ×           2 x nvidia-geforce-rtx-3090
Select cluster to switch [default]: onprem-poc
The current cluster is onprem-poc
```
{% endtab %}

{% tab title="Web UI" %}
Use the **Cluster** drop-down list to view available clusters and switch between them:

![](<../../.gitbook/assets/image (210).png>)
{% endtab %}
{% endtabs %}

### **How can I view my computation quota?**

You are assigned a computation quota of 100 credits when you sign up to Neu.ro. This quota is used whenever you run a job.&#x20;

You can view the remaining computation quota on your Web UI dashboard.

The `neuro config show` command also shows the amount of computation quota left as part of its output.

### How can I request for more computation quota?

You can top up your computation quota by clicking the **TOP UP NOW** button on the Web UI dashboard.

You can also write to [team@neu.ro](mailto:team@neu.ro) to learn about latest discounts and promotions, and then request the top up.

### How can I create a new cluster?&#x20;

Neu.ro lets you create new clusters for better management and orchestration of resources. However, you must remember that the new cluster would require more computation quota. Before you create a cluster, you must decide on the presets, storage, and registry that you want to assign for this cluster. You can create a cluster by writing to us at  [team@neu.ro](mailto:team@neu.ro).
