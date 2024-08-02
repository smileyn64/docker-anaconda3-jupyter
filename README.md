# anaconda3-jupyter Portainer
Instruction to deploy Anaconda 3 and jupyter notebook from Portainer, with read-to-use browser access :)  

## Usage  
### Step 1
Download the image continuumio/anaconda3 manually on the image screen

> continuumio/anaconda3

### Step 2
Click on "build a new image" button and use the following script to create an image named "anaconda3-jupyter"


> FROM continuumio/anaconda3
>
> RUN /opt/conda/bin/conda install jupyter -y --quiet && \ 
    mkdir /opt/notebooks && \
    /opt/conda/bin/jupyter notebook --generate-config
>
> RUN conda install conda-forge::cdsapi
>
> ADD configgen.py /root/
>
> ADD backup.cdsapirc /root/
>
> ENTRYPOINT ["/bin/bash", "-c", "python /root/configgen.py"]

### Step 3
Create a stack named "anaconda3-jupyter"

> version: '2'

> services:
>
>  anaconda3-jupyter:
>
>    image: anaconda3-jupyter:latest
>
>    container_name: anaconda3-jupyter
>
>    ports:
>
>    - "8888:8888"
>
>    environment:
>
>    - disable_password=$disable_password
>
>    - jupyter_password=$jupyter_password
>
>    - base_url='/'
>     
>    volumes:
>
>    - ./notebooks:/opt/notebooks



## Note
This is intended to be used behind nginx reverse proxy, thus no SSL is included  
Using SSL is highly recommanded, unless you just want to use it over LAN :)

## Credit
Forked from ICEFIRE/docker-anaconda3-jupyter 
ans was Mostly modified from Anaconda3 official docker image
Official Ananconda Docker  
  https://hub.docker.com/r/continuumio/anaconda3/dockerfile  
and walkthrough of installing Jupyter Notebook and Anaconda3 on Docker  
  https://github.com/luojiahai/XAI-Playground  
