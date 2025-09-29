---
title: Slimme lijsten
feature: REST API
description: Leer hoe te om Marketo REST APIs te gebruiken om, gebruiker-gecreeerde Slimme Lijsten, met inbegrip van eindpunten door identiteitskaart, naam, campagne, en programma met regels te vragen te klonen en te schrappen.
exl-id: 4ba37e57-ee56-48c3-bb2b-b4ec8e907911
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---

# Slimme lijsten

[ Slimme Verwijzing van Lijsten Eindpunt ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists)

Marketo biedt een set REST API&#39;s voor het uitvoeren van bewerkingen op slimme lijsten. Deze API&#39;s volgen het standaard interfacepatroon voor de bron-API&#39;s die de opties Query, Delete en Clone bieden.

Opmerking: deze API&#39;s worden alleen ondersteund voor door de gebruiker gemaakte slimme lijsten. Zij kunnen niet voor [ worden gebruikt ingebouwde/Slimme Lijsten van het Systeem ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/using-smart-lists/use-built-in-system-smart-lists).

## Query

Het vragen van slimme lijsten volgt de standaardvraagtypes voor activa van [ door identiteitskaart ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByIdUsingGET), [ door naam ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByNameUsingGET), en [ doorbladert ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListsUsingGET).

### Op id

[ Vraag door identiteitskaart ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByIdUsingGET) neemt één enkele slimme lijst `id` als wegparameter en keert één enkel slim lijstverslag terug. Desgewenst kunt u de Booleaanse parameter `includeRules` doorgeven om slimme-lijstregels in het antwoord op te nemen.

![ Smartlist Regels ](assets/smartlist-rules.png)

```
GET /rest/asset/v1/smartList/{id}.json?includeRules=true
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6efc#16c8967a21f",
    "warnings": [],
    "result": [
        {
            "id": 4363,
            "name": "Smart List Test 01",
            "createdAt": "2019-06-03T23:01:13Z+0000",
            "updatedAt": "2019-06-04T17:37:45Z+0000",
            "url": "https://app-sjqe.marketo.com/#SL4363A1LA1",
            "folder": {
                "id": 1041,
                "type": "Program"
            },
            "workspace": "Default",
            "rules": {
                "filterMatchType": "all",
                "triggers": [],
                "filters": [
                    {
                        "id": 459,
                        "name": "Visited Web Page",
                        "ruleTypeId": 1,
                        "ruleType": "Activity",
                        "operator": "occurs",
                        "conditions": [
                            {
                                "activityAttributeId": 1,
                                "activityAttributeName": "Web Page",
                                "operator": "is",
                                "values": [
                                    "Program Test.Landing Page Test 01"
                                ],
                                "isPrimary": true
                            },
                            {
                                "activityAttributeId": 6,
                                "activityAttributeName": "Browser",
                                "operator": "is",
                                "values": [
                                    "Chrome"
                                ],
                                "isPrimary": false
                            },
                            {
                                "activityAttributeId": -101,
                                "activityAttributeName": "Date of Activity",
                                "operator": "in past",
                                "values": [
                                    "30 days"
                                ],
                                "isPrimary": false
                            }
                        ]
                    }
                ]
            }
        }
    ]
}
```

### Op slimme campagne-id

[ Vraag door slimme campagne identiteitskaart ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartListBySmartCampaignIdUsingGET) neemt één enkele slimme campagne `id` als wegparameter en keert één enkel slim lijstverslag terug. Desgewenst kunt u de Booleaanse parameter `includeRules` doorgeven om slimme-lijstregels in het antwoord op te nemen.

```
GET /rest/asset/v1/smartCampaign/{smartCampaignId}/smartList.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6efc#16c8967a21f",
    "warnings": [],
    "result": [
        {
            "id": 4363,
            "name": "Smart List Test 01",
            "createdAt": "2019-06-03T23:01:13Z+0000",
            "updatedAt": "2019-06-04T17:37:45Z+0000",
            "url": "https://app-sjqe.marketo.com/#SL4363A1LA1",
            "folder": {
                "id": 1041,
                "type": "Program"
            },
            "workspace": "Default"
         }
    ]
}
```

### Op programma-id

[ Vraag door programma identiteitskaart ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getSmartListByProgramIdUsingGET) neemt één enkel e-mailprogramma `id` als wegparameter en keert één enkel slim lijstverslag terug. Desgewenst kunt u de Booleaanse parameter `includeRules` doorgeven om slimme-lijstregels in het antwoord op te nemen.

