# Monster card

Monster card is a magical type of card. Because it's dynamic if you're smart about it, you can have one card that adapts and that you don't need to touch when adding new entities & sensors to your setup.

Supports both inclusion and exclusion filters with wildcard for entity_ids.

**Options**

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| type | string | **Required** | `custom:monster-card`
| card | object | **Required** | Card object 
| filter | object | **Required** | `include` and `exclude` sections
| show_empty | boolean | true | Show/hide empty card
| when | object | optional | Extra condition for show/hide

Card object

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| type | string | **Required** | A type of card (ex.`glance`) from lovelace
| title | object | optional | Title of the card
| ... | other | optional | Other parameters supported by card type above

Filter options for `include` and `exclude`:

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| domain | string | optional | Filter all entities that match the domain
| state | string | optional | Match entities that match state. Note, in YAML, make sure you wrap it in quotes to make sure it is parsed as a string.
| entity_id | string | optional | Filter entities by id, supports wildcards (`*living_room*`). Case insensitive
| name | string | optional | Filter entities by friendly_name or title, supports wildcards (`*kitchen*`). Case insensitive
| attributes | object | optional | Filter entities by attributes object

Attributes object

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| * | any | **Required** | Any type of attribute that your entity can have (`battery: 50`)


When object

| Name | Type | Default | Description
| ---- | ---- | ------- | -----------
| entity | string | **Required** | Entity to use for state comparison
| state | any | **Required** | The state of the entity to use for showing the monster-card

**Example**

Show all with some exceptions:
```yaml
- type: custom:monster-card
  card:
    type: glance
    title: Monster
  filter:
    include: [{}]
    exclude:
      - entity_id: "*yweather*"
      - domain: group
      - domain: zone
```

Show all in `device_tracker` with battery at 53:

```yaml
- type: custom:monster-card
  card:
    type: glance
    title: Monster
  filter:
    include:
      - domain: device_tracker
        attributes:
          battery: 53
          source_type: gps
```

Snow monster card based on random entity state:
```yaml
- type: custom:monster-card
  card:
    type: glance
  filter:
    include:
      - domain: climate
  when:
    entity: device_tracker.demo_paulus
    state: not_home
```