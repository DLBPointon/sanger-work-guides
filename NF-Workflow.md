# NF - Workflow
## By Damon-Lee Pointon (dp24)
Written in [obsidian.md](https://obsidian.md/).
For TreeVal working group. Found [here](https://github.com/DLBPointon/treeval_pipeline)

## A. Download the GitHub Repo
```bash
git clone https://github.com/DLBPointon/treeval_pipeline.git
cd treeval_pipeline
```

## B. Ensure you have test data

### 1. Test Dir
Mattheiu has a test data directory where we should keep test data. We'll need to contact him to see where he wants it going.

### 2. Tests
For each additional component added to the workflow, it should be tested with the .

A file describing what the test is for, locations of test data and all params needed including config_profile_name and config_profile_description

An example is saved as test_illumina.config: 
![[images/test_example.png]]

In the case of treeval we'll need an input for:
- current gEVAL yaml or future TreeVal yaml
- Any other data that isn't included here, could use GRIT ticket id to pull data from JIRA directly

As all of the to be written workflows will rely on the same data, we only really need:
- test_small.config
- test_large.config

test_small would be used for testing your workflow on it's own and large for larger scale workflow pipeline changes.

Each test will need to be entered into the nextflow.config file too as something like this, using the example config above:
```bash
profiles {
	test_illumina	{ include 'config/test_illumina.config' }
}
```

## C. Set your env
This initially follows the same method as creating you're module.
From [here](https://ssg-confluence.internal.sanger.ac.uk/display/TOL/Creating+your+Nextflow+nf-core+module).

```bash
#Configure shell for Conda (if not done automatically in your .bashrc)
source /software/treeoflife/miniconda3/etc/profile.d/conda.sh
 
#Load nf-core environment
conda activate nf-core_dev
 
#Load Singularity
export MODULEPATH=$MODULEPATH:/software/modules/
module load ISG/singularity
```


## D. Make your issue
Infrastructure team very much like atomicity and issue tracking. 

When beginning work on your workflow, first you should set out your plan in a workflow master GitHub Issue detailing everything you'll be doing.

Example from variant calling:
![[images/master_issue.png]]

This should make note of what data it will use, and what the output will be. It should also contain a list of modules that either need to be written or those that already exist as well as a link to their main.nf in nf-core-modules.

Then each item on that list should have then have a child issue for when you are working on that item.
For example:
![[images/child_issue.png]]

So in our case there should be:
1. TreeVal Pipeline Plan Issue #1
	1-1. Synteny Plan - Probably doesn't need a further child issue due to simplicitiy #2
	1-2. Gene Alignment Plan #3
		1-2-1. Building custom python for X #4
		1-2-2. Building module for BLASTN #5
		
Each issue on should link to it's parent on commit.

Another Example:
- Say i build my fancy BLASTN module my commit message would be
```bash
git commit modules/fancy_blast.nf -m 'Built fancy_blast, effects issues #5 #3 #1, closes #5'
```

GitHub will link it all up and close the referenced issue.

## E. CHECKOUT
Please don't work on main branch.
Think of a branch name that accurately describes what your working on (this is my prefered method, as we don't have to worry about multiple people working on it), you can use `dev` but make sure no one else is using it.
```bash
git branch -a #lists all branches
git checkout {Your_chosen_name} #Makes new branch
git branch # To double check your current branch
```

## F. Installing modules
If your workflow needs a module nf-core makes it easy, as long as your env is set up propper, see [[#C Set your env]].

```bash
cd treeval_pipeline
nf-core modules install
```

This will bring up a fancy lil terminal application, start typing the name of the module you want and it will bring up a drop down to choose from. Select the one you want and it will install the module into `modules/nf-core/TheModuleYouInstalled/main.nf | meta.yaml`.

From this point you can use them in your sub-workflow.

If not, in the repo then yy5 or a member of infrastructure is your best bet. Along with [this](https://ssg-confluence.internal.sanger.ac.uk/display/TOL/Creating+your+Nextflow+nf-core+module).

## G. Actually getting to write your SUBWORKFLOW now
So you have your example data, your env is set up and your issues are open you can finally start working on your workflow.

Your Sub workflows will be written here: `treeval_pipelines/subworkflows/local/{what_ever_you_want_to_name_your_workflow}`

In this style:

![[images/subworkflow_example.png]]

There is your workflow descriptor, module imports, workflow statement.

Your worflow statement will include your `take` which will pull specified data from the main workflow, as well as youre `main` and anything else you want to include. Each process in main should also have a descriptor.

Write and commit, job 1 of x done.

## H.  WORKFLOW
Now you need to edit the workflow file.
Open `treeval_pipelines/workflows/treeval.nf` 

Add an `include` for your subworkflow here:
![[images/include_example.png]]

You won't need to once again `include` your modules here, as that will be done in you're subworkflow.

What you will need to do is add your subworkflow to the bottom of `workflow TREEVAL`.
The multiqc/fastqc stuff will get removed as we don't need it. 

Input check will need to be changed significantly too. So for now, i'd suggest just testing directly on your subworkflow until the input_check can be altered.

treeval.nf is already `include`d in the main.nf so we don't need to modify that.

Our structure at this point is:
`YourModule.nf -> YourSubworkflow.nf -> treeval.nf -> main.nf`

Somewhat complicated, but it's all in the name of modularity!

## I.  Linting
Everyone's favourite topic... linting.

This needs to be in the actual terminal app, not in VSCode... VSCode does not like trying to run prettier through its own terminal. 🤷🏼‍♂️

Once you have written everything up, you should lint. nf-core uses prettier which is nice because it has an autocorrection function.

```bash
prettier -w .
```

This may or may not work.... i've told the infrastructure team. This might be another "i'm the issue"  but I installed this locally and lint locally as a work around... annoying but works.


## J. Testing
If and when you finish your linting, you need to run your tests on test_small and preferably test_large too, to double check.

Your command will look something like:
```bash
bsub -e error -o out -n 16 -q normal -M1500 -R'select[mem>1500] rusage[mem=1500] span[hosts=1]' 'nextflow run main.nf -entry NFCORE_TREEVAL -c nextflow.conf -profile test_illumina'
```

You'll need to adapt this for your own use.

If your tests fail, check the logs and have fun decyphering. Need help then ask in TreeVal, pipelines, or nextflow slack channels in that order is probably best.

Once solved you'll need to go back through [[#I Linting]] and [[#J Testing]] and hope you don't end up in a loop.

## K. commit
Now you have written something that works, is linted, passes the tests and outputs your final files. Congrats.

Add all of your new files:
```bash
git add {relative_path_to_new_file}
```

You can do this for all of your NEW files.

And now either:
```bash
git commit -am "All my work for issues #1 #3 #5, closes #5"
```

Or do it atomically:
```bash
git commit {relative_path_to_modified_file} -m "Editted for blah effects issue #1 #3 #5"
```
Just remember the `closes #5` in the last commit.

Either way is fine.

You'll notice that if you try to `git push` it will through an error because on GitHub your branch doesn't exist yet. The error message will explain what to do, literally just a line or two of commands to copy and paste.

## L. Merge branch
So you are ready to merge.

Go to GitHub and nav to your branch:
```bash
https://github.com/DLBPointon/treeval_pipeline/tree/{BRANCH NAME}
```

There should be a `Open Pull Request button`.

This will open a templated merge request thing that will need to be filled out. Delete the checklist where not applicable. Set the reviewer and `Create Pull Request`.

Job Done.

## M. Pat Self on Back
And then start again for next item on todo list.