# Dataloader

## Code

### Extracting_parameters.ipynb

Takes a GeoJSON and returns all parameters, that are important for labeling the features, that YOLOv5 needs, as a list.

Input: GeoJSON

Output: List [class, x_center, y_center, width, height]

### 40_40/shp_to_geojson.ipynb

Takes aall shp files and converts them into a GeoJSON files

Inout: shp

Output: GeoJSON

## Yolov5

* Read more about YOLOv5: https://github.com/ultralytics/yolov5