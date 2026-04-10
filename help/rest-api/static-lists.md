---
title: Statische lijsten
feature: REST API, Static Lists
description: Gebruik Marketo REST API's om statische lijsten te zoeken, te maken, bij te werken en te verwijderen, met eindpunten voor ID, naam en bladeren, mapbereikregels, paginering en datumfilters.
exl-id: 20679fd2-fae2-473e-84bc-cb4fdf2f5151
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---

# Statische lijsten

[Verwijzing naar eindpunt statische lijsten](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists)

Marketo biedt een set REST API&#39;s voor het uitvoeren van CRUD-bewerkingen op statische lijsten. Deze API&#39;s volgen het standaard interfacepatroon voor de bron-API&#39;s die de opties Query, Maken, Bijwerken en Verwijderen bieden.

Voor de verrichtingen van het Gegevensbestand van het Lood op lijstleden, zie [&#x200B; Lidmaatschap van de Lijst &#x200B;](list-membership.md).

## Query

Het vragen van statische lijsten volgt de standaardvraagtypes voor activa van [&#x200B; door identiteitskaart &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET), [&#x200B; door naam &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET), en [&#x200B; doorbladert &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET).

### Op id

[&#x200B; Vraag door identiteitskaart &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) neemt één enkele statische lijst `id` als wegparameter en keert één enkel statisch lijstverslag terug.

```http
GET /rest/asset/v1/staticList/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "843c#1641f969e96",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        }
    ]
}
```

#### Op naam

[&#x200B; Vraag door naam &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) neemt een statische lijst `name` als parameter en keert één enkel statisch lijstverslag terug. Een nauwkeurige koordgelijke wordt uitgevoerd tegen alle statische lijstnamen in de instantie, en keert een resultaat voor de statische lijst terug die die naam aanpassen.

```http
GET /rest/asset/v1/staticList/byName.json?name=Foundation Seed List
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "28ab#1641fa246b9",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        }
    ]
}
```

#### Bladeren

De statische lijsten kunnen ook [&#x200B; worden teruggewonnen in partijen &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET). De parameter `folder` kan worden gebruikt om de bovenliggende map op te geven waaronder de query wordt uitgevoerd en wordt opgemaakt als een JSON-object met `id` en `type` . Net als andere eindpunten voor het ophalen van bulkmiddelen zijn `offset` en `maxReturn` optionele parameters die kunnen worden gebruikt voor paginering. Met de parameters `earliestUpdatedAt` en `latestUpdatedAt` kunt u lage en hoge datetime watermerken instellen voor het retourneren van statische lijsten die binnen het opgegeven bereik zijn gemaakt of bijgewerkt. Datumtijdwaarden moeten geldige ISO-8601-tekenreeksen zijn en mogen geen milliseconden bevatten.

```http
GET /rest/asset/v1/staticLists.json?folder={"id":13,"type":"Folder"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "2dc0#1641f846633",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        },
        {
            "id": 1022,
            "name": "Blacklist Seed List",
            "createdAt": "2017-07-27T23:19:33Z+0000",
            "updatedAt": "2017-07-27T23:21:29Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1022A1"
        },
        {
            "id": 1023,
            "name": "Possible Duplicates Seed List",
            "createdAt": "2017-07-28T00:10:02Z+0000",
            "updatedAt": "2017-07-28T00:11:22Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1023A1"
        }
    ]
}
```

## Maken en bijwerken

[&#x200B; Creërend een statische lijst &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/createStaticListUsingPOST) wordt uitgevoerd met `application/x-www-form-urlencoded` POST met twee vereiste parameters. De parameter `folder` wordt gebruikt om de bovenliggende map op te geven waaronder de statische lijst wordt gemaakt en wordt opgemaakt als een JSON-object met `id` en `type` . De parameter `name` wordt gebruikt om de statische lijst een naam te geven en moet uniek zijn. De parameter `description` kan optioneel worden gebruikt om de statische lijst te beschrijven.

```http
POST /rest/asset/v1/staticLists.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
folder={"id":1034,"type":"Program"}&name=My Static List
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1269d#164209d6e1e",
    "result": [
        {
            "id": 1027,
            "name": "My Static List",
            "createdAt": "2018-06-21T04:32:25Z+0000",
            "updatedAt": "2018-06-21T04:32:25Z+0000",
            "folder": {
                "id": 1034,
                "type": "Program"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1027A1"
        }
    ]
}
```

[&#x200B; het Bijwerken van een statische lijst &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/updateStaticListUsingPOST) wordt gedaan door een afzonderlijk eindpunt met twee facultatieve parameters. De parameter `description` kan worden gebruikt om de beschrijving van de statische lijst bij te werken. De parameter `name` kan worden gebruikt om de statische lijstnaam bij te werken en moet uniek zijn.

```http
POST /rest/asset/v1/staticList/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
description=This is a static list used for testing
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "f84f#16420b4c746",
    "result": [
        {
            "id": 1027,
            "name": "My Static List",
            "description": "This is a static list used for testing",
            "createdAt": "2018-06-21T04:32:26Z+0000",
            "updatedAt": "2018-06-21T04:57:55Z+0000",
            "folder": {
                "id": 1034,
                "type": "Program"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1027A1"
        }
    ]
}
```

## Verwijderen

[&#x200B; het schrappen van een statische lijst &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/deleteStaticListByIdUsingPOST) neemt één enkele statische lijst `id` als wegparameter. U kunt geen verwijderingen maken voor statische lijsten die worden gebruikt door een import- of exportbewerking of die worden gebruikt door andere elementen.

```http
POST /rest/asset/v1/staticList/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "2c79#16420ded0e9",
    "result": [
        {
            "id": 1027
        }
    ]
}
```
