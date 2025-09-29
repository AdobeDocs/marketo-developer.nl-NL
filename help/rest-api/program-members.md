---
title: Programmaleden
feature: REST API
description: Met de Marketo REST API kunt u leden van programma's lezen, maken, bijwerken en verwijderen, standaard- en aangepaste velden beheren en query's uitvoeren met behulp van doorzoekbare velden.
exl-id: 22f29a42-2a30-4dce-a571-d7776374cf43
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1726'
ht-degree: 0%

---

# Programmaleden

[ Verwijzing van het Eindpunt van de Leden van het Programma ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members)

Marketo stelt API&#39;s beschikbaar voor het lezen, maken, bijwerken en verwijderen van de records voor de leden van het programma. De dossiers van het programmalid zijn verwant met loodverslagen via het lood - identiteitskaart gebied. De records bestaan uit een set standaardvelden en eventueel uit maximaal 20 extra aangepaste velden. De velden bevatten programmaspecifieke gegevens voor elk lid en kunnen worden gebruikt in formulieren, filters, triggers en flowhandelingen. Dit gegeven is viewable in het 0&rbrace; LEDEN van het programma [ Landen in Marketo Engage UI.](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/manage-and-view-members)

## Beschrijven

[ beschrijf het Lid van het Programma ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) eindpunt volgt het standaardpatroon voor loodgegevensbestandvoorwerpen. De array `searchableFields` geeft u de reeks velden die geldig zijn om te vragen. De array `fields` bevat veldmetagegevens, zoals de naam van de REST API, de weergavenaam en de updatemogelijkheid van velden.

```
GET /rest/v1/programs/members/describe.json
```

