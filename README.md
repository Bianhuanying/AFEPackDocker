# Docker files to create Docker images for AFEPack

As AFEPack use some files of deal.II, and deal.II has changed some files. So I write two dockerfiles, Dockerfile-new using Ubuntu 16.04 and deal.II 8.1 ,
and Dockerfile-old using Ubuntu 14.04 and deal.II 6.3.1.

After clone this , then run command

	sudo docker build -f Dockerfile-new -t afepack:v0 . # if you want to using new deal.II

or

	sudo docker build -f Dockerfile-old -t afepack:v0 . # if you want to using old deal.II

to build the docker image for AFEPack. The `-t` option define a tag.

To use this image, just run:
	
	sudo docker run -ti afepack:v0 bash

This will drop you in an isolated environment where you can experiment with this image.

In addition, you can also use `-v` option to mount a directory to a docker, for eaxmple the following command will mount $HOME/Pkg on /opt directory 

	docker run -ti -v $HOME/Pkg:/opt afepack:v0 bash
