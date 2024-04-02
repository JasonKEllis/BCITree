# BCITree
###### An application directed towards analyzing urban forestry coverage and distribution, and engaging public awareness in urban forest equity.

## Team
- David Choi
- Jason Ellis
- Logan Salayka-Ladouceur

## Mission Statement
In 2018, the United Nations predicted that 68% of the global population will be living in urban areas by 2050 (1). Urbanization and the expansion of built-up areas to accommodate growing urban populations have accelerated deforestation globally. The average global urban tree cover declines by 40,000 hectares each year (2). Trees provide essential ecosystem services in urban environments for humans, such as wind breaks, shade, air pollution removal, stormwater regulation, and carbon storage and sequestration, and even provide physical and mental health benefits (3). These are all direct and indirect benefits from urban trees that benefit both the environment and human well-being. 

Similar to this global trend, the City of Vancouver in British Columbia is also facing the challenges of the loss of trees from urbanization. Although Vancouver initially set its canopy coverage goal of 22% by 2050, it was found that the City of Vancouver has 23% canopy coverage during their 2020 reassessment. Subsequently, the Vancouver Board of Parks and Recreation proposed the City of Vancouver to increase their city-wide canopy cover target to 30% by 2050 "as an ambitious but achievable target" (p6;4). 

While Vancouver may have set a city-wide tree canopy coverage target, Nature Canada notes that equitable access to trees at the neighborhood level is often inadequately addressed (5). They report that canopy coverage tends to be much lower in low-income and racialized communities compared to affluent communities. To address tree inequity in Canadian cities, Nature Canada recommends the 3-30-300 rule, developed by Cecil Konijnendijk, an Honorary Professor of Urban Forestry at the University of British Columbia. This rule of thumb asserts that 3 trees should be observable by everyone from their home, 30% canopy cover should be present in all neighborhoods, and all residents should live within 300 meters of a greenspace of at least one hectare (5,6).

Furthermore, engaging citizens in the urban forest and monitoring the status and condition of the urban forest are two of the five urban forest goals found in the City of Vancouver’s Urban forestry Strategy (5). Within these five goals, distributing ecosystem services equitably and measuring progress are two of twelve principles that underpin Vancouver’s urban forestry strategy (7).  Currently, there are no publicly available tree canopy data or polygons for Vancouver, making it challenging for citizens to assess the current status of the urban forest and the distribution of its ecosystem services, which hinders their ability to gauge the progress towards urban forestry goals.

BCITree aims to create an app which facilitates citizen engagement in urban forestry by providing the public audience, including those interested in urban forestry, urban planning, landscape architecture, public health, and their local municipality, with the ability to visualize the state of canopy coverage and the distribution of the 3-30-300 tree equity rule. By providing this visualization, we hope that citizens can better understand the current distribution of urban tree ecosystem services and Vancouver's progress towards the 30% canopy target by 2050. Additionally, citizens can use this information to advocate for addressing tree inequity and the implementation of the 3-30-300 rule to decision-makers. This tool also enables planners and relevant stakeholders to approximate canopy coverage and identify areas in need of attention before a formal tree inventory can be commissioned by the City of Vancouver, supporting more equitable urban forest planning and management.

#### Literature Sources
(1) https://www.un.org/development/desa/pd/content/urbanization-0

(2) https://www.fs.usda.gov/research/treesearch/59488

(3) https://ehjournal.biomedcentral.com/articles/10.1186/s12940-016-0103-6

(4) https://parkboardmeetings.vancouver.ca/2020/20201207/REPORT-UrbanForestStrategyUpdate-20201207.pdf

(5) https://naturecanada.ca/wp-content/uploads/2022/09/Nature-Canada-Report-Tree-Equity.pdf

(6) https://www.sciencedirect.com/science/article/abs/pii/S0048969723063660

(7) https://vancouver.ca/files/cov/urban-forest-strategy.pdf

## Explanation of Index 
We at BCITree wanted to create a single value to help determine a neighbourhood's level of urban forestry and how close it is to meeting the 3-30-300 goal. To accomplish this task, we gathered the percent of people within a park's service area, the percent of people able to see three trees, and the canopy cover percent. The canopy cover value first had to be normalized, as a value of 30% or more represented the neighbourhood hitting the goal of canopy coverage. The canopy coverage percent was divided by 30 with any values above 1, meaning a canopy coverage of more than 30%, being changed to 1 to signify they have met the goal. The two previous percent values along with the normalized canopy coverage value were then averaged to obtain a final index value. This index could also be seen as a percentage as it represents how close a neighbourhood is to meeting the 3-30-300 goal of urban forestry equity.

