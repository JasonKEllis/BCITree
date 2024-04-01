# BCITree
An application directed towards analyzing urban forestry coverage and distribution, and engaging public awareness in urban forest equity.

## Team
- David Choi
- Jason Ellis
- Logan Salayka-Ladouceur

### Group Biographies
#### David
I graduated from the University of British Columbia in 2023 with a Bachelor's degree in Urban Forestry and a minor in Urban Green-Space Management. My primary interest in GIS—encompassing data interoperability, visualizing data, managing geospatial data, deriving insights from remote sensing, and conducting geospatial analyses—is all aimed at enhancing decision-making capabilities. As I pursue the BCIT Advanced Diploma in GIS, I hope to hone my technical and programming skills, which are geared toward my interest in utilities management, natural resource management, urban planning, and data integration. During my free time, I enjoy playing PC games, exploring the outdoors, and occasionally engaging in backcountry hiking.

#### Jason
I graduated from the University of British Columbia in 2023 with a bachelor's degree in geography and a minor in GIS. I am now studying at BCIT in their Advanced Diploma in GIS program and am also currently working at a commercial real estate company, aiding in data acquisition and map production. I am excited to use what I have learned in my studies and hope to explore the demographic side of GIS in my career. In my free time, I like to watch movies, exercise, and hang out with my friends.  

#### Logan
I graduated from the University of Northern British Columbia with a bachelor’s in Math and Physics in 2016. I have an interest in GIS that relates to natural resources and have had the opportunity to work with several community forest organizations. This work included silviculture surveys, mapping, and modeling. I am currently enrolled in the GIS Advanced Diploma program at BCIT and am excited to broaden my skill set. In my free time, I enjoy outdoor activities such as biking, snowboarding, and rock climbing.

## Mission Statement
In 2018, the United Nations predicted that 68% of the global population will be living in urban areas by 2050 (1). Urbanization and the expansion of built-up areas to accommodate growing urban populations have accelerated deforestation globally. The average global urban tree cover declines by 40,000 hectares each year (2). Trees provide essential ecosystem services in urban environments for humans, such as wind breaks, shade, air pollution removal, stormwater regulation, and carbon storage and sequestration, and even provide physical and mental health benefits (3). These are all direct and indirect benefits from urban trees that benefit both the environment and human well-being. 

Similar to this global trend, the City of Vancouver in British Columbia is also facing the challenges of the loss of trees from urbanization. Although Vancouver initially set its canopy coverage goal of 22% by 2050, it was found that the City of Vancouver has 23% canopy coverage during their 2020 reassessment. Subsequently, the Vancouver Board of Parks and Recreation proposed the City of Vancouver to increase their city-wide canopy cover target to 30% by 2050 "as an ambitious but achievable target" (p6;4). 

While Vancouver may have set a city-wide tree canopy coverage target, Nature Canada notes that equitable access to trees at the neighborhood level is often inadequately addressed (5). They report that canopy coverage tends to be much lower in low-income and racialized communities compared to affluent communities. To address tree inequity in Canadian cities, Nature Canada recommends the 3-30-300 rule, developed by Cecil Konijnendijk, an Honorary Professor of Urban Forestry at the University of British Columbia. This rule of thumb asserts that 3 trees should be observable by everyone from their home, 30% canopy cover should be present in all neighborhoods, and all residents should live within 300 meters of a greenspace of at least one hectare (5,6).

Furthermore, engaging citizens in the urban forest and monitoring the status and condition of the urban forest are two of the five urban forest goals found in the City of Vancouver’s Urban forestry Strategy (5). Within these five goals, distributing ecosystem services equitably and measuring progress are two of twelve principles that underpin Vancouver’s urban forestry strategy (7).  Currently, there are no publicly available tree canopy data or polygons for Vancouver, making it challenging for citizens to assess the current status of the urban forest and the distribution of its ecosystem services, which hinders their ability to gauge the progress towards urban forestry goals.

