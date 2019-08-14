# 3D Workshop with PostGIS and QGIS

## Introduction

Duration: 15'

- Who we are
- Who you are
- Workshop content and schedule
- Disclaimer: experimental software !
- PostGIS reference: https://postgis.net/docs/reference.html
- PointCloud reference: https://github.com/pgpointcloud/pointcloud

Meanwhile, installation of the software 

## Infrastructure / Installation

Duration: 10' + 15' during introduction

To follow this workshop you will need one of these setups.

### Setup A: online PostGIS

We already setup a full server-side environment for you, with pre-loaded data.
On your computer, you will need: 

- The latest QGIS version with 3D enabled, and QuickMapService plugin
- This workshop: XXXXXXXXXX
- The dataset: XXXXXXXXX

### Setup B: local virtual machine

You can also install all necessary software on your local computer, whenever the online PostGIS instance would not be available.

You will need: 
- Virtualbox installed on your PC
- This Virtual Machine: XXXXXX
- Enough RAM

Everything you will need is already installed in the provided virtual machine.

### Setup C: local install

You can also install all necessary software on your local computer if you are already running Linux, whenever the online PostGIS instance would not be available.

You will need: 
- The latest QGIS version with 3D enabled, and QuickMapService plugin
- This workshop: XXXXXXXX
- The dataset: XXXXXXXXX
- A PGGIS docker container running, [following the steps here](XXXXXXXXXX)

## DATA

Duration: 10'

We will use datasets from Boulder, Colorado.

The data is split into the data you need on your local computer, and the data needed on the server.

See the [data description and download page](XXXXXXXXX) if you want to know more, skip to next step if you already downloaded the dataset.

### Dataset description

- Areas Of Interest (aoi): some polygons we are interested in
- Osm road boulder (osm_road_boulder): OpenStreetMap roads on a part of Boulder
- DTM: Digital Terrain Model from XXXX
- Ortho: Orthophoto from XXXXX
- Boulder LIDAR ( boulder_lidar ): LIDAR coverage from the City of Boulder, downsampled
- Building footprints ( building_footprint ): 2D building footprints from the city of Boulder
- Building Roofs: 3D layer of building roofs from the city of Boulder 
- Building Walls: 3D layer of building walls from the city of Boulder 

Other assets: 
- spider.obj: Spiiiiiideeeeeers !

### General data information

Boulder is located at 1655m above sea level.

Projection applied: 
- NAD83(HARN) / Colorado North (ftUS): ESPG 2876

See the areas of interests layer for the locations we are interested in.

LIDAR altitude are in feet. Conversion to meter: 
- 1 foot = 0.3048 meter

## QGIS 3D

Duration: 30'

We will first discover QGIS and its 3D capabilities.

We will: 
- Load building footprints ( 2D )
- Load a base layer: orthophoto
- Open a new 3D view for these layers
  - learn the 3D navigation with mouse & keyboard
  - learn available 3D tools
  - see available options
- View a DTM automatically downloaded online
- Load a local DTM and use it for 3D view
- See the flight mode viewing options in QGIS 3D
- Use the 3D symbology configuration to display footprints in 3D
- Play with advanced 3D symbology for 2D objects
    - rule-based rendering
    - (models for point layer)
- Open 3D objects

### Start: loading 2D data

The data is located in `~/workshop-postgis-qgis-3d/`, and the QGIS projects for each step are
located in `~/workshop-postgis-qgis-3d/qgis/`.

- Open QGIS
- Start a new QGIS project
- Set the projection to EPSG:2876
- Find your data directory in the browser panel, and add it to favorites
- Open building footprints shapefile
- Open the styling panel and apply a new color to the footprints
- Open the orthophoto

Corresponding project: `boulder_1.qgs`

### Step 2: 3D view

- Open a new 3D window: View->New 3D map view
- Navigate the 3D view
  - Mouse pan
  - Mouse scroll
  - Right mouse button pan
  - Ctrl-Mouse pan
  - Shift-Mouse pan
  - arrows keys
  - PgUp / PgDown
- Discover the basic 3D tools:
  - Camera control
  - Zoom full
  - On-screen navigation
  - Identify
  - Measurement
  - Save as image
