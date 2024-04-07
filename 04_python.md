# Geospatial Python

Python is widely applicable and used in the geospatial community. ArcGIS Pro has a python package called Arcpy and QGIS has a packaged named PYQGIS. However, Arcpy is not easy or intuitive to use, even though it's a python package. I tried it, I read an entire book on it, I struggled using the notebooks, which are slow to load and not easy to link to VS Code.

Then I found geemap and leafmap, incredible user friendly python packages developed by Qiushen Wu and available on github at GISWQS, along with clear tutorials at leafmap.org and geemap.org and videos at [Open Geospatial Solutions](https://www.youtube.com/@giswqs). Many analyses require only one line of code. Here, geospatial analysis using scripts just clicked for me and it opened up another world to using Python to rapidly analyse data.

## Getting Started

See the 5.4 Resources for more information.

## Sentinel-2 Data

Let's look at a simple example from a github [gist](https://gist.github.com/alexgleith/dc49156aab4b9270b0a0f145bd7fa0ce) posted by Alex Leith. Open [colab](https://colab.research.google.com/) and click the blue 'New notebook' button.

```{note} 'Uncommenting' a line in python means removing the hashtag before the command, or click on the line then click control or command plus back slash (/).
```

Install the dependencies:

```python
# Uncomment line below to install pystac and odc
# pip install pystac-client odc-stac odc-geo
```
Access data and create a bounding box:

```python
# Earth search is managed by Element-84 and provides access to a wide range of data sources
client = Client.open("https://earth-search.aws.element84.com/v1")
collection = "sentinel-2-l2a"

# Create a bounding box centered near New River Lagoon in Tasmania
# The bounding box is lower-left x, lower-left y, upper-right x, upper-right y
bbox = [146.5, -43.6, 146.7, -43.4]
```
Search and load the data:

```python
# Datetime can be a single date, like YEAR, YEAR-MONTH or YEAR-MONTH-DAY
# or a range, like YEAR-MONTH/YEAR-MONTH
datetime = "2023-12"

# Run a lazy-loaded search of the STAC API
search = client.search(collections=[collection], bbox=bbox, datetime=datetime)

# Pass the search results to the load function, which will lazy-load the data
data = load(search.items(), bbox=bbox, groupby="solar_day", chunks={}, crs="EPSG:8857", resolution=10)
```
Visualize it:

```python
# Visualize the data
# Time=2 is an arbitrary time slice picked because there's few clouds
data[["red", "green", "blue"]].isel(time=2).to_array().plot.imshow(vmin=0, vmax=1500)

# Alternately, we could visualise using odc.geo.xr's explore function
# data.isel(time=2).explore(bands=["red", "green", "blue"], vmin=0, vmax=1500)
```
Sentinel-2 true color image:

![](https://i.imgur.com/ea6GCzY.png)

The access and sharing of this code is another example of why free and open source is special, the community is willing to share it it is reproduceable, and easily modified to meet your needs. Thanks to Alex Leith for sharing this.

## Geemap
Compared to using Javascript, Geemap is a much easier way to access, analyze, and visualize Earth Engine data all within a python package environment developed by [Quisheng Wu](https://github.com/giswqs).

```{admonition} Getting Started
Watch this [installation video](https://www.youtube.com/watch?v=gyQ6wBqYGks&list=PLAxJ4-o7ZoPeXzIjOJx3vBF0ftKlcYH9J&index=3) followed by this [vs code and github](https://www.youtube.com/watch?v=gyQ6wBqYGks&list=PLAxJ4-o7ZoPeXzIjOJx3vBF0ftKlcYH9J&index=3) video. If you already have an IDE, miniconda, and virtual env's installed, go to the Geemap [installation](https://geemap.org/installation/) page.
```

Let's look at how to visualize the same map from Chapter 2 using Geemap. Open a jupyter notebook and add the following codeblock:

```python
# Import geemap and create an interactive map
import ee
import geemap
geemap.ee_initialize()
m = geemap.Map()
m
```
That will will give you a generic world map and several map widgets:

![](https://i.imgur.com/hKl0roO.png)

In the upper right corner of the map, click the wrench icon then click the two encircling arrows box (bottom row, middle) to open the Javascript to Python code converter:

![](https://i.imgur.com/XXdWssh.png)

Go to your Earth Engine File, open the code from Chapter 2 and select all and paste it into the converter. Click on convert. The code is copied to the clipboard. Paste it into a new codeblock and comment out the definition function on lines 4-6:

```python
# Add global carbon density map
dataset = ee.ImageCollection('NASA/ORNL/biomass_carbon_density/v1')

# def func_rif(image)return image.clip(bbox)};: \
#                     .map(function(image){return image.clip(bbox)} \
#                     .map(func_rif)

# Add visual parameter variables
vis_a = {
  'bands': ['agb'],
  'min': -50.0,
  'max': 80.0,
  'palette': ['d9f0a3', 'addd8e', '78c679', '41ab5d', '238443', '005a32']
}

vis_b = {
  'bands': ['bgb'],
  'min': -50.0,
  'max': 80.0,
  'palette': ['D6BCB1', 'AB8574', '784E3D', '3D2216', '26140C', '000000']
}

# Center map and add layers
m.setCenter(-120.2348, 38.8744, 9)
m.addLayer(dataset, vis_a, 'Aboveground biomass carbon')
m.addLayer(dataset, vis_b, 'Belowground biomass carbon')
```
Running that code block will give you the following:

![](https://i.imgur.com/uAQ9wBz.jpeg)

If you return to the wrench icon and select layers to the left you can switch layers on and off

![](https://i.imgur.com/RzJfVjV.png)



## Resources

- [Geemap](https://geemap.org/) has a webpage, book, tutorials, API, and much more to support this excellent Python package.
- [Leafmap](https://leafmap.org/) is a Python package for geospatial analysis in a Jupyter environment. Superb documentation, tutorials, and ease of use.
- [Open Geospatial Solutions](https://github.com/opengeos) host many open-source geospatial software projects and datasets.
- [Spatial Thoughts](https:spatialthoughts.com) run by Ujaval Gandhi has a free course called [Python Foundation for Spatial Analysis](https://courses.spatialthoughts.com/python-foundation.html). The site has many other free and paid courses and tutorials for geospatial analysis.
- [Geocomputation with Python](https://py.geocompx.org/) is an open source book inspired by the FOSS4G movement. 
