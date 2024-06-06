---
title: "Leads"
feature: REST API
description: "Details over de API-aanroepen voor leads"
source-git-commit: aea2812730fa5f6054e69dfa9d8045329aa724c7
workflow-type: tm+mt
source-wordcount: '3308'
ht-degree: 0%

---


# Leads

[Referentie van eindpunt voor lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads)

De API van Marketo Lead biedt een groot aantal mogelijkheden voor eenvoudige CRUD-toepassingen tegen leadrecords, maar ook de mogelijkheid om het lidmaatschap van een lead in statische lijsten en programma&#39;s te wijzigen en de verwerking van Smart Campagne voor leads te starten.

## Beschrijven

Een van de belangrijkste mogelijkheden van de API voor leads is de Describe-methode. Gebruik Beschrijf Leads om een volledige lijst op te halen van de velden die beschikbaar zijn voor interactie via zowel de REST API als de SOAP API, en metagegevens voor elke velden:

* Gegevenstype
* REST- en SOAP API-namen
* Lengte (indien van toepassing)
* Alleen-lezen
* Friendly label

Beschrijf is de primaire bron van waarheid voor of de gebieden voor gebruik, en meta-gegevens over die gebieden beschikbaar zijn.

### Verzoek

```
GET /rest/v1/leads/describe.json
```

### Antwoord

```json
{  
   "requestId":"37ca#1475b74e276",
   "success":true,
   "result":[  
      {  
         "id":2,
         "displayName":"Company Name",
         "dataType":"string",
         "length":255,
         "rest":{  
            "name":"company",
            "readOnly":false
         },
         "soap":{  
            "name":"Company",
            "readOnly":false
         }
      }
}
```

Normaal, omvatten de reacties een veel grotere reeks gebieden in de resultaatserie, maar wij weglaten hen voor demonstratiedoeleinden. Elk item in de resultaatarray komt overeen met een veld dat beschikbaar is in de lead record en heeft minimaal een id, een displayName en een datatype. De rest en de zeep onderliggende voorwerpen kunnen voor een bepaald gebied aanwezig zijn, en zijn aanwezigheid zal erop wijzen of het gebied voor gebruik in of REST of SOAP APIs geldig is. De `readOnly` geeft aan of het veld alleen-lezen is via de corresponderende API (REST of SOAP). De eigenschap length geeft de maximale lengte van het veld aan, indien aanwezig. De eigenschap dataType geeft het gegevenstype van het veld aan.

## Query

Er zijn twee primaire methoden voor het ophalen van leads: de methoden Get Lead by Id en Get Leads by Filter Type. Krijg lood door identiteitskaart neemt één enkele lood identiteitskaart als wegparameter en keert één enkel lood verslag terug.

U kunt desgewenst een veldparameter doorgeven die een door komma&#39;s gescheiden lijst met veldnamen bevat die moet worden geretourneerd. Als de parameter fields niet is opgenomen in deze aanvraag, worden de volgende standaardvelden geretourneerd: `email`, `updatedAt`, `createdAt`, `lastName`, `firstName`, en `id`. Als u een lijst met velden aanvraagt en een bepaald veld wordt opgevraagd, maar niet wordt geretourneerd, wordt de waarde impliciet ingesteld op null.

### Verzoek

```
GET /rest/v1/lead/{id}.json
```

### Antwoord

```json
{
   "requestId": "10226#14d3049e51b",
   "success": true,
   "result": [
      {
         "id": 318581,
         "updatedAt":"2015-05-07T11:47:30-08:00"
         "lastName": "Doe",
         "email": "jdoe@marketo.com",
         "createdAt": "2015-05-01T16:47:30-08:00",
         "firstName": "John"
      }
   ]
}
```

Voor deze methode is er altijd één record in de eerste positie van de resultaatarray.

Met de optie Leads ophalen op filtertype wordt hetzelfde type record geretourneerd, maar kan tot 300 per pagina worden geretourneerd. Het vereist `filterType` en `filterValues` queryparameters.

`filterType` Accepteert om het even welk Geheime Gebied van de Douane, of de meeste algemeen gebruikte gebieden. Roep de `Describe2` eindpunt voor een uitgebreide lijst van doorzoekbare velden die zijn toegestaan voor gebruik in `filterType`. Bij het zoeken op Aangepast veld worden alleen de volgende gegevenstypen ondersteund: `string`, `email`, `integer`. U kunt velddetails verkrijgen (beschrijving, type, enz.) met behulp van de bovengenoemde methode Describe.

`filterValues` Accepteert maximaal 300 waarden in komma-gescheiden formaat. De vraagonderzoeken naar verslagen waar het gebied van de lood één van inbegrepen aanpast `filterValues`. Als het aantal leads dat overeenkomt met het hoofdfilter groter is dan 1.000, wordt een fout geretourneerd: &quot;1003, Te veel resultaten komen overeen met het filter&quot;.

Als de totale lengte van uw GET- verzoek 8KB overschrijdt, is een fout van HTTP teruggekeerd: &quot;414, URI te lang&quot;(per RFC 7231). Als tussenoplossing kunt u uw GET in POST veranderen, de parameter _method=GET toevoegen, en een vraagkoord in het verzoeklichaam plaatsen.

