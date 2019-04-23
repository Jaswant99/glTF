# KHR_texture_features 

## Contributors

* Gary Hsu, Microsoft
* Mike Bond, Adobe
* Norbert Nopper, UX3D
* `Please add or remove!`

## Status

Draft

## Dependencies

Written against the glTF 2.0 spec and the `KHR_image_features` extension.

## Overview

This extension adds the ability to specify textures having cube and mip maps.

The extension is adding the properties `type` and `mipMap`.

```json
"textures": [
    {
        "source": 0,
        "extensions": {
            "KHR_texture_features" : {
                "type": "2D",
                "mipMap": false
            }
        }
    }
]
```

### Validation and implementation notes

- If `mipMap` or `type` does not match the source image, the texture is invalid.
- Both `2D` and `Cube` can have `mipMap` set to `true`.
- If `2D` texture do have a mip map chain, use these and do not generate them.
- Material textures referencing textures having several faces and/or a floating point image are invalid.

## glTF Schema Updates

* **JSON schema**: [texture.schema.json](../../../../specification/2.0/schema/texture.schema.json)

## Known Implementations

None.