# PROTAC docking and virtual screening benchmark

## Final_input:

All the input files for the benchmark and reproducibility study

- Reproduction
	- Files for reproducing the results of Drummond et al. & Zaidman et al.
- PROTAC_structure
	- Files for crystal structure prediction benchmark
- PROTAC_screening
	- Files for PROTAC virtual screening benchmark
- CRYSTAL_input
	- Files for WDR5 PROTAC screening using PDB: 7JTP and 7Q2J as input

## Final_results:

All the raw data files and analyzed data files. Including scripts to perform analysis.

- Reproduction
	- Analyzed data files for the reproduction
- CPU_times
	- CSV files with CPU times for runs
- PROTAC_structure
	- Analyzed data files + analysis for the Crystal structure prediction benchmark
- PROTAC_screening
	- Analyzed data files for the PROTAC virtual screening benchmark
- CRYSTAL_input
	- Analyzed data files for PROTAC virtual screening using crystal structures as input (WDR5)
- Conformation_cluster
	- Analysis of conformational clusters (using raw data files of ICM)			
- lysine_analysis
	- Analysis of lysine distances to multiprotein degradation complex (using raw data files of ICM)


## Reproduction of results

To reproduce the results, the following steps need to be performed:

- paste the analysis script in the folder (e.g. reproduction/PROTAC_structure/PROTAC_screening/etc.). The results of the screens should be in folders of the respective complex (e.g. VHL-BRD4BD1/CRBN-BRAF/etc.). The scripts can be run with the commands below.

MOE:
- Replace in method_4B_batch_three_body_method4B_XXXXX.svl "DIRECTORY" with the absolute path to the directory
- Run the screening using:
  `moebatch -mpu N -load three_body_csearch_method4B.svl -run method_4B_batch_three_body_method4B_XXXXX.svl`

ICM:
- Run the screening using:
	`PATH_TO_ICM_EXECUTABLE/icm -vlscluster protacModel.icm E3.ob TARGET.ob ligand_1.icb protac=PROTAC.sdf scsRad=8. number=1 nrun=1 >& ligand_1.ou &`

Change "number=" to the row index of the PROTAC screened (each PROTAC needs to be screened separately)
Change "nrun=1" keep on one, but run command 5x

- To combine results, open ICM:
	`processPROTACsims "ligand_1*"`

PRosettaC:
- Download docker image from https://hub.docker.com/r/erovers/prosettac
- Download Rosetta from https://hub.docker.com/r/erovers/prosettac
- Run the screening using:
	`docker run --user 0 --network host --rm -v /DIRECTORY/:/data/ -v /ROSETTA_FOLDER/:/rosetta/ -v /OUTPUT_DIR/:/output/ erovers/prosettac:latest /data/bash_script.sh`

## Analysis of results

To analyze the results, the following steps need to be performed:

- paste the analysis script in the folder (e.g. reproduction/PROTAC_structure/PROTAC_screening/etc.). The results of the screens should be in folders of the respective complex (e.g. VHL-BRD4BD1/CRBN-BRAF/etc.). The scripts can be run with the commands below.

### Analysis PROTAC structure benchmark

MOE:
- PDB files of the complex are saved with: create_pdb_files.txt
	`PATH_TO_MOEBATCH_EXECUTABLE/bin/moebatch -mpu 3 -run create_pdb_files.txt`
- An ICM script is run to calculate rmsd, rmsd_protac, rmsd_CH, rmsd_interface: script1.icm
	`PATH_TO_ICM_EXECUTABLE/icm -vlscluster script1.icm`

ICM/Haddock/PRosttaC/AlphaFold
- An ICM script is run to calculate rmsd, rmsd_protac, rmsd_CH, rmsd_interface: script1.icm
	`PATH_TO_ICM_EXECUTABLE/icm -vlscluster script1.icm`


### Analysis PROTAC screening benchmark

MOE:
- Create an csv files of with all the output "4B_DCP_Summary_XXXX.txt"
- Match up the ligand IDs with activity (see example "PROTAC_MOE2.csv")

ICM/PRosettaC:
- Open ICM and run script1.icm in the directory ("ICM" or "PRosettaC")
	`PATH_TO_ICM_EXECUTABLE/icm -vlscluster script1.icm`


### Analysis crystal input

MOE:
- Create an csv files of with all the output "4B_DCP_Summary_XXXX.txt"
- Match up the ligand IDs with activity (see example "PROTAC_MOE2.csv")


### Analysis conformation_cluster

ICM:
- Open ICM and run the script cluster_centers.icm in the directory
	`PATH_TO_ICM_EXECUTABLE/icm -vlscluster cluster_centers.icm`
- Run the script cluster_conformations.icm in the same directory after the cluster_centers.icm is finished
	`PATH_TO_ICM_EXECUTABLE/icm -vlscluster cluster_conformations.icm`


### Analysis lysine distances

ICM:
- Open ICM and run the script cluster_centers.icm in the directory
	`PATH_TO_ICM_EXECUTABLE/icm -vlscluster VHL_script.icm`
- Run the script cluster_conformations.icm in the same directory after the cluster_centers.icm is finished
	`PATH_TO_ICM_EXECUTABLE/icm -vlscluster lys_analysis.icm`


