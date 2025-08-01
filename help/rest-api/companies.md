---
title: Bedrijven
feature: REST API
description: Bedrijfsgegevens configureren met Marketo API's.
exl-id: 80e514a2-1c86-46a7-82bc-e4db702189b0
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Bedrijven

[ Verwijzing van het Eindpunt van Bedrijven ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies)

Bedrijven vertegenwoordigen de organisatie waartoe hoofdrecords behoren. De lood worden toegevoegd aan een Bedrijf door hun overeenkomstige `externalCompanyId` gebied te bevolken gebruikend [ Leads van de Synchronisatie ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) of [ BulkLood de 4&rbrace; eindpunten van de Invoer. ](bulk-lead-import.md) Nadat een lead aan een bedrijf is toegevoegd, kunt u de lead van dat bedrijf niet verwijderen (tenzij u de lead aan een ander bedrijf toevoegt). Leads die aan een bedrijfsrecord zijn gekoppeld, nemen de waarden van een bedrijfsrecord rechtstreeks over alsof de waarden in het eigen record van de lead staan.

Het bedrijf APIs is read-only toegang voor abonnementen die [ de Synchronisatie van SFDC ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) of [ Synchronisatie van Microsoft Dynamics ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) hebben wordt toegelaten.

## Beschrijven

Het beschrijven van het bedrijfvoorwerp geeft u alle informatie u met hen moet in wisselwerking staan.

```
GET /rest/v1/companies/describe.json
```

```json
{
   "success":true,
   "requestId":"5847#14d44113ad7",
   "result":[
      {
         "name":"Company",
         "description":"Company object",
         "createdAt":"2015-05-11T17:11:32Z",
         "updatedAt":"2015-05-11T17:11:32Z",
         "idField":"id",
         "dedupeFields":[
            "externalCompanyId"
         ],
         "searchableFields":[
            [
               "externalCompanyId"
            ],
            [
               "id"
            ],
            [
               "company"
            ]
         ],
         "fields":[
            {
               "name":"createdAt",
               "displayName":"Created At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"externalCompanyId",
               "displayName":"External Company Id",
               "dataType":"string",
               "length":100,
               "updateable":false
            },
            {
               "name":"id",
               "displayName":"Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"updatedAt",
               "displayName":"Updated At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"annualRevenue",
               "displayName":"Annual Revenue",
               "dataType":"currency",
               "updateable":true
            }
            {
               "name":"company",
               "displayName":"Company Name",
               "dataType":"string",
               "length":255,
               "updateable":true
            }
         ]
      }
   ]
}
```

## Query

Het patroon voor [ het vragen van bedrijven ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompaniesUsingGET) volgt dicht dat van lood API met de toegevoegde beperking die de `filterType` parameter de gebieden goedkeurt die in de searchFields serie van de beschrijf vraag van Bedrijven worden vermeld, of dedupeFields.

`filterType` en `filterValues` zijn vereiste queryparameters.  `fields` , `nextPageToken` en `batchSize` zijn optionele parameters.  De parameters werken net als de overeenkomstige parameters in de API&#39;s Leads en Opportunity. Wanneer u een lijst van `fields` aanvraagt, wordt de waarde impliciet null weergegeven als een bepaald veld wordt opgevraagd, maar niet wordt geretourneerd.

Als de parameter fields wordt weggelaten, is de standaardset geretourneerde velden:

- id
- dedupeFields
- updatedAt
- createdAt

```
GET /rest/v1/companies.json?filterType=id&filterValues=3433,5345
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "id":3433,
         "externalCompanyId":"19UYA31581L000000",
         "company":"Google"
      },
      {
         "seq":1,
         "id":5345,
         "externalCompanyId":"29UYA31581L000000",
         "company":"Yahoo"
      }
   ]
}
```

## Maken en bijwerken

Het [ eindpunt van de Bedrijven van de Synchronisatie ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) &lbrace;keurt de vereiste `input` parameter goed die een serie van bedrijfvoorwerpen bevat. Net als de mogelijkheden zijn er drie modi voor het maken en bijwerken van bedrijven: createOnly, updateOnly en createOrUpdate.  Modi worden opgegeven in de parameter `action` van de aanvraag. Zowel de parameters `dedupeBy` als `action` zijn optioneel en worden standaard ingesteld op respectievelijk de modi dedupeFields en createOrUpdate.

