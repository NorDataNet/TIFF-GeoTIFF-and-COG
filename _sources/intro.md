# TIFF, GeoTIFF, and Cloud Optimized GeoTIFF

Welcome to this short tutorial on TIFF, GeoTIFF, and Cloud Optimized GeoTIFF (COG). These formats appear in many image and geospatial workflows, and they are best understood as building on one another rather than as three unrelated file types.

- **TIFF** is a flexible image format widely used in scanning, photography, publishing, printing, and digital archiving. Two important advantages relatively to other image formats (e.g. JPEG) are lossless storage and the ability to include multiple images or layers within a single file.
- **GeoTIFF** extends TIFF by adding geographic metadata, which allows an image to be placed correctly in real-world coordinates and used directly in GIS and other applications.
- **COG** extends GeoTIFF by organizing the file for efficient online access, so users and software can read only the parts of a large dataset that are needed.

```{figure} images/tiff-geotiff-cog-nested-circles.png
---
alt: Nested circles showing TIFF as the outer layer, GeoTIFF inside TIFF, and COG inside GeoTIFF.
width: 60%
align: center
---
TIFF, GeoTIFF, and COG are layered formats: COG is a GeoTIFF, and GeoTIFF is a TIFF.
```

## **Why This Tutorial?**
This guide is designed for **researchers, scientific data managers, and anyone working with geospatial or image-based data** who needs to:
- ✅ Understand the **structure, metadata, and use cases** of TIFF, GeoTIFF, and COG.
- ✅ Assess their **strengths and weaknesses** for scientific applications.
- ✅ Learn **when to use (or avoid) these formats** in your workflows.
- ✅ Explore how these formats **compare to other formats** in terms of flexibility, metadata standards and interoperability.


```{tableofcontents}
```
