.. _cira:

CIRA
============

.. image:: images/cira18Logo.png
   :width: 400
   :align: center

I have recently transitioned to a research scientist role at the Cooperative Institute for Research in the Atmosphere (CIRA) located at Colorado State University. 
With every new position, has new HPC. This page is a place to document the CIRA resources. 

++++++++++++
Server List 
++++++++++++

There is no central node that talks to other computers at CIRA. But there is several computer resources available to folks at CIRA to use. 

.. rst-class:: centered-table

+-----------+-------------+------------------+-----------+----------------------+------------------+
|   Name    | Primary Use | # of CPU Threads | # of GPUs |     GPU Type(s)      | GPU RAM per Card |
+===========+=============+==================+===========+======================+==================+
| shannon   | ML Core     |       32         |    2/4    | RTX6000/RTXA6000     | 24GB/46GB        |
+-----------+-------------+------------------+-----------+----------------------+------------------+
| dorian    | TC Group    |       76         |    4      | RTX6000              | 24GB             |
+-----------+-------------+------------------+-----------+----------------------+------------------+
| overcast1 | OVERCAST    |       64         |    2      | RTXA6000             | 46GB             |
+-----------+-------------+------------------+-----------+----------------------+------------------+
| cloudy1   | OVERCAST    |      112         |    4      | RTXA600              | 46GB             |
+-----------+-------------+------------------+-----------+----------------------+------------------+
| cloudy2   | OVERCAST    |      112         |    4      | RTXA600              | 46GB             |
+-----------+-------------+------------------+-----------+----------------------+------------------+
| locust    | OVERCAST/   |       72         |    1      | GH200                | 100GB            |
|           | ML Core     |                  |           |                      |                  |
+-----------+-------------+------------------+-----------+----------------------+------------------+
| cicada    | OVERCAST/   |       72         |    1      | GH200                | 100GB            |
|           | ML Core     |                  |           |                      |                  |
+-----------+-------------+------------------+-----------+----------------------+------------------+


+++++++++++++
GH200 How To
+++++++++++++

GH200s are a pre-production hardware model out of NVIDIA. Because of this, the standard way to install pytorch doesnt work.
In order to get pytorch to talk to the GPU we have to use something called an NVIDIA toolkit, which has precompiled images of pytorch.
To run this nvidia toolkit, we need to use ``docker``. Don't worry, with the help of ChatGPT I was able to get this going easily.
To do so, do the following: 



1) Create Dockerfile 

Make a Docker file that you will compile. This file will have several lines in it here is an example of my diffusion env: 

.. code-block:: bash

    # Install specific pytorch image from NVIDIA toolkits 
    FROM nvcr.io/nvidia/pytorch:24.03-py3

    # Install PyTorch-based libraries diffusers and transformers
    RUN pip install diffusers["torch"] transformers matplotlib tensorboard accelerate

    # name the workspace 
    WORKDIR /workspace

    # bash shell, because this is what i am use to
    CMD ["bash"] 


Name this file Dockerfile (important so the config call identifies the right file). 

2) Compile Dockerfile  

Go ahead and compile your new docker image 

.. code-block:: console
    
    $ docker build -t my_custom_pytorch_image .


This will take a few minutes to run the first time, needs to download all the pytorch stuff etc. 

3) Launch Screen 

Before you start a long training job, you will want to launch a screen here so if you get disconnected. This is a helpful tip in general for linux hpc systems 

.. code-block:: console

    $ screen -S training 

This will launch a session that you can reconnect to if you get disconnected by: 

.. code-block:: console 
    
    $ screen -r training


4) Run Docker

You are ready to run your docker image, so go ahead and call it

.. code-block:: console 
    
    $ docker run --gpus all -it --rm -v /mnt/data1:/mnt/data1/ my_custom_pytorch_image ``` 

You will want to mount your data directory to it, to do that you can see the ``/mnt/data1:/mnt/data1/`` which is the ``source_dir_in_default_machine / where_you_want_the_dir_on_the_docker_image``. For this example I just map it to the same directory path. 

5) Launch training 

You should be good to go to run your pytorch python code here. To check you can launch a quick python session 

.. code-block:: console
    
    $ python 
    $ import torch 
    $ torch.cuda.is_available()

It should say ``True`` , exit out (``$ exit()``) and run your python now. Here is an example of me launching my diffusion training 

.. code-block:: console
    
    accelerate launch train_diffusion_model.py

Then to exit out of the screen (i.e., to run it in the background) do cntrl + a + d , this will 'detach' the screen so you can check nvidia-smi or run tensorboard (to monitor progress).