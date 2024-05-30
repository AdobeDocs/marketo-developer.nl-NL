---
title: "Tags"
feature: REST API, Tags
description: "Tags voor programma's in Marketo beheren."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---


# Tags

[Referentie eindpunt van tags](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags)

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

De [Programmatag bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) Hiermee kunt u de waarde voor een bepaald tagtype bijwerken. Het eindpunt neemt `id` en `tagType` padparameters die de programma-id en het tagtype dat moet worden bijgewerkt, opgeven. A `tagValue` de vraagparameter wordt gebruikt om de nieuwe waarde voor het markeringstype te specificeren. Alle parameters zijn vereist.

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

Labels kunnen met de massa en de massa worden bijgewerkt [Programmametagegevens bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) eindpunt. Een voorbeeld hiervan is te vinden [hier](programs.md#update).

## Verwijderen

De [Programmatag verwijderen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/deleteProgramUsingPOST) Hiermee kunt u een niet-vereist labeltype verwijderen. Het eindpunt neemt `id` en `tagType` padparameters die de programma-id opgeven en het type code dat moet worden verwijderd.

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
