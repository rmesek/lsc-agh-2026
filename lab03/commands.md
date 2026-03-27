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

### `hpc-jobs-history` output
```
ID                                  Name   Partition   Nodes   Cores   Decl._mem   Mem._%_usage    Eff.   CPU._used   Wall._Used   Wall._Req.              End_Time
--                                  ----   ---------   -----   -----   ---------   ------------    ----   ---------   ----------   ----------              --------
19646956_4                blender_render      plgrid       1       4      4.0GiB           5.5%   96.3%    00:06:40     00:01:40     00:30:00   2026-03-20 16:16:19
19646956_13               blender_render      plgrid       1       4      4.0GiB           5.5%   95.7%    00:06:52     00:01:43     00:30:00   2026-03-20 16:16:22
19646956_1                blender_render      plgrid       1       4      4.0GiB           5.5%   95.1%    00:07:00     00:01:45     00:30:00   2026-03-20 16:16:24
19646956_6                blender_render      plgrid       1       4      4.0GiB           5.7%   95.8%    00:07:04     00:01:46     00:30:00   2026-03-20 16:16:25
19646956_5                blender_render      plgrid       1       4      4.0GiB           5.4%   95.7%    00:07:08     00:01:47     00:30:00   2026-03-20 16:16:26
19646956_7                blender_render      plgrid       1       4      4.0GiB           5.5%   95.9%    00:07:08     00:01:47     00:30:00   2026-03-20 16:16:26
19646956_2                blender_render      plgrid       1       4      4.0GiB           5.5%   95.1%    00:07:12     00:01:48     00:30:00   2026-03-20 16:16:27
19646956_10               blender_render      plgrid       1       4      4.0GiB           5.5%   95.4%    00:07:12     00:01:48     00:30:00   2026-03-20 16:16:27
19646956_8                blender_render      plgrid       1       4      4.0GiB           5.6%   95.4%    00:07:20     00:01:50     00:30:00   2026-03-20 16:16:29
19646956_16               blender_render      plgrid       1       4      4.0GiB           5.6%   94.4%    00:07:20     00:01:50     00:30:00   2026-03-20 16:16:29
19646956_12               blender_render      plgrid       1       4      4.0GiB           5.6%   96.0%    00:07:24     00:01:51     00:30:00   2026-03-20 16:16:30
19646956_9                blender_render      plgrid       1       4      4.0GiB           5.6%   95.7%    00:07:32     00:01:53     00:30:00   2026-03-20 16:16:32
19646956_11               blender_render      plgrid       1       4      4.0GiB           5.6%   95.4%    00:07:32     00:01:53     00:30:00   2026-03-20 16:16:32
19646956_14               blender_render      plgrid       1       4      4.0GiB           5.5%   96.3%    00:07:32     00:01:53     00:30:00   2026-03-20 16:16:32
19646956_17               blender_render      plgrid       1       4      4.0GiB           5.5%   94.7%    00:07:36     00:01:54     00:30:00   2026-03-20 16:16:33
19646956_18               blender_render      plgrid       1       4      4.0GiB           5.5%   94.7%    00:07:40     00:01:55     00:30:00   2026-03-20 16:16:34
19646956_15               blender_render      plgrid       1       4      4.0GiB           5.6%   94.5%    00:07:44     00:01:56     00:30:00   2026-03-20 16:16:35
19646956_3                blender_render      plgrid       1       4      4.0GiB           5.6%   96.0%    00:08:04     00:02:01     00:30:00   2026-03-20 16:16:40
19646956_20               blender_render      plgrid       1       4      4.0GiB           5.5%   95.1%    00:08:20     00:02:05     00:30:00   2026-03-20 16:16:44
19646956_19               blender_render      plgrid       1       4      4.0GiB           5.6%   95.0%    00:08:36     00:02:09     00:30:00   2026-03-20 16:16:48
19646956_21               blender_render      plgrid       1       4      4.0GiB           5.6%   96.2%    00:09:12     00:02:18     00:30:00   2026-03-20 16:16:57
19646956_22               blender_render      plgrid       1       4      4.0GiB           5.5%   96.4%    00:09:32     00:02:23     00:30:00   2026-03-20 16:17:02
19646956_23               blender_render      plgrid       1       4      4.0GiB           5.5%   96.4%    00:09:48     00:02:27     00:30:00   2026-03-20 16:17:06
19646956_24               blender_render      plgrid       1       4      4.0GiB           5.5%   96.3%    00:10:36     00:02:39     00:30:00   2026-03-20 16:17:18
19646956_25               blender_render      plgrid       1       4      4.0GiB           5.5%   96.4%    00:11:08     00:02:47     00:30:00   2026-03-20 16:17:26
19646956_26               blender_render      plgrid       1       4      4.0GiB           5.5%   97.0%    00:11:36     00:02:54     00:30:00   2026-03-20 16:17:33
19646956_27               blender_render      plgrid       1       4      4.0GiB           5.6%   97.8%    00:12:16     00:03:04     00:30:00   2026-03-20 16:17:43
19646956_28               blender_render      plgrid       1       4      4.0GiB           5.6%   97.7%    00:13:16     00:03:19     00:30:00   2026-03-20 16:17:58
19646956_29               blender_render      plgrid       1       4      4.0GiB           5.5%   97.6%    00:14:32     00:03:38     00:30:00   2026-03-20 16:18:17
19646956_31               blender_render      plgrid       1       4      4.0GiB           8.0%   97.4%    00:16:04     00:04:01     00:30:00   2026-03-20 16:18:40
19646956_32               blender_render      plgrid       1       4      4.0GiB           5.5%   97.3%    00:16:12     00:04:03     00:30:00   2026-03-20 16:18:42
19646956_30               blender_render      plgrid       1       4      4.0GiB           5.5%   97.8%    00:16:24     00:04:06     00:30:00   2026-03-20 16:18:45
19646956_33               blender_render      plgrid       1       4      4.0GiB           5.5%   97.6%    00:17:32     00:04:23     00:30:00   2026-03-20 16:19:02
19646956_35               blender_render      plgrid       1       4      4.0GiB          21.8%   98.4%    00:20:08     00:05:02     00:30:00   2026-03-20 16:19:41
19646956_36               blender_render      plgrid       1       4      4.0GiB           5.6%   98.0%    00:21:16     00:05:19     00:30:00   2026-03-20 16:19:58
19646956_34               blender_render      plgrid       1       4      4.0GiB           5.6%   98.3%    00:21:52     00:05:28     00:30:00   2026-03-20 16:20:07
19646956_37               blender_render      plgrid       1       4      4.0GiB           5.6%   98.1%    00:22:48     00:05:42     00:30:00   2026-03-20 16:20:21
19646956_38               blender_render      plgrid       1       4      4.0GiB           5.5%   98.1%    00:24:48     00:06:12     00:30:00   2026-03-20 16:20:51
19646956_39               blender_render      plgrid       1       4      4.0GiB           5.6%   98.2%    00:26:20     00:06:35     00:30:00   2026-03-20 16:21:14
19646956_40               blender_render      plgrid       1       4      4.0GiB           5.5%   98.3%    00:29:36     00:07:24     00:30:00   2026-03-20 16:22:03
19646956_41               blender_render      plgrid       1       4      4.0GiB           5.5%   98.5%    00:30:52     00:07:43     00:30:00   2026-03-20 16:22:22
19646956_42               blender_render      plgrid       1       4      4.0GiB           5.5%   98.6%    00:33:20     00:08:20     00:30:00   2026-03-20 16:22:59
19646956_44               blender_render      plgrid       1       4      4.0GiB           5.5%   98.8%    00:33:24     00:08:21     00:30:00   2026-03-20 16:23:00
19646956_43               blender_render      plgrid       1       4      4.0GiB           5.5%   98.5%    00:34:40     00:08:40     00:30:00   2026-03-20 16:23:19
19646956_45               blender_render      plgrid       1       4      4.0GiB           5.5%   98.7%    00:38:40     00:09:40     00:30:00   2026-03-20 16:24:19
19646956_46               blender_render      plgrid       1       4      4.0GiB           5.5%   98.8%    00:41:12     00:10:18     00:30:00   2026-03-20 16:24:57
19646956_47               blender_render      plgrid       1       4      4.0GiB           5.6%   98.7%    00:44:04     00:11:01     00:30:00   2026-03-20 16:25:40
19646956_48               blender_render      plgrid       1       4      4.0GiB           5.6%   98.8%    00:45:48     00:11:27     00:30:00   2026-03-20 16:26:06
19646956_49               blender_render      plgrid       1       4      4.0GiB           5.5%   98.8%    00:47:52     00:11:58     00:30:00   2026-03-20 16:26:37
19646956_50               blender_render      plgrid       1       4      4.0GiB           5.5%   98.9%    00:48:12     00:12:03     00:30:00   2026-03-20 16:26:42
19646956_55               blender_render      plgrid       1       4      4.0GiB           5.5%   98.9%    00:49:08     00:12:17     00:30:00   2026-03-20 16:26:56
19646956_51               blender_render      plgrid       1       4      4.0GiB           5.6%   98.9%    00:49:24     00:12:21     00:30:00   2026-03-20 16:27:00
19646956_53               blender_render      plgrid       1       4      4.0GiB           5.5%   98.9%    00:49:36     00:12:24     00:30:00   2026-03-20 16:27:03
19646956_61               blender_render      plgrid       1       4      4.0GiB           6.0%   98.8%    00:49:56     00:12:29     00:30:00   2026-03-20 16:27:08
19646956_99               blender_render      plgrid       1       4      4.0GiB           6.0%   99.0%    00:50:32     00:12:38     00:30:00   2026-03-20 16:27:17
19646956_90               blender_render      plgrid       1       4      4.0GiB           5.6%   98.9%    00:51:16     00:12:49     00:30:00   2026-03-20 16:27:28
19646956_63               blender_render      plgrid       1       4      4.0GiB           6.0%   98.8%    00:51:20     00:12:50     00:30:00   2026-03-20 16:27:29
19646956_52               blender_render      plgrid       1       4      4.0GiB           5.6%   98.7%    00:51:36     00:12:54     00:30:00   2026-03-20 16:27:33
19646956_83               blender_render      plgrid       1       4      4.0GiB           6.0%   99.0%    00:51:44     00:12:56     00:30:00   2026-03-20 16:27:35
19646956_98               blender_render      plgrid       1       4      4.0GiB           6.1%   99.1%    00:51:56     00:12:59     00:30:00   2026-03-20 16:27:38
19646956_54               blender_render      plgrid       1       4      4.0GiB           5.6%   98.9%    00:52:16     00:13:04     00:30:00   2026-03-20 16:27:43
19646956_86               blender_render      plgrid       1       4      4.0GiB           5.5%   99.0%    00:52:40     00:13:10     00:30:00   2026-03-20 16:27:49
19646956_59               blender_render      plgrid       1       4      4.0GiB           5.5%   99.0%    00:52:56     00:13:14     00:30:00   2026-03-20 16:27:53
19646956_93               blender_render      plgrid       1       4      4.0GiB           5.9%   99.0%    00:53:20     00:13:20     00:30:00   2026-03-20 16:27:59
19646956_84               blender_render      plgrid       1       4      4.0GiB           5.6%   99.0%    00:53:48     00:13:27     00:30:00   2026-03-20 16:28:06
19646956_56               blender_render      plgrid       1       4      4.0GiB           5.5%   99.0%    00:54:00     00:13:30     00:30:00   2026-03-20 16:28:09
19646956_70               blender_render      plgrid       1       4      4.0GiB           5.9%   99.0%    00:54:16     00:13:34     00:30:00   2026-03-20 16:28:13
19646956_89               blender_render      plgrid       1       4      4.0GiB           5.6%   99.0%    00:54:20     00:13:35     00:30:00   2026-03-20 16:28:14
19646956_57               blender_render      plgrid       1       4      4.0GiB           5.6%   99.0%    00:54:56     00:13:44     00:30:00   2026-03-20 16:28:23
19646956_60               blender_render      plgrid       1       4      4.0GiB           5.9%   98.9%    00:55:00     00:13:45     00:30:00   2026-03-20 16:28:24
19646956_88               blender_render      plgrid       1       4      4.0GiB           6.0%   99.0%    00:55:00     00:13:45     00:30:00   2026-03-20 16:28:24
19646956_58               blender_render      plgrid       1       4      4.0GiB          31.8%   98.9%    00:55:36     00:13:54     00:30:00   2026-03-20 16:28:33
19646956_62               blender_render      plgrid       1       4      4.0GiB           5.6%   99.0%    00:55:36     00:13:54     00:30:00   2026-03-20 16:28:33
19646956_65               blender_render      plgrid       1       4      4.0GiB           5.9%   98.9%    00:55:48     00:13:57     00:30:00   2026-03-20 16:28:36
19646956_68               blender_render      plgrid       1       4      4.0GiB           6.0%   99.0%    00:55:52     00:13:58     00:30:00   2026-03-20 16:28:37
19646956_80               blender_render      plgrid       1       4      4.0GiB           5.9%   99.3%    00:55:56     00:13:59     00:30:00   2026-03-20 16:28:38
19646956_64               blender_render      plgrid       1       4      4.0GiB          25.0%   98.9%    00:56:08     00:14:02     00:30:00   2026-03-20 16:28:41
19646956_81               blender_render      plgrid       1       4      4.0GiB          21.4%   99.2%    00:56:08     00:14:02     00:30:00   2026-03-20 16:28:41
19646956_67               blender_render      plgrid       1       4      4.0GiB           6.0%   99.1%    00:56:12     00:14:03     00:30:00   2026-03-20 16:28:42
19646956_73               blender_render      plgrid       1       4      4.0GiB           5.6%   98.9%    00:56:12     00:14:03     00:30:00   2026-03-20 16:28:42
19646956_82               blender_render      plgrid       1       4      4.0GiB           6.0%   99.1%    00:56:12     00:14:03     00:30:00   2026-03-20 16:28:42
19646956_69               blender_render      plgrid       1       4      4.0GiB           6.0%   99.0%    00:56:20     00:14:05     00:30:00   2026-03-20 16:28:44
19646956_66               blender_render      plgrid       1       4      4.0GiB           5.9%   99.0%    00:56:24     00:14:06     00:30:00   2026-03-20 16:28:45
19646956_87               blender_render      plgrid       1       4      4.0GiB           5.5%   98.4%    00:56:36     00:14:09     00:30:00   2026-03-20 16:28:48
19646956_95               blender_render      plgrid       1       4      4.0GiB           5.5%   99.1%    00:56:36     00:14:09     00:30:00   2026-03-20 16:28:48
19646956_71               blender_render      plgrid       1       4      4.0GiB           6.0%   99.0%    00:56:40     00:14:10     00:30:00   2026-03-20 16:28:49
19646956_100              blender_render      plgrid       1       4      4.0GiB           6.0%   99.0%    00:56:40     00:14:10     00:30:00   2026-03-20 16:28:49
19646956_72               blender_render      plgrid       1       4      4.0GiB           6.1%   99.1%    00:56:44     00:14:11     00:30:00   2026-03-20 16:28:50
19646956_79               blender_render      plgrid       1       4      4.0GiB           5.6%   99.1%    00:56:44     00:14:11     00:30:00   2026-03-20 16:28:50
19646956_94               blender_render      plgrid       1       4      4.0GiB           5.5%   99.1%    00:56:44     00:14:11     00:30:00   2026-03-20 16:28:50
19646956_96               blender_render      plgrid       1       4      4.0GiB           6.1%   99.1%    00:56:52     00:14:13     00:30:00   2026-03-20 16:28:52
19646956_92               blender_render      plgrid       1       4      4.0GiB           5.5%   99.0%    00:57:20     00:14:20     00:30:00   2026-03-20 16:28:59
19646956_85               blender_render      plgrid       1       4      4.0GiB           5.9%   99.1%    00:57:24     00:14:21     00:30:00   2026-03-20 16:29:00
19646956_91               blender_render      plgrid       1       4      4.0GiB           5.9%   99.0%    00:57:24     00:14:21     00:30:00   2026-03-20 16:29:00
19646956_97               blender_render      plgrid       1       4      4.0GiB           5.9%   99.0%    00:57:24     00:14:21     00:30:00   2026-03-20 16:29:00
19646956_76               blender_render      plgrid       1       4      4.0GiB           5.5%   98.8%    00:57:36     00:14:24     00:30:00   2026-03-20 16:29:03
19646956_78               blender_render      plgrid       1       4      4.0GiB           5.6%   99.1%    00:57:40     00:14:25     00:30:00   2026-03-20 16:29:04
19646956_75               blender_render      plgrid       1       4      4.0GiB           5.5%   98.9%    00:57:56     00:14:29     00:30:00   2026-03-20 16:29:08
19646956_74               blender_render      plgrid       1       4      4.0GiB           5.5%   98.8%    00:58:00     00:14:30     00:30:00   2026-03-20 16:29:09
19646956_77               blender_render      plgrid       1       4      4.0GiB           5.5%   98.9%    01:03:16     00:15:49     00:30:00   2026-03-20 16:30:28
```