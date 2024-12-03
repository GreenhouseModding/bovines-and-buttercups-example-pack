# Bovines and Buttercups 2.0.0 Datapacking Guide

## Examples

An example resource pack and data pack can be found in [this repository](https://github.com/GreenhouseModding/bovines-and-buttercups-example-pack) under their respective folders.

## Table of Contents

- [Cow Types](#cow-types)
- [Nectar](#nectar)
- [Flower Crown Material](#flower-crown-material)
- [Custom Flower Type](#custom-flower-type)
- [Custom Mushroom Type](#custom-mushroom-type)
- [Cupcake](#cupcake)
- [Usage In Recipes](#usage-in-recipes)

## Cow Types

Cow Types are data that can be used to add new mooblooms or mooshrooms into the game.

The data has different fields depending on the `"type"` field. With just Bovines and Buttercups, this can be either `"bovinesandbuttercups:moobloom"`, or `"bovinesandbuttercups:mooshroom"`.

Cow Types are defined in the `/data/<namespace>/bovinesandbuttercups/cow_type` directory in your datapacks.

### Data Examples

<details>
<summary>bovinesandbuttercups:moobloom</summary>

```jsonc
{
    "type": "bovinesandbuttercups:moobloom",
    // Defines the flower that is associated with the adult moobloom.
    // This will appear on a moobloom of this type's back, which block a moobloom of this type will spread when bone meal is used on it, and what item you obtain when shearing a moobloom with this type.
    "flower": {
        // Accepts one (or more) of:
        // A block state. Accepts a "Name" and "Properties" field.
        "block_state": {
            "Name": "minecraft:bamboo",
            "Properties": {
                "age": 1,
                "leaves": "small"
            }
        },
        // A bovines model set (defined in /assets/<namespace>/bovinesandbuttercups/). Has to be of the "bovinesandbuttercups:generic" type.
        // If set alone, blocks/items will not be associated with the other functionalities of this field.
        "model_set": "example:generic_model_set",
        // A custom flower type to use
        // You may read the flower type section to get more information on this.
        "custom_flower": "example:fire_flower"
    },
    // Defines the bud that appears on a baby moobloom of this type's back.
    "bud": {
        "block_state": {
            "Name": "minecraft:bamboo_sapling"
        }
    },
    // The location of the entity texture for this type. Mapped to "assets/<namespace>/textures/entity/<path>.png"
    // This field is optional. If not set, will be set to "assets/<file_namespace>/textures/entity/bovinesandbuttercups/moobloom/<file_path>_moobloom.png"
    "texture_location": "example:bovinesandbuttercups/moobloom/example_moobloom",
    // Textures to render on top of the base moobloom texture.
    // In the base mod, this is used for the grass/moss/snow layers on each moobloom.
    // This field is optional.
    // The below will render the grass layer normally outside of biomes that are tagged as #minecraft:is_jungle, and will render emissively when in these biomes.
    "layers": [
        {
            // The location of the entity texture for this type. Mapped to "assets/<namespace>/textures/entity/<path>.png"
            "texture_location": "bovinesandbuttercups:bovinesandbuttercups/moobloom/moobloom_grass_layer",
            // The texture modifiers for this layer. These will have different effects on the texture when applied.
            // You may read the texture modifier section to get more information on these.
            "texture_modifiers": [
                {
                    "type": "bovinesandbuttercups:fallback"
                },
                {
                    "type": "bovinesandbuttercups:grass_tint"
                }
            ]
        },
        {
            "texture_location": "bovinesandbuttercups:bovinesandbuttercups/moobloom/moobloom_grass_layer",
            "texture_modifiers": [
                {
                    "type": "bovinesandbuttercups:conditioned",
                    "id": "example:example_moobloom/conditioned_modifier",
                    "condition": [
                        {
                            "condition": "minecraft:entity_properties",
                            "entity": "this",
                            "predicate": {
                                "location": {
                                    "biomes": "#minecraft:is_jungle"
                                }
                            }
                        }
                    ]
                },
                {
                    "type": "bovinesandbuttercups:grass_tint"
                },
                {
                    "type": "bovinesandbuttercups:emissive"
                }
            ]
        }
    ],
    // The biomes in which this entity will spawn. 
    // This field is optional.
    "natural_spawns": [
        // Accepts any amounts of a mix of either:
        // A biome key or biome tag (prefixed with #), specifying this way will always have a weight of 1.
        "#minecraft:is_jungle",
        // A weighted entry.
        {
            "biomes": "minecraft:bamboo_jungle",
            "weight": 5
        }
    ],
    // The types that this moobloom may change into when hit by thunder, randomised from the values in this list.
    // This field is optional.
    "thunder_conversion_types": [
        // Accepts any amounts of a mix of either:
        // A cow type key, specifying this way will always have a weight of 1.
        "bovinesandbuttercups:tropical_blue",
        // A weighted entry.
        {
            "type": "bovinesandbuttercups:chargelily",
            "weight": 7
        }
    ],
    // Conditions used for determining if a baby moobloom should become this type upon being bred.
    // Below is a condition that checks if one parent is a tropical blue moobloom and the other is a limelight moobloom.
    // See: https://minecraft.wiki/w/Predicate
    // This field is optional.
    "offspring_conditions": {
        "this_conditions": [
            {
                "condition": "minecraft:entity_properties",
                "entity": "this",
                "predicate": {
                    "type": "bovinesandbuttercups:cow",
                    "type_specific": {
                        "type": "bovinesandbuttercups:tropical_blue"
                    }
                }
            }
        ],
        "other_conditions": [
            {
                "condition": "minecraft:entity_properties",
                "entity": "this",
                "predicate": {
                    "type": "bovinesandbuttercups:cow",
                    "type_specific": {
                        "type": "bovinesandbuttercups:limelight"
                    }
                }
            }
        ]
    },
    // The nectar type (defined in /data/<namespace>/bovinesandbuttercups/nectar/) obtained when milking this cow with a bowl.
    // This field is optional. If unset, any mooblooms with this type will not be able to be milked with a bowl.
    "nectar": "example:example",
    // Particle options associated with this moobloom.
    // This is only used by breeding in the base mod, but may have other uses in addons/integrations.
    // See: https://minecraft.wiki/w/Particle_format
    // This field is optional.
    "particle": {
        "type": "bovinesandbuttercups:bloom",
        // Int color
        "color": 11337549
    }
}
```

</details>


<details>
<summary>bovinesandbuttercups:mooshroom</summary>

```jsonc
{
    "type": "bovinesandbuttercups:mooshroom",
    // Defines the flower that is associated with the adult mooshroom.
    // This will appear on a mooshroom of this type's back, and what item you obtain when shearing a mooshroom with this type.
    "mushroom": {
        // Accepts one (or more) of:
        // A block state. Accepts a "Name" and "Properties" field.
        "block_state": {
            "Name": "minecraft:crimson_fungus"
        },
        // A bovines model set (defined in /assets/<namespace>/bovinesandbuttercups/). Has to be of the "bovinesandbuttercups:generic" type.
        // If set alone, blocks/items will not be associated with the other functionalities of this field.
        "model_set": "example:generic_model_set",
        // A custom mushroom type to use.
        // You may read the mushroom type section to get more information on this.
        "custom_mushroom": "example:truffle"
    },
    // Whether the mooshroom can eat flowers. Accepts true or false.
    // This field is optional, will fall back to vanilla/other mod behavior if not set.
    "can_eat_flowers": true,
    // The vanilla type that this mooshroom type represents. Used for backwards compatibility.
    // This field is optional, should typically not be set unless you are adding a mooshroom from a mod that extends the vanilla mooshroom type enum.
    "vanilla_type": "red",
    // The location of the entity texture for this type. Mapped to "assets/<namespace>/textures/entity/<path>.png"
    // This field is optional. If not set, will be set to "assets/<file_namespace>/textures/entity/bovinesandbuttercups/mooshroom/<file_path>_mooshroom.png"
    "texture_location": "example:bovinesandbuttercups/mooshroom/example_mooshroom",
    // Textures to render on top of the base mooshroom texture.
    // In the base mod, this is used for the grass/moss/snow layers on each mooshroom.
    // This field is optional.
    // The below will render the grass layer normally outside of biomes that are tagged as #minecraft:is_jungle, and will render emissively when in these biomes.
    "layers": [
        {
            // The location of the entity texture for this type. Mapped to "assets/<namespace>/textures/entity/<path>.png"
            "texture_location": "bovinesandbuttercups:bovinesandbuttercups/mooshroom/mooshroom_mycelium_layer",
            // The texture modifiers for this layer. These will have different effects on the texture when applied.
            // You may read the texture modifier section to get more information on these.
            "texture_modifiers": [
                {
                    "type": "bovinesandbuttercups:fallback"
                }
            ]
        },
        {
            "texture_location": "bovinesandbuttercups:bovinesandbuttercups/mooshroom/mooshroom_mycelium_layer",
            "texture_modifiers": [
                {
                    "type": "bovinesandbuttercups:conditioned",
                    "id": "example:example_mooshroom/conditioned_modifier",
                    "condition": [
                        {
                            "condition": "minecraft:entity_properties",
                            "entity": "this",
                            "predicate": {
                                "location": {
                                    "biomes": "minecraft:crimson_forest"
                                }
                            }
                        }
                    ]
                },
                {
                    "type": "bovinesandbuttercups:emissive"
                }
            ]
        }
    ],
    // The biomes in which this entity will spawn. 
    // This field is optional.
    "natural_spawns": [
        // Accepts any amounts of a mix of either:
        // A biome key or biome tag (prefixed with #), specifying this way will always have a weight of 1.
        "#minecraft:is_nether",
        // A weighted entry.
        {
            "biomes": "minecraft:crimson_forest",
            "weight": 5
        }
    ],
    // The types that this mooshroom may change into when hit by thunder, randomised from the values in this list.
    // This field is optional.
    "thunder_conversion_types": [
        // Accepts any amounts of a mix of either:
        // A cow type key, specifying this way will always have a weight of 1.
        "bovinesandbuttercups:brown_mushroom",
        // A weighted entry.
        {
            "type": "bovinesandbuttercups:red_mushroom",
            "weight": 7
        }
    ],
    // Conditions used for determining if a baby mooshroom should become this type upon being bred.
    // Below is a condition that checks if one parent is a red mooshroom and the other is a brown mooshroom.
    // See: https://minecraft.wiki/w/Predicate
    // This field is optional.
    "offspring_conditions": {
        "this_conditions": [
            {
                "condition": "minecraft:entity_properties",
                "entity": "this",
                "predicate": {
                    "type": "bovinesandbuttercups:cow",
                    "type_specific": {
                        "type": "bovinesandbuttercups:red_mushroom"
                    }
                }
            }
        ],
        "other_conditions": [
            {
                "condition": "minecraft:entity_properties",
                "entity": "this",
                "predicate": {
                    "type": "bovinesandbuttercups:cow",
                    "type_specific": {
                        "type": "bovinesandbuttercups:brown_mushroom"
                    }
                }
            }
        ]
    },
    // Particle options associated with this mooshroom.
    // This is only used by breeding in the base mod, but may have other uses in addons/integrations.
    // See: https://minecraft.wiki/w/Particle_format
    // This field is optional.
    "particle": {
        "type": "bovinesandbuttercups:shroom",
        // Int color
        "color": 11337549
    }
}
```

</details>

## Nectar

Nectars are data that can be used to add new nectar types into the game.

Nectars can be referenced in Moobloom [Cow Types](#cow-types) to obtain them when milking a moobloom.

Nectars are defined in the `/data/<namespace>/bovinesandbuttercups/nectar` directory in your datapacks.

### Data Example

<details>

```jsonc
{
    // A list of effects to apply lockdown for.
    // Accepts an 'id' field and a 'duration' field.
    "effects": [
      {
        "duration": 9600,
        "id": "minecraft:fire_resistance"
      },
      {
        "duration": 2400,
        "id": "minecraft:regeneration"
      }
    ],
    // A bovines model set (defined in /assets/<namespace>/bovinesandbuttercups/). Has to be of the "bovinesandbuttercups:item" type.
    "model_set": "example:example_nectar_bowl"
}
```

</details>

### Model Set Example

<details>

Defined as `/assets/<namespace>/bovinesandbuttercups/<path>.json`

```jsonc
{
    "type": "bovinesandbuttercups:item",
    "item_model": "example:example_nectar_bowl"
}
```

</details>

### Tags

`bovinesandbuttercups:creative_menu_order` - Defines the order in which the nectar will appear in the Food  alongside other nectars. Should never be replaced unless you know what you are doing.

## Flower Crown Material

Flower Crown Materials are data that can be used to add new flower crown crafting ingredients to the game.

The Flower Crown recipe and actual textures are handled through code, no extra implementation other than palette textures are necessary.

Flower Crown Materials are defined in the `/data/<namespace>/bovinesandbuttercups/flower_crown_material` directory in your datapacks.

### Palette Texture Example

Palette textures should be placed in `/assets/<namespace>/textures/bovinesandbuttercups/flower_crown/color-palettes/`

![Base Palette](img/bamboo_leaf_palette.png)

[Original Size](bovines-example-resourcepack/assets/example/textures/bovinesandbuttercups/flower_crown/color_palettes/example_flower_crown_material.png)

### Base Palette
![Base Palette](img/flower_crown_palette.png)

[Original Size](https://github.com/GreenhouseModding/bovines-and-buttercups/blob/1.21/common/src/main/resources/assets/bovinesandbuttercups/textures/bovinesandbuttercups/flower_crown/color_palettes/flower_crown_palette.png)

### Atlas JSON Examples

> [!IMPORTANT]
>
> **Good Practice**
>
> A good practice with paletted permutations is to prefix your permutation keys with your pack's namespace. This is so your datapack will not clash with any other pack that adds the same key.
>
> For example, instead of just `bamboo`, you may want to use the key `bambeautiful_bamboo` instead.

The Paletted Permutations system requires modifications to the target texture atlas JSON to work.

If you have worked with armor trims, this system should be familiar, but if not, this will create textures from all palettes defined within the `permutations` key at `<texture_namespace>:<texture_path>_<palette_key>`.


More information on texture atlases can be found [here](https://minecraft.wiki/w/Atlas).

<details>
<summary>assets/minecraft/atlases/block.json</summary>

```jsonc
{
    "sources": [
        {
            // The type of atlas source, should be kept as paletted permutations.
            "type": "paletted_permutations",
            // Every texture to modify with the below palettes.
            // Should generally be kept to the Bovines textures unless you know what you are doing.
            // Some of the texture's pixels should map to the exact color of the palette key, any pixel that doesn't is ignored.
            "textures": [
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/top_left",
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/top",
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/top_right",
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/center_left",
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/center_right",
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/bottom_left",
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/bottom",
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/bottom_right"
            ],
            // The base palette, this should be kept to the Bovines texture unless you are using textures with a custom palette.
            "palette_key": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/color_palettes/flower_crown_palette",
            // A key value object with the trailing name of the texture as the key and the palette texture as the value.
            // Supports multiple key value pairs.
            "permutations": {
                "example_flower_crown_material": "example:bovinesandbuttercups/flower_crown/color_palettes/example_flower_crown_material"
            }
        }
    ]
}
```

</details>

<details>
<summary>assets/bovinesandbuttercups/atlases/flower_crown.json</summary>

```jsonc
{
    "sources": [
        {
            // The type of atlas source, should be kept as paletted permutations.
            "type": "paletted_permutations",
            // Every texture to modify with the below palettes.
            // Should generally be kept to the Bovines textures unless you know what you are doing.
            // Some of the texture's pixels should map to the exact color of the palette key, any pixel that doesn't is ignored.
            "textures": [
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/top_left",
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/top",
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/top_right",
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/center_left",
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/center_right",
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/bottom_left",
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/bottom",
                "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/bottom_right"
            ],
            // The base palette, this should be kept to the Bovines texture unless you are using textures with a custom palette.
            "palette_key": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/color_palettes/flower_crown_palette",
            // A key value object with the trailing name of the texture as the key and the palette texture as the value.
            // Supports multiple key value pairs.
            "permutations": {
                "example_flower_crown_material": "example:bovinesandbuttercups/flower_crown/color_palettes/example_flower_crown_material"
            }
        }
    ]
}
```

</details>

### Datapack Example

<details>

```jsonc
{
    // The description text to use in the item tooltip.
    // Will be displayed as <description> x <number>.
    "description": {
        "color": "#557F12",
        "translate": "flower_crown_material.example.example"
    },
    // The ingredient to use in the flower crown recipe to get this material while crafting a flower crown.
    "ingredient": {
        "id": "minecraft:bamboo"
    },
    // The textures to use whilst this flower crown is equipped on an entity.
    // Key names are based on where the item was placed in the crafting table.
    // If you are using the default paletted permutation system, this should be `bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/<key_name>`.
    "equipped_textures": {
        "bottom": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/bottom_example_flower_crown_material",
        "bottom_left": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/bottom_left_example_flower_crown_material",
        "bottom_right": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/bottom_right_example_flower_crown_material",
        "center_left": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/center_left_example_flower_crown_material",
        "center_right": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/center_right_example_flower_crown_material",
        "top": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/top_example_flower_crown_material",
        "top_left": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/top_left_example_flower_crown_material",
        "top_right": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/models/top_right_example_flower_crown_material"
    },
    // The textures to use for this flower crown as an item..
    // Key names are based on where the item was placed in the crafting table.
    // If you are using the default paletted permutation system, this should be `bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/<key_name>`.
    // Bottom Left, Bottom and Bottom Right are unused in the base mod, but are there for the sake of resource packs that modify the flower crown item textures.
    "item_textures": {
        "bottom": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/bottom_example_flower_crown_material",
        "bottom_left": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/bottom_left_example_flower_crown_material",
        "bottom_right": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/bottom_right_example_flower_crown_material",
        "center_left": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/center_left_example_flower_crown_material",
        "center_right": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/center_right_example_flower_crown_material",
        "top": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/top_example_flower_crown_material",
        "top_left": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/top_left_example_flower_crown_material",
        "top_right": "bovinesandbuttercups:bovinesandbuttercups/flower_crown/items/top_right_example_flower_crown_material"
    }
}
```

</details>

### Tags

`bovinesandbuttercups:creative_menu_order` - Defines the order in which a flower crown made of 8 of these materials will appear in the creative menu alongside other flower crowns. Should never be replaced unless you know what you are doing.

## Custom Flower Type

Custom Flower Types are data that can be used to add new custom flower types to the game.

These operate off of the `bovinesandbuttercups:custom_flower` item, block and block entity, as well as the `bovinesandbuttercups:potted_custom_flower` block and block entity if allowed to.

### Data Example

<details>

```jsonc
{
    // Whether this flower has a potted variant and can be potted. Accepts true or false.
    // Requires an extra model set that uses the `bovinesandbuttercups:potted_flower` type when true.
    "has_potted": true,
    // The effects for a valid mooshroom to give suspicious stew of when feeding this custom flower in item form to a mooshroom.
    "stew_effects": [
        {
            "id": "minecraft:fire_resistance",
            "duration": 80
        },
        {
            "id": "minecraft:water_breathing",
            "duration": 160
        }
    ]
}
```

</details>

### Flower Model Set Example

<details>

```jsonc
{
    "type": "bovinesandbuttercups:flower",
    // Is the same format as block state JSON.
    // https://minecraft.wiki/w/Tutorial:Models#Block_states
    "variants": {
        "": {
            "model": "example:block/example_flower"
        }
    },
    "item_model": "example:example_flower"
}
```

</details>

### Potted Flower Model Set Example

<details>

```jsonc
{
    "type": "bovinesandbuttercups:potted_flower",
    // Is the same format as block state JSON.
    // https://minecraft.wiki/w/Tutorial:Models#Block_states
    "variants": {
        "": {
            "model": "example:block/example_flower"
        }
    },
    "item_model": "example:example_flower"
}
```

</details>

## Custom Mushroom Type

Custom Mushroom Types are data that can be used to add new custom mushroom types to the game.

These operate off of the `bovinesandbuttercups:custom_mushroom` item, block and block entity, as well as the `bovinesandbuttercups:potted_custom_mushroom` and `bovinesandbuttercups:custom_mushroom_block` blocks and block entities if individually allowed to.

### Data Example

<details>
<summary>Custom Mushroom JSON</summary>

```jsonc
{
    // Whether this mushroom has a full block variant. Accepts true or false.
    // Requires an extra model set that uses the `bovinesandbuttercups:mushroom_block` type when true.
    "has_potted": true,
    // Whether this mushroom has a potted variant and can be potted. Accepts true or false.
    // Requires an extra model set that uses the `bovinesandbuttercups:potted_mushroom` type when true.
    "has_potted": true,
    // A template pool (defined in data/<namespace>/worldgen/template_pool/<path>.json) to set blocks to after using bone meal on your mushroom when it's on Mycelium.
    // https://minecraft.wiki/w/Template_pool
    "huge_mushroom_template_pool": "example:example_mushroom",
    // Whether the huge mushroom template pool should be randomly rotated. Typically used for when mushroom template pools are not symmetrical.
    "randomly_rotate_huge_mushroom": true
}
```

</details>

### Mushroom Model Set Example

<details>
<summary>Mushroom Bovines Model Set</summary>

```jsonc
{
    "type": "bovinesandbuttercups:mushroom",
    // Is the same format as block state JSON.
    // https://minecraft.wiki/w/Tutorial:Models#Block_states
    "variants": {
        "": {
            "model": "example:block/example_flower"
        }
    },
    "item_model": "example:example_flower"
}
```

</details>

### Potted Mushroom Model Set Example

<details>

```jsonc
{
    "type": "bovinesandbuttercups:potted_mushroom",
    // Is the same format as block state JSON.
    // https://minecraft.wiki/w/Tutorial:Models#Block_states
    "variants": {
        "": {
            "model": "example:block/example_flower"
        }
    },
    "item_model": "example:example_flower"
}
```

</details>

### Mushroom Block Model Set Example

<details>

```jsonc
{
    "type": "bovinesandbuttercups:mushroom_block",
    // Is the same format as block state JSON.
    // https://minecraft.wiki/w/Tutorial:Models#Block_states
    // This multipart is set up in like the vanilla mushroom blocks, with the unique texture when a face is true, and the inside texture when a face is false.
    "multipart": [
        {
            "apply": {
                "model": "example:block/example_mushroom_block"
            },
            "when": {
                "north": "true"
            }
        },
        {
        "apply": {
            "model": "example:block/example_mushroom_block",
            "uvlock": true,
            "y": 90
        },
        "when": {
            "east": "true"
        }
        },
        {
        "apply": {
            "model": "example:block/example_mushroom_block",
            "uvlock": true,
            "y": 180
        },
        "when": {
            "south": "true"
        }
        },
        {
        "apply": {
            "model": "example:block/example_mushroom_block",
            "uvlock": true,
            "y": 270
        },
        "when": {
            "west": "true"
        }
        },
        {
        "apply": {
            "model": "example:block/example_mushroom_block",
            "uvlock": true,
            "x": 270
        },
        "when": {
            "up": "true"
        }
        },
        {
        "apply": {
            "model": "example:block/example_mushroom_block",
            "uvlock": true,
            "x": 90
        },
        "when": {
            "down": "true"
        }
        },
        {
        "apply": {
            "model": "minecraft:block/mushroom_block_inside"
        },
        "when": {
            "north": "false"
        }
        },
        {
        "apply": {
            "model": "minecraft:block/mushroom_block_inside",
            "uvlock": false,
            "y": 90
        },
        "when": {
            "east": "false"
        }
        },
        {
            "apply": {
                "model": "minecraft:block/mushroom_block_inside",
                "uvlock": false,
                "y": 180
            },
            "when": {
                "south": "false"
            }
        },
        {
            "apply": {
                "model": "minecraft:block/mushroom_block_inside",
                "uvlock": false,
                "y": 270
            },
            "when": {
                "west": "false"
            }
        },
        {
            "apply": {
                "model": "minecraft:block/mushroom_block_inside",
                "uvlock": false,
                "x": 270
            },
            "when": {
                "up": "false"
            }
        },
        {
            "apply": {
                "model": "minecraft:block/mushroom_block_inside",
                "uvlock": false,
                "x": 90
            },
            "when": {
                "down": "false"
            }
        }
    ],
    "item_model": "example:example_mushroom_block"
}
```

</details>

## Cupcakes

- TODO

## Usage in Recipes

### Componentized Items

Componentized items may be used in recipes. In order to use your nectar, custom flower and/or custom mushrooms in recipes as an ingredient, you must use your loader's `components` ingredient type.

<details>
<summary>Nectar Ingredient Example</summary>

If you want to only support a specific loader, you may remove the content between the start and end comments.

```jsonc
{
    // Fabric start.
    "fabric:type": "fabric:components",
    "base": {
        "item": "bovinesandbuttercups:nectar_bowl"
    },
    // Fabric end.
    
    // NeoForge start.
    "type": "neoforge:components",
    "items": [
        "bovinesandbuttercups:nectar_bowl"
    ],
    // NeoForge end.

    // The components to check for within this ingredient.
    // This field is the same on both loaders.
    "components": {
        "bovinesandbuttercups:nectar": "example:example_nectar"
    }
}
```

</details>

<details>
<summary>Custom Flower Ingredient Example</summary>

If you want to only support a specific loader, you may remove the content between the start and end comments.

```jsonc
{
    // Fabric start.
    "fabric:type": "fabric:components",
    "base": {
        "item": "bovinesandbuttercups:custom_flower"
    },
    // Fabric end.
    
    // NeoForge start.
    "type": "neoforge:components",
    "items": [
        "bovinesandbuttercups:custom_flower"
    ],
    // NeoForge end.

    // The components to check for within this ingredient.
    // This field is the same on both loaders.
    "components": {
        "bovinesandbuttercups:custom_flower": "example:example_flower"
    }
}
```

</details>

<details>
<summary>Custom Mushroom Ingredient Example</summary>

If you want to only support a specific loader, you may remove the content between the start and end comments.

```jsonc
{
    // Fabric start.
    "fabric:type": "fabric:components",
    "base": {
        "item": "bovinesandbuttercups:custom_mushroom"
    },
    // Fabric end.
    
    // NeoForge start.
    "type": "neoforge:components",
    "items": [
        "bovinesandbuttercups:custom_mushroom"
    ],
    // NeoForge end.

    // The components to check for within this ingredient.
    // This field is the same on both loaders.
    "components": {
        "bovinesandbuttercups:custom_mushroom": "example:example_mushroom"
    }
}
```

</details>

Examples of usage of custom ingredients can be found at the recipes within the [GreenhouseModding/bovines-and-buttercups-example-pack GitHub Repository](https://github.com/GreenhouseModding/bovines-and-buttercups-example-pack). [Light Blue Dye from Example Flower](https://github.com/GreenhouseModding/bovines-and-buttercups-example-pack/blob/1.21.1/bovines-example-datapack/data/example/recipe/light_blue_dye_from_example_flower.json), [Suspicious Stew from Example Flower](https://github.com/GreenhouseModding/bovines-and-buttercups-example-pack/blob/1.21.1/bovines-example-datapack/data/example/recipe/suspicious_stew_from_example_flower.json).

### Remainder Ingredient Type

Bovines and Buttercups adds a `bovinesandbuttercups:remainder` ingredient type. Which will return the player with a specified item after the crafting has been processed within the slot that the item was in.

<details>

```jsonc
{
    "fabric:type": "bovinesandbuttercups:remainder",
    "type": "bovinesandbuttercups:remainder",
    // The base ingredient to use in the crafting.
    // This accepts any ingredient, including custom ingredient types.
    "base": {
        "item": "minecraft:suspicious_stew"
    },
    // The remainder item to return from this ingredient as soon as the crafting has been process.
    "remainder": {
        "id": "minecraft:bowl"
    }
}
```

</details>


### Crafting Special Suspicious Edible

To craft edible types based on suspicious stew effects found within the suspicious stew within the recipe, you may use the `bovinesandbuttercups:crafting_special_suspicious_edible` recipe type.

<details>

```jsonc
{
    "type": "bovinesandbuttercups:crafting_special_suspicious_edible",
    "group": "",
    "pattern": {
        "key": {
            // The suspicious stew part of the recipe.
            // With the type specified as `bovinesandbuttercups:crafting_special_suspicious_edible`.
            "S": {
                "fabric:type": "bovinesandbuttercups:remainder",
                "type": "bovinesandbuttercups:remainder",
                "base": {
                    "item": "minecraft:suspicious_stew"
                },
                "remainder": {
                    "id": "minecraft:bowl"
                }
            },
            "W": {
                "item": "minecraft:wheat"
            }
        },
        "pattern": [
            "WSW"
        ]
    },
    "result": "example:example_suspicious_edible"
}
```

</details>
