---
title: Benoemde accounts
feature: REST API
description: Benoemde accounts manipuleren via de API.
exl-id: 2aa1d2a0-9e54-4a9a-abb1-0d0479ed3558
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '679'
ht-degree: 0%

---

# Benoemde accounts

[ Benoemde Verwijzing van het Eindpunt van Rekeningen ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts)

Marketo biedt een set API&#39;s voor het uitvoeren van CRUD-bewerkingen op benoemde accounts voor gebruik met Marketo ABM. Deze API&#39;s volgen het standaard interfacepatroon voor API&#39;s voor hoofddatabases en bieden de opties Beschrijven, Maken/Bijwerken, Verwijderen en Query.

Momenteel zijn de CRUD-bewerkingen voor benoemde accounts de enige ABM-functies die beschikbaar zijn via Marketo API&#39;s. Leads kunnen niet worden gekoppeld aan benoemde accounts via API&#39;s.

## Beschrijven

Als benoemde benoemde accounts worden metagegevens geretourneerd die betrekking hebben op het gebruik van benoemde accounts via API&#39;s van Marketo, waaronder een lijst met geldige doorzoekbare velden bij het opvragen en een lijst met alle velden die beschikbaar zijn voor API-gebruik. De `idField` van een benoemde account is altijd `marketoGUID` en de enige beschikbare `dedupeField` . De sleutel voor het maken is het `name` -veld van het object.

```
GET /rest/v1/namedaccounts/describe.json
```

```json
{
   "requestId":"d65e#156c27ac57d",
   "result":[
      {
         "name":"Named Account",
         "description":"Marketo standard account attribute map",
         "createdAt":"2016-08-18T20:16:41Z",
         "updatedAt":"2016-08-18T20:16:41Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "name"
         ],
         "searchableFields":[
            [
               "marketoGUID",
            ],
            [
               "annualRevenue"
            ],
            [
               "city"
            ],
            [
               "country"
            ],
            [
               "domainName"
            ],
            [
               "industry"
            ],
            [
               "logoUrl"
            ],
            [
               "membershipCount"
            ],
            [
               "name"
            ],
            [
               "numberOfEmployees"
            ],
            [
               "opptyAmount"
            ],
            [
               "opptyCount"
            ],
            [
               "score1"
            ],
            [
               "score2"
            ],
            [
               "score3"
            ],
            [
               "score4"
            ],
            [
               "score5"
            ],
            [
               "sicCode"
            ],
            [
               "state"
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
               "name":"annualRevenue",
               "displayName":"annualRevenue",
               "dataType":"currency",
               "updateable":true
            },
            {
               "name":"city",
               "displayName":"city",
               "dataType":"string",
               "length":255,
               "updateable":true
            },
            {
               "name":"country",
               "displayName":"country",
               "dataType":"string",
               "length":255,
               "updateable":true
            }
         ]
      }
   ],
   "success":true
}
```

### Query

Het vragen voor genoemde rekeningen is gebaseerd op het gebruik van een filterType en een reeks tot 300 komma-gescheiden filterValues. `filterType` kan elk willekeurig veld zijn dat wordt geretourneerd in het `searchableFields` -lid van het beschrijvingsresultaat voor benoemde accounts, terwijl filterValues geldige invoer kan zijn voor het gegevenstype van het veld. Als u een specifieke set velden van wilt retourneren, moet een parameter fields worden doorgegeven, waarbij de waarde bestaat uit een door komma&#39;s gescheiden lijst met velden die moeten worden geretourneerd in het antwoord. Zoals andere vraagopties, is het maximumaantal verslagen voor één enkele vraagpagina 300, en de extra verslagen in de reeks moeten met het gebruik van nextPageToken worden gevraagd die door de vraag is teruggekeerd.

```
GET /rest/v1/namedaccounts.json?filterType=name&filterValues=Google,Yahoo
```

```json
{
    "requestId": "6dac#157d4ddc9d7",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "16efafdd-0148-4ea7-8782-f451d7c6345d",
            "createdAt": "2016-10-17T22:49:04Z",
            "name": "Google",
            "updatedAt": "2016-10-17T22:49:04Z"
        },
        {
            "seq": 1,
            "marketoGUID": "44d62353-7f9d-4d43-b9cc-7ef0f7a09137",
            "createdAt": "2016-10-17T22:49:04Z",
            "name": "Yahoo",
            "updatedAt": "2016-10-17T22:49:04Z"
        }
    ],
    "success": true
}
```

### Maken en bijwerken