```
POST /rest/v1/companies.json
```

```
Content-Type: application/json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalCompanyId":"19UYA31581L000000",
         "company":"Google"
      },
      {
         "externalCompanyId":"29UYA31581L000000",
         "company":"Yahoo"
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
         "id":1232
      },
      {
         "seq":1,
         "status":"created",
         "id":1323
      }
   ]
}
```

### Velden

Het bedrijfsobject bevat een set velden. Elke velddefinitie bestaat uit een set kenmerken die het veld beschrijven. Voorbeelden van kenmerken zijn weergavenaam, API-naam en dataType. Deze kenmerken worden gezamenlijk metagegevens genoemd.

De volgende eindpunten staan u toe om gebieden op het bedrijfvoorwerp te vragen. Deze API&#39;s vereisen dat de eigenaar-API-gebruiker een rol heeft met een of beide `Read-Write Schema Standard Field` - of `Read-Write Schema Custom Field` -machtigingen.

### Query-velden

Het vragen van bedrijfgebieden is ongecompliceerd. U kunt één enkel bedrijfgebied door API naam vragen of de reeks van alle bedrijfgebieden vragen.

#### Op naam

Het [ krijgt Veld van het Bedrijf door het eindpunt van de Naam ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldByNameUsingGET) wint meta-gegevens voor één enkel gebied op het bedrijfvoorwerp terug. De vereiste `fieldApiName` padparameter geeft de API-naam van het veld op. De reactie is als beschrijf het eindpunt van het Bedrijf maar bevat extra meta-gegevens zoals `isCustom` attributen die erop wijzen of het gebied een douanegebied is.

```
GET /rest/v1/companies/schema/fields/industry.json
```

```json
{
    "requestId": "88f6#17e976d6ab4",
    "result": [
        {
            "displayName": "Industry",
            "name": "industry",
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

Het [ krijgt het eindpunt van de Velden van het Bedrijf ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldsUsingGET) wint meta-gegevens voor alle gebieden op het bedrijfvoorwerp terug. Standaard worden maximaal 300 records geretourneerd. U kunt de query-parameter `batchSize` gebruiken om dit aantal te verlagen. Als het kenmerk `moreResult` true is, zijn er meer resultaten beschikbaar. Ga door met het aanroepen van dit eindpunt tot de eigenschap moreResult false retourneert, wat betekent dat er geen resultaten beschikbaar zijn. De `nextPageToken` die door deze API wordt geretourneerd, moet altijd opnieuw worden gebruikt voor de volgende herhaling van deze aanroep.

```
GET /rest/v1/companies/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "b50e#17e995c2d35",
    "result": [
        {
            "displayName": "Company Name",
            "name": "company",
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
            "displayName": "Site",
            "name": "site",
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
            "displayName": "Website",
            "name": "website",
            "description": null,
            "dataType": "url",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Main Phone",
            "name": "mainPhone",
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
            "displayName": "Annual Revenue",
            "name": "annualRevenue",
            "description": null,
            "dataType": "currency",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "L7XD3EFJ3OLFZKXKJBWYULOTRA======",
    "moreResult": true
}
```

### Verwijderen

De verwijderingscriteria worden opgegeven in de array `input` , die een lijst met zoekwaarden bevat.  De verwijderingsmethode wordt opgegeven in de parameter `deleteBy` .  Toegestane waarden zijn: dedupeFields, idField.  Standaard is dedupeFields.

```
Content-Type: application/json
```

```
POST /rest/v1/companies/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "externalCompanyId":"19UYA31581L000000"
      },
      {
         "externalCompanyId":"29UYA31581L000000"
      },
      {
         "externalCompanyId":"39UYA31581L000000"
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
         "id":1234,
         "status":"deleted"
      },
      {
         "seq":1,
         "id":56456,
         "status":"deleted"
      },
      {
         "seq":2,
         "status":"skipped",
         "reasons":[
            {
               "code":"1013",
               "message":"Record not found"
            }
         ]
      }
   ]
}
```

## Tijdstippen

- Eindpunten van bedrijven hebben een time-out van 30 seconden, tenzij hieronder vermeld
   - Bedrijven synchroniseren: 60 seconden 
   - Bedrijven verwijderen: 60 seconden
