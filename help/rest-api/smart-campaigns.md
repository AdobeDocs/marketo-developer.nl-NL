---
title: Slimme campagnes
feature: REST API, Smart Campaigns
description: Leer hoe u Marketo REST API's kunt gebruiken voor slimme campagnes, zoals zoeken op id of naam, filters doorbladeren, klonen verwijderen en triggers voor planning of aanvraag plannen
exl-id: 540bdf59-b102-4081-a3d7-225494a19fdd
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1012'
ht-degree: 0%

---

# Slimme campagnes

[ Slimme Verwijzing van het Eindpunt van Campagnes (Activa) ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns)

[ Verwijzing van het Eindpunt van campagnes (Leidingen) ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns)

Marketo biedt een set REST API&#39;s voor het uitvoeren van bewerkingen op slimme campagnes. Deze API&#39;s volgen het standaard interfacepatroon voor element-API&#39;s die query-, maak-, kloon- en verwijderopties bieden. U kunt ook de uitvoering van slimme campagnes beheren door batchcampagnes te plannen of triggercampagnes aan te vragen.

## Query

Het vragen van slimme campagnes volgt de standaardvraagtypes voor activa van [ door identiteitskaart ](#by_id), [ door naam ](#by_name), en [ doorbladerend ](#browse).

### Op id

[ krijgt Slimme Campagne door identiteitskaart ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByIdUsingGET) eindpunt neemt één enkele slimme campagne `id` als wegparameter en keert één enkel slim campagneverslag terug.

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

Bij dit eindpunt zal er altijd één record zijn in de eerste positie van de array `result` .

### Op naam

[ krijgt Slimme Campagne door het 1&rbrace; eindpunt van de Naam &lbrace;neemt één enkele slimme campagne ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByNameUsingGET) als parameter en keert één enkel slim campagneverslag terug.`name`

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

Bij dit eindpunt zal er altijd één record zijn in de eerste positie van de array `result` .

### Bladeren

[ krijgt Slimme Campagnes ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getAllSmartCampaignsGET) eindpuntwerken zoals andere activa API doorbladert eindpunten en staat verscheidene facultatieve vraagparameters toe om het filtreren criteria te specificeren.

De parameters `earliestUpdatedAt` en `latestUpdatedAt` accept `datetimes` in de ISO-8601-indeling (zonder milliseconden). Als beide zijn ingesteld, moet firstUpdatedAt voorafgaan aan latestUpdatedAt.

De parameter `folder` geeft de bovenliggende map aan waarin moet worden gebladerd. De indeling is JSON-blok met `id` - en `type` -kenmerken.

De parameter `maxReturn` is een geheel getal dat het maximale aantal items opgeeft dat moet worden geretourneerd. De standaardwaarde is 20. Maximaal is 200.

De parameter `offset` is een geheel getal dat opgeeft waar moet worden begonnen met het ophalen van vermeldingen. Kan samen met `maxReturn` worden gebruikt. De standaardwaarde is 0.

De parameter `isActive` is een Booleaanse waarde die opgeeft dat alleen actieve triggercampagnes moeten worden geretourneerd.

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

Bij dit eindpunt zijn er een of meer records in de array `result` .

## Maken

Het [ creeert Slimme 1&rbrace; eindpunt van de Campagne &lbrace;wordt uitgevoerd met een toepassing/x-www-vorm-urlencoded POST met twee vereiste parameters. ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/createSmartCampaignUsingPOST) De parameter `name` geeft de naam op van de slimme campagne die u wilt maken. De parameter `folder` geeft de bovenliggende map aan waar de slimme campagne is gemaakt. De indeling is JSON-blok met `id` - en `type` -kenmerken.

Desgewenst kunt u de slimme campagne beschrijven met de parameter `description` (maximaal 2000 tekens).

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

Het [ eindpunt van de Campagne van 0&rbrace; Update Slimme wordt uitgevoerd met een toepassing/x-www-vorm-urlencoded POST. ](https://developer.adobe.com/marketo-apis/api/asset/) Er is één slimme campagne `id` voor nodig als padparameter. Met de parameter `name` kunt u de naam van de slimme campagne bijwerken. Met de parameter `description` kunt u de beschrijving van de slimme campagne bijwerken.

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

Het [ eindpunt van de Campagne van de Kloon Slimme ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) wordt uitgevoerd met een toepassing/x-www-vorm-urlencoded POST met drie vereiste parameters. Er is een parameter `id` nodig die de slimme campagne opgeeft die moet worden gekloond, een parameter `name` die de naam van de nieuwe slimme campagne opgeeft en een parameter `folder` om de bovenliggende map op te geven waarin de nieuwe slimme campagne wordt gemaakt. De indeling is JSON-blok met `id` - en `type` -kenmerken.

Desgewenst kunt u de slimme campagne beschrijven met de parameter `description` (maximaal 2000 tekens).

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

Het [ punt van de Schrapping Slimme Campagne ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deleteSmartCampaignUsingPOST) neemt één enkele slimme campagne `id` als wegparameter.

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

Gebruik het [ eindpunt van de Campagne van het Programma ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/scheduleCampaignUsingPOST) om een partijcampagne te plannen om of onmiddellijk of op een toekomstige datum in werking te stellen. De campagne `id` is een vereiste padparameter. Optionele parameters zijn `tokens` , `runAt` en `cloneToProgram` die in de aanvraagtekst worden doorgegeven als application/json.

De parameter van de tokens serie is een serie van Mijn Tokens die bestaande programmatokens met voeten treden. Nadat de campagne is gestart, worden de tokens genegeerd.  Elk token-arrayitem bevat naam/waardeparen. De naam van het teken moet als &quot;{{my.name}}&quot; worden geformatteerd.

De runtime parameter specificeert wanneer om de campagne in werking te stellen. Als gespecificeerd niet, zal de campagne 5 minuten nadat het eindpunt is geroepen in werking worden gesteld. De datetime-waarde mag in de toekomst niet meer dan twee jaar bedragen.

Campagnes die via deze API zijn gepland, wachten altijd minimaal vijf minuten voordat ze worden uitgevoerd.

De tekenreeksparameter `cloneToProgram` bevat de naam van een resulterend programma.  Wanneer deze optie is ingesteld, worden de campagne, het bovenliggende programma en alle elementen ervan gemaakt met de resulterende nieuwe naam. Het bovenliggende programma is gekloond en de nieuwe campagne wordt gepland. Het resulterende programma wordt onder het bovenliggende onderdeel gemaakt. Programma&#39;s met fragmenten, pushmeldingen, in-app berichten, statische lijsten, rapporten en sociale elementen worden mogelijk niet op deze manier gekloond. Wanneer gebruikt, is dit eindpunt beperkt tot 20 vraag per dag. Het [ kloonprogramma ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) eindpunt is het geadviseerde alternatief.

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

Gebruik het [ eindpunt van de Campagne van het Verzoek ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/triggerCampaignUsingPOST) om een reeks lood tot een trekkercampagne over te gaan om door de stroom van de campagne te lopen. De campagne moet een &quot;Campagne wordt Gevraagd&quot;trekker met &quot;de Dienst API van het Web&quot;als bron hebben.

Dit eindpunt vereist een campagne `id` als padparameter, en een `leads` integer-arrayparameter die loodid bevat. Een maximum van 100 lood wordt toegestaan per vraag.

Optioneel kan de arrayparameter `tokens` worden gebruikt om My Tokens lokaal te negeren in het bovenliggende programma van de campagne. `tokens` accepteert maximaal 100 tokens. Elk `tokens` -arrayitem bevat een naam/waardepaar. De naam van het teken moet als &quot;{{my.name}}&quot; worden geformatteerd. Als u [ gebruikt voeg een Symbolisch van het Systeem als Verbinding in een E-mail ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/add-a-system-token-as-a-link-in-an-email) benadering toe om het &quot;viewAsWebpageLink&quot;systeemteken toe te voegen, kunt u niet het met voeten treden gebruikend `tokens`. In plaats daarvan gebruik [ voegt een Mening als Verbinding van de Pagina van het Web aan een E-mail ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-a-view-as-web-page-link-to-an-email) benadering toe die u toestaat om &quot;viewAsWebPageLink&quot;met voeten te treden gebruikend `tokens`.

De parameters `leads` en `tokens` worden in de aanvraagtekst doorgegeven als application/json.

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

Het [ activeert Slimme 1&rbrace; eindpunt van de Campagne &lbrace;is ongecompliceerd. ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/activateSmartCampaignUsingPOST) Een padparameter `id` is vereist. Activering is alleen succesvol als de volgende waarden gelden voor de campagne:

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

[ Deactivate Slimme Campagne ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deactivateSmartCampaignUsingPOST) is ongecompliceerd. Een padparameter `id` is vereist. De deactivering kan alleen slagen als de campagne is geactiveerd.

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
