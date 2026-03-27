<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({ tex2jax: {inlineMath: [['$', '$']]}, messageStyle: "none" });
</script>
# Lab Report: Batch jobs with SLURM

**Name:** Robert Mesek  
**Lab:** 3  
**Date:** March 26, 2026

---

## Assignment
(8p) Use an array job to render an animation from the Blender demo-files. Warning! Rendering on a cluster is fast, but queue times are, in some cases, unpredictable. Please account for queue times from minutes to, in some cases, hours!

Detailed instructions:
1. (6p) Submit a job which uses an animation from the Blender demo page, for example Repeat Zone – Flower (frame range 1-100). Not all demo scenes work in an environment without display!
    1. Include all parameters in the job script.
    2. Use the “plgrid” partition.
    3. Assuming you chose to use the linked demo scene: using a batch job configuration of 1 node with 4 CPUs per task can be a good choice, as rendering one frame may take up to 20 minutes. Requesting more CPUs will shorten render time, but queue times might increase. You can declare that each job will use up to 1GB of memory instead of the default 4GB per CPU – this might help with queue times.
        - (note) In a real-world scenario, choosing job configuration, accounting for application performance, and queue times is one of the main challenges.
    2. Blender is available through the modules system described above.
    3. Each job should render one frame which can be achieved with Blender in the following way: https://docs.blender.org/manual/en/latest/advanced/command_line/render.html
        - (hint) Ensure that the -f parameter is at the end of a command line! Blender tends to ignore it otherwise.
    6. Verify if the images were rendered and if the animation looks OK.
2) (1p) Provide a part of hpc-jobs-history command output with information about your jobs. What efficiency was achieved?
3) (1p) Estimate how many CPU-hours were used for the whole animation.
    - (hint) This can be estimated based on job parameters and/or read from the hpc-jobs-history output.

## Solution

### Job script
```bash
#!/bin/bash

#SBATCH --job-name=blender_render
#SBATCH --output=out/blender_render_%A_%a.out
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4
#SBATCH --mem-per-cpu=1G
#SBATCH --time=00:30:00
#SBATCH --partition=plgrid
#SBATCH --account=plglscclass26-cpu
#SBATCH --array=1-100

# Ensure output directories exist before rendering
mkdir -p out render

# Load Blender module
module load blender

# The Blender file
BLENDER_FILE="repeat_zone_flower_by_MiRA.blend"

# Execute render
blender -b "$BLENDER_FILE" -o "//render/frame_####" -f $SLURM_ARRAY_TASK_ID
```

### Download rendered frames
```bash
scp -r ares:/net/people/plgrid/plgrmesek/lsclab/render .
```
Example rendered frame:
![frame_0067](../render/frame_0067.png)

### Part of hpc-jobs-history output
| ID          | Name           | Partition | Nodes | Cores | Decl._mem | Mem._%_usage | Eff.  | CPU._used | Wall._Used | Wall._Req. | End_Time            |
|-------------|----------------|-----------|-------|-------|-----------|--------------|-------|-----------|------------|------------|---------------------|
| 19646956_4  | blender_render | plgrid    | 1     | 4     | 4.0GiB    | 5.5%         | 96.3% | 00:06:40  | 00:01:40   | 00:30:00   | 2026-03-20 16:16:19 |
| 19646956_13 | blender_render | plgrid    | 1     | 4     | 4.0GiB    | 5.5%         | 95.7% | 00:06:52  | 00:01:43   | 00:30:00   | 2026-03-20 16:16:22 |
| ...         | ...            | ...       | ...   | ...   | ...       | ...          | ...   | ...       | ...        | ...        | ...                 |
| 19646956_77 | blender_render | plgrid    | 1     | 4     | 4.0GiB    | 5.5%         | 98.9% | 01:03:16  | 00:15:49   | 00:30:00   | 2026-03-20 16:30:28 |

### CPU Efficiency
Found in `hpc-jobs-history`. It is calculated as $\frac{\text{CPU Time Used}}{\text{Wallclock Time} \times \text{CPUs Requested}}$

If efficiency is low, it means Blender isn't utilizing all 4 cores effectively for that specific scene.

### Estimated CPU-hours
$\text{Total CPU-Hours} = \text{Avg. Time per Job in Hours} \times \text{Number of Tasks} \times \text{CPUs per Task}$

Example: If a frame takes 15 mins (0.25h) on 4 CPUs: $0.25 \times 100 \times 4 = 100$ CPU-hours.
