# ZoeLog JSON file documentation<br>v3.0 (300) pre-release

{{TOC}}

## Introduction

**ZoeLog 3.0 features the ability to export a JSON file alongside its standard file formats (PDF, CSV, plain text) to enable seamless integration with third-party video processing applications.** This file provides information entered by users on set during filming, offering developers a powerful tool to enrich their editing workflows.

**Key features**
- Comprehensive metadata from each report, entry, take, and clip.
- ISO timestamps and a reference timecode (if recorded) for precise association with video footage.
- JSON format for easy parsing and integration.

**This documentation details the structure and contents of the ZoeLog JSON file.**

*Note: While it is not anticipated that the format will change, it is worth noting that development of ZoeLog 3.0 is not complete. There is not a release date, and a beta version is not ready to be available at this time.*

## Example

The following is an example of a file containing one report with one entry. Users have the option to export the file “pretty printed” as below, or as condensed json.

```
{
    "production_title": "Space Movie",
    "production_metadata": {
        "director": "Some Director",
        "production": "Some Production Company",
        "A Custom Variable Field": "Some data",
        "camera": "Some DOP",
        "contact2": "Some phone number",
        "contact1": "Some address"
    },
    "reports": [
        {
            "roll": "A34",
            "camera": "A",
            "shooting_date": "2024-01-03",
            "entries": [
                {
                    "slate": null,
                    "scene": "X3",
                    "episode": null,
                    "timecode": "13:50:53:18",
                    "origin_date": "2024-01-03T12:49:33.580-08:00",
                    "report_metadata": {
                        "director": "Some Director",
                        "A Custom Variable Field": "Some data",
                        "camera": "Some DOP",
                        "signed": "Roxy"
                    },
                    "log_data": {
                        "Lens": "14mm",
                        "Lens Type": "Cooke S4i",
                        "Color Temp": "5600K",
                        "Description": "Low angle wide establishing. The captain and crew appear.",
                        "FPS": "23.976fps",
                        "Filters": "POLA, IRND2.1",
                        "Focus": "16’",
                        "ISO": "800EI",
                        "Lens Height": "8”",
                        "LUT": "EXT_Day",
                        "Shutter": "180∢",
                        "Stop": "T2.8½",
                        "Tilt": "4°up"
                    },
                    "takes": [
                        {
                            "name": "1",
                            "circled": false,
                            "clip": 1,
                            "origin": "2024-01-03T12:56:49.178-08:00"
                        },
                        {
                            "name": "2",
                            "circled": true,
                            "clip": 2,
                            "origin": "2024-01-03T12:58:50.180-08:00"
                        },
                        {
                            "name": "3",
                            "circled": true,
                            "clip": 3,
                            "origin": "2024-01-03T13:01:51.037-08:00"
                        },
                        {
                            "name": "4 SER",
                            "circled": true,
                            "clip": 4,
                            "origin": "2024-01-03T13:06:52.009-08:00"
                        },
                        {
                            "name": "4 SER",
                            "circled": true,
                            "clip": 5,
                            "origin": "2024-01-03T13:06:52.009-08:00"
                        },
                        {
                            "name": "4 SER",
                            "circled": true,
                            "clip": 6,
                            "origin": "2024-01-03T13:06:52.009-08:00"
                        }
                    ]
                }
            ]
        }
    ]
}

```

## Production

Contains information about the production itself, including the title and any default metadata such as the director. It is the root, containing the report objects.

| key | value |
|:--|:--|
| `production_title`<br>required | string |
| `production_metadata`<br>required | object<br>`ReportMetadata` |
| `reports`<br>required | Array of objects<br>`Report`

```
{
	"production_title": "Space Movie",
	"production_metadata": {...},
	"reports": [...]
}
```

## ReportMetadata

Both the Production and Entry object will have report metadata which includes the director, camera person, and custom fields.

| key | value |
|:--|:--|
| `director` | string |
| `production` | string<br>*production company* |
| `camera` | string<br>*director of photography* |
| `contact1` | string<br>*contact information.<br>*e.g.: an address. |
| `contact2` | string<br>*contact information.<br>*e.g.: a phone number or email address |
| custom field<br>unique, user generated | string |

```
{
	"director": "Some Director",
	"production": "Some Production Company",
	"A Custom Variable Field": "Some data",
	"camera": "Some DOP",
	"contact2": "Some phone number",
	"contact1": "Some address"
}
```

## Report

Contains information that would be at the heading of a traditional camera report, and all associated entries.

