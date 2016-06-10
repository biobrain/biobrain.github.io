How to convert MRI images to 3D models in STL format
====================================================

I made this instruction mainly according to the article on [brainer.](https://brainder.org/2012/05/08/importing-freesurfer-cortical-meshes-into-blender/)

1. Use dcm2nii to convert the dcm file to nii file. For more detail see [mricron.](http://people.cas.sc.edu/rorden/mricron/dcm2nii.html)
2. For T1 structure, dcm2nii will generate three files, one is original file, reoriented file start with 'o' and croped file started with 'co'. Here I just used the original file.
3. Install and configure [FreeSurfer](http://surfer.nmr.mgh.harvard.edu) according to the official [guide](http://surfer.nmr.mgh.harvard.edu/fswiki/Installation).
4. Use FreeSurfer function recon-all to do the segmentation for structure images.

```bash
cd $SUBJECTS_DIR
recon-all -s subj1 -i anat.nii
recon-all -s subj1 -all
```

> The command in first line `cd` help us to navigate to the Subject Directory of FreeSurfer.
>
> The second line call for the `recon-all` function in FreeSurfer, the parameter `-i` means to import the T1 images named anat.nii to the Subject Directory and create a subject folder named subj1. You can change "subj1" to any other name.
>
> The third line used `-all` parameter to do the segmentation to the structure images. This step may take two days according to your computer.

5. Copy [seg2srf](https://github.com/ins-amu/scripts/blob/master/aseg2srf) file to current directory. Use aseg2srf to generate srf file for subcortical regions.

```bash
./seg2srf -s subj1

```
> There is many other parameter like `-i` could be set.
> You can just type ./seg2srf to see detail for the parameters.


6. Convert the srf files generated from seg2srf to obj file which could be imported into most of the modeling software.

This step you should copy srf2obj to current file and install [gawk](https://www.gnu.org/software/gawk/) on your computer. And also copy [srf2obj4blender](https://github.com/biobrain/3d-printer) to current directory. Then execute the following command.

```bash
./srf2obj4blender -s subj1

```
> This program will generate a blender directory into the subj1 directory. And all the obj files of brain tissues will be put into your $SUBJECTS_DIR/subj1/blender directory.

Finally you can use blender to import the obj files in $SUBJECTS_DIR/subj1/blender, note that `subj1` could change to any subject name in your experiment.

Done.
