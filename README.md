# gtfs2emis: Generating estimates of public transport emissions from GTFS data <img align="right" src="man/figures/logo.png" alt="logo" width="180">

[![R-CMD-check](https://github.com/rafapereirabr/gtfs2emis/workflows/R-CMD-check/badge.svg)](https://github.com/rafapereirabr/gtfs2emis/actions)
[![Lifecycle:
     experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html)
[![Codecov test
coverage](https://codecov.io/gh/rafapereirabr/gtfs2emis/branch/main/graph/badge.svg)](https://app.codecov.io/gh/rafapereirabr/gtfs2emis?branch=main)


**gtfs2emis** is an R package to estimate the emission levels of public
transport networks based on GTFS data. The package requires two main inputs: i) public transport data in the GTFS standard format; and ii) some basic information on fleet characteristics such as fleet age, technology, fuel and Euro stage. As it
stands, the the package estimates several pollutants (see table below) at high
spatial and temporal resolutions. Pollution levels can be calculated for
specific transport routes, trips, time of the day or for the transport
system as a whole. The output with emission estimates can be extracted in different
formats, supporting analysis on how emission levels vary across space,
time and by fleet characteristics.

<!-- The **gtfs2emis** package leverages on standard GTFS data format and develops a computational method that can be easily used to estimate enviromental emissions of several public transport systems around the world. One of the core advantages of **gtfs2emis** is that it makes extremely easy to simulate how the environmental performance of a public transport system would change under different policy scenarios. Simple modifications to the package input would allow one to estimate how emissions levels could be affected by different interventions such as electrifying the fleet, building new transport corridors, changing route itineraries or frequencies and fleet renewal. -->


## Installation

`gtfs2emis` will soon be on CRAN. In the meantime, you can install the dev version from Github:

```R
# or use the development version with latest features
  utils::remove.packages('gtfs2emis')
  devtools::install_github("ipeaGIT/gtfs2emis")
  library(gtfs2emis)

```

## Usage and Data requirements

The `gtfs2emis` package has two core functions.

1. `transport_model()` converts GTFS data into a GPS-like table with the space-time positions and speeds of public transport vehicles. The only input required is a `GTFS.zip` feed.

2. `emission_model()` estimates hot-exhaust emissions based on four inputs:
     * a) the result from the `transport_model()`;
     * b) a `data.frame` with info on fleet characteristics;
     * c) a `string` indicating which emission factor model should be considered;
     * d) a `string` indicating which pollutants should be estimated.

To help users analyze the output from `emission_model()`, the `gtfs2emis` package has few functions:

3. `emis_summary()` to aggregate emission estimates by time of the day, vehicle type or road segment.
4. `emis_grid()` to spatially aggregate emission estimates using any custom spatial grid or polygons.
5. `emis_to_dt()` to convert the output of `emission_model()` from `list` to `data.table`.



## Demonstration on sample data

See a detailed demonstration of `gtfs2emis` in this [intro Vignette](add_link). To illustrate functionality, the package includes small sample data sets of the public transport and fleet of Curitiba (Brazil), Detroit (USA) and Dublin (Ireland). Estimating the emissions of a given public transport system using `gtfs2emis` can be done three simple steps, as follows.

### 1. Run transport model

The first step is to use the `transport_model()` function to convert GTFS data into a GPS-like table, so that we can get the space-time position and speed of each vehicle of the public transport system at high spatial and temporal resolutions.

```{r, message = FALSE}
# read GTFS.zip
gtfs_file <- system.file("extdata/irl_dub/irl_dub_gtfs.zip", package = "gtfs2emis")
gtfs <- gtfstools::read_gtfs(gtfs_file)

# generate transport model
tp_model <- transport_model(gtfs_data = gtfs,
                            spatial_resolution = 100,
                            parallel = TRUE) 
```


### 2. Prepare fleet data

The second step is to prepare a `data.frame` with some characteristics of the public transport fleet. Note that different emission factor models may require information on different fleet characteristics, such as vehicle age, type, Euro standard, technology and fuel. This can be either: 
- A simple table with the overall composition of the fleet. In this case, the `gtfs2emis` will assume that fleet is homogeneously distributed across all routes; OR
- A detailed table that (1) brings info on the characteristics of each individual vehicle and, (2) tells the probability with which each vehicle type is allocated to each transport route.

Here is how a simple fleet table to be used with the Emep-EEA emission factor model looks like:

```{r, message = FALSE}
fleet_file <- system.file("extdata/irl_dub/irl_dub_fleet.txt", package = "gtfs2emis")

fleet_df <- read.csv(fleet_file)
head(fleet_df)

>           veh_type euro fuel   N fleet_composition    tech
> Ubus Std 15 - 18 t  III    D  10             0.009       -
> Ubus Std 15 - 18 t   IV    D 296             0.295     SCR
> Ubus Std 15 - 18 t    V    D 148             0.147     SCR
> Ubus Std 15 - 18 t   VI    D 548             0.546 DPF+SCR
```

