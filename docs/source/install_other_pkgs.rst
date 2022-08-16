Install Other Pkgs
==================

++++++++++
Background
++++++++++

While you might think it is easy to install other python packages along side tensorflow, this is not the case. 
Often times what happens is that one package will want to degrade tensorflow to an older version, which could
screw up the co-dependancies for tensorflow. This was first noticed by it taking more than 20 mins to just load
the tensorflow libraries (e.g., it looked like the script was doing nothing for more than 20 mins and never starts training). 
Thus, the best practice is to make sure you use the conda-forge channel for packages, and to abort the package install if it 
wants to downgrade the tensorflow, cudnn or cudatoolkit versions. 

+++++++++++
Clean Conda
+++++++++++

As you will soon find out, conda can often be very bloated (> 5 GB). Given we only have 20 GB of space on the home dir, 
this can be a BIG problem. To prevent conda from getting too big, it is best to use the clean command, which will clear
out a cache of packages that are being stored for install at a later date. Don't worry, conda can just go download these
files again later if it needs them. 

.. code-block:: console

    $ conda clean --all 

If after running the clean command check your home dir space

.. code-block:: console

    $ du -sh /home/username/

If you still have way too much stuff (>15 GB) and anaconda is not the culprit, you might want to check your hidden 
directories. Often times pip likes to install things there (like .cache, and .local/lib/pythonX.X). You CAN remove 
.cache and .local/lib/pythonX.X to free up space. 

++++++++++++++++++++
Install new packages
++++++++++++++++++++

You are now ready to install other packages you need. For example, here is how I would install matplotlib 


.. code-block:: console

    $ conda activate tf_gpu
    $ mamba install -c conda-forge matplotlib 

You can repeat this for other packages. Remember ALWAYS use `-c conda-forge`!

+++++
UNETs
+++++

A lot of members of Dr. McGovern's Lab have used the `keras_unet_collection <https://github.com/yingkaisha/keras-unet-collection>`_ python package. 
This package helps with making UNETs be a bit less verbose when actually building the arch. While that original package is nice,
I have made some changes to allow for more flexibility. If you want to use my fork do the following

.. code-block:: console

    $ conda activate tf_gpu
    $ pip install git+https://github.com/dopplerchase/keras-unet-collection.git

Now you should be able to use the package. 
To test it go ahead and try the keras_unet_collection_test.py, a script that was in the tutorial folder
that builds a very simple UNET model. 