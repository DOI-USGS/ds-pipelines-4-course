## Risky Targets

Depending on other packages within a pipeline is (1) essential to getting work done efficiently and (2) a potential source of involuntary pipeline changes: when a depended-on package changes, sometimes the pipeline targets built by that package also change in ways that cause a ripple of rebuilds throughout your pipline. Plan on this happening sometimes, but also know that there are ways to avoid some issues and manage others.

## Identifying risky targets

* Packages versions under 1.0.0 are a loud, clear signal that the package is likely to change substantially. Developers are explicitly not promising to keep storage formats, function arguments, or other package traits the same, or even backwards-compatible. This doesn't mean you shouldn't use such packages, but you should anticipate breaking changes.

* Does a change in a depended-on package's function cause a rebuild of targets whose command uses that function? I think it does not, but should check.

* Fast-developing packages that we often use are **sf** and members of the **tidyverse** collection. **sf** in particular has multiple dependencies of its own (GEOS, GDAL, PROJ) that can also change, and **sf** objects have earned notoriety on our team for changing their internal representations from one **sf** version to the next.

## Mitigating risks

* Occasionally you can opt not to use a package because you know it's too new. But more often the package is just too valuable not to use.

* We haven't done this much, but one option is to fix your package versions using a Docker or Selenium container or possibly a conda environment.

* The lowest-hanging fruit for risk mitigation is in making sure that your scipiper targets are minimal representations of the information you need. For example, you might be tempted to save an sf object with the lat-lon coordinates of your sites or the bounding box of your map view, but consider saving it as a data.frame, tibble, matrix, or .csv file instead. (Are tibbles likely to change format? hmm.) You might even be able to save more complex spatial info (e.g., polygons) as geotiff or geojson files, though here you might find that changes to GDAL, PROJ, etc. also change the contents of those files.
