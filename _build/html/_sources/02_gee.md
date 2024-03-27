# Free and Open Source vs. No Code

```{contents}
:local:
:depth: 2
```

## TL;DR


## Scenario
You need to do a quick exploratory data analysis of above and below ground carbon biomass for an area of interest near Lake Tahoe in the Central Sierra, California. A donor has asked you to this by end of the week. You go down the hall to the right where the map plotters are and ask the GIS guy. He peers up above his array of screens, wipes his mouth and goatee with a bit of his somewhat tatty Missile Command t-shirt and tries not to look exasperated when you ask him about some help. He's busy, he's working on a dozen other projects, he might be able to get you something in 2 weeks. 

The GIS guy mumbles look it up on Google Earth Engine. Yes you've heard of this, you're thinking Google Earth, it will be easy, but you check out Earth Engine and find a console with an empty map and the required Javascript codingis another universe, let alone language, to you. 

## DIY
OK, so this is even if you have a GIS guy down the hall with the defunct plotter. Many of you are too small and can't afford to pay someone or a consultant to do your geospatial analysis. So how do you DIY it? Free and open source software (FOSS), or FOSS4G when it refers to free and open source geospatial, is readily available, and, much better than a costly yearly subscription fee it is free. Contrary to popular opinion, it's not that difficult to start. Here's how:

