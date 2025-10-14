---
title: Opportuniteitsrollen
feature: REST API
description: U kunt Marketo-opportuniteitsrollen beheren via REST API, zoals beschrijvingen, query's met samengestelde gededupliceerde velden, update verwijderen, time-outs en geen CRM-synchronisatie maken.
exl-id: 2ba84f4d-82d0-4368-94e8-1fc6d17b69ed
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Opportuniteitsrollen

[&#x200B; Verwijzing van het Eindpunt van de Rollen van de Kans &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityRolesUsingGET)

Leads worden gekoppeld aan mogelijkheden via het tussenliggende `opportunityRole` -object.

Opportunity Role API&#39;s worden alleen weergegeven voor abonnementen waarvoor geen native CRM-synchronisatie is ingeschakeld.

## Beschrijven

Als kansen, beschrijven vraag en de verrichtingen CRUD worden blootgesteld aan opportuniteitsrollen.

```
GET /rest/v1/opportunities/roles/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"opportunityRole",
         "displayName":"Opportunity Role",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "externalOpportunityId",
            "leadId",
            "role"
         ],
         "searchableFields":[
            [
               "externalOpportunityId",
               "leadId",
               "role"
            ],
            [
               "marketoGUID"
            ],
            [
               "leadId"
            ],
            [
               "externalOpportunityId"
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
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"leadId",
               "displayName":"Lead Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"role",
               "displayName":"Role",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"isPrimary",
               "displayName":"Is Primary",
               "dataType":"boolean",
               "updateable":true
            },
            {
               "name":"externalCreatedDate",
               "displayName":"External Created Date",
               "dataType":"datetime",
               "updateable":true
            }
         ]
      }
   ]
}
```

## Query

`dedupeFields` en `searchableFields` verschillen iets van kansen. `dedupeFields` biedt in feite een samengestelde sleutel, waarbij alle drie `externalOpportunityId` , `leadId` en `role` vereist zijn. Zowel moeten de opportuniteit als de leidende verbinding door de identiteitskaart- gebieden in de bestemmingsinstantie bestaan, voor verslagverwezenlijking om te slagen. `searchableFields` , `marketoGUID` , `leadId` en `externalOpportunityId` zijn allemaal geldig voor query&#39;s op zichzelf en gebruiken een patroon dat identiek is aan Opportunity, maar er is een extra optie om de samengestelde sleutel te gebruiken voor query. Hiervoor moet een JSON-object via POST worden verzonden, met de extra queryparameter `_method=GET` .

```
POST /rest/v1/opportunities/roles.json?_method=GET
```

```json
{
   "filterType": "dedupeFields",
   "fields": [
      "marketoGuid",
      "externalOpportunityId",
      "leadId",
      "role"
   ],
   "input": [
      {
        "externalOpportunityId": "Opportunity1",
        "leadId": 1,
        "role": "Captain"
      },
      {
        "externalOpportunityId": "Opportunity2",
        "leadId": 1872,
        "role": "Commander"
      },
      {
        "externalOpportunityId": "Opportunity3",
        "leadId": 273891,
        "role": "Lieutenant Commander"
      }
   ]
}
```

Dit veroorzaakt het zelfde type van reactie zoals een standaardvraag van GET, heeft het eenvoudig een verschillende interface voor het maken van het verzoek.

## Maken en bijwerken

De rollen van de kans hebben de zelfde interface voor het creëren van en het bijwerken van verslagen zoals kansen.

```
POST /rest/v1/opportunities/roles.json
```

```json
{
   "action": "createOrUpdate",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "externalOpportunityId": "19UYA31581L000000",
         "leadId": 456783,
         "role": "Technical Buyer",
         "isPrimary": false
      },
      {
         "externalOpportunityId": "19UYA31581L000000",
         "leadId": 456784,
         "role": "Technical Buyer",
         "isPrimary": false
      }
   ]
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result":[
      {
         "seq": 0,
         "status": "updated",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq": 1,
         "status": "created",
         "marketoGUID": "cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

## Verwijderen

U kunt opportuniteitsrollen verwijderen door velden te dedupliceren of het veld Id. Geef aan met behulp van de parameter deleteBy de waarde dedupeFields of idField. Als deze optie niet is opgegeven, wordt standaard dedupeFields gebruikt. De aanvraaginstantie bevat een invoerarray met opportuniteitsrollen die moeten worden verwijderd. Een maximum van 300 opportuniteitsrollen per vraag wordt toegestaan.

```
POST /rest/v1/opportunities/roles/delete.json
```

```json
{
   "deleteBy": "dedupeFields",
   "input": [
      {
        "externalOpportunityId": "19UYA31581L000000",
        "leadId": 456783,
        "role": "Technical Buyer"
      }
   ]
}
```

```json
{
    "requestId": "10f7c#173264db42d",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
            "status": "deleted"
        }
    ]
    "success": true
}
```

## Tijdstippen

- De eindpunten van de Rol van de kans hebben een onderbreking van 30 s tenzij hieronder vermeld
   - Opportuniteitsrollen synchroniseren: 60 seconden
   - Opportuniteitsrollen verwijderen: 60 seconden
