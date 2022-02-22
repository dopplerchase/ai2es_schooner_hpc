Test GPU
=========

++++++++++
Background
++++++++++

Just because we have tensorflow installed, doesnt mean that your session on Schooner 
will automatically use the GPU. In this page, we will guide you through a simple example
to check that tensorflow can properly access the GPU. 

+++++++++++++++++
Grab example code
+++++++++++++++++

Copy my example files to your home dir 

.. code-block:: console

    $ cp /ourdisk/hpc/ai2es/shared/tutorial/* ./

Edit the drive_test_gpu.sh so that the email and usernames are filled in!

.. code-block:: console

    $nano ./drive_test_gpu.sh 

This is what you will see: 

.. code-block:: console

    GNU nano 2.3.1                                    File: drive_test_gpu.sh 
    #!/bin/bash
    #SBATCH -p ai2es
    #SBATCH --nodes=1
    #SBATCH -n 1
    #SBATCH --mem-per-cpu=1000
    #SBATCH --time=00:30:00
    #SBATCH --job-name="test_tensorflowgpu"
    #SBATCH --mail-user=username@university.edu <-- change this!
    #SBATCH --mail-type=ALL
    #SBATCH --mail-type=END

    #need to source your bash script to access your python! 
    source /home/username/.bashrc <-- change this!
    bash

    #activate your tensorflow env
    conda activate tf_gpu 

    #run the simple test 
    python -u simple_test_gpu.py

Submit the test job to SLURM

.. code-block:: console

    $ sbatch ./drive_test_gpu.sh 

Running this job will result in a .out file in your home directory named: slurm-XXXXXXX.out. 
Within this file if things worked properly, you should see something like the following (use nano to see it)

.. code-block:: console

    Num GPUs Available:  1
    2022-02-02 11:35:51.381711: I tensorflow/core/platform/cpu_feature_guard.cc:142] This TensorFlow binary is optimized with oneAPI Deep Neural Network Library (oneDNN) to use the following CPU instructions in performance-critical operations:  SSE4.1 SSE4.2 AVX AVX2 FMA
    To enable them in other operations, rebuild TensorFlow with the appropriate compiler flags.
    2022-02-02 11:35:55.512881: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1510] Created device /job:localhost/replica:0/task:0/device:GPU:0 with 30985 MB memory:  -> device: 0, name: Tesla V100-PCIE-32GB, pci bus id: 0000:82:00.0, compute capability: 7.0
    2022-02-02 11:35:56.147581: I tensorflow/core/common_runtime/eager/execute.cc:1161] Executing op _EagerConst in device /job:localhost/replica:0/task:0/device:GPU:0
    2022-02-02 11:35:56.148136: I tensorflow/core/common_runtime/eager/execute.cc:1161] Executing op _EagerConst in device /job:localhost/replica:0/task:0/device:GPU:0
    2022-02-02 11:35:56.148383: I tensorflow/core/common_runtime/eager/execute.cc:1161] Executing op MatMul in device /job:localhost/replica:0/task:0/device:GPU:0
    tf.Tensor(
    [[22. 28.]
    [49. 64.]], shape=(2, 2), dtype=float32)
 
This should only take about 1 min to run. So if it is taking more than 10-15 mins please see my other note
on installing other packages (on the left panel).

CONGRATS! You should be ready to train your amazing ML models. 
The next doc will talk about SHARING these amazing computing resources. 
