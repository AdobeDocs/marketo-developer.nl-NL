---
title: "Dynamische inhoud"
feature: REST API, Dynamic Content
description: "Configureer dynamische inhoud met Marketo API's."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 0%

---


# Dynamische inhoud

Marketo vereenvoudigt het gebruik van dynamische inhoud door loodsegmentatie op meerdere elementtypen:

- E-mails
- Landingspagina&#39;s
- Fragmenten

## Overzicht

Dynamische inhoud wordt geïmplementeerd op sectieniveau door specifieke variaties aan te wijzen van een sectie die aan een lead moet worden aangeboden op basis van hun kwalificatie in een segment binnen een gekozen segmentatie. Als een stuk van inhoud wordt gevormd om dynamische inhoud te dienen die op een bepaalde segmentatie wordt gebaseerd, dan een lood die dat de inhoud ziet wordt gediend de inhoudsvariatie die het segment aanpast dat zij vallen, of de Standaard inhoud, als zij niet voor een segment kwalificeren.

## Voorbeeld

Om te demonstreren, kijk naar een e-mailvoorbeeld, waar wij een Gebied (VS) segmentatie hebben, en een gebeurtenisbevordering slechts voor lood willen tonen die in het zuidoostsegment vallen, dat Californië, Nevada, Utah, Colorado, Arizona, en Nieuw Mexico leidt. Hiervoor maken we een bewerkbare sectie in onze e-mail met de id &quot;Q1-promotie-banner&quot; in een DynamicContent-sectie. Om dit te doen, moeten we de [Sectie E-mailinhoud bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST) eindpunt voor onze e-mail. De `value` parameter wordt gebruikt om Id van de segmentatie te specificeren.

Opmerking: zowel e-mails als bestemmingspagina&#39;s volgen dit patroon. Fragmenten hebben een ander patroon, dat wordt beschreven in de API-documentatie voor fragmenten.

In het volgende voorbeeld wordt de sectie ingesteld op een sectie Dynamische inhoud, gesegmenteerd door segmentatie 1001.

```
POST /rest/asset/v1/email/{id}/content/Q1-promotion-banner.json
```

```
type=DynamicContent&value=1001
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "891b#1729b34b9a5",
  "warnings": [],
  "result": [
    {
      "id": 1909
    }
  ]
}
```

Om inhoud voor individuele segmenten toe te voegen, moeten wij roepen [Sectie Dynamische inhoud e-mail bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailDynamicContentUsingPOST) eindpunt voor de specifieke sectie.

In het volgende voorbeeld wordt de sectie zo ingesteld dat deze onze speciale bannerafbeelding voor leads in het segment Zuidwest weergeeft in plaats van de standaardinstelling. Als wij meer variaties voor meer segmenten wilden tot stand brengen, dan zouden wij dit eindpunt opnieuw voor elk segment en sectie roepen.

```
POST /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json
```

```
segment=Southwest&type=HTML&value=<img src='//www.example.com/SuperSpecialBannerForAmericanSouthwestLeads.jpg'/>
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "891b#1729b34b9a5",
  "warnings": [],
  "result": [
    {
      "id": 1637
    }
  ]
}
```

## Segmentatie

Segmentatie is de kern van dynamische inhoud van Marketo. Een segmentatie is een user-defined lijst van individuele reeksen regels die van boven tot onder tegen het volledige loodgegevensbestand worden geëvalueerd. Een lead mag slechts lid zijn van één segment in elke segmentatie en zal lid zijn van de eerste die zij in elke segmentatie in aanmerking neemt. Als het niet voor een segment kwalificeert, dan zal het een lid van het Standaardsegment zijn, en zal de standaardinhoud voor om het even welk bepaald stuk van dynamische inhoud ontvangen gebruikend die segmentatie.

### Lijst

De segmentaties hebben een lijsteindpunt dat een reactie met een lijst van beschikbare segmentaties terugkeert.

```
GET /rest/asset/v1/segmentation.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "78eb#14e9de95868",
  "result": [
    {
      "id": 1001,
      "name": "My Industry Segmentation",
      "description": "",
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:10Z+0000",
      "url": "https://app-abm.marketo.com/#SG1001A1",
      "folder": {
        "type": "Program",
        "value": 396,
        "folderName": null
      },
      "status": "approved",
      "workspace": "Default"
    },
    {
      "id": 1002,
      "name": "My Country Segmentation",
      "description": "",
      "createdAt": "2015-04-06T18:28:23Z+0000",
      "updatedAt": "2015-04-06T18:37:18Z+0000",
      "url": "https://app-abm.marketo.com/#SG1002A1",
      "folder": {
        "type": "Program",
        "value": 396,
        "folderName": null
      },
      "status": "approved",
      "workspace": "Default"
    }
  ]
}
```

De segmentaties hebben ook een eindpunt dat een reactie met een lijst van segmenten van een oudersegmentatie terugkeert.

```
GET /rest/asset/v1/segmentation/1001/segments.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "2031#14e9df08796",
  "result": [
    {
      "id": 1001,
      "name": "Manufacturing",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1002,
      "name": "Healthcare",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769688A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1003,
      "name": "Financial",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769690A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1004,
      "name": "Technology",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769692A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1005,
      "name": "Default",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769694A1",
      "status": "approved",
      "segmentationId": 1001
    }
  ]
}
```