## Methodology and Justification

#### Calculating Park Service Area
A simpler method of calculating which houses are within 300 meters of a park would be to use a buffer. However, this buffer uses a euclidean distance which does not accurately describe a property’s accessibility to a park. Instead, we used ArcGIS’s Network Analyst extension to create a service area using their service area analysis toolset. Using Vancouver streets as the network, a more accurate representation of the park’s service area was created.

#### Using Park Vertices as Entrances
Due to network analysis being constrained to a point-to-point logic, the park polygons were turned into points using the feature vertices to points tool. These vertices provide a good approximation of where pedestrians may enter these parks from.

#### Cleaning Property boundaries
The property boundary layer contained every property in Vancouver and had no attribute data to discern between residential properties and other properties not being used in this analysis. Metro Vancouver land use data from 2016 was used to overlay onto the property boundaries and any property boundaries which did not intersect with the various housing land use polygons were discarded. However, there were 346 polygons labeled as unclassified or undeveloped which were cross referenced with an aerial imagery basemap to discern if any of the polygons were either housing properties or parks. From the 346 polygons, 138 were classified as housing, and 3 were classified as parks larger than 1 hectare.

#### Creating Canopy Polygon from LiDAR Data
The 2022 Vancouver LiDAR data is well classified into tall vegetation and other categories so the work in this section is largely complete. If the data was unclassified or poorly classified a dataset this size would be a large undertaking. When creating a raster from a LAS dataset the cell size controls the resolution of the output raster and it was found that a cell size of 0.25m was found to be a good balance of retaining detail without being too cumbersome. Small polygons and small holes in polygons were later filtered out and after checking the size of many polygons a value of 0.5m was chosen for both.

#### Finding the Number of Trees Visible for each Building
This is nearly impossible to do with much accuracy as you would need an extremely detailed LiDAR dataset, window locations, and huge amounts of processing time. We have chosen a simple and easily repeatable process to get a rough estimate. We have also made a very conservative estimate and the actual tree visibility is likely higher - Vancouver is doing really well in this regard. Our process involves buffering every residential building by 30m and choosing no dissolve so we have an individual polygon buffer for each building. Then we count the number of trees in each buffer polygon and join this count back to the buildings layer. Finally we multiply the count by a factor that represents the percentage of trees that will be visible. In this case, a conservative estimate of 0.25 was used. This method could be combined with more detailed viewshed analyses in small areas or a field analysis to derive more accurate parameters.

## Limitations
#### Temporal Data Resolution Disparity
Unfortunately, not all of the data used in this analysis was from the same year. For example, the Metro Vancouver land use data was from 2016 and had to be cross-referenced with basemap aerial imagery from 2021 to increase accuracy. However, a thorough analysis of the remaining unclassified and undeveloped polygons was made to minimize data discrepancies and ensure the reliability of the findings.

#### Vancouver Street Tree Data Quality
We had many doubts on the maintenance quality of the Vancouver Street Tree dataset so we chose to derive our own tree dataset using lidar gathered in 2022. From our findings, we gathered that it was right to create our own dataset as many trees were unaccounted for in the vancouver dataset and would not have been adequate for our analysis had we used it.

#### "Seeing" Three Trees
The first part of the 3-30-300 goal is that people should be able to see three trees from their house. The questions then are how far does each person see from their house? where is every window in Vancouver? are there any obstructions? All of these questions could only be answered with an extensive analysis which we did not have time nor the data for. Instead, we decided every person, given obstructions, could see 25% of trees within a 30m buffer of ones house. We chose these parameters to ensure a conservative analysis of visible trees. However, even with the conservative take on this goal of visible trees, roughly 98% of all homes in Vancouver are able to see three trees. Because of this conservative analysis and the immense success of visible trees in Vancouver, we felt confident to utilize the findings of this analysis.

#### Definition of Park Service
The 3-30-300 goal specifically looks at whether a park is within 300 meters of a home which drove our approach in this analysis. We think the 3-30-300 goal should also analyze whether a park meets a certain quality such as amenities, safety and ecological value. Future analyses related to this project could be to judge each park's quality and whether or not they are fit to still be considered an adequate park in this analysis.

#### Vancouver Street Data Quality
In creating our network analysis, it is worth noting not every trail or pathway is located in Vancouvers data repository. Therefore, over the area of Vancouver, there may be some homes which are within 300 meters of a park but given certain paths were not included in the network, were not counted as being serviced. Nevertheless, this is a minor discrepency in the data given the scale.

