# Analyzing-Infrastructure-Data

Using Docker for development work means that applications run in containers and are therefore isolated from other system resources. This is useful in many applications. Here, we use this isolation to take a look at resource consumption for a very simple application.

This experiment begins with a series of simple questions:

   1. What size dataset is too large for a t2.micro to load into memory?
   2. What size dataset is so large that, on a t2.micro, it will prevent Jupyter from fitting different kinds of simple machine learning classification models e.g. a K Nearest Neighbor model? a Decision Tree model? a Logistic Regression?

### Configuration

We will be working with a Jupyter Notebook server being managed by the Docker daemon on a t2.micro running on AWS. We will use the jupyter/datascience-notebook image to run the server.

### Capturing System Usage

While conducting the experiment, we will capture resource consumption using the docker stats tool.
This command will be used to actively monitor system resource usage.

$ docker stats \
  --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}" 
  
We will capture the output to a file(stats-experiment-1.txt, stats-experiment-2.txt) that we can analyze the performance later.

### Experimental Datasets

We will be creating a series of datasets with a specified 𝑛×𝑝 size, where 𝑛 is the number of samples and 𝑝 is the number of features. To create these datasets we will use the make_classification function from the sklearn.datasets module.

dataset 	Shape of Feature Set
1 	      100×20
2 	      193×32
3 	      373×54
4 	      720×88
5 	      1389×144
6 	      2683×236
7 	      5179×386
8 	      10000×632
9 	      19307×1036
10 	      37276×1697
11 	      71967×2779
12 	      138950×4552
13 	      268370×7455
14 	      517947×12211
15 	      1000000×20000

### Experimental Process

We will use the following processes to conduct the experiments:

**Experiment 1**
1. Start docker stats capture to stats-experiment-1.txt
2. Create each dataset
3. Pickle each dataset
4. Remove dataset from memory
5. Restart the Jupyter Kernel

**Experiment 2**

1. Start docker stats capture to stats-experiment-2.txt
1. Load each pickled dataset
1. Fit and score a KNN model
1. Restart the Jupyter Kernel
