# GeoTIFF: Adding Geospatial Context

A standard TIFF file describes the image itself: its dimensions, pixel values, compression, and basic properties. However, a TIFF file does not inherently describe where the image is located on the Earth or how pixels correspond to real-world coordinates.

GeoTIFF is still a TIFF file — it just also includes metadata that links the pixel grid to a geographic coordinate system, allowing the image to be correctly positioned, analysed, and combined with other spatial datasets.

GeoTIFF is formally standardized by the Open Geospatial Consortium (OGC), which defines the tag structure and semantics for georeferenced TIFF imagery ([OGC GeoTIFF Standard](https://www.ogc.org/standards/geotiff/)). The OGC GeoTIFF 1.1 standard is a backward-compatible revision of GeoTIFF 1.0 and aligns the specification with current OGC practices and EPSG-based CRS definitions.

## GeoTIFF Metadata

GeoTIFF metadata is stored using additional TIFF tags within the same Image File Directory (IFD) structure used by standard TIFF metadata.

The main types of information added by GeoTIFF are:

- **Coordinate Reference System (CRS)**: Defines the coordinate system used to locate pixels on the Earth.
- **Georeferencing transformation**: How to convert pixel coordinates to real-world coordinates.
- **Datum and Ellipsoid**: Reference models for Earth’s shape (e.g., WGS84, NAD83).

Unlike standard TIFF metadata, GeoTIFF metadata provides the information required to allow software systems to understand the geospatial context of the data.

## Key GeoTIFF Tags

Here are some of the most important tags for geospatial metadata:

| **Tag ID** | **Tag Name** | **Requirement** | **Example Value** | **Purpose** |
|------------|--------------|-----------------|-------------------|-------------|
| 33550 | ModelPixelScaleTag | Required for georeferenced images | `10, 10, 0` | Defines the size of pixels in model space (for example, metres per pixel). |
| 33922 | ModelTiepointTag | Required for georeferenced images | `0,0,0,500000,4500000,0` | Defines the relationship between raster coordinates and geographic coordinates. |
| 34735 | GeoKeyDirectoryTag | Required for georeferenced images | CRS keys | Directory of geospatial keys (e.g., coordinate system, datum). |
| 34736 | GeoDoubleParamsTag | Optional | Floating point values | Double-precision parameters (e.g., ellipsoid axes). |
| 34737 | GeoAsciiParamsTag | Optional | `WGS 84` | Human-readable parameters (e.g., coordinate system name). |

## Coordinate Systems and Projections

GeoTIFF supports two main types of coordinate reference systems:

### Geographic Coordinate System (GCS)

A Geographic Coordinate System uses angular coordinates to describe locations on the Earth's surface.

Coordinates are represented as:

- **Latitude**: North–south position.
- **Longitude**: East–west position.

Example: `(10.5, 20.5)` represents `10.5°E longitude, 20.5°N latitude`.

A common example is:

- **WGS 84 (EPSG:4326)**: A global geographic coordinate system used by GPS and many web mapping applications.

Common GeoTIFF tags used for Geographic Coordinate Systems:

| **GeoTIFF Key** | **Purpose** |
|-----------------|-------------|
| GeodeticCRSGeoKey | Defines the geographic coordinate system (for example, EPSG:4326 for WGS 84). |
| GeoKeyDirectoryTag | Stores the structured GeoTIFF keys that describe the coordinate system. |

### Projected Coordinate System (PCS)

A Projected Coordinate System converts the curved surface of the Earth into a flat coordinate system using a mathematical projection.

Coordinates are represented as Cartesian values:

- **X coordinate**: Easting.
- **Y coordinate**: Northing.

Coordinates are usually expressed in linear units such as metres.

Example: `(500000, 4649776)` represents `500,000 m east, 4,649,776 m north` in `WGS 84 / UTM Zone 33N (EPSG:32633)`.

Common GeoTIFF tags used for Projected Coordinate Systems:

| **GeoTIFF Key** | **Purpose** |
|-----------------|-------------|
| ProjectedCRSGeoKey | Defines the projected coordinate system (for example, EPSG:32633). |
| ModelTiepointTag | Defines the relationship between pixel coordinates and projected coordinates. |
| ModelPixelScaleTag | Defines the size of pixels in projected units (for example, metres per pixel). |
| GeoKeyDirectoryTag | Stores the structured GeoTIFF keys describing the projection. |

## Example: Converting Pixel Coordinates to Real-World Coordinates

A GeoTIFF contains a raster grid. GeoTIFF metadata defines how pixel positions correspond to real-world coordinates.

Suppose a GeoTIFF has:

### ModelTiepointTag

`(0, 0, 0, 500000, 4649776, 0)` 

Meaning:

```
Pixel (0, 0) corresponds to:
X = 500000
Y = 4649776

in UTM Zone 33N (EPSG:32633).
```

### ModelPixelScaleTag

`(10, 10, 0)`
 
Meaning: 

```
Each pixel represents:
10 metres in X direction
10 metres in Y direction
```

To find the real-world coordinates of pixel:

```
Pixel (100, 200)
```

#### 1. Apply the pixel scale

```
ModelX = 100 × 10 = 1000
ModelY = 200 × 10 = 2000
```

#### 2. Apply the tiepoint offset

```
RealWorldX = 1000 + 500000 = 501000
RealWorldY = 2000 + 4649776 = 4651776
```

#### Result

Pixel `(100, 200)` corresponds to:

```
X = 501000 m
Y = 4651776 m

in UTM Zone 33N (EPSG:32633).
```

This transformation allows software to determine the geographic location of every pixel in the raster.

(limitations-of-geotiff)=
## Limitations of GeoTIFF

GeoTIFF is a widely adopted format for exchanging geospatial raster data. It provides strong interoperability for describing raster structure and spatial reference information, but it does not require or define a complete, standardised metadata model for describing the scientific meaning, provenance, and context of the data.

- **Scientific variables**: GeoTIFF stores pixel values but does not define their scientific meaning. Information such as variable names, units, uncertainty, and measurement methods are missing by default.

- **Scientific context**: GeoTIFF provides strong support for spatial metadata, but does not define comprehensive information about calibration, instrument details, experimental conditions, or other domain-specific context.

- **Provenance**: GeoTIFF does not provide a standard mechanism for recording data lineage, processing history, or quality control information.

## Summary

GeoTIFF is well suited for spatial raster data where the primary requirement is preserving pixel values and geographic location.

For complex scientific datasets requiring rich metadata, multiple dimensions, temporal information, or detailed provenance, additional metadata standards or alternative data formats may be needed.