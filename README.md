# gtfs2emis

### Generating estimates of public transport emissions from GTFS data

**gtfs2emis** is an R package to estimate the emission levels of public transport networks based on GTFS data. The package combines two **inputs**: geolocated time tables of public transport services organized in GTFS format, and some basic information on fleet characteristics such as fleet age, ..... . As it stands, the **gtfs2emis** package generates estimates for several environmental pollutants (see table below) at high spatial and temporal resolutions. Pollution levels can be calculated for specific transport routes, trips, time of the day or for the transport system as a whole. These **outputs** can be extracted in different formats, supporting analysis on how emission levels vary across space, time and by fleet characteristics.

The **gtfs2emis** package leverages on standard GTFS data format and develops a computational method that can be easily used to estimate enviromental emissions of several public transport systems around the world. One of the core advantages of **gtfs2emis** is that it makes extremely easy to simulate how the environmental performance of a public transport system would change under different policy scenarios. Simple modifications to the package input would allow one to estimate how emissions levels could be affected by different interventions such as electrifying the fleet, building new transport corridors, changing route itineraries or frequencies and fleet renewal.
files.






## Installation

``` r
devtools::install_github("rafapereirabr/gtfs2emis",auth_token = "94c88a642a2f62fce15e4c05cc0b6ead55ff5851")
```

    ## 
    ##      checking for file ‘/tmp/Rtmpst04MI/remotes743366bdef0b/rafapereirabr-gtfs2emis-1581f85a352f01ef8908408440914fdde0d70dd4/DESCRIPTION’ ...  ✓  checking for file ‘/tmp/Rtmpst04MI/remotes743366bdef0b/rafapereirabr-gtfs2emis-1581f85a352f01ef8908408440914fdde0d70dd4/DESCRIPTION’
    ##   ─  preparing ‘gps2emis’:
    ##      checking DESCRIPTION meta-information ...  ✓  checking DESCRIPTION meta-information
    ##   ─  checking for LF line-endings in source and make files and shell scripts
    ##   ─  checking for empty or unneeded directories
    ## ─  looking to see if a ‘data/datalist’ file should be added
    ##   ─  building ‘gps2emis_0.0.0.9000.tar.gz’
    ##      
    ## 

## Basic Usage

For now, the function are a little bit adapted to Curitiba’s GTFS and
fleet characteristics. Also, it still possible to simulate emissions
only for CO and
NOx.

``` r
# ef_hdv_speed(vel = units::set_units(10,km/h),veh = "Urban Buses Midi <=15 t",
#             fuel = "Diesel",euro = "Euro III",tech = NA,pol = "CO",show.equation = TRUE)
#
# gtfs2gps(raw_gtfs = "inst/extdata/gtfs_cur.zip",filepath = "test_joao/gps/",filter_weekdays = TRUE)
#
# gps_to_linestring(input_filepath = "test_joao/gps/",output_filepath = "test_joao/lines/",
#                  fleet_data = "inst/extdata/cur_fleet.tar.xz",overwrite = FALSE)
```






``` r
# quick example
```
More examples in the [intro Vignette](https://cran.r-project.org)



# Available transport emission factors:


| a   |  b  |  c  | Source |
|-----|-----|-----|-----|
| ... | ... | ... | Vein   |


obs. The current version of **gtfs2emis** does not support the analysis of rail-based transport modes yet.


### Preparation scripts

##### 01\) `prep/01_*.R` - prep scripts

1 - download gps cur

2 - prep fleet cur

3 - check valid shapeids (perhaps it could be add into gtfs2gps package)

4 - create hexagons (not working so far I dont know why)

5 - read\_gps (from points data to linestring)

##### 02\) `prep/02_*.R` - emission

1 - emission estimation

##### 03\) `prep/03_*.R` - main

1 - main script

##### 04\) `prep/08_*.R` - plot and visualization

used only for check data purposes

##### 05\) `prep/09_*.R` - auxiliar script / internal functions

1 - emission factor from EMEEA

### Project

The main steps of the development should be presented in `projects`
sheet





#### **Related projects**

It would be nice to mention {vein} and other R packages related to transport emissions.


-----

### Credits <img align="right" src="man/figures/ipea_logo.png" alt="ipea" width="300">

The **gtfs2emis** package is developed by a team at the Institute for Applied Economic Research (Ipea) with collaboration from the National Institute for Space Research (INPE), both from Brazil. You can cite this package as:

* Pereira, R.H.M.; Bazzo, J.; Andrade, P.R.; (2020) gtfs2emis: Generating Estimates Of Public Transport Emissions From GTFS Data
