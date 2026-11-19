# Registry

The registry lets you declare custom game content.

## Custom Items

A custom item is a vanilla base item with custom data components.

```yaml
# data/my_pack/registry/items.pipe
register:
  - id: "my_namespace:custom_sword"
    name: "&6Blade of the Matrix"
    material: "NETHERITE_SWORD"
    lore:
      - "&7Forged in the simulation"
      - "&7Damage: &c+5"
    enchantments:
      - enchantment: "minecraft:sharpness"
        level: 5
    attributes:
      - attribute: "attack_damage"
        amount: 5
        operation: "ADD_VALUE"
        slot: "MAINHAND"
    nbt:
      unbreakable: true
      custom_model_data: 1001
    recipe: "my_namespace:custom_sword_recipe"
```

A `generic.` prefix on the attribute name is stripped; unknown operations fall back to `ADD_VALUE` and unknown slots to `ANY`.

`recipe` is reserved for a future release and is currently ignored.

Give it to a player:

```yaml
- give_item: "my_namespace:custom_sword"
  to: "{{player.name}}"
  amount: 1
```

## Custom Blocks

Custom blocks are registered with metadata and a vanilla proxy ID. Block metadata (`hardness`, `drop`, sounds) is stored for your handlers to read; Pipeline does not apply it automatically. Use `block_break`/`block_place` event handlers to act on it.

```yaml
# data/my_pack/registry/blocks.pipe
register:
  - id: "my_namespace:matrix_block"
    name: "&aMatrix Block"
    hardness: 5.0
    resistance: 10.0
    light_level: 15
    drop: "my_namespace:matrix_shard"
    place_sound: "minecraft:block.amethyst_block.place"
    break_sound: "minecraft:block.amethyst_block.break"
    visual:
      vanilla_proxy: "minecraft:emerald_block"
```

## Resource Packs

Pipeline can generate a resource pack from registered content. The generated pack is written to `resourcepacks/<pack>/` next to `pipeline.jar`, and only items with `custom_model_data` get generated model overrides. You supply your own `assets/` textures and models to merge in.

## Network Packets

Declare custom packet schemas:

```yaml
# data/my_pack/network/custom_ui.pipe
packets:
  - id: "my_namespace:shop_ui"
    direction: "server_to_client"
    fields:
      - name: "title"
        type: "string"
      - name: "items"
        type: "list<shop_item>"
    custom_types:
      shop_item:
        item_id: "string"
        price: "decimal"
        stock: "integer"
```

Send a packet:

```yaml
- network.send: "my_namespace:shop_ui"
  to: "{{player.name}}"
  fields:
    title: "Shop"
    items: []
```

!!! note
    `network.send` currently logs what it would send. Wire transport to an unmodified vanilla client is deferred because vanilla's custom payload system needs more server patching and vanilla clients can't read arbitrary packets anyway.
