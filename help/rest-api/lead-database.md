---
title: Database lead
feature: REST API, Database
description: Bewerk de hoofddatabase.
exl-id: e62e381f-916b-4d56-bc3d-0046219b68d3
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '1342'
ht-degree: 0%

---

# Database lead

De API&#39;s van de Marketo Lead Database zijn de API&#39;s die Marketo het vaakst gebruikt, omdat ze de uitwisseling van gegevens van personen en personen uit Marketo mogelijk maken, zoals activiteiten, kansen en bedrijven.

## Objecten

De voorwerpen van het Gegevensbestand van de lood omvatten het volgende:

- Leads
- Ondernemingen/rekeningen
- Benoemde accounts
- Kansen
- OpportunityRoles
- Verkoopmedewerkers
- Aangepaste objecten
- Activiteiten
- Lijst en programmalidmaatschap

De meeste van deze objecten bevatten ten minste de methoden Maken, Lezen, Bijwerken en Verwijderen. Ook is er een &quot;Beschrijf&quot;-methode die een lijst bevat met beschikbare velden voor elk type en een lijst met velden die worden gebruikt voor deduplicatie (voor niet-hoofdobjecten), en die velden bevat waarop kan worden gezocht naar opvragen van records. De rijkste set wordt geleverd voor lood, aangezien deze de grootste verscheidenheid aan mogelijkheden binnen Marketo-toepassingen hebben.

## API

