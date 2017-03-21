# Research module template

The goal of this template is to layout Python code in a module format, using an Anaconda environment, to enable R&D on a self-contained project.

The `setup.py` file is used to create a module (to allow easy imports).

# Things you should do an change

* Create a local Anaconda environment (shown below)
* Activate the Anaconda environment
* Edit `setup.py` to change `research_project` to a relevant module name (you'll be doing `from research_project import xyz` in your code so make `research_project` something useful to you)
* Add a folder inside `research_project` perhaps with a date (e.g. `research_project\20170402_my_first_idea`) and add your Notebooks inside that folder
* Inside the Notebooks you can do `from research_project.utilities import utility` to share generic functions amongst your Notebooks
* Export your Anaconda environment to the `environments` folder
* Update the README so a collaborator can rebuild their environment from your environment so they can collaborate on the same code

There's a _caveat_ below about exporting an environment.

## Setting up an Anaconda environment

Build an Anaconda environment for this project, this example uses Python 3.6 and adds jupyter and sklearn.

`~/anaconda3/bin/conda create -n research_module_layout_template python=3.6 jupyter scikit-learn`

Activate the environment using:

`source ~/anaconda3/bin/activate research_module_layout_template`

Now you have a fresh Anaconda environment. It doesn't know about the module structure in `research_project` yet so you can't share code (yet) amongst your Notebooks.

## Adding _this project_ to the Anaconda environment to enable `import` with your own modules

Take a look in `anaconda3/envs/research_module_layout_template/lib/python3.6/site-packages` and you'll see your Anaconda environment's libraries.

We want to add a link from this folder _back to our local development folder_ so that everything we work on can access the `utilities` folder in our development folder, to enable code sharing.

In the following `setup.py` line note **develop** and not *install*.

From the root of this project run `python setup.py develop`, the output will look like:
```
running develop
running egg_info
writing research_project.egg-info/PKG-INFO
writing dependency_links to research_project.egg-info/dependency_links.txt
writing top-level names to research_project.egg-info/top_level.txt
reading manifest file 'research_project.egg-info/SOURCES.txt'
writing manifest file 'research_project.egg-info/SOURCES.txt'
running build_ext
Creating /home/ian/anaconda3/envs/research_module_layout_template/lib/python3.6/site-packages/research-project.egg-link (link to .)
Adding research-project 0.1.0 to easy-install.pth file

Installed /home/ian/workspace/personal_projects/kaggle/research_module_layout_template
Processing dependencies for research-project==0.1.0
Finished processing dependencies for research-project==0.1.0
```

Take a look in the above folder again, you'll now see something like:

```
-rw-r--r--  1 ian ian     78 Mar 21 12:02 research-project.egg-link
drwxr-xr-x 28 ian ian  12288 Mar 21 11:46 sklearn
...
```

If you read that file you'll see that it adds our development folder to the search path:
```
anaconda3/envs/research_module_layout_template/lib/python3.6/site-packages $ more research-project.egg-link
/home/ian/workspace/<snip>/research_module_layout_template
.
```

If you made a mistake and missed **develop** above run `pip uninstall research_project` inside your activated Anaconda environment and it'll delete the module. If you miss **develop** then it'll _copy_ your folder structure into `site-packages` and at this stage it'll be empty.

# Create your first research Notebook and use the `utilities` folder

```
cd research_project # you might have renamed this
mkdir 20170402_my_first_idea
pwd # research_module_layout_template/research_project/20170402_my_first_idea
jupyter notebook # starts Notebook in our working folder
# open `example_working_notebook` in this folder and run it
```

You'll see `from research_project.utilities import utility` which imports our utility module and then we call `utility.hello_world()` (this is a dummy utility function).

You can add your own shared code in this folder to new modules, they can be imported between your Notebooks.

As you're inside a fully working Anaconda environment you can using `ipython` (if you prefer the command line to Jupyter Notebooks) to do the same import.

You can also use `conda install` and `pip` to add your favourite projects.

# TO ADD exporting and rebuilding an environment that you've made already
