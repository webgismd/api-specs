# DataBC’s Open Web Map Services (WMS)
# Developer Guide
This guide is aimed at developers and web masters that would like to incorporate the DataBC's Web Map Services into their applications and websites.
<br>
## Introduction

A Web Map Service (WMS) as defined here is specifically a web standard developed by the Open Geospatial Consortium (OGC) (see http://www.opengeospatial.org/), an international not for profit organization.
It is a standardized HTTP interface used for styling & rendering data into a map (image) and perform identify operations. 

The Metadata record for this service can be found here:<br> 
BC Web Map Library http://catalogue.data.gov.bc.ca/dataset/6164a2af-d3ac-4e92-8dbe-51a93bb5e24b. <br> In addition, each WMS layer has its own specific URL and can be found within each metadata record. See - http://catalogue.data.gov.bc.ca/dataset?download_audience=Public&res_format=wms

<br><br>
DataBC offers web map services (WMS) that allow users to view hundreds of data layers from the B.C. Geographic Warehouse (BCGW) in desktop geospatial software or via web-based map applications.  
These connection services deliver current data directly from the BCGW so that you or users of your web mapping applications can work with the latest data available without needing to repeatedly download the data.
These data connection services are provided to offer wide flexibility in map creation and data sharing. 
All data layers are available according to the industry standard (OGC) Web Map Service (WMS) over HTTP and HTTPS (GET/POST).
<br><br>
This service provides georeferenced images (maps) and data dynamically from data stored in the B.C. Geographic Warehouse database. 
WMS-produced maps are generally rendered in image formats such as PNG, GIF or JPEG, or as vector-based graphical elements in Scalable Vector Graphics (SVG) or as Keyhole Markup (KML) image overlay or placemark formats. 
<br><br>
A WMS classifies its geographic information holdings into “Layers” and offers a finite number of predefined “Styles” in which to display those layers. As a queryable WMS service offering, users are also able to make requests that provide attribute information regarding a point, line or polygon.
 
Using Web Maps Services can be used through application programming interface (API) specifications such as:
<br><br>
Leaflet https://leafletjs.com/<br>
Openlayers https://openlayers.org/<br>
ArcGIS API for JavaScript https://developers.arcgis.com/javascript/<br>
ArcGIS Online https://doc.arcgis.com/en/arcgis-online/reference/ogc.htm<br> 
Google Maps API v3<br>
<br>
https://www.opengeospatial.org/resource/products/byspec/?specid=107<br> 
https://en.wikipedia.org/wiki/Web_Map_Service

## A Note on WHEN to use WMS vs WFS
When to use WMS: <br> 
1.) You want to create an image that is styled (fastest response) (ie. has predefined cartography)<br> 
2.) You want to do Very simple Querying of data <br> <br> 
When to use WFS:<br> 
1.) You want to spatial on non-spatial query a dataset and retrieve the features geometry and attributes (using CQL Filters) <br> 
2.) You want to find the feature definition (feature’s property names and types with the WFS DescribeFeatureType request)<br> 

## Resource Overview
DataBC's Web Map Services uses Geoserver (an Opensource project - http://geoserver.org/) to provide both WMS and WFS services. 
GeoServer is an open source software server written in Java that allows users to share and edit geospatial data. Designed for interoperability, it publishes data from any major spatial data source using open standards.
<br>
Being a community-driven project, GeoServer is developed, tested, and supported by a diverse group of individuals and organizations from around the world.
<br>
GeoServer is the reference implementation of the Open Geospatial Consortium (OGC) Web Feature Service (WFS) and Web Coverage Service (WCS) standards, as well as a high performance certified compliant Web Map Service (WMS). GeoServer forms a core component of the Geospatial Web.
<br>
https://docs.geoserver.org/latest/en/user/services/wms/index.html
<br>


Any Questions about the service? Contact datamaps@gov.bc.ca

## Types of  WMS Requests:
### <GetCapabilities>
The GetCapabilities operation requests metadata about the operations, services, and data (“capabilities”) that are offered by a WMS server.

The parameters for the GetCapabilities operation are:
https://docs.geoserver.org/latest/en/user/services/wms/reference.html#getcapabilities 

Parameter|	Required?|	Description
---------------------: | --- |--- |
service|	Yes	|Service name. Value is WMS.
version|	Yes	|Service version. Value is one of 1.0.0, 1.1.0, 1.1.1, 1.3.0.
request	|Yes	|Operation name. Value is GetCapabilities.

### <GetMap> 
The GetMap operation requests that the server generate a map. The core parameters specify one or more layers and styles to appear on the map, a bounding box for the map extent, a target spatial reference system, and a width, height, and format for the output. The information needed to specify values for parameters such as layers, styles and srs can be obtained from the Capabilities document.

The response is a map image, or other map output artifact, depending on the format requested. GeoServer provides a wide variety of output formats, described in WMS output formats.

The standard parameters for the GetMap operation are:
https://docs.geoserver.org/latest/en/user/services/wms/reference.html#getmap

