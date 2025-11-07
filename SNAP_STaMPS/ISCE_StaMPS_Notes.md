# Step 0: activate the virtual environment that includes ISCE2 software

Assumung that you have processed the data with SLC workflow previously

remember to modify path in ~/.bashrc

`
export ISCE_STACK=$HOME/tools/src/isce2/contrib/stack/
export ISCE_TIMESERIES=$HOME/tools/src/isce2/contrib/timeseries
export PYTHONPATH=${PYTHONPATH}:${ISCE_STACK}:${ISCE_TIMESERIES}
export PATH=${PATH}:${ISCE_STACK}/stripmapStack:${ISCE_TIMESERIES}/bin
`

# Step 1: crop area of interest

## 1.1
go to folder `/home/davidncu/tools/src/isce2/contrib/timeseries/prepStackToStaMPS/bin/` to modify the content of `run_SLCcropStack.csh`

## 1.2
in the file `run_SLCcropStack.csh`, modify `data_path` and `proc_dir`

- `data_path` : is where your stackStripMap/stackSentinel output files are located

- `proc_dir` : location of output files after cropping

- **Note**: you can copy the output of stackStripmap/stackSentinel to a separate folder for further processing

## 1.3

**(Only for stackStripmap output)** Before going to next, please open script `crop_rdr.py` located in the same folder `tools/src/isce2/contrib/timeseries/prepStackToStaMPS/bin/`, then modify the following lines:

	- parser.add_argument('-lat'......
	
	- parser.add_argument('-lon'......
	
- change the extension from `.rdr.full` to `.rdr`)

come back to file `run_SLCcropStack.csh`
- after `$proc_dir` there are 3 original coding lines:

`
ls -1 $data_path/geom_reference/*.full  > $proc_dir/geomFiles2crop.txt
ls -1 $data_path/SLC/2*/2*.slc.full > $proc_dir/slcFiles2crop.txt
ls -1 $data_path/baselines/2*/2*.full.vrt >  $proc_dir/baselineFiles2crop.txt
`

- for Stripmap data, please change them to:

`
ls -1 $data_path/geom_reference/*.rdr  > $proc_dir/geomFiles2crop.txt
ls -1 $data_path/SLC/2*/2*.slc > $proc_dir/slcFiles2crop.txt
ls -1 $data_path/baselines/2*/2*.full.vrt >  $proc_dir/baselineFiles2crop.txt
`

- save the script and exit
	
## 1.4

come back to the content of the file `run_SLCcropStack.csh`, then modify the coordinates (SNWE) after `crop_rdr.py -b` to define your area of interest

## 1.5

run `run_SLCcropStack.csh` to crop the data

# Step 2: prepare StaMPS-friendly input files

## step 2.1:

in the folder where you want to do `PS processing`, create a text file named `input_file`, with the following cotent:
>
> source_data slc_stack

> slc_stack_path /home/davidncu/ISCE_MintPy/StaMPS_Workspace/TSX_Run013_PS/tsx013_crop/SLC/
>
> slc_stack_geom_path /home/davidncu/ISCE_MintPy/StaMPS_Workspace/TSX_Run013_PS/tsx013_crop/geom_reference/
>
> slc_stack_baseline_path /home/davidncu/ISCE_MintPy/StaMPS_Workspace/TSX_Run013_PS/tsx013_crop/baselines/
>
> slc_stack_reference 20230718
>
> range_looks 10
>
> azimuth_looks 5
>
> aspect_ratio 2
>
> lambda 0.031
>
> slc_suffix
>
> geom_suffix

modify the parameters depending on your image data

if you work with stackStripmap, keep the above settings

if you work with stackSentinel, the following one should be changed:

> lambda 0.056
>
> slc_suffix .full
>
> geom_suffix .full
>

## step 2.2:

go back to the folder where you want to run your PS processing, run
> make_single_reference_stack_isce

## step 2.3:

Go to the folder after running previous step, maybe name `INSAR_YYYYMMDD` with `YYYMMDD` is your reference date, defined above.

then run `mt_prep_isce 0.4 5 5 100 200`, in which
- 0.4 = amplitude dispersion index
- 5 = number of patches in range (default: 1)
- 5 = number of patches in azimuth (default: 1)
- 100 = overlapping pixels between patches in range
- 200 = overlapping pixels between patches in azimuth

The number of patches you choose will depend on the size of your area and the memory on your
computer. Generally, patches containing < 5 million SLC pixels are OK.

After running this step, the program will generate PATCH_xxx folder


# Temporary Notes:

## Modification 1:

(2024/11/12) Some modifications to deal with errors when running `stamps(1,1)`:

1. reinstall gcc-7 and g++-7
	a. sudo add-apt-repository 'deb http://archive.ubuntu.com/ubuntu/ focal main universe'
	b. sudo apt-get update
	c. sudo apt-get install gcc-7 g++-7
	d. sudo add-apt-repository --remove 'deb http://archive.ubuntu.com/ubuntu/ focal main universe'
	e. gcc-7 --version
	f. g++-7 --version

2. rename all `*.c` files in folder `~/StaMPS-4.1-beta/src/` to `*.cpp`
3. modify the content of `Makefile` (a copy is saved in NAS storage)


After all, I still get the error:

> Error using load
> Unable to find file or directory '../master_day.1.in'.
>
> Error in ps_load_initial_isce (line 65)
> master_day=load(masterdayname);
>           ^^^^^^^^^^^^^^^^^^^
> Error in stamps (line 280)
>                    ps_load_initial_isce(data_inc)
>                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
>

After checking the website https://aria.atlassian.net/wiki/spaces/ARIA/pages/18317313/Displacement+Time+Series+using+Persistent+Scatterers

I found the problem is at the filename, `mt_prep_isce` produce a reference_day.1.in`, not `master_day.1.in`

Therefore, I try modifying the source `ps_load_initial_isce.m` located in `~/StaMPS-4.1-beta/matlab/`, replace the following line:

> masterdayname=['master_day.1.in'];  % YYYYMMDD

by a new one:

> masterdayname=['reference_day.1.in'];  % YYYYMMDD

## Modification 2:

install `Signal Processing Toolbox` for Matlab

First of all, need to run the following command:

> sudo chown -R yourusername:yourusername /usr/local/MATLAB/R2024b

then launch Matlab, click to Add-Ons, then search and install the package

## Modification 3:

install `Statistics and Machine Learning Toolbox` to run step 7


# StaMPS step 2 parameters:

Remember to change `select_resset_gamma_flag` to 'n' to save processing time

# Notes

You can modify the content of `patch.list` to decide which PATCH you would like to process. For example, if there is one specific PATCH that you have already done processing (from step 1 to step 4), and do not want to repeat it, you can remove that PATCH from the list


# SB Processing in StaMPS

## Step 1:

Assuming that you have performed PS process previously, and every function or software were on set.

First, go into folder `INSAR_YYYYMMDD`, run `mt_extract_info_isce`, the following file will be generated:
- slc_osfactor.1.in
- master_day.1.in
- heading.1.in
- day.1.in
- bperp.1.in

However, there are errors with `master_day.1.in` and `heading.1.in`, so it will better to copy the value of heading.1.in before running `mt_extract_info_isce`, then paste the value into already-created file. For the `master_day.1.in` please take a look at `Modification 1` below.

## Step 2:
run `sb_find(rho_min, Ddiff_max, Bdiff_max);` until you satisfy with the output baseline plot
- rho_min: minimum coherence based on the maximum temporal (Ddiff_max) and perpendicular (Bdiff_max) decorrelated baselines

More connections can be made by reducing rho min, or individual connections can be added by editing `small_baselines.list`, which is created by `sb_find`.

The connections in small baselines.list can then be plotted with: `plot_sb_baselines`

## Step 3:
To create the small baseline interferograms listed in `small_baselines.list` in the
`INSAR_master_date` directory, run: `make_small_baselines_isce` in command prompt with current directory is `INSAR_YYYYMMDD` (not in Matlab

After running this, a folder named `SMALL_BASELINES` will be created within folder `INSAR_YYYYMMDD`

## Step 4:
Within the `SMALL_BASELINES` directory, run `mt_prep_isce 0.6 3 2 50 200`, in which 0.6 is the `amplitude` *difference* `dispersion` $D_{\Delta A}$ (more details in *A multi-temporal InSAR method incorporating both persistent scatterer and small baseline approaches*)



### Modification 1:

In the file `ps_load_info.m`, modify `masterdayname=['master_day.1.in'];` by `masterdayname=['reference_day.1.in'];`

### Modification 2:

In the file `sb_load_initial_isce.m`, modify `masterdayname=['master_day.1.in'];` by `masterdayname=['reference_day.1.in'];`