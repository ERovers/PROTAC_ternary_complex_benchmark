# PROTAC ternary complex benchmark
More details on how to run the protocols can be found here:
[link](https://www.sciencedirect.com/science/article/pii/S0076687923002288?via%3Dihub#sec0070)

If you decide to use the benchmark or protocols, please cite one of these papers:
```
@article{Benchmark Ternary complex prediction,
	title={Benchmarking methods for PROTAC ternary complex structure prediction},
	volume={64},
	DOI={10.1021/acs.jcim.4c00426},
	number={15},
	journal={Journal of Chemical Information and Modeling},
	author={Rovers, Evianne and Schapira, Matthieu},
	year={2024},
	month={Aug},
	pages={6162–6173}}

@article{Methods,
	title = {Chapter Ten - Methods for computer-assisted PROTAC design},
	series = {Methods in Enzymology},
	publisher = {Academic Press},
	volume = {690},
	pages = {311-340},
	year = {2023},
	booktitle = {Modern Methods of Drug Design and Development},
	issn = {0076-6879},
	doi = {https://doi.org/10.1016/bs.mie.2023.06.020},
	url = {https://www.sciencedirect.com/science/article/pii/S0076687923002288},
	author = {Evianne Rovers and Matthieu Schapira}}
```

## Final_input:

All the input files for the benchmark and reproducibility study

- Reproduction
	- Files for reproducing the results of Drummond et al. & Zaidman et al.
- PROTAC_screening
	- Files for crystal structure prediction benchmark

## Reproduction of results

To reproduce the results, the following steps need to be performed:

MOE:
- Prepare batch files by opening three_body_csearch_method4B.svl
- Run the screening using:
  `moebatch -mpu N -load three_body_csearch_method4B.svl -run method_4B_batch_three_body_method4B_XXXXX.svl`

ICM:
- Run the screening using:
	`PATH_TO_ICM_EXECUTABLE/icm -vlsc PATH_TO_ICM_SCRIPTS/protacModel.icm E3_name.ob Target_name.ob ligand_1.icb protac=PROTAC.sdf scsRad=8. number=NUMBER_ROW_LIGAND nrun=1 >& ligand_1.ou &`

Change "number=" to the row index of the PROTAC screened (each PROTAC needs to be screened separately)
Change "nrun=1" keep on one, but run command 5x

- To combine results, open ICM:
	`processPROTACsims "ligand_1*"`

PRosettaC:
- Download docker image from https://hub.docker.com/r/erovers/prosettac
- Download Rosetta from https://hub.docker.com/r/erovers/prosettac
- Run the screening using:
	`docker run --user 0 --network host --rm -v /DIRECTORY/:/data/ -v /ROSETTA_FOLDER/:/rosetta/ -v /OUTPUT_DIR/:/output/ erovers/prosettac:latest /data/bash_script.sh`

