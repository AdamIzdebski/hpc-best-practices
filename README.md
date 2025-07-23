# Accessing JUWELS Booster module

This overview contains the minimal list of steps a user needs to execute to run a job utilizing a GPU. 

## Getting started

#### 1. Apply for computing resources via the [application form](https://application.fz-juelich.de/Antragsserver/haicore/WEB/application/login.php?appkind=haicore).

#### 2. Set up an SSH key and upload it via [JUDOOR](https://judoor.fz-juelich.de/login).
> **Windows**: you'll need a workaround, described in user documentation's FAQ. 

**Tip:** Debug via `ssh -vvv`.

#### 3. Link your HOME directory to PROJECT space.  

Your HOME directory is limited in space. To avoid issues, create a personal directory in your PROJECT space and link it as `~/my-project`:
  ```bash
  mkdir -p /p/project1/my-project/"$USER"
  ln -s /p/project1/my-project/"$USER" ~/my-project
  ```
  Use `~/my-project` as your main working directory for code and important files.

For temporary files, use SCRATCH. Move and link cache directories:
  ```bash
  mkdir -p /p/scratch/my-project/"$USER"
  [ -d ~/.cache ] && mv ~/.cache /p/scratch/my-project/"$USER"/.cache
  mkdir -p /p/scratch/my-project/"$USER"/.cache
  ln -s /p/scratch/my-project/"$USER"/.cache ~/.cache
  ```
  > Example usage: use `/p/scratch/my-project/$USER` for temporary data. Use `~/my-project` to store final model weights. **Note:** Files in SCRATCH are deleted after 90 days of inactivity.

See the [File System](https://sdlaml.pages.jsc.fz-juelich.de/ai/guides/jsc_basics/#file-system) section below for more details on HOME, PROJECT, and SCRATCH.

#### 4. Set-up environment

> **Warning:** do not use conda :)

Refer to the [template](https://gitlab.jsc.fz-juelich.de/kesselheim1/sc_venv_template)

Can modify files to use `uv`.

#### 5. Set-up a remote connections for your IDE.

## Using the cluster

### Single GPU training
```bash
#!/bin/bash -x

#SBATCH --nodes=1            
#SBATCH --gres=gpu:1
#SBATCH --ntasks-per-node=1  
#SBATCH --cpus-per-task=96
#SBATCH --time=06:00:00
#SBATCH --partition=dc-gpu
#SBATCH --account=training2434
#SBATCH --output=%j.out
#SBATCH --error=%j.err
#SBATCH --reservation=training2434_day2

# To get number of cpu per task
export SRUN_CPUS_PER_TASK="$SLURM_CPUS_PER_TASK"
# activate env
source $HOME/course/$USER/sc_venv_template/activate.sh
# run script from above
time srun python3 gpu_training.py
```

### Multiple GPU training

```bash
#!/bin/bash
#SBATCH --account=training2338           # Account details
#SBATCH --nodes=1                        # Number of compute nodes required
#SBATCH --ntasks-per-node=4              # Number of tasks per node
#SBATCH --cpus-per-task=24               # CPU cores per task (all cpus available = 96 / num_gpus)
#SBATCH --gres=gpu:4                     # Use the 4 GPUs available
#SBATCH --output=output.%j               # File for standard output
#SBATCH --error=error.%j                 # File for standard error
#SBATCH --time=02:00:00                  # Maximum runtime
#SBATCH --partition=booster              # Specified machine partition

export CUDA_VISIBLE_DEVICES=0,1,2,3      # Very important to make the GPUs visible
export SRUN_CPUS_PER_TASK="$SLURM_CPUS_PER_TASK"  # Get number of cpu per task in the script

source sc_venv_template/activate.sh      # Command to activate the environment
time srun python3 ddp_training.py
```


---
## References
### JUWELS Resources
[User guides](https://sdlaml.pages.jsc.fz-juelich.de/ai/guides/getting_started/)

[User documentation](https://apps.fz-juelich.de/jsc/hps/juwels/index.html)

[Pytorch and DDP](https://gitlab.jsc.fz-juelich.de/sdlaml/pytorch-at-jsc)

[Bringing Deep Learning to JSC supercomputers](https://github.com/HelmholtzAI-FZJ/2024-08-course-Bringing-Deep-Learning-Workloads-to-JSC-supercomputers)


### Read more about GPUs and PyTorch

[Intro to SLURM](https://researchcomputing.princeton.edu/support/knowledge-base/slurm)

[PyTorch on Clusters](https://researchcomputing.princeton.edu/support/knowledge-base/pytorch)

[Multi-GPU Training](https://github.com/PrincetonUniversity/multi_gpu_training)

[Performance Tuning Guide to PyTorch](https://pytorch.org/tutorials/recipes/recipes/tuning_guide.html), [Slides](https://tigress-web.princeton.edu/~jdh4/PyTorchPerformanceTuningGuide_GTC2021.pdf)

[Reproducibility with PyTorch](https://pytorch.org/docs/stable/notes/randomness.html)

[Profiling](https://pytorch.org/tutorials/beginner/profiler.html)

[Distributed PyTorch](https://pytorch.org/tutorials/intermediate/dist_tuto.html)

[SWA](https://pytorch.org/blog/pytorch-1.6-now-includes-stochastic-weight-averaging/)

---

## FAQ

### Setting up the configuration file for DNS queries

Test your internet connection with 
```
curl -I https://linuxconfig.org
```
If problems occur check the DNS server with `ipconfig /all` and modify 

```
sudo chattr -i /etc/resolv.conf
sudo nano /etc/resolv.conf
```
Make the file imutable with
```
sudo chattr +i /etc/resolv.conf
```
 
Important - order of the servers do matter!