| key | value |
|:--|:--|
| `roll`<br>required | string |
| `camera`<br>required | string<br>*camera index*<br>e.g.: `A` or `B` |
| `shooting_date`<br>required | string <br>*extended format iso date*<br>`YYYY-MM-DD` |
| `entries`<br>required | Array of objects<br>`Entry` |

```
{
	"roll": "A34",
	"camera": "A",
	"shooting_date": "2024-01-03",
	"entries": [...]
}
```

## Entry

The heart of ZoeLog’s data. Contains information about shot identification, camera setup, and takes.

| key | value |
|:--|:--|
| `slate` | string or null<br>*european-style slating*<br>e.g.: `#22` |
| `scene` | string or null<br>e.g.: `13A` |
| `episode` | string or null<br>*for television*<br>e.g.: `ep#304` |
| `timecode` | string or null<br>*user generated timecode for reference use<br>may be multiple values separated by commas*<br>`HH:MM:SS:FF` (hour, minute, second, frame)<br>e.g.: `13:50:53:18, 13:53:08:23` |
| `origin_date`<br>required | string<br>*ISO 8601 date-time with fractional seconds*<br>e.g: `2024-01-03T13:06:52.009-08:00` |
| `report_metadata`<br>required | object<br>`ReportMetadata` |
| `log_data`<br>required | object<br>`LogData` |
| `takes`<br>required | Array of objects<br>`Take` |

```
{
	"slate": null,
	"scene": "X3",
	"episode": null,
	"timecode": "13:50:53:18",
	"origin_date": "2024-01-03T12:49:33.580-08:00",
	"report_metadata": {...},
	"log_data": {...},
	"takes": [...]
}
```

## LogData

Traditional log book data. Keys and values are user-facing. All values are strings. No keys are required. New keys are added occasionally, but there are currently no custom keys.

| key | value |
|:--|:--|
| `Lens` | string<br>*focal length*<br>e.g.: `35mm` |
| `Lens Type` | string<br>e.g.: `Super Speed` |
| `Filters` | string |
| `Stop` | string<br>*T or F stop*<br>e.g.: `T2.8` |
| ` Focus ` | string<br>*focus distance or distance to subject*<br>e.g.: `16’ - 18’3”` |
| ` Lens Height ` | string<br>e.g.: `55”` |
| ` Color Temp ` | string<br>e.g.: `3200K` |
| ` FPS ` | string<br>*frames per second, or frame rate*<br>e.g.: `23.976fps` |
| ` Shutter ` | string<br>*shutter angle*<br>e.g.: `180∢` |
| ` ISO ` | string<br>*exposure index, or ASA*<br>e.g.: `800EI` |
| ` Description ` | string<br>*plain language description of the shot.* |
| ` Notes ` | string<br>*additional information as provided by the user* |
| ` Tilt ` | string<br>*the angle of the camera in degrees*<br>e.g.: `67.8° up - 36.3° up` |
| ` Film Stock ` | string<br>e.g.: `5219` |
| ` LUT ` | string<br>*lookup table*<br>e.g.: `INT_NIGHT_CROSS_PROCESS` |
| ` Aspect Ratio ` | string<br>e.g.: `16x9` or `2.35:1` |
| ` Format ` | string<br>e.g.: `70mm`, `2-perf 35`, or `Raw` |
| ` Resolution ` | string<br>e.g.: `8K`, or `4096x2160` |

```
{
    "Lens": "14mm",
    "Lens Type": "Cooke S4i",
    "Color Temp": "5600K",
    "Description": "Low angle wide establishing. The captain and crew appear.",
    "FPS": "23.976fps",
    "Filters": "POLA, IRND2.1",
    "Focus": "16’",
    "ISO": "800EI",
    "Lens Height": "8”",
    "LUT": "EXT_Day",
    "Shutter": "180∢",
    "Stop": "T2.8½",
    "Tilt": "4°up"
}
```

## Take

Individual take data.

| key | value |
|:--|:--|
| `name`<br>required | string<br>*may not be unique if there are multiple clips* |
| `circled`<br>required | boolean<br>*the script supervisor circles ”good” takes per the director’s instruction* |
| `clip` | number or null<br>*clip number on the card as noted by the user* |
| `origin` | string or null<br>*ISO 8601 date-time with fractional seconds*<br>NOTE: origins in older take data may be null<br>e.g: `2024-01-03T13:06:52.009-08:00` |

```
{
    "name": "3 PU",
    "circled": true,
    "clip": 11,
    "origin": "2024-01-03T14:53:10.597-08:00"
}
```
