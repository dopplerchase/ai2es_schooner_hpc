General GPU Tips
================

++++++++++
Background
++++++++++

Having access to these amazing machines is truly a privelage. Please consider the tips and best practices listed here 
to make sure you are not hogging all the computing time to yourself. 

++++++++++++++++++++++++++
Understanding AI2ES Nodes
++++++++++++++++++++++++++

As of 16 Aug 2022, AI2ES has a total of 12 GPUs that we can use. More specifically, 
we have 2 NVIDIA TESLA V100s which each have their own node (c314 and c315) and 8 NVIDIA A100s 
that are split across 3 nodes (c731, c732 and c733). c731 has 2 A100s, while c732 
and c733 both have 4 GPUs attached to them. The V100s have 32 GB of RAM while the A100s have 40 GB of RAM. 
The V100s are about half as fast as the A100s, so keep that in mind if you notice speed differences between then nodes.

In order to request ANY of the nodes available, add the following line of code to your sbatch file

.. code-block:: console

    #SBATCH -p ai2es

The priorty list for this command will be c314, c315, c731, c732 and then c733. So usually you will
get allocated the V100s first. 

If you want to specifcally get the V100 add the following

.. code-block:: console

    #SBATCH -p ai2es_v100

Alternatively, if you want to specifcally get the A100 add the following

.. code-block:: console

    #SBATCH -p ai2es_a100

If you want to specifcally get the A100 with 2 GPUS do the following

.. code-block:: console

    #SBATCH -p ai2es_a100_2

Or if you want to specifcally get the A100 with 4 GPUS do the following

.. code-block:: console

    #SBATCH -p ai2es_a100_4

+++++++++++++
Sharing GPUs 
+++++++++++++

Now that you have figured out how to request a gpu node with sbatch, it is important to only use the 
number of GPUs you NEED. A100s are a very valuable GPU and are some of the fastest GPUs out there for 
deep learning. So you could see how requesting all 4 of them at time (for c732 and c733) and only using 1
of them is a waste of computational hours. 

By default, tensorflow will share the memory of your job across all 'visible' GPUs, which is all that are connected 
to the node you are using. Consider the example where your deep learning model only takes up a total of 20 GB of RAM. 
This job would be able to fit entirely on 1 GPU. So a more appropriate use of your job would be to specifically only use 1 GPU, 
not all 4 (or 2 if you are on c731). 

To do this, we will use that pip package `py3nvml <https://github.com/fbcotter/py3nvml>`_ which allows use to select the GPU 
you wish to use. 

.. code-block:: python

    import py3nvml
    py3nvml.grab_gpus(num_gpus=1, gpu_select=[0])
    
The following technique is suggested. Start with n_gpu=1, then if it fails saying not enough memory, then try n_gpu=2 
(you will have to change gpu_select to be [0,1]) and so on. 

If you know you will use ALL of the GPUs attached to a specifc node, you can use the following flag in your sbatch 

.. code-block:: bash 

    #SBATCH --exclusive

This will make sure no one else can use your node or GPUs. Quick note, if you are using ALL of the GPUs you should be doing 
distributed training. If you don't know what distributed training is, your probably don't need it. 

If you are confused by all this, please reach out to me (Randy Chase; randychase 'at' ou 'dot' edu). 

++++++++++
Long Jobs 
++++++++++

Even though we have some of the fastest GPUs out there, big deep learning jobs can still take days. As a good
rule of thumb, if you plan to train for more than 24 hours, PLEASE PLEASE PLEASE let other AI2ES memebers know.
It is best to drop a line in the #schooner channel in the ai2es slack, and ask if it is alright you will be using up 
a GPU for over 12 hours.

The main reason behind this is because often times people have deadlines. Consider the frantic PhD student trying to 
finish up their general exam and the come to find out ALL the GPUs are already in use.... yeah not a good scenario. Or 
consider the scientist working on addressing the major reviews on their paper which are due in a couple days. Yeah they
should have preference. 

Currently there is no limit to the number of computational hours any one user can use. I would love to keep it this way. 