### Verzoek

```
GET /rest/v1/leads.json?filterType=id&filterValues=318581,318592
```

### Antwoord

```json
{
    "requestId": "12951#15699db5c97",
    "result": [
        {
            "id": 318581,
            "updatedAt": "2016-05-17T22:11:45Z",
            "lastName": "Lincoln",
            "email": "abe@usa.gov",
            "createdAt": "2015-03-17T00:18:40Z",
            "firstName": "Abraham"
        },
        {
            "id": 318592,
            "updatedAt": "2016-05-17T22:20:51Z",
            "lastName": "Washington",
            "email": "george@usa.gov",
            "createdAt": "2015-04-06T16:29:21Z",
            "firstName": "George"
        }
    ],
    "success": true
}
```

Deze oproep zoekt naar records die overeenkomen met de id&#39;s die zijn opgenomen in `filterValues`en retourneert alle overeenkomende records.

Als er geen records worden gevonden, geeft de reactie aan dat de array is gelukt, maar is de array result leeg.

### Antwoord

```json
{
"requestId": "177a1#1578b643357",
"result": [],
"success": true
}
```

Zowel krijgen lood door Identiteitskaart als krijgt lood door het Type van Filter zal ook een parameter van de gebiedsvraag goedkeuren, die een komma gescheiden lijst van API gebieden goedkeurt. Als dit is opgenomen, worden bij elke record in het antwoord de weergegeven velden opgenomen.  Als deze wordt weggelaten, wordt een standaardset velden geretourneerd: `id`, `email`, `updatedAt`, `createdAt`, `firstName`, en `lastName`.

## Adobe-ECID

Als de functie voor delen van publiek via Adobe Experience Cloud is ingeschakeld, wordt een cookie gesynchroniseerd waarbij de Adobe Experience Cloud-id (ECID) aan Marketo-leads wordt gekoppeld.  De hierboven vermelde methoden voor het ophalen van leads kunnen worden gebruikt om bijbehorende ECID-waarden op te halen.  Dit doet u door &quot;ecids&quot; op te geven in de parameter fields. Bijvoorbeeld &quot;&amp;fields=email,firstName,lastName,ecids&quot;.

## Maken en bijwerken

Naast het ophalen van gegevens voor leads kunt u ook een lead record maken, bijwerken en verwijderen via de API. Het creëren van en het bijwerken van lood delen het zelfde eindpunt met het verrichtingstype dat in het verzoek wordt bepaald, en tot 300 verslagen kunnen tezelfdertijd worden gecreeerd of worden bijgewerkt.

### Verzoek

```
POST /rest/v1/leads.json
```

### Lichaam

```json
{  
   "action":"createOnly",
   "lookupField":"email",
   "input":[  
      {  
         "email":"kjashaedd-1@klooblept.com",
         "firstName":"Kataldar-1",
         "postalCode":"04828"
      },
      {  
         "email":"kjashaedd-2@klooblept.com",
         "firstName":"Kataldar-2",
         "postalCode":"04828"
      },
      {  
         "email":"kjashaedd-3@klooblept.com",
         "firstName":"Kataldar-3",
         "postalCode":"04828"
      }
   ]
}
```

### Antwoord

```json
{  
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[  
      {  
         "id":50,
         "status":"created"
      },
      {  
         "id":51,
         "status":"created"
      },
      {  
         "id":52,
         "status":"created"
      }
   ]
}
```

In dit verzoek ziet u twee belangrijke gebieden: `action` en `lookupField`.  `action` geeft het type bewerking van de aanvraag aan en kan `createOrUpdate`, `createOnly`, `updateOnly`, of `createDuplicate`. Als deze wordt weggelaten, wordt de actie standaard ingesteld op `createOrUpdate`.  De `lookupField` parameter geeft de toets aan die moet worden gebruikt wanneer de handeling een van de `createOrUpdate` of `updateOnly`. Indien `lookupField` wordt weggelaten, is de standaardsleutel `email`.

Standaard wordt de standaardpartitie gebruikt. U kunt desgewenst de optie `partitionName` parameter, die alleen werkt als de handeling `createOnly` of `createOrUpdate`. Voor `partitionName` om als extra deduplicatiecriteria te werken, moet het deel van brontype in douanededuplicatieregels uitmaken. Als tijdens een updatebewerking geen lead bestaat in de opgegeven partitie, wordt een fout geretourneerd. Als de gebruiker met alleen de API geen toestemming heeft om de opgegeven partitie te openen, wordt een fout geretourneerd.

De `id` veld kan alleen als parameter worden opgenomen wanneer het veld `updateOnly` handeling, als `id` is een door het systeem beheerde unieke sleutel.

Het verzoek moet ook een `input` parameter, die een array van lead records is. Elke lead-record is een JSON-object met een willekeurig aantal lead-velden. De sleutels in een record moeten uniek zijn voor die record en alle JSON-tekenreeksen moeten met UTF-8 zijn gecodeerd. De `externalCompanyId` kan worden gebruikt om de loodrecord te koppelen aan een bedrijfsrecord. De `externalSalesPersonId` kan worden gebruikt om de loodrecord te koppelen aan een record van een verkooppersoon.

