# KHR_lights_imageBased_texture

## Contributors

* Gary Hsu, Microsoft
* Mike Bond, Adobe
* Norbert Nopper, UX3D
* `Please add or remove!`

## Status

Draft

## Dependencies

Written against the glTF 2.0 spec and the `KHR_lights_imageBased` extension.

## Overview

This extension adds the ability to specify a prefiltered diffuse environment map for IBL.

The extension is adding the property `diffuseEnvironmentMap`.

```json
"extensions": {
    "KHR_lights_imageBased" : {
        "imageBasedLights": [
            {
                "specularEnvironmentMap": 0,
                "extensions": {
                    "KHR_lights_imageBased_texture" : {
                        "diffuseEnvironmentMap": 1
                    }
                }
            }
        ]
    }
}
```

If `diffuseEnvironmentMap` is defined, the `diffuseSphericalHarmonics` has to be ignored and/or is not required.

### Validation and implementation notes

- If `diffuseEnvironmentMap` is not a cube map without a mip map, the light is invalid.

## glTF Schema Updates

* **JSON schema**: [imageBasedLight.schema.json](schema/imageBasedLight.schema.json)

## Known Implementations

None.