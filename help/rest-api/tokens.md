---
title: Tokens
feature: REST API, Tokens
description: Marketo My Tokens beheren met Asset REST API. Zie ondersteunde gegevenstypen, get by folder or program, create or update via form-encoded POST, and delete by name.
exl-id: 4f8d87d7-ba2a-4c90-8b39-4d20679d404a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 0%

---

# Tokens

[ Symbolische Verwijzing van het Eindpunt ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens)

Tokens in Marketo zijn speciale tekenreeksen, vergelijkbaar met snelcodes, die tijdens de uitvoering worden vervangen door een afzonderlijk stukje gegevens. Er zijn verschillende typen tokens beschikbaar in Marketo, maar alleen Mijn tokens kunnen worden bewerkt via de API. Mijn tokens zijn onderliggende tokens die lokaal zijn voor een bepaalde map of programma. Tokens kunnen worden gelezen, gemaakt en verwijderd via de API.

## Gegevenstype

Tokens kunnen met de volgende gegevenstypen worden gecreeerd:

| Type | Beschrijving |
|---------------|----------------------------------------------------|
| date | Datum en waarde van de notatie &quot;jjjj-MM-dd&quot; |
| getal | Een geheel getal of een drijvende-kommagetal |
| rijke tekst | Een HTML-tekenreeks |
| score | Een 32-bits geheel getal met teken |
| sfdc-campagne | Wordt gebruikt in Salesforce-integratie voor campagnemanagement |
| text | Een tekstreeks |

Dit zijn de enige gegevenstypen die kunnen worden gebruikt bij het maken van een token via API.

## Query

[ krijgt Tokens door Identiteitskaart van de Omslag ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/getTokensByFolderIdUsingGET) neemt `id` als wegparameter van of een programma of het type van Omslag. Dit type wordt opgegeven door de parameter `folderType` .

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

[ creeer Symbolische ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/addTokenTOFolderUsingPOST) eindpunt leidt tot tekenen, of als zij bestaan werk hen met voorgelegde waarden bij. Tokens worden gemaakt in de context van een map of programma. De vereiste `id` padparameter is de id van de map waaraan het token wordt gekoppeld. De parameters `name`, `type`, `value` en `folderType` zijn alle vereiste parameters van het token. Gegevens worden doorgegeven als POST x-www-form-urlencoded, niet als JSON. Het veld `name` van het token mag niet langer zijn dan 50 tekens.

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

[ Symbolisch van de Schrapping door Naam ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/deleteTokenByNameUsingPOST) neemt identiteitskaart als wegparameter van of een Programma of het type van Omslag. Dit type wordt opgegeven door de parameter `folderType` . Tokens worden verwijderd op basis van hun bovenliggende map, de map `name` en de token `type` , die allemaal verplicht zijn. Gegevens worden doorgegeven als POST x-www-form-urlencoded, niet als JSON.

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