### 3. Run emission model

In the final step, the `emission_model()` function to estimate hot exhaust emissions of our public transport system. Here, the user needs to pass the results from `transport_model()`, some fleet data as described above, and select which emission factor model and pollutants should be considered (see the options available below). The output from `emission_model()` is a `list` with several `vectors` and `data.frames` with emission estimates and related information such as vehicle variables (`fuel`, `age`, `tech`, `euro`, `fleet_composition`), travel variables (`slope`, `load`, `gps`) or pollution (`EF`, `emi`).

```R
emi_list <- emission_model(tp_model = tp_model
                          , ef_model = "ef_europe_emep"
                          , fleet_data = fleet_df
                          , pollutant = c("CO2","PM10")
                          )

names(emi_list)
```


## Emission factor models and pollutants available

Currently the `gtfs2emis` package provides a computational method to estimate running exhaust emissions factors based on the following emission factor models:

* Brazil
     - [CETESB](https://cetesb.sp.gov.br/veicular/relatorios-e-publicacoes/): 2017 model from the Environmental Company of São Paulo (CETESB)
* Europe
     - [EMEP/EEA](https://www.eea.europa.eu/themes/air/air-pollution-sources-1/emep-eea-air-pollutant-emission-inventory-guidebook/emep): European Monitoring and Evaluation Programme, developed by the European Environment Agency (EEA).
* United States
     - [EMFAC2017/CARB](https://arb.ca.gov/emfac/): California Emission Factor model, developed by the California Air Resources Board (CARB).
     - [MOVES3/EPA](https://www.epa.gov/moves): Vehicle Emission Simulator, developed by the Environmental Protection Agency (EPA).


#### List of pollutants available by emission factor model

| Source        | Pollutants                                                                                                                        |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------|
| CETESB        | CH4, CO2, PM10, N2O, KML, FC (Fuel Consumption), gD/KWH, gCO2/KWH, CO, HC, NMHC, NOx, NO2, NO, RCHO, ETOH, FS(Fuel Sales) and NH3 |
| EMFAC2017/CARB   | CH4, CO, CO2, N2O, NOx, PM10, PM2.5, SOX, TOG (Total Organic Gases), ROG (Reactive Organic Gases)                                 |
| EMEP/EEA      | FC, CO2, CO, NOx, VOC, PM10, EC, CH4, NH3, N2O, and FC                                                                            |
| MOVES3/EPA | CH4, CO, CO2, EC, HONO, N2O, NH3, NH4, NO, NO2, NO3, NOx, PM10, PM25, SO2, THC, TOG and VOC                                       |


#### Fleet characteristics required by each emission factor model

| Source        | Buses                        | Characteristics                              |
|---------------|------------------------------|----------------------------------------------|
| CETESB        | Micro, Standard, Articulated | Age, Fuel, EURO stantard                     |
| EMEP/EAA      | Micro, Standard, Articulated | Fuel, EURO stantard, technology, load, slope |
| EMFAC2017/CARB| Urban Buses                  | Age, Fuel                                    |
| MOVES3/EPA    | Urban Buses                  | Age, Fuel                                    |





## Related packages

There several others transport emissions models available for different purposes (see below). As of today, `gtfs2emis` is the only method with the capability to estimate emissions of public transport systems using GTFS data.

-   R: [vein](https://github.com/atmoschem/vein) Bottom-up and top-down
    inventory using GPS data.
-   R: [EmissV](https://github.com/atmoschem/emissv) Top-down inventory.
-   Python: [PythonEmissData](https://github.com/adelgadop/PythonEmissData)
    Jupyter notebook to estimate simple top-down emissions.
-   Python: [YETI](https://github.com/twollnik/YETI) YETI - Yet Another Emissions From Traffic Inventory
-   Python: [mobair](https://github.com/matteoboh/mobility_emissions) bottom-up model using GPS data.

## **Future enhancements**

-   Include cold-start, and evaporative emissions factors
-   Add railway emission factors

------------------------------------------------------------------------

### Credits <img align="right" src="man/figures/ipea_logo.png" alt="ipea" width="300">

The **gtfs2emis** package is developed by a team at the Institute for
Applied Economic Research (Ipea) with collaboration from the National
Institute for Space Research (INPE), both from Brazil. You can cite this
package as:

-   Bazzo, J.P.; Pereira, R.H.M.; Andrade, P.R.; (2020) gtfs2emis:
    Generating estimates of public transport emissions from GTFS data