Voor het maken en bijwerken van benoemde accounts wordt het standaarddatabasepatroon voor leads gebruikt. Records moeten in een POST-verzoek worden doorgegeven in het invoerlid van een JSON-instantie. `input` is het enige vereiste lid, met `action` en `dedupeBy` als optionele leden. Er kunnen maximaal 300 records in de invoer worden opgenomen. Actie kan van het type createOnly, updateOnly of createOrUpdate zijn. Als deze waarde niet is opgegeven, wordt de handeling standaard ingesteld op createOrUpdate. dedupeBy kan slechts worden gespecificeerd wanneer de actie updateOnly is, en slechts één van dedupeFields of idField goedkeurt, die aan de naam en marketoGUID gebieden beantwoorden, respectievelijk.

```
POST /rest/v1/namedaccounts.json
```

```
Content-Type: application/json
```

```json
{
   "action":"updateOnly",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "name":"Google",
         "domainName":"www.google.com"
      },
      {
         "name":"Yahoo",
         "domainName":"www.yahoo.com"
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
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc"
      }
   ]
}
```

### Velden

Het benoemde accountobject bevat een set velden. Elke velddefinitie bestaat uit een set kenmerken die het veld beschrijven. Voorbeelden van kenmerken zijn weergavenaam, API-naam en dataType. Deze kenmerken worden gezamenlijk metagegevens genoemd.

De volgende eindpunten staan u toe om gebieden op het bedrijfvoorwerp te vragen. Deze APIs vereist dat de het bezitten gebruiker API een rol met één of allebei van het Gelezen-Schrijf StandaardGebied van het Schema of Gelezen-Schrijf de toestemmingen van het Gebied van het Schema van de Douane heeft.

### Query-velden

Het opvragen van benoemde accountvelden is eenvoudig. U kunt één benoemd accountveld opvragen op API-naam of de set met alle bedrijfsvelden opvragen.

#### Op naam

Het [ krijgt Benoemd Gebied van de Rekening door het eindpunt van de Naam ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) wint meta-gegevens voor één enkel gebied op het genoemde rekeningsvoorwerp terug. De vereiste padparameter fieldApiName geeft de API-naam van het veld op. De reactie is als het beschrijf Benoemde eindpunt van de Rekening maar bevat extra meta-gegevens zoals het isCustom attribuut dat erop wijst of het gebied een douanegebied is.

```
GET /rest/v1/namedaccounts/schema/fields/annualRevenue.json
```

```json
{
    "requestId": "371c#17e979c5d1f",
    "result": [
        {
            "displayName": "Annual Revenue",
            "name": "annualRevenue",
            "description": null,
            "dataType": "currency",
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

Het [ krijgt Benoemde eindpunt van de Rekening Gebieden ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) wint meta-gegevens voor alle gebieden op het genoemde rekeningsvoorwerp terug. Standaard worden maximaal 300 records geretourneerd. U kunt de batchSize vraagparameter gebruiken om dit aantal te verminderen. Als het kenmerk moreResult waar is, zijn er meer resultaten beschikbaar. Ga door met het aanroepen van dit eindpunt tot de eigenschap moreResult false retourneert, wat betekent dat er geen resultaten beschikbaar zijn. nextPageToken die van deze API is teruggekeerd zou altijd voor de volgende herhaling van deze vraag moeten worden opnieuw gebruikt.

```
GET /rest/v1/namedaccounts/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "f287#17e995bd0c5",
    "result": [
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
            "displayName": "Domain Name",
            "name": "domainName",
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
        },
        {
            "displayName": "SIC Code",
            "name": "sicCode",
            "description": null,
            "dataType": "string",
            "length": 40,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "City",
            "name": "city",
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
    "nextPageToken": "N42LHXWEULHZ3N2I77DKOJUVOY======",
    "moreResult": true
}
```

### Verwijderen

Verwijderingen worden uitgevoerd via een JSON POST-aanvraag en hebben een vereist invoerlid en een optioneel deleteBy-lid. deleteBy kan één van &quot;dedupeFields&quot;of &quot;idField&quot;zijn, die aan naam of marketoGUID, respectievelijk beantwoorden, en zal aan dedupeFields in gebreke blijven als unset. Het inputlid keurt een serie van tot 300 verslagen goed, die één lid bevatten elk, of naam of marketoGUID afhankelijk van het plaatsen van deleteBy.

```
POST /rest/v1/namedaccounts/delete.json
```

```
Content-Type: application/json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "name":"Google"
      },
      {
         "name":"Yahoo"
      },
      {
         "name":"Marketo"
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
         "id":"dff23271-f996-47d7-984f-f2676861b5fc",
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

- De eindpunten van benoemde accounts hebben een time-out van 30 seconden, tenzij hieronder vermeld
   - Benoemde accounts synchroniseren: 120s 
   - Benoemde accounts verwijderen: 60
