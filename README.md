
<!-- README.md is generated from README.Rmd. Please edit that file -->

\`\# gps2emis

Respository of the R package to estimate public transport emissions
based on GTFS files.

`prep/01_*.R` - prep scripts

1 - download gps cur

2 - prep fleet cur

3 - check valid shapeids (perhaps it could be add into gtfs2gps package)

4 - create hexagons (not working so far I dont know why)

5 - read\_gps (from points data to linestring)

`prep/02_*.R` - emission

1 - emission estimation

`prep/03_*.R` - main

1 - main script

`prep/08_*.R` - plot and visualization

used only for check data purposes

`prep/09_*.R` - auxiliar script / internal functions

1 - emission factor from EMEEA