#### Lidar Classificaton Issues
Despite our extensive analysis and cleaning of lidar data, given the scale and shear amount of points, there are likely some points that were misclassified. However, the city of Vancouver completed some classification of their own for the lidar data set and were quite adequate to start for our analysis.




## User Guide


#### Distribution of Tree Type Pie Chart
Located on the top left of the app, a pie chart is automatically updated as the user views around the map. Just wait a few seconds to let the chart load, and it will display the breakdown of tree types in the map's extent.


#### Other Products
Please use these buttons to navigate to our various sources; our website, user guide, and Github.

#### Navigation Buttons
These buttons will help you navigate from the main map displaying our created index of Vancouver as well as the sub-maps displaying the individual 3-30-300 analyses. Also make sure to use the button to change between displaying Local Area Boundaries and Dissemination Areas. 

#### Legend
The legend colour scheme was chosen to highlight areas failing to meet the goals of the 3-30-300 target. The greens signify areas which are either meeting the goal or are close enough where there is no immediate action needed. The brown areas on the other hand are areas where future development and consideration could be needed. Areas with low index ratings should especially be considered for future urban forest development to increase Vancouver's green space equity.

#### Select/Export Function
If you are interested in exporting the Index layers, please navigate to their respective map and select the data you wish to export. Once selected, click the four circles to then select the export option. If you wish to export the tree points or canopy cover polygons, you may do this from any layer following the same steps.

#### Filter
If you are wishing to visualize the polygons containing a certain range of canopy percentages or index values for example, please navigate to the filter at the bottom left of the map. Here, you can enter in values to see which polygons fall within your designated range. Note, filtering percent of people serviced by parks as well as percent of people who can see three trees was calculated as a fraction representing a percentage. In doing so, using the filter will require entering in values from 0 to 1 as percent fractions.


## General Workflow Processes
#### Visible Trees Analysis Workflow
Part I: Create TreeTops point layer

1. Create LAS dataset with all 181 Vancouver 2022 LiDAR tiles and select high vegetation.
2. Run _LAS Dataset to Raster tool_ with a cell size of 0.5 m, interpolation type of binning, cell assignment of maximum, and output data type of integer.
3. Run _Focal Statistics tool_ with a 15x15 m rectangular neighbourhood (this value was determined with experimentation) and statistics type of maximum.
4. Run _Raster Calculator tool_ with Con(“raster”==”rasterfocalstats”,1) to select cells that represent a local maxima.
5. Run _Raster to Multipoint tool_ to create a point layer from the our raster with cells that are 1, representing local maxima.
6. Run _Clip tool_ with our point layer as the input and CanopyPoly as the clip feature. This removes trees that we already removed from the CanopyPoly layer for being too small to represent a tree.

Part II: Find Number of trees near each building.
1. Run the _Buffer tool_ on the residential property polygon layer from 300m park workflow using a 30m buffer and set to no dissolve, ending up with a unique polygon buffer for each building.
2. Run _Summarize Within tool_ for this buffered layer and the TreeTops point layer to find the number of trees in each buffer.
3. Run _Add Join tool_ to join this summarized layer to the buildings layer so we have the point count in the buildings layer.
4. Add a field in buildings layer called VisibleTrees.
5. Calculate field for VisibleTrees by multiplying PointCount by a factor representing the average percentage of nearby trees that are visible.

#### Canopy Cover Workflow

Part I: Create city-wide canopy cover polygon

1. Create LAS dataset with all 181 Vancouver 2022 LiDAR tiles
2. Select only high vegetation and run _LAS Dataset to Raster tool_ with a cell size of 0.25 m, interpolation type of binning, and output data type of integer.
3. Run _Raster Calculator tool_ with Con(“inputraster”>0,1) to turn all pixels greater than 0 to 1 so the polygons we create are already dissolved.
4. Run _Raster to Polygon tool_ and don’t simplify polygons so we can have greater control in the next steps.
5. Run _Select Layer by Attribute tool_ to select shapes with area greater than 0.5 m.
6. Run _Eliminate Polygon Part tool_ to remove holes less than 0.5 m in area and previous polygons not selected in step 5.
7. Run _Generalize tool_ with a tolerance of 0.15 m to create CanopyPoly layer.

