---
title: Aangepaste objecten
feature: REST API, Custom Objects
description: Aangepaste Marketo-objecten maken en bewerken.
exl-id: 88e8829b-f8f1-46d7-a753-5aa6e20e2c40
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '2909'
ht-degree: 0%

---

# Aangepaste objecten

[**&#x200B;** Marketo van het Eindpunt van Objecten van de Douane van 0&rbrace; Verwijzing van het Eind van Objecten van de Douane staat gebruikers toe om de Voorwerpen van de Douane van Marketo te bepalen die met de StandaardVoorwerpen van Marketo (Leads, Bedrijven) of andere Voorwerpen van de Douane van Marketo verwant zijn.](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects)  De Voorwerpen van de Douane van Marketo kunnen worden gecreeerd gebruikend Marketo UI zoals die [ hier ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects) wordt beschreven, of door de Meta-gegevens van Objecten van de Douane te gebruiken API zoals hieronder beschreven.

Een geschikt Marketo-abonnementstype is vereist voor toegang tot de API voor metagegevens van aangepaste objecten.  Raadpleeg uw CSM voor meer informatie.

## Lijst

Naast standaard beschrijf, vraag, update, en schrap vraag beschikbaar voor de voorwerpen van het loodgegevensbestand, hebben de Voorwerpen van de Douane de vraag van de a [ lijst ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET) beschikbaar.  Het aanroepen van dit eindpunt zal een reactie met een lijst van douanevoorwerpen beschikbaar in de bestemmingsinstantie, samen met extra meta-gegevens over de voorwerpen terugkeren.

```
GET /rest/v1/customobjects.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"Car",
         "displayName":"Car",
         "description":"Car owner",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":["vin"],
         "searchableFields":[
            ["vin"],
            ["marketoGUID"],
            ["siebelId"]
         ],
         "relationships":[
            {
               "field":"siebelId",
               "type":"parent",
               "relatedTo":{
                  "name":"Lead",
                  "field":"siebelId"
               }
            }
         ]
      }
   ]
}
```

In het antwoord wordt een lijst weergegeven met de relaties die op elk object aanwezig zijn.  Een relatie heeft een `field` -lid dat het veld op het object aangeeft dat de koppelingswaarde bevat, een `type` -lid dat aangeeft of de relatie betrekking heeft op een bovenliggend of een onderliggend type-object, en een `relatedTo` -object dat de naam van het verwante object aangeeft, en het koppelingsveld op dat object.

## Beschrijven

[ beschrijf vraag ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) voor douanevoorwerpen het zelfde patroon zoals dat van Kansen en Bedrijven, met de toevoeging van de `relationships` serie in de reactie en een `apiName` wegparameter in URI volgt die de API naam van het te beschrijven type van douaneobjecten neemt.  Zoals de lijstvraag, zal dit om het even welke verhoudingen vermelden die voor dit type van douaneobjecten beschikbaar zijn.

```
GET /rest/v1/customobjects/{apiName}/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"Car",
         "displayName":"Car",
         "description":"Car owner",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":["vin"],
         "searchableFields":[
            ["vin"],
            ["marketoGUID"],
            ["siebelId"]
         ],
         "relationships":[
            {
               "field":"siebelId",
               "type":"parent",
               "object":{
                  "name":"Lead",
                  "field":"siebelId"
               }
            }
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
               "name":"vin",
               "displayName":"VIN",
               "description":"Vehicle Identification Number",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {
               "name":"siebelId",
               "displayName":"External Id",
               "description":"External Id",
               "dataType":"string",
               "length":36,
               "updateable":true
            },
            {
               "name":"make",
               "displayName":"Make",
               "dataType":"string",
               "length":36,
               "updateable":true
            },
            {
               "name":"model",
               "displayName":"Model",
               "description":"Vehicle Model",
               "dataType":"string",
               "length":255,
               "updateable":true
            },
            {
               "name":"year",
               "displayName":"Year",
               "dataType":"integer",
               "updateable":true
            },
            {
               "name":"color",
               "displayName":"Color",
               "description":"Vehicle color",
               "dataType":"String",
               "length": 255,
               "updateable":true
            }
         ]
      }
   ]
}
```

## Query

[ het vragen van douanevoorwerpen ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET) is lichtjes verschillend van andere het Gegevensbestand van het Lood APIs, en neemt `apiName` wegparameter als beschrijf.  Voor normale filterType-parameters is de query een eenvoudige GET, zoals query&#39;s voor andere typen records, waarvoor een `filterType` en `filterValues` vereist is.  Optioneel worden de parameters `**fields**` , `batchSize` en `nextPageToken` geaccepteerd.  Als u een lijst met velden aanvraagt en een bepaald veld wordt opgevraagd, maar niet wordt geretourneerd, wordt de waarde impliciet ingesteld op null.

```
GET /rest/v1/customobjects/{apiName}.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa,dff23271-f996-47d7-984f-f2676861b5fb
```

```
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa",
         "vin":"19UYA31581L000000",
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "vin":"29UYA31581L000000",
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
   ]
}
```

Als u echter zoekt met samengestelde sleutels, gedraagt de API zich als de Opportunity Roles API, waarbij een POST met een JSON-hoofdtekst wordt geaccepteerd.  De JSON-hoofdtekst kan dezelfde leden hebben als een GET-query, behalve voor `filterValues` .  In plaats van filterwaarden is er een array `input` die objecten neemt die een lid bevatten dat voor elk objecttype `dedupeFields` is benoemd.

```
POST /rest/v1/customobjects/{apiName}.json?_method=GET
```

```json
{
   "filterType":"dedupeFields",
   "fields":[
      "marketoGuid",
      "Bedrooms",
      "yearBuilt"
   ],
   "input":[
      {
         "mlsNum":"1962352",
         "houseOwnerId":"42645756"
      },
      {
         "mlsNum":"2962352",
         "houseOwnerId":"52645756"
      },
      {
         "mlsNum":"3962352",
         "houseOwnerId":"62645756"
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
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa",
         "Bedrooms":3,
         "yearBuilt":1948,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "Bedrooms":4,
         "yearBuilt":1956,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":2,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc",
         "Bedrooms":3,
         "yearBuilt":2001,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      }
   ]
}
```

## Maken en bijwerken

Gebruik het [ eindpunt van de Objecten van de Douane van de Synchronisatie ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) om douanevoorwerpen tot stand te brengen of bij te werken, kunt u de verrichting specificeren gebruikend de `action` parameter.  Tot 300 verslagen kunnen in één vraag worden gecreeerd of worden bijgewerkt.  De waarden die in de `input` serie worden gebruikt zijn grotendeels gebaseerd op de informatie die door [ is teruggekeerd beschrijven het 2&rbrace; eindpunt van Objecten van de Douane &lbrace;. ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/endpoint-reference#!/Custom_Objects/describeUsingGET_1) In een voorbeeld van een auto-object is er slechts één deduplicatieveld, `vin` .  Als u records wilt bijwerken of maken wanneer u de dedupeFields-modus gebruikt, moet elke record in de invoerarray ten minste een `vin` -veld bevatten.

```
POST /rest/v1/customobjects/{apiName}.json
```

```json
{
   "action":"updateOnly",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000",
         "siebelId":"f2676861b5fb",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
      },
      {
         "vin":"29UYA31581L000000",
         "siebelId":"f2676861b5fc",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
      },
      {
         "vin":"39UYA31581L000000",
         "siebelId":"f2676861b5fd",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
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
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status": "created",
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1004",
               "message":"Lead not found"
            }
         ]
      }
   ]
}
```

Wanneer updates worden uitgevoerd via de `idField` -modus, is de waarde `idField` altijd `marketoGUID` , zodat voor elke record altijd een `marketoGUID` -veld is vereist.  Vergeet niet dat `idField` alleen geldig is voor het actietype updateOnly, aangezien dit veld altijd door het systeem wordt beheerd.  Uw reactie zal de **status** van elk individueel verslag in de resultaatserie omvatten, en of a `marketoGUID` of a `reasons` serie afhankelijk van al dan niet de verrichting voor een individueel verslag succesvol was.

## Verwijderen

[ het Schrappen van verslagen ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) is zeer ongecompliceerd.  Selecteer gewoon de modus `deleteBy` ( `idField` of `dedupeFields` ) en neem de bijbehorende velden op in elk van de records in de array `input` . Een maximum van 300 verslagen per vraag wordt toegestaan.

```
POST /rest/v1/customobjects/{apiName}/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000"
      },
      {
         "vin":"29UYA31581L000000"
      },
      {
         "vin":"39UYA31581L000000"
      }
   ]
}

{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      {
         "seq":1,
         "marketoGUID":"da42707c-4dc4-4fc1-9fef-f30a3017240a",
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1013",
               "message":"Object not found"
            }
         ]
      }
   ]
}
```

Net als bij het bijwerken bevat het resultaat een status voor elke afzonderlijke record en een array `marketoGUID` of `reasons` , afhankelijk van het feit of het verwijderen is gelukt.

## Aangepaste objecttypen

Met de API voor metagegevens van aangepaste objecten kunt u op afstand aangepaste objectschema&#39;s beheren.  Met de API kunt u een nieuw type aangepast object maken of een bestaand type wijzigen.  Nadat het type Aangepast object is gemaakt of gewijzigd, moet dit worden goedgekeurd voor gebruik.  Voor meer informatie over douanevoorwerpen, te zien gelieve productdocumentatie [ hier ](https://experienceleague.adobe.com/en/docs/marketo/using/home).

* Aangepaste objecttypen die door de API zijn gemaakt, kunnen niet worden gewijzigd met de gebruikersinterface van Marketo
* Het maximumaantal toegestane aangepaste objecttypen is 10
* Het maximumaantal toegestane aangepaste objectvelden is 50 per type
* API-namen en weergavenamen van aangepaste objecttypen kunnen alfanumerieke tekens en onderstrepingsteken &quot;_&quot; bevatten

### Type query

Er zijn twee manieren om metagegevens van aangepaste objecttypen op te halen: beschrijf Aangepast objecttype, dat wordt geretourneerd  De record voor één aangepast objecttype. De record kan worden gefilterd door goedkeuringsstatus en Aangepaste objecttypen weergeven. Deze typen retourneren een lijst met alle aangepaste objecttypen in het abonnement en kunnen worden gefilterd op naam en goedkeuringsstatus.

### Type beschrijving

Het [ beschrijft het type van Objecten van de Douane ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) eindpunt keert meta-gegevens voor één enkel type van douaneobjecten terug. De vereiste `apiName` padparameter is de API-naam van het aangepaste objecttype dat wordt beschreven.  Als er een goedgekeurde versie bestaat, wordt deze geretourneerd.  Anders wordt de conceptversie geretourneerd.  De optionele parameter `state` wordt gebruikt om op te geven welke versie moet worden geretourneerd: `draft` , `approved` of `approvedWithDraft` .

```
GET /rest/v1/customobjects/schema/{apiName}/describe.json?state=approved
```

```json
{
    "requestId": "d9bf#16876fa84b9",
    "result": [
        {
            "state": "approved",
            "version": "approved",
            "displayName": "Car",
            "description": "Automobile owned",
            "apiName": "car",
            "idField": "marketoGUID",
            "createdAt": "2019-01-22T19:12:18Z",
            "updatedAt": "2019-01-22T19:12:18Z",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
                ],
                [
                    "marketoGUID"
                ],
                [
                    "leadID"
                ]
            ],
            "relationships": [
                {
                    "field": "leadID",
                    "type": "child",
                    "relatedTo": {
                        "name": "Lead",
                        "field": "id"
                    }
                }
            ],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "leadID",
                    "displayName": "Lead ID",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "year",
                    "displayName": "Year",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

Hier kunnen de volgende kenmerken worden weergegeven:

* Metagegevens: status, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, doorzoekbareFields, relaties
* Standaardvelden: marketoGUID, createdAt, updatedAt
* Aangepaste velden leadId, vin, make,  model, jaar

### Lijsttypen

Het [ eindpunt van de Objecttypes van de Lijst van de Douane keert meta-gegevens voor alle types van douaneobjecten beschikbaar in de bestemmingsinstantie terug.](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/listCustomObjectTypesUsingGET)  Merk op dat dit eindpunt aan [ de Voorwerpen van de Douane van de Lijst ](https://experienceleague.adobe.com/docs/marketo-developer/marketo/soap/custom-objects/custom-objects.html?lang=en) gelijkaardig is, maar uitvoeriger is en extra meta-gegevens zoals staat, verhoudingen, en gebieden omvat. Als er een goedgekeurde versie bestaat, wordt deze geretourneerd.  Anders wordt de conceptversie geretourneerd.  De facultatieve **staat** parameter wordt gebruikt om de versie van het type van douaneobjecten te specificeren om terug te keren: **ontwerp**, **goedgekeurd**, of **selectedWithDraft**.  De facultatieve **namen** parameter wordt gebruikt om specifieke namen van te keren types van douaneobjecten te specificeren; het is gestructureerd als komma gescheiden lijst van API namen.

```
GET /rest/v1/customobjects/schema.json?names=purchaseHistory
```

```json
{
    "requestId": "a181#167ebe94703",
    "result": [
        {
            "state": "approved",
            "displayName": "Purchases",
            "description": "Purchase data",
            "apiName": "purchaseHistory",
            "idField": "marketoGUID",
            "createdAt": "2014-09-12T16:13:37Z",
            "updatedAt": "2014-09-12T16:13:42Z",
            "dedupeFields": [
                "lead_id",
                "product_name"
            ],
            "searchableFields": [
                [
                    "lead_id",
                    "product_name"
                ],
                [
                    "marketoGUID"
                ],
                [
                    "lead_id"
                ]
            ],
            "relationships": [
                {
                    "field": "lead_id",
                    "type": "child",
                    "relatedTo": {
                        "name": "Lead",
                        "field": "lead_id"
                    }
                }
            ],
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "marketoGUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "amount",
                    "displayName": "Amount",
                    "dataType": "float",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "lead_id",
                    "displayName": "lead_id",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "product_name",
                    "displayName": "Product Name",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "purchase_date",
                    "displayName": "Transaction Date",
                    "dataType": "datetime",
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        },
        {
            "state": "approved",
            "version": "approved",
            "displayName": "Car",
            "description": "No really, it's a car!",
            "apiName": "car_c",
            "idField": "marketoGUID",
            "createdAt": "2017-02-22T19:55:51Z",
            "updatedAt": "2018-12-11T23:52:56Z",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
                ],
                [
                    "marketoGUID"
                ]
            ],
            "relationships": [],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "color",
                    "displayName": "Color",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "year",
                    "displayName": "Year",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

### Tekst maken en bijwerken

#### Tekst maken

Het [ eindpunt van het Type van Objecten van de Douane van 0&rbrace; Synchronisatie wordt gebruikt om een type van douaneobjecten tot stand te brengen of bij te werken.](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST)  De verslagverrichting om uit te voeren wordt gecontroleerd door de facultatieve **actie** attributen die **kunnen zijn createOnly**, **createOrUpdate**, of **updateOnly**.  De standaardinstelling is createOrUpdate. De **displayName** en **apiName** attributen worden vereist, behalve wanneer het gebruiken van updateOnly als uw actie.   Beide moeten uniek zijn om botsingen met klant-provisioned types te vermijden.  Als u een partner van LaunchPoint bent, zou u een representatieve namespace aan deze namen moeten prepend.  Voor apiName is het gebruikelijk kleine letters of camelCase te gebruiken om onderscheid te maken tussen andere tekstreeksen. Het facultatieve **attribuut 0&rbrace; pluralName {specificeert de meervorm van displayName.**  Het facultatieve **beschrijvings** attribuut wordt gebruikt om het type van douaneobjecten te beschrijven.  Het facultatieve **booleaanse attribuut 0} showInLeadDetail wordt gebruikt om het bekijken van douaneobjecten gegevens binnen de Lead pagina van het Gegevensbestand van Marketo toe te laten UI.**  De standaardinstelling is false.

Wees voorzichtig bij het benoemen van aangepaste objecten. Wanneer u een nieuw aangepast object maakt, kunt u het beste vóór de naam een tekenreeks plaatsen die de naam van het bedrijf aangeeft (alfanumeriek of onderstrepingsteken is toegestaan). Dit maakt het douanevoorwerp gemakkelijk doorzoekbaar in MLM UI, en helpt ook onzeker zijn dat de naam uniek is.

Hier volgt een voorbeeld van het maken van een nieuw aangepast objecttype met API  Naam &quot;transactie&quot;.

```
POST /rest/v1/customobjects/schema.json
```

```json
{
  "action":"createOnly",
  "displayName": "Transaction",
  "apiName": "transaction",
  "description": "Commerce happens"
}
```

```json
{
    "requestId": "fb9d#167f2879557",
    "result": [],
    "success": true
}
```

Hier volgt een aanroep om het zojuist gemaakte type te beschrijven.

```
GET /rest/v1/customobjects/schema/transaction/describe.json
```

```json
{
    "requestId": "cf9b#167f28db0a9",
    "result": [
        {
            "state": "draft",
            "displayName": "Transaction",
            "description": "Commerce happens",
            "apiName": "transaction",
            "idField": null,
            "createdAt": null,
            "updatedAt": null,
            "dedupeFields": [],
            "searchableFields": [
                []
            ],
            "relationships": [],
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

Hier kunnen de volgende gegevens over aangepaste objecten worden weergegeven:

* Metagegevens: status, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, doorzoekbareFields, relaties
* Standaardvelden: marketoGUID, createdAt, updatedAt

#### Type update

Hier is een voorbeeld van het bijwerken van de Beschrijving voor een bestaand type waarvan API Naam &quot;transactie&quot;is.  Het **attribuut 0&rbrace; apiName wordt vereist.**  Hier zullen wij veronderstellen dat het type reeds bestaat en updateOnly voor de facultatieve **actie** attributen gebruikt.  Naast **apiName**, kunnen de attributen beschikbaar voor verwezenlijking worden bijgewerkt.

```
POST /rest/v1/customobjects/schema.json
```

```json
{
  "action":"updateOnly",
  "apiName": "transaction",
  "description":"No really, commerce happens!"
}
```

```json
{
    "requestId": "103c3#167f2223fd7",
    "result": [],
    "success": true
}
```

## Typegoedkeuring

Aangepaste objecttypen moeten worden goedgekeurd voordat ze kunnen worden gebruikt. Wanneer een nieuw type van douaneobjecten wordt gecreeerd gebruikend [ eindpunt van het Type van Objecten van de Douane van 0&rbrace; Sync &lbrace;, wordt het gecreeerd als ontwerp versie. ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectTypeUsingPOST) Als u klaar bent met het toevoegen van aangepaste velden, moet u de conceptversie goedkeuren. Hiermee maakt u een goedgekeurde versie en verwijdert u de conceptversie. Wanneer een bestaand type van douaneobjecten wordt gewijzigd door het eindpunt van het Type van Objecten van de Douane van de Synchronisatie te gebruiken, of door één van Add te gebruiken/werkt/schrapt de eindpunten van het Gebied van het Type van Objecten van de Douane, wordt een ontwerp versie gecreeerd. Alle wijzigingen aan het type of aan zijn gebieden beïnvloeden slechts de ontwerp versie. Wanneer u klaar bent met het wijzigen, moet u de conceptversie goedkeuren. Hiermee vervangt u de goedgekeurde versie door de conceptversie en verwijdert u de conceptversie. Voor meer informatie over de goedkeuring van douaneobjecten, te zien gelieve productdocumentatie [ hier ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

Wanneer een aangepast objecttype is goedgekeurd, kunt u het volgende niet doen:

* Werk de `displayName` of `apiName` bij
* Een koppelingsveld toevoegen of verwijderen
* Een deduplicatieveld toevoegen of verwijderen

Om deze redenen, is het belangrijk om door het schema en noemende overeenkomst zorgvuldig te denken die u van plan bent te gebruiken.

### Type goedkeuren

Gebruik het [ goedkeurt 1&rbrace; eindpunt van het Type van Objecten van de Douane &lbrace;om een ontwerp versie als nieuwe goedgekeurde versie te publiceren.](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/approveCustomObjectTypeUsingPOST)  **apiName** is de enige vereiste parameter als wegparameter.  Een type kan niet worden goedgekeurd tenzij het in ontwerp staat is, en voldoet een reeks hier beschreven bevestigingsregels [&#128279;](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

```
POST /rest/v1/customobjects/schema/{apiName}/approve.json
```

```json
{
    "requestId": "11d86#1685304a983",
    "result": [],
    "success": true
}
```

### Type negeren

Gebruik het [ Concept van het Type van Objecten van de Douane van de Weigering ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/discardCustomObjectTypeUsingPOST) eindpunt om een ontwerp versie te schrappen. `apiName` is de enige vereiste parameter als padparameter. Een type moet in ontwerpstaat zijn om te worden verworpen, d.w.z. kan een goedgekeurd type niet worden verworpen.

```
POST /rest/v1/customobjects/schema/{apiName}/discardDraft.json
```

```json
{
    "requestId": "5228#1684edde793",
    "result": [],
    "success": true
}
```

### Tekst verwijderen

Gebruik het [ eindpunt van het Type van Objecten van de Douane van de Schrapping ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) om een goedgekeurde versie te schrappen.  `apiName` is de enige vereiste parameter als padparameter.  Dit is een destructieve bewerking die niet ongedaan kan worden gemaakt.  Een type kan alleen worden verwijderd als het uit gebruik is genomen door elementen, zoals triggers of filters.  Het Get Eigen Voorwerp Afhankelijke eindpunt van Assets kan worden gebruikt om een lijst van afhankelijke activa voor een bepaald type terug te winnen.

POST /rest/v1/customobjects/schema/{apiName} /delete.json

```json
{
    "requestId": "14e36#1684efc4227",
    "result": [],
    "success": true
}
```

## Aangepaste objectvelden

Standaard bevatten alle typen aangepaste objecten de volgende standaardvelden:

* Marketo GUID - Unieke id voor aangepast objecttype
* Gemaakt bij - datum waarop het aangepaste objecttype werd gemaakt
* Bijgewerkt bij - Datetime toen het aangepaste objecttype voor het laatst werd bijgewerkt

U kunt zelf aangepaste velden toevoegen, wijzigen of verwijderen met behulp van de onderstaande eindpunten.

* Het maximum aantal toegestane velden is 50
* Nadat een aangepast object is goedgekeurd, kunt u maximaal 20 extra velden toevoegen aan het aangepaste object
* Er is ten minste 1 deduplicatieveld vereist, maximaal 3
* Veld-API-namen en weergavenamen mogen alfanumerieke tekens en onderstrepingsteken &quot;_&quot; bevatten

Voor meer informatie over de gebieden van douaneobjecten, gelieve productdocumentatie [ hier ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields) te zien.

### Velden toevoegen

[ voeg de Gebieden van het Type van Objecten van de Douane ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/addCustomObjectTypeFieldsUsingPOST) eindpunt toe staat u toe om één of meerdere gebieden aan uw douanevoorwerp toe te voegen.  De aanvraaghoofdtekst bevat een `input` -array met een of meer elementen.  Elk element is een JSON-object met kenmerken die een veld beschrijven. Het vereiste `name` -kenmerk is de API-naam van het veld en moet uniek zijn voor het aangepaste object.   De conventie is om kleine letters of camelCase te gebruiken om onderscheid te maken tussen andere tekstreeksen. Het vereiste `displayName` -kenmerk is de leesbare naam van het veld en moet uniek zijn voor het aangepaste object. Het vereiste `dataType` -kenmerk is het gegevenstype van het veld.  A  lijst van toelaatbare gegevenstypes kan worden verkregen door [ te roepen krijgt de Types van Gegevens van het Gebied van het Type van Douane van Objecten ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) eindpunt.  Aangepaste objecten kunnen velden bevatten met het gegevenstype &quot;link&quot;.  De gebieden van de verbinding worden gebruikt om verhoudingen tussen douanevoorwerpen en andere objecten types in het systeem, b.v. Lead, Bedrijf te vestigen.  Meer informatie over verbindingsgebieden kan [ hier ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields) worden gevonden. Het optionele kenmerk `description` is de beschrijving van het veld. Het optionele `isDedupeField` boolean-kenmerk geeft aan of het veld wordt gebruikt voor deduplicatie tijdens aangepaste objectupdatebewerkingen.  De standaardinstelling is false.  Voor een-op-veel relaties is een deduplicatieveld vereist. Het optionele `relatedTo` -objectkenmerk geeft een koppelingsveld op.  Voor een-op-veel-relaties bevat dit object een `name` -kenmerk dat het &quot;link-object&quot; of bovenliggend object is waarnaar moet worden gekoppeld, en een `field` -kenmerk dat het &quot;link field&quot; is,  of het veld binnen het bovenliggende object dat als sleutelkenmerk moet worden gebruikt.  Roep het [ Aangepast Voorwerp Linksys ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) eindpunt van Objecten van de Objecten van de Douane om een lijst van toegestane verbindingsvoorwerpen terug te winnen.  Voor meer informatie over verbindingsgebieden, zie productdocumentatie [ hier ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). Een aangepast object kan niet worden gekoppeld aan een ander aangepast object met een bestaand koppelingsveld.

### Een-op-een-relatie

Voor een één-op-vele structuur van het douanevoorwerp, gebruik een verbindingsgebied in een douanevoorwerp om het met een standaardobject te verbinden: Lood of Bedrijf. Gebruikend het voorbeeld van de autoeigenaar van het productdocumentatie van Marketo [&#128279;](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure), leiden wij tot een douanevoorwerp dat op auto betrekking hebbende informatie bevat om met Leads te verbinden.

1. Creeer a **het voorwerp van de Auto**
1. Voeg gebieden aan **auto** voorwerp toe: dedupe op **VIN**, verbinding aan **Lood**&#x200B;**/Lood ID*
1. Goedkeuren **het voorwerp van de Auto**

Maak eerst het aangepaste objecttype dat auto-specifieke informatie bevat.

```
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action":"createOnly",
    "displayName": "Car",
    "pluralName": "Cars"
    "apiName": "car",
    "description": "Automobile owned",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "cbaa#16876dd3da6",
    "result": [],
    "success": true
}
```

Voeg nu velden toe aan het aangepaste objecttype Auto. We gebruiken een koppelingsveld om het object en het veld op te geven waarmee verbinding moet worden gemaakt. In dit geval is het koppelingsobject Lead en is het koppelingsveld Id. Gebruik een tekenreeksveld voor deduplicatie (VIN).  We voegen nog drie velden toe waarin extra kenmerken voor de auto kunnen worden opgeslagen (Merk, Model, Jaar).

```
POST /rest/v1/customobjects/schema/car/addField.json
```

```json
{
  "input": [
    {
      "displayName": "Lead ID",
      "description": "Link field to Lead object",
      "name": "leadID",
      "dataType": "link",
      "relatedTo": {
        "field": "id",
        "name": "lead"
      }
    },
    {
      "displayName": "VIN",
      "description": "Vehicle ID number",
      "name": "vin",
      "dataType": "string",
      "isDedupeField": true
    },
    {
      "displayName": "Make",
      "description": "Vehicle make",
      "name": "make",
      "dataType": "string"
    },
    {
      "displayName": "Model",
      "description": "Vehicle model",
      "name": "model",
      "dataType": "string"
    },
    {
      "displayName": "Year",
      "description": "Vehicle year",
      "name": "year",
      "dataType": "integer"
    }
  ]
}

{
    "requestId": "b359#16876f17996",
    "result": [],
    "success": true
}
```

Tot slot keur het type van douaneobjecten goed.

```
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

### Vele-aan-Vele Verhouding

Vele-aan-vele verhoudingen worden vertegenwoordigd gebruikend een &quot;brug,&quot;of een intermediair, douanevoorwerp binnen tussen een standaard douanevoorwerp, zoals Lead of Bedrijf, en een &quot;rand&quot;douanevoorwerp. Het object edge is de primaire entiteit die beschrijvende kenmerken (velden) bevat. Het bridge-object bevat de gegevens om de objectrelaties op te lossen met behulp van twee koppelingsvelden.  Eén koppelingsveld verwijst net als in een  één-aan-vele relatieconfiguratie.  Het andere koppelingsveld verwijst naar het object edge, een aangepast object zonder koppelingen.  Het bridge-object kan ook beschrijvende kenmerken (velden) bevatten. Gebruikend het de inschrijvingsvoorbeeld van de hoscursus van het het productdocumentatie van Marketo [&#128279;](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure), creëren wij een voorwerp van de randdouane om cursus-verwante informatie te bevatten, en een voorwerp van de inschrijvingsbrug dat wordt gebruikt om Cursussen met Leads te verbinden. Hier volgen de volgende stappen:

1. Creeer a **Cursus** randvoorwerp
1. Voeg gebieden aan **Cursus toe:** dedupe op **identiteitskaart van de Cursus**
1. Goedkeuren **Cursus**
1. Creeer een **Inschrijving** brugvoorwerp
1. Voeg gebieden aan **Inschrijving toe:** dedupe op **identiteitskaart van de Inschrijving**, verbinding aan **Cursus**&#x200B;**/Cursus ID** gebied en verbinding aan **Lood**&#x200B;**/Lood ID**
1. Goedkeuren **Inschrijving**

Maak eerst het objecttype voor de rand zodat het cursusspecifieke informatie bevat:

```
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action":"createOnly",
    "displayName": "Course",
    "pluralName": "Courses",
    "apiName": "course",
    "description": "Modeling a college course, an edge object in Marketo",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "4aec#168879ede00",
    "result": [],
    "success": true
}
```

Laten we vervolgens aangepaste velden toevoegen aan het type randobject.  In dit voorbeeld voegen we de volgende vier aangepaste velden toe aan een modelcursus: Cursus-id, Cursusinstructeur, Cursuslocatie, Cursusnaam.  Merk op dat wij Cursus identiteitskaart als ons dedupe gebied aanwijzen, aangezien minstens één dedupe gebied wordt vereist.

```
POST /rest/v1/customobjects/schema/course/addField.json
```

```json
{
    "input": [
        {
            "displayName": "Course ID",
            "name": "courseID",
            "dataType": "string",
            "isDedupeField": true
        },
        {
            "displayName": "Course Instructor",
            "name": "courseInstructor",
            "dataType": "string"
        },
        {
            "displayName": "Course Location",
            "name": "courseLocation",
            "dataType": "string"
        },
        {
            "displayName": "Course Name",
            "name": "courseName",
            "dataType": "string"
        }
    ]
}
```

```
{
    "requestId": "cc36#16895b82a41",
    "result": [],
    "success": true
}
```

Nu moeten wij het type van randobjecten goedkeuren zodat wij het kunnen later van verwijzingen voorzien wanneer het verbinden met het brugobjecten type.  Aangepaste objecttypen moeten zijn goedgekeurd om te kunnen worden geselecteerd als een koppelingsobject.

```
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

Het randobject is voltooid.  Laten we nu verdergaan om het bridge-objecttype te maken voor het bevatten van inschrijvingsspecifieke informatie.

```
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action": "createOnly",
    "displayName": "Enrollment",
    "pluralName": "Enrollments",
    "apiName": "enrollment",
    "description": "Bridge object for Course custom object",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "8fbb#168960f671b",
    "result": [],
    "success": true
}
```

Als u aangepaste velden wilt toevoegen aan het objecttype bridge, voegt u twee koppelingsvelden toe: een veld dat is gekoppeld aan het object Lead en een veld dat is gekoppeld aan het object Cursus dat we zojuist hebben gemaakt. Gebruik het veld Id van lead om een koppeling naar het object lead tot stand te brengen. Als u een koppeling wilt maken naar het object Cursus, gebruikt u het veld Cursus-id.  Voeg vervolgens een unieke id voor de inschrijving-id toe als ons dedupliceveld, aangezien er ten minste één dedupliceveld vereist is. Tot slot voeg een gebied van de Graad toe om te volgen hoe de student deed.

```
POST /rest/v1/customobjects/schema/enrollment/addField.json
```

```json
{
    "input": [
        {
            "displayName": "Lead ID",
            "description": "Link field to Lead object",
            "name": "leadID",
            "dataType": "link",
            "relatedTo": {
                "field": "id",
                "name": "lead"
            }
        },
        {
            "displayName": "Course ID",
            "description": "Link field to Course object",
            "name": "courseID",
            "dataType": "link",
            "relatedTo": {
                "field": "courseID",
                "name": "course"
            }
        },
        {
            "displayName": "Enrollment ID",
            "description": "Unique ID for deduplication",
            "name": "enrollmentID",
            "dataType": "string",
            "isDedupeField": true
        },
        {
            "displayName": "Grade",
            "description": "Grade for the course",
            "name": "grade",
            "dataType": "string"
        }
    ]
}
```

```json
{
    "requestId": "7be5#168973f5052",
    "result": [],
    "success": true
}
```

Tot slot keur het brugvoorwerp goed.

```
POST /rest/v1/customobjects/schema/enrollment/approve.json
```

```json
{
    "requestId": "9a76#16897b0e84b",
    "result": [],
    "success": true
}
```

U kunt douaneobjecten verslagen programmatically bevolken door [ het Voorwerp van de Douane van de Synchronisatie te gebruiken ](#create_and_update), of [ Bulk de Invoer van de Objecten van de Douane ](https://experienceleague.adobe.com/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import.html?lang=en). Alternatief, kunt u de functionaliteit gebruiken van Marketo UI [ de Gegevens van de Objecten van de Invoer van de Douane ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/import-custom-object-data).

## Veld bijwerken

Het [ eindpunt van het Gebied van het Type van Objecten van de Update van het Type van Douane ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/updateCustomObjectTypeFieldUsingPOST) staat u toe om een gebied in uw voorwerp van de ontwerpdouane bij te werken.  De vereiste padparameter `apiName` is de API-naam van het aangepaste objecttype.  De vereiste padparameter `fieldAPIName` is de API-naam van het veld voor het aangepaste objecttype.  De aanvraaghoofdtekst bevat een JSON-object met sleutel/waardeparen die de veldkenmerken opgeven die moeten worden bijgewerkt.

```
POST /rest/v1/customobjects/schema/{apiName}/{fieldApiName}/updateField.json
```

```json
{
  "displayName": "Very Long Title",
  "dataType": "text"
}
```

```json
{
    "requestId": "d523#1684f355db9",
    "result": [],
    "success": true
}
```

## Velden verwijderen

Het [ eindpunt van het Type van Objecten van de Douane van de Schrapping ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectTypeFieldsUsingPOST) staat u toe om één of meerdere gebieden van uw douanevoorwerp te schrappen.  De vereiste padparameter `apiName` is de API-naam van het aangepaste objecttype.  De aanvraaghoofdtekst bevat een JSON-object met een `input` -array met een of meer elementen.  Elk element is een JSON-object met een `name` -kenmerk dat de API-naam opgeeft van het veld dat moet worden verwijderd.

```
POST /rest/v1/customobjects/schema/{apiName}/deleteField.json
```

```json
{
    "input":
    [
        {
            "name": "title"
        },
        {
            "name": "author"
        }
    ]
}
```

```json
{
"requestId": "b359#19934f17996",
"result": [],
"success": true
}
```

## Gegevenstypen lijstvelden

Het [ krijgt de Types van Gegevens van het Gebied van het Type van Douane van Objecten ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) eindpunt keert de lijst van alle toelaatbare types van gebiedsgegevens terug. Dit is nuttig wanneer het modelleren van uw type van douaneobjecten om de gegevenstypes van het douanegebied te identificeren die worden gesteund.

```
GET /rest/v1/customobjects/schema/fieldDataTypes.json
```

```json
{
    "requestId": "c405#167ed49e866",
    "result": [
        "string",
        "boolean",
        "integer",
        "float",
        "link",
        "email",
        "currency",
        "date",
        "datetime",
        "phone",
        "text"
    ],
    "success": true
}
```

## Aanpasbare aangepaste objecten weergeven

Het [ krijgt Aangepast Voorwerp Linksys ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) eindpunt keert een lijst van alle toegelaten verbindingsvoorwerpen, en hun verbindingsgebieden terug.  De lijst zal Standaardvoorwerpen (Lood, Bedrijf), en om het even welke Voorwerpen van de Douane bevatten die in de instantie zijn gecreeerd.

```
GET /rest/v1/customobjects/schema/linkableObjects.json
```

```json
{
    "requestId": "11e62#167f1160e4e",
    "result": [
        {
            "name": "lead",
            "displayName": "Lead",
            "fields": [
                {
                    "name": "Account Balance",
                    "displayName": "Account Balance",
                    "dataType": "integer"
                },
                {
                    "name": "Email Address",
                    "displayName": "Email Address",
                    "dataType": "email"
                },
                {
                    "name": "Id",
                    "displayName": "Id",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Display Name",
                    "displayName": "Marketo Social Facebook Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Id",
                    "displayName": "Marketo Social Facebook Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Photo URL",
                    "displayName": "Marketo Social Facebook Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Profile URL",
                    "displayName": "Marketo Social Facebook Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Reach",
                    "displayName": "Marketo Social Facebook Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Referred Enrollments",
                    "displayName": "Marketo Social Facebook Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Referred Visits",
                    "displayName": "Marketo Social Facebook Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Gender",
                    "displayName": "Marketo Social Gender",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Display Name",
                    "displayName": "Marketo Social LinkedIn Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Id",
                    "displayName": "Marketo Social LinkedIn Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Photo URL",
                    "displayName": "Marketo Social LinkedIn Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Profile URL",
                    "displayName": "Marketo Social LinkedIn Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Reach",
                    "displayName": "Marketo Social LinkedIn Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social LinkedIn Referred Enrollments",
                    "displayName": "Marketo Social LinkedIn Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social LinkedIn Referred Visits",
                    "displayName": "Marketo Social LinkedIn Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Syndication Id",
                    "displayName": "Marketo Social Syndication Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Total Referred Enrollments",
                    "displayName": "Marketo Social Total Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Total Referred Visits",
                    "displayName": "Marketo Social Total Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Display Name",
                    "displayName": "Marketo Social Twitter Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Id",
                    "displayName": "Marketo Social Twitter Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Photo URL",
                    "displayName": "Marketo Social Twitter Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Profile URL",
                    "displayName": "Marketo Social Twitter Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Reach",
                    "displayName": "Marketo Social Twitter Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Referred Enrollments",
                    "displayName": "Marketo Social Twitter Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Referred Visits",
                    "displayName": "Marketo Social Twitter Referred Visits",
                    "dataType": "integer"
                }
            ]
        },
        {
            "name": "company",
            "displayName": "Company",
            "fields": [
                {
                    "name": "Id",
                    "displayName": "Id",
                    "dataType": "integer"
                }
            ]
        },
        {
            "name": "car_c",
            "displayName": "Car",
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string"
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string"
                }
            ]
        }
    ],
    "success": true
}
```

## Assets voor aangepast object ophalen

Het [ krijgt Afhankelijke Assets van Objecten van de Douane ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeDependentAssetsUsingGET) eindpunt keert een lijst van afhankelijke activa van een type van douaneobjecten, met inbegrip van hun in-instantie plaats terug.  Dit is nuttig wanneer het verwijderen van een integratie en u moet overal identificeren dat een type van douaneobjecten in gebruik is.

```
GET /rest/v1/customobjects/schema/{apiName}/dependentAssets.json
```


```json
{
    "requestId": "71cf#16a21f30ed6",
    "result": [
        {
            "assetType": "Smart Campaign",
            "assetId": 3773,
            "assetName": "CarTest.HasCar (Smart List)"
        },
        {
            "assetType": "Smart Campaign",
            "assetId": 3773,
            "assetName": "CarTest.HasCar (Smart List)",
            "usedFields": [
                "leadID",
                "make",
                "model",
                "vin",
                "year"
            ]
        }
    ],
    "success": true
}
```

## Tijdstippen

* Eindpunten van aangepaste objecten hebben een time-out van 30 seconden, tenzij hieronder vermeld
   * Aangepaste objecten synchroniseren: 120s 
   * Aangepaste objecten verwijderen: 60 s
