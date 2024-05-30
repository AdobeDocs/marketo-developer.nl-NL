---
title: "Tokens"
feature: REST API, Tokens
description: "Tkens beheren in Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---


# Tokens

[Token-eindpuntverwijzing](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens)

Tokens in Marketo zijn speciale tekenreeksen, vergelijkbaar met snelcodes, die tijdens de uitvoering worden vervangen door een afzonderlijk stukje gegevens. Er zijn verschillende typen tokens beschikbaar in Marketo, maar alleen Mijn tokens kunnen worden bewerkt via de API. Mijn tokens zijn onderliggende tokens die lokaal zijn voor een bepaalde map of programma. Tokens kunnen worden gelezen, gemaakt en verwijderd via de API.

## Gegevenstype

Tokens kunnen met de volgende gegevenstypen worden gecreeerd:

| Type | Beschrijving |
|---------------|----------------------------------------------------|
| date | Datum en waarde van de notatie &quot;jjjj-MM-dd&quot; |
| getal | Een geheel getal of een drijvende-kommagetal |
| rijke tekst | Een HTML-tekenreeks |
| score | Een 32-bits geheel getal met teken |
| sfdc-campagne | Gebruikt in de integratie van Salesforce-campagnebeheer |
| text | Een tekstreeks |


Dit zijn de enige gegevenstypen die kunnen worden gebruikt bij het maken van een token via API.

## Query

[Tokens ophalen op map-id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/getTokensByFolderIdUsingGET) neemt een `id` als padparameter van het type Program of Folder. Dit type wordt opgegeven door de `folderType` parameter.

```curl
GET /rest/asset/v1/folder/{id}/tokens.json?folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "4fbe#14e27fc9bbf",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "AprilFool - deverly",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

## Maken en bijwerken

De [Token maken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/addTokenTOFolderUsingPOST) het eindpunt leidt tot tekenen, of als zij bestaan werkt hen met voorgelegde waarden bij. Tokens worden gemaakt in de context van een map of programma. De vereiste `id` padparameter is de id van de map waaraan het token wordt gekoppeld. De `name`, `type`, `value`, en `folderType` zijn alle vereiste parameters van het token. Gegevens worden doorgegeven als POST x-www-form-urlencoded, niet als JSON. De `name` het token mag niet meer dan 50 tekens bevatten.

```
POST /rest/asset/v1/folder/{id}/tokens.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=April Fools&type=date&value=2015-04-01&folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "e3c2#14e280db5dc",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "April Fools",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

## Verwijderen

[Token op naam verwijderen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/deleteTokenByNameUsingPOST) neemt identiteitskaart als wegparameter van of een Programma of het type van Omslag. Dit type wordt opgegeven door de `folderType` parameter. Tokens worden verwijderd op basis van hun bovenliggende map, de `name`en de `type` van de token, die elk vereist is. Gegevens worden doorgegeven als POST x-www-form-urlencoded, niet als JSON.

```
POST /rest/asset/v1/folder/{id}/tokens/delete.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=AprilFool - deverly&type=date&folderType=Program
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "12ed2#14e2800f89c",
    "result": [
        {
            "id": 416
        }
    ]
}
```