- See available options and their impact
  - Field of view for the camera
  - Terrain mode
    - flat is no elevation
    - We will use DEM later with an elevation layer
    - Online will download the elevation from the internet
    - Vertical scale will enhance the scale for the terrain
  - Terrain shading will make the terrain visible
  - Lights to configure global lights to your 3D scene
  - Show labels: 
    - configure labels for footprints and see the change with this option
  - Show map tile info, bounding boxes, camera view center for more technical informations

You can choose online terrain, use terrain shading and see the result.

Now load the `boulder_dtm` raster, used as a terrain model: 
- open the raster file
- configure its styling for 2D mode
    - blending mode multiply
    - Color gradient White to black
    - add some contrast
- Open 3D view configuration
- Choose DEM terrain type and select `boulder_dtm` layer
- Change the vertical scale and observe the result

Corresponding project: `boulder_2.qgz`

### Step 3: Flight mode

- Activate "Animation" in the 3D window
- Select keyframe 0
- Move to the view you want to start with
- Select keyframe 5s
- Move to the view you want to end with
- Press play
- Try to export the frames to a directory
    - choose a small framerate
    - this generates images, which will have to be merged to a movie with an external tool like ffmpeg
- Now change the interpolation method and replay
- Add some keyframes to the movie

Corresponding project: `boulder_3.qgz`

### Step 4: basic 3D symbology

We will now display our footprints in 3D.

- Open the building footprints layer propertiesq
- Select 3D symbology (cube symbol)
- Select "single symbol"
- Verify that altitude clamping is set to "Relative"
- Choose extrusion height 10
- See the result in 3D window
- Change diffuse and ambient colors
- See the result
- Add Edges and change the color
- Now set the extrusion height to the building height using an expression (hint: height attribute is in feet)

Corresponding project: `boulder_4.qgz`

### Step 5: advanced 3D symbology

We will now use rule-based symbology for 2D objects.

- For the building footprints, choose rule-based rendering in 3D styling
- Configure the symbology so that: 
    - Garages are all 4 meters high
    - Public buildings are extruded, in red
    - All other buildings are extruded in grey
- Find another nice thematic 3D mapping of your choice

Corresponding project: `boulder_5.qgz`

#### Step 6: relative versus absolute

We will now extrude the buildings and use absolute positionning instead of relative to the terrain.

- Choose Single Symbol 3D rendering
- Choose height as attribute value Ground Elevation (convert feet to meters)
    - you will need to multiply the height by the vertical scale set in the 3D window
- Choose altitude clamping absolute
- Set extrusion to building height (convert feet to meters)

Corresponding project: `boulder_6.qgz`

### Step 7: 3D objects

We will now open 3D building roofs and walls.

- locate and open the roofs and wall shapefiles
- Activate 3D symbology for both layers
    - coordinates in Z are absolute (and meters)
    - choose nice colors :-)
- Be patient!
- Enjoy your 3D buildings!
- Use rule-based symbology for thematic 3D mapping

Corresponding project: `boulder_7.qgz`

## PostGIS 3D

Duration: 70' + 15' pause

We will now discover PostGIS and its 3D capabilities

We will: 
- Create a new schema for each of you
- Load 2D data
- Open this data in QGIS
- Load 3D data into the database
    - Building roofs
    - Building walls
- Build 3D data from 2D data
- Run queries on this data

### DB Manager

We will use QGIS BD Manager for all database-related work.

Now, let's follow these steps.

- Create a new QGIS project
- Open QGIS Data source manager (Layer menu)
- Create a new PostGIS connection (host=localhost, db=osgeo, user=osgeo, password=osgeo)
- Try to connect to the database
- Now open QGIS Database Manager
- Open your database and create a new schema with your name

We will now import some data.

Note that in all following work, you will have to prefix table names by your own schema name.

- Use the DB Manager to load the building footprints shapefiles
- Now import the OSM roads

When importing, create spatial indexes.

### Display PostGIS data

Let's see it!

- Just like what we did for Shapefile, display the PostGIS layers in 3D
- Now load the roof and wall files into PostGIS (it takes some time)
- Display the roofs and walls in the 3D view

### Querying in 3D: PolyhedralSurfaces and buildings

