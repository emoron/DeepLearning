# DeepLearning

## How I Run Theano on Windows 10

I couldn't get Theano to run natively on Windows 10 with Python 3.5, 
despite some [helpful][blog-guide] [guides][reddit-guide]. Therefore, I 
used Docker to install and run a virtual Linux machine configured just for 
running theano. The steps for a new machine, adapted from [here][so-ipython]:

### Installing the docker image

Install [docker](https://www.docker.com/docker-toolbox) and 
[get started](https://docs.docker.com/engine/installation/windows/) with docker

To use docker, open the Docker Quickstart Terminal.

Install the [Theano CPU docker image][docker-theano] 

```
docker pull kaixhin/theano
```

### Customizing the docker image

Let's open the docker image

```
docker run -it kaixhin theano
```

Now we're running `bash` inside the docker image. The prompt will look like:

```
root@81d18d41f513:/# 
```

Note the machine hash (the numbers between the `@` and the `:/#`).

Now we install more stuff on top of this image and exit the image.

```
pip install ipython
pip install version_information
pip install notebook
exit
```

Now, we [save a snapshot of the image][making-images], so we don't have to re-install these packages.
This is where the machine hash comes in:

```
docker commit -m "install ipython" -a "TJ Mahr" 81d18d tjmahr/theano
```

where `-m` is a commit message, `-a` is the author. These are followed by the 
image hash and the new name for the image.


### Running iPython

Now, we can fire up the virtual machine image:  

```
docker run -dit -v //c/Users/trist/theano:/media/disk -p 8888:8888 tjmahr/theano 
```
where `-v x:y` says to mount my Windows folder `x` in the 
location `y` in the virtual machine and `-p 8888:8888` maps the virtual 
machine's port 8888 to the Windows machine's port 8888 available. 

The machine is now running in the background. View its name:  

```
docker ps
```

Open `bash` on the machine

```
docker exec -ti [docker-id] bash
```

where `[docker-id]` is the machine's `CONTAINER ID` or `NAMES` from the last step.

Inside the virtual machine, install ipython and open it in the mounted Windows folder:  

```
cd media/disk
ipython notebook --ip='*'
```

Open another terminal and look up the IP address of the virtual machine:  

```
docker-machine ip default
```

Go to the http://[docker-machine-ip]:8888 to open the Jupyter notebooks

[blog-guide]: http://blog.ihsgnef.tk/theano-cuda-windows/
[reddit-guide]: https://www.reddit.com/r/MachineLearning/comments/3hkv2b/most_recent_way_to_install_theano_for_windows_10/
[docker-theano]: http://deeplearning.net/software/theano/install.html#docker-images
[so-ipython]: https://stackoverflow.com/questions/33616094/tensorflow-is-it-or-will-it-sometime-soon-be-compatible-with-a-windows-work/33635663#33635663
[making-images]: https://docs.docker.com/engine/userguide/dockerimages/#updating-and-committing-an-image
