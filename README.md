# Docker files to create Docker images for AFEPack

As [AFEPack](http://dsec.pku.edu.cn/~rli/software.php#AFEPack) use some files of deal.II, and deal.II has changed since version 6.3 . So I write two dockerfiles, `Dockerfile-new` use Ubuntu 16.04 and deal.II 8.1 ,
and `Dockerfile-old` use Ubuntu 14.04 and deal.II 6.3.1.

After clone this repo , then run command
```bash
sudo docker build -f Dockerfile-new -t afepack:v0 . # if you want to use new deal.II
```
or
```bash
sudo docker build -f Dockerfile-old -t afepack:v0 . # if you want to use old deal.II
```
to build the docker image for AFEPack. The `-t` option define a tag.

To use this image, just run:
```bash
sudo docker run -ti afepack:v0 /bin/bash
```

This will drop you in an isolated environment where you can experiment with this image.

In addition, you can also use `-v` option to mount a directory to a docker, for eaxmple, following command will mount host's `$HOME/Pkg` on container's `/opt` directory.
```bash
sudo docker run -ti -v $HOME/Pkg:/opt afepack:v0 /bin/bash
```

For `old` configuration, `deal.II 6.3.1` and `AFEPack` are installed in `/root/Pkg`.
For `new` configuration, `deal.II 8.1.` installed in `/usr` and `AFEPack` installed in `/root/Pkg`.

file `Dockerfile-old-mb` use docker's new "mult-stage build", so the image would be much smaller.