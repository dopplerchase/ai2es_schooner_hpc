.. _github:

Github Tips
============

.. image:: images/GitHub-Emblem.png
   :width: 400
   :align: center

.. note::

   This page will come in the next release of these docs. At the bottom of this page, there are the steps listed out. I just haven't had time to be more detailed yet. 

`Github <https://github.com/ai2es>`_ is a fantastic software base that allows you to archive, track code changes and share code with the world. A key part of AI2ES is the open-access nature of its code. Thus, all AI2ES funded research shall encorperate open-acess code/data. One easily accesible way to do this is by posting code for your research on github. While there are likely countless tutorials about github out there, I (Randy) have not seen anywhere that lays out simple steps to follow when using github. Thus, this page will hopefully guide you. 

I will assume you already have a github account. If not, go sign up for one. Go to `https://github.com/ <https://github.com/>`_

+++++++++++++++++++++++++
Step 1: Create Repository
+++++++++++++++++++++++++

If you are looking to start a new project you need to create a new repository. AI2ES has made a template to make this easy. 

+++++++++++++
Steps Listed
+++++++++++++

1. ``git clone repo``

2. ``git checkout -b NEW_BRANCH_NAME`` <-- open an new branch

3. make changes to code 

4. test changes 

5. ``git add -A`` #add all changes into the code 

6. ``git commit -m "MESSAGE"`` <-- add a descriptive but short message here 

7. ``git push https://TOKEN@github.com/PATH/TO/REPO/REPO_NAME.git NEW_BRANCH_NAME`` 

8. on Github: open pull request on your fork

9. merge in changes when you are happy with your changes.