Parameter|	Required?	|Description
---------------------: | --- |--- |
service|	Yes|	Service name. Value is WMS.
version|	Yes|	Service version. Value is one of 1.0.0, 1.1.0, 1.1.1, 1.3.0.
request|	Yes|	Operation name. Value is GetMap.
layers|	Yes	|Layers to display on map. Value is a comma-separated list of layer names.
styles|	Yes	|Styles in which layers are to be rendered. Value is a comma-separated list of style names, or empty if default styling is required. Style names may be empty in the list, to use default layer styling.
srs or crs|	Yes|	Spatial Reference System for map output. Value is in form EPSG:nnn. crs is the parameter key used in WMS 1.3.0.
bbox|	Yes	|Bounding box for map extent. Value is minx,miny,maxx,maxy in units of the SRS.
width|	Yes	|Width of map output, in pixels.
height|	Yes	|Height of map output, in pixels.
format	|Yes|	Format for the map output. See WMS output formats for supported values.
transparent|	No|	Whether the map background should be transparent. Values are true or false. Default is false
bgcolor|	No|	Background color for the map image. Value is in the form RRGGBB. Default is FFFFFF (white).
exceptions|	No|	Format in which to report exceptions. Default value is  application/vnd.ogc.se_xml.
time|	No|	Time value or range for map data. See Time Support in GeoServer WMS for more information.
sld|	No|	A URL referencing a StyledLayerDescriptor XML file which controls or enhances map layers and styling
sld_body|	No|	A URL-encoded StyledLayerDescriptor XML document which controls or enhances map layers and styling


### <GetFeatureInfo> 
The GetFeatureInfo operation requests the spatial and attribute data for the features at a given location on a map. It is similar to the WFS GetFeature operation, but less flexible in both input and output. Since GeoServer provides a WFS service we recommend using it instead of GetFeatureInfo whenever possible.

The one advantage of GetFeatureInfo is that the request uses an (x,y) pixel value from a returned WMS image. This is easier to use for a naive client that is not able to perform true geographic referencing.

The standard parameters for the GetFeatureInfo operation are:
https://docs.geoserver.org/latest/en/user/services/wms/reference.html#getfeatureinfo

Parameter|	Required?	|Description
---------------------: | --- |--- |
service|	Yes|	Service name. Value is WMS.
version|	Yes|	Service version. Value is one of 1.0.0, 1.1.0, 1.1.1, 1.3.0.
request|	Yes|	Operation name. Value is GetFeatureInfo.
layers|	Yes|	See GetMap
styles|	Yes|	See GetMap
srs or crs|	Yes|	See GetMap
bbox|	Yes|	See GetMap
width|	Yes|	See GetMap
height|	Yes|	See GetMap
query_layers|	Yes|	Comma-separated list of one or more layers to query.
info_format|	No|	Format for the feature information response. See below for values.
feature_count|	No|	Maximum number of features to return. Default is 1.
x or i	|Yes|	X ordinate of query point on map, in pixels. 0 is left side. i is the parameter key used in WMS 1.3.0.
y or j	|Yes|	Y ordinate of query point on map, in pixels. 0 is the top. j is the parameter key used in WMS 1.3.0.
exceptions|	No|	Format in which to report exceptions. The default value is  application/vnd.ogc.se_xml.


### <Describelayer>
The DescribeLayer operation is used primarily by clients that understand SLD-based WMS. In order to make an SLD one needs to know the structure of the data. WMS and WFS both have operations to do this, so the DescribeLayer operation just routes the client to the appropriate service. (in this instance the WMS call makes a reference to the WFS equivalent - for example) http://openmaps.gov.bc.ca/geo/pub/WHSE_FOREST_VEGETATION.VEG_COMP_LYR_R1_POLY/ows?service=wfs&version=1.0.0&request=DescribeFeatureType   

The standard parameters for the DescribeLayer operation are:
https://docs.geoserver.org/latest/en/user/services/wms/reference.html#describeLayer

Parameter	|Required?|	Description
---------------------: | --- |--- |
service	|Yes|	Service name. Value is WMS.
version	|Yes|	Service version. Value is 1.1.1.
request	|Yes|	Operation name. Value is DescribeLayer.
layers	|Yes|	See GetMap
exceptions	|No|	Format in which to report exceptions. The default value is  application/vnd.ogc.se_xml.

### <GetLegendGraphic>
The GetLegendGraphic operation provides a mechanism for generating legend graphics as images, beyond the LegendURL reference of WMS Capabilities. 
It generates a legend based on the style defined on the server, or alternatively based on a user-supplied SLD. 
For more information on this operation and the various options that GeoServer supports see GetLegendGraphic. https://docs.geoserver.org/latest/en/user/services/wms/get_legend_graphic/index.html#get-legend-graphic


## Cross-Origin Resource Sharing (CORS)
CORS is enabled for any domain if you include an apikey with each request.

## WMS Example requests 

