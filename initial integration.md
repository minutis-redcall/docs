# Minutis and RedCall resources

The resources described below are the minimal requirements to make Minutis and RedCall work. There are much more functionalities that you can integrate through our APIs, but this format is a first
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

Structures are independant Red Cross sections that manage volunteers and organize operations. 

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

- `type` can be either `leaf` (if it is a structure that can have operations) or `zone_geo` ()

- `geoLongitude` and `geoLatitude` are longitude and latitude of the center of the structure's physical location. It is only required for structures of type `leaf` and is used to render an operation with the map centered by default on the structure's location.

## Volunteers

A volunteer is a physical person belonging to the Red Cross. 

```json
{
  "externalId":"UNIQUE_ID",
  "structureExternalId":"STRUCTURE_EXTERNAL_ID",
  "badgeExternalIds": ["BADGE_1", "BADGE_2", ...],
  "firstname":"FIRSTNAME",
  "lastname":"LASTNAME",
  "mail":"EMAIL",
  "phone":"PHONE_NUMBER_IN_E164_FORMAT",
  "canTriggerStructureExternalIds": [STRUCTURE_EXTERNAL_ID_1, STRUCTURE_EXTERNAL_ID_2...],
  "canUseRedcallAPI": true|false,
  "isRedcallAdmin": true|false
}
```

- `structureExternalId` is the structure volunteer belongs to, in which it can be triggered (in order to join an operation).

- `badgeExternalIds` is the collection of badges (see above) volunteer has

- `firstname` and `lastname` have a maximum length of **80** bytes

- `mail` (max length **80** bytes) is volunteer's main email, in which it will receive RedCall triggers

- `phone` (max length **32** bytes) is the volunteer's phone number in [E164](https://en.wikipedia.org/wiki/E.164) format (eg. French number 06 60 93 61 63 becomes +33660936163)

- `canTriggerStructureExternalIds` is the collection of structures a volunteer is allowed to trigger (if any), note that if one of these structures has children, volunteer will also be able to trigger them

- `canUseRedcallAPI` is a boolean that you can set to `true` if the volunteer is a software developer that will integrate RedCall APIs

- `isRedCallAdmin` is a boolean that you can set to `true` if the volunteer is a person that will manage RedCall resources (useful for support teams, project lead, etc)

## Vehicles

Vehicles are usually ambulances, cars, trucks or any other mobilized vehicles during operations.

```json
{
  "externalId":"LICENSE_PLATE",
  "structureExternalId":"STRUCTURE_EXTERNAL_ID",
  "name":"UNIQUE_NAME",
  "type":"TYPE_OF_THE_VEHICLE"
}
```

- `structureExternalId` is the structure for which the vehicle belongs to

- `name` (max length **XX** bytes) is the vehicle's name

- `type` is the type of vehicle, coming from a static enumeration (we currently have a list in French, please provide the name you want to be rendered. We will develop APIs to have lists in according to the language).

## Radios

To comment

```json
// Radios:
{
  "externalId":"UNIQUE_ID",
  "structureExternalId":"STRUCTURE_EXTERNAL_ID",
  "name":"NAME"
}
```

- `structureExternalId` is the structure for which the radio belongs to

- `name` (max length **XX** bytes) is the radio's name

## Relays

Relays are antennas that cover radio emissions

```
{
  "externalId":"UNIQUE_ID",
  "structureExternalId":"STRUCTURE_EXTERNAL_ID",
  "name":"NAME"
}
```

- `structureExternalId` is the structure for which the relay belongs to

- `name` (max length **XX** bytes) is the relay's name







