# Custom Scenarion and Applications
- [Scenario Convert](https://www.eclipse.org/mosaic/docs/scenarios/create_a_new_scenario/)
## Creating a complete scenario to Eclipse Mosaic from OSM file
```bash
java -jar scenario-convert.jar --osm2mosaic -i steglitz.osm   # other options: --osm2db and --db2mosaic
````
or
without cleaning the osm-file
```bash
java -jar scenario-convert.jar --osm2mosaic -i steglitz.osm --skip-osm-filter
````
or
with route generation
```bash
java -jar scenario-convert.jar --osm2mosaic -i steglitz.osm --generate-routes
--route-begin-latlon 52.4551693,13.3193474 --route-end-latlon 52.4643101,13.3206834 --number-of-routes 3

# This will calculate three routes between the two given coordinates.
```

What happens:
- SQLite-database is created
- Folders and files are generated as below:
```
└── <working-directory>
    └── steglitz
        ├── steglitz.osm
        ├── application
        │   └── steglitz.db
        ├── cell
        │   ├── cell_config.json
        │   ├── network.json
        │   └── regions.json
        ├── environment
        │   └── environment_config.json
        ├── mapping
        │   └── mapping_config.json
        ├── ns3
        │   ├── ns3_config.json
        │   └── ns3_federate_config.xml
        ├── omnetpp
        │   ├── omnetpp_config.json
        │   └── omnetpp.ini 
        ├── output
        │   └── output_config.xml
        ├── sns
        │   └── sns_config.json
        ├── sumo
        │   ├── steglitz.net.xml
        │   └── steglitz.sumocfg
        └── scenario_config.json .................. General configuration of the simulation scenario
```
-  
 ## Import Routes
 
 
 ## Summary

```bash
usage: scenario-convert [OPERATION] [OPTIONS]
Examples:
1. Import an osm file and write data into database
   > scenario-convert --osm2db -i <OSMFILE> [-d <DATABASE>]
2. Export database content to SUMO-readable files
   > scenario-convert --db2sumo -d <DATABASE> [-s <SUMOPREFIX>]
3. Import a SUMO routefile into a database
   > scenario-convert --sumo2db -d <DATABASE> -i <ROUTEFILE>.rou.xml
4. Combine steps 1 and 2
   > scenario-convert --osm2sumo -d <DATABASE> -i <OSMFILE> [-s <SUMOPREFIX>]
5. Export db content to Shapefile format
   > scenario-convert --db2shp -d <DATABASE>
6. Import an srtm file and write elevation data to nodes of an existing database
   > scenario-convert --srtm2db -i <SRTMFILE> -d <DATABASE>
7. Create a complete MOSAIC scenario from an osm file
   > scenario-convert --osm2mosaic -i <OSMFILE>
8. Create a scenario database from an SUMO net file
   > scenario-convert --sumo2db -i <NETFILE>.net.xml
9. Calculate a route from node 123 to node 456
   > scenario-convert --generate-routes -d <DATABASE> --route-begin-node-id 123 --route-end-node-id 456

The following arguments are available:
 -h,--help                                 Prints this help screen.

 -c,--config-file <PATH>                   Optional, refers to a configuration file which contains all parameters in
                                           JSON format.
    --osm2db                               Converts a OpenStreetMap file to a new scenario database.
    --sumo2db                              Imports a SUMO Network/Routefile. Be aware that you may have to re-export an
                                           imported network.
    --srtm2db                              Imports an SRTM file and writes elevation data to nodes.
    --db2sumo                              Exports the network to SUMO node and edge files.
    --db2shp                               Exports the database into Shapefile format.
    --db2mosaic                            Creates directory structure for a MOSAIC scenario based on the database
                                           contents.
    --osm2sumo                             Combination of osm2db and db2sumo.
    --osm2mosaic                           Combination of osm2db and db2mosaic.
    --update                               Updates the database to the current scheme.

 -d,--database <PATH>                      The path to the database.
                                           For import: File to import to. If not given, a new file is created.
                                           For export: File to load from.
                                           For update operations: file to update.
 -i,--input <PATH>                         Defines an input file to use in an import. File type depends on the import
                                           operation (OSM File/Database file/SUMO net file/SUMO rou file).
 -s,--sumo-prefix <STRING>                 Prefix for the generated sumo files  (uses database name when not defined).
 -f,--force                                Force overwrite of existing files instead of incrementing file names.
 -m,--mapping-file <PATH>                  Uses the given mapping configuration to export existing vehicleTypes and
                                           vehicles data to *.rou.xml.
    --import-lat <DOUBLE>                  Center latitude of imported region to project coordinates.
    --import-lon <DOUBLE>                  Center longitude of imported region to project coordinates.
    --import-zone <STRING>                 UTM zone of location for projecting coords in default format (e.g. 32n).

 -g,--generate-routes                      Generates route(s) from one point to another. The begin and end for the route
                                           must be known (see below options).
    --route-begin-lat <DOUBLE>             Latitude of the starting point, needs --route-begin-lon as well.
    --route-begin-lon <DOUBLE>             Longitude of the starting point, needs --route-begin-lat as well.
    --route-begin-latlon <DOUBLE,DOUBLE>   [latitude],[longitude] of the starting point.
    --route-begin-node-id <STRING>         OSM node id of the starting point (use instead of lat/lon).
    --route-end-lat <DOUBLE>               Latitude of the starting point, needs --route-end-lon as well.
    --route-end-lon <DOUBLE>               Longitude of the starting point, needs --route-end-lat as well.
    --route-end-latlon <DOUBLE,DOUBLE>     [latitude],[longitude] of the ending point.
    --route-end-node-id <STRING>           OSM node id of the ending point (use instead of lat/lon).
    --route-matrix <STRING,STRING,...>     Calculates all possible routes starting and ending at the given nodes, given
                                           as comma-separated list (e.g. 12345,87123,89123)
    --number-of-routes <INTEGER>           Defines the number of alternative routes to be calculated (default=1).

    --skip-osm-filter                      Skips automatic filtering of the OSM file (only with osm2xxx).
    --skip-turn-restrictions               Ignore all defined turn restrictions on OSM import.
    --skip-graph-cleanup                   Turns off the removal of unconnected parts from the main traffic network
                                           graph . Since several components of MOSAIC require one main graph without
                                           disconnected ways and nodes, this option should be used only if the cleanup
                                           procedure is faulty.
    --skip-netconvert                      Skips starting sumo netconvert for creating netfile (only with xxx2sumo).
    --skip-traffic-lights-export           Skips exporting traffic light information for nodes (only with xxx2sumo).
    --osm-speeds-file <PATH>               Define a property file which contains speed information which are used to set
                                           the speed for OSM ways without a max speed tag (only with osm2xxx).
    --osm-speeds-overwrite                 If set to true , the maxspeed tags of ways are ignored and replaced by either
                                           default values , or by speed information defined via the --osm-speeds-file
                                           option (only with osm2xxx).
  ```
