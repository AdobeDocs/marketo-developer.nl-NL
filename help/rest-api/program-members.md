---
title: "Programmaleden"
feature: REST API
description: "Maak en beheer programmaleden."
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '1712'
ht-degree: 0%

---


# Programmaleden

[Eindpuntverwijzing van programmaleden](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members)

Marketo stelt API&#39;s beschikbaar voor het lezen, maken, bijwerken en verwijderen van de records voor de leden van het programma. De dossiers van het programmalid zijn verwant met loodverslagen via het lood - identiteitskaart gebied. De records bestaan uit een set standaardvelden en eventueel uit maximaal 20 extra aangepaste velden. De velden bevatten programmaspecifieke gegevens voor elk lid en kunnen worden gebruikt in formulieren, filters, triggers en flowhandelingen. Deze gegevens kunnen worden weergegeven in de [Tabblad Leden](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/manage-and-view-members) in de gebruikersinterface van het Marketo Engage.

## Beschrijven

De [Lid van programma beschrijven](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) volgt het standaardpatroon voor databaseobjecten voor leads. De `searchableFields` -array geeft u de reeks velden die geldig zijn voor het opvragen. De `fields` array bevat veldmetagegevens, zoals de naam van de REST API, de weergavenaam en de updatemogelijkheid van het veld.

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

De [Programmaleden ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMembersUsingGET) eindpunt staat u toe om leden van een programma terug te winnen. Hiervoor is een `programId` padparameter, en `filterType` en `filterValues` queryparameters.

`programId` wordt gebruikt om aan te geven welk programma moet worden doorzocht.

`filterType` wordt gebruikt om aan te geven welk veld moet worden gebruikt als zoekfilter. Hiermee worden alle velden in de lijst &quot;doorzoekbareFields&quot; geaccepteerd die door de [Lid van programma beschrijven](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) eindpunt. Als u een filterType opgeeft dat een aangepast veld is, moet het dataType van het aangepaste veld &#39;string&#39; of &#39;integer&#39; zijn. Als u een filterType buiten &quot;leadId&quot;specificeert, kan een maximum van 100.000 verslagen van het programmalid door het verzoek worden verwerkt. Afhankelijk van de configuratie van uw Marketo-instantie ontvangt u een van de volgende fouten:

- Als het totale aantal programmaleden meer dan 100.000 bedraagt, wordt een fout geretourneerd: &quot;1003, Totale lidmaatschapsgrootte: 100.001 overschrijdt de toegestane limiet van 100.000 voor het filter&quot;.
- Indien het totale aantal programmaleden _die overeenkomen met het filter_ is groter dan 100.000, er wordt een fout geretourneerd: &quot;1003, Matching membership size: 100.001 overschrijdt de toegestane limiet (100.000) voor deze api&quot;.

Als u een query wilt uitvoeren op een programma waarvan het aantal leden de limiet overschrijdt, gebruikt u de opdracht [Extraheren-API voor leden van het Bulkprogramma](bulk-program-member-extract.md) in plaats daarvan.

`filterValues` wordt gebruikt om op te geven naar welke waarden moet worden gezocht en accepteert maximaal 300 waarden in een komma-gescheiden indeling. De vraag zoekt naar verslagen waar het gebied van het programmalid één van inbegrepen filterValues aanpast.

U kunt ook filteren op datumbereik door `updatedAt` als filterType met `startAt` en `endAt` datetime-parameters. Het bereik moet zeven dagen of minder zijn. Datumtijden moeten een ISO-8601-indeling hebben, zonder milliseconden.

De optionele `fields` queryparameter accepteert een door komma&#39;s gescheiden lijst met veld-API-namen die door de [Lid van programma beschrijven](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) eindpunt. Wanneer deze worden opgenomen, bevat elke record in het antwoord de opgegeven velden. Als u deze waarde weglaat, wordt de standaardset geretourneerde velden gebruikt `acquiredBy`, `leadId`, `membershipDate`, `programId`, en `reachedSuccess`.

Standaard worden maximaal 300 records geretourneerd. U kunt de `batchSize` query parameter om dit aantal te verminderen. Als de **moreResult** Het kenmerk is true, dit betekent dat er meer resultaten beschikbaar zijn. Ga door met het aanroepen van dit eindpunt tot de eigenschap moreResult false retourneert, wat betekent dat er geen resultaten beschikbaar zijn. De `nextPageToken` is teruggekeerd van deze API zou altijd voor de volgende herhaling van deze vraag moeten worden opnieuw gebruikt.

