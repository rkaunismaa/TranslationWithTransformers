# TranslationWithTransformers
This repository contains a curated selection of notebooks and code related to Translation using Transformers. 

## Initial setup of the run time environment

### Thursday, February 16, 2023

Create a new docker image from the following image:

	docker pull pytorch/pytorch:1.13.1-cuda11.6-cudnn8-devel
	
Run the following command to create this new image:

	docker build ~/Data/Documents/Github/rkaunismaa/TranslationWithTransformers -f DockerfilePT1131 -t pt1131:20230216
	
Dammit ... the build was trying to download torch 1.13.0 because of the torch data install. So remove that layer from 
the build and try again. 

Ok. I now see ...

	(base) rob@KAUWITB:~$ docker images
	REPOSITORY                                TAG                            IMAGE ID       CREATED         SIZE
	pt1131                                    20230216                       2af8128e198b   2 minutes ago   17.9GB
	huggingface/transformers-pytorch-gpu      latest                         47dc5ba9a731   39 hours ago    17.8GB
	tf210gpu                                  20230208                       836ae37e3612   8 days ago      6.15GB
	
So let's give er a try, shall we ... !?

    docker run --gpus all -it -v $(realpath ~/):/tf/All -v /home/rob/Data2:/home/rob/Data2 --env HF_DATASETS_CACHE=/home/rob/Data2/huggingface/datasets --env TRANSFORMERS_CACHE=/home/rob/Data2/huggingface/transformers -p 8888:8888 -p 6006:6006 pt1131:20230216

As I was working through the notebooks, some additional resources were needed to run ...

pip install spacy
pip install torchdata==0.5.1

### Friday, February 17, 2023

Resume with the container we created yesterday ...

    docker container start intelligent_golick

Oh for fucks sake ... I ran ..

    pip install -U trax

... and now I see it's install tensorflow 2.11 ... gotta wonder what this is gonna break! .. 
Plus jax, tensorflow text, tensorflow datasets ...

If this breaks stuff, then I may have to rebuild the image. WOW, does it ever install a ton of new libraries!



	



	



