Data Fields

ticket_loc:: ilYpoCagn1

ticket:: ilYpoCagn1Pri

tickethap:: ilYpoCagn1Hap

ticket_type:: darwin

latin:: Agonum fuliginosum

latin_again:: Agonum_fuliginosum

clade:: insects

scratch_space:: scratch124

scratch_space:: {asg: scratch124, darwin: scratch123}

Project_type:: [darwin, asg]

Possible_clade:: {xb: molluscs, i: insects, m: mammals, d: dicots, w: annelids, gl: fungi, f:fish, ng: nematodes, qe: arthropods}

## Step 1 - env set up
```
unset PERL5LIB
unset PYTHONPATH
export LD_LIBRARY_PATH=/software/python-3.7.4/lib/:${LD_LIBRARY_PATH}
export PYTHONPATH=/software/grit/lib/python3_lib:${PYTHONPATH}
export PERL5LIB=/software/grit/projects/vr-runner/modules:${PERL5LIB}
```

## Step 2 - Get data

Set up a pair of folders at:

```
mkdir /lustre/scratch123/tol/teams/grit/btk_runs/data/ilYpoCagn1Pri && mkdir /lustre/scratch123/tol/teams/grit/btk_runs/data/ilYpoCagn1Hap
```

To get the pipeline to work you need FASTA files and the pacbio run FASTA files.

These are generally in a location such as

```
/lustre/scratch123/tol/projects/darwin/data/insects/Agonum_fuliginosum/genomic_data/ilYpoCagn1/pacbio/fasta/
```

External Faculty data will be here:

```
/lustre/scratch124/tol/projects/external_curation/data/
```

And COPY to the pair of folders you made above and do not gunzip.

## Step 3 - Generate YAML

Get data trim the file names and don't gunzip them.

For both haplotypes

```
cd /lustre/scratch123/tol/teams/grit/btk_runs/data/ilYpoCagn1Pri

/usr/bin/perl /nfs/team135/yy5/btk_config/run-btkconfig_v2 +loop 60 -b -s ilYpoCagn1Pri -t 'Agonum fuliginosum' -r ilYpoCagn1Pri.fasta.gz -o . -z config_done &

cd /lustre/scratch123/tol/teams/grit/btk_runs/data/ilYpoCagn1Hap

/usr/bin/perl /nfs/team135/yy5/btk_config/run-btkconfig_v2 +loop 60 -b -s ilYpoCagn1Hap -t 'Agonum fuliginosum' -r ilYpoCagn1Hap.fasta.gz -o . -z config_done
```

Check the config for errors that WILL kill the pipe

## Step 4 - Run the pipeline

For btk 2.6.3 (new)

```
cd /lustre/scratch123/tol/teams/grit/btk_runs/data/ilYpoCagn1Pri

ASSEMBLY=ilYpoCagn1Pri TRANSFER=true bsub < /nfs/team135/yy5/btk_sig/run_pipeline_263.sh

cd /lustre/scratch123/tol/teams/grit/btk_runs/data/ilYpoCagn1Hap

ASSEMBLY=ilYpoCagn1Hap TRANSFER=true bsub < /nfs/team135/yy5/btk_sig/run_pipeline_263.sh
```

## Step 5 - REVISION

The server has been decommissioned, Guoying (gq2) has set the btk viewer up as a kubernetes server.

Primary

```
cd /lustre/scratch123/tol/teams/grit/btk_runs/result/ilYpoCagn1Pri

tar -xvf ilYpoCagn1Pri.tar

gunzip ilYpoCagn1Pri/*

cp -r ilYpoCagn1Pri /lustre/scratch123/tol/share/grit-btk-prod/blobplots/ilYpoCagn1Pri

chmod -R g+w /lustre/scratch123/tol/share/grit-btk-prod/blobplots/ilYpoCagn1Pri

gzip ilYpoCagn1Pri/*
```

Haplo

```
cd /lustre/scratch123/tol/teams/grit/btk_runs/result/ilYpoCagn1Hap

tar -xvf ilYpoCagn1Hap.tar

gunzip ilYpoCagn1Hap/*

cp -r ilYpoCagn1Hap /lustre/scratch123/tol/share/grit-btk-prod/blobplots/ilYpoCagn1Hap

chmod -R g+w /lustre/scratch123/tol/share/grit-btk-prod/blobplots/ilYpoCagn1Hap

gzip ilYpoCagn1Hap/*
```

## Step 6 - Post to Jira
```
BTK DONE
[Pri|https://grit-btk.tol.sanger.ac.uk/ilYpoCagn1Pri/dataset/ilYpoCagn1Pri/blob?plotShape=circle&zScale=scaleLog#Settings]
[Hap|https://grit-btk.tol.sanger.ac.uk/ilYpoCagn1Hap/dataset/ilYpoCagn1Hap/blob?plotShape=circle&zScale=scaleLog#Settings]

```

## Step 7 - Update the Site index

This is internal only so no bad actors can mess with it.
```
curl -s 'https://grit-btk-api.tol.sanger.ac.uk/api/v1/search/reload/testkey%20npm%20start'
```
