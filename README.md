# singletrail-tiles

Additional map overlays for https://gpx.studio that highlight singletrails for mountain biking.

## Introduction

I find https://gpx.studio tremendously useful to prepare GPX tracks for my hiking and cycling adventures. However, I was
missing overlays that show single trails for mountain biking along with their riding difficulties according to
[OSM's MTB Scale](https://wiki.openstreetmap.org/wiki/Key:mtb:scale). This project provides additional map layers that
can be used as custom MTB singletrail overlays in https://gpx.studio.

![Singletrail Map](./img/singletrail-map.png)

## Serving the Singletrail Map Tiles

The singletrail map is served to https://gpx.studio as `.png` tiles by a map tile server that is running on your local computer.
https://gpx.studio will automatically load the `.png` tiles as needed when scrolling the map.

### Prerequisites

In order to run this project locally you need `git` as well as `docker` and `docker-compose` installed on your computer.

Additionally, an `.mbtiles` file containing the map data must be generated first for the desired area.
Use my [openmaptiles-singletrailmap project](https://github.com/joe-akeem/openmaptiles-singletrailmap) to create an `.mbtiles`
file for the desired area. For example to create suitable data for the area of the whole alps, you would run

```bash
$ ./quickstart.sh europe/alps
```

in that project. The resulting `tiles.mbtiles` file must then be copied to this project's `data` folder.

### Running the Tile Server

1. Clone the project to your local computer:

```bash
$ git clone https://github.com/joe-akeem/singletrail-tiles.git
$ cd singletrail-tiles
``` 

2. Copy the `tiles.mbtiles` file (see above):

```bash
$ cp ../openmaptiles-singletrailmap/data/tiles.mbtiles data
``` 

3. Run the tile server:

```bash
$ docker-compose up
``` 

In order to verify that the tile server is running properly visit http://localhost:8080/. You should be able to inspect
the data as well as a list of map styles.

### Adding the custom Singletrail Basemap and Overlay to gpx.studio

TODO

### Reading the Singletrail Map

The paths and trails that are suited for mountain biking will be highlighted in the basemap as well as the overlay
according to the following color codes:

| TODO |  |
|---|---|
|  |  |
|  |  |
