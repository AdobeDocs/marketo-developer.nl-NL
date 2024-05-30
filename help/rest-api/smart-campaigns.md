---
title: "Slimme campagnes"
feature: REST API, Smart Campaigns
description: "Slim campagneoverzicht"
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '989'
ht-degree: 0%

---


# Slimme campagnes

[Smart Campaigns Endpoint Reference (Asset)](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns)

[Campagne Endpoint Reference (Leads)](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns)

Marketo biedt een set REST API&#39;s voor het uitvoeren van bewerkingen op slimme campagnes. Deze API&#39;s volgen het standaard interfacepatroon voor element-API&#39;s die query-, maak-, kloon- en verwijderopties bieden. U kunt ook de uitvoering van slimme campagnes beheren door batchcampagnes te plannen of triggercampagnes aan te vragen.

## Query

Het vragen van slimme campagnes volgt de standaardvraagtypes voor activa van [door id](#by_id), [op naam](#by_name), en [bladeren](#browse).

### Op id

De [Slimme campagne ophalen op id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByIdUsingGET) eindpunt neemt één enkele slimme campagne `id` als padparameter en retourneert één slimme-campagnerecord.

```
GET /rest/asset/v1/smartCampaign/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "7883#169838a32f0",
    "warnings": [],
    "result": [
        {
            "id": 1001,
            "name": "Process Bounced Emails",
            "description": "System smart campaign for processing bounced email events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1001,
            "flowId": 1001,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1001A1"
        }
    ]
}
```

Met dit eindpunt, zal er altijd één enkel verslag in de eerste positie van zijn `result` array.

### Op naam

De [Slimme campagne op naam ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByNameUsingGET) eindpunt neemt één enkele slimme campagne `name` als een parameter en retourneert één slimme-campagnerecord.

```
GET /rest/asset/v1/smartCampaign/byName.json?name=Test Trigger Campaign
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "14494#16c886ffa44",
    "warnings": [],
    "result": [
        {
            "id": 1069,
            "name": "Test Trigger Campaign",
            "description": "",
            "createdAt": "2018-02-16T01:34:39Z+0000",
            "updatedAt": "2019-08-13T00:45:21Z+0000",
            "folder": {
                "id": 327,
                "type": "Folder"
            },
            "status": "Inactive",
            "type": "trigger",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 2747,
            "flowId": 1088,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1069A1"
        }
    ]
}
```

Met dit eindpunt, zal er altijd één enkel verslag in de eerste positie van zijn `result` array.

### Bladeren

De [Slimme campagnes ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getAllSmartCampaignsGET) het eindpunt werkt zoals andere activa API doorbladert eindpunten en staat verscheidene facultatieve vraagparameters toe om het filtreren criteria te specificeren.

De `earliestUpdatedAt` en `latestUpdatedAt` parameters accepteren `datetimes` in de ISO-8601-indeling (zonder milliseconden). Als beide zijn ingesteld, moet firstUpdatedAt voorafgaan aan latestUpdatedAt.

De `folder` parameter geeft de bovenliggende map aan waarin moet worden gezocht. De indeling is JSON-blok met `id` en `type` kenmerken.

De `maxReturn` parameter is een geheel getal dat het maximale aantal items aangeeft dat moet worden geretourneerd. De standaardwaarde is 20. Maximaal is 200.

De `offset` parameter is een geheel getal dat opgeeft waar moet worden begonnen met het ophalen van vermeldingen. Kan samen met `maxReturn`. De standaardwaarde is 0.

De `isActive` parameter is een Booleaanse waarde die aangeeft dat alleen actieve triggercampagnes moeten worden geretourneerd.

```
GET /rest/asset/v1/smartCampaigns.json?earliestUpdatedAt=2016-09-10T23:15:00-00:00&latestUpdatedAt=2016-09-10T23:17:00-00:00
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "626#16983a92965",
    "warnings": [],
    "result": [
        {
            "id": 1001,
            "name": "Process Bounced Emails",
            "description": "System smart campaign for processing bounced email events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1001,
            "flowId": 1001,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1001A1"
        },
        {
            "id": 1002,
            "name": "Process Unsubscribes",
            "description": "System smart campaign for processing unsubscribe events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1002,
            "flowId": 1002,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1002A1"
        }
    ]
}
```

Met dit eindpunt, zal er één of meerdere verslagen in zijn `result` array.

## Maken

De [Slimme campagne maken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/createSmartCampaignUsingPOST) Het eindpunt wordt uitgevoerd met een application/x-www-form-urlencoded POST met twee vereiste parameters. De `name` parameter geeft de naam aan van de slimme campagne die moet worden gemaakt. De `folder` parameter geeft de bovenliggende map aan waarin de slimme campagne is gemaakt. De indeling is JSON-blok met `id` en `type` kenmerken.

U kunt desgewenst de slimme campagne beschrijven met de opdracht `description` parameter (maximaal 2000 tekens).

```
POST /rest/asset/v1/smartCampaigns.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Smart Campaign 02&folder={"type": "folder","id": 640}&description=This is a smart campaign creation test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "25bc#16c9138f148",
    "warnings": [],
    "result": [
        {
            "id": 1076,
            "name": "Smart Campaign 02",
            "description": "This is a smart campaign creation test.",
            "createdAt": "2019-08-14T17:42:04Z+0000",
            "updatedAt": "2019-08-14T17:42:04Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": true,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5132,
            "flowId": 1095,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1076A1"
        }
    ]
}
```

## Bijwerken

De [Slimme campagne bijwerken](https://developer.adobe.com/marketo-apis/api/asset/) Het eindpunt wordt uitgevoerd met een application/x-www-form-urlencoded POST. Er is één slimme campagne voor nodig `id` als padparameter. U kunt de `name` parameter om de naam van de slimme campagne bij te werken, of `description` om de beschrijving van de slimme campagne bij te werken.

```
POST /rest/asset/v1/smartCampaign/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Smart Campaign 02 Update&description=This is a smart campaign update test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "14b6a#16c924b992f",
    "warnings": [],
    "result": [
        {
            "id": 1076,
            "name": "Smart Campaign 02 Update",
            "description": "This is a smart campaign update test.",
            "createdAt": "2019-08-14T17:42:04Z+0000",
            "updatedAt": "2019-08-14T22:42:04Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": true,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5132,
            "flowId": 1095,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1076A1"
        }
    ]
}
```

## Klonen

De [Slimme klonen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) Het eindpunt wordt uitgevoerd met een application/x-www-form-urlencoded POST met drie vereiste parameters. Het kost een `id` parameter die aangeeft welke slimme campagne moet worden gekloond, een `name` parameter die de naam van nieuwe slimme campagne specificeert, en `folder` om de bovenliggende map op te geven waarin de nieuwe slimme campagne wordt gemaakt. De indeling is JSON-blok met `id` en `type` kenmerken.

U kunt desgewenst de slimme campagne beschrijven met de opdracht `description` parameter (maximaal 2000 tekens).

```
POST /rest/asset/v1/smartCampaign/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Test Trigger Campaign Clone&folder={"type": "folder","id": 640}&description=This is a smart campaign clone test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "681d#16c9339499b",
    "warnings": [],
    "result": [
        {
            "id": 1077,
            "name": "Test Trigger Campaign Clone",
            "description": "This is a smart campaign clone test.",
            "createdAt": "2019-08-15T03:01:41Z+0000",
            "updatedAt": "2019-08-15T03:01:41Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Inactive",
            "type": "trigger",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5135,
            "flowId": 1096,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1077A1"
        }
    ]
}
```

## Verwijderen

De [Slimme campagne verwijderen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deleteSmartCampaignUsingPOST) eindpunt neemt één enkele slimme campagne `id` als padparameter.

```
POST /rest/asset/v1/smartCampaign/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d757#16c934216ac",
    "warnings": [],
    "result": [
        {
            "id": 1077
        }
    ]
}
```

## Batch

De slimme campagnes van de partij lanceren op een specifiek ogenblik en beïnvloeden een specifieke reeks lood allen in één keer.

## Schema

Gebruik de [Campagne plannen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/scheduleCampaignUsingPOST) om een batchcampagne onmiddellijk of op een toekomstige datum te plannen. De campagne `id` is een vereiste padparameter. Optionele parameters zijn `tokens`, `runAt`, en `cloneToProgram` die in de aanvraaginstantie worden doorgegeven als applicatie/json.

De parameter van de tokens serie is een serie van Mijn Tokens die bestaande programmatokens met voeten treden. Nadat de campagne is gestart, worden de tokens genegeerd.  Elk token-arrayitem bevat naam/waardeparen. De naam van het token moet worden opgemaakt als &quot;{{my.name}}&quot;.

De runtime parameter specificeert wanneer om de campagne in werking te stellen. Als gespecificeerd niet, zal de campagne 5 minuten nadat het eindpunt is geroepen in werking worden gesteld. De datetime-waarde mag in de toekomst niet meer dan twee jaar bedragen.

Campagnes die via deze API zijn gepland, wachten altijd minimaal vijf minuten voordat ze worden uitgevoerd.

De `cloneToProgram` Tekenreeksparameter bevat de naam van een resulterend programma.  Wanneer deze optie is ingesteld, worden de campagne, het bovenliggende programma en alle elementen ervan gemaakt met de resulterende nieuwe naam. Het bovenliggende programma is gekloond en de nieuwe campagne wordt gepland. Het resulterende programma wordt onder het bovenliggende onderdeel gemaakt. Programma&#39;s met fragmenten, pushmeldingen, in-app berichten, statische lijsten, rapporten en sociale elementen worden mogelijk niet op deze manier gekloond. Wanneer gebruikt, is dit eindpunt beperkt tot 20 vraag per dag. De [clone-programma](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) eindpunt is het aanbevolen alternatief.

```
POST /rest/v1/campaigns/{id}/schedule.json
```

```json
{
   "input":
      {
         "runAt": "2018-03-28T18:05:00+0000",
         "tokens": [
            {
               "name": "{{my.message}}",
               "value": "Updated message"
            },
            {
               "name": "{{my.other token}}",
               "value": "Value for other token"
            }
          ]
      }
}
```

```json
{
    "requestId": "52b#161d90e1743",
    "result": [
        {
            "id": 3713
        }
    ],
    "success": true
}
```

## Trigger

Slimme campagnes activeren beïnvloedt één persoon per keer op basis van een getriggerde gebeurtenis.

### Verzoek

Gebruik de [Campagne aanvragen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/triggerCampaignUsingPOST) eindpunt om een reeks lood tot een trekkercampagne over te gaan om door de stroom van de campagne te lopen. De campagne moet een &quot;Campagne wordt Gevraagd&quot;trekker met &quot;de Dienst API van het Web&quot;als bron hebben.

Dit eindpunt vereist een campagne `id` als padparameter en als een `leads` parameter integer array met lead ids . Een maximum van 100 lood wordt toegestaan per vraag.

Naar keuze, `tokens` arrayparameter kan worden gebruikt om My Tokens lokaal aan het ouderprogramma van de campagne met voeten te treden. `tokens` accepteert maximaal 100 tokens. Elk `tokens` arrayitem bevat een naam/waardepaar. De naam van het token moet worden opgemaakt als &quot;{{my.name}}&quot;. Als u [Een systeemtoken toevoegen als een koppeling in een e-mailbericht](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/add-a-system-token-as-a-link-in-an-email) de methode om het &quot;viewAsWebpageLink&quot;systeemteken toe te voegen, kunt u niet het met voeten treden gebruikend `tokens`. In plaats daarvan gebruiken [Een koppeling Weergeven als webpagina toevoegen aan een e-mailbericht](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-a-view-as-web-page-link-to-an-email) aanpak waarmee u &quot;viewAsWebPageLink&quot; kunt negeren met behulp van `tokens`.

De `leads` en `tokens` parameters worden in de aanvraaginstantie doorgegeven als application/json.

```
POST /rest/v1/campaigns/{id}/trigger.json
```

```json
{
   "input":
      {
         "leads" : [
            {
               "id" : 318592
            },
            {
               "id" : 318593
            }
         ],
         "tokens" : [
            {
               "name": "{{my.message}}",
               "value": "Updated message"
            },
            {
               "name": "{{my.other token}}",
               "value": "Value for other token"
            }
         ]
      }
}
```

```json
{
    "requestId": "9e01#161d922f1aa",
    "result": [
        {
            "id": 3712
        }
    ],
    "success": true
}
```

### Activeren

De [Slimme campagne activeren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/activateSmartCampaignUsingPOST) het eindpunt is ongecompliceerd. An `id` padparameter is vereist. Activering is alleen succesvol als de volgende waarden gelden voor de campagne:

- Moet worden gedeactiveerd
- Moet ten minste één trigger en één flowstap hebben
- Moet foutvrije triggers, filters en flowstappen hebben

```
POST /rest/asset/v1/smartCampaign/{id}/activate.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "a33a#161d9c0dcf3",
    "result": [
        {
            "id": 1069
        }
    ]
}
```

### Deactiveren

De [Slimme campagne deactiveren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deactivateSmartCampaignUsingPOST) is eenvoudig. An `id` padparameter is vereist. De deactivering kan alleen slagen als de campagne is geactiveerd.

```
POST /rest/asset/v1/smartCampaign/{id}/deactivate.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6228#161d9c29fbf",
    "result": [
        {
            "id": 1069
        }
    ]
}
```
