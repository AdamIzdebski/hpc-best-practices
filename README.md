# HPC Best Practices
Defines the workflow for accessing HPC. [Chatbot](https://teams.microsoft.com/l/app/f6405520-7907-4464-8f6e-9889e2fb7d8f?templateInstanceId=e249fd29-3a61-4e73-baae-65341c449294&environment=Default-e229e493-1bf2-40a7-9b84-85f6c23aeed8)

## How to get access?

## How to set things up?

### Cluster

#### Miniconda

#### Others

### PC

#### WSL

#### VSCode

#### MobaXterm

#### FileZilla

#### Putty

## How to use the cluster?

### File System

```
/home/<group>/<username>  # Personal, not computationally efficient
/lustre/groups/<group>  # Can be accessed within the institute
/lustre/groups/shared  # Can be accessed by everyone
/localscratch  # tmp, not optimized for parralel processing, good to store logs, computation specific files etc. 
/tmp # tmp, not optimized for parralel processing
```

### File Transfer

#### RSync

### Submiting Jobs

```bash
#!/bin/bash

#SBATCH -J id
#SBATCH -o stodut_file
#SBATCH -e stderr_file
#SBATCH -p interactive_gpu_p
#SBATCH --qos interactive_gpu
#SBATCH --gres=gpu:1 <- how many GPUs you request, use 1
#SBATCH -t 02:00:00
#SBATCH -c 6 <- don't use more than 50% (25% for icb-gpusrv0[1-2]) 
          # of GPU queue node cores unless you request entire node
#SBATCH --mem=15G <- dont use more than 50% (25% for icb-gpusrv0[1-2]) 
          # of GPU queue node memory unless you request entire node
#SBATCH --nice=10000 <- manual priority (should always be set to 10000), 
          # always include this line

source $HOME/.bashrc
# do stuff
conda activate my-env
python your_script.py
```

### Submiting Job Arrays

```bash
#!/bin/bash
#SBATCH --job-name=my_job_array_script
#SBATCH --output=array_%A_%a.out
#SBATCH --error=array_%A_%a.err
#SBATCH --array=1-10:2  # Run tasks 1, 3, 5, 7, 9

#SBATCH -p interactive_cpu_p
#SBATCH --qos interactive_cpu

#SBATCH 
#SBATCH --nodes=1
#SBATCH --cpus-per-task=4
#SBATCH --mem=8G
#SBATCH --time=1:00:00

#SBATCH --nice=10000

# This script will run 5 tasks in a job array, skipping alternate tasks

# Define an array of input files
input_files=("input1.txt" "input2.txt" "input3.txt" "input4.txt" "input5.txt" 
s"input6.txt" "input7.txt" "input8.txt" "input9.txt" "input10.txt")

# Calculate the index of the current task in the array
task_index=$((SLURM_ARRAY_TASK_ID - 1))

# Check if the current task is within the range of input files
if [ "$task_index" -lt "${#input_files[@]}" ]; then
    # Extract the input file for the current task
    current_input=${input_files[$task_index]}

    # Print task information
    echo "Running task $SLURM_ARRAY_TASK_ID with input file: $current_input"

    # Replace the following line with the actual command to run your task 
    # using the current input file
    # For example: ./my_application $current_input

    echo "Task $SLURM_ARRAY_TASK_ID completed"
else
    echo "No task to run for task index $task_index"
fi
```

#### Submiting Jobs with VSCode

### Run
```
sbatch my_job_script.sh
```
or
```
sbatch --array=1-10 my_job_array_script.sh


Monitor
squeue -u username
scontrol show job <job_id>

Cancel

scancel <job_id>

```