```
GET /rest/asset/v1/program/{programId}/smartList.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6efc#16c8967a21f",
    "warnings": [],
    "result": [
        {
            "id": 4363,
            "name": "Smart List Test 01",
            "createdAt": "2019-06-03T23:01:13Z+0000",
            "updatedAt": "2019-06-04T17:37:45Z+0000",
            "url": "https://app-sjqe.marketo.com/#SL4363A1LA1",
            "folder": {
                "id": 1041,
                "type": "Program"
            },
            "workspace": "Default"
         }
    ]
}
```

### Op naam

[ Vraag door naam ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByNameUsingGET) neemt een slimme lijst `name` als parameter en keert één enkel slim lijstverslag terug.  Er wordt een exacte tekenreeksovereenkomst uitgevoerd met alle slimme lijstnamen in de instantie en er wordt een resultaat geretourneerd voor de slimme lijst die overeenkomt met die naam.

```
GET /rest/asset/v1/smartList/byName.json?name=2018 Leads
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "115d7#16423bc13b4",
    "result": [
        {
            "id": 283988,
            "name": "2018 Leads",
            "createdAt": "2008-10-07T15:20:39Z+0000",
            "updatedAt": "2010-04-13T15:34:32Z+0000",
            "url": "https://app-abm.marketo.com/#SL283988A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        }
    ]
}
```

### Bladeren

De slimme lijsten kunnen ook [ worden teruggewonnen in partijen ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListsUsingGET). De parameter `folder` wordt gebruikt om de bovenliggende map op te geven waaronder de query wordt uitgevoerd. Het object is opgemaakt als een JSON-object met `id` en `type` . Net als andere eindpunten voor het ophalen van bulkmiddelen zijn `offset` en `maxReturn` optionele parameters die kunnen worden gebruikt voor paginering. De optionele parameters `earliestUpdatedAt` en `latestUpdatedAt` datetime kunnen worden gebruikt om de resultaten te filteren op UpdatedAt-datumbereik.

```
GET /rest/asset/v1/smartLists.json?folder={"id":31,"type":"Folder"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "9aa4#16423c0e969",
    "result": [
        {
            "id": 283988,
            "name": "2018 Leads",
            "createdAt": "2008-10-07T15:20:39Z+0000",
            "updatedAt": "2010-04-13T15:34:32Z+0000",
            "url": "https://app-abm.marketo.com/#SL283988A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        },
        {
            "id": 299697,
            "name": "Active Prospects",
            "createdAt": "2008-10-17T02:09:49Z+0000",
            "updatedAt": "2010-03-27T18:27:46Z+0000",
            "url": "https://app-abm.marketo.com/#SL299697A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        },
        {
            "id": 400517,
            "name": "Leads by Score",
            "createdAt": "2009-01-07T18:52:52Z+0000",
            "updatedAt": "2010-04-13T15:36:09Z+0000",
            "url": "https://app-abm.marketo.com/#SL400517A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        }
    ]
}
```

## Klonen

[ het Klonen van een slimme lijst ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/cloneSmartListUsingPOST) wordt uitgevoerd met een toepassing/x-www-vorm-urlencoded POST. De slimme lijst die moet worden gekloond, wordt opgegeven in de padparameter `id` . De parameter `folder` wordt gebruikt om de bovenliggende map op te geven waaronder de slimme lijst wordt gemaakt en wordt opgemaakt als een JSON-object met id en type. De bovenliggende map moet een map van het type Program of Smart List zijn. De parameter `name` wordt gebruikt om de nieuwe slimme lijst een naam te geven en moet uniek zijn. De parameter `description` kan optioneel worden gebruikt om de slimme lijst te beschrijven.

```
POST /rest/asset/v1/smartList/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
folder={"id":31,"type":"Folder"}&name=2018 Leads Qualified
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "a672#16423d755ed",
    "result": [
        {
            "id": 788645,
            "name": "2018 Leads Qualified",
            "createdAt": "2018-06-21T19:34:32Z+0000",
            "updatedAt": "2018-06-21T19:34:32Z+0000",
            "url": "https://app-abm.marketo.com/#SL788645A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        }
    ]
}
```

## Verwijderen

[ het schrappen van een slimme lijst ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/deleteSmartListByIdUsingPOST) neemt één enkele slimme lijst `id` als wegparameter.

```
POST /rest/asset/v1/smartList/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "8f5#16423dd0fbe",
    "result": [
        {
            "id": 788645
        }
    ]
}
```
