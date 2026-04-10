---
title: Lidmaatschap weergeven (statische lijsten)
feature: REST API, Static Lists
description: Gebruik de REST-API's van Marketo Lead Database om leads toe te voegen aan statische lijsten, leads te verwijderen, leden van lijsten op te halen en het lidmaatschap van lijsten te controleren.
exl-id: b8f74bcf-834a-44db-81fd-621048afeba4
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---

# Lidmaatschap weergeven (statische lijsten)

[Referentie eindpunt van lidmaatschap weergeven](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

De APIs van het Lidmaatschap van de Lijst verstrekken de eindpunten van het Gegevensbestand van de Leiding voor het werken met statische lijstleden. Met deze eindpunten kunt u leads toevoegen aan een lijst, leads verwijderen uit een lijst, leden van een lijst ophalen en bepalen of een of meer leads lid van een lijst zijn.

## Eindpunten

| Endpoint | Methode | Pad |
| --- | --- | --- |
| Toevoegen aan lijst | POST | `/rest/v1/lists/{listId}/leads.json` |
| Verwijderen uit lijst | DELETE | `/rest/v1/lists/{listId}/leads.json` |
| Leden ophalen op lijst-id | GET | `/rest/v1/lists/{listId}/leads.json` |
| Lid van de lijst | GET | `/rest/v1/lists/{listId}/leads/ismember.json` |

## Toevoegen aan lijst

[&#x200B; voegt aan het eindpunt van de Lijst &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/addLeadsToListUsingPOST) toe wordt gebruikt om één of meerdere leden aan een lijst toe te voegen. Het eindpunt neemt een vereiste `listId` wegparameter, en één of meerdere `id` vraagparameters die loodids bevatten (maximaal toegestaan is 300).

De reactie bevat een `result` -array die bestaat uit JSON-objecten met de status voor elke lead-id die in de aanvraag is opgegeven.

```http
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

## Verwijderen uit lijst

[&#x200B; verwijder uit &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE) eindpunt van de Lijst &lbrace;wordt gebruikt om één of meerdere leden uit een lijst te verwijderen. Het eindpunt neemt een vereiste `listId` wegparameter, en één of meerdere `id` vraagparameters die loodids bevatten (maximaal toegestaan is 300).

De reactie bevat een `result` -array die bestaat uit JSON-objecten met de status voor elke lead-id die in de aanvraag is opgegeven.

```http
DELETE /rest/v1/lists/{listId}/leads.json?id=318603&id=318595&id=999999
```

```json
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

## Leden ophalen op lijst-id

[&#x200B; krijgt Leads door Identiteitskaart van de Lijst &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET) eindpunt wordt gebruikt om leden van een lijst terug te winnen. Het eindpunt neemt een vereiste `listId` wegparameter, en staat verscheidene facultatieve vraagparameters toe om het filtreren criteria te specificeren.

De parameter `batchSize` wordt gebruikt om het aantal lead records op te geven dat in één aanroep moet worden geretourneerd. De standaardwaarde en het maximum is 300.

De parameter `nextPageToken` wordt gebruikt om door grote resultaatreeksen te pagineren. Deze parameter wordt niet overgegaan in de eerste vraag, maar slechts in verdere vraag naar paginering.

De parameter `fields` bevat een door komma&#39;s gescheiden lijst met veldnamen die in het antwoord moeten worden geretourneerd. Als de parameter `fields` niet in deze aanvraag wordt opgenomen, worden de volgende standaardvelden geretourneerd: `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` en `id` .

De reactie bevat een `result` -array die bestaat uit JSON-objecten die de hoofdvelden bevatten die in de aanvraag zijn opgegeven.

```http
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

## Lid van de lijst

Het [&#x200B; Lid van het eindpunt van de Lijst &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) wordt gebruikt om te zien of zijn één of meerdere lood leden van een lijst. Het eindpunt neemt een vereiste `listId` wegparameter, en één of meerdere `id` vraagparameters die loodids bevatten (maximaal toegestaan is 300).

De reactie bevat een `result` -array die bestaat uit JSON-objecten met de status voor elke lead-id die in de aanvraag is opgegeven.

```http
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
