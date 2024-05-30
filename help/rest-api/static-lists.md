---
title: Statische lijsten
feature: REST API, Static Lists
description: "Voer CRUD verrichtingen op statische lijsten uit."
source-git-commit: e8bb45a7b3bee71c3d0ab6117296a75c8959d72e
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 0%

---


# Statische lijsten

[Verwijzing naar eindpunt statische lijsten](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists)

[Referentie eindpunt van lidmaatschap weergeven](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

Marketo biedt een set REST API&#39;s voor het uitvoeren van CRUD-bewerkingen op statische lijsten. Deze API&#39;s volgen het standaard interfacepatroon voor de bron-API&#39;s die de opties Query, Maken, Bijwerken en Verwijderen bieden.

## Query

Het vragen van statische lijsten volgt de standaardvraagtypes voor activa van [door id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET), [op naam](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET), en [doorbladeren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET).

### Op id

[Query uitvoeren op id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) neemt één enkele statische lijst `id` als padparameter en retourneert één statische lijstrecord.

```
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

[Query uitvoeren op naam](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByNameUsingGET) neemt een statische lijst `name` als een parameter en retourneert één statische List-record. Een nauwkeurige koordgelijke wordt uitgevoerd tegen alle statische lijstnamen in de instantie, en keert een resultaat voor de statische lijst terug die die naam aanpassen.

```
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

Statische lijsten kunnen ook [opgehaald in batches](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET). De `folder` parameter kan worden gebruikt om de bovenliggende map op te geven waaronder de query wordt uitgevoerd en wordt opgemaakt als een JSON-object met id en type. Als andere eindpunten van de bulkmiddelherwinning, `offset` en `maxReturn` Dit zijn optionele parameters die kunnen worden gebruikt voor pagineren. De `earliestUpdatedAt` en `latestUpdatedAt` Met parameters kunt u lage en hoge datumtijdwatermerken instellen voor het retourneren van statische lijsten die zijn gemaakt of bijgewerkt binnen het opgegeven bereik. Datumtijdwaarden moeten geldige ISO-8601-tekenreeksen zijn en mogen geen milliseconden bevatten

```
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

[Een statische lijst maken](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/createStaticListUsingPOST) wordt uitgevoerd met een application/x-www-form-urlencoded POST met twee vereiste parameters. De `folder` parameter wordt gebruikt om de bovenliggende map op te geven waaronder de statische lijst wordt gemaakt en wordt opgemaakt als een JSON-object met id en type. De `name` parameter wordt gebruikt om de statische lijst te noemen en moet uniek zijn. Naar keuze de `description` parameter kan worden gebruikt om de statische lijst te beschrijven.

```
POST /rest/asset/v1/staticLists.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

[Updates van een statische lijst](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/updateStaticListUsingPOST) worden gemaakt door een afzonderlijk eindpunt met twee optionele parameters. De `description` kan worden gebruikt om de beschrijving van de statische lijst bij te werken. De `name` parameter kan worden gebruikt om de statische lijstnaam bij te werken en moet uniek zijn.

```
POST /rest/asset/v1/staticList/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

### Verwijderen

[Een statische lijst verwijderen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/deleteStaticListByIdUsingPOST) neemt één enkele statische lijst `id` als padparameter. U kunt geen verwijderingen maken voor statische lijsten die worden gebruikt door een import- of exportbewerking of die worden gebruikt door andere elementen.

```
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

## Lidmaatschap lijst

De eindpunten van het lijstlidmaatschap verstrekken capaciteit om, statische lijstleden toe te voegen te verwijderen en te vragen. Bovendien kunt u een query uitvoeren op het lidmaatschap van een statische lijst.

### Toevoegen aan lijst

De [Toevoegen aan lijst](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/addLeadsToListUsingPOST) het eindpunt wordt gebruikt voegt één of meerdere leden aan een lijst toe. Het eindpunt neemt vereist `listId` padparameter en een of meer id-query-parameters die lead id&#39;s bevatten (maximaal toegestaan is 300).

De reactie bevat een `result` array die bestaat uit JSON-objecten met de status voor elke lead-id die in de aanvraag is opgegeven.

```
POST /rest/v1/lists/{listId}/leads.json?id=318594&id=318595
```

```json
{
    "requestId": "6860#1706170ba29",
    "result": [
        {
            "id": 318594,
            "status": "added"
        },
        {
            "id": 318595,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```

### Verwijderen uit lijst

De [Verwijderen uit lijst](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE) het eindpunt wordt gebruikt verwijdert één of meerdere leden uit een lijst. Het eindpunt neemt vereist `listId` padparameter en een of meer `id` query-parameters die lead ids bevatten (maximaal toegestaan is 300).

De reactie bevat een `result` array die bestaat uit JSON-objecten met de status voor elke lead-id die in de aanvraag is opgegeven.

```
DELETE /rest/v1/lists/{listId}/leads.json?id=318603&id=318595&id=999999
```

```
{
    "requestId": "9e79#17061689ac3",
    "result": [
        {
            "id": 318603,
            "status": "removed"
        },
        {
            "id": 318595,
            "status": "removed"
        },
        {
            "id": 999999,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```

### Zoeklijst

De [Leden ophalen op lijst-id](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET) eindpunt wordt gebruikt om leden van een lijst terug te winnen. Het eindpunt neemt vereist `listId` padparameter en staat verschillende optionele queryparameters toe om filtercriteria op te geven.

De `batchSize` parameter wordt gebruikt om het aantal loodverslagen te specificeren die in één enkele vraag (gebrek en maximum is 300) moeten zijn teruggekeerd.

De `nextPageToken` parameter wordt gebruikt om door grote resultaatreeksen te pagineren. Deze parameter wordt niet overgegaan in de eerste vraag, maar slechts in verdere vraag naar de paginering.

De `fields` bevat een door komma&#39;s gescheiden lijst met veldnamen die in het antwoord moeten worden geretourneerd. Als de parameter fields niet is opgenomen in deze aanvraag, worden de volgende standaardvelden geretourneerd: email, updatedAt, createdAt, lastName, firstName en id.

De reactie bevat een `result` array die bestaat uit JSON-objecten die de hoofdvelden bevatten die in de aanvraag zijn opgegeven.

```
GET /rest/v1/lists/{listId}/leads.json?batchSize=3
```

```json
{
    "requestId": "ddae#170615ba0cc",
    "result": [
        {
            "id": 318594,
            "firstName": "Hanna",
            "lastName": "Crawford",
            "email": "208161Robert.L.Deacon@pookmail.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        },
        {
            "id": 318595,
            "firstName": "Bertha",
            "lastName": "Fulton",
            "email": "208160Tyrone.V.Dyer@trashymail.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        },
        {
            "id": 318596,
            "firstName": "Faith",
            "lastName": "England",
            "email": "208159Rex.M.Bailey@dodgit.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        }
    ],
    "success": true,
    "nextPageToken": "PS5VL5WD4UOWGOUCJR6VY7JQO24LC2U5DRBU4WO4RQMPHDHTK2T3BEZOR75VLQXYB3245WW2GMDSK==="
}
```

#### Vraag lijstlidmaatschap door lead-id

De [Lid van de lijst](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) eindpunt wordt gebruikt om te zien of zijn één of meerdere lood lid van een lijst. Het eindpunt neemt vereist `listId` padparameter en een of meer `id` query-parameters die lead ids bevatten (maximaal toegestaan is 300).

De reactie bevat een `result` array die bestaat uit JSON-objecten met de status voor elke lead-id die in de aanvraag is opgegeven.

```
GET /rest/v1/lists/{listId}/leads/ismember.json?id=309901&id=318603&id=999999
```

```json
{
    "requestId": "693a#17061475cf9",
    "result": [
        {
            "id": 309901,
            "status": "memberof"
        },
        {
            "id": 318603,
            "status": "notmemberof"
        },
        {
            "id": 999999,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```
