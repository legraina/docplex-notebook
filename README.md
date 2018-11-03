# DOCPLEX NOTEBOOK


We will be using a notebook docker image to run cplex on your computer. We are using the free edition that cannot solve problems with more than 1000 variables and 1000 constraints.

## DOCKER AND JUPYTERHUB

First, you need to install docker. This software is managing all the images for you. You can download the adequate version depending on your system on https://store.docker.com/search?type=edition&offering=community. You will need to create account first. Once installed, just launch it.

Then, we will test the system with a basic notebook. Open a terminal, and run:
```bash
docker run --rm -it -p 8888:8888 jupyter/base-notebook
```
Once it's running, you will see:
```bash
Copy/paste this URL into your browser when you connect for the first time,
to login with a token:
    http://(58a38e1b28f5 or 127.0.0.1):8888/?token=8f2df45e526c81bdbfe17e3a9d1da64e4a6edf6573e7cdca
```
Note that the token will be different.

Finally, open your favorite browser and go to http://127.0.0.1:8888/?token=8f2df45e526c81bdbfe17e3a9d1da64e4a6edf6573e7cdca (replace the token with yours).

You are now on jupyter notebook: you can create new notebook and be ready to code.

## CPLEX AND DOCPLEX

We will now install cplex inside a docker. First, you need an installer for cplex on linux 64 bits. It can be either the free edition or one that you already have. However, your version needs to support python 3.6 (12.8.0 supports it). To get the free edition, create an account on ibm and download it on: https://www.ibm.com/account/reg/us-en/signup?formid=urx-20028. You must choose the version for Linux x86-64.

Then, you need to install it within a container:
1. Run a container with docplex (the python framework for cplex) and jupyterhub:
```bash
docker run -v "/path/to/cplex/installer/directory:/home/jovyan/cplex" --name empty-docplex-notebook legraina/empty-docplex-notebook
```
2. Enter the container:
```bash
docker exec -it empty-docplex-notebook bash
```
3. Install cplex (for 12.8 for example):
```bash
./cplex/COSCE128LIN64.bin
```
You will then answer to the questions asked by the script. You need to give the path for the installation after accepting the license, YOU MUST ENTER /home/jovyan/CPLEX_Studio.
4. Once the installation is finished, you finally need to install cplex in your python environment:
```bash
cd CPLEX_Studio/cplex/python/3.6/x86-64_linux/ && python3 setup.py install
```

Now that cplex is installed in your container and ready to be used, we will save the image. First quit the container, with "ctrl+D" or by typing "exit". Then, commit the container:
```bash
docker commit empty-docplex-notebook docplex-notebook
```
You have now an image "docplex-notebook" ready to be used. You may finally launch the your jupyter server and play with docplex.
```bash
docker run -v "/path/to/some/exercices:/home/jovyan/work/exercices" -p "8888:8888" --name docplex-notebook docplex-notebook
```
You may add a volume with the command "-v": the idea is to load some existing notebooks within jupytherhub, for exemple some exercices. You can now connect to the jupytherhub and try the examples.

## DOCPLEX

MP documentation: https://ibmdecisionoptimization.github.io/docplex-doc/mp/index.html

CP documentation: https://rawgit.com/IBMDecisionOptimization/docplex-doc/master/docs/cp/index.html

Examples: https://github.com/IBMDecisionOptimization/docplex-examples
