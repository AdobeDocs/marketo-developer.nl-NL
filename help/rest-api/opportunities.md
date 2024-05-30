---
title: "Opportunity"
feature: REST API
description: " Configureer mogelijkheden met de Marketo API."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 0%

---


# Kansen

[Opportunity Endpoint Reference](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities)

Marketo stelt API&#39;s beschikbaar voor het lezen, schrijven, maken en bijwerken van opportuniteitsrecords. In Marketo zijn opportuniteitsrecords gekoppeld aan hoofd- en contactrecords via het tussentijdse object Opportunity Role. Een opportuniteit kan dus aan vele individuele leads worden gekoppeld.  Beide objecttypen worden via de API beschikbaar gemaakt en hebben, net als de meeste objecttypen voor databases met leads, beide een bijbehorende beschrijvingsaanroep, die metagegevens over de objecttypen retourneert.

Opportunity-API&#39;s zijn alleen-lezen toegang voor abonnementen die [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) of [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) zijn ingeschakeld.

## Beschrijven

Bij het beschrijven van opportunityrecords wordt het standaardpatroon voor databaseobjecten voor leads gevolgd.

```
GET /rest/v1/opportunities/describe.json
```

```json
{  
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[  
      {  
         "name":"opportunity",
         "displayName":"Opportunity",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[  
            "externalOpportunityId"
         ],
         "searchableFields":[  
            [  
               "externalOpportunityId"
            ],
            [  
               "marketoGUID"
            ]
         ],
         "fields":[  
            {  
               "name":"marketoGUID",
               "displayName":"Marketo GUID",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {  
               "name":"createdAt",
               "displayName":"Created At",
               "dataType":"datetime",
               "updateable":false
            },
            {  
               "name":"updatedAt",
               "displayName":"Updated At",
               "dataType":"datetime",
               "updateable":false
            },
            {  
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            }
         ]
      }
   ]
}
```

De belangrijkste velden voor dit type reactie zijn `idField`, `dedupeFields`, en `searchableFields`.  idField wijst op de primaire sleutel voor kansen, marketoGUID.  Dit is een systeem produceerde unieke sleutel, die voor gelezen en updateverrichtingen, maar niet voor tussenvoegsels kan worden gebruikt, aangezien het systeem wordt beheerd.  De array dedupeFields geeft aan welke velden geldige sleutels zijn voor invoegbewerkingen. In het geval van opportunity is dit alleen externalOpportunityId.  De searchFields serie geeft u de reeks gebieden die voor het vragen, externalOpportunityId en marketoGUID geldig zijn.

## Query

Het patroon voor [zoeken naar mogelijkheden](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunitiesUsingGET) volgt de API voor leads nauwlettend met de toegevoegde beperking die de `filterType` parameter accepteert de velden die worden vermeld in de `searchableFields` array of van de corresponderende beschrijf aanroep, of dedupeFields.  Als u aangepaste opportuniteitsvelden gebruikt, worden alleen aangepaste opportuniteitsvelden van het type String of Integer vermeld in een doorzoekbareFields-array.

