---
title: "Programma's"
feature: REST API, Programs
description: "Programmagegevens maken en bewerken."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 0%

---


# Programma&#39;s

[Referentie voor eindpunt van programma&#39;s](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs)

Programma&#39;s vormen een centrale organisatorische component van Marketo Marketing Activiteiten. Zij kunnen een ouder aan de meeste soorten activa zijn, en voor het volgen van lidmaatschap en succes van lood in de context van individuele marketing initiatieven. De programma&#39;s kunnen ouders aan elk type van verslagen behalve LP, E-mailMalplaatjes, en Dossiers zijn.

## Programmatypen

Er zijn vijf kerntypen programma&#39;s in Marketo:

- Standaard
- Gebeurtenis
- Gebeurtenis met webinar
- Betrokkenheid
- E-mail

Betrokkenheidsprogramma&#39;s kunnen bovenliggende programma&#39;s zijn voor elk ander type programma, terwijl Standaard, Gebeurtenis en Gebeurtenis met webinar alleen bovenliggende programma&#39;s van e-mailprogramma&#39;s kunnen zijn.

Programma&#39;s hebben altijd een kanaal, ze halen de mogelijke oprichtingsprogramma-lidstaten af van het kanaal waarmee ze zijn gemaakt, dat kan worden opgehaald met de API voor kanalen ophalen. Een programma kan ook een set bijbehorende tags hebben. Tags zijn aanpasbare velden die kunnen worden geconfigureerd als optioneel of vereist voor een bepaald type programma. In deze velden wordt een waarde geselecteerd uit een lijst die is geconfigureerd in Marketo Admin.

## Query

De programma&#39;s volgen het standaardpatroon voor activa vragen met een extra optie om door markeringstype en waarden te vragen. Beschikbare tags en waarden kunnen worden opgehaald met [Tagtypen ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags/operation/getTagTypesUsingGET).

### Op id

De [Programma ophalen op id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) eindpunt vereist een `id` padparameter.

De programma-id kan worden verkregen via de URL van het programma in de gebruikersinterface, waar de URL lijkt `https://app-\*\*\*.marketo.com/#PG1001A1`. In deze URL, `id` is 1001. Deze zal altijd tussen de eerste set letters in de URL en de tweede set letters liggen.

```
GET /rest/asset/v1/program/{id}.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#14db037ec71",
    "result": [
        {
            "id": 1107,
            "name": "AAA2QueryProgramName",
            "description": "AssetAPI: getProgram tests",
            "createdAt": "2015-05-21T22:45:13Z+0000",
            "updatedAt": "2015-05-21T22:45:13Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1107A1",
            "type": "Default",
            "channel": "Online Advertising",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "",
            "workspace": "Default",
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                }
            ],
            "costs": null,
            "headStart": false
        }
    ]
}
```

### Op naam

De [Programma ophalen op naam](https://developer.adobe.com/marketo-apis/api/asset/) eindpunt vereist `name` queryparameter. Optionele Booleaanse queryparameters zijn `includeTags` en `includeCosts` die worden gebruikt voor het retourneren van programmalabels en programmakosten.

```
GET /rest/asset/v1/program/byName.json?name=TestProgramName&includeTags=true
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#14db03e070c",
    "result": [
        {
            "id": 1107,
            "name": "AAA2QueryProgramName",
            "description": "AssetAPI: getProgram tests",
            "createdAt": "2015-05-21T22:45:13Z+0000",
            "updatedAt": "2015-05-21T22:45:13Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1107A1",
            "type": "Default",
            "channel": "Online Advertising",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "",
            "workspace": "Default",
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                }
            ],
            "costs": null,
            "headStart": false
        }
    ]
}
```

### Bladeren

De [Programma&#39;s ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) het eindpunt staat u toe om voor programma&#39;s te doorbladeren.

De optionele `status` kunt u filteren op de status van het programma. Deze parameter is alleen van toepassing op betrokkenheid- en e-mailprogramma&#39;s. De mogelijke waarden zijn &quot;aan&quot; en &quot;uit&quot; voor betrokkenheidsprogramma&#39;s en &quot;ontgrendeld&quot; voor e-mailprogramma&#39;s.

De optionele `maxReturn` parameter bepaalt het aantal programma&#39;s dat moet worden geretourneerd (maximum is 200, standaardwaarde is 20). De optionele `offset` parameter gebruikt voor het pagineren van resultaten (gebrek is 0).

Merk op dat de markeringen verbonden aan een programma niet door dit eindpunt worden teruggekeerd. Programmatags kunnen worden opgehaald met een van de volgende [Programma&#39;s ophalen op id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET) of [Programma&#39;s ophalen op naam](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET).

```
GET /rest/asset/v1/programs.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "7a39#1511bf8a41c",
    "result": [
        {
            "id": 1035,
            "name": "clone it",
            "description": "",
            "createdAt": "2015-11-18T15:25:35Z+0000",
            "updatedAt": "2015-11-18T15:25:46Z+0000",
            "url": "https://app-devlocal1.marketo.com/#NP1035A1",
            "type": "Engagement",
            "channel": "Nurture",
            "folder": {
                "type": "Folder",
                "value": 28,
                "folderName": "Nurturing"
            },
            "status": "on",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1032,
            "name": "email prog",
            "description": "",
            "createdAt": "2015-11-18T14:56:28Z+0000",
            "updatedAt": "2015-11-18T14:56:28Z+0000",
            "url": "https://app-devlocal1.marketo.com/#EBP1032A1",
            "type": "Email",
            "channel": "Email Send",
            "folder": {
                "type": "Folder",
                "value": 26,
                "folderName": "Data Management"
            },
            "status": "unlocked",
            "workspace": "Default",
            "headStart": false
        }
    ]
}
```

### Op datumbereik

De `earliestUpdatedAt` en `latestUpdatedAt` parameters voor onze [Programma&#39;s ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) Met het eindpunt kunt u lage en hoge datetime watermerken instellen voor het retourneren van programma&#39;s die zijn bijgewerkt of oorspronkelijk binnen het opgegeven bereik zijn gemaakt.

```
GET /rest/asset/v1/programs.json?earliestUpdatedAt=2017-01-01T00:00:00-05:00&latestUpdatedAt=2017-01-30T00:00:00-05:00
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1225a#15f82a83875",
    "warnings": [],
    "result": [
        {
            "id": 1070,
            "name": "Bulk Import - Test",
            "description": "",
            "createdAt": "2017-01-13T19:34:17Z+0000",
            "updatedAt": "2017-01-13T19:34:18Z+0000",
            "url": "https://app-abm.marketo.com/#PG1070A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 637,
                "folderName": "Avention"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1069,
            "name": "Program With Email",
            "description": "",
            "createdAt": "2017-01-03T22:53:14Z+0000",
            "updatedAt": "2017-01-03T22:53:15Z+0000",
            "url": "https://app-abm.marketo.com/#EBP1069A1",
            "type": "Email",
            "channel": "Email Send",
            "folder": {
                "type": "Folder",
                "value": 621,
                "folderName": "Smartling"
            },
            "status": "unlocked",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1071,
            "name": "Program with Guided Landing Page Template",
            "description": "",
            "createdAt": "2017-01-24T22:59:21Z+0000",
            "updatedAt": "2017-01-24T22:59:22Z+0000",
            "url": "https://app-abm.marketo.com/#PG1071A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 621,
                "folderName": "Smartling"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1047,
            "name": "ReachForce List Update",
            "description": "",
            "createdAt": "2016-05-24T19:38:35Z+0000",
            "updatedAt": "2017-01-13T19:28:09Z+0000",
            "url": "https://app-abm.marketo.com/#PG1047A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 407,
                "folderName": "Everly Tests"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        }
    ]
}
```

### Op tagtype

De [Programma&#39;s ophalen op tag](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramListByTagUsingGET) Hiermee wordt een lijst opgehaald met programma&#39;s die overeenkomen met het opgegeven tagtype en de opgegeven tagwaarden.

Er zijn twee vereiste parameters. `tagType` het type tag waarop moet worden gefilterd, en `tagValue` Dit is de tagwaarde waarop moet worden gefilterd.  Er is een optioneel geheel getal `maxReturn` parameter die het aantal programma&#39;s bepaalt om terug te keren (maximum is 200, gebrek is 20), en een facultatief geheel `offset` parameter gebruikt voor het pagineren van resultaten (gebrek is 0).  Resultaten worden in willekeurige volgorde geretourneerd.

```
GET /rest/asset/v1/program/byTag.json?tagType=Presenter&tagValue=Dennis
```

```json
{
    "success" : true,
    "warnings" : [],
    "errors" : [],
    "requestId" : "13b6d#152b38d5be4",
    "result" : [{
            "id" : 1004,
            "name" : "It's a Program",
            "description" : "",
            "createdAt" : "2013-02-26T00:37:37Z+0000",
            "updatedAt" : "2013-03-11T15:32:02Z+0000",
            "url" : "https://app-sjst.marketo.com/#PG1004A1",
            "type" : "Default",
            "channel" : "Email Blast",
            "folder" : {
                "type" : "Folder",
                "value" : 38,
                "folderName" : "Test"
            },
            "status" : "",
            "workspace" : "Default",
            "tags" : [{
                    "tagType" : "Presenter",
                    "tagValue" : "Dennis"
                }
            ],
                        "headStart": false
    ]
}
```

## Maken en bijwerken

[Maken]https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/createProgramUsingPOST) en [bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) programma&#39;s volgens het standaard assetpatroon en `folder`, `name`, `type` en `channel` zoals voorgeschreven parameters, met `description`, `costs` en `tags` facultatief zijn. Kanaal en type mogen alleen worden ingesteld bij het maken van programma&#39;s. Alleen beschrijving, naam, `tags` en `costs` kan na het maken worden bijgewerkt met een extra `costsDestructiveUpdate` parameter toegestaan. Passend `costsDestructiveUpdate` zoals true , zullen alle bestaande kosten worden vereffend en vervangen door alle kosten die in de uitnodiging zijn opgenomen . Merk op dat de markeringen voor sommige programmatypes in sommige abonnementen kunnen worden vereist, maar dit is configuratie-afhankelijk en zou eerst met Get Markeringen moeten worden gecontroleerd om te zien of zijn er instantie-specifieke vereisten.

Wanneer u een e-mailprogramma maakt of bijwerkt, kunt u `startDate` en `endDate` kan ook worden goedgekeurd.

### Maken

```
POST /rest/asset/v1/programs.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=API Test Program&folder={"id":1035,"type":"Folder"}&description=Sample API Program&type=Default&channel=Email Blast&costs=[{"startDate":"2015-01-01","cost":2000}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "d505#14d9bd96352",
    "result": [
        {
            "id": 1207,
            "name": "newProgram",
            "description": "This is a test",
            "createdAt": "2015-05-28T18:47:15Z+0000",
            "updatedAt": "2015-05-28T18:47:15Z+0000",
            "url": "https://app-devlocal1.marketo.com/#ME1207A1",
            "type": "Event",
            "channel": "channelOne",
            "folder": {
                "type": "Folder",
                "value": 59,
                "folderName": "blah blah"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
            "tags": null,
            "costs": [
                {
                    "startDate":"2015-01-01",
                    "cost":2000
                }
            ]
        }
    ]
}
```

### Bijwerken

Als u programmalasten bijwerkt en nieuwe kosten wilt toevoegen, voegt u deze gewoon toe aan uw `costs` array. Als u een destructieve update wilt uitvoeren, geeft u uw nieuwe kosten door, samen met de parameter `costsDestructiveUpdate` instellen op `true`. Als u alle kosten van een programma wilt wegnemen, moet u `costs` parameter, en pas enkel over `costsDestructiveUpdate` instellen op `true`.

```
POST /rest/asset/v1/program/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
description=This is an updated description&name=Updated Program Name&costs=[{"startDate":"2016-01-01","cost":200,"note":"Google Adwords"}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5c37#14db05608aa",
    "result": [
        {
            "id": 1110,
            "name": "Updated Program Name",
            "description": "This is a updated description",
            "createdAt": "2015-05-21T22:45:14Z+0000",
            "updatedAt": "2015-06-01T18:13:58Z+0000",
            "url": "https://app-devlocal1.marketo.com/#NP1110A1",
            "type": "Engagement",
            "channel": "Nurture",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "on",
            "workspace": "Default",
            "headStart": false,
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                },
                {
                    "tagType": "tagTypeOne",
                    "tagValue": "tagTypeValue1"
                }
            ],
            "costs": [
                {
                    "startDate": "2016-01-01",
                    "cost": 200,
                    "note": "Google Adwords"
                }
            ]
        }
    ]
}
```

## Goedkeuring

E-mailprogramma&#39;s kunnen op afstand worden goedgekeurd of niet-goedgekeurd, waardoor het programma op de opgegeven startdatum wordt uitgevoerd en op de opgegeven einddatum wordt afgesloten. Beide moeten worden ingesteld om het programma goed te keuren en moeten beschikken over een geldige en goedgekeurde e-mail en slimme lijst die via de gebruikersinterface zijn geconfigureerd.

### Goedkeuren

```
POST /rest/asset/v1/program/{id}/approve.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#150b5bf7692",
    "result": [
        {
            "id": 11062
        }
    ]
}
```

### Niet goedkeuren

```
POST /rest/asset/v1/program/{id}/unapprove.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#150b5bf7692",
    "result": [
        {
            "id": 11062
        }
    ]
}
```

## Klonen

[Kloonprogramma&#39;s](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/cloneProgramUsingPOST) volgt het standaardelementpatroon de nieuwe naam en map zoals vereist en een optionele beschrijving.  De `name` parameter moet globaal uniek zijn en mag niet langer zijn dan 255 tekens.  De `folder` parameter is de bovenliggende map.  De `folder` parametertype-kenmerk moet zijn ingesteld op &quot;Map&quot; en de doelmap moet zich in dezelfde werkruimte bevinden als het programma dat wordt gekloond.

Programma&#39;s die bepaalde typen elementen bevatten, worden mogelijk niet via deze API gekloond, zoals pushberichten, In-App-berichten, rapporten en sociale middelen. Programma&#39;s in de app worden mogelijk niet gekloond via deze API.

```
POST /rest/asset/v1/program/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Cloned Program - PHP&folder={"id":5562,"type":"Folder"}&description=Description
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "3a7f#14db06990cc",
    "result": [
        {
            "id": 1221,
            "name": "cloneProgram",
            "description": "This is a description for the cloned program",
            "createdAt": "2015-06-01T18:36:57Z+0000",
            "updatedAt": "2015-06-01T18:36:57Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1221A1",
            "type": "Default",
            "channel": "Blog",
            "folder": {
                "type": "Folder",
                "value": 59,
                "folderName": "blah blah"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
            "tags": null,
            "costs": null
        }
    ]
}
```

## Programma verwijderen

Als u programma&#39;s verwijdert, volgt u het standaardpatroon voor het verwijderen van elementen.

```
POST /rest/asset/v1/program/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16501#14db042c6b7",
    "result": [
        {
            "id": 1109
        }
    ]
}
```
