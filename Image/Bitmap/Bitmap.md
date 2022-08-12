# Table of Contents
- [General](#general)
- [Structure](#structure)
    - [File Header](#file-header)
        - [Identifier](#identifier)
    - [DIB Header](#dib-header)
        - [List of DIB Headers](#list-of-dib-headers)
        - [BITMAPCOREHEADER / OS21XBITMAPHEADER Header](#bitmapcoreheader--os21xbitmapheader-header)
        - [OS22XBITMAPHEADER (16) Header](#os22xbitmapheader-16-header)
        - [BITMAPINFOHEADER Header](#bitmapinfoheader-header)
            - [Compression method table](#compression-method-table)
        - [BITMAPV2INFOHEADER Header](#bitmapv2infoheader-header)
            - [Compression method table (V2)](#compression-method-table-v2)
        - [BITMAPV3INFOHEADER Header](#bitmapv3infoheader-header)
            - [Compression method table (V3)](#compression-method-table-v3)
        - [OS22XBITMAPHEADER Header](#os21xbitmapheader-header)
            - [Compression method table (OS22X)](#compression-method-table-os22x)
            - [Halftoning algorithm](#halftoning-algorithm)
        - [BITMAPV4HEADER](#bitmapv4header)
            - [Compression method table (V4)](#compression-method-table-v4)
            - [Endpoints (V4)](#endpoints-v4)
        - [BITMAPV5HEADER](#bitmapv5header)
            - [Compression method table (V5)](#compression-method-table-v5)
            - [Color space table](#color-space-table)
            - [Endpoints (V5)](#endpoints-v5)
            - [Intent table](#intent-table)
    - [Extra bit masks](#extra-bit-masks)
        - [RGB bit fields](#rgb-bit-fields)
        - [RGBA bit fields](#rgba-bit-fields)
    - [Color Table](#color-table)
    - [Colors for DIP header bpp and compression](#colors-for-dip-header-bpp-and-compression)
    - [Pixel Storage](#pixel-storage)
        - [Compression Methods](#compression-methods)
            - [RLE 8-bit/pixel](#rle-4-bitpixel)
            - [RLE 4-bit/pixel](#rle-4-bitpixel)
            - [RGB bit field masks](#rgb-bit-field-masks)
            - [JPEG image](#jpeg-image)
            - [PNG image](#png-image)
            - [RGBA bit field masks](#rgba-bit-field-masks)
            - [RLE-24](#rle-24)
            - [Huffman 1D](#huffman-1d)
    - [ICC Color Profile](#icc-color-profile)
- [Sources](#sources)

# General

Extensions:
- `.bmp`
- `.dib`

Internet media type (MIME):
- `image/bmp`
- `image/x-bmp`

Type code:
- `'BMP '`
- `'BMPf'`
- `'BMPp'`

Uniform type identifier
- `com.microsoft.bmp`

# Structure
Total size: 32 - ?

All numerical values are stored in the *Little-Endian* byte order.

The exact structure of the file depends on the **DIB header**.

| Size                          | Index | Name              | Description                                     |
|-------------------------------|-------|-------------------|-------------------------------------------------|
| 14                            | 0     | File header       | general information about the file              |
| 12, 16, 40, 52, 56, 108, 124  | 14    | DIB header        | detailed information about the image and format |
| 0, 12, 16                     | ?     | Extra bit masks   | defines pixel format                            |
| 0, ?                          | ?     | Color table       | defines colors in **pixel array**               |
| 0, ?                          | ?     | Gap 1             | alignment                                       |
| 0, ?                          | ?     | Pixel array       | Defines actual values of pixels                 |
| 0, ?                          | ?     | Gap 2             | alignment                                       |
| 0, ?                          | ?     | ICC color profile | color profile for color managment               |

## File Header
- Total size: 14
- Mandatory

| Size | Index | Name       | Description                         | Value                              |
|------|-------|------------|-------------------------------------|------------------------------------|
| 2    | 0     | Identifier | identifies valid bitmap file        | "BM", "BA", "CI", "CP", "IC", "PT" |
| 4    | 2     | File size  | size of the whole BMP file          | LE unsigned                        |
| 2    | 6     | Reserved 1 | app dependent                       |                                    |
| 2    | 8     | Reserved 2 | app dependent                       |                                    |
| 4    | 10    | Offset     | position of **pixel array** in file | LE unsigned                        |

### Identifier
- "BM" Windows bitmap
- "BA" OS/2 struct bitmap array
- "CI" OS/2 struct color icon
- "CP" OS/2 const color pointer
- "IC" OS/2 struct icon
- "PT" OS/2 pointer

## DIB Header
- Total size: 12, 16, 40, 52, 56, 108, 124
- Mandatory

There are multiple DIB headers which are differentiated by their size,
specified in the first 4 bytes.

### List of DIB Headers
| Size | Name               | Comment      |
|------|--------------------|--------------|
| 12   | BITMAPCOREHEADER   |              |
| 12   | OS21XBITMAPHEADER  |              |
| 16   | OS22XBITMAPHEADER  |              |
| 40   | BITMAPINFOHEADER   |              |
| 52   | BITMAPV2INFOHEADER | undocumented |
| 56   | BITMAPV3INFOHEADER | undocumented |
| 108  | BITMAPV4INFOHEADER |              |
| 124  | BITMAPV5INFOHEADER |              |

### BITMAPCOREHEADER / OS21XBITMAPHEADER Header
Total size: 12

| Size | Index | Name        | Description              | Value       |
|------|-------|-------------|--------------------------|-------------|
| 4    | 0     | Size        | size of this header      | 12          |
| 2    | 4     | Width       | bitmap width in pixels   | LE unsigned |
| 2    | 6     | Height      | bitmap height in pixels  | LE unsigned |
| 2    | 8     | Plane Count | number of color planes   | 1           |
| 2    | 10    | Bpp         | number of bits per pixel | 1, 4, 8, 24 |

### OS22XBITMAPHEADER (16) Header
Total size: 16

| Size | Index | Name        | Description              | Value           |
|------|-------|-------------|--------------------------|-----------------|
| 4    | 0     | Size        | size of this header      | 16              |
| 4    | 4     | Width       | bitmap width in pixels   | LE unsigned?    |
| 4    | 8     | Height      | bitmap height in pixels  | LE unsigned?    |
| 2    | 12    | Plane Count | number of color planes   | 1               |
| 2    | 14    | Bpp         | number of bits per pixel | 1, 2, 4, 8, 24  |

### BITMAPINFOHEADER Header
Total size: 40

| Size | Index | Name                  | Description                       | Value                     |
|------|-------|-----------------------|-----------------------------------|---------------------------|
| 4    | 0     | Size                  | size of this header               | 40                        |
| 4    | 4     | Width                 | bitmap width in pixels            | LE signed                 |
| 4    | 8     | Height                | bitmap height in pixels           | LE signed                 |
| 2    | 12    | Plane Count           | number of color planes            | 1                         |
| 2    | 14    | Bpp                   | number of bits per pixel          | 0, 1, 2, 4, 8, 16, 24, 32 |
| 4    | 16    | Compression           | compression method used           | 0 - 6, 11 - 13            |
| 4    | 20    | Data size             | actual size of bitmap             | LE unsigned               |
| 4    | 24    | Horizontal resolution | pixels per meter                  | LE signed                 |
| 4    | 28    | Vertical resolution   | pixels per meter                  | LE signed                 |
| 4    | 32    | Palette size          | number of pixels in color palette | 0 - 2^Bpp; LE unsigned    |
| 4    | 36    | Important color count | minimum color indexes, 0 is all   | 0 - 2^Bpp; LE unsigned    |

- **Bpp** of length 0 is used only when the compression **BI_JPEG** or **BI_PNG** is used
- **Important color count** is generally ignored

#### Compression method table
| Value | Name              | Method               |
|-------|-------------------|----------------------|
| 0     | BI_RGB            | none                 |
| 1     | BI_RLE8           | RLE 8-bit/pixel      |
| 2     | BI_RLE4           | RLE 4-bit/pixel      |
| 3     | BI_BITFIELDS      | RGB bit field masks  |
| 4     | BI_JPEG           | jpeg image           |
| 5     | BI_PNG            | png image            |
| 6     | BA_ALPHABITFIELDS | RGBA bit field masks |
| 11    | BI_CMYK           | none                 |
| 12    | BI_CMYKRLE8       | RLE-8                |
| 13    | BI_CMYKRLE4       | RLE-4                |

### BITMAPV2INFOHEADER Header
Total size: 52

| Size | Index | Name                  | Description                       | Value                     |
|------|-------|-----------------------|-----------------------------------|---------------------------|
| 4    | 0     | Size                  | size of this header               | 52                        |
| 4    | 4     | Width                 | bitmap width in pixels            | LE signed                 |
| 4    | 8     | Height                | bitmap height in pixels           | LE signed                 |
| 2    | 12    | Plane Count           | number of color planes            | 1                         |
| 2    | 14    | Bpp                   | number of bits per pixel          | 0, 1, 2, 4, 8, 16, 24, 32 |
| 4    | 16    | Compression           | compression method used           | 0 - 5, 11 - 13            |
| 4    | 20    | Data size             | actual size of bitmap             | LE unsigned               |
| 4    | 24    | Horizontal resolution | pixels per meter                  | LE signed                 |
| 4    | 28    | Vertical resolution   | pixels per meter                  | LE signed                 |
| 4    | 32    | Palette size          | number of pixels in color palette | 0 - 2^Bpp; LE unsigned    |
| 4    | 36    | Important color count | minimum color indexes, 0 is all   | 0 - 2^Bpp; LE unsigned    |
| 4    | 40    | Red mask              | red channel bit mask              | LE                        |
| 4    | 44    | Green mask            | green channel bit mask            | LE                        |
| 4    | 48    | Blue mask             | blue channel bit mask             | LE                        |

- **Bpp** of length 0 is used only when the compression **BI_JPEG** or **BI_PNG** is used
- **Important color count** is generally ignored

#### Compression method table (V2)
| Value | Name              | Method               |
|-------|-------------------|----------------------|
| 0     | BI_RGB            | none                 |
| 1     | BI_RLE8           | RLE 8-bit/pixel      |
| 2     | BI_RLE4           | RLE 4-bit/pixel      |
| 3     | BI_BITFIELDS      | RGB bit field masks  |
| 4     | BI_JPEG           | jpeg image           |
| 5     | BI_PNG            | png image            |
| 11    | BI_CMYK           | none                 |
| 12    | BI_CMYKRLE8       | RLE-8                |
| 13    | BI_CMYKRLE4       | RLE-4                |

### BITMAPV3INFOHEADER Header
Total size: 56

| Size | Index | Name                  | Description                       | Value                     |
|------|-------|-----------------------|-----------------------------------|---------------------------|
| 4    | 0     | Size                  | size of this header               | 52                        |
| 4    | 4     | Width                 | bitmap width in pixels            | LE signed                 |
| 4    | 8     | Height                | bitmap height in pixels           | LE signed                 |
| 2    | 12    | Plane Count           | number of color planes            | 1                         |
| 2    | 14    | Bpp                   | number of bits per pixel          | 0, 1, 2, 4, 8, 16, 24, 32 |
| 4    | 16    | Compression           | compression method used           | 0 - 5, 11 - 13            |
| 4    | 20    | Data size             | actual size of bitmap             | LE unsigned               |
| 4    | 24    | Horizontal resolution | pixels per meter                  | LE signed                 |
| 4    | 28    | Vertical resolution   | pixels per meter                  | LE signed                 |
| 4    | 32    | Palette size          | number of pixels in color palette | 0 - 2^Bpp; LE unsigned    |
| 4    | 36    | Important color count | minimum color indexes, 0 is all   | 0 - 2^Bpp; LE unsigned    |
| 4    | 40    | Red mask              | red channel bit mask              | LE                        |
| 4    | 44    | Green mask            | green channel bit mask            | LE                        |
| 4    | 48    | Blue mask             | blue channel bit mask             | LE                        |
| 4    | 52    | Alpha mask            | alpha channel bit mask            | LE                        |

- **Bpp** of length 0 is used only when the compression **BI_JPEG** or **BI_PNG** is used
- **Important color count** is generally ignored

#### Compression method table (V3)
| Value | Name              | Method               |
|-------|-------------------|----------------------|
| 0     | BI_RGB            | none                 |
| 1     | BI_RLE8           | RLE 8-bit/pixel      |
| 2     | BI_RLE4           | RLE 4-bit/pixel      |
| 3     | BI_BITFIELDS      | RGBA bit field masks |
| 4     | BI_JPEG           | jpeg image           |
| 5     | BI_PNG            | png image            |
| 11    | BI_CMYK           | none                 |
| 12    | BI_CMYKRLE8       | RLE-8                |
| 13    | BI_CMYKRLE4       | RLE-4                |

### OS22XBITMAPHEADER Header
Total size: 64

| Size | Index | Name                   | Description                       | Value                   |
|------|-------|------------------------|-----------------------------------|-------------------------|
| 4    | 0     | Size                   | size of this header               | 64                      |
| 4    | 4     | Width                  | bitmap width in pixels            | LE unsigned?            |
| 4    | 8     | Height                 | bitmap height in pixels           | LE unsigned?            |
| 2    | 12    | Plane Count            | number of color planes            | 1                       |
| 2    | 14    | Bpp                    | number of bits per pixel          | LE unsigned?            |
| 4    | 16    | Compression            | compression method used           | 0 - 4                   |
| 4    | 20    | Data size              | actual size of bitmap             | LE unsigned?            |
| 4    | 24    | Horizontal resolution  | pixels per meter                  | LE signed               |
| 4    | 28    | Vertical resolution    | pixels per meter                  | LE signed               |
| 4    | 32    | Palette size           | number of pixels in color palette | 0 - 2^Bpp; LE unsigned? |
| 4    | 36    | Important color count  | number of important colors used   | LE unsigned?            |
| 2    | 40    | Resolution unit        | pixels per meter                  | 0                       |
| 2    | 42    | Padding                | ignored                           | 0                       |
| 2    | 44    | Fill Direction         | left-right; top-bottom            | 0                       |
| 2    | 46    | Halftoning algorithm   | halftoning algorithm to use       | 0 - 3                   |
| 4    | 50    | Halftoning parameter 1 | meaning based on algorithm        | LE signed?              |
| 4    | 54    | Halftoning parameter 2 | meaning based on algorithm        | LE signed?              |
| 4    | 56    | Color encoding         | RGB                               | 0                       |
| 4    | 60    | App-defined identifier | application-defined identifier    |                         |

- **Important color count** is generally ignored

#### Compression method table (OS22X)
| Value | Name              | Method               |
|-------|-------------------|----------------------|
| 0     | BI_RGB            | none                 |
| 1     | BI_RLE8           | RLE 8-bit/pixel      |
| 2     | BI_RLE4           | RLE 4-bit/pixel      |
| 3     | BI_BITFIELDS      | Huffman 1D           |
| 4     | BI_JPEG           | RLE-24               |

#### Halftoning algorithm
| Value | Algorithm       | Halftoning parameter 1               | Halftoning parameter 2               |
|-------|-----------------|--------------------------------------|--------------------------------------|
| 0     | none            |                                      |                                      |
| 1     | error diffusion | 0 - 100; percentage of error damping |                                      |
| 2     | PANDA           | x dimension of the pattern in pixels | y dimension of the pattern in pixels |
| 3     | super-circle    | x dimension of the pattern in pixels | y dimension of the pattern in pixels |

### BITMAPV4HEADER
Total size: 108

| Size | Index | Name                  | Description                       | Value                     |
|------|-------|-----------------------|-----------------------------------|---------------------------|
| 4    | 0     | Size                  | size of this header               | 52                        |
| 4    | 4     | Width                 | bitmap width in pixels            | LE signed                 |
| 4    | 8     | Height                | bitmap height in pixels           | LE signed                 |
| 2    | 12    | Plane Count           | number of color planes            | 1                         |
| 2    | 14    | Bpp                   | number of bits per pixel          | 0, 1, 2, 4, 8, 16, 24, 32 |
| 4    | 16    | Compression           | compression method used           | 0 - 5, 11 - 13            |
| 4    | 20    | Data size             | actual size of bitmap             | LE unsigned               |
| 4    | 24    | Horizontal resolution | pixels per meter                  | LE signed                 |
| 4    | 28    | Vertical resolution   | pixels per meter                  | LE signed                 |
| 4    | 32    | Palette size          | number of pixels in color palette | 0 - 2^Bpp; LE unsigned    |
| 4    | 36    | Important color count | minimum color indexes, 0 is all   | 0 - 2^Bpp; LE unsigned    |
| 4    | 40    | Red mask              | red channel bit mask              | LE                        |
| 4    | 44    | Green mask            | green channel bit mask            | LE                        |
| 4    | 48    | Blue mask             | blue channel bit mask             | LE                        |
| 4    | 52    | Alpha mask            | alpha channel bit mask            | LE                        |
| 4    | 56    | Color Space           | calibrated rgb values             | 0                         |
| 36   | 60    | Endpoints             | chromacity values                 |                           |
| 4    | 96    | Gamma red             | response curve for red            | fixed 8.8 << 8            |
| 4    | 100   | Gamma green           | response curve for green          | fixed 8.8 << 8            |
| 4    | 104   | Gamma blue            | response curve for blue           | fixed 8.8 << 8            |

- **Bpp** of length 0 is used only when the compression **BI_JPEG** or **BI_PNG** is used
- **Important color count** is generally ignored

#### Compression method table (V4)
| Value | Name              | Method               |
|-------|-------------------|----------------------|
| 0     | BI_RGB            | none                 |
| 1     | BI_RLE8           | RLE 8-bit/pixel      |
| 2     | BI_RLE4           | RLE 4-bit/pixel      |
| 3     | BI_BITFIELDS      | RGBA bit field masks |
| 4     | BI_JPEG           | jpeg image           |
| 5     | BI_PNG            | png image            |
| 11    | BI_CMYK           | none                 |
| 12    | BI_CMYKRLE8       | RLE-8                |
| 13    | BI_CMYKRLE4       | RLE-4                |

#### Endpoints (V4)
Total size: 36

| Size | Index | Name    | Value      |
|------|-------|---------|------------|
| 4    | 0     | red x   | fixed 2.30 |
| 4    | 4     | red y   | fixed 2.30 |
| 4    | 8     | red z   | fixed 2.30 |
| 4    | 12    | green x | fixed 2.30 |
| 4    | 16    | green y | fixed 2.30 |
| 4    | 20    | green z | fixed 2.30 |
| 4    | 24    | blue x  | fixed 2.30 |
| 4    | 28    | blue y  | fixed 2.30 |
| 4    | 32    | blue z  | fixed 2.30 |

### BITMAPV5HEADER
Total size: 124

| Size | Index | Name                  | Description                                        | Value                     |
|------|-------|-----------------------|----------------------------------------------------|---------------------------|
| 4    | 0     | Size                  | size of this header                                | 52                        |
| 4    | 4     | Width                 | bitmap width in pixels                             | LE signed                 |
| 4    | 8     | Height                | bitmap height in pixels                            | LE signed                 |
| 2    | 12    | Plane Count           | number of color planes                             | 1                         |
| 2    | 14    | Bpp                   | number of bits per pixel                           | 0, 1, 2, 4, 8, 16, 24, 32 |
| 4    | 16    | Compression           | compression method used                            | 0 - 5, 11 - 13            |
| 4    | 20    | Data size             | actual size of bitmap                              | LE unsigned               |
| 4    | 24    | Horizontal resolution | pixels per meter                                   | LE signed                 |
| 4    | 28    | Vertical resolution   | pixels per meter                                   | LE signed                 |
| 4    | 32    | Palette size          | number of pixels in color palette                  | 0 - 2^Bpp; LE unsigned    |
| 4    | 36    | Important color count | minimum color indexes, 0 is all                    | 0 - 2^Bpp; LE unsigned    |
| 4    | 40    | Red mask              | red channel bit mask                               | LE                        |
| 4    | 44    | Green mask            | green channel bit mask                             | LE                        |
| 4    | 48    | Blue mask             | blue channel bit mask                              | LE                        |
| 4    | 52    | Alpha mask            | alpha channel bit mask                             | LE                        |
| 4    | 56    | Color Space           | defines color space                                | 0, 0x4C494E4B, 0x4D424544 |
| 36   | 60    | Endpoints             | chromacity values                                  |                           |
| 4    | 96    | Gamma red             | response curve for red                             | fixed 8.8 << 8            |
| 4    | 100   | Gamma green           | response curve for green                           | fixed 8.8 << 8            |
| 4    | 104   | Gamma blue            | response curve for blue                            | fixed 8.8 << 8            |
| 4    | 108   | Intent                | rendering intent                                   | 1 \| 2 \| 4 \| 8          |
| 4    | 112   | Color data            | offset from start this header to the color profile | LE unsigned               |
| 4    | 116   | Profile size          | size of the color profile in bytes                 | LE unsigned               |
| 4    | 120   | Reserved              | should be ignored                                  | LE unsigned               |

- **Bpp** of length 0 is used only when the compression **BI_JPEG** or **BI_PNG** is used
- **Important color count** is generally ignored

#### Compression method table (V5)
| Value | Name              | Method               |
|-------|-------------------|----------------------|
| 0     | BI_RGB            | none                 |
| 1     | BI_RLE8           | RLE 8-bit/pixel      |
| 2     | BI_RLE4           | RLE 4-bit/pixel      |
| 3     | BI_BITFIELDS      | RGBA bit field masks |
| 4     | BI_JPEG           | jpeg image           |
| 5     | BI_PNG            | png image            |
| 11    | BI_CMYK           | none                 |
| 12    | BI_CMYKRLE8       | RLE-8                |
| 13    | BI_CMYKRLE4       | RLE-4                |

#### Color space table
| Value      | Name                | Description           |
|------------|---------------------|-----------------------|
| 0          | LCS_CALIBRATED_RGB  | calibrated rgb values |
| 0x4C494E4B | LCS_PROFILE_LINKED  | profile is linked     |
| 0x4D424544 | LCS_PROFILE_EMBEDED | profile is embeded    |

#### Endpoints (V5)
Total size: 36

| Size | Index | Name    | Value      |
|------|-------|---------|------------|
| 4    | 0     | red x   | fixed 2.30 |
| 4    | 4     | red y   | fixed 2.30 |
| 4    | 8     | red z   | fixed 2.30 |
| 4    | 12    | green x | fixed 2.30 |
| 4    | 16    | green y | fixed 2.30 |
| 4    | 20    | green z | fixed 2.30 |
| 4    | 24    | blue x  | fixed 2.30 |
| 4    | 28    | blue y  | fixed 2.30 |
| 4    | 32    | blue z  | fixed 2.30 |

#### Intent table
| Value | Name                | Description                          |
|-------|---------------------|--------------------------------------|
| 1     | LCS_GM_BUSINESS     | saturation should be mantained       |
| 2     | LCS_GM_GRAPHICS     | colometric match should be mantained |
| 4     | LCS_GM_IMAGES       | contrast should be mantained         |
| 8     | LCS_GM_COLORIMETRIC | white point should be mantained      |

## Extra bit masks
Can be present only when using the **BITMAPINFOHEADER**.

### RGB bit fields
Present only if the **compression** is set to **BI_BITFIELDS**.

| Size | Index | Name       | Description            | Value |
|------|-------|------------|------------------------|-------|
| 4    | 0     | Red mask   | Red channel bit mask   | LE    |
| 4    | 4     | Green mask | Green channel bit mask | LE    |
| 4    | 8     | Blue maske | Blue channel bit mask  | LE    |

### RGBA bit fields
Present only if the **compression** is set to **BI_ALPHABITFIELDS**

| Size | Index | Name       | Description            | Value |
|------|-------|------------|------------------------|-------|
| 4    | 0     | Red mask   | Red channel bit mask   | LE    |
| 4    | 4     | Green mask | Green channel bit mask | LE    |
| 4    | 8     | Blue mask  | Blue channel bit mask  | LE    |
| 4    | 12    | Alpha mask | Alpha channel bit mask | LE    |

## Color table
- Indexed list of colors in the picture
- Has length of **palette length** or 2^**bpp**
- Present only when **bpp** is smaller or equal to 8.

## Colors for DIP header bpp and compression
- shown in R.G.B.A.X notation
- the order of colors is always BGRAX
- OS versions are unknown
- Colors for **bpp** less than or equal to 8 are for the **color table**

| Bpp | BITMAPCOREHEADER / OS21XBITMAPHEADER <br> (OS22XBITMAPHEADER (16))? | BITMAPINFOHEADER <br> OS22XBITMAPHEADER? | BITMAPV2INFOHEADER | BITMAPV3INFOHEADER <br> BITMAPV4HEADER <br> BITMAPV5HEADER | BITMAPINFOHEADER <br> BITMAPV2INFOHEADER | BITMAPV3INFOHEADER <br> BITMAPV4HEADER <br> BITMAPV5HEADER | BITMAPINFOHEADER                   |
|-----|---------------------------------------------------------------------|------------------------------------------|--------------------|------------------------------------------------------------|------------------------------------------|------------------------------------------------------------|------------------------------------|
| --- | N/A                                                                 | BI_RGB                                   | BI_RGB             | BI_RGB                                                     | BI_BITFIELDS                             | BI_BITFIELDS                                               | BI_ALPHABITFIELDS                  |
| 1   | 8.8.8.0.0                                                           | 8.8.8.0.8                                | 8.8.8.0.8          | 8.8.8.0.8                                                  | Illegal                                  | Illegal                                                    | Illegal                            |
| 2   | Illegal                                                             | 8.8.8.0.8                                | 8.8.8.0.8?         | 8.8.8.0.8?                                                 | Illegal                                  | Illegal                                                    | Illegal                            |
| 4   | 8.8.8.0.0                                                           | 8.8.8.0.8                                | 8.8.8.0.8          | 8.8.8.0.8                                                  | Illegal                                  | Illegal                                                    | Illegal                            |
| 8   | 8.8.8.0.0                                                           | 8.8.8.0.8                                | 8.8.8.0.8          | 8.8.8.0.8                                                  | Illegal                                  | Illegal                                                    | Illegal                            |
| 16  | Illegal                                                             | 5.5.5.0.1                                | 5.5.5.0.1          | 5.5.5.(0-1).(0-1)                                          | (0-16).(0-16).(0-16).0.(0-16)            | (0-16).(0-16).(0-16).(0-16).(0-16)                         | (0-16).(0-16).(0-16).(0-16).(0-16) |
| 24  | 8.8.8.0.0                                                           | 8.8.8.0.0                                | 8.8.8.0.0          | 8.8.8.0.0                                                  | Illegal                                  | Illegal                                                    | Illegal                            |
| 32  | Illegal                                                             | 8.8.8.0.8                                | 8.8.8.0.8          | 8.8.8.(0-8).(0-8)                                          | (0-32).(0-32).(0-32).0.(0-32)            | (0-32).(0-32).(0-32).(0-32).(0-32)                         | (0-32).(0-32).(0-32).(0-32).(0-32) |

## Pixel storage
- The bits representing the bitmap pixels are packed in rows
- Each row is padded at the end, so that the byte count of that row is divisible by 4
- Pixels in row are ordered from left to right
- Rows are ordered from bottom to top for positive height, for negative height they are ordered from top to bottom

### Compression methods
| Method               |
|----------------------|
| none                 |
| RLE 8-bit/pixel      |
| RLE 4-bit/pixel      |
| RGB bit field masks  |
| jpeg image           |
| png image            |
| RGBA bit field masks |
| RLE-24               |
| Huffman-1D           |

### none
- No compression

### RLE 8-bit/pixel
- Run length encoding for 8 bpp bitmaps
- Each pixel is represented with 2 bytes

| Size | Index | Name  | Description                               |
|------|-------|-------|-------------------------------------------|
| 1    | 0     | Count | how many times to repeat the color        |
| 1    | 1     | Color | index to the color in the **color table** |

### RLE 4-bit/pixel
- Run length encoding for 4 bpp bitmaps
- Each pixel is represented with 2 bytes

| Size | Index | Name  | Description                                                 |
|------|-------|-------|-------------------------------------------------------------|
| 1    | 0     | Count | how many times to repeat the colors                         |
| 1    | 1     | Color | 2 packed 4-bit indexes to the colors in the **color table** |

### RGB bit field masks
- Continous non-overlapping bit-field RGB masks

### JPEG image
- JPEG compressed image for printing

### PNG image
- PNG compressed image for printing

### RGBA bit field masks
- Continous non-overlapping bit-field RGBA masks

### RLE-24
- (PackBits where literal is one 24-bit pixel)?

### Huffman 1D
- ?

## ICC Color Profile
- Can be present only when using the **BITMAPV5HEADER**
- This is a ICC profile if the **Color Space** field is set to **LCS_PROFILE_EMBEDED**
- If the **Color Space** field is set to **ICS_PROFILE_LINKED** this is a null terminated string in Windows-1252 encoding that points to a file with ICC color profile
- Position of this segment is written in the field **Color Data** as a offset from beggining of the header
- Length of this segment is written in the field **Profile Size**

# Sources

- [wikipedia (BMP File Format)](https://en.wikipedia.org/wiki/BMP_file_format)
- [liquisearch (DIP Header)](https://www.liquisearch.com/bmp_file_format/file_structure/dib_header_bitmap_information_header)
- [gitgub (BITMAPCOREHEADER)](https://github.com/MicrosoftDocs/sdk-api/blob/docs/sdk-api-src/content/wingdi/ns-wingdi-bitmapcoreheader.md)
- [github (BITMAPINFOHEADER)](https://github.com/MicrosoftDocs/sdk-api/blob/docs/sdk-api-src/content/wingdi/ns-wingdi-bitmapinfoheader.md)
- [github (BITMAPV4HEADER)](https://github.com/MicrosoftDocs/sdk-api/blob/docs/sdk-api-src/content/wingdi/ns-wingdi-bitmapv4header.md)
- [microsoft docs (BITMAPV4HEADER - Color space)](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-wmf/eb4bbd50-b3ce-4917-895c-be31f214797f)
- [microsotf docs (BITMAPV4HEADER)](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-wmf/071b0c0d-c2df-4f1c-9828-d03c26002c61)
- [microsoft docs (BITMAPINFOHEADER)](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-wmf/567172fa-b8a2-4d79-86a2-5e21d6659ef3)
- [microsoft docs (Compression Enumeration)](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-wmf/4e588f70-bd92-4a6f-b77f-35d0feaf7a57)
- [microsoft docs (BitCount Enumaration)](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-wmf/792153f4-1e99-4ec8-93cf-d171a5f33903)
- [microsoft docs (Endpoints)](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-wmf/07b63c50-c07b-44bc-9523-be4e90d20a8e)
- [microsoft docs (Endpoints - chromacity)](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-wmf/21164882-3cb3-49d6-8133-74396704f14d)
- [microsoft docs (BITMAPV5HEADER)](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-wmf/8a37c67b-dab0-4693-98a9-fe0499095da6)
- [microsoft docs (BITMAPV5HEADER - Color space)](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-wmf/3c289fe1-c42e-42f6-b125-4b5fc49a2b20)
- [microsoft docs (BITMAPV5HEADER - Intent)](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-wmf/9fec0834-607d-427d-abd5-ab240fb0db38)