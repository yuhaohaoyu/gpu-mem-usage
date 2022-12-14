
# KEY: Use a Singularity container they had with NCU in

```
ssh hy2467@access.cims.nyu.edu
ssh greene.hpc.nyu.edu
module load cuda/11.1.74 python/intel/3.8.6	# do this right after log ingo greene
ssh burst
srun --account=csci_ga_3033_085-2022fa --partition=c12m85-a100-1 --gres=gpu:1 --pty /bin/bash	# start a slurm interactive job
```


Create and enter the python venv (virtual environment), in this case I have named my python venv HAO.

```
cd ~
python3.8 -m venv env_name
source env_name/bin/activate
```

Inside the python venv, with the right version python3 and pip3, you can install torch, torchsummary, numpy. Maybe pip packages

```
pip3 install torch
pip3 install torchsummary
pip3 install torchvision
pip3 install -U numpy
```

Now get out of the virtual environment and get into the Singularity Container, then get into your virtual python environment.

```
(HAO) deactivate
/share/apps/images/run-nsight-comput-2021.2.2.1.bash	# start a singularity container (the magic)
Singularity> source ~/env_name/bin/activate
(HAO) Singularity> which ncu
	/ext3/nsight-compute/2021.2.2.1/ncu
```

# Verify python3 and torch

```
(HAO) Singularity> python3
Python 3.8.10 (default, Sep 28 2021, 16:10:42)
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> torch.__version__
'1.13.0+cu117'
>>> torch.cuda.is_available()
True
```


# 2 tests: clone this repo 

```
cd mnist
./test-no-ncu.sh
./test-w-ncu.sh			# this will create a ncu.out
vi ncu.out
```

# Copy out the data files

a way to SCP from Burst to local system. (on NYU VPN)
1. SCP from Burst to Greene. On Burst, issue this command:

```scp <log-file-path> <user-ID>@greene-dtn.hpc.nyu.edu:/home/<user-ID/```

2. SCP from Greene to local system. ON your local system, issue this command:
	
```scp <user-ID>@greene-dtn.hpc.nyu.edu:/home/<user-ID>/<log-file> <path-on-your-local(to save the log file)> ```


