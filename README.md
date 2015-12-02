# DeepLearning

Python notebooks of me learning deep learning

## How I Run Theano on Windows 10

I couldn't get Theano to run natively on Windows 10 with Python 3.5, 
despite some [helpful][blog-guide] [guides][reddit-guide]. Therefore, I 
used Docker to install and run a virtual Linux machine configured just for 
running theano. The steps for a new machine, adapted from [here][so-ipython]:

1. Install [docker](https://www.docker.com/docker-toolbox) and 
   [get started](https://docs.docker.com/engine/installation/windows/) with docker
2. To use docker, open the Docker Quickstart Terminal.
3. Install the [Theano CPU docker image][docker-theano]:  
   `docker pull kaixhin/theano`
4. Fire up the virtual machine image:  
   `docker run -dit -v //c/Users/trist/theano:/media/disk -p 8888:8888 kaixhin/theano`  
   where `-v x:y` says to mount my Windows folder `x` in the 
   location `y` in the virtual machine and `-p 8888:8888` maps the virtual 
   machine's port 8888 to the Windows machine's port 8888 available. 
5. The machine is now running in the background. View its name:  
   `docker ps`
6. Open `bash` on the machine:  
   `docker exec -ti [docker-id] bash`  
   where `[docker-id]` is the machine's `CONTAINER ID` or `NAMES` from the last step.
7. Inside the virtual machine, install ipython and open it in the mounted Windows folder:  
   `pip install notebook`
   `cd media/disk`
   `ipython notebook --ip='*'`
8. Open another terminal and look up the IP address of the virtual machine:  
   `docker-machine ip default`
9. Go to the http://[docker-machine-ip]:8888 to open the Jupyter notebooks

[blog-guide]: http://blog.ihsgnef.tk/theano-cuda-windows/
[reddit-guide]: https://www.reddit.com/r/MachineLearning/comments/3hkv2b/most_recent_way_to_install_theano_for_windows_10/
[docker-theano]: http://deeplearning.net/software/theano/install.html#docker-images
[so-ipython]: https://stackoverflow.com/questions/33616094/tensorflow-is-it-or-will-it-sometime-soon-be-compatible-with-a-windows-work/33635663#33635663
