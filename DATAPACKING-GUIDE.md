# Bovines and Buttercups 2.0.0 Datapacking Guide

## Table of Contents

- [Cow Types](#cow-types)
- [Nectar](#nectar)
- [Flower Crown Material](#flower-crown-material)

## Cow Types

Cow Types are data that can be used to add new mooblooms or mooshrooms into the game.

The data has different fields depending on the `"type"` field. With just Bovines and Buttercups, this can be either `"bovinesandbuttercups:moobloom"`, or `"bovinesandbuttercups:mooshroom"`.

Cow Types are defined in the `/data/<namespace>/bovinesandbuttercups/cow_type` directory in your datapacks.

### Examples

<details>
<summary>bovinesandbuttercups:moobloom</summary>

```json
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

```json
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
    // Whether the mooshroom can eat flowers.
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

```json
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

```json
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

[Original Size](https://raw.githubusercontent.com/GreenhouseModding/bovines-and-buttercups/6e4a616310459a30eb739632fed4c10dc2405405/common/src/main/resources/assets/bovinesandbuttercups/textures/bovinesandbuttercups/flower_crown/color_palettes/flower_crown_palette.png)

### Atlas JSON Examples

The Paletted Permutations system requires modifications to the target texture atlas JSON to work.

If you have worked with armor trims, this system should be familiar, but if not, this will create textures from all palettes defined within the `permutations` key at `<texture_namespace>:<texture_path>_<palette_key>`.

> [!IMPORTANT]
>
> ## Good Practice
>
> A good practice with paletted permutations is to prefix your permutation keys with your pack's namespace. This is so your datapack will not clash with any other pack that adds the same key.
>
> For example, instead of just `bamboo`, you may want to use the key `bambeautiful_bamboo` instead.


More information on texture atlases can be found [here](https://minecraft.wiki/w/Atlas).

<details>
<summary>assets/minecraft/atlases/block.json</summary>

```json
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

```json
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

```json
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
    // If you are using the paletted permutation system, this should be `<namespace>:bovinesandbuttercups/flower_crown/models/<key_name>_<path>`.
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
    // If you are using the paletted permutation system, this should be `<namespace>:bovinesandbuttercups/flower_crown/items/<key_name>_<path>`.
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
