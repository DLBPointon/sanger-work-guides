As of V2 there can be a few issues that occur.

# Data in the wrong place
Make sure data is in the blobtoolkit/data folder.

# Data files with incorrect naming scheme
NO _ at all in file names if it can be helped.

# If it keeps failing at BUSCO
Use the minimum datasets:

```
  - nematoda_odb10
  - metazoa_odb10
  - eukaryota_odb10
  - bacteria_odb10
  - archaea_odb10
```

# Taxon
If you need to manually fill out the taxon you can write the yaml with.

```
/software/bin/python-3.7.4 /nfs/team135/yy5/btk_config/write_yaml_v2.py -a ./assem.size -s xgBioGlab47Pri -r xgBioGlab47Pri.fasta.gz -t ./tax.txt -o ././config.yaml.part
```

# Cleaning up the BTK directory
rm -rf assem.size blobtoolkit/ .jobs/ btk* busco/ chunk_stat* config* *gz.* measure_done minima* tax.txt windowmaske*
