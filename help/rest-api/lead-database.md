---
title: "Lead Database"
feature: REST API, Database
description: "Bewerk de hoofddatabase."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1345'
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

Voor een volledig overzicht van de eindpunten van de Looddatabase-API, inclusief parameters, en modelleringsinformatie raadpleegt u de [Referentie voor eindpuntdatabase-API](https://developer.adobe.com/marketo-apis/api/mapi/).

Voor instanties met toegelaten een inheemse integratie van CRM (of de Dynamica van Microsoft of Salesforce.com), zijn het Bedrijf, de Kanaal, de Rol van de Kanaal, en de Persoon van de Verkoop APIs gehandicapt. De records worden beheerd via de CRM wanneer deze is ingeschakeld en kunnen niet worden geopend of bijgewerkt via Marketo API&#39;s.

- Maximale batchgrootte (standaard): 300 records
- Max. batchgrootte (bulk): 10 MB bestand
- Standaardpartijgrootte: 300 records
- Koptekst van type inhoud (standaard): application/json
- Koptekst van inhoudstype (bulk): multipart/form-data

## Beschrijven

Voor Leads, Companies, Opportunity, Roles, SalesPersons en Custom Objects wordt een beschrijvende API verschaft. Door dit aan te roepen, worden metagegevens voor het object opgehaald, alsmede een lijst met velden die beschikbaar zijn voor bijwerken en opvragen. Beschrijven is een cruciaal onderdeel van het ontwerpen van een goede integratie met Marketo. Deze biedt rijke metagegevens over hoe objecten kunnen en kunnen worden gebruikt en over hoe ze kunnen worden gemaakt, bijgewerkt en opgevraagd. Behalve Leads beschrijven, geeft elk van deze een lijst met beschikbare toetsen voor `deduplication` in de `dedupeFields` responsparameter. Een lijst met velden is beschikbaar als sleutels voor het opvragen in het dialoogvenster `searchableFields` responsparameter.

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

In dit voorbeeld: `dedupeFields` is eigenlijk een samengestelde sleutel. Dit betekent dat in de toekomst updates en creeert wanneer het gebruiken van `dedupeFields` modus, moet u alle drie de `externalOpportunityId`, `leadId`, en `role` voor elke rol. De `searchableFields` array, bevat ook de lijst met velden die beschikbaar zijn voor het opvragen van rolrecords. Dit omvat ook de samengestelde sleutel van `externalOpportunityId`, `leadId`, en `role`.

Er is ook een parameter van de gebiedsreactie, die de naam van elk gebied zal verstrekken, `displayName` zoals deze wordt weergegeven in de gebruikersinterface van Marketo, het gegevenstype van het veld, of het veld na het maken kan worden bijgewerkt en de lengte van het veld, indien van toepassing.

## Query

Databaseobjecten voor leads hebben allemaal hetzelfde basispatroon voor zoekopdrachten op basis van eenvoudige sleutels, waarbij slechts één veld wordt gebruikt.

```
GET /rest/v1/{type}.json?filterType={field to query}&filterValues={comma-separated list of possible values}
```

Voor alle voorwerpen behalve lood, kunt u uw {gebied aan vraag} van doorzoekbareFields van corresponderen selecteren beschrijven vraag, en een komma-gescheiden lijst van maximaal 300 waarden samenstellen. Er zijn ook deze optionele queryparameters:

- `batchSize` - Een geheel getal van het aantal resultaten dat moet worden geretourneerd. Standaard en Maximaal zijn 300.
- `nextPageToken` - Token dat door een vorige vraag voor het pagineren is teruggekeerd. Zie [Pagingtokens](paging-tokens.md) voor meer details.
- `fields` - Een door komma&#39;s gescheiden lijst met veldnamen die voor elke record moeten worden geretourneerd. Zie de bijbehorende beschrijving voor een lijst met geldige velden. Als een bepaald veld wordt opgevraagd, maar niet wordt geretourneerd, wordt de waarde impliciet ingesteld op null.
- `_method` - Gebruikt voor het voorleggen van vragen gebruikend de methode van HTTP van de POST. Zie de sectie _method=GET hieronder voor gebruik.

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

De `filterType` gespecificeerd in deze vraag is &quot;idField&quot;en niet &quot;marketoGUID&quot;. Dit en &quot;dedupeFields&quot;zijn beide speciale gevallen, waar het gebied dat aan idField beantwoordt, of dedupeFields op deze manier kan worden aliased. &quot;marketoGUID&quot;is nog het resulterende raadplegingsgebied in de vraag, maar het is niet uitdrukkelijk plaats in de vraag. De velden en/of sets velden die door de `idField` en `dedupeFields` van een objectbeschrijving altijd geldig is `filterTypes` voor een query. Deze vraag zoekt naar verslagen die GUIDs aanpassen inbegrepen in filterValues, en keert om het even welke passende verslagen terug. Als er geen verslagen worden gevonden gebruikend deze methode, zal de reactie nog op succes wijzen, nochtans zal de resultaatserie leeg zijn, aangezien het onderzoek met succes werd uitgevoerd, maar er waren geen verslagen om terug te keren.

Als de reeks verslagen in de vraag 300 of overschrijdt `batchSize` welke is gespecificeerd, welke kleiner is, dan heeft het antwoord een lid `moreResult` met de waarde true en een `nextPageToken`, die in een volgende vraag kan worden omvat om meer van de reeks terug te winnen. Zie [Pagingtokens](paging-tokens.md) voor meer informatie .

### Lange URI&#39;s

Soms, zoals wanneer het vragen door GUIDs, kan uw URI lang zijn en 8KB overschrijden die door de dienst REST wordt toegelaten. In dit geval moet u de methode HTTP-POST gebruiken in plaats van GET en een queryparameter toevoegen `_method=GET`. Bovendien moeten de rest vraagparameters in het lichaam van de POST als &quot;application/x-www-form-urlencoded&quot;koord worden overgegaan, en de bijbehorende Content-type kopbal overgaan.

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

Het patroon voor het opvragen van samengestelde sleutels is anders dan eenvoudige toetsen, omdat hiervoor een POST met een JSON-hoofdtekst moet worden ingediend. Dit is niet in alle gevallen noodzakelijk, alleen in die gevallen waarin `dedupeFields` optie met meerdere velden wordt gebruikt als `filterType`. Momenteel worden samengestelde sleutels alleen gebruikt door Opportunity Roles en sommige aangepaste objecten. Kijk naar een voorbeeld van een vraag voor de Rollen van de Kanaal met de samengestelde sleutel van `dedupeFields`:

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

De structuur van het JSON-object is meestal vlak en alle queryparameters voor query&#39;s met eenvoudige sleutels zijn geldige leden, behalve `filterValues`. In plaats van een filterwaarde is er een &quot;input&quot;-array van JSON-objecten, die elk een lid moeten hebben voor elk veld in de samengestelde sleutel; in dit geval zijn het de `externalOpportunityId`, `leadId`, en `role`. Hiermee wordt een query uitgevoerd voor `roles`, tegen de opgegeven inputs en retourneert de resultaten die hiermee overeenkomen. Als de reactie een parameter met terugkeert `moreResult=true`en `nextPageToken`, moet u alle oorspronkelijke invoer en de `nextPageToken` voor de query correct wordt uitgevoerd.

## Maken en bijwerken

Creeert en de updates voor loodgegevensbestandverslagen, worden allen uitgevoerd door POSTs met de organismen JSON. De interface voor Kansen, Rollen, de Voorwerpen van de Douane, Bedrijven, en SalesPersonen zijn het zelfde. De interface van de leider is een beetje anders, en je kunt er daar meer over lezen.

De enige vereiste parameter is een array die `input` met maximaal 300 objecten, elk met de velden die u als leden wilt invoegen/bijwerken. U kunt ook een `action` parameter die een van de volgende kan zijn: `createOnly`, `updateOnly`, of `createOrUpdate`. Als de handeling wordt weggelaten, wordt de modus standaard ingesteld op `createOrUpdate`. `dedupeBy` is een andere optionele parameter die kan worden gebruikt wanneer de handeling is ingesteld op createOnly of `createOrUpdate`. ` dedupeBy` kan `idField`, of `dedupeFields`. Indien `idField` wordt geselecteerd, dan `idField` in de beschrijving wordt gebruikt voor deduplicatie en moet in elke record worden opgenomen. `idField` mode is niet compatibel met `createOnly` -modus. Indien `dedupeFields` zijn geselecteerd, dan `dedupeFields` worden weergegeven in de objectbeschrijving die wordt gebruikt, en elke beschrijving moet in elke record worden opgenomen. Als de `dedupeBy` parameter wordt weggelaten, wordt de wijze gebrek aan `dedupeFields`.

Wanneer u een lijst met veldwaarden doorgeeft, geeft u de waarde `null`, of een lege tekenreeks, als `null`.

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

Met uitzondering van de API voor leads, retourneren aanroepen om databaseobjecten lead te maken of bij te werken een `seq` veld in elk object in het dialoogvenster `result` array. Het vermelde nummer komt overeen met de volgorde van de bijgewerkte record in het ingediende verzoek. Elk item retourneert de waarde van het gereedschap `idField` voor het objecttype en een `status`. Het statusveld geeft een van de volgende waarden aan: &quot;gemaakt&quot;, &quot;bijgewerkt&quot; of &quot;overgeslagen&quot;.  Als de status wordt overgeslagen, dan zal er ook een overeenkomstige &quot;redenen&quot;serie met één of meerdere redenvoorwerpen zijn die een code en een bericht omvatten, die erop wijzen waarom een verslag werd overgeslagen. Zie [foutcodes](error-codes.md) voor meer informatie.

### Verwijderen

De interface voor schrappingen is standaard voor de voorwerpen van het Gegevensbestand van de Lood naast lood. Naast invoer is er slechts één vereiste parameter `deleteBy,` die de waarde idField of dedupeFields kunnen hebben. Laten we eens kijken naar het verwijderen van aangepaste objecten.

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

De `seq`, `status`, `marketoGUID`, en `reasons` moeten jullie nu allemaal bekend zijn.

Voor meer informatie over het werken met CRUD verrichtingen voor elk individueel objecten type, controleer hun respectieve pagina&#39;s.