Opmerking: wanneer u meerdere aanvragen voor het uploaden van leads tegelijkertijd of snel achter elkaar uitvoert, kunnen dubbele records ontstaan wanneer u meerdere aanvragen met dezelfde sleutelwaarde indient als een volgende aanroep met dezelfde waarde wordt uitgevoerd voordat de eerste retourneert. Dit kan worden vermeden door het `createOnly`, of `updateOnly` zoals aangewezen, of door vraag een rij te vormen en op uw vraag te wachten terug alvorens verdere upsert vraag met de zelfde sleutel te maken.

## Velden

Het hoofdobject bevat standaardvelden en eventueel aangepaste velden. In elk abonnement op een Marketo Engage staan standaardvelden, terwijl de gebruiker aangepaste velden maakt. Elke velddefinitie bestaat uit een set kenmerken die het veld beschrijven. Voorbeelden van kenmerken zijn weergavenaam, API-naam en dataType. Deze kenmerken worden gezamenlijk metagegevens genoemd.

Met de volgende eindpunten kunt u velden in het hoofdobject opvragen, maken en bijwerken. Deze APIs vereist dat de het bezitten gebruiker API een rol met één of allebei van het Gelezen-Schrijf StandaardGebied van het Schema of Gelezen-Schrijf de toestemmingen van het Gebied van het Schema van de Douane heeft.

## Query-velden

Het opvragen van loodvelden is eenvoudig. U kunt één leadveld opvragen op API-naam of een query uitvoeren op de set met alle loden velden. Zowel standaardvelden als aangepaste velden kunnen worden opgehaald, afhankelijk van de rolmachtigingen die worden gebruikt. Verborgen velden worden ook opgehaald.

## Op naam

Met het eindpunt Lead ophalen op naam haalt u metagegevens op voor één veld op het hoofdobject. De vereiste padparameter fieldApiName geeft de API-naam van het veld op. De reactie is als beschrijf het Punt van de Lood maar bevat extra meta-gegevens zoals het isCustom attribuut, dat erop wijst of het gebied een douanegebied is.

### Verzoek

```
GET /rest/v1/leads/schema/fields/{fieldApiName}.json
```

### Antwoord

```json
{
    "requestId": "cd97#1793ee0fec4",
    "result": [
        {
            "displayName": "Email Address",
            "name": "email",
            "description": null,
            "dataType": "email",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        }
    ],
    "success": true
}
```

## Bladeren

Met het eindpunt Ophalen worden metagegevens opgehaald voor alle velden in het hoofdobject, inclusief. Standaard worden maximaal 300 records geretourneerd. U kunt de `batchSize` query parameter om dit aantal te verminderen. Als de `moreResult` Het kenmerk is true, dit betekent dat er meer resultaten beschikbaar zijn. Ga door met het aanroepen van dit eindpunt tot de `moreResult` kenmerk retourneert false, wat betekent dat er geen resultaten beschikbaar zijn. De `nextPageToken` is teruggekeerd van deze API zou altijd voor de volgende herhaling van deze vraag moeten worden opnieuw gebruikt.

### Verzoek

```
GET /rest/v1/leads/schema/fields.json
```

### Respons (gekort)

```json
{
    "requestId": "142c3#1793eb976d8",
    "result": [
        {
            "displayName": "Salutation",
            "name": "salutation",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "First Name",
            "name": "firstName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Middle Name",
            "name": "middleName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Last Name",
            "name": "lastName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Date of Birth",
            "name": "dateOfBirth",
            "description": null,
            "dataType": "date",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Email Address",
            "name": "email",
            "description": null,
            "dataType": "email",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Phone Number",
            "name": "phone",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Mobile Phone Number",
            "name": "mobilePhone",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Fax Number",
            "name": "fax",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Job Title",
            "name": "title",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Unsubscribed",
            "name": "unsubscribed",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": true,
            "isCustom": false
        },
        ...
    ],
    "success": true,
    "moreResult": false
}
```

## Velden maken

Met het eindpunt Voorloopvelden maken maakt u een of meer aangepaste velden op het hoofdobject. Dit eindpunt verstrekt functionaliteit die aan wat in het Marketo Engage UI beschikbaar is vergelijkbaar is. Met dit eindpunt kunt u maximaal 100 aangepaste velden maken.
Houd zorgvuldig rekening met elk veld dat u maakt in de productie-instantie van een Marketo Engage met behulp van de API.  Nadat een veld is gemaakt, kunt u het niet verwijderen (u kunt het alleen verbergen). De proliferatie van ongebruikte gebieden is een slechte praktijk die uw geval zal bemoeilijken.

De vereiste invoerparameter is een array van hoofdobjecten. Elk object bevat een of meer kenmerken. De vereiste kenmerken zijn `displayName`, `name`, en `dataType` die overeenkomen met de weergavenaam van de gebruikersinterface van het veld, de API-naam van het veld en het veldtype.  U kunt desgewenst opgeven `description`, `isHidden`, `isHtmlEncodingInEmail`, en `isSensitive`.