Part II: Calculate Percentage 
1. Run _Pairwise Dissolve tool_ on CanopyPoly to dissolve all polygons into one multipart polygon. This will simplify the next steps.
2. Run _Pairwise Intersect tool_ with the DisseminationAreas Layer and the CanopyPoly layer. Make sure shape_area updates correctly.
3. Run _Join Field tool_ on the DisseminationAreas feature class to perform a one-to-one join of the layer from step 2 to DisseminationAreas on any unique field (we chose DAUID). This will join the shape_area of the intersected layer to DisseminationAreas.
4. Add a field called CanopyPercentage of type double to DisseminationAreas.
5. Calculate this field as 100*Shape_Area_1/Shape_Area.
6. Repeat with VancouverNeighbourhoods but first clip to DisseminationAreas to remove the water in the neighbourhoods layer to ensure the area is accurate.

#### Park Service Area Workflow

Part I: Cleaning Data
1. Combine park datasets and filter out any parks under a hectare.
2. Use Metro Vancouver land use data to determine property lots which are residential.
3. Clean up unclassified parcels from land use layer using satellite imagery to diminish 2016 and 2024 discrepancy.
4. Match cleaned residential land use layer to property boundary layer to get a property boundary layer only displaying housing property boundaries.

Part II: Running Network Analysis
1. Using _Features to Vertices tool_ determine park vertices to be used as park entrances in network analysis.
2. Create a point layer displaying points for geometric center of each property boundary.
3. Create network analysis layer using street data.
4. Run service area analysis using park vertices, streets, and a 300 meter distance.
5. Use 300 meter service area layer to determine which property points fall within service area.
6. Use _Summarize Within tool_ and amount of serviced points within dissemination polygons to determine percent of homes within 300 meters of a park per dissemination area.
7. Repeat Part II for Local Area Boundaries.


## Data Sources

#### Data Used for Visible Tree Analysis
| Data Name | Data Source | Data Type | Data Link |
|-----------------|-----------------|-----------------|-----------------|
| Vancouver Lidar      | Vancouver Open Data      | Point      | [Vancouver Lidar Data](https://opendata.vancouver.ca/explore/dataset/lidar-2022/information/)      |
| Land Use      | Metro Vancouver      | Polygon      | [Land Use](https://open-data-portal-metrovancouver.hub.arcgis.com/search?q=landuse)      |
| Property Boundaries      | Vancouver Open Data      | Polyline      | [Property Boundaries](https://opendata.vancouver.ca/explore/dataset/property-cadastral-boundaries/information/)     | 

#### Data Used for Canopy Cover Analysis
| Data Name | Data Source | Data Type | Data Link |
|-----------------|-----------------|-----------------|-----------------|
| Vancouver Lidar      | Vancouver Open Data      | Point      | [Vancouver Lidar Data](https://opendata.vancouver.ca/explore/dataset/lidar-2022/information/)      |

#### Data Used for Park Analysis
| Data Name | Data Source | Data Type | Data Link |
|-----------------|-----------------|-----------------|-----------------|
| Public Streets      | Vancouver Open Data      | PolyLine      | [Public Streets](https://www12.statcan.gc.ca/census-recensement/2021/geo/sip-pis/boundary-limites/index2021-eng.cfm?year=21)      |
| Vancouver Parks      | Vancouver Open Data      | Polygon      | [Vancouver Parks](https://opendata.vancouver.ca/explore/dataset/public-streets/information/)      |
| Land Use      | Metro Vancouver      | Polygon      | [Land Use](https://open-data-portal-metrovancouver.hub.arcgis.com/search?q=landuse)      |
| Burnaby Parks      | Burnaby Open Data      | Polygon      | [Burnaby Parks](https://data.burnaby.ca/datasets/a1a896a4209d4325bacacea417ffc400_6/explore?location=49.237308%2C-122.958050%2C12.70)      |
| Property Boundaries      | Vancouver Open Data      | Polyline      | [Property Boundaries](https://opendata.vancouver.ca/explore/dataset/property-cadastral-boundaries/information/)     | 

#### General Data Used for all Analyses
| Data Name | Data Source | Data Type | Data Link |
|-----------------|-----------------|-----------------|-----------------|
| Local Area Boundary      | Vancouver Open Data      | Polyline      | [Local Area Boundary](https://opendata.vancouver.ca/explore/dataset/local-area-boundary/information/?disjunctive.name)      |
| Dissemination Areas      | Canada Census      | Polygon      | [Dissemination Areas](https://www12.statcan.gc.ca/census-recensement/2021/geo/sip-pis/boundary-limites/index2021-eng.cfm?year=21)      |
