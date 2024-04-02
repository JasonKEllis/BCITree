# 30% Canopy Cover workflow (ArcGIS Pro with 3D Analyst Extension)

### Part I
#### 1.	Create LAS dataset with all 181 Vancouver 2022 LiDAR tiles:
![Comparison](/Photos/CanopyCoverWorklow1.png)

#### 2.	Select only high vegetation and run LAS Dataset to Raster tool with a cell size of 0.25 m, interpolation type of binning, and output data type of integer:
![Comparison](/Photos/CanopyCoverWorklow2.png)

#### 3.	Run Raster Calculator tool with Con(“inputraster”>0,1) to turn all pixels greater than 0 to 1 so the polygons we create are already dissolved:
![Comparison](/Photos/CanopyCoverWorklow3.png)

#### 4.	Run Raster to Polygon tool and don’t simplify polygons so we can have greater control in the next steps:
![Comparison](/Photos/CanopyCoverWorklow4.png)

#### 5.	Run Select Layer by Attribute tool to select shapes with area greater than 0.5 m
![Comparison](/Photos/CanopyCoverWorklow5.png)

#### 6.	Run Eliminate Polygon Part tool to remove holes less than 0.5 m in area and previous polygons not selected in step 5
![Comparison](/Photos/CanopyCoverWorklow6.png)

#### 7.	Run Generalize tool with a tolerance of 0.15 m to create CanopyPoly layer
![Comparison](/Photos/CanopyCoverWorklow7.png)

### Part II
