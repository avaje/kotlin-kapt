# kotlin-kapt
Maven tiles for Kotlin kapt (annotation processors)

## Why

To simplify the configuration of the Kotlin compiler with multiple
annotation processors (KAPT).

## Example

In maven build / plugins add:

```xml
<plugin>
<groupId>io.repaint.maven</groupId>
<artifactId>tiles-maven-plugin</artifactId>
<version>2.16</version>
<extensions>true</extensions>
<configuration>
  <tiles>
    <!-- kotlin compile with kapt support -->
    <tile>io.avaje.kapt:compile:1.1</tile>

    <!-- multiple annotation processors -->
    <tile>io.avaje.kapt:dinject-generator:1.1</tile>
    <tile>io.avaje.kapt:javalin-generator:1.1</tile>
    <tile>io.avaje.kapt:querybean-generator:1.1</tile>

    <!-- other tiles -->
    <tile>org.avaje.tile:lib-classpath:1.1</tile>
    <tile>io.ebean.tile:enhancement:12.1.3</tile>
  </tiles>
</configuration>
</plugin>
```  

# The tiles

## compile tile
This tile that adds kotlin compiler and java compiler 

## kapt tiles - dinject-generator, javalin-generator, querybean-generator
These tiles specify the annotation processor to include
with the kotlin compiler


## How this works

This relies on the `io.repaint.maven` tiles plugin version `2.16` (or greater). 
The tiles plugin has the feature where we can have tiles that effectively merge 
their configuration (for our use case merging the annotation processors into the
kotlin compile plugin). 
 
A tile can be defined as a `tile-merge-target` (our io.avaje.kapt:compile tile). 
This means that tile defines a `tiles-append` attribute specifying where other 
tiles can merge content into. In this case we are merging into `annotationProcessorPaths`.

Tiles can be defined as `tile-merge-source` meaning that they have their source merged 
into a target tile. For dinject-generator, javalin-generator and querybean-generator
these tiles all supply content in `annotationProcessorPaths` that is merged into the
`io.avaje.kapt:compile` tile.
