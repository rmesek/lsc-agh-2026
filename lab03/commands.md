# Exercise

1. Create a “hello world” batch job which will: 
    a. Get information about CPU (hint: use lscpu or similar command) 
    b. Report how many cores are available for the job (there are multiple ways to do this) 

```bash
#!/bin/bash

# submit options
#SBATCH --job-name=hello_world
#SBATCH --output=hello_world_%j.out
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --time=00:00:10
#SBATCH --partition=plgrid
#SBATCH --account=plglscclass26-cpu

# initialization

# get CPU information
lscpu

# report number of cores available for the job
echo "Number of cores available (using nproc): $(nproc)"

exit
```

2. Create an array job in which each job prints out a n-th line of the same file, where n is the job’s 
task id (available through an environment variable SLURM_ARRAY_TASK_ID). Use file name 
formatting to create separate output files. 

```bash
#!/bin/bash

# submit options
#SBATCH --job-name=array_line_reader
#SBATCH --output=array_line_reader_%A_%a.out  # %A is Job ID, %a is Task ID
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --time=00:00:10
#SBATCH --partition=plgrid
#SBATCH --account=plglscclass26-cpu

# Define the range of lines to read (e.g., lines 1 to 4)
#SBATCH --array=1-4

# The file we want to read from
INPUT_FILE="input_data.txt"

# Get the n-th line where n is the SLURM_ARRAY_TASK_ID
# sed -n 'Np' prints only the Nth line
line_content=$(sed -n "${SLURM_ARRAY_TASK_ID}p" "$INPUT_FILE")

echo "Task ID: $SLURM_ARRAY_TASK_ID"
echo "Content of line $SLURM_ARRAY_TASK_ID: $line_content"

exit
```

# Assignment
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

## Submission
Submit the job script, job outputs and answers to questions

## Solution
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

### CPU Efficiency
Found in `hpc-jobs-history`. 
It is $\frac{\text{CPU Time Used}}{\text{Wallclock Time} \times \text{CPUs Requested}}$.

If efficiency is low (e.g., < 50%), it means Blender isn't utilizing all 4 cores effectively for that specific scene.

### Total CPU-Hours Estimate
$\text{Total CPU-Hours} = \text{Avg. Time per Job in Hours} \times \text{Number of Tasks} \times \text{CPUs per Task}$

Example: If a frame takes 15 mins (0.25h) on 4 CPUs: $0.25 \times 100 \times 4 = 100$ CPU-hours.

### Download rendered frames
```bash
scp -r ares:/net/people/plgrid/plgrmesek/lsclab/render .
```