We will now use 3D PostGIS functions to display 3D geometries.
We are going to use mainly the DB manager of QGIS, but you can also use `psql` in a terminal window to run queries.

Start with 1 PolyhedralSurface

- Open DB Manager and execute the following query on the database.
- Load query results as a new layer
- Configure the 3D view

```
select 
    -- we need an id
    1 as id,
    -- translate to ground level
    st_translate(
        -- extrude 300 meters
	    ST_Extrude(
            -- create a 2D shape around point
		   	ST_Buffer(
				 st_setsrid(ST_GeomFromText('POINT(3059378 1247489)'), 2876)
				, 150, 'quad_segs=8')
    	,0, 0, 300)
    , 0, 0, 1650)
 as geom;
```

Now 100 random PolyhedralSurfaces

```
select 
    nb as id,
    -- translate to ground level
    st_translate(
        -- create 3D shape
    	ST_Extrude(
            -- from a 2D shape
			ST_Buffer(
                -- the shape will be 1.5km around a central point, randomly placed
				 st_setsrid(st_makepoint(3059378 + random() * 3000 - 1500, 1247489 + random() * 3000 - 1500), 2876)
				, 150, 'quad_segs=8' )
    	,0, 0, random() * 300)
    , 0, 0, 1650) as geom
from 
    -- create 100 lines of result
	generate_series(1, 100) as nb;
```

We now want to make PolyhedralSurfaces out of the footprints.

```
select
    id
    -- translate to ground level
    , st_translate(
        -- only first geometry of set (there should be only one anyway)
        st_geometryn(
            -- extrude the geometry to given height
            st_extrude(geom, 0, 0, bldgheight)
            , 1)
     , 0, 0, 1650) as geom
from
    building_footprints_2876;
```

- Load the layer into QGIS 3D
- Use rule-based rendering to change styling according to height
- Add other attributes to the result
- Use rule-based rendering to change styling according to building type

### Querying in 3D: Tubes! 

We will make tube 3D objects for the lines coming from OpenStreetMap.

First, cut our lines into individual segments
See here for more information about this query: http://blog.cleverelephant.ca/2015/02/breaking-linestring-into-segments.html

Query steps are: 
- select all lines from osm_roads_boulder
- dump all points of the geometries, keeping the line id
- use a window function to make a line between two consecutive points
    - partition by line id and order by its id and the position of the point in the line
    - make a segment between last point in the partition and current point
    - generate id for every segment

Do not forget to add your schema to table names in the following queries.

```
create table 
    osm_roads_boulder_segment as
WITH segments AS (
SELECT  
	id
	, ST_MakeLine(lag((pt).geom, 1, NULL) OVER (PARTITION BY id ORDER BY id, (pt).path), (pt).geom)::geometry(LineStringZ, 2876) AS geom
  FROM (
	SELECT 
		id
		, ST_DumpPoints(geom) AS pt 
	FROM 
	    boulder_osm_road_2876	
	) as dumps
)
SELECT 
	row_number() over () as uid
	, * 
FROM 
	segments 
WHERE 
	geom IS NOT NULL;
```

- Visualize this new table

Now, let's make tubes ! The query steps are the following.
- Create a circle at the origin using a buffer
- extrude the circle at the length of the segment in Z direction
- rotate the circle to be along the XY plane
- rotate the circle to be oriented in the direction of the segment
- translate the circle to the starting point of the line and a correct height


The query for segment 234:

```
select 
    uid
	, st_translate(
	    st_rotatez(
		    st_rotatex(
			    st_extrude(
                    st_buffer(
                        st_setsrid(st_makepoint(0, 0, 0), 2876)
                        , 20)
                    , 0, 0, st_length(geom) )
                , pi() / 2 )
		    , - st_azimuth(st_endpoint(geom), st_startpoint(geom)) )
        , st_x(st_startpoint(geom))
		, st_y(st_startpoint(geom))
        , 1650 + 100) as tube
from 
	osm_roads_boulder_segment 
where 
	uid = 234;
```

- visualize the result in QGIS 3D.

Now write the query for: 
- another specific line id
- all lines

