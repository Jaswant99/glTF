# KHR_image_features 

## Contributors

* Gary Hsu, Microsoft
* Mike Bond, Adobe
* Norbert Nopper, UX3D
* `Please add or remove!`

## Status

Draft

## Dependencies

Written against the glTF 2.0 spec.

## Overview

This extension adds the ability to specify images using the [KTX file format](http://github.khronos.org/KTX-Specification/) (KTX). An implementation of this extension can use the images provided in the KTX files as an alternative to the PNG or JPG images available in glTF 2.0.
Furthermore, specifying images having mip map levels and/or cube map faces is possible.

The extension is adding the value `image/ktx` to the `mimeType` property. Also the properties `mipLevels` and `faces` are added.

```json
"images": [
    {
        "uri": "KTXimage.ktx",
        "mimeType": "image/ktx",
        "extensions": {
            "KHR_image_features" : {
                "mipLevels": 1,
                "faces": 0
            }
        }
    }
]
```

### Validation and implementation notes

For maximum compatibility using the KTX file format and its cube and mip maps, following limitations are defined for its fields:

`vkFormat`
- `VK_FORMAT_R8G8B8_UNORM`
- `VK_FORMAT_R8G8B8A8_UNORM`
- `VK_FORMAT_BC6H_SFLOAT_BLOCK`

`pixelDepth`
- Only 0

`numberOfArrayElements`
- Only 0

`numberOfFaces`
- Either 1 or 6

`supercompressionScheme`
- Only 0

If `mipLevels` and/or `faces` does not match the file, the image is invalid.

## glTF Schema Updates

* **JSON schema**: [image.schema.json](../../../../specification/2.0/schema/image.schema.json)

## Known Implementations

None.