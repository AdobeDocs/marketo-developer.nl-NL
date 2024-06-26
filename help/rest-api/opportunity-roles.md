---
title: "Opportunity Roles"
feature: REST API
description: "Hanteer opportunityrollen in Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---


# Opportuniteitsrollen

[Opportunity Roles Endpoint Reference](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityRolesUsingGET)

Leads worden gekoppeld aan kansen via tussenliggende `opportunityRole` object.

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

Merk op dat beide `dedupeFields` en `searchableFields` zijn iets anders dan kansen. `dedupeFields` verstrekt eigenlijk een samengestelde sleutel, waar alle drie `externalOpportunityId`, `leadId`, en `role` zijn vereist. Zowel moeten de opportuniteit als de leidende verbinding door de identiteitskaart- gebieden in de bestemmingsinstantie bestaan, voor verslagverwezenlijking om te slagen. Voor `searchableFields`, `marketoGUID`, `leadId`, en `externalOpportunityId` zijn allen geldig voor vragen op zich en gebruiken een patroon identiek aan Kansen, maar er is een extra optie om de samengestelde sleutel aan vraag te gebruiken, die vereist het voorleggen van een voorwerp JSON via POST, met de extra vraagparameter `_method=GET`.

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

Dit veroorzaakt het zelfde type van reactie zoals een standaardvraag van de GET, heeft het eenvoudig een verschillende interface voor het doen van het verzoek.

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
