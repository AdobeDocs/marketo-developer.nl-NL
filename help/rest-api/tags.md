---
title: Tags
feature: REST API, Tags
description: Type query-tag, toegestane waarden ophalen op naam, programmatags bijwerken of verwijderen in Marketo via REST Asset API, met aanvraagvoorbeelden.
exl-id: 64731d1a-a749-4d6f-b336-16c733d002f0
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 1%

---

# Tags

[&#x200B; Verwijzing van het Eindpunt van Markeringen &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags)

Tags zijn door de gebruiker gedefinieerde velden voor programma&#39;s. Elke tag kan van toepassing zijn op een of meer programmatypen en kan verplicht of optioneel zijn, afhankelijk van de manier waarop de tag is gedefinieerd. Tags kunnen ook een lijst bevatten met toegestane waarden die moeten worden geselecteerd voor gebruik.

## Query

De markeringen worden gevraagd met het standaardelementpatroon, maar hebben geen eindpunt voor door Id. De lijst met toegestane waarden voor een tag wordt alleen geretourneerd wanneer de tag op naam wordt opgevraagd.

### Tags ophalen

```
GET /rest/asset/v1/tagTypes.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1488a#1504ecfccf8",
    "result": [
        {
            "tagType": "AAA1 Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": true
        },
        {
            "tagType": "AAA2 Required Event Tag Type",
            "applicableProgramTypes": "[event]",
            "required": true
        },
        {
            "tagType": "AAA3 Not Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": false
        }
    ]
}
```

### Op naam

```
GET /rest/asset/v1/tagType/byName.json?name=AAA1 Required Tag Type
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "8a44#1504ed0da2f",
    "result": [
        {
            "tagType": "AAA1 Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": true,
            "allowableValues": "[AAA1 RT1, AAA1 RT2, AAA1 RT3, AAA1 RT4]"
        }
    ]
}
```

## Bijwerken

Het [&#x200B; eindpunt van de Markering van het Programma van de Update &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) staat u toe om de waarde voor een bepaald markeringstype bij te werken. Het eindpunt neemt `id` en `tagType` wegparameters die programma identiteitskaart, en het markeringstype specificeren om bij te werken. Er wordt een query-parameter `tagValue` gebruikt om de nieuwe waarde voor het tagtype op te geven. Alle parameters zijn vereist.

```
POST /rest/asset/v1/program/{id}/tag/{tagType}.json?tagValue=David
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "fd84#17f84a885a6",
    "warnings": [],
    "result": [
        {
            "id": 1067
        }
    ]
}
```

De markeringen kunnen en massa worden bijgewerkt gebruikend het [&#x200B; Metagegevens van het Programma van de Update &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) eindpunt. Een voorbeeld van dat kan worden gevonden [&#x200B; hier &#x200B;](programs.md#update).

## Verwijderen

Het [&#x200B; eindpunt van de Markering van het Programma van de Schrapping &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/deleteProgramUsingPOST) staat u toe om een niet-vereist markeringstype te schrappen. Het eindpunt neemt `id` - en `tagType` padparameters aan die de programma-id en het te verwijderen tagtype opgeven.

```
POST /rest/asset/v1/program/{id}/tag/{tagType}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d998#17f84ad36a7",
    "warnings": [],
    "result": [
        {
            "id": 1067
        }
    ]
}
```
