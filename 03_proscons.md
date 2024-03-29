
# Pros & Cons

## Open source vs. paid
Navigating the world of open source and subscription-based geospatial tools is challenging. It can also be costly when you pay for software, imagery, and technicians to conduct your analyses. 

## False dichotomy
In truth, it's a false dichotomy to pit FOSS vs. paid geospatial tools. Your work flow may involve data analysis in GEE then import datasets into ArcGIS pro for further analysis and creating layouts, web apps or online maps. You may find that working with many government agencies requires use of Arc products because it's what their employees know and their agencies have license to use (*see the [Modern Geospatial](https://mapscaping.com/podcast/modern-geospatial/) Mapscaping Podcast on using FOSS with different clients*). 

## Table
{numref}`example-table` is a non-exhaustive and very brief summary of the more commonly used geospatial sofware products available.

```{list-table} Pros and Cons of geospatial software providers
:header-rows: 1
:name: proscons
```
Table 1: 

| SOFTWARE | PROS      | CONS            | Price |
| :------- | :------   | :-------        | ----: |
| ArcGIS Pro | Use, Support, Vis | Price, Credits, Notebooks | $100-6,000+/user/yr |
| CARTO | Use, Vis, Data | Haven't used | High |
| DuckDB | Use, Fast, Documentation | Best for large datasets | Free |
| Earth Blox | Use, Data, Solutions | Price, Slow | Unclear: ~2,400/user/yr |
| Felt | Use, Vis, QGIS integ | Price, Sharing, Long-term? | $360-1,080/user/yr |
| GEE  | Cloud, Support, Colab | Javascript, Cost | Free to mucho |
| Geemap | Use, Support, GEE/Colab integ | Setup in windows | Free |
| Leafmap | Use, Feature rich, one line code solutions | CLI, IDE setup in Windows | Free |
| Planet | Vis, Data | Haven't used | High |
| Post-GIS | Use, Fast, QGIS integ, Industry Standard | UI | Free |
| QGIS | Free, Community, Analysis | Vis, Crashing, Future? | Free |
| R, RStudio | Use, Plugins, Academic standard | Slow at times, Vis | Free |

## Discussion & Caveats
Use means ease of use. For software I marked as slow, Earth Blox was sometimes slow when I tested it nearly one year ago, it may be much better now. RStudio can sometimes be slow but typically for much larger datasets. Vis = visualization is very good and feature rich. Support and documentation indicate good or bad support, typically the software documentation and tutorials are very good or the company or software provider is quick to respond to bugs or questions. 

Pricing is difficult since several of these companies are opaque about their pricing, ask you to submit information for a quote, or want you to participate in a call to give you bespoke pricing quotes. I've tried to add information on pricing when I have an approximate or specific idea.

One common thread with pros and cons and software use is that free is good. So is long-term viability of a provider. You don't want to put all your assets, analysis, and projects into a proprietary system to have the company later go belly up. Then your data and analysis is difficult to transfer. A common thread with several of the providers such as QGIS, Google Earth Engine (GEE)