```
GET /rest/v1/opportunities.json?filterType=marketoGUID&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
{  
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[  
      {  
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa ",
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {  
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc ",
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

U kunt ook de optionele queryparameters opnemen `fields`, voor het terugsturen van extra opportuniteitsvelden, `nextPageToken`, voor het doorbladeren door sets die groter zijn dan de batchgrootte, `batchSize`, met een standaardwaarde van en een maximum van 300.  Bij het aanvragen van een lijst met `fields`Als een bepaald veld wordt opgevraagd, maar niet wordt geretourneerd, wordt de waarde impliciet ingesteld op null.

## Maken en bijwerken

De kansen volgen het lood API patroon, met sommige beperkingen.  De beschikbare waarden voor `action` zijn: createOnly, createOrUpdate en updateOnly.  Wanneer u de modus createOnly of createOrUpdate gebruikt, moet het veld externalOpportunityId in elke record worden opgenomen.  Voor de updateOnly wijze, of marketoGUID of externalOpportunityId kan worden gebruikt.  De modus is standaard ingesteld op createOrUpdate als deze niet is opgegeven.

De `lookupField` parameter van de API voor leads is niet beschikbaar en wordt vervangen door de parameter dedupeBy, die alleen geldig is als de handeling updateOnly is.  De waarden beschikbaar voor dedupeBy zijn of &quot;dedupeFields&quot;of &quot;idField&quot;die door worden gespecificeerd beschrijven vraag als externalOpportunityId en marketoGUID respectievelijk.  Als dedupeBy niet gespecificeerd is, blijft het aan dedupeFields wijze in gebreke.  Het veld Naam mag niet null zijn.

U kunt maximaal 300 records tegelijk verzenden.

```
POST /rest/v1/opportunities.json
```

```json
{  
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[  
      {  
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {  
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

```json
{  
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[  
      {  
         "seq":0,
         "status":"updated",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {  
         "seq":1,
         "status":"created",
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

De API reageert met de `marketoGUID` voor elke record, alsmede een `status` veld, met vermelding van het individuele succes of de mislukking van elke record, en een `seq` veld dat wordt gebruikt om de ingediende verslagen te correleren, met de volgorde van de reactie.  Het getal in het veld is de index van de record die in het verzoek is ingediend.

### Velden

Het bedrijfsobject bevat een set velden.  Elke velddefinitie bestaat uit een set kenmerken die het veld beschrijven.  Voorbeelden van kenmerken zijn weergavenaam, API-naam en dataType.  Deze kenmerken worden gezamenlijk metagegevens genoemd.

De volgende eindpunten staan u toe om gebieden op het bedrijfvoorwerp te vragen. Deze API&#39;s vereisen dat de eigenaar-API-gebruiker een rol heeft met een van beide `Read-Write Schema Standard Field` of `Read-Write Schema Custom Field` machtigingen.

### Query-velden

Velden met zoekmogelijkheden zijn eenvoudig.  U kunt één enkel bedrijfgebied door API naam vragen of de reeks van alle bedrijfgebieden vragen.

#### Op naam

De [Opportunity-veld op naam ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldByNameUsingGET) het eindpunt wint meta-gegevens voor één enkel gebied op het bedrijfvoorwerp terug.  De vereiste `fieldApiName` path parameter specifies the API name of the field.  De reactie is als het beschrijf eindpunt van de Kans maar bevat extra meta-gegevens zoals `isCustom` kenmerk dat aangeeft of het veld een aangepast veld is.

```
GET /rest/v1/opportunities/schema/fields/externalOpportunityId.json
```

```json
{
    "requestId": "12331#17e9779cb4b",
    "result": [
        {
            "displayName": "SFDC Oppty Id",
            "name": "externalOpportunityId",
            "description": null,
            "dataType": "string",
            "length": 50,
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

De [Opportunity-velden ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldsUsingGET) het eindpunt wint meta-gegevens voor alle gebieden op het bedrijfvoorwerp terug.  Standaard worden maximaal 300 records geretourneerd.  U kunt de `batchSize` query parameter om dit aantal te verminderen.  Als de `moreResult` Het kenmerk is true, dit betekent dat er meer resultaten beschikbaar zijn.  Ga door met het aanroepen van dit eindpunt tot de eigenschap moreResult false retourneert, wat betekent dat er geen resultaten beschikbaar zijn.  De `nextPageToken` is teruggekeerd van deze API zou altijd voor de volgende herhaling van deze vraag moeten worden opnieuw gebruikt.

```
GET /rest/v1/opportunities/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "b4a#17e995b31da",
    "result": [
        {
            "displayName": "SFDC Oppty Id",
            "name": "externalOpportunityId",
            "description": null,
            "dataType": "string",
            "length": 50,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Name",
            "name": "name",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Description",
            "name": "description",
            "description": null,
            "dataType": "string",
            "length": 2000,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Type",
            "name": "type",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Stage",
            "name": "stage",
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
    "nextPageToken": "E5ZONGE4SAHALYYW6FS25KB5BM======",
    "moreResult": true
}
```

#### Verwijderen

U kunt mogelijkheden verwijderen door velden of id-velden te dedupliceren. Opgeven met de opdracht `deleteBy` parameter met een waarde van dedupeFields of idField. Als deze optie niet is opgegeven, wordt standaard dedupeFields gebruikt. De verzoekende instantie bevat een `input` array met mogelijkheden om te verwijderen. Een maximum van 300 kansen per vraag wordt toegestaan.

```
POST /rest/v1/opportunities/delete.json
```

```json
{ 
   "deleteBy":"dedupeFields",
   "input":[ 
      { 
         "externalOpportunityId":"19UYA31581L000000"
      },
      { 
         "externalOpportunityId":"29UYA31581L000000"
      }
   ]
}
```

```json
{ 
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[ 
      { 
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status":"deleted"
      },
      { 
         "seq":1,
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb",
         "status":"deleted"
      }
   ]
}
```

## Tijdstippen

- De eindpunten van de kans hebben een onderbreking van 30 s tenzij hieronder vermeld
   - Synchronisatiemogelijkheden: 60 seconden 
   - Kansen verwijderen: 60 s
