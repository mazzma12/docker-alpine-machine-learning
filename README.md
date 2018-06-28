# Dockerfiles

Host Dockerfiles running Alpine images with Python for machine learning projects.
Alpine images are 30x times lighter than Debian images, which save a significantly amount of time (and money) during deploy: [Check it out](https://nickjanetakis.com/blog/the-3-biggest-wins-when-using-alpine-as-a-base-docker-imagA)


# Images

## python-core

Alpine:3.7 base image with python 3.6.5 and python-dev 

## ml-core 

Alpine:3.7 base image with python with Python 3.6 and 4 packages : 
* numpy
* scipy
* sklearn
* pandas 

## ml-geo

base image like ml-core with tools to deals with geodata : 
* GDAL
* geos
* fiona
* shapely
* geopandas 
* Rtree and its C++ dependency libspatialindex 