Now try the query with: 
- different width for the tubes
- different heights
- different buffer types ( e.g. octogon )
- a polygon as original shape instead of a buffer around a point

## PostGIS 3D: Point Clouds

Duration: 90'

We will now discover PostGIS PointCloud capabilities

We will: 
- Load Point Cloud data into PostGIS
- Visualize the Point Cloud data in QGIS ( 2D envelopes only )
- Query the Point Cloud layer

### Loading point clouds

We will use PDAL to load point clouds into PgPointCloud / PostGIS
We can also use lasinfo tools to get information on LAS files.

PDAL documentation is available here:  https://pdal.io/

Get information about `boulder_lite.las`. 
- Open a terminal
- navigate to the location of your data, into the LIDAR folder
- run the following commands

```bash
$ lasinfo boulder_lite.las
```

Or with PDAL: 

```bash
$ pdal info --summary boulder_lite.las
```

We will now load the LAS file into our PostgreSQL database. You need to edit the PDAL pipeline called `write_pg_pipeline.json` and change the database credentials according to the database you want to store the data to. Do not forget to specify your schema name.

Now run the pipeline (it takes some time): 
```bash
$ pdal pipeline --stream write_pg_pipeline.json
```

You can now: 
- open QGIS DB manager
- refresh the table list
- check that the LIDAR table has been created
- check the `pointcloud_columns` view to see the reference to your data
- check in the `pointcloud_formats` that the specific point cloud schema has been registered
- Use QGIS data browser to open the newly created layer

PDAL has chipped the data into 400 points chunks, called patches, as specified in the pipeline. What you see in QGIS is the patches geometries. QGIS cannot display LIDAR data directly yet (hint: fund it !)

You can now: 
- try to re-run the PDAL pipeline with 1000 points per patch

Corresponding project: `boulder_11_3doslandiacom_vpi.qgz`

### PgPointCloud: basic, Z, intensity

We will now work on our PointCloud data in SQL.


Through the SQL editor, we are able to run the same queries than those
previously seen with pgAdmin:

```sql
> SELECT COUNT(PA) FROM boulder_lidar;
```

```sql
> SELECT sum(pc_numpoints(pa)) from boulder_lidar;
```

- Display a patch content

In order to retrieve points contained by the patch with the id *18290* we can run the following query 

```sql
with tmp as (
	select 
		pc_explode(pa) as pts
	from 
		boulder_lidar
	where 
		id = 18290
)
select
	row_number() over() as id
	, pts::geometry(PointZM, 2876) as geom
	, pc_astext(pts) as content
from 
	tmp;
```

- Then you can load these points as a new layer in the QGIS canvas
- Try the same query with other patches ( like 15716, 22883, 22754 )

You can use QGIS styling capabilities to display the Z value of the points. Go to the styling window of the generated layer, and setup a graduated style using as an expression the z value of the geometry: `z($geometry)`.

Corresponding project: `boulder_12_3doslandiacom_vpi.qgz`

Now we do the same with the intensity value. We use the `PC_Get(pt pcpoint, dimname text)` function to get the intensity value in our query. Run and style it using a graduated style on the intensity value.


```sql
with tmp as (
    select
        pc_explode(pa) as pts
    from boulder_lidar
    where id = 15716
)
select
    row_number() over () as id
    , pts::geometry(pointzm, 2876) as geom
    , PC_Get(pts, 'Intensity') as intensity
    -- note that once the point is converted to a geometry we
    -- can use any PostGIS function, like getting the z value
    , ST_Z(pts::geometry(pointzm, 2876)) as z
from tmp;
```

- Then you can load these points as a new layer in the QGIS canvas
- Try the same query with other patches ( like 15716, 22482, 22754 )

Corresponding project: `boulder_13_3doslandiacom_vpi.qgz`

### Compression algorithm

Just a quick reminder that with a dimensional compression, each dimension of
a *pcpatch* has its own compression algorithm, dynamically determined during the
filling of the database. We can determined which algorithm is currently used
for each of the dimensions thanks to the next query:

```sql
select json_array_elements(pc_summary(pa)::json->'dims') from boulder_lidar where id = 1;
```

### PointCloud queries: Averaging Z values


In fact, there are at least 2 ways to retrieve the average altitude of a patch.

