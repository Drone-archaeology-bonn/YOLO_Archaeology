# Detecting archaeological structures with YOLOv5

# Problem statement

In this project I successfully trained YOLOv5 to detect three given classes: bombcraters, charcoal piles, and barrows on a digital surface model (DSM).

# Best results

The best results appeared in taining exp47

# Preparation

* We recived a DSM with already annotated instances of the classes

* The original data gets split using Gridsplitter in QGIS, we receive two folders:
    * "split_images" containing all the splitted imges as TIFF files
    * "split_features" containing all the splitted features as Shapefiles

* Make sure that all the folders in "split_features" are 5 digits long, if not do in the console:

```console
mkdir Not_five
for f in *; do if [ ${#f} -lt 5 ]; then mv $f Not_five; fi; done   # Move all folders with less than five digits
cd Not_five
for D in */; do cd "${D}" for f in *; do mv "$f" "0$f"; done cd .. mv "${D}" "0${D}" done   # Add a 0 in front of every folder and file recurively
mv * ..
rm -rf Not_five
```


# Code for creating the datasets

I created two relevant datasets:
1. "dataset" contains all 35154 images that are in split_images
2. "9_tvt_dataset" contains 2324 images. 2,113 images with class instances and 211 background images


## Vulkaneifel/file_organisation.ipynb

Prepares the given data in split_images and split_featues for further processes by
1. Renames the TIFF's in split_images so that they match the feature naming convention e.g. "split_images/0001/0001/0001_0001.tif" -> "Images/00001.tif"
2. Converting the Shapefiles to GeoJSON e.g. e.g. "split_features/00001/00001.shp" -> "GeoJSON/00001.geojson"


## Vulkaneifel/parameter_extraction.ipynb

Takes a GeoJSON file and creates a correspnding TXT file for each image. The specifications are:
* One row per object
* Each row is ```class x_center y_center width height``` format
* Box coordinates are normalized (from 0 - 1)
* Class numbers are zero-indexed (start from 0)

Also splits the images in two folders:
1. "Labeled_images" all images that have a feature file with at least one instance of an object
2. "No_label_images" all images that either have an empty feature file or that don't have a feature file


## Vulkaneifel/dataset_creation.ipynb

Creates two diffrent datasets
- "9_tvt_dataset":
    - Takes all images in "Labeled_images" and 211 random images from "No_label_images"
    - Puts randomly 87% of the images with matching features in train and 15% in validation, and 15% in test
    - I choosed not to create a test set because the dataset is relatively small
    
- "dataset":
    - Ueses all 35,154 images, this means about 94% of the images are either black or without class instances
    - Pud randomly 70% of the images with matching feature file in train, 15% in validation and 15% in test

# Extra Code that is not relevant to create the dataset

## Progress.ipynb

Here I keep track of my progress and log my training, validating, and infrence progress

## Extracting_parameters.ipynb   

Takes a GeoJSON file and returns all parameters, that are important for labeling the features, that YOLOv5 needs, as a list.

Input: GeoJSON

Output: List [class, x_center, y_center, width, height]


## geojson_exploration.ipynb   
Explores the original big feature file "features_lidar.geojson" for better understanding of the given data


# Yolov5

* Read more about YOLOv5: https://github.com/ultralytics/yolov5
