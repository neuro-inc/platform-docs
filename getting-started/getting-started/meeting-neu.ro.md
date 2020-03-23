# Meeting Neu.ro

On **Neu.ro Core** level, one works with jobs, environments, and storage. To be more specific, one runs a job \(an execution unit\) in a given environment \(Docker container\) on a given preset \(a combination of CPU, GPU, and memory resources allocated for this job\) with several parts of storage \(block or object storage\) available \(attached\).

Let us show several examples.

Run a job on CPU which prints “Hello, World!” and shuts down:

```text
neuro run --preset cpu-small --name test ubuntu echo Hello, World!
```

Upon execution of this command you’ll see an output like this:

```text
Job ID: job-2b743322-f53a-4211-be4e-5d493e6cc770 Status: pending
Name: test
Http URL: https://test--johndoe.jobs.neuro-ai-public.org.neu.ro
Shortcuts:
  neuro status test     # check job status
  neuro logs test       # monitor job stdout
  neuro top test        # display real-time job telemetry
  neuro exec test bash  # execute bash shell to the job
  neuro kill test       # kill job
Status: pending Creating
Status: pending Scheduling
StatStatus: pending ContainerCreating
Status: succeeded
Terminal is attached to the remote job, so you receive the job's output.
Use 'Ctrl-C' to detach (it will NOT terminate the job), or restart the
job with `--detach` option.                
Hello, World!
```

Run a job in Neu.ro default environment \(`neuromation/base`\) on GPU which prints checks if CUDA is available in this environment:

```text
neuro run -preset gpu-small --name test neuromation/base python -c "import torch; print(torch.cuda.is_available());"
```

Check the presets you can use:

```text
neuro config show
```

Create a directory `demo` in your platform storage root:

```text
neuro storage mkdir storage:demo
```

Run a job which mounts `demo` directory on storage to `/demo` directory in the job container and creates a file in it:

```text
neuro run --preset cpu-small --name test --volume storage:demo:/demo:rw ubuntu "echo Hello >> /demo/hello.txt"
```

Check that the file is on storage:

```text
neuro ls storage:demo
```

