# Rust Image Service

A HTTP service for image processing built with Rust, Axum, and the `image` crate.

## Features

- **Process images from URLs or file uploads**
- **Image transformations:**
  - Resize (with aspect ratio preservation)
  - Crop
  - Multiple filters (grayscale, blur, invert, sharpen, brighten, contrast)
- **Format conversion:**
  - PNG, JPEG, WebP, BMP, GIF
- **Quality control for lossy formats**

## Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/rust-image-service.git
cd rust-image-service

# Build the project
cargo build --release

# Run the service
cargo run --release
```

The service will start on `0.0.0.0:3000` by default.

## API

### Filter Options

| Filter | Parameters | Example | Description |
|--------|------------|---------|-------------|
| grayscale | none | `grayscale` | Convert to grayscale |
| invert | none | `invert` | Invert image colors |
| blur | sigma | `blur:3.5` | Gaussian blur with sigma value |
| sharpen | sigma, threshold | `sharpen:2.0:5` | Sharpen image |
| brighten | value | `brighten:15` | Adjust brightness (positive or negative values) |
| contrast | value | `contrast:25.5` | Adjust contrast |

### Process Image from URL

`GET /url`

Process an image from a remote URL.

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| url | string | **Required**. URL of the image to process |
| w | number | Width in pixels for resizing |
| h | number | Height in pixels for resizing |
| crop_x | number | X coordinate for crop starting point |
| crop_y | number | Y coordinate for crop starting point |
| crop_w | number | Width of the crop area |
| crop_h | number | Height of the crop area |
| filter | string | Filter to apply (e.g., "grayscale", "blur:5.0", "invert") |
| output_format | string | Output format (png, jpeg, webp, bmp, gif) |
| quality | number | Quality for JPEG/WebP (1-100) |

#### Example

```
GET /url?filter=grayscale&url=https://images.unsplash.com/photo-1574158622682-e40e69881006
```

Or, open the URL <http://localhost:3000/url?filter=grayscale&url=https://images.unsplash.com/photo-1574158622682-e40e69881006> in the browser.

### Process Uploaded Image

`POST /upload`

Process an uploaded image file.

#### Form Data Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| image | file | **Required**. Image file to process |
| w | number | Width in pixels for resizing |
| h | number | Height in pixels for resizing |
| crop_x | number | X coordinate for crop starting point |
| crop_y | number | Y coordinate for crop starting point |
| crop_w | number | Width of the crop area |
| crop_h | number | Height of the crop area |
| filter | string | Filter to apply (e.g., "grayscale", "blur:5.0", "invert") |
| output_format | string | Output format (png, jpeg, webp, bmp, gif) |
| quality | number | Quality for JPEG/WebP (1-100) |

#### Example

```
curl -X POST -F "image=@cat.jpg" -F "filter=grayscale" http://localhost:3000/upload --output cat-gray.jpg
```
