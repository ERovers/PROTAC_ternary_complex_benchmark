# PROTAC docking and virtual screening benchmark

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

