Welcome to the AI2ES Schooner HPC Guide
========================================

.. image:: images/ai2es-logo-web-trans.png
   :width: 300
   :align: center

.. note::

   This project is under active development. Last edit made on 25 Feb 2023 by RJC

++++++++++++
Introduction
++++++++++++

This guide is intended to help all potential users of the 
University of Oklahoma Supercomputing Center for Education and Research (`OSCER <https://www.ou.edu/oscer>`_) High Performance Computing facility named "Schooner". 
More specifically, this guide was built for `AI2ES <https://www.ai2es.org>`_ members who are using the 
Graphical Processing Units (GPUs) installed for exclusive AI2ES use. 

While there is a lot of nuances with the AI2ES/Schooner machine, there is likely some helpful information for folks on other HPC machines, but your milage may very. 

++++++++++
Background
++++++++++

Machine Learning, more specifically deep learning, is accelerated by the use of GPUs. Thus, as part of AI2ES 
there are a suite of computational nodes that have GPUs available for AI2ES members to use. More specifically,
we currently have a total of 2 V100s (32 GB of RAM each) and 18 A100s (40 GB of RAM each).
Unfortunately, there is a bit of a learning curve to get up to speed with efficent use of these computing resources, so 
this guide is intended to support everyone trying to learn how to use them. If you have any specific questions, 
please feel free to reach out to me, Randy Chase, by email (randy.chase 'at' colostate 'dot' edu) or in our AI2ES Slack (either directly, or in the #schooner channel). 
I am a meteorologist by training, so this guide has been written through my trial and error.

Contents
--------

.. toctree::  
   general_hpc
   install_tensorflow
   test_gpu
   general_gpu
   install_other_pkgs
   tensorboard
   github