Once you log into [Earth Engine](https://code.earthengine.google.com/), search for 'carbon' in the search bar at the top of the page. Select Global Aboveground and Belowground Carbon Density Maps. In the lower left of the page select see example: 

![aboveground | 500](https://i.imgur.com/z3xt9WD.png)

That will open a new code editor with the following javascript code:
```javascript
var dataset = ee.ImageCollection('NASA/ORNL/biomass_carbon_density/v1');

var visualization = {
  bands: ['agb'],
  min: -50.0,
  max: 80.0,
  palette: ['d9f0a3', 'addd8e', '78c679', '41ab5d', '238443', '005a32']
};

Map.setCenter(-60.0, 7.0, 4);

Map.addLayer(dataset, visualization, 'Aboveground biomass carbon');
```
 
 Hitting run will gives you a basic map of aboveground biomass:
 
![](https://i.imgur.com/m7dOCRr.png)

You can zoom in or out and click the layer on or off. Selecting the inspector in the upper right panel will allow you to click on any pixel in the map and get the information from the raster displayed:

![](https://i.imgur.com/LZcLWWh.png)

In this case the Mosaic image has 4 bands related to above and belowground caronb, the lat lon of the point clicked is (-68.16, 1.83) and the pixel size is 10km. Add in the belowground (bgb) carbon with

```javascript
var vis_b = {
  bands: ['bgb'],
  min: -50.0,
  max: 80.0,
  palette: ['D6BCB1', 'AB8574', '784E3D', '3D2216', '26140C', '000000']
};
Map.addLayer(dataset, vis_b, 'Belowground biomass carbon');
```

You can zoom the map out and center to approximately world level by changing line 10 of the code:

```javascript
Map.setCenter(-24.83, 19.88, 2)
```

Hit run or control + enter (windows) or command + enter (mac) to more rapidly run the new code. Cleaning up the code and adding comments with // to explain what each section does and standard coding practice (the double back slash allows you to add a comment that does not run when the code executes and is equivalent to the hash tag for python). You can comment out any line of code by clicking on the line and typing control/command enter on your keyboard. Reorganiztion will give you the following code and image:

```javascript
// Add global carbon density map
var dataset = ee.ImageCollection('NASA/ORNL/biomass_carbon_density/v1');

// Add visual parameter variables
var vis_a = {
  bands: ['agb'],
  min: -50.0,
  max: 80.0,
  palette: ['d9f0a3', 'addd8e', '78c679', '41ab5d', '238443', '005a32']
};

var vis_b = {
  bands: ['bgb'],
  min: -50.0,
  max: 80.0,
  palette: ['D6BCB1', 'AB8574', '784E3D', '3D2216', '26140C', '000000']
};

// Center map and add layers
Map.setCenter(-24.83, 19.88, 2);
Map.addLayer(dataset, vis_a, 'Aboveground biomass carbon');
Map.addLayer(dataset, vis_b, 'Belowground biomass carbon');
```
![](https://i.imgur.com/7hkdrV0.png)
 Note the belowground now shows up on top--turn off by clicking the checkmark in the layers tab or to view it first reverse the order of the code, e.g., line 22 moved to 21. You can also control layers by altering the Map.addLayer function adding False to add the layer, but it won't be turned on in the map (until you click it on):
 
```javascript
Map.addLayer(dataset, vis_b, 'Belowground biomass carbon', false);
```
## AOI Clip
Getting back to the original request, clipping the dataset to an area of interest, in this case around Lake Tahoe. You can also clip to existing datasets such as Tiger Lines for states or by country. Clipping by specific shape files requires you to upload the vector as a shape file into Earth Engine. This is fiddly and not as easy as drag/dropping into Arc or QGIS or online resources such as Felt or Earth Blox. We'll keep this to a simple box.

Turn off the map layers and zoom into your area of interest AOI, in this case near Lake Tahoe

![](https://i.imgur.com/wqAZ3bC.png)

Click on the square 'Draw a Rectangle' button highlighted and draft a square in the area

![](https://i.imgur.com/Il9YmB4.png)
That will add a layer in the imports section of the top of your code called geometry. I have changed the name by double clicking the layer name to call it bbox, but you can use the name geometry, just remember to change the name from the code offered below from bbox to geometry. Under the dataset variable change the code to the ImageCollection as follows. Note the end of the dataset line no longer has a semicolon but that has been moved to the function added to the dataset variable to clip to the bbox (replace this with geometry if you don't rename the added rectangle).

```javascript
// Add global carbon density map
var dataset = ee.ImageCollection('NASA/ORNL/biomass_carbon_density/v1')
                    .map(function(image){return image.clip(bbox)});

```

Run everything again and turn of the geometry by clicking on geometry imports and unchecking the AOI you added. 

One thing that might help this map is changing the basemap. This can be relatively simply or complicated in Earth Engine. To get to map layers you want, is simple but requires a lot of code lines. Going to snazzymap.com, selecting a basemap you like and copying the javascript can help with this. We'll choose simple gray. Adding it to the map adds about 130 lines of code to your map, but don't be scared, you can paste this below the main part of your code copy everything including the brackets and add a couple of code lines as follows

```javascript
// Change basemap to snazzymaps.com subtle gray
var gray = [
'all the lines of javascript code'
]
Map.setOptions('Gray', {gray: gray});
```
Once you hit enter you'll have 'gray' as an additional map option in the middle right portion of your screen to go along with the default 'Map' and 'Satellite' options. Earth Engine has other defaults you can add without a lot of code and can be found [here](https://developers.google.com/earth-engine/tutorials/community/customizing-base-map-styles).

![](https://i.imgur.com/X16YsFz.png)

## Geospatial EDA
This type of exploratory data is extraordinarily useful to quickly look at large datasets of multiple types of biotic and abiotic data that the Earth Engine Catalog offers. You can search for any set in the catalog, click on sample and get an instantaneous view of the data. In reality, this is almost akin to no-code. It also computes and adds any layer in the cloud, so it doesn't matter how old or slow your computer is, you can run everything on Google servers. This is a tremendous advantage, especially when you get to computationally complex analysis.

## Resources
If you want to get serious about Google Earth Engine, I highly recommend you read the free online Cloud-Based Remote Sensing with Google Earth Engine [book](https://www.eefabook.org/) by Rebecca Moore et al. This book is worth reading as a remote sensing textbook and guides you through how to nearly everything from basic to complex using Earth Engine. For answers to many other questions the Google Earth Engine Developers [listserv](https://groups.google.com/g/google-earth-engine-developers) is quite useful.