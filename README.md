# This is a quick demo on how to set up and share virtual environments in Python and R 

## Python using conda
Install `anaconda`/`miniconda` on your computer: https://www.anaconda.com/

**Creating and sharing envronment recipe** 
Firstly, a comprehensive guide can be found here: https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html 
This is just a quick-refernce resource. 

Create a `yml` file with environment specifications. You can use any editor. Save it as `my_env.yml`, replacing `my_env` with the name of your environment. 
Here is an example of such `yml` file: 

```yml
name: my_python_env
channels:
  - conda-forge
  - defaults
dependencies:
  - python=3.10
  - jupyterlab==3.2.5
  - matplotlib==3.3.0
  - pandas==1.4.2
  - numpy==1.20.2
  - seaborn==0.11.2
```

Save the file in your project repo and commit ( `git commit *.yml`) and push (`git push`).

**Turning yml file to conda environment**

Set up env 

```bash
conda env create -f my_python_env.yml
```

Activate the environment 

```bash
conda activate my_python_env
```

Add to Ipython kernels 

```bash
ipython kernel install --name "my_python_env" --user
```

You can then install packages using pip/conda, to take snapshot of the environment use:

```bash
conda env export --no-builds | grep -v "prefix" > my_python_env.yml
```

In practice, this means that a code just needs to come with a `yml` file, and, based on this file, anyone should be able to reproduce the analysis. 

**Alternative: Creating an environment using `virtualenv`/`pyenv`**

Conda sometimes has issues with re-creating environments (the more I look into this issue the less I understand it).  If that happens, there are two alternatives: 
1) recreate the env manually using `conda install  <<packagename>>` or 'pip install <<packagename>>`
2) Use a requirements file (e.g., `requuirements.txt`), if provided. 

First, make sure that you have `virtualenv` installed in your local python. 

```bash
pip install virtualenv
```
Use virtualenv to create new environment

```bash
virtualenv my_python_env (called 'venv' here)
```
Note that this creates a `my_python_env` folder with all packages being installed there. 
Suggestion: Do not commit this folder. Instead, include it in `.gitignore` file. 

Tell the env which python kernel it should be using: 

```bash
virtualenv my_python_env --python=/usr/bin/python3.7
```

Activate env

```
source my_python_env/bin/activate 
```
Once it's activated, upgrade `pip`. 

```bash
pip install --upgrade pip
```

You can then directly install packages using `pip` (e.g. `pip install matplotlib`). 

**Sharing and reproducing an environment** 

Save information about all packages to `requirements.txt`, commit and push. 

```
pip freeze >> requirements.txt
``` 

Anyone can then recreate the environment by following the steps above plus

```bash
pip install -r requirements.txt

```


### R 

To share computational environments in R, we will be using `renv`. Comprehensive description here: https://rstudio.github.io/renv/articles/renv.html

If you don't have it, install `renv` and `here` packages. 

```r
install.packages("renv", "here")
```
**Create renv** 

Go to the folder in which you want to create `renv`. I recommend using `here::here()` to check, and if necessary to use `here::i_am(".root_file")` (where `.root_file` is a hidden file located in the project root). 

Initialize the environment

```r
renv::init()
```
This creates a `renv.lock` file containing the necessary packages and their dependencies (it's recommended to only interact with this file using `renv`, manual changes can break things). 

You can install the necesssary packages using `renv::install()` or 'install.packages()`

To create a snapshot of the state of the env use, this will update the `renv.lock` file. 

```r
renv::snapshot()
```
One can now commit the `renv.lock` file. 

**Restoring environment**

Go to the folder containing `renv.lock` and simply restore the environment 

```r
renv::restore() 
```
Example Rmd script using renv: https://github.com/ozika/trait-anxiety-and-state-inference-zika2022/blob/master/scripts/stats_main.Rmd
















2. Clone the repo containing the  repo
```
git clone git@github.com:ozika/demo-virtual-envs.git
```
3. Re-create the  
