<h1 align="center">
  <br>
  <a href="https://github.com/r-spatial/rgee/"><img src="https://user-images.githubusercontent.com/16768318/118376965-5f7dca80-b5cb-11eb-9a82-47876680a3e6.png" alt="Markdownify" width="200"></a>
  <a href="https://github.com/r-earthengine/rgeeExtra/"><img src="https://user-images.githubusercontent.com/16768318/118376968-63a9e800-b5cb-11eb-83e7-3f36299e17cb.png" alt="Markdownify" width="200"></a>
  <a href="https://r-earthengine.com/rgeebook/"><img src="https://user-images.githubusercontent.com/16768318/118376966-60aef780-b5cb-11eb-8df2-ca70dcfe04c5.png" alt="Markdownify" width="200"></a>
  <br>
  rgee: Google Earth Engine for R
  <br>
</h1>


<h4 align="center">rgee is an R binding package for calling <a href="https://developers.google.com/earth-engine/" target="_blank">Google Earth Engine API</a> from within R. <a href="https://r-spatial.github.io/rgee/reference/index.html" target="_blank">Various functions</a> are implemented to simplify the connection with the R spatial ecosystem.</h4>

<p align="center">
<a href="https://colab.research.google.com/github/r-spatial/rgee/blob/examples/rgee_colab.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open in Colab" title="Open and Execute in Google Colaboratory"></a>
<a href="https://github.com/r-spatial/rgee/actions"><img src="https://github.com/r-spatial/rgee/workflows/R-CMD-check/badge.svg" alt="R build status"></a>
<a href="https://www.repostatus.org/#active"><img src="https://www.repostatus.org/badges/latest/active.svg" alt="Project Status: Active – The project has reached a stable, usable
state and is being actively
developed."></a>
<a href="https://codecov.io/gh/r-spatial/rgee"><img src="https://codecov.io/gh/r-spatial/rgee/branch/master/graph/badge.svg" alt="codecov"></a>
<a href="https://opensource.org/licenses/Apache-2.0"><img src="https://img.shields.io/badge/License-Apache%202.0-blue.svg" alt="License"></a>
<a href="https://www.tidyverse.org/lifecycle/#maturing"><img src="https://img.shields.io/badge/lifecycle-maturing-blue.svg" alt="lifecycle"></a>
<a href="https://joss.theoj.org/papers/aea42ddddd79df480a858bc1e51857fc"><img src="https://joss.theoj.org/papers/aea42ddddd79df480a858bc1e51857fc/status.svg" alt="status"></a>
<a href="https://cran.r-project.org/package=rgee"><img src="https://www.r-pkg.org/badges/version/rgee" alt="CRAN
status"></a>
<a href="https://doi.org/10.5281/zenodo.3945409"><img src="https://zenodo.org/badge/DOI/10.5281/zenodo.3945409.svg" alt="DOI"></a>
<a href="https://cran.r-project.org/web/packages/rgee/index.html"><img src="https://cranlogs.r-pkg.org/badges/rgee" alt="CRAN status"></a>
<br>
<a href="https://www.buymeacoffee.com/csay" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: 41px !important;width: 174px !important;box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;" ></a>
</p>

<p align="center">
  •
  <a href="#installation">Installation</a> &nbsp;•
  <a href="#hello-world">Hello World</a> &nbsp;•
  <a href="#how-does-rgee-work">How does rgee work?</a> &nbsp;•
  <a href="#quick-start-users-guide-for-rgee">Guides</a> &nbsp;•
  <a href="#contributing-guide">Contributing</a> &nbsp;•
  <a href="#share-the-love">Citation</a> &nbsp;•
  <a href="#credits">Credits</a>
</p>

## What is Google Earth Engine?

