# X-ray_spectral_modelling_for_Chandra_data (Under Construction)

#### This can be mainly used for Spectral Modelling on Chandra X-ray Observatory data observations on ACIS sources with no gratings. Also it searches for redshift informations in case of extragalactic objects from NED database. 
#### The key thing to remember is, this is created for personal use with necessary changes in the available codes. While doing spectral study of many numbers of AGNs during my Master's project on "Detailed timing and spectral study of some newly identified AGNs from Chandra Source Catalog", I wrote these codes to aid my work. These are nothing for general use and these only served for my purpose.

#### There are 5 different steps after downloading the data
 	1. Data Reduction and creating Source region files (Code 1)
	2. Spectrum Extraction (Code 2)
	3. Looking for galactic column density values (Code 3)
	4. Looking for redshift for the given source, If redshift is not measured by some other missions then it will return a null values (Code 4)
	5. Spectral Modelling (Code 5)

#### The workflow is as follows
	1. After downloading the data from https://cxc.cfa.harvard.edu/cdaftp/byobsid/ each and
	every observation id will constitute primary and secondary directories. Although primary
	directory directly provides you the evt2 files for further data reduction and scientific analysis,
	it is better to reprocess them using "chandra_repro" script of CIAO to apply the latest caldb files.
	This program will perform this task for all those sources. But for that, it needs some details of the sources.
	It will first go to the source folder where the observation was mentioned.
	For this inside the source folder other than source file one needs a csv file
	containing the observation ids (can be multiple) of the source on which chandra_repro is needed.
	Then it will create a source region and background region by calculating the psf of the source of interest in the reprocessed event file.
	For this it will need the ra and dec of the sources from a file named 'details.txt'. It will calculate the psf through "psfsize_srcs" script of CIAO.
	Here in this case the percentage of energy of the total psf (ecf) can be manually entered as desired.
	By default it takes 92% ecf. The source region file with name "source_region.reg" will be created by a circular psf of radius r_psf
	obtained by the script around the given ra dec of the source and a background region (named as "background_region.reg")
	with annulus of inner radius of 1.2*r_psf and an outer radius of 2*r_psf. Then, it will open ds9 to display the region files and
	save it as .jpg in a folder named "regions" outside the source folder. At the end this code will create a csv file containing a
	list of source folder names and there corresponding observation ids named as "acis_sources.csv".
	The saved pictures of the region files should be checked manually to confirm for the created region.
	If any discrepancy happens then that source and observation id should be tackled manually and
	that source and obsid should be removed from the output csv file in order to go ahead to the further steps of the automation.
	It will also create a text file "data_red_problematic.txt" for which the code face difficulty in any of its tasks. Those also should be taken care of manually

	2. Now that we have the "acis_sources.csv", where, source folder name and corresponding observation ids are saved we can extract the spectrum using Code 2.
	In code 2 it will go to the "repro" folder of each sources obsid and from the event file it will extract the spectrum using "specextract" task.
	The parameters of the task can be manipulated by the user as needed. The default grouping is such that there is atleast "grp" counts per bin. AFter doing that it
	will calculate the number of bins (data points) in that grouping condition. If the number is more than "min_bins" then it will finalize that grouping.
	Otherwise it will reduce the number of counts per group by 5 and it will continue reduce the number in same manner untill the no. of bins becomes more than "min_bins",
	however the grouping should not go below 15 counts per bin. If 15 counts reached per bin, but the number of bins is still less than "min_bins" it will group
	such that there are atleast one count per bin. The default value of the initial grouping is 20 and "min_bins" is 15.
	After extracting and grouping the spectrum it will store the information in "spec_info.csv" where the source folder name, corresponding obsids and the grouping number.
	It will also create a text file "problematic_source.txt" where the source folder name and obsid for which the spectrum could not be
	extracted, will be saved.
	
	3.
