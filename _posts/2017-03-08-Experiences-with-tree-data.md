

<img src="/assets/images/BCC-41328.jpg" width="200" title="Black Pine"/>

The [BristolTrees](http://bristoltrees.space) site currently maps around 66,000 trees in Bristol and surrounds obtained from multiple sources.  Each source has provided data in its own format on the trees in their care so it has been necessary to convert this heterogeneous data to a unified data set.  The following notes on this project are a contribution to the discussion of standardisation.

Trees are complex dynamic objects and the records kept reflect the different purposes for which data is collected and stored.  Data from an Arboretum will be different to that from the part of the council responsible for planting and general maintenance and different again from the data needed for Tree Protection orders (TPO)  Not only does purpose affect what data is held, it determines the precision of the data and it determines accuracy since the cost of errors will vary by purpose.

## Core data
The core data used in Bristol Trees are:

* Identifier:  constructed {sourcecode}-{sourceid}
* Species - Primary: Botanical (latin) name; derived Common name
* Location - Primary: Lat/long; derived: OS Easting/Northing; Secondary: Site, Address
* Dimensions: survey date;  girth (trunk circumference) in cm; height in m ; canopy width in m
* Age : age at survey date ; date planted ; maturity
* State:  State category; tree condition
* Description: information about this specific tree; history etc
```
<trees>
  <id>Bcc-23456</id>
</trees>
```
### Tree identity
To provide a unique internal identifier which retains the identity in the source, Bristol Trees constructs a binomial code, using a code for the source and the source code.  It is important for the source identifier be recoverable so that matching to source data and export of updated data is possible.  This depends on the existence of a unique code or combination of codes in the source data and this is not always the case. Since identifiers are used in URLs, some reformatting may be needed, for example to replace / by underscore. (or uri-encoding)

### Species
The species of tree is specified by the Botanical name. Since Bristol Trees aims to be a ‘Virtual Arboretum’, we strife for precision at least to the level of genus and species. Precision in the identification is more important for botanical reasons than it is for TPOs where genus may be sufficient. For a number of species, there are multiple botanical names in use and since such synonyms are common, common synonyms are acceptable; for example whilst the preferred botanical name for the London Plane is Platanus x acerifolia, the synonym Platanus x hispanica  is still widely used.

There is a recommend style of the Botanical name, for example as described by this document from the RHS.  Names which don’t conform to this style, for example by omission of quotes around cultivar names, or incorrect notations finer than species (var., subsp., f., cultivar) have to be standardised.  

Although tree species are often known by their common name, the quality of these names in the data is very variable in precision and accuracy.  In Bristol Trees the common name is derived from the Botanical name via a species dataset, now containing over 1000 species and variants, built as data was added.  Derivation is preferable since to be usable, common names need standardisation in name and format (is it Common beech or Common Beech?) and are also culturally dependant - Sycamore in the US is Platanus occidentalis, in the UK it is Acer pseudoplatanus. 

The coverage of The Collins Tree Guide by Owen Johnson and David More is excellent although there are a few missing species   A machine-readable source of approved names would be very useful as part of  a standardisation project and although there are several searchable on-line sources for specific purposes, I haven’t found a suitable open data set. Wikipedia is very useful for automating checking to species level.

### Location
Location is provided in several forms: OS easting/northing coordinates, OS Grid Reference and latitude/longitude (implicity in WGS84).  These are functionally equivalent and in Bristol Trees, for ease of mapping and lookup from a smartphone, lat/long is the primary data.  There are however some benefits in using OS coordinates. The resolution of 6 digit coordinates,at 1 metre, is generally appropriate for the location of trees, whilst  lat/longs are often given with excess precision, sometimes down to a micron, presumably as a result of conversion from OS coordinates which did not preserve the original precision.  Another benefit is that OS coordinates can be used to define tiles of a size useful for selecting trees by proximity. Preferable both systems can be held but any one is satisfactory in source data.

Additionally, most data sources supply less precise locations, in the form of addresses, street names, park names, wards etc.  Useful as these may be as supporting data, such names need supporting definition to be really valuable. Given that the boundaries of such units are subject to change (ward names may reflect old boundaries) it would be better to derive such names in demand, especially since such services would be valuable in other contexts. Nevertheless, local area names, especially park and reserves names are useful for searching.

### Tree dimensions
Tree dimensions vary between sources, again depending on purpose.  The Tree Register provides a good summary of tree measurement.

The most common dimension is girth, partly because it is easy to measure and partly because it is a good surrogate for age. Two measures are used in practice: foresters use the measure Diameter at Breast Height (DBH) whereas popularly, the circumference (girth) is given.  Girth can be read directly from a tape measure whereas DBH requires conversion, or as I recently discovered, the use of a tape calibrated so that the diameter can be read directly. I’ve opted to record the girth in cm.

Height of tree is often provided but this is difficult to measure with accuracy and precision may be limited to height bands, which vary between sources.  For street trees, the size of the canopy is important and one source provides coverage data in each quadrant.  Banding is usually used here too.

For any measurement, the survey date is necessary yet is missing in many sources. This greatly reduces the value of the data.

### Tree age
For historical records of trees, particularly ancient ones, the date of planting / age is important, as it is for local historians.  Sometimes the date of planting is know with some accuracy, such as oaks planted for the coronation of Edward VII in 1902.  For others, the planting date can be deduced from historical records. In rare cases it is measured by ring count.  Clearly it is preferable to store the date and derive the age, but care is needed to avoid spurious precision. 

### Tree state
Whilst species name, location and dimensions are relatively easy to standardise data on the state of the tree varies greatly with purpose. For a Stump survey aimed at tree replacement, gross categories of live tree, dead tree, stump, tree pit, no tree are needed.  The state of ‘sculpture’ was added for trees carved to form a sculpture.  For growing trees, stages in maturity are needed.  For ancient tree survey, forms of the tree - maiden, pollarded, fallen etc are recorded.  Disease and other damage observations are needed for tree maintenance. Further analysis and standardisation is needed in Bristol Trees.

### Tree description
Notes are often provided by naturalists on the special features of a specific tree, as opposed to notes about the species. For example a tree may be a species champion in its county, or display particular uncommon features for its species, or be growing in a unique manner or location.

## Complications
Real world data is imperfect and cleaning is nearly always going to be needed, so validation and standardisation scripts and manual intervention are generally needed.

Missing data is represented in source-specific ways often involving marker values. In the XML, missing data is handled by omitting the missing element. 

Partially missing data for species is ambiguous. I have taken Quercus and Quercus sp. to be synonymous but sometimes Quercus alone means the (locally) most common species of that Genus (in Bristol, Quercus robur), in which case Quercus sp. may mean a species of Quercus which is not the default.  Full species naming is of course desirable but notation for imprecise identification is needed in practice.

Round-tripping Having made corrections and other re-codings, export of the revised data in the original format (round-tripping) becomes problematic. This capability would be needed if citizen-surveyed data is to be contributed back to the source organisation. (Whether the source organisation can input from its export format is of course another matter)  An early decision on whether round-tripping is to be supported is necessary. For example it would be sensible to write scripts to load from the source and its inverse to export to the source at the same time. At present round-tripping is not yet fully supported in Bristol Trees.

Duplication.  Taking data from multiple sources raises the problem of multiple representations of the same tree. De-duplication has yet to be handled automatically, since it is possible that species as well as location will be different for the same tree.

Accuracy
Effort has so far focused on data acquisition, cleaning and normalisation.  We have yet to face the much bigger problem of assessing the accuracy of the data supplied, not least because it is often acquired some time ago, but is undated. 

Re-surveying 66,000 trees seems a daunting task, even if supported by citizen data collection.  However sampling at least is needed to gain a sense of the accuracy of the data. The development of the smartphone tree finding and surveying tools in BristolTrees will be valuable for data collection.  For standardisation purposes it would be helpful if data set metadata provided information on the precision and accuracy of data items.

### Standardisation  
Increased standardisation of tree data would certainly ease the problems of dataset use and integration. However the functional nature of the data, the lack of funds and capability in data management in the source organisations may be limiting factors in the implementation of any standard at source.  It is perhaps more realistic for organisations gathering data to focus on the accuracy and currency of data whilst the open data community could establish collections of data reformatted to a standard, and curate a master species list.  
