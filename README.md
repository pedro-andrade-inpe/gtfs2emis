
# gtfs2emis

### Generating estimates of public transport emissions from GTFS data

**gtfs2emis** is an R package to estimate the emission levels of public
transport networks based on GTFS data. The package combines two
**inputs**: geolocated time tables of public transport services
organized in GTFS format, and some basic information on fleet
characteristics such as fleet age, ….. . As it stands, the **gtfs2emis**
package generates estimates for several environmental pollutants (see
table below) at high spatial and temporal resolutions. Pollution levels
can be calculated for specific transport routes, trips, time of the day
or for the transport system as a whole. These **outputs** can be
extracted in different formats, supporting analysis on how emission
levels vary across space, time and by fleet characteristics.

The **gtfs2emis** package leverages on standard GTFS data format and
develops a computational method that can be easily used to estimate
enviromental emissions of several public transport systems around the
world. One of the core advantages of **gtfs2emis** is that it makes
extremely easy to simulate how the environmental performance of a public
transport system would change under different policy scenarios. Simple
modifications to the package input would allow one to estimate how
emissions levels could be affected by different interventions such as
electrifying the fleet, building new transport corridors, changing route
itineraries or frequencies and fleet renewal. files.

### Installation

``` r
devtools::install_github("rafapereirabr/gtfs2emis",auth_token = "94c88a642a2f62fce15e4c05cc0b6ead55ff5851")
```

### Basic Usage

More examples in the [intro
Vignette](https://github.com/rafapereirabr/gtfs2emis/blob/master/doc/gtfs2emis-vignette.md)

#### Available transport emission factors:

| a | b | c | Source |
| - | - | - | ------ |
| … | … | … | Vein   |

### **Related projects**

It would be nice to mention {vein} and other R packages related to
transport emissions.

  - [vein](https://github.com/atmoschem/vein) Bottom-up and top-down
    inventory.
  - [EmissV](https://github.com/atmoschem/emissv) Top-down inventory.
  - [Yeti](https://github.com/twollnik/YETI) (It seems abandon).
  - [PythonEmissData](https://github.com/adelgadop/PythonEmissData)
    Jupyter notebook to estimate simple top-down emissions.

-----

### Credits <img align="right" src="man/figures/ipea_logo.png" alt="ipea" width="300">

The **gtfs2emis** package is developed by a team at the Institute for
Applied Economic Research (Ipea) with collaboration from the National
Institute for Space Research (INPE), both from Brazil. You can cite this
package as:

  - Pereira, R.H.M.; Bazzo, J.; Andrade, P.R.; (2020) gtfs2emis:
    Generating Estimates Of Public Transport Emissions From GTFS Data