BCITree aims to facilitate citizen engagement in urban forestry by providing the public audience, including those interested in urban forestry, urban planning, landscape architecture, public health, and their local municipality, with the ability to visualize the state of canopy coverage and the distribution of the 3-30-300 tree equity rule. By providing this visualization, we hope that citizens can better understand the current distribution of urban tree ecosystem services and Vancouver's progress towards the 30% canopy target by 2050. Additionally, citizens can use this information to advocate for addressing tree inequity and the implementation of the 3-30-300 rule to decision-makers. This tool also enables planners and relevant stakeholders to approximate canopy coverage and identify areas in need of attention before a formal tree inventory can be commissioned by the City of Vancouver, supporting more equitable urban forest planning and management.

#### Literature Sources
(1) https://www.un.org/development/desa/pd/content/urbanization-0

(2) https://www.fs.usda.gov/research/treesearch/59488

(3) https://ehjournal.biomedcentral.com/articles/10.1186/s12940-016-0103-6

(4) https://parkboardmeetings.vancouver.ca/2020/20201207/REPORT-UrbanForestStrategyUpdate-20201207.pdf

(5) https://naturecanada.ca/wp-content/uploads/2022/09/Nature-Canada-Report-Tree-Equity.pdf

(6) https://www.sciencedirect.com/science/article/abs/pii/S0048969723063660

(7) https://vancouver.ca/files/cov/urban-forest-strategy.pdf

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











## Data Sources

##### Data Used for Visible Tree Analysis
| Data Name | Data Source | Data Type | Data Link |
|-----------------|-----------------|-----------------|-----------------|
| Vancouver Lidar      | Vancouver Open Data      | Point      | [Lidar Data](https://opendata.vancouver.ca/explore/dataset/lidar-2022/information/)      |
| Land Use      | Metro Vancouver      | Polygon      | [Land Use](https://open-data-portal-metrovancouver.hub.arcgis.com/search?q=landuse)      |
| Property Boundaries      | Vancouver Open Data      | Polyline      | [Property Boundaries](https://opendata.vancouver.ca/explore/dataset/property-cadastral-boundaries/information/)     | 

##### Data Used for Canopy Cover Analysis
| Data Name | Data Source | Data Type | Data Link |
|-----------------|-----------------|-----------------|-----------------|
| Vancouver Lidar      | Vancouver Open Data      | Point      | [Lidar Data](https://opendata.vancouver.ca/explore/dataset/lidar-2022/information/)      |

##### Data Used for Park Analysis
| Data Name | Data Source | Data Type | Data Link |
|-----------------|-----------------|-----------------|-----------------|
| Public Streets      | Vancouver Open Data      | PolyLine      | [Public Streets](https://www12.statcan.gc.ca/census-recensement/2021/geo/sip-pis/boundary-limites/index2021-eng.cfm?year=21)      |
| Vancouver Parks      | Vancouver Open Data      | Polygon      | [Vancover Parks](https://opendata.vancouver.ca/explore/dataset/public-streets/information/)      |
| Land Use      | Metro Vancouver      | Polygon      | [Land Use](https://open-data-portal-metrovancouver.hub.arcgis.com/search?q=landuse)      |
| Burnaby Parks      | Burnaby Open Data      | Polygon      | [Burnaby Parks](https://data.burnaby.ca/datasets/a1a896a4209d4325bacacea417ffc400_6/explore?location=49.237308%2C-122.958050%2C12.70)      |
| Property Boundaries      | Vancouver Open Data      | Polyline      | [Property Boundaries](https://opendata.vancouver.ca/explore/dataset/property-cadastral-boundaries/information/)     | 

##### General Data Used for all Analyses
| Data Name | Data Source | Data Type | Data Link |
|-----------------|-----------------|-----------------|-----------------|
| Local Area Boundary      | Vancover Open Data      | Polyline      | [Local Area Boundary](https://opendata.vancouver.ca/explore/dataset/local-area-boundary/information/?disjunctive.name)      |
| Dissemination Areas      | Canada Census      | Polygon      | [Dissemination Areas](https://www12.statcan.gc.ca/census-recensement/2021/geo/sip-pis/boundary-limites/index2021-eng.cfm?year=21)      |