Either we use the *pc_summary* function on patches:

```sql
with tmp as (
    select
        json_array_elements(pc_summary(pa)::json->'dims') as dims
    from 
        boulder_lidar 
    where 
        id = 1
)
select
    dims->'stats'->'avg' as avgz
from tmp
where 
    dims->>'name' = 'Z';
```

or we compute it given z values of individual points:

```sql
with tmp as (
    select
        pc_get(pc_explode(pa), 'z') as z
    from boulder_lidar
    where id = 1
)
select avg(z) from tmp;
```

### Minimum and maximum altitude over the layer

In order to determine the minimum and maximum values for the altitude over all
patches of the layer, we can use the *pc_patchmin* and *pc_patchmax* functions:

```sql
select
    min(pc_patchmin(pa, 'z')) as min,
    max(pc_patchmax(pa, 'z')) as max
from boulder_lidar;
```

### Street average height

We focus on a specific street in Boulder. This street is included in the `aoi` table, and its polygon definition is the following: 

```
Polygon ((3059754.11213863966986537 1242957.59447286371141672, 3059783.05083170812577009 1242956.8886510815937072, 3059777.0513465590775013 1243585.42294808942824602, 3059752.34758418379351497 1243587.54041343578137457, 3059754.11213863966986537 1242957.59447286371141672))
```

We want to compute the average height of the street thanks to the LIDAR coverage.

So, we have to indicate in our SQL query that we only want to work with patches
contained within this box.

Moreover, note that our LIDAR data has been loaded from files having
a LAS format with version 1.4, revision 6:
http://www.asprs.org/wp-content/uploads/2010/12/LAS_1-4_R6.pdf.

So, if we want to get the altitude, we must concentrate on points representing
the ground (and not buildings, vegetation, ...). Thus, we have to sort the data
according to the classification field.

Once you have found all the necessary information to fullfill the below SQL
query, you can run it and compare the thus obtained altitude with the real
one.

We first explode the patches and filter points.

```sql
with points_in_area as (
	select
		pc_explode(pa) as pt
	from 
		boulder_lidar
where 
	pc_intersects(pa,
		st_geomfromtext('Polygon ((3059754.11213863966986537 1242957.59447286371141672, 3059783.05083170812577009 1242956.8886510815937072, 3059777.0513465590775013 1243585.42294808942824602, 3059752.34758418379351497 1243587.54041343578137457, 3059754.11213863966986537 1242957.59447286371141672))'
		,'2876'))
)
select
	avg(pc_get(pt, 'TODO_DIMENSION')) as alt
from 
	points_in_area
where 
	pc_get(pt, 'Classification') = TODO_CLASSIFICATION_LEVEL
order by 
	alt desc 
limit 1;
```

- Run the query and get the value
- What about max altitude of the street ?
- What about min altitude of the street ?

### Building footprints

We want to determine the altitude of the building footprints from the LIDAR data, so as to be able to extrude them with a real altitude value.

We will focus on building 234, and follow logical steps to get the final queries.

1. Patches intersecting building 234

```sql
-- patches intersecting building 234
select 
	bl.* 
from 
	building_footprints_2876 as b
join
	boulder_lidar as bl
on
	pc_intersects(bl.pa, b.geom)
where 
	b.id = 234
```

- visualize the resulting patches in QGIS

Corresponding project: `boulder_14_3doslandiacom_vpi.qgz`

2. Points on patches intersecting building 234

We now explode the resulting patches to get the points.

```sql
-- points on patches intersecting building 234
with points_234 as (
select 
	b.id as building_id
	, pc_explode(bl.pa) as pt 
from 
	building_footprints_2876 as b
join
	boulder_lidar as bl
on
	pc_intersects(bl.pa, b.geom)
where 
	b.id = 234
)
select
	row_number() over () as id
	, pt::geometry(pointzm, 2876) as geom
from
	points_234;
```

- Display the points in QGIS
- Apply a classification to see the Z values

Corresponding project: `boulder_15_3doslandiacom_vpi.qgz`

3. All points intersecting building 234

We now want only points inside the building

