# Preprocessing


For Task 1, all image data has only been defaced using PyDeface as a preliminary attempt with manual interventions when the automatic process was not sufficient.

For Task 2, low field trio of orthogonal images which have matching high field T2s were first processed via the ANTs multivariate template construction script (http://stnava.github.io/ANTs/).  The following options were used with the latest ANTs toolbox installation on a CentOS7 based high performance computing cluster. 

${ANTSPATH}/antsMultivariateTemplateConstruction2.sh -d 3 -o ${CISO_file} -i 4 -g 0.2 -j 32 -c 2 -s 3x2x1x0 -q 100x70x50x10 -n 1 -r 1 -l 1 -m MI -t BSplineSyN[0.1,26,0] -z ${template} ${trio_folder}

Template is 4.5-8.5 year old T2 template from  the McConnell Brain Imaging Centre’s NIH Pediatric Database https://www.mcgill.ca/bic/software/tools-data-analysis/anatomical-mri/atlases/nihpd2 which has been resampled to 1.5 mm isotropic using AFNI’s 3dresample https://afni.nimh.nih.gov/pub/dist/doc/program_help/3dresample.html 

The subsequent CISO_file was then 9-pt linearly coregistered to its matching high field T2 (the same T2 which was used as the background anatomical image for the manual hippocampi segmentations) via FLIRT from the FSL toolbox (https://fsl.fmrib.ox.ac.uk/fsl/fslwiki) using the following options

flirt -in ${CISO_file} -ref ${HF_file} -out ${coregistered_output} -cost mutualinfo -interp sinc -sincwindow hanning -sincwidth 14 -dof 9

HF_file is the matching 1mm isotropic high field T2 image and the coregistered_output is the subsequent coregistered CISO file
