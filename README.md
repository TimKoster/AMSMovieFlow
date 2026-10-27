# Purpose
Automatically run analysis on [Amsterdam Modeling Suite](https://www.scm.com/amsterdam-modeling-suite/) xyz movies using formatted input files and [SLURM](https://github.com/schedmd/slurm). Any engine is supported, but simple energy data scraping has only been developed for ADF and BAND engines, and EDA data scraping only for the BAND engine.

This is not clean, and the code is not great. I needed to efficiently calculate tons of data, and getting the script working as fast as possible was always the priority. 

### BAND-EDA
Reports the following EDA terms over the span of a reaction (or movement):

 * Interaction of system
 * Pauli repulsion
 * Dispersion
 * Elecrostatic interction
 * Orbital interaction

And these
 * Strain of first fragment 
 * Strain of second fragment
 * Strain of system

### ADF/BAND Simple Jobs
When properly configured, can automatically run a job on an ams.amv collection of xyz coordinates. There are some sample files included. Make sure to set dumb_scrape to true so it knows only to search for simple energies and not a full EDA

### Parallel SLURM running
You can either run every calculation of an ams.amv sequentially, or split them into seperate jobs and submit them all simultaneously (by setting parall_slurm_execution to true). Data is gathered either at the end for the sequential jobs, or after every job for the parallel running mode. There are no safeties for the parallel running mode, but what are the odds that multiple jobs finish at once? You can always resubmit the master job with only_regenerate_results set to true. 

### Installation
1. Download/clone the repository
2. Navigate to the project

`  cd LightFrag/`

3. Run the installation script
   
`  bash installation.sh`

4. Test your installation by running the sample input (LightFrag/sample_input/sample_bash)
   
`  sbatch sample_bash` on slurm or `bash sample_bash` (not recommended)

### Usage
1. Copy the sample_bash input file to the desired location for your calculations, as long as LightFrag is located in the root folder (cd ~).
2. Change the slurm, ams and engine settings in your input file to your desired settings. 
3. Add your .ams.amv (xyz movie) to the same directory as your input file
4. Configure the LightFrag settings in your input file
5. Run your input file with `sbatch whatever_your_input_file_is`
6. Analyse results. Raw results are displayed in the .out and a .csv file.

### EDA Workflow
To perform the fragment analysis, the input_file is split into `fragment_1`, `fragment_2` and `fragment_full` for every frame, using the .ams.amv coordinates and frames, and using the fragment_mapping 
defined in the input file.

The script compiles all results, using the strain_fragment_1 and strain_fragment_2 energies as basis, plotted over the bond_length defined in the input file. 
Running a LightFrag job also produces the following:
* `scripts/`: Folder containing the scripts that were run for every frame
* `frame_n`: For every frame, contains the ams output for the fragments. By default, these are pruned to only contain the ams.rkf and the ams.log for storage efficiency.

<img width="1810" height="865" alt="image" src="https://github.com/user-attachments/assets/e7c717ee-a5e1-4688-a448-a849cb5df3fe" />