Er zijn een paar regels verbonden aan naam en `displayName` naamgeving. Het kenmerk name moet uniek zijn, beginnen met een letter en mag alleen letters, cijfers of onderstrepingsteken bevatten. De `displayName` moet uniek zijn en mag geen speciale tekens bevatten.  Een algemene naamgevingsconventie is het toepassen van camelcase op `displayName` om naam te produceren. Bijvoorbeeld een `displayName` van &quot;Mijn Douane Gebied&quot;zou een naam van &quot;myCustomField&quot;veroorzaken.

### Verzoek

```
POST /rest/v1/leads/schema/fields.json
```


### Lichaam

```json
{
  "input": [
      {
        "displayName": "Acme Access Code",
        "name": "acmeAccessCode",
        "description": "Acme Direct Mail Integration",
        "dataType": "string"
      },
      {
        "displayName": "Acme Mail Date",
        "name": "acmeMailDate",
        "description": "Acme Direct Mail Integration",
        "dataType": "string"
      }
  ]
}
```


### Antwoord

```json
{
    "requestId": "d9f1#17943666811",
    "result": [
        {
            "name": "acmeAccessCode",
            "status": "created"
        },
        {
            "name": "acmeMailDate",
            "status": "created"
        }
    ],
    "success": true
}
```

## Veld bijwerken

Het eindpunt van het Gebied van de Lood van de Update werkt één enkel douaneveld op het loodvoorwerp bij. Voor het grootste deel zijn bewerkingen voor veldupdates die worden uitgevoerd met de interface van het Marketo Engage, haalbaar met behulp van de API. In de onderstaande tabel zijn enkele verschillen samengevat.

<table>
<tbody>
<tr>
<td style="width: 26.5306%;" rowspan="2"><strong>Kenmerk</strong></td>
<td style="width: 35%;" colspan="2"><strong>Standaardveld</strong></td>
<td style="width: 38.2654%;" colspan="2"><strong>Aangepast veld</strong></td>
</tr>
<tr>
<td style="width: 17.449%;"><strong>Kan worden bijgewerkt door API?</strong></td>
<td style="width: 17.551%;"><strong>Kan worden bijgewerkt via gebruikersinterface?</strong></td>
<td style="width: 19.3878%;"><strong>Kan worden bijgewerkt door API?</strong></td>
<td style="width: 18.8776%;"><strong>Kan worden bijgewerkt via gebruikersinterface?</strong></td>
</tr>
<tr>
<td style="width: 26.5306%;">dataType</td>
<td style="width: 17.449%;">nee</td>
<td style="width: 17.551%;">nee</td>
<td style="width: 19.3878%;">nee</td>
<td style="width: 18.8776%;">ja</td>
</tr>
<tr>
<td style="width: 26.5306%;">beschrijving</td>
<td style="width: 17.449%;">ja</td>
<td style="width: 17.551%;">ja</td>
<td style="width: 19.3878%;">ja</td>
<td style="width: 18.8776%;">ja</td>
</tr>
<tr>
<td style="width: 26.5306%;">displayName</td>
<td style="width: 17.449%;">nee</td>
<td style="width: 17.551%;">nee</td>
<td style="width: 19.3878%;">ja</td>
<td style="width: 18.8776%;">ja</td>
</tr>
<tr>
<td style="width: 26.5306%;">isCustom</td>
<td style="width: 17.449%;">nee</td>
<td style="width: 17.551%;">nee</td>
<td style="width: 19.3878%;">nee</td>
<td style="width: 18.8776%;">nee</td>
</tr>
<tr>
<td style="width: 26.5306%;">isHidden</td>
<td style="width: 17.449%;">nee</td>
<td style="width: 17.551%;">ja</td>
<td style="width: 19.3878%;">yes (indien gemaakt door API)</td>
<td style="width: 18.8776%;">ja</td>
</tr>
<tr>
<td style="width: 26.5306%;">isHtmlEncodingInEmail</td>
<td style="width: 17.449%;">ja</td>
<td style="width: 17.551%;">ja</td>
<td style="width: 19.3878%;">ja</td>
<td style="width: 18.8776%;">ja</td>
</tr>
<tr>
<td style="width: 26.5306%;">isSensitive</td>
<td style="width: 17.449%;">ja</td>
<td style="width: 17.551%;">ja</td>
<td style="width: 19.3878%;">ja</td>
<td style="width: 18.8776%;">ja</td>
</tr>
<tr>
<td style="width: 26.5306%;">length</td>
<td style="width: 17.449%;">nee</td>
<td style="width: 17.551%;">nee</td>
<td style="width: 19.3878%;">nee</td>
<td style="width: 18.8776%;">nee</td>
</tr>
<tr>
<td style="width: 26.5306%;">name</td>
<td style="width: 17.449%;">nee</td>
<td style="width: 17.551%;">nee</td>
<td style="width: 19.3878%;">nee</td>
<td style="width: 18.8776%;">nee</td>
</tr>
</tbody>
</table>

De vereiste `fieldApiName` path parameter specifies the API name of the field to update. De vereiste invoerparameter is een array die één hoofdobject in het veld bevat.  Het veldobject bevat een of meer kenmerken.

### Verzoek

```
POST /rest/v1/leads/schema/fields/{fieldApiName}.json
```

### Lichaam

