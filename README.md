## Quantification of human nuclear phenotypes   

##### Requirements    
You will need Fiji (ImageJ) to estimate certain starting parameters before you attempt adapting/building the pipeline. 
Download and install CellProfiler from: http://www.cellprofiler.org/. The release used to build the pipeline below is: 2.1.1_6c2d896 (provided). 
Make sure you follow a pattern in naming your images and folders. This pattern is then used to both find all images of interest and to extract experimental metadata.
To load any of the pipelines developed here, use: “File -> Open project” for files with an extension cpproj and “File -> Import -> Pipeline from file” for files for files with an extension cppipe.
The pipeline below takes ~2.5 hours to analyze 149 TIFF images on MacBook Pro, 8GB RAM, 1.3 GHz Intel Core i5 processor (4 CPUs).

##### Building a pipeline for segmentation and quantification of human nuclei based on nuclear lamina staining only  
- extract individual optical slices from the original LSM z-stacks using custom ImageJ macro (it should be noted that the CellProfiler can deal with stacks in lsm format); the macro (batch_split_Z_stack.ijm) can be found at: 
- rename images to allow for extraction of metadata from image names (open Terminal, change directory by typing ‘cd’ and dragging and dropping the folder where your images live, hit Enter, then type: rename 's/R3_4/R3-4/' *); you can also reorganize the data, but this is the fastest way, and will not influence the way you organize your data in future
- start Cell Profiler
- load an existing pipeline with  “File -> Open project” for files with an extension cpproj and “File -> Import -> Pipeline from file” for files for files with an extension cppipe

*Below are some steps to have in mind when you try to build/re-build/change pipelines*    

- in **Image** module of CellProfiler it was specified that only files in tiff format should be analyzed
- regular expression for identification of images and extracting metadata was set to:  *^(?P<ExperimentID>.*)_(?P<Treatment>[a-z,A-Z,0-9,-]+)_(?P<Stain>.*)_(?P<ImageID>[0-9]+).tiff*
- given the quality of staining and imaging, the prepossessing steps were limited to  smoothing with Gaussian filter to limit the impact of noise on segmentation steps
- segmentation used maximum correlation thresholding method {Padmanabhan:2010wg} (suitable for images with sparse foreground signal densities, e.g. the outlines of nuclei from lamin staining) and thresholding was used on tile by tile bases in lieu of global illumination correction (this is OK for images of the quality you provided)
- additional steps were added to capture potential phenotypical differences identified upon initial image evaluation in FIJI (disclaimer: all of these approaches are approximate and designed to capture differences, not to derive absolute quantification results):
- to calculate approximate signal ratio between the lamina and nuclear interior, detected nuclei were shrunk and the shrunk version of objects was subtracted from the original objects to obtain lamina rings
- to quantify speckled appearance of the nuclei, shrunk version of the nuclei was enhanced to bring out small speckle-like objects and Otsu algorithm was applied on Gaussian-smoothed shrunk nuclei to detect ‘speckle’-like object; these were than counted per nucleus 
- quantification steps were adapted to also capture variables of interest described above; texture quantification was also added.

Analogues logic can be followed to adapt or create pipelines for other applications.


