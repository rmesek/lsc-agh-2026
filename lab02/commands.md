`srun --time=2:00:00 --mem=8G --ntasks 1 --gres=gpu:1 --partition=plgrid-gpu-v100 --account=plglscclass26-gpu --pty /bin/bash`

`cp $PLG_GROUPS_STORAGE/plgglscclass/lsc_lab02.ipynb $HOME` (once)

`module load miniconda3 sqlite`

`exec bash` (optional)

`# conda activate $PLG_GROUPS_STORAGE/plgglscclass/.conda/envs/lsclab` (broken, mismatched versions of Python and libs)

`conda config --add envs_dirs ${SCRATCH}/.conda/envs`

`conda config --add pkgs_dirs ${SCRATCH}/.conda/pkgs`

`conda create -n lsclab python=3.13 jupyter -c conda-forge` (once)

`conda activate lsclab`

`jupyter notebook --no-browser --port=8888 --ip=$(hostname)`

`squeue --me` (to check the job status)

`srun --jobid=<YOUR_JOBID> --overlap --pty /bin/bash` (to reconnect to the job)

`ssh ares` (In VSCode -> Remote-SSH: Connect to Host)

`$PLG_GROUPS_STORAGE/plgglscclass/yelp-dataset/`