Voor een volledige lijst van het gegevensbestand API eindpunten van het Lood, met inbegrip van parameters, en modelleringsinformatie, zie de [ Verwijzing van het Eindpunt van het Gegevensbestand Lood API ](https://developer.adobe.com/marketo-apis/api/mapi/).

Voor instanties met een native CRM-integratie ingeschakeld (Microsoft Dynamics of Salesforce.com), zijn de bedrijf-, opportunity-, Opportunity Role- en Sales Person-API&#39;s uitgeschakeld. De records worden beheerd via de CRM wanneer deze is ingeschakeld en kunnen niet worden geopend of bijgewerkt via Marketo API&#39;s.

- Maximale batchgrootte (standaard): 300 records
- Max. batchgrootte (bulk): 10 MB bestand
- Standaardpartijgrootte: 300 records
- Koptekst van type inhoud (standaard): application/json
- Koptekst van inhoudstype (bulk): multipart/form-data

## Beschrijven

Voor Leads, Companies, Opportunity, Roles, SalesPersons en Custom Objects wordt een beschrijvende API verschaft. Door dit aan te roepen, worden metagegevens voor het object opgehaald, alsmede een lijst met velden die beschikbaar zijn voor bijwerken en opvragen. Beschrijven is een cruciaal onderdeel van het ontwerpen van een goede integratie met Marketo. Deze biedt rijke metagegevens over hoe objecten kunnen en kunnen worden gebruikt en over hoe ze kunnen worden gemaakt, bijgewerkt en opgevraagd. Naast het beschrijf lood, keert elk van deze een lijst van sleutels beschikbaar voor `deduplication` in de `dedupeFields` reactieparameter terug. Een lijst met velden is beschikbaar als sleutels voor het opvragen in de parameter `searchableFields` response.

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

In dit voorbeeld is `dedupeFields` in feite een samengestelde sleutel. Dit betekent dat u in toekomstige updates en creaties, wanneer het gebruiken van de `dedupeFields` wijze, alle drie van `externalOpportunityId`, `leadId`, en `role` voor elke rol moet omvatten. De array `searchableFields` bevat ook een lijst met velden die beschikbaar zijn voor het opvragen van rolrecords. Dit omvat ook de samengestelde sleutel `externalOpportunityId`, `leadId` en `role` .

Er is ook een parameter voor veldreacties, die de naam van elk veld, de `displayName` bevat zoals deze wordt weergegeven in de gebruikersinterface van Marketo, het gegevenstype van het veld, of deze kan worden bijgewerkt na het maken en de lengte van het veld, indien van toepassing.

## Query

Databaseobjecten voor leads hebben allemaal hetzelfde basispatroon voor zoekopdrachten op basis van eenvoudige sleutels, waarbij slechts één veld wordt gebruikt.

```
GET /rest/v1/{type}.json?filterType={field to query}&filterValues={comma-separated list of possible values}
```

Voor alle voorwerpen behalve lood, kunt u {field to query} van doorzoekbareFields van de overeenkomstige beschrijf vraag selecteren, en een komma-gescheiden lijst van maximaal 300 waarden samenstellen. Er zijn ook deze optionele queryparameters:

- `batchSize` - Een geheel getal van het aantal resultaten dat moet worden geretourneerd. Standaard en Maximaal zijn 300.
- `nextPageToken` - Token dat door een vorige vraag voor het pagineren is teruggekeerd. Zie [ Paging Tokens ](paging-tokens.md) voor meer detail.
- `fields` - Een door komma&#39;s gescheiden lijst met veldnamen die voor elke record moeten worden geretourneerd. Zie de bijbehorende beschrijving voor een lijst met geldige velden. Als een bepaald veld wordt opgevraagd, maar niet wordt geretourneerd, wordt de waarde impliciet ingesteld op null.
- `_method` - Wordt gebruikt voor het verzenden van query&#39;s met de HTTP-methode POST. Zie de sectie _method=GET hieronder voor gebruik.

Bij een kort voorbeeld, laten wij het vragen van kansen bekijken:

```
GET /rest/v1/opportunities.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb
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

`filterType` die in deze vraag wordt gespecificeerd is &quot;idField&quot;en niet &quot;marketoGUID&quot;. Dit en &quot;dedupeFields&quot;zijn beide speciale gevallen, waar het gebied dat aan idField beantwoordt, of dedupeFields op deze manier kan worden aliased. &quot;marketoGUID&quot;is nog het resulterende raadplegingsgebied in de vraag, maar het is niet uitdrukkelijk plaats in de vraag. De velden en/of sets velden die worden aangegeven door de `idField` en `dedupeFields` van een objectbeschrijving, zijn altijd geldig `filterTypes` voor een query. Deze vraag zoekt naar verslagen die GUIDs aanpassen inbegrepen in filterValues, en keert om het even welke passende verslagen terug. Als er geen verslagen worden gevonden gebruikend deze methode, zal de reactie nog op succes wijzen, nochtans zal de resultaatserie leeg zijn, aangezien het onderzoek met succes werd uitgevoerd, maar er waren geen verslagen om terug te keren.

Als de reeks verslagen in de vraag 300 overschrijdt of `batchSize` die werd gespecificeerd, welke kleiner is, dan heeft de reactie een lid `moreResult` met een waarde van waar, en a `nextPageToken`, die in een verdere vraag kan worden omvat om meer van de reeks terug te winnen. Zie [ het Pagelen Tokens ](paging-tokens.md) voor meer details.

### Lange URI&#39;s

Soms, zoals wanneer het vragen door GUIDs, kan uw URI lang zijn en 8KB overschrijden die door de dienst REST wordt toegelaten. In dit geval moet u de HTTP POST-methode gebruiken in plaats van GET en een queryparameter `_method=GET` toevoegen. Bovendien moeten de rest vraagparameters in het POST lichaam als &quot;application/x-www-form-urlencoded&quot;koord worden overgegaan, en de bijbehorende Content-type kopbal overgaan.

```
POST /rest/v1/opportunities.json?_method=GET
```

```
Content-Type: application/x-www-form-urlencoded
```

```
filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb,544fb7f5-2ddf-4fca-ae32-7e6ef1415e9f,f1ba41a2-69d1-4a35-9807-0e159d66f2c9,f7521272-3331-4a89-a768-222baff2f894
```

Naast lange URI&#39;s is deze parameter ook vereist voor het opvragen van samengestelde sleutels.

### Samengestelde toetsen

Het patroon voor het opvragen van samengestelde sleutels is anders dan eenvoudige sleutels, omdat hiervoor een POST met een JSON-hoofdtekst moet worden ingediend. Dit is niet altijd nodig, alleen in die gevallen waarin een `dedupeFields` -optie met meerdere velden wordt gebruikt als de `filterType` . Momenteel worden samengestelde sleutels alleen gebruikt door Opportunity Roles en sommige aangepaste objecten. Bekijk een voorbeeld van een vraag voor de Rollen van de Opportunity met de samengestelde sleutel van `dedupeFields`:

```
POST /rest/v1/opportunities/roles.json?_method=GET
```

```json
{
   "filterType":"dedupeFields",
   "fields":[
      "marketoGuid",
      "externalOpportunityId",
      "leadId",
      "role"
   ],
   "input":[
      {
        "externalOpportunityId":"Opportunity1",
        "leadId": 1,
        "role": "Captain"
      },
      {
        "externalOpportunityId":"Opportunity2",
        "leadId": 1872,
        "role": "Commander"
      },
      {
        "externalOpportunityId":"Opportunity3",
        "leadId": 273891,
        "role": "Lieutenant Commander"
      }
   ]
}
```

De structuur van het JSON-object is meestal vlak en alle queryparameters voor query&#39;s met eenvoudige toetsen zijn geldige leden, behalve `filterValues` . In plaats van een filterwaarde is er een &quot;input&quot;-array van JSON-objecten, die elk een lid moeten hebben voor elk veld in de samengestelde sleutel. In dit geval zijn dat `externalOpportunityId` , `leadId` en `role` . Hiermee wordt een query voor `roles` uitgevoerd op de opgegeven invoer en worden de resultaten geretourneerd. Als de reactie een parameter met `moreResult=true` en een `nextPageToken` retourneert, moet u alle oorspronkelijke invoer en de `nextPageToken` opnemen om de query correct uit te voeren.

## Maken en bijwerken

Creeert en de updates voor loodgegevensbestandverslagen, worden allen uitgevoerd door POSTs met de organismen JSON. De interface voor Kansen, Rollen, de Voorwerpen van de Douane, Bedrijven, en SalesPersonen zijn het zelfde. De interface van de leider is een beetje anders, en je kunt er daar meer over lezen.

De enige vereiste parameter is een array met de naam `input` die maximaal 300 objecten bevat, elk met de velden die u als leden wilt invoegen/bijwerken. U kunt desgewenst ook een parameter `action` opnemen, die een van de volgende kan zijn: `createOnly`, `updateOnly` of `createOrUpdate` . Als de handeling wordt weggelaten, wordt de modus standaard ingesteld op `createOrUpdate` . `dedupeBy` is een andere optionele parameter die kan worden gebruikt wanneer de handeling is ingesteld op createOnly of `createOrUpdate` . `dedupeBy` kan `idField` of `dedupeFields` zijn. Als `idField` is geselecteerd, wordt de `idField` in de beschrijving gebruikt voor deduplicatie en moet deze worden opgenomen in elke record. De modus `idField` is niet compatibel met de modus `createOnly` . Als `dedupeFields` is geselecteerd, wordt de `dedupeFields` weergegeven in de gebruikte objectbeschrijving en moet elke record in de record worden opgenomen. Als de parameter `dedupeBy` wordt weggelaten, wordt de modus standaard ingesteld op `dedupeFields` .

Wanneer een lijst met veldwaarden wordt doorgegeven, wordt de waarde `null` of een lege tekenreeks als `null` naar de database geschreven.

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

Met uitzondering van de API voor leads, retourneren aanroepen om databaseobjecten lead te maken of bij te werken een `seq` -veld in elk object in de `result` -array. Het vermelde nummer komt overeen met de volgorde van de bijgewerkte record in het ingediende verzoek. Elk item retourneert de waarde van `idField` voor het objecttype en een `status` . Het statusveld geeft een van de volgende waarden aan: &quot;gemaakt&quot;, &quot;bijgewerkt&quot; of &quot;overgeslagen&quot;.  Als de status wordt overgeslagen, dan zal er ook een overeenkomstige &quot;redenen&quot;serie met één of meerdere redenvoorwerpen zijn die een code en een bericht omvatten, die erop wijzen waarom een verslag werd overgeslagen. Zie [ foutencodes ](error-codes.md) voor extra details.

### Verwijderen

De interface voor schrappingen is standaard voor de voorwerpen van het Gegevensbestand van de Lood naast lood. Naast invoer is er slechts één vereiste parameter `deleteBy,` die de waarde idField of dedupeFields kan hebben. Laten we eens kijken naar het verwijderen van aangepaste objecten.

```
POST /rest/v1/customobjects/{name}/delete.json
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
```

```json
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

`seq`, `status`, `marketoGUID`, en `reasons` zouden allen aan u op dit ogenblik moeten vertrouwd zijn.

Voor meer informatie over het werken met CRUD verrichtingen voor elk individueel objecten type, controleer hun respectieve pagina&#39;s.
