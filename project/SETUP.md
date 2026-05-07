## Step 0: Connect to Ares (from VSCode)
First, in **VSCode** install **Remote - SSH** by Microsoft, then:
* Open the **Command Palette** (`CMD+SHIFT+P`)
    * Search for `Remote-SSH: Connect to Host...`
    * Select SSH host e.g. `<PLGUSER_NAME>@ares.cyfronet.pl` 
    * (Optionally) Include below entry in `~/.ssh/config`:
        ```
        Host ares.cyfronet.pl
          AddKeysToAgent yes
          UseKeychain yes
          IdentityFile ~/.ssh/id_ed25519
          ServerAliveInterval 60
        ```

## Step 1: Environment Setup (One-time)
First, ensure your environment is installed on `${SCRATCH}` to avoid disk quota issues on your home directory.
```bash
# Log in to Ares and start a temporary session to build the environment
srun --time=1:00:00 --mem=8G --ntasks 1 --gres=gpu:1 --partition=plgrid-gpu-v100 --account=plglscclass26-gpu --pty /bin/bash

# Load and configure conda
module load miniconda3
conda config --add envs_dirs ${SCRATCH}/.conda/envs
conda config --add pkgs_dirs ${SCRATCH}/.conda/pkgs

# Create Python 3.12 environment
conda create -n ray-cluster python=3.12 jupyter ipykernel -c conda-forge -y
conda activate ray-cluster

# Install Ray (without PyTorch for now)
pip install "ray[default]" pandas numpy

# Register the kernel for Jupyter
python -m ipykernel install --user --name ray-cluster --display-name "Python 3.12 (Ray Cluster)"
exit
```

## Step 2: The Multi-Node Ray Launch Script
Create a file named `start-ray.sh` in your home or scratch directory.
```bash

```

## Step 3: Submit the Job
From integrated terminal in VSCode submit the job.
```bash
sbatch start-ray.sh
```

Check job status and id.
```bash
squeue --me
```

(Optionally) Cancel the job after the work is done.
```bash
scancel <JOB_ID>
```

## Step 4: Configure Jupyter Notebook
Open or create Jupyter Notebook with a single cell.
```python
import ray
import os

ray.shutdown()
ray.init(address='auto')

print("Cluster Resources:", ray.cluster_resources())
print("Nodes in cluster:", len(ray.nodes()))

ray.shutdown()
```

With notebook open click on `Select Kernel` > `Select Another Kernel...` > `Existing Jupyter Server...` and enter `http://<IP>:<PORT>/?token=...`.  
If the Jupyter Server started successfully get the URL from `jupyter-url-<JOB_ID>.txt` (e.g. `http://172.22.26.1:8423/?token=ray-course`).  
The kernel should be called `Python 3.12 (Ray Cluster)`.  
(Optionally) Check the logs in `slurm-<JOB_ID>.out`.

## Step 5: Execute Cell and Access Ray Dashboard
<!-- TODO: Make it clearer to reader. -->
If the kernel is connected, run the first cell and note the dashboard URL. In VSCode you can then open `Ports` > `Forward a Port` and paste dashboard's `<IP>:<PORT>`. After this you should access Ray's dashboard on localhost address displayed in VSCode.