_[Home](index.html) > Custom result display plugins_


# Sparnatural YASGUI plugins

Strictly speaking, Sparnatural scope is limited to outputing the SPARQL query generated by the component. That's all. It is not responsible for displaying the query results. This is typically in the scope of [YasGUI](https://triply.cc/docs/yasgui/), and so Sparnatural is often used in combination with this other component.

YasGUI offer a basic table rendering of query results, which is sufficient for simplistic integration. However, for better user rendering, some specific plugins of YasGUI are provided to be used together with Sparnatural. These specific plugins have *[their own Github repository](https://github.com/sparna-git/Sparnatural-yasgui-plugins)* from which you can download the release.

## "TableX" Plugin : hide URI behind labels

### What it does


This is an adaptation of the original YaSR Table plugin. Its main enhancement is that it can hide URI columns "behind" a correspondig label column, thus *hiding technical URIs for end-users*. The link URLs can also be customized to navigate to another URL than the original URI.

For every column `?xxx_label`, the plugin will look for a column `?xxx` having a literal property (for example `?Person_1` and `?Person_1_label`), and merge these columns. The merge is done on the SPARQL result set itself, by creating a new kind of result `x-labelled-uri` containing both the URI and its label. Only the `?xxx` column will be shown.

Instead of this:

![YasR without plugin](/assets/images/yasr-without-plugin.png)

You would get this:

![YasR with plugin](/assets/images/yasr-with-plugin.png)


### Configuration in Sparnatural

The default labels are configured with the `core:defaultLabelProperty` annotation of the OWL configuration (see the [OWL configuration reference](OWL-based-configuration.md)), or the equivalent `dash:propertyRole` annotation with the `dash:LabelRole` value of the SHACL configuration (see the [SHACL configuration reference](SHACL-based-configuration.md)). These annotations will cause a new column to be inserted in the result set, with a column name set to `xxx_label`, where `xxx` is the name of the original column having a default label property (for example `?Person_1` and `?Person_1_label`).


### Javascript integration

See the [corresponding section in the plugin integration page](https://docs.sparnatural.eu/YasGUI-plugins-integration.html#tablex-plugin).

## "Grid" Plugin : nice merged cards, with images

### What it does

The Grid plugin works in conjunction with Sparnatural, not for any SPARQL query. It needs to be notified of the Sparnatural original query, and the Sparnatural original configuration.

The Grid plugin:

- looks for a main title column for each entity
- merges the lines in the result set with the same title
- displays the other columns in each card by replicating the original query structure from Sparnatural
- can look for an image column (containing URIs ending in .png or .jpg)

Here is how it looks like with an image column selected:

![Grid plugin](https://github.com/sparna-git/Sparnatural-yasgui-plugins/raw/main/docs/images/grid-1.png)

The original query was the following: _(All chairmans of EU political groups, with dates of chair and image, based on the European Parliament Open Data Portal datasets)_

![Grid plugin query](https://github.com/sparna-git/Sparnatural-yasgui-plugins/raw/main/docs/images/grid-3.png)

The original result set was the following - note how each person is duplicated as many times as it was a chairman of a group, with different dates:

![Grid plugin original result table](https://github.com/sparna-git/Sparnatural-yasgui-plugins/raw/main/docs/images/grid-4.png)

Here is a focus on one card, note how the lines corresponding to the same entity have been merged, and how the original query structure is replicated inside the card:

![Grid plugin card](https://github.com/sparna-git/Sparnatural-yasgui-plugins/raw/main/docs/images/grid-2.png)

### Configuration in Sparnatural

_No specific configuration needed in Sparnatural_

### Integration

See the [corresponding section in the plugin integration page](https://docs.sparnatural.eu/YasGUI-plugins-integration.html#grid-plugin).

## Stats Plugin

### What it does

The Stats plugin can:
- display a simple COUNT query, with only an integer result :

![](https://github.com/sparna-git/Sparnatural-yasgui-plugins/raw/main/docs/images/stats-1.png)

- generate simple pie or bar charts from a `COUNT` + `GROUP BY` query

![](https://github.com/sparna-git/Sparnatural-yasgui-plugins/raw/main/docs/images/stats-2.png)


![](https://github.com/sparna-git/Sparnatural-yasgui-plugins/raw/main/docs/images/stats-3.png)


### Configuration

No specific configuration needed in Sparnatural, you simply need to issue a "count" query, typically with other columns as "group by" columns.

### Integration

See the [corresponding section in the plugin integration page](https://docs.sparnatural.eu/YasGUI-plugins-integration.html#stats-plugin).


## Map Plugin : Points and Polygons

### What it does

The map plugin:

- Reads columns with a datatype of `geo:wktLiteral`
- Or reads 2 columns from a property where the URI contains (ignoring case) `lat` and `long`
- Can display Points and Polygons. MultiPolygons are currently not supported
- Displays the original query area coming from the query, if present
- Displays information bubbles by reading all columns from the result set
- Splits polygons into different layers according to their size, so that the user can reduce the number of polygons displayed on the map for clarity
- Can warn users if some lines in the result set do not have their coordinates filled int

Here is an example:

![Map plugin](https://github.com/sparna-git/Sparnatural-yasgui-plugins/raw/main/docs/images/map-1.png)

### Configuration

No specific configuration needed in Sparnatural, you simply need to issue a query that either :
- returns a `geo:wktLiteral` in a column
- or returns 2 columns from 2 properties that contain respectively `lat` and `long` in their URI.

### Integration

See the [corresponding section in the plugin integration page](https://docs.sparnatural.eu/YasGUI-plugins-integration.html#map-plugin).