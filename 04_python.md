# Geospatial Python

Python is widely applicable and used in the geospatial community. ArcGIS Pro has a python package called Arcpy and QGIS has a packaged named PYQGIS. However, Arcpy is not easy or intuitive to use, even though it's a python package. I tried it, I read an entire book on it, I struggled using the notebooks, which are slow to load and not easy to link to VS Code.

Then I found geemap and leafmap, incredible user friendly python packages developed by Qiushen Wu and available on github at GISWQS, along with clear tutorials at leafmap.org and geemap.org and videos at [Open Geospatial Solutions](https://www.youtube.com/@giswqs). Many analyses require only one line of code. Here, geospatial analysis using scripts just clicked for me and it opened up another world to using Python to rapidly analyse data.

## Getting Started

See the 5.4 Resources for more information.

## Sentinel-2 Data

Let's look at a simple example from a github [gist](https://gist.github.com/alexgleith/dc49156aab4b9270b0a0f145bd7fa0ce) posted by Alex Leith. Note when programmers say to uncomment a line, in python it means removing the hashtag before the command, or click on the line and hit control or command /. Install the dependencies:

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
Search and load the data

```python
# Datetime can be a single date, like YEAR, YEAR-MONTH or YEAR-MONTH-DAY
# or a range, like YEAR-MONTH/YEAR-MONTH
datetime = "2023-12"

# Run a lazy-loaded search of the STAC API
search = client.search(collections=[collection], bbox=bbox, datetime=datetime)

# Pass the search results to the load function, which will lazy-load the data
data = load(search.items(), bbox=bbox, groupby="solar_day", chunks={}, crs="EPSG:8857", resolution=10)
```
Visualize it!

```python
# Visualize the data
# Time=2 is an arbitrary time slice picked because there's few clouds
data[["red", "green", "blue"]].isel(time=2).to_array().plot.imshow(vmin=0, vmax=1500)

# Alternately, we could visualise using odc.geo.xr's explore function
# data.isel(time=2).explore(bands=["red", "green", "blue"], vmin=0, vmax=1500)
```
To give you this Sentinel-2 imagec

![](https://i.imgur.com/ea6GCzY.png)

The access and sharing of this code is another example of why free and open source is special, the community is willing to share it it is reproduceable, and easily modified to meet your needs. Thanks to Alex Leith for sharing this.

## Geemap





## Resources

- **Geemap,**.
- **Leafmap,**.
- **Opengeos**.
- **Ujaval Gandhi, Spatial Thoughts**. Free course on [Python Foundation for Spatial Analysis](https://courses.spatialthoughts.com/python-foundation.html).
- **Dorman et al**. [Geocomputation with Python](https://py.geocompx.org/) is an open source book inspired by the FOSS4G movement. 