```json
{
  "input": [
      {
        "displayName": "Acme Access Code",
        "description": "Acme Direct Mail Integration",
        "isHtmlEncodingInEmail": true
      }
  ]
}
```

### Antwoord

```json
{
    "requestId": "9f57#1794324f44c",
    "result": [
        {
            "name": "acmeAccessCode",
            "status": "updated"
        }
    ],
    "success": true
}
```

## Regelafstand naar Marketo

Push Lead is een alternatief voor het synchroniseren van leads naar Marketo, vooral ontworpen om een grotere mate van triggercapaciteit mogelijk te maken dan de standaard Sync Leads (vergelijkbaar met een Marketo-formulier). Naast synchronisatie van loodgebieden, staat dit eindpunt voor loodvereniging toe die op koekjeswaarden wordt gebaseerd, die tot het eindpunt worden overgegaan. Dit doet u door het `mkt_tok` waarde door door Marketo e-mail wordt geproduceerd te klikken, of door een programmanaam in de vraag over te gaan die. Dit eindpunt leidt ook tot één enkele trekkerbare activiteit, die aan een programma en/of campagne in Marketo wordt geassocieerd. Hierdoor kunnen gebeurtenissen voor het vastleggen van leads worden geactiveerd die worden toegeschreven aan een specifieke campagne of een specifiek programma om de bijbehorende workflows uit Marketo op te starten.

De interface Push Lead lijkt sterk op Leads synchroniseren. Alle primaire sleutels zijn geldig, en de zelfde API namen worden gebruikt voor gebieden (er is geen actieparameter omdat dit altijd een opseringsverrichting is). De `programName` en de invoerparameters vereist zijn, en `lookupField`, `source`, en `reason` parameters zijn optioneel. De invoerparameter is een array van lead-objecten. De resulterende activiteit wordt toegeschreven aan het corresponderende benoemde programma. De `source` en `reason` parameters zijn willekeurige tekenreeksvelden die kunnen worden toegevoegd aan het verzoek om die waarden in te sluiten in de resulterende activiteiten. Deze kunnen worden gebruikt als restricties in de corresponderende triggers (Lood wordt naar Marketo geduwd) en filters (Lood werd naar Marketo geduwd).

Opmerking over anonieme activiteiten. Als u eerdere anonieme activiteiten wilt koppelen aan de nieuwe lead, geeft u geen cookies-kenmerk op in het hoofdobject en roept u Lead koppelen na pushlead aan. Als u een nieuwe lood zonder activiteitengeschiedenis wilt tot stand brengen, dan specificeer eenvoudig de koekjesattributen in het lood voorwerp.

### Verzoek

```
POST /rest/v1/leads/push.json
```

### Lichaam

```json
{
    "programName": "Big Blue Thing Product Launch",
    "source": "Cool Sales Site",
    "reason": "Downloaded pricing sheet",
    "lookupField": "email",
    "input": [
        {
             "email": "Theresa.May@westminister.gov.uk",
             "country": "united kingdom",
             "firstName": "Theresa",
             "website": "www.brexit.com",
             "leadScore": 45,
             "marketoSocialFacebookProfileURL": "http://www.facebook.com/id/23434456",
             "jobTitle": "Prime Minister"
         },
         {
             "email": "Justin.Trudeau@ottowa.gov.ca",
             "country": "canada",
             "firstName": "Justin",
             "website": "www.take-off-eh.com",
             "leadScore": 92,
             "marketoSocialFacebookProfileURL": "http://www.facebook.com/id/42434",
             "jobTitle": "Sonny"
         }
     ]
}
```

### Antwoord

```json
{
    "requestId": "939079529805",
    "success": true,
    "warnings": [],
    "result": [
       {
           "id": 483894,
           "status": "created"
       },
       {
           "id": 1087425,
           "status": "updated"
       },
       {
           "id": 3525,
           "reasons": [
                    {
                        "code": "501",
                        "message": "Bad stuff happened"
                    }
           ]
       }
    ]
}
```

Als u de opdracht `mkt_tok` -parameter, wijst u als volgt de waarde toe aan het lid marketToken in een lead record in de invoerparameter.

### Lichaam

```json
{
  "programName": "Big Blue Thing Product Launch",
  "source": "Cool Sales Site",
  "reason": "Downloaded pricing sheet",
  "lookupField": "mktToken",
  "input" : [
     {
       "mktToken" : "<tokenValue>",
       "firstName" : "Thelma"
     },
     {
       "mktToken" : "<tokenValue>",
       "firstName" : "Louise"
     }
   ]
}
```

## Formulier verzenden

Verzendformulier is een alternatief voor het synchroniseren van leads naar Marketo en is ontworpen voor functionaliteit die gelijk is aan het verzenden van een Marketo-formulier. Hierdoor kunnen gebeurtenissen voor het vastleggen van leads worden geactiveerd die worden toegeschreven aan een specifieke campagne of een specifiek programma om de bijbehorende workflows uit Marketo op te starten.

Het eindpunt van de Vorm van de Verzending steunt de volgende functionaliteit:

* Hiermee wordt een lead-record bijgewerkt met het e-mailveld als primaire sleutel
* Hiermee maakt u een activiteit Formulier invullen die aan een programma en/of campagne is gekoppeld
* Hiermee wordt een koppeling naar een cookie toegestaan
* Hiermee wordt formulierveldvalidatie uitgevoerd

