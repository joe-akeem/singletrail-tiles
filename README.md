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

### Adding the custom Singletrail Basemap and Overlays to gpx.studio

From the main page at http://localhost:8080/ you can navigate to the TileJSON definitions of each style. Each style has
an individual URL under which the map tiles are served. They look something like `http://localhost:8080/styles/mtb/{z}/{x}/{y}.png`.
These URLs are the ones gpx.studio needs to request the map tiles depending on the currently displayed section of the map.

Here is a list of the provided styles along with the URLs that are relevant to gpx.studio:

|Style|Description|Tiles URL|
|---|---|---|
| mtb | A map style that can be used as an _overlay_ to highlight the paths on the map that have an `mtb_scale` tag according to OSM. The paths are highlighted using different colors depending on the [riding difficulty](https://wiki.openstreetmap.org/wiki/Key:mtb:scale) (green for S0, blue for S1 etc.) |http://localhost:8080/styles/mtb/{z}/{x}/{y}.png |
| mtb_potential | A map style that can be used as an _overlay_ to highlight the paths on the map that might be suitable for MTB riding but that don't have an `mtb_scale` tag according to OSM. The paths are highlighted using different colors depending on the how likely they are suited (based on some other tags like hiking difficulty etc. Orange means higher potential than yellow) |http://localhost:8080/styles/mtb_potential/{z}/{x}/{y}.png |
| singletrailmap | A map that can be used as a _base map_. It is a complete map style that is similar to the swisstopo style and that has additionally the singletrails highlighted as in the `mtb`style above. This map can be combined with other overlays in gpx.studio. | http://localhost:8080/styles/singletrailmap/{z}/{x}/{y}.png |

Please refer to the [gpx.studio User Guide](https://gpx.studio/about.html#custom-layer) for instructions on how to add a
custom map layer. This is where the tiles URLs above become relevant. If you generated the `tiles.mbtiles` file using the
[openmaptiles-singletrailmap project](https://github.com/joe-akeem/openmaptiles-singletrailmap) as described above, the
max zoom level has to be set to `20` (this information can also be found in TileJSON e.g. at http://localhost:8080/styles/mtb.json
in case of the `mtb` style). Make sure you choose the right layer type. `Overlay` is meaningful for the `mtb`and `mtb_potential`
styles while `singletrailmap` is more suitable as `Basemap`. Here's an example on how to add the `mtb` layer:

![Add custom Overlay](img/add_custom_layer.png)

Once the custom layers have been added in gpx.studio, they can be used to highlight trails best suited for mountain biking
and according to their difficulties. They work like any other layer and can be combined with your favorite base map and other overlays:

![Custom Layers](img/custom_layers.png)

