[![Build Status](https://travis-ci.org/osmlab/name-suggestion-index.svg?branch=master)](https://travis-ci.org/osmlab/name-suggestion-index)
[![npm version](https://badge.fury.io/js/name-suggestion-index.svg)](https://badge.fury.io/js/name-suggestion-index)

## name-suggestion-index

Canonical common brand names for OpenStreetMap


### What is it?

The goal of this project is to maintain a [canonical](https://en.wikipedia.org/wiki/Canonicalization)
list of commonly used names for suggesting consistent spelling and tagging of features
in OpenStreetMap.


### How it's used

When mappers create features in OpenStreetMap, they are not always consistent about how they
name and tag things. For example, we may prefer `McDonald's` tagged as `amenity=fast_food`
but we see many examples of other spellings (`Mc Donald's`, `McDonalds`, `McDonald’s`) and
taggings (`amenity=restaurant`).

Building a canonical name index allows two very useful things:
- We can suggest the most "correct" way to tag things as users create them while editing.
- We can scan the OSM data for "incorrect" features and produce lists for review and cleanup.

![name-suggestion-index in use in iD](http://i.imgur.com/9p1E6S4.gif)

*The name-suggestion-index is in use in iD when adding a new item*

Currently used in:
* iD (see above)
* [Vespucci](http://vespucci.io/tutorials/name_suggestions/)
* JOSM presets available


### Participate!

* Read the project [Code of Conduct](CODE_OF_CONDUCT.md) and remember to be nice to one another.
* See [CONTRIBUTING.md](CONTRIBUTING.md) for info about how to contribute to this index.

We're always looking for help!  If you have any questions or want to reach out to a maintainer, ping `bhousel` on:
* [OpenStreetMap US Slack](https://slack.openstreetmap.us/)
(`#poi` or `#general` channels)


### Prerequisites

* [Node.js](https://nodejs.org/) version 6 or newer
* [`git`](https://www.atlassian.com/git/tutorials/install-git/) for your platform


### Installing

* Clone this project, for example:
  `git clone git@github.com:osmlab/name-suggestion-index.git`
* `cd` into the project folder,
* Run `npm install` to install libraries


### About the index

#### Generated files (do not edit):

Preset files (used by OSM editors):
* `dist/name-suggestions.json` - Name suggestion presets
* `dist/name-suggestions.min.json` - Name suggestion presets, minified
* `dist/name-suggestions.presets.xml` - Name suggestion presets, as JOSM-style preset XML

Name lists:
* `dist/allNames.json` - all the frequent names and tags collected from OpenStreetMap
* `dist/discardNames.json` - discarded subset of allNames
* `dist/keepNames.json` - kept subset of allNames

#### Configuration files (edit these):

* `config/*`
  * `config/filters.json`- Regular expressions used to filter `allNames` into `keepNames` / `discardNames`
* `brands/*` - Config files for each kind of branded business, organized by OpenStreetMap tag
  * `brands/amenity/*.json`
  * `brands/leisure/*.json`
  * `brands/shop/*.json`
  * `brands/tourism/*.json`

:point_right: See [CONTRIBUTING.md](CONTRIBUTING.md) for info about how to contribute to this index.


### Building the index

* `npm run build`
  * Regenerates `dist/keepNames.json` and `dist/discardNames.json`
  * Any new `keepNames` not already present in the index will be added to it
  * Outputs warnings to suggest updates to `brands/**/*.json`


### Updating `dist/allNames.json` from planet

This takes a long time and a lot of disk space. It can be done occasionally by project maintainers.
You do not need to do these steps in order to contribute to the index.

- Install `osmium` commandline tool
  - `apt-get install osmium-tool` or `brew install osmium-tool` or similar
- [Download the planet](http://planet.osm.org/pbf/)
  - `curl -o planet-latest.osm.pbf https://planet.openstreetmap.org/pbf/planet-latest.osm.pbf`
- Prefilter the planet file to only include named items with keys we are looking for:
  - `osmium tags-filter planet-latest.osm.pbf -R name -o named.osm.pbf`
  - `osmium tags-filter named.osm.pbf -R amenity,shop,leisure,tourism -o wanted.osm.pbf`
- Run `node build_allNames wanted.osm.pbf`
  - results will go in `dist/allNames.json`
  - `git add dist/allNames.json && git commit -m 'Updated dist/allNames.json'`


### License

name-suggestion-index is available under the [3-Clause BSD License](https://opensource.org/licenses/BSD-3-Clause).
See the [LICENSE.md](LICENSE.md) file for more details.
