# Docker files to create Docker images with AFEPack

After clone this reJust run command :

	docker build ocker build -t afepack:v0 .

the `-t` option define a tag.

Then run:
	
	docker run -ti afepack:v0 bash

This will drop you in an isolated environment where you can experiment with This image. In addition, you can also use `-v` option to mount a directory to this image, for eaxmple the following command will mount $HOME/Pkg on /opt directory 

	docker run -ti -v $HOME/Pkg:/opt afepack:v0 bash

