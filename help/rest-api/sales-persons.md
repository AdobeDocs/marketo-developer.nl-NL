---
title: Verkopers
feature: REST API
description: Lees gegevens over verkooppersonen.
exl-id: f8ed5aa5-63c1-4c5b-8683-bf47eed1ea18
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---

# Verkopers

[ Verwijzing van het Eindpunt van de Verkoper ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Sales-Persons)

De Persoon van de verkoop APIs is read-only toegang voor abonnementen die [ de Synchronisatie van SFDC ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) of [ Synchronisatie van Microsoft Dynamics ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) hebben wordt toegelaten. Verkooppersonen zijn een soort persoonrecord die de eigenaars van loodrecords zijn. Zij zijn verwant met verslagen van het Lood door het externalSalesPersonId gebied op elk Loodverslag. Wanneer een lead aan een verkooppersoon is gerelateerd door een ingevuld veld externalSalesPersonId, worden de desbetreffende opzoekvelden van de eigenaar van de lead in Marketo ingevuld, zodat de bijbehorende filters en tokens kunnen worden gebruikt.

De Personen van de verkoop zijn verwant met de verslagen van het Lood door de [ Synchronisatie te gebruiken leidt ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) eindpunt en het overgaan van het externalSalesPersonId attribuut.

De Personen van de verkoop zijn verwant met de verslagen van de Kans van de Kans van de Kans van de Kans van de Kans van de Kans door het [ eindpunt van de Synchronisatie te gebruiken en het externalSalesPersonId attribuut over te gaan.](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/syncOpportunitiesUsingPOST)

De Personen van de verkoop zijn verwant met de verslagen van het Bedrijf door het [ eindpunt van de Bedrijven van de Synchronisatie te gebruiken ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) en het overgaan van het externalSalesPersonId attribuut.

De verslagen van de Persoon van de verkoop zijn slechts editable via API.

## Beschrijven

Bij de beschrijving van de records van Verkooppersoon wordt het standaardpatroon voor hoofddatabaseobjecten gevolgd.

```
GET /rest/v1/salespersons/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"SalesPerson",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"id",
         "dedupeFields":[
            "externalSalesPersonId"
         ],
         "searchableFields":[
            [
               "email"
            ],
            [
               "id"
            ],
            [
               "externalSalesPersonId"
            ]
         ],
         "fields":[
            {
               "name":"id",
               "displayName":"Marketo Id",
               "dataType":"integer",
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
               "name":"email",
               "displayName":"Email",
               "dataType":"string",
               "length":255,
               "updateable":false
            },
            {
               "name":"externalSalesPersonId",
               "displayName":"External Sales Person Id",
               "dataType":"string",
               "length":255,
               "updateable":false
            }
         ]
      }
   ]
}
```

Standaard is `idField` van Verkooppersonen &quot;id&quot; en is `dedupeFields` alleen &quot;externalSalesPersonId&quot;.

## Query

De Personen van de verkoop die het standaardvraagpatroon voor eenvoudige sleutels gebruiken. In dit voorbeeld wordt de e-mail van de gebruiker weergegeven die als externalSalesPersonId wordt gebruikt. Standaard retourneert de query alle velden die zijn ingevuld voor de geretourneerde records.

```
GET /rest/v1/salespersons.json?filterType=dedupeFields&filterValues=david@test.com,sam@test.com
```

```json
 {
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "id":53453,
         "externalSalesPersonId":"sam@test.com",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:23Z"
      },
      {
         "seq":1,
         "id":53454,
         "externalSalesPersonId":"david@test.com",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:23Z"
      }
   ]
}
```

## Maken en bijwerken

Het patroon voor updates is standaard.

```
POST /rest/v1/salespersons.json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalSalesPersonId":"sam@test.com",
         "email":"sam@test.com",
         "firstName":"Sam",
         "lastName":"Sanosin"
      },
      {
         "externalSalesPersonId":"david@test.com",
         "email":"david@test.com",
         "firstName":"David",
         "lastName":"Aulassak"
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
         "status": "updated",
         "id":45232
      },
      {
         "seq":1,
         "status": "created",
         "id":45236
      }
   ]
}
```

## Verwijderen

Het patroon voor verwijderen is standaard.

Verwijderen van Verkooppersonen is niet toegestaan wanneer zij &quot;in gebruik&quot; zijn. In dit geval wordt de verkoper overgeslagen. Voorbeelden:

- Wanneer de Verkoper met actieve Leads wordt geassocieerd
- Wanneer de Persoon van de Verkoop met een Bedrijf wordt geassocieerd dat is geschrapt

```
POST /rest/v1/salespersons/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "externalSalesPersonId":"sam@test.com"
      },
      {
         "externalSalesPersonId":"david@test.com"
      },
      {
         "externalSalesPersonId":"raj@test.com"
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
         "id":56343,
         "status": "deleted"
      },
      {
         "seq":1,
         "id":53453,
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
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

- Eindpunten van verkopers hebben een time-out van 30 seconden, tenzij hieronder vermeld
   - Verkoopmedewerkers synchroniseren: 60 seconden
   - Verkopers verwijderen: 60 s