```json
{
    "requestId": "f813#1791563c7cc",
    "result": [
        {
            "name": "API Program Membership",
            "description": "Map for API program membership fields",
            "createdAt": "2021-03-20T01:30:05Z",
            "updatedAt": "2021-03-20T01:30:05Z",
            "dedupeFields": [
                "leadId",
                "programId"
            ],
            "searchableFields": [
                [
                    "leadId"
                ],
                [
                    "myCustomField"
                ],
                [
                    "reachedSuccess"
                ],
                [
                    "statusName"
                ]
            ],
            "fields": [
                {
                    "name": "acquiredBy",
                    "displayName": "acquiredBy",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "attendanceLikelihood",
                    "displayName": "attendanceLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "createdAt",
                    "displayName": "createdAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "isExhausted",
                    "displayName": "isExhausted",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "leadId",
                    "displayName": "leadId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "membershipDate",
                    "displayName": "membershipDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "nurtureCadence",
                    "displayName": "nurtureCadence",
                    "dataType": "string",
                    "length": 4,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "program",
                    "displayName": "program",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "programId",
                    "displayName": "programId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccess",
                    "displayName": "reachedSuccess",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccessDate",
                    "displayName": "reachedSuccessDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "registrationLikelihood",
                    "displayName": "registrationLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusName",
                    "displayName": "statusName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusReason",
                    "displayName": "statusReason",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "trackName",
                    "displayName": "trackName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "updatedAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "waitlistPriority",
                    "displayName": "waitlistPriority",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "myCustomField",
                    "displayName": "myCustomField",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "registrationCode",
                    "displayName": "registrationCode",
                    "dataType": "string",
                    "length": 100,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "webinarUrl",
                    "displayName": "webinarUrl",
                    "dataType": "string",
                    "length": 2000,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

## Query

Het [ krijgt 1&rbrace; eindpunt van de Leden van het Programma &lbrace;staat u toe om leden van een programma terug te winnen. ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMembersUsingGET) Hiervoor zijn een parameter `programId` path en queryparameters `filterType` en `filterValues` vereist.

`programId` wordt gebruikt om op te geven welk programma moet worden doorzocht.

`filterType` wordt gebruikt om op te geven welk veld moet worden gebruikt als zoekfilter. Het keurt om het even welk gebied in de &quot;doorzoekableFields&quot;lijst goed die door [ is teruggekeerd beschrijft het Lid van het Programma ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) eindpunt. Als u een filterType opgeeft dat een aangepast veld is, moet het dataType van het aangepaste veld &#39;string&#39; of &#39;integer&#39; zijn. Als u een filterType buiten &quot;leadId&quot;specificeert, kan een maximum van 100.000 verslagen van het programmalid door het verzoek worden verwerkt. Afhankelijk van de configuratie van uw Marketo-instantie ontvangt u een van de volgende fouten:

- Als het totale aantal programmaleden meer dan 100.000 bedraagt, wordt een fout geretourneerd: &quot;1003, Totale lidmaatschapsgrootte: 100.001 overschrijdt de toegestane limiet van 100.000 voor het filter&quot;.
- Als het totale aantal programmaleden _dat de filter_ aanpast 100.000 overschrijdt, is een fout teruggekeerd: &quot;1003, het Aanpassen lidmaatschapsgrootte: 100.001 overschrijdt de toegestane grens (100.000) voor deze api&quot;.

Om een programma te vragen de waarvan lidmaatschapstelling de grens overschrijdt, gebruik in plaats daarvan het [ BulkLid van het Programma API van het Uittreksel ](bulk-program-member-extract.md).

`filterValues` wordt gebruikt om op te geven naar welke waarden moet worden gezocht en accepteert maximaal 300 waarden in een door komma&#39;s gescheiden indeling. De vraag zoekt naar verslagen waar het gebied van het programmalid één van inbegrepen filterValues aanpast.

U kunt ook filteren op datumbereik door `updatedAt` op te geven als filterType met de datetime-parameters `startAt` en `endAt` . Het bereik moet zeven dagen of minder zijn. Datumtijden moeten een ISO-8601-indeling hebben, zonder milliseconden.

De facultatieve `fields` vraagparameter keurt een komma-gescheiden lijst van gebied API namen goed die door [ zijn teruggekeerd beschrijf 2&rbrace; eindpunt van het Lid van het Programma. ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) Wanneer deze worden opgenomen, bevat elke record in het antwoord de opgegeven velden. Als u deze waarde weglaat, wordt de standaardset met velden `acquiredBy` , `leadId` , `membershipDate` , `programId` en `reachedSuccess` geretourneerd.

Standaard worden maximaal 300 records geretourneerd. U kunt de query-parameter `batchSize` gebruiken om dit aantal te verlagen. Als het **moreResult** attribuut waar is, betekent dit meer resultaten beschikbaar zijn. Ga door met het aanroepen van dit eindpunt tot de eigenschap moreResult false retourneert, wat betekent dat er geen resultaten beschikbaar zijn. De `nextPageToken` die door deze API wordt geretourneerd, moet altijd opnieuw worden gebruikt voor de volgende herhaling van deze aanroep.

Als de totale lengte van uw GET-aanvraag groter is dan 8 kB, wordt een HTTP-fout geretourneerd: &quot;414, URI te lang&quot;. Als tussenoplossing kunt u GET wijzigen in POST, parameter `_method=GET` toevoegen en een queryreeks in de hoofdtekst van de aanvraag plaatsen.

```
GET /rest/v1/programs/{programId}/members.json?filterType=statusName&filterValues=Influenced
```

```json
{
    "requestId": "109da#17915eec072",
    "result": [
        {
            "seq": 0,
            "leadId": 1789,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 1,
            "leadId": 1790,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 2,
            "leadId": 1791,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 3,
            "leadId": 1792,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 4,
            "leadId": 1793,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 5,
            "leadId": 1794,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 6,
            "leadId": 1795,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 7,
            "leadId": 1796,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 8,
            "leadId": 1797,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 9,
            "leadId": 1798,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 10,
            "leadId": 1799,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 11,
            "leadId": 1800,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        }
    ],
    "success": true,
    "moreResult": false
}
```

## Maken en bijwerken

Er zijn twee eindpunten die het maken/bijwerken van verrichting op programmaleden steunen. Met een van deze statussen kunt u alleen de status van een programmalid bijwerken. Met de andere functie kunt u de set met velden voor programmaleden bijwerken die zijn gemarkeerd als &#39;updateable&#39;. Beide eindpunten staan u toe om tot 300 verslagen van het programmalid per vraag te wijzigen.

### Status van programmalid

Het [ eindpunt van de Status van het Lid van het Programma van de Synchronisatie ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberStatusUsingPOST) wordt gebruikt om de programmastatus voor één of meerdere leden tot stand te brengen of bij te werken.

De vereiste `programId` padparameter geeft het programma op met leden die moeten worden gemaakt of bijgewerkt.

De vereiste parameter `statusName` geeft de status van het programma aan die moet worden toegepast op een lijst met leads. De statusName moet overeenkomen met een beschikbare status voor het kanaal van het programma. De geldige statussen kunnen worden teruggewonnen gebruikend [ krijgt het eindpunt van Kanalen ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Channels/operation/getAllChannelsUsingGET). Als de status van een lead een grotere stapwaarde heeft dan de opgegeven statusName, wordt die lead overgeslagen.

De vereiste parameter `input` is een array van `leadId` die overeenkomt met de programmeerleden. U kunt tot 300 leadIds per vraag voorleggen. Voor elke record wordt een upsertbewerking uitgevoerd. Als leadId aan een programmalid wordt geassocieerd, dan wordt zijn lidmaatschapsstatus bijgewerkt. Als niet, wordt een nieuw verslag van het programmalid gecreeerd, wordt het verslag geassocieerd met leadId, en de lidmaatschapsstatus wordt toegewezen.

Het eindpunt reageert met een `status` van &quot;bijgewerkt&quot;, &quot;gemaakt&quot; of &quot;overgeslagen&quot;. Als deze wordt overgeslagen, wordt ook een array `reasons` opgenomen. Het eindpunt zal ook met een `seq` gebied antwoorden dat een index is die kan worden gebruikt om de voorgelegde verslagen aan de orde van de reactie te correleren.

Als de vraag succesvol is, wordt een &quot;activiteit van de Status van het Programma van de Verandering&quot;geschreven aan het de activiteitenlogboek van de lood.

```
POST /rest/v1/programs/{programId}/members/status.json
```

```
Content-Type: application/json
```

```json
{
    "statusName":"Influenced",
    "input":[
        {
            "leadId": 1800
        },
        {
            "leadId": 1801
        },
        {
            "leadId": 1235
        }
    ]
}
```

```json
{
    "requestId": "14b2d#17916378ec5",
    "result": [
        {
            "seq": 0,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1037",
                    "message": "Lead skipped because it is already in or past this status"
                }
            ]
        },
        {
            "seq": 1,
            "status": "updated",
            "leadId": 1801
        },
        {
            "seq": 2,
            "status": "created",
            "leadId": 1235
        }
    ],
    "success": true
}
```

### Gegevens van programmalid

Het [ eindpunt van de Gegevens van het Lid van het Programma van de Synchronisatie ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberDataUsingPOST) wordt gebruikt om de gegevens van het programmalid voor één of meerdere leden bij te werken. U kunt om het even welk douanegebied wijzigen, of standaardgebieden die &quot;updateable&quot;zijn (zie [ beschrijven Lid van het Programma ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) eindpunt).

De vereiste `programId` padparameter geeft het programma op met leden die moeten worden bijgewerkt.

De vereiste parameter `input` is een array. Elk arrayelement bevat een `leadId` en een of meer velden die moeten worden bijgewerkt (met behulp van de API-naam). Voor elke record wordt een updatebewerking uitgevoerd. De leadId moet aan een programmalid worden geassocieerd. De velden moeten kunnen worden bijgewerkt. U kunt tot 300 leadIds per vraag voorleggen.

Het eindpunt reageert met een `status` van &quot;bijgewerkt&quot; of &quot;overgeslagen&quot;. Als deze wordt overgeslagen, wordt ook een array `reasons` opgenomen. Het eindpunt zal ook met een `seq` gebied antwoorden dat een index is die kan worden gebruikt om de voorgelegde verslagen aan de orde van de reactie te correleren.

Als de vraag succesvol is, wordt een activiteit van het &quot;Lid van het Programma van de Verandering&quot;geschreven aan het activiteitenlogboek van de lood.

```
POST /rest/v1/programs/{programId}/members.json
```

```
Content-Type: application/json
```

```json
{
    "input":[
        {
            "leadId": 1789,
            "registrationCode": "dcff5f12-a7c7-11eb-bcbc-0242ac130002"
        },
        {
            "leadId": 1790,
            "registrationCode": "c0404b78-d3fd-47bf-82c4-d16f3852ab3a"
        },
        {
            "leadId": 1003,
            "registrationCode": "aa880c57-75b8-426b-a33a-fbf6302d7cb4"
        }
    ]
}
```

```json
{
    "requestId": "edc3#1791659b8d2",
    "result": [
        {
            "seq": 0,
            "status": "updated",
            "leadId": 1789
        },
        {
            "seq": 1,
            "status": "updated",
            "leadId": 1790
        },
        {
            "seq": 2,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1013",
                    "message": "Membership not found"
                }
            ]
        }
    ],
    "success": true
}
```

## Velden

Het programmalidobject bevat standaardvelden en optionele aangepaste velden. Alle Marketo Engage-abonnementen bevatten standaardvelden, terwijl de gebruiker aangepaste velden maakt. Elke velddefinitie bestaat uit een set kenmerken die het veld beschrijven. Voorbeelden van kenmerken zijn weergavenaam, API-naam en dataType. Deze kenmerken worden gezamenlijk metagegevens genoemd.

Met de volgende eindpunten kunt u velden in het programmalidobject opvragen, maken en bijwerken. Deze APIs vereisen dat de het bezitten API gebruiker een rol met één of allebei van het **lezen-schrijven StandaardGebied van het Schema** of **lezen-schrijven het Gebied van de Douane van het Schema** toestemmingen heeft.

### Query-velden

De velden voor het opvragen van programmaleden zijn eenvoudig. U kunt één veld voor een programmalid opvragen op API-naam of de set met alle velden voor programmaleden opvragen. Zowel standaardvelden als aangepaste velden kunnen worden opgehaald, afhankelijk van de rolmachtigingen die worden gebruikt. Verborgen velden worden ook opgehaald.

#### Op naam

Het [ krijgt Gebied van het Lid van het Programma door het eindpunt van de Naam ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldByNameUsingGET) wint meta-gegevens voor één enkel gebied op het voorwerp van het programmalid terug. De vereiste `fieldApiName` padparameter geeft de API-naam van het veld op. De reactie is als het beschrijf het eindpunt van het Lid van het Programma maar bevat extra meta-gegevens zoals `isCustom` attributen die erop wijzen of het gebied een douanegebied is.

```
GET /rest/v1/programs/members/schema/fields/{fieldApiName}.json
```

```json
{
    "requestId": "15416#17e955554de",
    "result": [
        {
            "displayName": "Status",
            "name": "statusName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true
}
```

#### Bladeren

Het [ krijgt de Eind van de Velden van het Lid van het Programma ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldsUsingGET) terugwint meta-gegevens voor alle gebieden op het voorwerp van het programmalid. Standaard worden maximaal 300 records geretourneerd. U kunt de query-parameter `batchSize` gebruiken om dit aantal te verlagen. Als het kenmerk `moreResult` true is, zijn er meer resultaten beschikbaar. Ga door met het aanroepen van dit eindpunt tot de eigenschap moreResult false retourneert, wat betekent dat er geen resultaten beschikbaar zijn. De `nextPageToken` die door deze API wordt geretourneerd, moet altijd opnieuw worden gebruikt voor de volgende herhaling van deze aanroep.

```
GET /rest/v1/programs/members/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "102f6#17e9557f123",
    "result": [
        {
            "displayName": "Acquired By",
            "name": "acquiredBy",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Nurture Cadence",
            "name": "nurtureCadence",
            "description": null,
            "dataType": "string",
            "length": 4,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Nurture Exhausted",
            "name": "isExhausted",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Member Date",
            "name": "membershipDate",
            "description": null,
            "dataType": "datetime",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Program",
            "name": "program",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "BC7J6EPVLT6T4B5FKUU3APCYN4======",
    "moreResult": true
}
```

### Velden maken

Het [ creeert de Eind van het Lid van het Programma ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/createProgramMemberFieldUsingPOST) leidt tot één of meerdere douanegebieden op het voorwerp van het programmalid. Dit eindpunt verstrekt functionaliteit die aan vergelijkbaar is wat [ beschikbaar in Marketo Engage UI ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/program-member-custom-fields) is. Met dit eindpunt kunt u maximaal 20 aangepaste velden maken.

Houd zorgvuldig rekening met elk veld dat u met de API maakt in de productie-instantie van Marketo Engage. Zodra een gebied is gecreeerd, kunt u niet het schrappen ([ u het ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/field-management/delete-a-custom-field-in-marketo) slechts kunt verbergen). De proliferatie van ongebruikte gebieden is een slechte praktijk die uw geval zal bemoeilijken.

De vereiste parameter `input` is een array van veldobjecten van programmalidden. Elk object bevat een of meer kenmerken. Vereiste kenmerken zijn de `displayName` , `name` en `dataType` die overeenkomen met respectievelijk de weergavenaam van de gebruikersinterface van het veld, de API-naam van het veld en het veldtype. U kunt optioneel `description` , `isHidden` , `isHtmlEncodingInEmail` en `isSensitive` opgeven.

Er zijn een paar regels verbonden aan `name` en `displayName` het noemen. Het kenmerk `name` moet uniek zijn, beginnen met een letter en alleen letters, cijfers of onderstrepingsteken bevatten. * `isplayName` moet uniek zijn, en kan geen speciale karakters bevatten. Een gemeenschappelijke noemende overeenkomst moet [ camel geval ](https://en.wikipedia.org/wiki/Camel_case#) op `displayName` toepassen om `name` te produceren. Een `displayName` van &quot;Mijn aangepaste veld&quot; zou bijvoorbeeld een `name` van &quot;myCustomField&quot; produceren.

```
POST /rest/v1/programs/members/schema/fields.json
```

```json
{
  "input": [
    {
        "displayName": "PMCF Custom Field 03",
        "name": "pMCFCustomField03",
        "description": "My third custom field",
        "dataType": "string"
    }
  ]
}
```

```json
{
    "requestId": "13a7#17e955fcb44",
    "result": [
        {
            "name": "pMCFCustomField03",
            "status": "created"
        }
    ],
    "success": true
}
```

### Veld bijwerken

Het [ eindpunt van het Lid van het Programma van de Update ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/updateProgramMemberFieldUsingPOST) werkt één enkel douanegebied op het voorwerp van het programmalid bij. Over het algemeen zijn bewerkingen voor veldupdates die worden uitgevoerd met de gebruikersinterface van Marketo Engage haalbaar met de API. In de onderstaande tabel zijn enkele verschillen samengevat.

| Kenmerk | Kan worden bijgewerkt door API? | Kan worden bijgewerkt via gebruikersinterface? | Kan worden bijgewerkt door API? | Kan worden bijgewerkt via gebruikersinterface? |
|---|---|---|---|---|
| dataType | nee | nee | nee | ja |
| beschrijving | ja | ja | ja | ja |
| displayName | nee | nee | ja | ja |
| isCustom | nee | nee | nee | nee |
| isHidden | nee | ja | yes (indien gemaakt door API) | ja |
| isHtmlEncodingInEmail | ja | ja | ja | ja |
| isSensitive | ja | ja | ja | ja |
| length | nee | nee | nee | nee |
| name | nee | nee | nee | nee |

De vereiste `fieldApiName` padparameter geeft de API-naam op van het veld dat moet worden bijgewerkt. De vereiste parameter `input` is een array die één hoofdobject in het veld bevat. Het veldobject bevat een of meer kenmerken.

```
POST /rest/v1/programs/members/schema/fields/pMCFCustomField03.json
```

```json
{
  "input": [
      {
        "displayName": "Lunch Preference",
        "description": "Attendee food preference",
        "isHtmlEncodingInEmail": true
      }
  ]
}
```

```json
{
    "requestId": "215f#17e95663955",
    "result": [
        {
            "name": "pMCFCustomField03",
            "status": "updated"
        }
    ],
    "success": true
}
```

## Verwijderen

Het [ punt van de Leden van het Programma van de Schrapping ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/deleteProgramMemberUsingPOST) wordt gebruikt om de verslagen van het programmalid te schrappen. De vereiste `programId` padparameter geeft het programma op dat leden bevat die moeten worden verwijderd. De aanvraaghoofdtekst bevat een `input` -array met loodid. Maximaal 300 loodhoudende stoffen  per oproep zijn toegestaan.

Het eindpunt reageert met een `status` van &quot;removed&quot; of &quot;overgeslagen&quot;. Als deze wordt overgeslagen, wordt ook een array `reasons` opgenomen. Het eindpunt zal ook met een `seq` gebied antwoorden dat een index is die kan worden gebruikt om de voorgelegde verslagen aan de orde van de reactie te correleren.

```
POST /rest/v1/programs/{programId}/members/delete.json
```

```
Content-Type: application/json
```

```json
{
    "input":[
        {
            "leadId": 1235
        },
        {
            "leadId": 77
        }
    ]
}
```

```json
{
    "requestId": "302a#17916619417",
    "result": [
        {
            "seq": 0,
            "status": "deleted",
            "leadId": 1235
        },
        {
            "seq": 1,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1037",
                    "message": "Lead not in program"
                }
            ]
        }
    ],
    "success": true
}
```
