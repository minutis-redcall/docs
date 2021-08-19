# Minutis and RedCall resources

The resources described below are the minimal requirements to make Minutis and RedCall work. There
are much more functionalities that you can integrate through our APIs, but this format is a first
step.

## Introduction

### Format

We require, if possible, that you provide a JSON file containing all resources. JSON is more flexible than CSVs because it supports collections (useful to assign a list of skills to a volunteer for example). 

Resources should be provided in the following format:

```json
{
  "badges": [COLLECTION,OF,BADGES],
  "structures": [COLLECTION,OF,STRUCTURES],
  "volunteers": [COLLECTION,OF,VOLUNTEERS],
  "vehicles": [COLLECTION,OF,VEHICLES],
  "radios": [COLLECTION,OF,RADIOS],
  "relays": [COLLECTION,OF,RELAYS]
}
```

If you cannot provide a JSON file, let us know.

### Identification

All resources are identified using an `externalId`: this identifier should be unique for every resource, and should help you synchronize your own datasource with Minutis. 

For example, in the French Red Cross database, `10646X` is Gerard's identifier, thus his volunteer's `externalId` is also `10646X` on Minutis applications.

`externalId` can be any string having a maximum length of **64** bytes.

## Badges

A badge is a skill, a nomination, a training certificate, or anything that helps you filter out which people you need in a given situation. 

```json
{
  "externalId":"UNIQUE_ID",
  "shortcut":"SKILL_SHORTCUT",
  "description":"SKILL_FULL_NAME"
}
```

- `shortcut` (max length: **64** bytes) is an acronym rendered on pages where there are not much space (such as forms or below a volunteer's name), like "VL" for "Light Vehicle" in France.

- `description` (max length: **255** bytes) is the long name of the badge, it is rendered on large pages, or if the mouse go over a shortcut.

## Structures

Structures are independant Red Cross sections that manage volunteers and organize operations. In Minutis, they are also used to define geographic zones in the main page.

```json
{
  "externalId":"STRUCTURE_UNIQUE_ID",
  "parentExternalId":"STRUCTURE_EXTERNAL_ID_OF_PARENT_STRUCTURE",
  "name":"NAME_OF_THE_STRUCTURE",
  "type":"zone_geo or leaf",
  "geoLongitude":"LONGITUDE_OF_THE_CENTER_OF_THE_STRUCTURE",
  "geoLatitude":"LATITUDE_OF_THE_CENTER_OF_THE_STRUCTURE"

```

- `externalId`: just a side note here, on Minutis API endpoints returning structures, your structure `externalId` will be prefixed with `red_cross_spain_` (for example if you set *1234* as `externalId`, the API will expose *red\_cross\_spain_1234*).

- `parentExternalId`: every structures may have a parent if the parent structure covers or leads them. For example, "Paris" leads all Paris districts which have their own structures (ex: structure of "Paris VII"). "Ile-de-France" is "Paris"'s parent and covers the whole region.

- `name` (max length: **64** bytes) is the structure name.

- `type` can be either:

 -  `leaf` (if it is a structure that can have operations)
 
 -  `zone_geo` () For each structure of type zone_geo, please also provide a javascript file to describe the map area. Example can be found here: https://github.com/iblancasa/Spain-Provinces-Javascript-Map/blob/master/map.js
 -   **to be continued, this part seems super broad, and I don't even understand why these "zone geo" are the same resource as physical structures?)**

- `geoLongitude` and `geoLatitude` are longitude and latitude of the center of the structure's  location. It is only required for structures of type `leaf`.










