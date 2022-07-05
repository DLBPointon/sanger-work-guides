# runChar for curators v2

Welcome to curators guide to running Bionano's runChar.

## Step 1 - Setting up your environment

Navigate to the working directory of the organism which should look like this:

#### GenomeArk/VGP
```
/lustre/scratch123/tol/teams/grit/geval_pipeline/geval_runs/VGP/bCorHaw1_1/
```

or

#### DTOL
```
/lustre/scratch123/tol/teams/grit/geval_pipeline/geval_runs/DTOL/{ORG OF INTEREST}

```

Older tickets have another location, but we shouldn't see many of those anymore.

## Step 1.2
Now to load all of the variables you might need you should run:
```
conda activate perl5.18
```

and then:
```
souce run_output/dp24_20-08-21_bash.conf
```
The .conf file above might start with either dp24 or yy5 depending on who built the geval but will look similar to the above.

## Step 1.3
Now set up the Bionano directory for your round 2 analysis:
```
mkdir $GEVALWD/bionano/round2 && cd $GEVALWD/bionano/round2
```

Check for the CMAP file exists:
```
ls $BN_CMAP
```

If you get a doesn't exist, then it is because the cmap directory is generated via a bunch of other variables that arn't important here.

## Step 1.4 - Skip if you have the CMAP
It will have generated something like this:
```
/lustre/scratch123/tol/teams/grit/geval_pipeline/grit_rawdata/vgp/data/birds/Corvus_hawaiiensis/genomic_data/bCorHaw1/bionano/bCorHaw1_Saphyr_DLE1.cmap
```

Where as the cmap probably lives in a directory looking like:
```
/lustre/scratch123/tol/teams/grit/geval_pipeline/grit_rawdata/vgp/data/birds/Corvus_hawaiiensis/genomic_data/bionano/bCorHaw1_Saphyr_DLE1_3680695.cmap
```

It's a pretty minor difference but it does happen and you should be aware.

From now on however GenomeArk data should be found here:
```
 /lustre/scratch116/tol/projects/genomeark
```

## Step 1.5 - Move your files locally

Now we move the cmap file here:
```
cp $BN_CMAP .
```

Now your PRODFILE or your curated assembly file:
```
cp {YOUR FASTA} .
```

## Step 2 - Running runChar
At this point we have the biggest divergence from the standard geval pipeline. The runChar command, it relies on there being only a .cmap file and a .fa file being in the folder at the moment:
```
~wc2/gitlab/geval/dev_modular_pipeline/newschool_farm5_manual/run_bionanoalign.sh *cmap *fa $BN_ENZYME $BN_CMAP_TYPE
```

You should get an email when it finishes.

## Step 3 - Download
At this point 3 files need to be uploaded to the Bionano site.
The easiest way to do this is probably to first download them to your machine.
So:
```
cd alignref/
```

And make sure the following files are here:
```
myRunChar_DLE1_q.cmap -> Query Map
     
myRunChar_DLE1_r.cmap -> Reference Map

myRunChar_DLE1.xmap -> Alignment Map
```

Now to download them open a new terminal and make a folder for your new files.

```
mkdir {TICKET}_maps

list=('myRunChar_DLE1.xmap' 'myRunChar_DLE1_q.cmap' 'myRunChar_DLE1_r.cmap')

for i in ${list[*]}; do
    scp {USER}@tol.internal.sanger.ac.uk:{ALIGNREF FOLDER}/$i {TICKET}_maps/; done
```

Using the above bCorHaw example this would look something like:

```
mkdir this_maps

list=('myRunChar_DLE1.xmap' 'myRunChar_DLE1_q.cmap' 'myRunChar_DLE1_r.cmap')

for i in ${list[*]}; do
    scp dp24@tol.internal.sanger.ac.uk:/lustre/scratch123/tol/teams/grit/geval_pipeline/geval_runs/VGP/bFalPer1_1/bionano/runChar/alignref/$i bFalPer1_maps/; done
	
```

Annoyingly you'll still need to put your password in for every file that downloads.

Now that folder should have your maps ready for upload to the bionano site.

## Step 4
In your browser head to, must be on sanger internet:
```
http://web-wwwbionano-01:8000/
```

Log in and there should already be a project folder for the organism your working on. 

Go into that folder and there will be an import button along the top.

Click import, select alignment and next.

The next page should be fairly self-explanatory
The name should relate to the organism so in this cause bcorhaw1_curation2.

Sample should be the organism name again, selected from the drop down menu.

Make sure Anchor to genome map is selected

And the next 3 boxes relate to the files we downloaded earlier.

upload the respective file and hit next.

It should now be uploading the files and when done you will have your alignment.

## Congrats