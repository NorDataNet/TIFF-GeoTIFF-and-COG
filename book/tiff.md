# TIFF: The Foundation

## **What is TIFF?**
**TIFF (Tagged Image File Format)** is a **flexible, high-quality image format** developed in the 1980s by **Aldus Corporation** (later acquired by Adobe) for **professional imaging applications**. It is widely used in:
- **Publishing and printing** (e.g., high-resolution scans, photographs).
- **Graphic design** (e.g., layered images, high-color-depth artwork).
- **Medical and scientific imaging** (e.g., microscopy, medical scans).
- **Archival purposes** (e.g., digital preservation of documents or artwork).

Unlike more common formats like **JPEG** or **PNG**, TIFF is designed for **maximum flexibility and quality**:
- **Typically lossless, but not always**: TIFF is designed to preserve original data. Lossless compression is often used with TIFF, though lossless compression is also possible.
- **High color depth**: Supports up to **64-bit** per color channel.
- **Multi-page**: Can store **multiple images** in a single file (e.g., multi-page scans or animations).
- **Extensible**: Supports **custom tags** for metadata, allowing users to add domain-specific information.

## Structure of a TIFF File

A TIFF file is organized into **three main parts**:

### **1. File Header (8 bytes)**
The header identifies the file as a TIFF and specifies basic properties:
- **Byte Order**: Indicates the **endianness** (Intel `II` or Motorola `MM`).
- **Version Number**: Always `42` (0x002A).
- **Offset to First IFD**: Byte offset to the first **Image File Directory (IFD)**.

### **2. Image File Directories (IFDs)**

The main metadata in a TIFF file is stored in **Image File Directories (IFDs)**. An IFD is a table of tags that describe how the image data should be interpreted.

Each tag contains:
  - **Tag ID** (e.g., `256` for `ImageWidth`).
  - **Data Type** (e.g., `Short`, `Long`, `ASCII`).
  - **Count**: Number of values for the tag.
  - **Value or Offset**: The actual value or a pointer to where the data is stored in the file.

**TIFF** defines a set of standard metadata tags, some of which are required to correctly interpret the image data. Other tags are optional and provide additional descriptive information. The presence of optional metadata introduces flexibility, but excessive variation can reduce interoperability between software systems.

Common TIFF tags include:

| **Tag ID** | **Tag Name** | **Requirement** | **Example Value** | **Purpose** |
|------------|--------------|-----------------|-------------------|-------------|
| 256 | ImageWidth | Required | 1920 | Number of pixels across the image. |
| 257 | ImageLength | Required | 1080 | Number of pixels down the image. |
| 258 | BitsPerSample | Required | 8 | Number of bits used to represent each sample. |
| 259 | Compression | Required | LZW / Deflate | Compression method used for image data. |
| 262 | PhotometricInterpretation | Required | RGB | Defines how pixel values should be interpreted. |
| 273 | StripOffsets | Required for stripped images | 123456 | Location of image data in the file. |
| 277 | SamplesPerPixel | Required | 3 | Number of samples per pixel (e.g. RGB channels). |
| 278 | RowsPerStrip | Required for stripped images | 256 | Number of rows stored in each strip. |
| 279 | StripByteCounts | Required for stripped images | 500000 | Size of each stored strip. |
| 282 | XResolution | Required | 300 | Horizontal resolution. |
| 283 | YResolution | Required | 300 | Vertical resolution. |
| 296 | ResolutionUnit | Required | Inch | Units used for resolution. |
| 305 | Software | Optional | GDAL | Software used to create the file. |
| 306 | DateTime | Optional | 2025:01:01 12:00:00 | File modification timestamp. |
| 315 | Artist | Optional | Example Name | Creator or source information. |

### **3. Pixel Data**
The actual image data is stored as:
- **Strips**: Rows of pixels (default in most TIFFs including GeoTIFFs).
- **Tiles**: 2D blocks of pixels (e.g., 256x256), which are more efficient for partial access (used in Cloud Optimized GeoTIFFs).

The pixel data can be:
- **Uncompressed** (raw pixel values).
- **Compressed** (e.g., LZW, JPEG).