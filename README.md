# Want to run your model on MHGU HPC cluster?

Quick troubleshooting? [Talk with the Chatbot!](https://teams.microsoft.com/l/app/f6405520-7907-4464-8f6e-9889e2fb7d8f?templateInstanceId=e249fd29-3a61-4e73-baae-65341c449294&environment=Default-e229e493-1bf2-40a7-9b84-85f6c23aeed8)

## Personal Workflow

1. Open a terminal inside WSL 1 with an Ubuntu distribution on my local machine and login to the cluster with
   ```
   ssh <username>@hpc-build01.scidom.de
   ```
   This allows me to manage conda and git.
2. Manage files with FileZilla
   
   This is an easy way to send files to the cluster. 
  
3. Open my project with VSCode

   The project is stored on the server and VSCode is connected to the Host (which is the cluster). This is easy code edition. 

4. Run scripts from the terminal (WSL or VSCode)

Have fun :) 

### Running your script

You can run the exemplary script with 
```
sbatch job.slurm
```

Running the job will create `output-torch-test.txt` file, which recognizes what is the latest version of the CUDA driver compatible with your GPU and verifies torch installation. You are welcome :) 

To monitor the queue run
```
squeue -u username
```
and cancel jobs using
```
scancel <job_id>
```

#### Running your script on an A100 GPU

E.g. the highest supported CUDA version for NVIDIA TESLA A100 is 11.6, which requires the following packages
```
pytorch==1.13.1
torchvision==0.14.1
torchaudio==0.13.1
pytorch-cuda=11.6
```

## Read more about GPU Computing, whether you need to run your script on multiple GPUs and PyTorch optimization

[Intro to SLURM](https://researchcomputing.princeton.edu/support/knowledge-base/slurm)

[PyTorch on Clusters](https://researchcomputing.princeton.edu/support/knowledge-base/pytorch)

[Multi-GPU Training](https://github.com/PrincetonUniversity/multi_gpu_training)

[Performance Tuning Guide to PyTorch](https://pytorch.org/tutorials/recipes/recipes/tuning_guide.html), [Slides](https://tigress-web.princeton.edu/~jdh4/PyTorchPerformanceTuningGuide_GTC2021.pdf)

[Reproducibility with PyTorch](https://pytorch.org/docs/stable/notes/randomness.html)

[Profiling](https://pytorch.org/tutorials/beginner/profiler.html)

## Other Info

### File System

```
/home/<group>/<username>  # Personal, not computationally efficient
/lustre/groups/<group>  # Can be accessed within the institute
/lustre/groups/shared  # Can be accessed by everyone
/localscratch  # tmp, not optimized for parralel processing, good to store logs, computation specific files etc. 
/tmp # tmp, not optimized for parralel processing
```

## How to Get Started?

### 1. Get Access

In order to get access to HMGU HPC, follow the [steps](https://hmgu.sharepoint.com/sites/hpc-wiki/SitePages/HPC-Onboarding.aspx?xsdata=MDV8MDJ8YWRhbS5pemRlYnNraUBoZWxtaG9sdHotbXVuaWNoLmRlfGI2NTEwY2I2NDcxMDQ2Y2I4NWYxMDhkYzVmYTM4MDEzfGUyMjllNDkzMWJmMjQwYTc5Yjg0ODVmNmMyM2FlZWQ4fDB8MHw2Mzg0OTA0MDMwMjQxNDUyMzh8VW5rbm93bnxUV0ZwYkdac2IzZDhleUpXSWpvaU1DNHdMakF3TURBaUxDSlFJam9pVjJsdU16SWlMQ0pCVGlJNklrMWhhV3dpTENKWFZDSTZNbjA9fDB8fHw%3D&sdata=YjhFWG51ZmY3VHlsWFJzdnBGcEdiUWhDMkxjT3NiZ2RadEpYd3hJOGlNOD0%3D&CT=1716966009623&OR=OWA-NT-Mail&CID=d7428f61-5cd7-52c5-4840-bfc600eafc2c&clickParams=eyJYLUFwcE5hbWUiOiJNaWNyb3NvZnQgT3V0bG9vayBXZWIgQXBwIiwiWC1BcHBWZXJzaW9uIjoiMjAyNDA0MTkwMDcuMjgiLCJPUyI6IldpbmRvd3MgMTEifQ%3D%3D), including filling in the form, getting the neccesary approvals and sending it to [DigIT](digit-hpc@helmholtz-munich.de). 


### 2. Install WSL on your PC

[How to set up a WSL development environment?](https://learn.microsoft.com/en-us/windows/wsl/setup/environment)

*Disclaimer: WSL 2 is not working for me, I need to use WSL 1. You can verify this, by executing `sudo apt update` from WSl terminal.*


### 3. Install the neccessary software (PC and cluster)

Install all neccessary tools by following the [tool installation guide](https://bioinformatics_core.ascgitlab.helmholtz-muenchen.de/it_hpc_documentation/Installations.html)


### 4. Connect

Establish [VSCode connection](https://bioinformatics_core.ascgitlab.helmholtz-muenchen.de/it_hpc_documentation/Installations.html#VSCode-Cluster-Connection)

## Buy me a coffee

If you found this helpful, buy me a coffee or contribute :)

