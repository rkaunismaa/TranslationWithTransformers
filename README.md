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

### Saturday, Feburary 18, 2023

I reran some of the older notebooks just to ensure they still run without errors, and they do. Nice!

Installed transformers ...

    pip install transformers
    
Hmm looks like I need a few more libraries ...

    pip install transformers datasets evaluate
    
    pip install sentencepiece
    
    pip install sacrebleu
    
Hmm ... also need to install git ... for the huggingface login stuff.
    
    conda install -c anaconda git
    conda install git-lfs

For the notebook AnnotatedTransformer.ipynb, I needed to install ...

    pip install altair
    pip install gputil
    
### Sunday, February 19, 2023

Yesterday, I was having some issues with downloading and use models in spacy. 

Ok, now that is odd. Code which works just fine in one docker environment does not work in another docker environment. 

    try:
        spacy_de = spacy.load("de_core_news_sm")
    except IOError:
        os.system("python -m spacy download de_core_news_sm")
        spacy_de = spacy.load("de_core_news_sm")

    try:
        spacy_en = spacy.load("en_core_web_sm")
    except IOError:
        os.system("python -m spacy download en_core_web_sm")
        spacy_en = spacy.load("en_core_web_sm")
        
Both environments have spacy version 3.5.0

docker container start intelligent_golick => fails

docker container start cool_noether => works

So yeah, I am going to trash the container intelligent_golick and start fresh from the original image. 

Killed it, then ran ...


        docker run --gpus all -it -v $(realpath ~/):/tf/All -v /home/rob/Data2:/home/rob/Data2 --env HF_DATASETS_CACHE=/home/rob/Data2/huggingface/datasets --env TRANSFORMERS_CACHE=/home/rob/Data2/huggingface/transformers -p 8888:8888 -p 6006:6006 pt1131:20230216
        
Spin up fine. Started the AnnotatedTransformer.ipynb notebook. Ran 'pip install spacy'. Then ran the code to download the models. Stuff works! So I guess the takeaway is that it's always good to try stuff in another environment. I dicked around in the same intelligent_golick environment for too long, trying to see why the spacy stuff was no longer working, not realizing it was something outside of spacy that was causing the problem. 

Below are the various packages I installed as I worked my way through this notebook:

    pip install spacy
    
    pip install altair
    
    pip install GPUtil
    
    pip install torchdata==0.5.1
    
    
I now have a new container,  docker container start sad_nightingale.

Re-running the notebook HuggingFace.ipynb

    pip install transformers
    
    pip install datasets
    
    pip install sentencepiece
    
    pip install evaluate
    
    pip install sacrebleu
    
HuggingFace_2.ipynb ... same error as before. meh .. 

Ok, nice. I figured out the problem. I needed to load the model from one of the checkpoint sub folders. 



	



	



