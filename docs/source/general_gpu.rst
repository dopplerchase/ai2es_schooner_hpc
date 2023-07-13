.. _general_gpu_tips:

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

-------------
Current Setup
-------------

As of 18 Dec 2022, AI2ES has a total of 20 GPUs that we own
and can use. More specifically, we have 2 NVIDIA V100s
(previous generation) and 18 NVIDIA A100s (current generation). Inside some of these nodes, "NVlink" connects the GPUs together
at high bandwidth (i.e., can share information quickly) [the nodes with more than one GPU]. Each node has a local disk to do some fast computations on. This local disk is called $lscratch and varies per node. Also, the 
amount of $lscratch you get scales with how many CPU threads you requested ``$SBATCH -n X; where X is the number of threads`` . 


Here is a nice summary table of the nodes and their attributess

.. rst-class:: centered-table

    +---------+------------+---------------------+------------------+-----------------+-------------------+
    |  node   | $lscratch  |  # of CPU threads   |  # of GPUs       |   GPU Type      | GPU RAM per card  |
    +=========+============+=====================+==================+=================+===================+
    |  c314   |   892GB    |        24           |       1          |      V100       |       32GB        |
    +---------+------------+---------------------+------------------+-----------------+-------------------+
    |  c315   |   892GB    |        24           |       1          |      V100       |       32GB        |
    +---------+------------+---------------------+------------------+-----------------+-------------------+
    |  c731   |   852GB    |        112          |       2          |      A100       |       40GB        |
    +---------+------------+---------------------+------------------+-----------------+-------------------+
    |  c732   |   384GB    |        128          |       4          |      A100       |       40GB        |
    +---------+------------+---------------------+------------------+-----------------+-------------------+
    |  c733   |   384GB    |        128          |       4          |      A100       |       40GB        |
    +---------+------------+---------------------+------------------+-----------------+-------------------+
    |  c829   |   384GB    |        128          |       4          |      A100       |       40GB        |
    +---------+------------+---------------------+------------------+-----------------+-------------------+
    |  c830   |   384GB    |        128          |       4          |      A100       |       40GB        |
    +---------+------------+---------------------+------------------+-----------------+-------------------+



______________________
New Schooner Resources
______________________

In addition to the new AI2ES resources, AI2ES project members will also have
access to OSCER-owned GPUs as well. At this time I don't have specifics, but know
there is a partition named ``gpu`` which is where the OSCER gpus can be used. 

``?``: 3 nodes, dual A100s each (40 GB GPU RAM each), NO NVlink (i.e., can't share data across them)

``?``: 3 nodes, dual A100s each (80 GB GPU RAM each), with NVlink between GPUs in the same node (600 GB/sec)

Lastly, with these new GPU resources for Schooner, there are plans to obtain the next generation of GPUs (H100s). The current plan is for these to arrive maybe sometime late in 2023. 

.. note::

    Know that the OSCER-owned GPUs will be for general use for all (e.g., the > 1,300 Schooner users). So queue times will likely be much longer than our ai2es nodes. 

+++++++++++++++++++++++
Requesting AI2ES Nodes
+++++++++++++++++++++++

In order to request ANY of the nodes available, add the following line of code to your sbatch file

.. code-block:: console

    #SBATCH -p ai2es


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

By default, ``tensorflow-gpu`` will use all available GPUs and GPU memory. This is fine, if your node only has one GPU (e.g., the v100 nodes, c314 and c315), but our newer nodes have 
multiple GPU cards installed. Thus, when multiple programs, or multiple users, attempt to use the same GPU, they can interfere destructively with one-another (i.e., crash the job and hault training). 
To then prevent this from happening you can request the number of GPU cards in your sbatch script by adding the following: 

.. code-block:: bash 
    
    #SBATCH -p ai2es_a100
    #SBATCH --gres=gpu:1

where this example requests 1 gpu from the a100 queue. If you wanted 2, change the ``1`` to ``2`` and so on. During execution, your batch file environment variable ``$CUDA_VISIBLE_DEVICES`` 
will be set to a comma-separated string containing the integers of the physical GPUS that have been allocated. 

Now once you added the above bit to your bash file, please add the following near the top of your python script: 

.. code-block:: python 

    import os 
    import tensorflow as tf 

    if "CUDA_VISIBLE_DEVICES" in os.environ.keys():
        # Fetch list of logical GPUs that have been allocated
        #  Will always be numbered 0, 1, …
        physical_devices = tf.config.list_visible_devices('GPU')
        n_physical_devices = len(physical_devices)

        # Set memory growth for each
        for device in physical_devices:
            tf.config.experimental.set_memory_growth(device, True)
    else:
	    #No allocated GPUs: do not delete this case!                                                                	 
    	tf.config.set_visible_devices([], 'GPU')

    # Do the work …

This will ensure your training session only uses the GPUs that you have been allocated from your bash script. 

This might seem annoying, but this is VITAL to the AI2ES GPUs and ensuring everyone can get what the they need done. 


---------------------
Checking GPU traffic
---------------------

It is hard to tell who is using which GPUs, but you can check to see which nodes are currently in use with the following: 

.. code-block:: bash 

    $ squeue -p ai2es,ai2es_v100,ai2es_a100,ai2es_a100_2,ai2es_a100_4

If the resulting output is blank, no one is using the nodes. If there are names listed, it shows you who is using what node and for how long the jobs have been running but it does not tell you how many of the GPUs on any single node (or which specific GPUs they are using). To assure smooth sharing, right now we will use the following 'sign-out' table: 

.. image:: images/GPU_Sharing_Table.png
   :width: 300
   :align: center

When you submit a jon and if SLURM tells you ``PRIORITY``, then it is likely the other people on the node have consumed all the CPU resources or all the GPUs for the queue you chose
are being used (e.g., ``#SBATCH -p ai2es_a100``). As a reminder, each node has about 20 CPU cores and ~ 30 GB of RAM. Try adjusting your following lines

.. code-block:: bash 

    #SBATCH --ntasks=4
    #SBATCH --mem=16G

``--ntasks=4`` will allocated 4 CPUs to your job and ``--mem=16G`` will allocate 16 GB of RAM or submit to a different GPU queue. 

---------
Long Jobs 
---------

Even though we have some of the fastest GPUs out there, big deep learning jobs can still take days. As a good
rule of thumb, if you plan to train for more than 24 hours, PLEASE PLEASE PLEASE let other AI2ES memebers know.
It is best to drop a line in the #schooner channel in the ai2es slack, and ask if it is alright you will be using up 
a GPU for over 12 hours.

The main reason behind this is because often times people have deadlines. Consider the frantic PhD student trying to 
finish up their general exam and the come to find out ALL the GPUs are already in use.... yeah not a good scenario. Or 
consider the scientist working on addressing the major reviews on their paper which are due in a couple days. Yeah they
should have preference. 

Currently there is no limit to the number of computational hours any one user can use. I would love to keep it this way. 