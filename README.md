# River Distance Markers

This repository contains the geofiles for the positions of known distance
markers that are found along rivers of the Murray-Darling Basin in Australia. 

## Description

Along the Murray River from the Hume Dam there are blue signs that were put up
by the NSW and SA water authorities to mark the distance from the river mouth 
in kilometres. These are based on mile marker references from older maritime 
pilots of the river.

These allow users to find their position on hard copy maps, accurately share
locations of points of interest or hazards along the river and as a way of
communicating their position to friends and family as well as various maritime
organisations in the event of an emergency.

Since these markers reflect the distance from the river mouth at the time these
were originally assigned, these do deviate from the actual mainstem of the
river as we know it today and cause some inconsistencies with distance
measurements. 

The highest blue marker found on the river is 2,224 that can be found just 
downstream from the Hume Dam / Weir. For most paddlers, the mainstem of the
river is just 2,142 km from this point with significant savings possible by
cutting corners, taking a direct path across Lake Mulwala, and going directly
to the river mouth via the Tauwitchere Barrage rather than going passing 
Goolwa.

Similar markers are found on the lower Darling River, starting at 0 at Wentworth
and extend up to around Pooncarie.  

There are smaller metal signs in many alpine regions, notably above 
Jingellic on the Murray River along with some on the lower Swampy Plains and
Tumut rivers. The Murray River markers above Jingellic are only three numbers
long and you need to add 2,000 to get the distance to the ocean. Being small
and often behind willow trees, these can be hard to spot. These are referred 
to as the Alpine Markers. Any calculated marker above the Hume Dam is considered 
to be an Aline marker.

## Coverage

Murray River
- Blue Markers: 606 GPS confirmed locations with another 198 cross-referenced
  locations, 2,230 in total
- Alpine Markers: 47 GPS confirmed locations, 353 total
                        
Darling River
- Blue Markers: 20 GPS confirmed locations

Swampy Plain River
- Alpine Markers: 4 GPS confirmed locations, 12 total

## Shapefile attributes

The data is stored in a MultiPoint ESRI Shapefile with the following attributes:

name: The number. This is not an unique field with a number of locations
      containing duplicate signs. Numbers below 10 are prefixed with a zero.

desc: A description containing additional information about the marker, such as
      its condition, location or if hidden behind other objects.

status:
  - g: Good condition or unclassified (default)
  - d: Damaged or faded, difficult to read,
  - n: No known records of a sign at this location.
       i.e. any calculated position with no known historical references.

type: Describes what type of sign it is.
  - bm: the normal blue marker.
  - b: marker is on a bouy. These were all replaced prior to 2022.
  - am: the small metal alpine markers.
  
    These may be extended to include:
  
  - wm: older white mile markers signs put up by the Libra Libra houseboat company.
  - mt: even older mile markers that were carved into tree trunks ("mile tree").
  - other custom markers.

river: Defines what river the marker is on. A single files may tracks a few locations
       on smaller tributaries.
       
src: The source reference for the marker. This is one of:
  - gps: Manually recorded GPS point of an existing sign.
  - calc: Calculated by creating a chainage between known sign locations.
  - ref: From one of the listed references, potentially moved slightly towards
         the calculated location if this is sightly unclear.

year: Year that the location or description was last verified. 

### Coordinate Reference System (CRS)

These are based on the Geocentric Datum of Australia (GDA2020) using the NSW
Lambert Projected CRS.

Name: EPSG:8058 - GDA2020 / NSW Lambert
Units: meters

I choose the NSW Lambert since the basin traverses two MGA longitude zones 54
and 55. This decision has not been vetted by anyone with formal training in
these matters, but this appeared to provide more accurate measurements than using
EPSG:1168 GDA2020.

## How to Use

You can use any GIS package (ArcGIS, QGIS, GRASS GIS, etc) to access and export 
the data according to your requirements. These may have to be reprojected into
a Coordinate Reference System that your app or device supports. 

For example, Avenza maps failed to project both EPSG:8058 (GDA2020 / NSW Lambert)
and EPSG:7854 (GDA2020 / MGA zone 54) and I had to reproject these maps to World
Mercator WGS 84 (EPSG:3395).

For the markers on the River Murray, you can obtain the latest version of here 
with multiple export formats in World Mercator WGS 84 (EPSG:3395):

* Keyhole Markup Language (KML)
* GPS Exchange Format (GPX)
* Well Known Text (WKT)
* JavaScript Object Notation (JSON)
* Comma-separated values (CSV)

https://etaunknown.com/expeditions/murray-river/planning/markers

These are filtered down to only include even markers below Hume Dam and all of markers
above but with only a few selected markers across the three lakes lakes.

The following QGIS filter is used to make these:

    if (river = 'murray', if ((name > 34 and name < 72) or (name > 1986 and name < 2006) or (name > 2224 and name < 2314), if (name % 4 = 0, 1, 0), if (name < 2226, if (name % 2 = 0, 1, 0), 1)), 1)

And the description field is generated by:

    concat( trim(desc), if(length(trim(desc)), '. ', ''), if (src = 'gps', concat('Last seen in ',  year, '.'), if (src = 'calc', 'Calculated position.', 'Reference material.')), if (river = 'murray', '', ' (Swampy Plain River)'))

### Mapping software to visualise the data points

There are a number of different tools and Apps that can be used

*Google My Maps*
This allows you to create and view KML files on a web browser while online.

*Google Earth*
This allows you to import and view KML files while online.

*Avenza Maps*
The premion version allows you to add custom layers over existing maps. 

## Authors

Alan Davison

## Version History

* 1.0
    Initial public release that is based on a single trip down the Murray
    with a number of trips downstream of Wentworth.
* 1.1
    Updated with details from an additional trip down the Murray in 2023 
    with calculated references generated from a more accurate river course.
* 1.2
    Full coverage along the river and repositioned many calculated positions
    to match external references that appear to be trustworthy.

## License

This project is licensed under the Creative Commons Attribution-NonCommercial
4.0 International License (CC BY-NC 4.0) - see the LICENSE.md file for details.

Attribution: Alan Davison with a link to this repository or https://etaunknown.com/expeditions/murray-river

## Acknowledgments

As well as manual GPS corrodinates from various trips, the following sources 
were used to estimate the locations of various missing markers or simply are
alternative references that show the positions of some of these markers.

* River of Islands - River Murray Charts, Yarrawonga To Hume Dam by Kath and Leon Bentley
* [Murray River Charts](https://rivermurraycharts.com.au/) - Renmark to Yarrawonga by Maureen Wright
* Murray River Pilot Charts - Goolwa to the SA/NSW border and Lower Murray, Lakes and Coorong by Baker and Reschke
* [VICMAP BOOKS](https://spatialvision.com.au/vicmap-books/) North East and North West Regions by Spatial Vision
* [Murray River Access Maps](https://spatialvision.com.au/murray-river-access-guides/) by Spatial Vision
* [Paddle SA](https://paddlesa.au/) Maps
