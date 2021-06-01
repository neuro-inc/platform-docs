# Reports

The **Information** tab contains widgets for accessing cluster-wide data reports which can help you get a clearer picture of how your cluster is operating and find any issues if there are any.

The **reports** functionality is based on [Grafana](https://grafana.com), an open-source analytics and interactive visualization web service. Feel free to refer to their [documentation](https://grafana.com/docs/grafana/latest/?utm_source=grafana_footer) to learn more about Grafana's functionality.

## Types of reports

There are three types of cluster-wide reports you can access:

* Cluster nodes monitor
* Cluster jobs monitor
* Cluster prices monitor

Let's look at each one of them more closely.

### Cluster nodes monitor

This report provides in-depth information about the activity of the cluster's nodes, including their uptime, CPU and RAM usage, system processes, network traffic, and much more.

You can access it by clicking **CLUSTER NODES MONITOR** on the **Information** tab:

![](../../.gitbook/assets/image%20%28147%29.png)

The **Nodes** section provides a quick overview of the cluster's nodes: 

![](../../.gitbook/assets/image%20%28160%29.png)

Clicking a node's name in the **Nodes** section will make it active, allowing you to see detailed information about it in the sections below:

![](../../.gitbook/assets/image%20%28163%29.png)

You can expand any specific section by clicking it's name:

![](../../.gitbook/assets/image%20%28170%29.png)

### Cluster jobs monitor

This report provides a thorough look at the cluster's jobs and their activity, including the jobs' owners, IDs, start times, CPU and memory usage, and more. 

You can access it by clicking **CLUSTER JOBS MONITOR** in the **Information** tab:

![](../../.gitbook/assets/image%20%28152%29.png)

The **Running Jobs** widget gives an overview of all currently running jobs, including the information about resource consumption:

![](../../.gitbook/assets/image%20%28164%29.png)

You can view the amount of jobs per user in the **Number of Jobs** and **Total Number of Jobs** widgets:

![](../../.gitbook/assets/image%20%28155%29.png)

Additional sections with information can be expanded by clicking their names:

![](../../.gitbook/assets/image%20%28145%29.png)

Clicking a job's name will bring you to a page with detailed information about this specific job:

![](../../.gitbook/assets/image%20%28146%29.png)

### Cluster prices monitor

This report provides information related to cluster pricing, such as node costs, job credit use, job duration, and more.

You can access this report by clicking **CLUSTER PRICES MONITOR** in the **Information** tab:

![](../../.gitbook/assets/image%20%28158%29.png)

The uppermost section displays total job credits used in the selected time range, job duration, total cost of available nodes, and the uptime of these nodes. The **Jobs** widget shows a rundown of the jobs - their IDs, owners, presets, start times, and credit costs:

![](../../.gitbook/assets/image%20%28151%29.png)

The lowermost widgets show more information about users and nodes:

![](../../.gitbook/assets/image%20%28161%29.png)

## Report time ranges and refresh time

Each report has a default time range for which it's generated, based on the report's type. You can specify any other range in the **Time range** drop-down tab:

![](../../.gitbook/assets/image%20%28153%29.png)

You can also choose how often the report refreshes in the **Refresh rate** drop-down list:

![](../../.gitbook/assets/image%20%28156%29.png)

