# TileServer

## mbtiles
### creazione
si tratta di mappe vettoriali in un contenitore sqlite
si scarica la versione gratuita da openmaptiles.com
per generarle usare
git clone https://github.com/openmaptiles/openmaptiles.git

per la lista delle countries:
sudo make download-geofabrik-list     
per generare i tiles                                                                                                                                  
sudo ./quickstart.sh italy   

## Working with mbtiles

Country boundaries extracted from OpenStreetMap in GeoJSON format
git clone https://github.com/tripviss/osm-countries-geojson.git

### splitting
https://github.com/mapbox/mbtiles-extracts.git
wget https://github.com/datasets/geo-countries/raw/master/data/countries.geojson
wget https://github.com/drei01/geojson-world-cities/raw/master/cities.geojson

./mbtiles-extracts ../data/mbtiles/other/2019-07-26-italy-spain-2017-07-03_europe.mbtiles ./countries.geojson ADMIN

### Editor
sudo docker run -p 8888:8888 maputnik/editor

### merging
https://jeromegagnonvoyer.wordpress.com/2015/08/06/merging-multiple-mbtiles-together/

git clone https://github.com/mapbox/mbutil.git

## Serving
docker run -d -p 3456:80 -v $(pwd):/data lsaggese/opentileserver


## Using it
''''
var map = L.map('map').setView([38.912753, -77.032194], 15);
L.marker([38.912753, -77.032194])
    .bindPopup("Hello <b>Leaflet GL</b>!<br>Whoa, it works!")
    .addTo(map)
    .openPopup();
var gl = L.mapboxGL({
    accessToken: 'no-token',
    style: 'http://as02.gotraxx.com:4445/styles/osm-bright/style.json'
}).addTo(map);
''''

# OSRM
##docker image
https://hub.docker.com/r/osrm/osrm-backend/

Download OpenStreetMap extracts for example from Geofabrik

wget http://download.geofabrik.de/europe/germany/berlin-latest.osm.pbf
Pre-process the extract with the car profile and start a routing engine HTTP server on port 5000
''''
docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-extract -p /opt/car.lua /data/berlin-latest.osm.pbf
''''
The flag -v "${PWD}:/data" creates the directory /data inside the docker container and makes the current working directory "${PWD}" available there. The file /data/berlin-latest.osm.pbf inside the container is referring to "${PWD}/berlin-latest.osm.pbf" on the host.
''''
docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-partition /data/berlin-latest.osrm
docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-customize /data/berlin-latest.osrm
''''
Note that berlin-latest.osrm has a different file extension.
''''
docker run -t -i -p 5000:5000 -v "${PWD}:/data" osrm/osrm-backend osrm-routed --algorithm mld /data/berlin-latest.osrm
''''
Make requests against the HTTP server
''''
curl "http://127.0.0.1:5000/route/v1/driving/13.388860,52.517037;13.385983,52.496891?steps=true"
''''
Optionally start a user-friendly frontend on port 9966, and open it up in your browser
''''
docker run -p 9966:9966 osrm/osrm-frontend
''''

# Geocoder
git clone https://github.com/luca-saggese/interpolation.git
docker run -d lsaggese/geocoder