### GetCapabilities
WMS requests can be made for all layers or as a separate service each layer/feature class:<br>
http://openmaps.gov.bc.ca/geo/pub/wms?request=GetCapabilities
http://openmaps.gov.bc.ca/geo/pub/WHSE_FOREST_VEGETATION.VEG_COMP_LYR_R1_POLY/wms?request=GetCapabilities


### GetMap
Give me Vegetation Coverage coloured by age for a given bounding box:<br>
http://openmaps.gov.bc.ca/geo/pub/wms?SERVICE=WMS&VERSION=1.1.1&REQUEST=GetMap&FORMAT=application/openlayers&TRANSPARENT=true&STYLES=1748&LAYERS=pub%3AWHSE_FOREST_VEGETATION.VEG_COMP_LYR_R1_POLY&SRS=EPSG%3A3005&WIDTH=512&HEIGHT=440&BBOX=1069159.051186301%2C1050414.7675306%2C1074045.5446851396%2C1054614.0978811644<br><br>
Give me all the Dams in the Cariboo District, as KML points:<br>
https://openmaps.gov.bc.ca/geo/pub/wms?service=wms&request=GetMap&version=1.1.1&format=application/vnd.google-earth.kml+xml&layers=WHSE_WATER_MANAGEMENT.WRIS_DAMS_PUBLIC_SVW&styles=3959&height=2048&width=2048&transparent=false&srs=EPSG:4326&format_options=AUTOFIT:true;KMATTR:true;KMPLACEMARK:true;KMSCORE:100;MODE:download;SUPEROVERLAY:false&CQL_FILTER=%22REGION_NAME%22=%27CARIBOO%27&bbox=-139.46653152270716,39.3982201780243,-110.0651303636062,68.79962133712526<br><br>
Give me the Bathymetry layer coloured by depth at the extent of the province of BC:<br>
http://openmaps.gov.bc.ca/geo/pub/wms?service=WMS&version=1.3.0&request=GetMap&layers=pub:WHSE_BASEMAPPING.DBM_BC_7H_MIL_BATHYMETRC_POLY&styles=&bbox=273875.663,362346.895,1870571.76,1735670.856&width=512&height=440&crs=EPSG:3005&format=image/png<br><br>



### GetFeatureInfo
Give me all the attributes of a vegetated polygon at a specific location in html output: <br>
http://openmaps.gov.bc.ca/geo/pub/wms?SERVICE=WMS&VERSION=1.1.1&REQUEST=GetFeatureInfo&FORMAT=image%2Fpng&TRANSPARENT=true&QUERY_LAYERS=pub%3AWHSE_FOREST_VEGETATION.VEG_COMP_LYR_R1_POLY&STYLES=1748&LAYERS=pub%3AWHSE_FOREST_VEGETATION.VEG_COMP_LYR_R1_POLY&exceptions=application%2Fvnd.ogc.se_inimage&INFO_FORMAT=text%2Fhtml&FEATURE_COUNT=50&X=50&Y=50&SRS=EPSG%3A3005&WIDTH=101&HEIGHT=101&BBOX=1071597.5259694126%2C1050887.1921950383%2C1072561.4631635193%2C1051851.129389145<br><br> And the same in json output:
http://openmaps.gov.bc.ca/geo/pub/wms?SERVICE=WMS&VERSION=1.1.1&REQUEST=GetFeatureInfo&FORMAT=image%2Fpng&TRANSPARENT=true&QUERY_LAYERS=pub%3AWHSE_FOREST_VEGETATION.VEG_COMP_LYR_R1_POLY&STYLES=1748&LAYERS=pub%3AWHSE_FOREST_VEGETATION.VEG_COMP_LYR_R1_POLY&exceptions=application%2Fvnd.ogc.se_inimage&INFO_FORMAT=application%2Fjson&FEATURE_COUNT=50&X=50&Y=50&SRS=EPSG%3A3005&WIDTH=101&HEIGHT=101&BBOX=1071597.5259694126%2C1050887.1921950383%2C1072561.4631635193%2C1051851.129389145

### Describelayer
The describelayer request like this - http://openmaps.gov.bc.ca/geo/pub/wms?service=WMS&version=1.1.1&request=DescribeLayer&layers=pub:WHSE_FOREST_VEGETATION.VEG_COMP_LYR_R1_POLY
will respond in a reference to the WFS DescribeFeatureType request which lists the attributes/fields of the spatial layer requested- 
http://openmaps.gov.bc.ca/geo/pub/WHSE_FOREST_VEGETATION.VEG_COMP_LYR_R1_POLY/ows?service=wfs&version=1.0.0&request=DescribeFeatureType   
 
### GetLegendGraphic 
Give me the legend for the Style associated with Age in the vegetative land cover layer (NOTE: style code and name is in the getcapabilities response):
http://openmaps.gov.bc.ca/geo/pub/WHSE_FOREST_VEGETATION.VEG_COMP_LYR_R1_POLY/ows?service=WMS&request=GetLegendGraphic&format=image%2Fpng&width=20&height=20&layer=pub%3AWHSE_FOREST_VEGETATION.VEG_COMP_LYR_R1_POLY&style=1748

