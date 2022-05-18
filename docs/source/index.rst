Welcome to the AI2ES Schooner HPC Guide
========================================

.. note::

   This project is under active development. Last edit made on 22 Feb 2022 by RJC

++++++++++++
Introduction
++++++++++++

This guide is intended to help all potential users of the 
University of Oklahoma Supercomputing Center for Education and Research (`OSCER <https://www.ou.edu/oscer>`_) High Performance Computing facility named "Schooner". 
More specifically, this guide was built for `AI2ES <https://www.ai2es.org>`_ members who are using the 
Graphical Processing Units (GPUs) installed for exclusive AI2ES use. 

++++++++++
Background
++++++++++

Machine Learning, more specifically deep learning, is accelerated by the use of GPUs. Thus, as part of AI2ES 
there are a suite of computational nodes that have GPUs available for AI2ES members to use. More specifically,
we currently have a total of 2 V100s (32 GB of RAM each) and 10 A100s (40 GB of RAM each) [More A100s are ordered!].
Unfortunately, there is a bit of a learning curve to get up to speed with efficent use of these computing resources, so 
this guide is intended to support everyone trying to learn how to use them. If you have any specific questions, 
please feel free to reach out to me, Randy Chase, by email (randychase 'at' ou 'dot' edu) or in our AI2ES Slack. 
I am a meteorologist by training, so this guide has been written through my trial and error. Your milage may vary. 

Contents
--------

.. toctree::

   install_tensorflow
   test_gpu
   general_gpu
   install_other_pkgs
   tensorboard
   other_tips
