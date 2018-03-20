Based on tensorflow.def, here's a sequence of instructions for how to
build an image sandbox that includes tensorflow, add anything you want
to the sandbox, then create a bootstrapped singularity image from the
sandbox. This is useful for testing complicated build addons to
Tensorflow when creating an image. 

#1 Build Sandbox space from full tensorflow definition (corrected, of course)
sudo singularity build --sandbox sandbox_horovod_no_nccl/ tensorflow.def

#2 Launch a shell that is writable in the sandbox
sudo singularity shell --writable sandbox_horovod_no_nccl/

#3 Use shell to install/recompile relevant software
#...

#4 Create an empty image (based on TITAN people's suggestion) (5000 is insufficient)
sudo singularity create --force --size 10000 hvd_gather.img

#5 Create final image from the sandbox
sudo -E singularity bootstrap hvd_gather.img sandbox_horovod_no_nccl/