Als de totale lengte van uw GET- verzoek 8KB overschrijdt, is een fout van HTTP teruggekeerd: &quot;414, URI te lang&quot; (per [RFC 7231](https://datatracker.ietf.org/doc/html/rfc72316.5.12)). Als tijdelijke oplossing kunt u uw GET wijzigen in POST, toevoegen `_method=GET` parameter, en plaats vraagkoord in het verzoeklichaam.

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

De [Status van programmalid synchroniseren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberStatusUsingPOST) eindpunt wordt gebruikt om de programmastatus voor één of meerdere leden tot stand te brengen of bij te werken.

De vereiste `programId` path parameter specifies the program containing members to create or update.

De vereiste `statusName` parameter geeft de status van het programma aan die op een lijst met leads moet worden toegepast. De statusName moet overeenkomen met een beschikbare status voor het kanaal van het programma. Geldige statussen kunnen worden opgehaald met de opdracht [Kanalen ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Channels/operation/getAllChannelsUsingGET) eindpunt. Als de status van een lead een grotere stapwaarde heeft dan de opgegeven statusName, wordt die lead overgeslagen.

De vereiste `input` parameter is een array van `leadId` die overeenkomen met de programmaleden. U kunt tot 300 leadIds per vraag voorleggen. Voor elke record wordt een upsertbewerking uitgevoerd. Als leadId aan een programmalid wordt geassocieerd, dan wordt zijn lidmaatschapsstatus bijgewerkt. Als niet, wordt een nieuw verslag van het programmalid gecreeerd, wordt het verslag geassocieerd met leadId, en de lidmaatschapsstatus wordt toegewezen.

Het eindpunt reageert met a `status` van &quot;bijgewerkt&quot;, &quot;gemaakt&quot; of &quot;overgeslagen&quot;. Indien overgeslagen, `reasons` array wordt ook opgenomen. Het eindpunt zal ook met a reageren `seq` veld dat een index is die kan worden gebruikt om de ingediende records te correleren met de volgorde van de reactie.

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

De [Gegevens programmalid synchroniseren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberDataUsingPOST) het eindpunt wordt gebruikt om de gegevens van het programmalid voor één of meerdere leden bij te werken. U kunt elk aangepast veld of elke standaardveld wijzigen dat &quot;kan worden bijgewerkt&quot; (zie [Lid van programma beschrijven](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) eindpunt).

De vereiste `programId` path parameter specifies the program containing members to update.

De vereiste `input` parameter is een array. Elk arrayelement bevat een `leadId` en een of meer velden die moeten worden bijgewerkt (met gebruik van de API-naam). Voor elke record wordt een updatebewerking uitgevoerd. De leadId moet aan een programmalid worden geassocieerd. De velden moeten kunnen worden bijgewerkt. U kunt tot 300 leadIds per vraag voorleggen.

Het eindpunt reageert met a `status` van &quot;bijgewerkt&quot; of &quot;overgeslagen&quot;. Indien overgeslagen, `reasons` array wordt ook opgenomen. Het eindpunt zal ook met a reageren `seq` veld dat een index is die kan worden gebruikt om de ingediende records te correleren met de volgorde van de reactie.

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

Het programmalidobject bevat standaardvelden en optionele aangepaste velden. In elk abonnement op een Marketo Engage staan standaardvelden, terwijl de gebruiker aangepaste velden maakt. Elke velddefinitie bestaat uit een set kenmerken die het veld beschrijven. Voorbeelden van kenmerken zijn weergavenaam, API-naam en dataType. Deze kenmerken worden gezamenlijk metagegevens genoemd.

Met de volgende eindpunten kunt u velden in het programmalidobject opvragen, maken en bijwerken. Deze API&#39;s vereisen dat de eigenaar-API-gebruiker een rol heeft met een van beide **Standaardveld voor schema-lezen** of **Aangepast veld schema voor lezen/schrijven** machtigingen.

### Query-velden

De velden voor het opvragen van programmaleden zijn eenvoudig. U kunt één veld voor een programmalid opvragen op API-naam of de set met alle velden voor programmaleden opvragen. Zowel standaardvelden als aangepaste velden kunnen worden opgehaald, afhankelijk van de rolmachtigingen die worden gebruikt. Verborgen velden worden ook opgehaald.

#### Op naam

De [Veld voor programmalid ophalen op naam](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldByNameUsingGET) het eindpunt wint meta-gegevens voor één enkel gebied op het voorwerp van het programmalid terug. De vereiste `fieldApiName` path parameter specifies the API name of the field. De reactie is als het beschrijf het eindpunt van het Lid van het Programma maar bevat extra meta-gegevens zoals `isCustom` kenmerk dat aangeeft of het veld een aangepast veld is.

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

De [Veld voor programmalid ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldsUsingGET) het eindpunt wint meta-gegevens voor alle gebieden op het voorwerp van het programmalid terug. Standaard worden maximaal 300 records geretourneerd. U kunt de `batchSize` query parameter om dit aantal te verminderen. Als de `moreResult` Het kenmerk is true, dit betekent dat er meer resultaten beschikbaar zijn. Ga door met het aanroepen van dit eindpunt tot de eigenschap moreResult false retourneert, wat betekent dat er geen resultaten beschikbaar zijn. De `nextPageToken` is teruggekeerd van deze API zou altijd voor de volgende herhaling van deze vraag moeten worden opnieuw gebruikt.

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

De [Veld voor programmaleden maken](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/createProgramMemberFieldUsingPOST) het eindpunt leidt tot één of meerdere douanegebieden op het voorwerp van het programmalid. Dit eindpunt verstrekt functionaliteit die vergelijkbaar is met wat is [beschikbaar in de gebruikersinterface van het Marketo Engage](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/program-member-custom-fields). Met dit eindpunt kunt u maximaal 20 aangepaste velden maken.

Houd zorgvuldig rekening met elk veld dat u maakt in de productie-instantie van een Marketo Engage met behulp van de API. Nadat een veld is gemaakt, kunt u het niet verwijderen ([u kunt het alleen verbergen](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/field-management/delete-a-custom-field-in-marketo)). De proliferatie van ongebruikte gebieden is een slechte praktijk die uw geval zal bemoeilijken.

De vereiste `input` parameter is een array van veldobjecten van het programmalid. Elk object bevat een of meer kenmerken. De vereiste kenmerken zijn `displayName`, `name`, en `dataType` die overeenkomen met de weergavenaam van de gebruikersinterface van het veld, de API-naam van het veld en het veldtype. U kunt desgewenst opgeven `description`, `isHidden`, `isHtmlEncodingInEmail`, en `isSensitive`.

Er zijn een paar regels verbonden aan `name` en `displayName` naamgeving. De `name` Het kenmerk moet uniek zijn, beginnen met een letter en mag alleen letters, cijfers of onderstrepingsteken bevatten. De *`isplayName` moet uniek zijn en mag geen speciale tekens bevatten. Een gemeenschappelijke naamgevingsconventie is van toepassing [kamelenkast](https://en.wikipedia.org/wiki/Camel_case#) tot `displayName` produceren `name`. Bijvoorbeeld een `displayName` van &quot;Mijn aangepaste veld&quot; zou een `name` van &quot;myCustomField&quot;.

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

De [Veld van programmalid bijwerken](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/updateProgramMemberFieldUsingPOST) het eindpunt werkt één enkel douanegebied op het voorwerp van het programmalid bij. Over het algemeen zijn bewerkingen voor veldupdates die worden uitgevoerd met de interface van het Marketo Engage haalbaar met de API. In de onderstaande tabel zijn enkele verschillen samengevat.

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

De vereiste `fieldApiName` path parameter specifies the API name of the field to update. De vereiste `input` parameter is een array die één hoofdobject bevat. Het veldobject bevat een of meer kenmerken.

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

De [Programmeleden verwijderen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/deleteProgramMemberUsingPOST) het eindpunt wordt gebruikt om de verslagen van het programmalid te schrappen. De vereiste `programId` path parameter specifies the program containing members to delete. De verzoekende instantie bevat een `input` array van loodhoudende id&#39;s. Per oproep mogen maximaal 300 lead id&#39;s worden gebruikt.

Het eindpunt reageert met a `status` van &quot;verwijderde&quot; of &quot;overgeslagen&quot;. Indien overgeslagen, `reasons` array wordt ook opgenomen. Het eindpunt zal ook met a reageren `seq` veld dat een index is die kan worden gebruikt om de ingediende records te correleren met de volgorde van de reactie.

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