Bij het verzenden van een formulier wordt het standaarddatabasepatroon voor leads gevolgd. Er wordt één objectrecord doorgegeven in het vereiste invoerlid van de JSON-hoofdtekst van een POST-aanvraag. De vereiste `formId` lid bevat de doel-Marketo-formulier-id.

De optionele `programId` kan worden gebruikt om het programma op te geven waaraan de lead moet worden toegevoegd en/of om het programma op te geven waaraan aangepaste velden voor programmaleden moeten worden toegevoegd. Indien `programId` wordt verstrekt, wordt het lood toegevoegd aan het programma en om het even welke gebieden van de programmaleden die in vorm aanwezig zijn worden ook toegevoegd. Het opgegeven programma moet zich in dezelfde werkruimte bevinden als het formulier. Als het formulier geen aangepaste velden voor programmaleden bevat en `programId` niet wordt opgegeven, wordt de lead niet aan een programma toegevoegd. Indien het formulier zich in een programma bevindt en `programId` niet wordt verstrekt, wordt dat programma gebruikt wanneer één of meerdere gebieden van het programmalid in vorm aanwezig zijn.

In de invoerrecord worden de `leadFormFields` -object is vereist. Dit object bevat een of meer naam-/waardeparen die overeenkomen met de formuliervelden die moeten worden ingevuld.  Alle opgegeven velden moeten binnen het opgegeven formulier worden gedefinieerd. De naam is de REST API-naam voor het veld. Let erop dat de `email` is vereist.

De `visitorData` Het lidvoorwerp is facultatief en bevat naam/waardeparen die aan pagina-bezoek gegevens met inbegrip van beantwoorden `pageURL`, `queryString`, `leadClientIpAddress`, en `userAgentString`. Kan worden gebruikt om aanvullende activiteitsvelden te vullen voor filteren en activeren.

De lidtekenreeks van het cookie is optioneel en kunt u een Munchkin-cookie koppelen aan een persoonrecord in Marketo. Wanneer een nieuwe lood wordt gecreeerd, worden om het even welke vroegere anonieme activiteiten geassocieerd met die lood, tenzij de koekjeswaarde eerder met een ander gekend verslag was geassocieerd. Als de waarde van het cookie eerder gekoppeld was, worden nieuwe activiteiten bijgehouden op basis van de record, maar worden oude activiteiten niet van de bestaande bekende record verwijderd. Als u een nieuwe lead zonder activiteitengeschiedenis wilt maken, laat u gewoon het cookielid weg.

Nieuwe leads worden gemaakt in de primaire partitie voor de werkruimte waarin het formulier zich bevindt.

### Verzoek

```
POST /rest/v1/leads/submitForm.json
```

### Koptekst

```
Content-Type: application/json
```

### Lichaam

```json
{
  "formId": 1029,
  "input": [
    {
      "leadFormFields": {
        "firstName": "Marge",
        "lastName": "Simpson",
        "email": "marge.simpson@fox.com",
        "pMCFField": "PMCF value"
      },
      "visitorData": {
        "pageURL": "https://na-sjst.marketo.com/lp/063-GJP-217/UnsubscribePage.html",
        "queryString": "Unsubscribed=yes",
        "leadClientIpAddress": "192.150.22.5",
        "userAgentString": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.89 Safari/537.36"
      },
      "cookie": "id:063-GJP-217&token:_mch-marketo.com-1594662481190-60776"
    }
  ]
}
```

### Antwoord

```json
{
  "requestId": "10667#173bc585ca5",
  "result": [
    {
      "id": 319174,
      "status": "updated"
    }
  ],
  "success": true
}
```

Hier kunnen de overeenkomstige activiteitendetails van het formulier invullen worden weergegeven vanuit de gebruikersinterface van het Marketo Engage:

![Gebruikersinterface Formulier invullen](assets/fill_out_form_activity_details.png)

## Samenvoegen

Soms is het nodig dubbele records samen te voegen en Marketo vereenvoudigt dit via de Merge Leads-API. Bij het samenvoegen van leads worden hun activiteitenlogbestanden, programma&#39;s, campagnes en lijstlidmaatschappen en CRM-informatie gecombineerd, en worden alle veldwaarden samengevoegd tot één record. De Leads van de fusie neemt loodidentiteitskaart als wegparameter, en of één van beiden `leadId` als een queryparameter, of een lijst met door komma&#39;s gescheiden id&#39;s in het dialoogvenster `leadIds` parameter.

### Verzoek

```
POST /rest/v1/leads/{id}/merge.json?leadId=1324
```

### Antwoord

