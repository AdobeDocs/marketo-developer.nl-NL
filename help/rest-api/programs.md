---
title: Programma's
feature: REST API, Programs
description: De handleiding Marketo-programma's voor de REST-API voor middelen, waarin de typen, kanalen, tags, lidstatussen en eindpunten worden beschreven die op id of naam worden opgehaald, worden doorzocht en op status worden gefilterd.
exl-id: 30700de2-8f4a-4580-92f2-7036905deb80
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '870'
ht-degree: 0%

---

# Programma&#39;s

[ Referentie van het Eindpunt van Programma&#39;s ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs)

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

De programma&#39;s volgen het standaardpatroon voor activa vragen met een extra optie om door markeringstype en waarden te vragen. De beschikbare markeringen en de waarden kunnen met [ worden teruggewonnen krijgen de Types van Markering ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags/operation/getTagTypesUsingGET).

### Op id

Het [ krijgt Programma door identiteitskaart ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) eindpunt vereist een `id` wegparameter.

De programma-id kan worden verkregen via de URL van het programma in de gebruikersinterface, waar de URL op `https://app-\*\*\*.marketo.com/#PG1001A1` lijkt. In deze URL is `id` 1001. Deze zal altijd tussen de eerste set letters in de URL en de tweede set letters liggen.

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

Het [ krijgt Programma door het 1} eindpunt van de Naam {vereist een ](https://developer.adobe.com/marketo-apis/api/asset/) vraagparameter. `name` Optionele Booleaanse queryparameters zijn `includeTags` en `includeCosts` die worden gebruikt om programmalabels en programmakosten te retourneren.

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

Het [ krijgt het 1} eindpunt van Programma&#39;s {staat u toe om voor programma&#39;s te doorbladeren.](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5)

Met de optionele parameter `status` kunt u filteren op de status van het programma. Deze parameter is alleen van toepassing op betrokkenheid- en e-mailprogramma&#39;s. De mogelijke waarden zijn &quot;aan&quot; en &quot;uit&quot; voor betrokkenheidsprogramma&#39;s en &quot;ontgrendeld&quot; voor e-mailprogramma&#39;s.

De optionele parameter `maxReturn` bepaalt het aantal programma&#39;s dat moet worden geretourneerd (het maximum is 200, de standaardwaarde is 20). De optionele parameter `offset` die wordt gebruikt voor het pagineren van resultaten (standaardwaarde is 0).

Merk op dat de markeringen verbonden aan een programma niet door dit eindpunt worden teruggekeerd. De markeringen van het programma kunnen worden teruggewonnen door of [ te gebruiken krijgen Programma&#39;s door Identiteitskaart ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET) of [ krijgt Programma&#39;s door Naam ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET).

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

De `earliestUpdatedAt` en `latestUpdatedAt` parameters aan ons [ krijgen het eindpunt van Programma&#39;s ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) staat u toe om lage en hoge datetime watermerken voor het terugkeren van programma&#39;s te plaatsen die of werden bijgewerkt of aanvankelijk binnen de bepaalde waaier gecreeerd.

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

[ krijgt Programma&#39;s door Eind van de Markering ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramListByTagUsingGET) eindpunt wint een lijst van programma&#39;s terug die het markeringstype en de markeringswaarden aanpassen verstrekt.

Er zijn twee vereiste parameters, `tagType` (het type tag waarop moet worden gefilterd) en `tagValue` (de tagwaarde waarop moet worden gefilterd).  Er is een optionele parameter integer `maxReturn` die het aantal programma&#39;s bepaalt dat moet worden geretourneerd (maximum is 200, standaardwaarde is 20) en een optionele parameter integer `offset` die wordt gebruikt voor het pagineren van resultaten (standaardwaarde is 0).  Resultaten worden in willekeurige volgorde geretourneerd.

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

[ Creërend ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/createProgramUsingPOST) en [ het bijwerken ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) programma&#39;s volgt het standaardmiddelpatroon en heeft `folder`, `name`, `type` en `channel` als vereiste parameters, met `description`, `costs` en `tags` facultatief zijn. Kanaal en type mogen alleen worden ingesteld bij het maken van programma&#39;s. Alleen beschrijving, naam, `tags` en `costs` kunnen na het maken worden bijgewerkt, met een extra parameter `costsDestructiveUpdate` toegestaan. Als u `costsDestructiveUpdate` als true doorgeeft, worden alle bestaande kosten gewist en vervangen door alle kosten die in de oproep zijn opgenomen. Merk op dat de markeringen voor sommige programmatypes in sommige abonnementen kunnen worden vereist, maar dit is configuratie-afhankelijk en zou eerst met Get Markeringen moeten worden gecontroleerd om te zien of zijn er instantie-specifieke vereisten.

Wanneer u een e-mailprogramma maakt of bijwerkt, kunnen een `startDate` en `endDate` ook als UTC-datum/tijd worden doorgegeven:

`"startDate": "2022-10-19T15:00:00.000Z"`
`"endDate": "2022-10-19T15:00:00.000Z"`

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

Wanneer u programmalasten bijwerkt en nieuwe kosten toevoegt, voegt u deze toe aan uw array `costs` . Als u een destructieve update wilt uitvoeren, geeft u de nieuwe kosten door, samen met de parameter `costsDestructiveUpdate` die is ingesteld op `true` . Als u alle kosten van een programma wilt wissen, geeft u geen parameter `costs` door en geeft u `costsDestructiveUpdate` gewoon door aan `true` .

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

[ het Klonen programma&#39;s ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/cloneProgramUsingPOST) volgt het standaardelementpatroon de nieuwe naam en de omslag zoals vereiste parameters en een facultatieve beschrijving.  De parameter `name` moet globaal uniek zijn en mag niet langer zijn dan 255 tekens.  De parameter `folder` is de bovenliggende map.  Het parametertype `folder` moet worden ingesteld op &quot;Map&quot; en de doelmap moet zich in dezelfde werkruimte bevinden als het programma dat wordt gekloond.

Programma&#39;s die bepaalde typen elementen bevatten, worden mogelijk niet via deze API gekloond, zoals pushberichten, In-App-berichten, rapporten en sociale Assets. Programma&#39;s in de app worden mogelijk niet gekloond via deze API.

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