[Google Earth Engine](https://earthengine.google.com/) is a cloud-based platform that enables users to access a petabyte-scale archive of remote sensing data and conduct geospatial analysis on Google's infrastructure. Currently, Google offers support only for Python and JavaScript. `rgee` fills that gap **by providing support for R!**. Below, you will find a comparison between the syntax of `rgee` and the two other client libraries supported by Google.

<table>
<tr>
<th> JS (Code Editor) </th>
<th> Python </th>
<th> R </th>
</tr>
<tr>
<td>

``` javascript
var db = 'CGIAR/SRTM90_V4'
var image = ee.Image(db)
print(image.bandNames())
#> 'elevation'
```

</td>
<td>

``` python
import ee
ee.Initialize(project = "my-project-id")
db = 'CGIAR/SRTM90_V4'
image = ee.Image(db)
image.bandNames().getInfo()
#> [u'elevation']
```

</td>
<td>

``` r
library(rgee)
ee_Initialize(project = "my-project-id")
db <- 'CGIAR/SRTM90_V4'
image <- ee$Image(db)
image$bandNames()$getInfo()
#> [1] "elevation"
```
</td>
</tr>
</table>


**Quite similar, isn't it?**. However, additional more minor changes should be considered when using Google Earth Engine with R. Please check the [consideration section](https://r-spatial.github.io/rgee/articles/rgee02.html) before you start coding!

## How to use

**NOTE: Create a [.Renviron file](https://cran.r-project.org/web/packages/startup/vignettes/startup-intro.html) file to prevent setting RETICULATE_PYTHON and EARTHENGINE_GCLOUD every time you authenticate/init your account.**

``` r
library(rgee)

# Set your Python ENV
Sys.setenv("RETICULATE_PYTHON" = "/C:Users/Zekuean/miniconda3/envs/rgee_py")

# Set Google Cloud SDK. Only need it the first time you log in. 
Sys.setenv("EARTHENGINE_GCLOUD" = "C:/Program Files (x86)/Google/Cloud SDK/google-cloud-sdk/bin/")
ee_Authenticate()

# Initialize your Earth Engine Session 
ee_Initialize(project = "my-project-id")
```

### Earth Engine initialization 

You will need to create and register a Google Cloud project to use Earth Engine (via rgee).
See the following "Installation" section for instructions. The ID of the Cloud project will need to
be supplied to `ee_Initialize` each time you start a new rgee session. Whenever you see "my-project-id"
in rgee example code, replace the string with your specific Cloud project ID. For more information on
these topics see about [Earth Engine access](https://developers.google.com/earth-engine/guides/access)
and [authentication and inialization](https://developers.google.com/earth-engine/guides/auth) pages. 

## Installation

Install from CRAN with:

``` r
install.packages("rgee")
```

Install the development versions from github with

``` r
library(remotes)
install_github("r-spatial/rgee")
```

Furthermore, `rgee` depends on [numpy](https://pypi.org/project/numpy/) and [earthengine-api](https://pypi.org/project/earthengine-api/) and it requires **[gcloud CLI](https://cloud.google.com/sdk/docs/install#deb)** to authenticate new users. The following example shows how to install and set up 'rgee' on a new Ubuntu computer. If you intend to use rgee on a server, please refer to this example in RStudio Cloud." -- https://posit.cloud/content/5175749)

Create and register a Google Cloud project. Follow the [Earth Engine access](
  https://developers.google.com/earth-engine/guides/access#get_access_to_earth_engine) instructions. 

``` r
install.packages(c("remotes", "googledrive"))
remotes::install_github("r-spatial/rgee")

library(rgee)

# Get the username
HOME <- Sys.getenv("HOME")

# 1. Install miniconda
reticulate::install_miniconda()

# 2. Install Google Cloud SDK
system("curl -sSL https://sdk.cloud.google.com | bash")

# 3 Set global parameters
Sys.setenv("RETICULATE_PYTHON" = sprintf("%s/C:Users/Zekuean/miniconda3/envs/rgee_py", HOME))
Sys.setenv("EARTHENGINE_GCLOUD" = sprintf("%s/google-cloud-sdk/bin/", HOME))

# 4 Install rgee Python dependencies
ee_install() 

# 5. Authenticate and initialize your Earth Engine session
# Replace "my-project-id" with the ID of the Cloud project you created above 
ee_Initialize(project = "my-project-id") 
```

There are three (3) different ways to install rgee Python dependencies:

1.  Use [**ee_install**](https://zekuwany.github.io/rgee/reference/ee_install.html) (Highly recommended for users with no experience with Python environments)

``` r
rgee::ee_install()
```

2.  Use [**ee_install_set_pyenv**](https://zekuwany.github.io/rgee/reference/ee_install_set_pyenv.html) (Recommended for users with experience in Python environments)

``` r
rgee::ee_install_set_pyenv(
  py_path = "/C:/Users/Zekuwan/miniconda3/envs/rgee_py", # Change it for your own Python PATH
  py_env = "rgee_py" # Change it for your own Python ENV
)
```

Take into account that the Python PATH you set must have earthengine-api and `numpy installed. The use of **miniconda/anaconda is mandatory for Windows users,** Linux and MacOS users could also use virtualenv. See [reticulate](https://rstudio.github.io/reticulate/articles/python_packages.html) documentation for more details.

If you are using MacOS or Linux, you can choose setting the Python PATH directly:

``` r
rgee::ee_install_set_pyenv(
  py_path = "/C:/Users/Zekuwan/miniconda3/envs/rgee_py",
  py_env = NULL
)
```

However, [**rgee::ee_install_upgrade**](https://r-spatial.github.io/rgee/reference/ee_install_upgrade.html) and [**reticulate::py_install**](https://rstudio.github.io/reticulate/reference/py_install.html) will not work until you have set up a Python ENV.

3.  Use the Python PATH setting support that offer [Rstudio v.1.4 \>](https://blog.rstudio.com/2020/10/07/rstudio-v1-4-preview-python-support/). See this [tutorial](https://github.com/r-spatial/rgee/tree/help/rstudio/).

After install `Python dependencies`, you might want to use the function below for checking the rgee status.

``` r
ee_check() # Check non-R dependencies
```

## Sync rgee with other Python packages

Integrate [rgee](https://r-spatial.github.io/rgee/) with [geemap](https://geemap.org/).

``` r
library(reticulate)
library(rgee)

# 1. Initialize the Python Environment
ee_Initialize(project = "my-project-id")

# 2. Install geemap in the same Python ENV that use rgee
py_install("geemap")
gm <- import("geemap")
```

Upgrade the [earthengine-api](https://pypi.org/project/earthengine-api/)

``` r
library(rgee)
ee_Initialize(project = "my-project-id")
ee_install_upgrade()
```

## Package Conventions

-   All `rgee` functions have the prefix ee\_. Auto-completion is your best ally :).
-   Full access to the Earth Engine API with the prefix [**ee\$...**](https://developers.google.com/earth-engine/).
-   Authenticate and Initialize the Earth Engine R API with [**ee_Initialize**](https://r-spatial.github.io/rgee/reference/ee_Initialize.html). It is necessary once per session!.
-   `rgee` is "pipe-friendly"; we re-export %\>% but do not require to use it.

## Hello World

### 1. Compute the trend of night-time lights ([JS version](https://github.com/google/earthengine-api/))

Authenticate and Initialize the Earth Engine R API.

``` r
library(rgee)
ee_Initialize(project = "my-project-id")
```

Let's create a new band containing the image date as years since 1991 by extracting the year of the image acquisition date and subtracting it from 1991.

``` r
createTimeBand <-function(img) {
  year <- ee$Date(img$get('system:time_start'))$get('year')$subtract(1991L) 
  ee$Image(year)$byte()$addBands(img)
}
```

Use your TimeBand function to map it over the [night-time lights collection](https://developers.google.com/earth-engine/datasets/catalog/NOAA_DMSP-OLS_NIGHTTIME_LIGHTS/).

``` r
collection <- ee$
  ImageCollection('NOAA/DMSP-OLS/NIGHTTIME_LIGHTS')$
  select('stable_lights')$
  map(createTimeBand)
```

Compute a linear fit over the series of values at each pixel, so that you can visualize the y-intercept as green, and the positive/negative slopes as red/blue.

``` r
col_reduce <- collection$reduce(ee$Reducer$linearFit())
col_reduce <- col_reduce$addBands(
  col_reduce$select('scale'))
ee_print(col_reduce)
```

Let's visualize our map!

``` r
Map$setCenter(9.08203, 47.39835, 3)
Map$addLayer(
  eeObject = col_reduce,
  visParams = list(
    bands = c("scale", "offset", "scale"),
    min = 0,
    max = c(0.18, 20, -0.18)
  ),
  name = "stable lights trend"
)
```

![rgee_01](https://user-images.githubusercontent.com/16768318/71565699-51e4a500-2aa9-11ea-83c3-9e1d32c82ba6.png)

### 2. Let's play with some precipitation values

Install and load `tidyverse` and `sf` R packages, and initialize the Earth Engine R API.

``` r
library(tidyverse)
library(rgee)
library(sf)

ee_Initialize(project = "my-project-id")
```

Read the `nc` shapefile.

``` r
nc <- st_read(system.file("shape/nc.shp", package = "sf"), quiet = TRUE)
```

We will use the [Terraclimate dataset](https://developers.google.com/earth-engine/datasets/catalog/IDAHO_EPSCOR_TERRACLIMATE/) to extract the monthly precipitation (Pr) from 2001

``` r
terraclimate <- ee$ImageCollection("IDAHO_EPSCOR/TERRACLIMATE") %>%
  ee$ImageCollection$filterDate("2001-01-01", "2002-01-01") %>%
  ee$ImageCollection$map(function(x) x$select("pr")) %>% # Select only precipitation bands
  ee$ImageCollection$toBands() %>% # from imagecollection to image
  ee$Image$rename(sprintf("PP_%02d",1:12)) # rename the bands of an image
```

`ee_extract` will help you to extract monthly precipitation values from the Terraclimate ImageCollection. `ee_extract` works similar to `raster::extract`, you just need to define: the ImageCollection object (x), the geometry (y), and a function to summarize the values (fun).

``` r
ee_nc_rain <- ee_extract(x = terraclimate, y = nc["NAME"], sf = FALSE)
```

Use ggplot2 to generate a beautiful static plot!

``` r
ee_nc_rain %>%
  pivot_longer(-NAME, names_to = "month", values_to = "pr") %>%
  mutate(month, month=gsub("PP_", "", month)) %>%
  ggplot(aes(x = month, y = pr, group = NAME, color = pr)) +
  geom_line(alpha = 0.4) +
  xlab("Month") +
  ylab("Precipitation (mm)") +
  theme_minimal()
```

<p align="center">

  <img src="https://user-images.githubusercontent.com/16768318/81945044-2cbd8280-95c3-11ea-9ef5-fd9f6fd5fe89.png" width="80%"/>

  </p>

  ### 3. Create an NDVI-animation ([JS version](https://developers.google.com/earth-engine/tutorials/community/modis-ndvi-time-series-animation/))

 Install and load `sf`. after that, initialize the Earth Engine R API.

``` r
library(magick)
library(rgee)
library(sf)

ee_Initialize(project = "my-project-id")
```

Define the regional bounds of animation frames and a mask to clip the NDVI data by.

``` r
mask <- system.file("shp/arequipa.shp", package = "rgee") %>%
  st_read(quiet = TRUE) %>%
  sf_as_ee()
region <- mask$geometry()$bounds()
```

Retrieve the MODIS Terra Vegetation Indices 16-Day Global 1km dataset as an `ee.ImageCollection` and then, select the NDVI band.

``` r
col <- ee$ImageCollection('MODIS/006/MOD13A2')$select('NDVI')
```

Group images by composite date

``` r
col <- col$map(function(img) {
  doy <- ee$Date(img$get('system:time_start'))$getRelative('day', 'year')
  img$set('doy', doy)
})
distinctDOY <- col$filterDate('2013-01-01', '2014-01-01')
```

Now, let's define a filter that identifies which images from the complete collection match the DOY from the distinct DOY collection.

``` r
filter <- ee$Filter$equals(leftField = 'doy', rightField = 'doy')
```

Define a join and convert the resulting FeatureCollection to an ImageCollection... it will take you only 2 lines of code!

``` r
join <- ee$Join$saveAll('doy_matches')
joinCol <- ee$ImageCollection(join$apply(distinctDOY, col, filter))
```

Apply median reduction among the matching DOY collections.

``` r
comp <- joinCol$map(function(img) {
  doyCol = ee$ImageCollection$fromImages(
    img$get('doy_matches')
  )
  doyCol$reduce(ee$Reducer$median())
})
```

Almost ready! but let's define RGB visualization parameters first.

``` r
visParams = list(
  min = 0.0,
  max = 9000.0,
  bands = "NDVI_median",
  palette = c(
    'FFFFFF', 'CE7E45', 'DF923D', 'F1B555', 'FCD163', '99B718', '74A901',
    '66A000', '529400', '3E8601', '207401', '056201', '004C00', '023B01',
    '012E01', '011D01', '011301'
  )
)
```

Create RGB visualization images for use as animation frames.

``` r
rgbVis <- comp$map(function(img) {
  do.call(img$visualize, visParams) %>%
    ee$Image$clip(mask)
})
```

Let's animate this. Define GIF visualization parameters.

``` r
gifParams <- list(
  region = region,
  dimensions = 600,
  crs = 'EPSG:3857',
  framesPerSecond = 10
)
```

Get month names

``` r
dates_modis_mabbr <- distinctDOY %>%
  ee_get_date_ic %>% # Get Image Collection dates
  '[['("time_start") %>% # Select time_start column
  lubridate::month() %>% # Get the month component of the datetime
  '['(month.abb, .) # subset around month abbreviations
```

And finally, use ee_utils_gif\_\* functions to render the GIF animation and add some texts.

``` r
animation <- ee_utils_gif_creator(rgbVis, gifParams, mode = "wb")
animation %>%
  ee_utils_gif_annotate(
    text = "NDVI: MODIS/006/MOD13A2",
    size = 15, color = "white",
    location = "+10+10"
  ) %>%
  ee_utils_gif_annotate(
    text = dates_modis_mabbr,
    size = 30,
    location = "+290+350",
    color = "white",
    font = "arial",
    boxcolor = "#000000"
  ) # -> animation_wtxt

# ee_utils_gif_save(animation_wtxt, path = "raster_as_ee.gif")
```

<p align="center">

  <img src="https://user-images.githubusercontent.com/16768318/77121867-203e0300-6a34-11ea-97ba-6bed74ef4300.gif"/>

  </p>

  ## How does rgee work?


  `rgee` is **not** a native Earth Engine API like the Javascript or Python client. Developing an Earth Engine API from scratch would create too much maintenance burden, especially considering that the API is in [active development](https://github.com/google/earthengine-api). So, how is it possible to run Earth Engine using R? the answer is [reticulate]! (https://rstudio.github.io/reticulate/). `reticulate` is an R package designed to allow seamless interoperability between R and Python. When an Earth Engine **request** is created in R, `reticulate` will translate this request into Python and pass it to the `Earth Engine Python API`, which  converts the request to a `JSON` format. Finally, the request is received by the GEE Platform through a Web REST API. The **response** will follow the same path in reverse.

![workflow](https://user-images.githubusercontent.com/16768318/71569603-3341d680-2ac8-11ea-8787-4dd1fbba326f.png)

## Code of Conduct

  Please note that the `rgee` project is released with a [Contributor Code of Conduct](CODE_OF_CONDUCT.md). By contributing to this project, you agree to abide by its terms.

## Contributing Guide

👍 Thanks for taking the time to contribute! 🎉👍 Please review our [Contributing Guide](CONTRIBUTING.md).

## Share the love

Enjoying **rgee**? Let others know about it! Share it on Twitter, LinkedIN or in a blog post to spread the word.

Using **rgee** for your scientific article? here's how you can cite it

``` r
citation("rgee")
To cite rgee in publications use:

  C Aybar, Q Wu, L Bautista, R Yali and A Barja (2020) rgee: An R
  package for interacting with Google Earth Engine Journal of Open
  Source Software URL https://github.com/r-spatial/rgee/.

A BibTeX entry for LaTeX users is

@Article{,
  title = {rgee: An R package for interacting with Google Earth Engine},
  author = {Cesar Aybar and Quisheng Wu and Lesly Bautista and Roy Yali and Antony Barja},
  journal = {Journal of Open Source Software},
  year = {2020},
}
```

## Credits

We want to offer a **special thanks** :raised_hands: :clap: to [**Justin Braaten**](https://github.com/jdbcode) for his wise and helpful comments in the whole development of **rgee**. As well, we would like to mention the following third-party R/Python packages for contributing indirectly to the improvement of rgee:

-   [**gee_asset_manager - Lukasz Tracewski**](https://github.com/tracek/gee_asset_manager/)
-   [**geeup - Samapriya Roy**](https://github.com/samapriya/geeup/)
-   [**geeadd - Samapriya Roy**](https://github.com/samapriya/gee_asset_manager_addon/)
-   [**cartoee - Kel Markert**](https://github.com/KMarkert/cartoee/)
-   [**geetools - Rodrigo E. Principe**](https://github.com/gee-community/gee_tools/)
-   [**landsat-extract-gee - Loïc Dutrieux**](https://github.com/loicdtx/landsat-extract-gee/)
-   [**earthEngineGrabR - JesJehle**](https://github.com/JesJehle/earthEngineGrabR/)
-   [**sf - Edzer Pebesma**](https://github.com/r-spatial/sf/)
-   [**stars - Edzer Pebesma**](https://github.com/r-spatial/stars/)
-   [**gdalcubes - Marius Appel**](https://github.com/appelmar/gdalcubes/)
