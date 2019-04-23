# KHR_lights_imageBased

## Contributors

* Gary Hsu, Microsoft
* Mike Bond, Adobe
* Norbert Nopper, UX3D
* `Please add or remove!`

## Status

Draft

## Dependencies

Written against the glTF 2.0 spec and the `KHR_texture_features` extension.

## Overview

This extension provides the ability to define image-based lights in a glTF scene. Image-based lights consist of an environment map that represents specular radiance for the scene as well as irradiance information.

Many 3D tools and engines support image-based global illumination but the exact technique and data formats employed vary. Using this extension, tools can export and engines can import image-based lights and the result should be highly consistent. 

This extension specifies exactly one way to format and reference the environment map to be used. The goals of this are two-fold. First, it makes implementing support for this extension easier. Secondly, it ensures that rendering of the image-based lighting is consistent across runtimes.

A conforming implementation of this extension must be able to load the image-based environment data and render the PBR materials using this lighting.

## Defining an Image-Based Light

The `KHR_lights_imageBased` extension defines a array of image-based lights at the root of the glTF and then each scene can reference one. Each image-based light definition consists of a single cubemap that describes the specular radiance of the scene, the l=2 spherical harmonics coefficients for diffuse irradiance and rotation, brighness factor and offset values.

```json
"extensions": {
    "KHR_lights_imageBased" : {
        "imageBasedLights": [
            {
                "rotation": [0, 0, 0, 1],
                "brightnessFactor": 1.0,
                "brightnessOffset": 0.0,
                "specularEnvironmentMap": 0,
                "diffuseSphericalHarmonics": [
                    [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0],
                    [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0],
                    [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]
                ]
            }
        ]
    }
}
```

## Specular BRDF integration and Irradiance Coefficients

This extension uses a prefiltered environment map to define the specular lighting utilizing the split sum approximation. More information:    
[Specular BRDF integration in Google Filament](https://google.github.io/filament/Filament.md.html#lighting/imagebasedlights/processinglightprobes)  
[Pre-Filtered Environment Map in Unreal Engine 4](https://blog.selfshadow.com/publications/s2013-shading-course/karis/s2013_pbs_epic_notes_v2.pdf)

This extension uses spherical harmonic coefficients to define irradiance used for diffuse lighting. Coefficients are calculated for the first 3 SH bands (l=2) and take the form of a 9x3 array. More information:  
[Realtime Image Based Lighting using Spherical Harmonics](https://metashapes.com/blog/realtime-image-based-lighting-using-spherical-harmonics/)  
[An Efficient Representation for Irradiance Environment Maps](http://graphics.stanford.edu/papers/envmap/)

## Adding Light Instances to Scenes

Each scene can have a single IBL light attached to it by defining the `extensions` and `KHR_lights_imageBased` property and, within that, an index into the `imageBasedLights` array using the `imageBasedLight` property.

```json
"scenes" : [
    {
        "extensions" : {
            "KHR_lights_imageBased" : {
                "imageBasedLight" : 0
            }
        }
    }            
]
```

### Validation and implementation notes

- If `specularEnvironmentMap` is not a cube map with mip map, the light is invalid.
- The color is encoded in linear space for all image formats.
- Formula in shader code:

```
finalSampledColor = sampledColor * brightnessFactor + brightnessOffset;
```

- Shader code does not need a change, when further formats are supported
  - Values larger than 1.0 possible
  - Negative values possible

### Encoding

#### Normal Quality (Embedded and Mobile)
- [RGBM](http://graphicrants.blogspot.com/2009/04/rgbm-color-encoding.html) 
- Range is [0;8]
  - Use `brightnessOffset` for negative values
  - Use `brightnessFactor` for larger range
- Supported in Filament and Unity

RGBM is stored as `VK_FORMAT_R8G8B8A8_UNORM`.  
It should be decoded and not be used directly. See [Light is beautiful](http://lousodrome.net/blog/light/tag/rgbm/).

#### High Quality (Console and Desktop)
- [BC6H](http://khronos.org/registry/DataFormat/specs/1.2/dataformat.1.2.html#_bc6h)
- Supported in Unity and Unreal

If BC6H is not supported, the image should be decoded and can be reduced to 8 bits per color channel.

## glTF Schema Updates

* **JSON schema**: [scene.schema.json](../../../../specification/2.0/schema/scene.schema.json) and [glTF.schema.json](../../../../specification/2.0/schema/glTF.schema.json)

## Known Implementations

None.