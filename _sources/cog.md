# Cloud Optimized GeoTIFF (COG): The Cloud-Ready Evolution

## What is COG?

Cloud Optimized GeoTIFF (COG) is a profile of GeoTIFF designed for efficient access over the web (e.g., from cloud storage like AWS S3 or HTTP servers). It is not a new format, but a constrained way of organising a GeoTIFF so clients can efficiently read only the parts they need. COG is defined as an OGC standard ([OGC Cloud Optimized GeoTIFF Standard](https://www.ogc.org/standards/ogc-cloud-optimized-geotiff/)). In practice, this allows:

- Partial access: Fetch only the tiles or overviews you need (e.g., a single 256x256 tile from a 10,000x10,000 image).
- Streaming: Serve data on-demand without downloading the entire file.
- Scalability: Work with large datasets (e.g., global satellite imagery) without local storage.

## Physical Differences in COG

COG reorganises the GeoTIFF file structure to optimise for HTTP range requests. A GeoTIFF can validly use several different internal layouts, but a COG narrows that flexibility to a predictable arrangement that supports efficient partial reads. Here are the main differences.

 | **Feature**               | **General GeoTIFF**                           | **Cloud Optimized GeoTIFF**                     | **Why It Matters**                                                                 |
 |---------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------------------------------------------------------------|
 | **Tiling**                | Optional (strips or tiles)                   | Mandatory (e.g., 256x256 or 512x512 tiles)       | Enables partial tile requests (e.g., fetch only Tile 5).                           |
 | **Overviews**             | Optional, can be striped, stored anywhere      | Mandatory, tiled, stored before main data      | Allows fast low-resolution previews without downloading the full image.           |
 | **Metadata Duplication**  | Usually only in main IFD                              | Duplicated in every IFD (main + overviews)     | Any tile/overview can be interpreted standalone (no need to read the full file). |
 | **IFD Order**             | No requirement                                | Overviews first, then main image              | Prioritizes fast access to overviews (e.g., for zoomed-out views).                 |

### Example File Structure: COG

There is no single required physical layout for a GeoTIFF. A valid GeoTIFF may have IFDs in different orders, may omit overviews entirely, and may store image data as strips or tiles. The point of COG is to provide a GeoTIFF profile that is optimised for efficient access over the web.

```text
[Header]
[IFD 0: Main Image] → Points to tiled data at byte offset 1,000,000
[IFD 1: Overview 1] → Points to tiled data at byte offset 100,000
[IFD 2: Overview 2] → Points to tiled data at byte offset 50,000
[IFD 3: Overview 3] → Points to tiled data at byte offset 25,000
[Overview 3 Tiles] (Aligned to 16-byte boundaries)
[Overview 2 Tiles]
[Overview 1 Tiles]
[Main Image Tiles] (Aligned to 16-byte boundaries)
```

## Limitations

While COG offers significant advantages for cloud-based workflows, it may not be necessary for all use cases. Some applications or workflows without cloud integration might not benefit from the specific optimisations provided by COG. Additionally, all the metadata and interoperability limitations {ref}`mentioned in the GeoTIFF section <limitations-of-geotiff>` still apply and have not been addressed in the COG format.

## Summary

Cloud Optimized GeoTIFF represents a powerful evolution of the GeoTIFF format, tailored for the demands of modern, cloud-based geospatial workflows. Its design ensures efficient, scalable, and interoperable access to geospatial data, making it a valuable tool for a wide range of applications.

For complex scientific datasets requiring rich metadata, multiple dimensions, temporal information, or detailed provenance, additional metadata standards or alternative data formats may be needed.