---
title: "Slimme lijsten"
feature: REST API
description: "Slimme lijsten maken en bewerken."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 0%

---


# Slimme lijsten

[Verwijzing naar eindpunt slimme lijsten](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists)

Marketo biedt een set REST API&#39;s voor het uitvoeren van bewerkingen op slimme lijsten. Deze API&#39;s volgen het standaard interfacepatroon voor de bron-API&#39;s die de opties Query, Delete en Clone bieden.

Opmerking: deze API&#39;s worden alleen ondersteund voor door de gebruiker gemaakte slimme lijsten. Ze kunnen niet worden gebruikt voor [Geïntegreerde/systeemslimme lijsten](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/using-smart-lists/use-built-in-system-smart-lists).

## Query

Het vragen van slimme lijsten volgt de standaardvraagtypes voor activa van [door id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByIdUsingGET), [op naam](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByNameUsingGET), en [doorbladeren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListsUsingGET).

### Op id

[Query uitvoeren op id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByIdUsingGET) neemt één slimme lijst `id` als padparameter en retourneert één slimme-lijstrecord. U kunt optioneel het `includeRules` booleaanse parameter om slimme-lijstregels op te nemen in de reactie.

![Smartlist-regels](assets/smartlist-rules.png)

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

[Query uitvoeren op id van slimme campagne](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartListBySmartCampaignIdUsingGET) neemt één slimme campagne `id` als padparameter en retourneert één slimme-lijstrecord. U kunt optioneel het `includeRules` booleaanse parameter om slimme-lijstregels op te nemen in de reactie.

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

[Query uitvoeren op programma-id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getSmartListByProgramIdUsingGET) neemt één e-mailprogramma `id` als padparameter en retourneert één slimme-lijstrecord. U kunt optioneel het `includeRules` booleaanse parameter om slimme-lijstregels op te nemen in de reactie.

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

[Query uitvoeren op naam](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByNameUsingGET) neemt een slimme lijst `name` als een parameter en retourneert één record in de slimme lijst.  Er wordt een exacte tekenreeksovereenkomst uitgevoerd met alle slimme lijstnamen in de instantie en er wordt een resultaat geretourneerd voor de slimme lijst die overeenkomt met die naam.

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

Slimme lijsten kunnen ook [opgehaald in batches](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListsUsingGET). De `folder` parameter wordt gebruikt om de bovenliggende map op te geven waaronder de query wordt uitgevoerd. Het object is opgemaakt als een JSON-object dat `id` en `type`. Als andere eindpunten van de bulkmiddelherwinning, `offset` en `maxReturn` Dit zijn optionele parameters die kunnen worden gebruikt voor pagineren. De optionele `earliestUpdatedAt` en `latestUpdatedAt` de datetime parameters kunnen worden gebruikt om de resultaten door UpdatedAt datumwaaier te filtreren.

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

[Een slimme lijst klonen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/cloneSmartListUsingPOST) wordt uitgevoerd met een application/x-www-form-urlencoded POST. De slimme lijst die moet worden gekloond, wordt opgegeven in het dialoogvenster `id` padparameter. De `folder` parameter wordt gebruikt om de bovenliggende map op te geven waaronder de slimme lijst wordt gemaakt en wordt opgemaakt als een JSON-object met id en type. De bovenliggende map moet een map van het type Program of Smart List zijn. De `name` wordt gebruikt om de nieuwe slimme lijst te noemen en moet uniek zijn. Naar keuze de `description` kan worden gebruikt om de slimme lijst te beschrijven.

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

[Een slimme lijst verwijderen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/deleteSmartListByIdUsingPOST) neemt één slimme lijst `id` als padparameter.

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