```json
{  
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

De lead die is opgegeven in de padparameter, is de winnende lead. Als er dus velden zijn die conflicteren tussen de records die worden samengevoegd, wordt de waarde van de winnaar gebruikt, behalve als het veld in de winnende record leeg is en het corresponderende veld in de laatste record niet. De leads die worden opgegeven in `leadId` of `leadIds` parameter zijn de verliezende leads .

Als u een SFDC-sync-toegelaten abonnement hebt, dan kunt u ook gebruiken `mergeInCRM` in uw verzoek. Indien ingesteld op true, wordt de corresponderende samenvoeging in uw CRM ook uitgevoerd. Als beide leads zich in SFDC bevinden en de ene een CRM-lead is en de andere een CRM-contactpersoon, is de winnaar de CRM-contactpersoon (ongeacht welke lead als winnaar is opgegeven). Als een van de leads zich in SFDC bevindt en de andere alleen Marketo is, dan is de winnaar de SFDC-lead (ongeacht welke lead als winnaar is opgegeven).

## Webactiviteit koppelen

Marketo registreert via Lead Tracking (Munchkin) webactiviteiten voor bezoekers van uw website en Marketo Landing Pages. Deze activiteiten, bezoeken en klikken, worden vastgelegd met een sleutel die overeenkomt met een cookie &quot;_mkto_trk&quot; die is ingesteld in de browser van de lead. Marketo gebruikt deze code om de activiteiten van dezelfde persoon bij te houden. Normaal, komt de vereniging aan loodverslagen voor wanneer een lood door van een e-mail van Marketo klikt of een vorm van Marketo invult, maar soms kan een vereniging door een verschillend type van gebeurtenis worden teweeggebracht, en u kunt het Associate eindpunt van de Lood daartoe gebruiken. Het eindpunt neemt bekende identiteitskaart van het loodverslag als wegparameter en de &quot;_mkto_trk&quot;koekjeswaarde in de koekjesvraagparameter.

### Verzoek

```
POST /rest/v1/leads/{id}/associate.json?cookie=id:287-GTJ-838%26token:_mch-marketo.com-1396310362214-46169
```

### Antwoord

```json
{  
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Als een cookie al is gekoppeld aan een bekende lead record, zorgt het gebruik van deze API in een andere lead record ervoor dat nieuwe webactiviteit wordt opgenomen op basis van die record, maar wordt bestaande webactiviteit niet naar de nieuwe record verplaatst.
Lidmaatschap

De dossiers van de lood kunnen ook worden teruggewonnen gebaseerd op lidmaatschap in een statische lijst, of een programma. Daarnaast kunt u alle statische lijsten, programma&#39;s of slimme campagnes ophalen waarvan een lead lid is.

De responsstructuur en optionele parameters zijn identiek aan die van Get Leads door Filtertype, hoewel filterType en filterValues niet kunnen worden gebruikt met deze API.
Navigeer naar de lijst als u de lijst-id wilt openen via de gebruikersinterface van Marketo. De lijst `id` bevindt zich in de URL van de statische lijst; `https://app-****.marketo.com/#ST1001A1`. In dit voorbeeld is 1001 de `id` voor de lijst.

### Verzoek

```
GET /rest/v1/list/{listId}/leads.json?batchSize=3
```

### Antwoord

```json
{ 
   "requestId":"e42b#14272d07d78",
   "success":true,
   "nextPageToken":
"PS5VL5WD4UOWGOUCJR6VY7JQO2KUXL7BGBYXL4XH4BYZVPYSFBAONP4V4KQKN4SSBS55U4LEMAKE6===",
    "result":[
       {
            "id":50,  
            "email":"kjashaedd@klooblept.com",
            "firstName":"Kataldar",
             "postalCode":"04828"
       },
       {
           "id":2343,
           "email":"kjashaedd@klooblept.com",
           "firstName":"Kataldar",
           "postalCode":"04828" 
       },
      {
           "id":88498,
           "email":"kjashaedd@klooblept.com", 
           "firstName":"Kataldar",
         "postalCode":"04828"
         }
    ]
}
```

De Get Lijsten door het eindpunt van identiteitskaart van de Lood neemt een loodverslag `id` de wegparameter en keert alle statische lijstverslagen terug die de lood een lid van is.

### Verzoek

```
GET /rest/v1/leads/{id}/listMembership.json?batchSize=3
```

### Antwoord

```json
{
    "requestId": "1184b#1706f0ec23f",
    "result": [
        {
            "listId": 3379,
            "createdAt": "2016-05-17T19:32:44Z",
            "updatedAt": "2016-05-17T19:32:44Z"
        },
        {
            "listId": 2792,
            "createdAt": "2009-05-19T18:29:15Z",
            "updatedAt": "2009-05-19T18:29:15Z"
        },
        {
            "listId": 42,
            "createdAt": "2009-04-22T19:24:22Z",
            "updatedAt": "2009-04-22T19:24:22Z"
        }
    ],
    "success": true,
    "nextPageToken": "BFRV7OMVSNJWDVKVTUFS3XHT4E======",
    "moreResult": true
}
```

## Programma&#39;s

Het lidmaatschap van het programma kan op een gelijkaardige manier zoals lijsten worden teruggewonnen. De zelfde facultatieve verzoekparameters zijn beschikbaar wanneer het roepen van krijgen leidt door het eindpunt van Id van het Programma en gaat over `programId` padparameter.

U kunt desgewenst een veldparameter doorgeven die een door komma&#39;s gescheiden lijst met veldnamen bevat die moet worden geretourneerd. Als de parameter fields niet is opgenomen in deze aanvraag, worden de volgende standaardvelden geretourneerd: `email`, `updatedAt`, `createdAt`, `lastName`, `firstName`, `membership`, en `id`. Als u een lijst met velden aanvraagt en een bepaald veld wordt opgevraagd, maar niet wordt geretourneerd, wordt de waarde impliciet ingesteld op null.

De responsstructuur lijkt sterk op elkaar, aangezien elk item in de resultaatarray een lead is, behalve dat elke record ook een onderliggend object heeft met de naam &quot;membership&quot;. Dit lidmaatschapsobject bevat gegevens over de relatie van de lead met het programma dat in de oproep wordt aangegeven, waarbij altijd de relatie van de lead wordt getoond `progressionStatus`, `acquiredBy`, `reachedSuccess`, en `membershipDate`. Als het bovenliggende programma ook een betrokkenheidsprogramma is, hebben leden `stream`, `nurtureCadence`, en `isExhausted` haar positie en activiteit in het betrokkenheidsprogramma aan te geven.

### Verzoek

```
GET /rest/v1/leads/programs/{programId}.json?batchSize=3
```

### Antwoord

```json
{
    "requestId": "13ad4#1727b748a17",
    "result": [
        {
            "id": 319141,
            "firstName": "Meera",
            "lastName": "Reed",
            "email": "mree@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        },
        {
            "id": 319142,
            "firstName": "Jon",
            "lastName": "Umber",
            "email": "jumb@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        },
        {
            "id": 319143,
            "firstName": "Lyanna",
            "lastName": "Mormont",
            "email": "lmor@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        }
    ],
    "success": true,
    "nextPageToken": "SW3PTMBVFCNHSHJGZ7LQH3ZWNUOHKADJZ3MOQ2LOZZVNO3WEIUPDKPRTTHBSMW756KOCWURTOF2XS==="
}
```

Het Get Programma&#39;s door het eindpunt van identiteitskaart van het Loodverslag neemt een de wegparameter van loodverslag id en keert alle programmaverslagen terug die de lood een lid van is. De optionele `filterType` en `filterValues` met parameters kunt u filteren op programma-id.

### Verzoek

```
GET /rest/v1/leads/{id}/programMembership.json
```

### Antwoord

```json
{
    "requestId": "12e84#1706f13a379",
    "result": [
        {
            "id": 1044,
            "progressionStatus": "Sent",
            "isExhausted": false,
            "acquiredBy": false,
            "reachedSuccess": false,
            "membershipDate": "2016-05-27T19:50:29Z",
            "updatedAt": "2016-05-27T19:50:29Z"
        }
    ],
    "success": true,
    "moreResult": false
}
```

## Slimme campagnes

De Get Slimme Campagnes door het eindpunt van identiteitskaart van het Lood neemt een de wegparameter van loodverslag id en keert alle slimme campagneverslagen terug die de lood een lid van is.

### Verzoek

```
GET /rest/v1/leads/{id}/smartCampaignMembership.json?batchSize=3
```

### Antwoord

```json
{
    "requestId": "e7b0#1706f163632",
    "result": [
        {
            "smartCampaignId": 3746,
            "createdAt": "2018-06-01T18:00:04Z",
            "updatedAt": "2018-06-01T18:00:06Z"
        },
        {
            "smartCampaignId": 3678,
            "createdAt": "2015-04-06T18:37:30Z",
            "updatedAt": "2015-04-06T18:37:41Z"
        },
        {
            "smartCampaignId": 3680,
            "createdAt": "2015-04-06T18:37:30Z",
            "updatedAt": "2015-04-06T18:37:40Z"
        }
    ],
    "success": true,
    "nextPageToken": "TNGAH3NKDUFDHNXUVGTNBXJCQM======",
    "moreResult": true
}
```

## Verwijderen

Het verwijderen van leads is eenvoudig met het eindpunt Leads verwijderen.  Geef loodnummers op die u wilt verwijderen met de id-kenmerken in de hoofdtekst.  Het maximum aantal leads is 300 per aanvraag.  Content-Type gebruiken: application/json header.

### Verzoek

```
POST /rest/v1/leads/delete.json
```

### Lichaam

```json
{
   "input":[
      {
         "id": 235
      },
      {
         "id":766
      }
   ]
}
```

### Antwoord

```json
{
  "requestId":"3608#16664333670",
  "result":[
    {
      "id":235,
      "status":"deleted"
    },
    {
      "id":766,
      "status":"deleted"
    }
  ],
  "success":true
}
```

## Relaties

* Bedrijven via het veld externalCompanyId op lead record
* Verkoopmedewerkers via het veld externalSalesPersonId in hoofdrecord
* Programma&#39;s via programmalandschap
* Lijsten door lijstlidmaatschap
* Activiteiten via het veld leadId in de activiteit
* Segmentering via afzonderlijke segmentvelden in hoofdrecord
* Partities via leadPartitionId bij lead-record

## Tijdstippen

Eindpunten van leads hebben een time-out van 30 seconden, tenzij hieronder vermeld:

* Leads synchroniseren: 90 s
* Associate Lead: 60 s
* Leads samenvoegen: 180 s
* Loodpartitie bijwerken: 60 s
* Push Lead naar Marketo: 90 seconden
* Leads ophalen op filtertype: 60 s
* Leads ophalen op lijst-id: 60 s
