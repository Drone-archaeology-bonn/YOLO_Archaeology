# Dataloader

# Preparation
Make sure that all the folders in split_features are 5 digits long, if not do in the console:

```console
mkdir Not_five
for f in *; do if [ ${#f} -lt 5 ]; then mv $f Not_five; fi; done   # Move all folders with less than five digits
cd Not_five
for D in */; do cd "${D}" for f in *; do mv "$f" "0$f"; done cd .. mv "${D}" "0${D}" done   # Add a 0 in front of every folder and file recurively
mv * ..
rm -rf Not_five
```

# Code for creatin the dataset

## Vulkaneifel/dataset_creation.ipynb

Does three things:
1. Renames the tif's in split_images so that they match the feature naming scheme
2. Divide the images into a train, validation and test set
3. Converting the shp files in split_features to GeoJSON

## Vulkaneifel/parameter_extraction.ipynb

Takes a GeoJSON file and creates a correspnding TXT file for each image. The specifications are:
* One row per object
* Each row is ```class x_center y_center width height``` format
* Box coordinates are normalized (from 0 - 1)
* Class numbers are zero-indexed (start from 0)


## Extracting_parameters.ipynb   (Not important for creating the dataset)

Takes a GeoJSON file and returns all parameters, that are important for labeling the features, that YOLOv5 needs, as a list.

Input: GeoJSON

Output: List [class, x_center, y_center, width, height]


# Yolov5

* Read more about YOLOv5: https://github.com/ultralytics/yolov5