```sql
-- all points intersecting building
with points_234 as (
select 
	b.id as building_id
	, pc_explode(pc_intersection(bl.pa, b.geom)) as pt 
from 
	building_footprints_2876 as b
join
	boulder_lidar as bl
on
	pc_intersects(bl.pa, b.geom)
where 
	b.id = 234
)
select
	row_number() over () as id
	, pt::geometry(pointzm, 2876) as geom
from
	points_234;
```

- Display the points in QGIS
- Apply a classification to see the Z values

Corresponding project: `boulder_16_3doslandiacom_vpi.qgz`

4. Average elevation

Now that we have the right point set, compute the average Z value.

```sql
-- average size for building 234
-- all points in patches intersecting building
with points_234 as (
select 
	b.id as building_id
	, pc_explode(pc_intersection(bl.pa, b.geom)) as pt 
from 
	building_footprints_2876 as b
join
	boulder_lidar as bl
on
	pc_intersects(bl.pa, b.geom)
where 
	b.id = 234
)
select
	building_id
	, avg(st_z(pt::geometry(pointzm, 2876))) as z_avg
from
	points_234
group by
	building_id;
```

- What is the elevation for building 234 ?
- Do the same query for every building in our footprint table
- For each building, get the footprint geometry and a thematic symbology according to its elevation

### Max altitude of buildings

This query is simpler, since the max of all patches is the max of all points: 

```sql
-- max altitude for each building
-- easier: max(max(x), max(y)) = max(x,y)
select 
	b.id as building_id
	, max(pc_patchmax(pc_intersection(bl.pa, b.geom), 'Z')) as z_max
from 
	building_footprints_2876 as b
join
	boulder_lidar as bl
on
	pc_intersects(bl.pa, b.geom)
group by
	b.id;
```

### Extrude buildings by their altitude

We can now extrude the buildings by their altitude, getting real values instead of a fixed value like before.

We extrude to the maximum altitude of the building.

Steps are: 
- join footprint table and lidar height
- get building ID and max LIDAR height
- extrude the building

```sql
select 
    -- keep the building id
	b.id as building_id
    -- get the max value of the max values of intersections of the patch and the geometry on Z
    -- and convert to meters
	, max(pc_patchmax(pc_intersection(bl.pa, b.geom), 'Z')) * 0.3048 as z_max_m
    -- get this same value and use it to extrude the footprint geometry
	, st_geometryn(st_extrude(b.geom, 0, 0, max(pc_patchmax(pc_intersection(bl.pa, b.geom), 'Z')) * 0.3048), 1) as geom
from 
	building_footprints_2876 as b
join
	boulder_lidar as bl
on
	pc_intersects(bl.pa, b.geom)
group by
    -- get one result per building
    -- we need to group by geom also to be able to use the geometry in the extrusion function
	b.id, b.geom;
```

- run the query on all buildings
- visualize the results in QGIS 3D (hint: no symbology for 2D representation is much faster)
- determine what the problem is with our extrusion

Corresponding project: `boulder_17_3doslandiacom_vpi.qgz`

### Spider attack

We can simulate a spider attack along main roads. Generate a point layer along the roads using the following queries. Then use the `model` 3D representation for point layers. Choose the `spider.obj` model.

Warning: QGIS is very unstable for this feature and will probably crash.

```sql
select 
    row_number() over() as id
    , geom 
from ( 
    select
        (st_dumppoints(
            st_subdivide(
                st_difference(
                    st_exteriorring(
                        st_buffer(geom, 10,'endcap=flat side=right')
                    )
                , geom)
            , 10))).geom as geom 
    from 
        boulder_osm_roads_2876
    limit 100 
) as pts
```

Corresponding project: `boulder_18_3doslandiacom_vpi.qgz`

### Further exercises

- extrude the building footprints only from their ground altitude to their max altitude
  - find the ground LIDAR values around the building using a buffer
  - extrude to the altitude difference
  - translate to the ground altitude value
- make the tubes have the right height levels
    - find LIDAR ground altitude at beginning and end of segment
    - rotate the tube to the right slope

## Wrap-up

We used a lot of features: 
- 3D visualization with QGIS
- PostGIS 2D features
- PostGIS 3D features
- PgPointCloud 3D features

- Showcase your best 3D visualization
