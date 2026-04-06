---
title: Gegevensinname
feature: REST API, Dynamic Content
description: Gebruik de API voor gegevensinsluiting van Marketo voor grote hoeveelheden, lage latentie bij het opnemen van personen, aangepaste objecten, bedrijven en programmaleden.
exl-id: 1d501916-53ac-42d8-a804-abb4ab01c7e8
source-git-commit: 6dc068f92d5b0c94035ca484fd1508dfe87bbd76
workflow-type: tm+mt
source-wordcount: '1789'
ht-degree: 5%

---

# Data Ingestie-API

De API voor gegevensinsluiting is een service met een hoog volume, lage latentie en hoge beschikbaarheid die is ontworpen om de inname van grote hoeveelheden gegevens van personen en personen efficiënt en met minimale vertragingen te verwerken.

Gegevens worden opgenomen door aanvragen in te dienen die asynchroon worden uitgevoerd. De status van het verzoek kan worden teruggewonnen door aan gebeurtenissen van de [&#x200B; stroom van de Gegevens van de Waarneming van Marketo &#x200B;](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup) in te tekenen.

Interfaces worden aangeboden voor vier objecttypen: Personen, Aangepaste objecten, Ondernemingen en Programmaleden. De recordbewerking is alleen &quot;invoegen of bijwerken&quot;, behalve voor programmaleden die ook verwijderen ondersteunen.

>[!NOTE]
>
>De toegang tot de Ingestie API van Gegevens vereist beding aan het [&#x200B; Reeks van de Prestaties van Marketo Engage &#x200B;](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835) Pakket.

## Verificatie

De API voor gegevensverwerking gebruikt dezelfde OAuth 2.0-verificatiemethode als de Marketo REST API om een toegangstoken te genereren, maar het toegangstoken moet via HTTP-header `X-Mkto-User-Token` worden doorgegeven. U kunt het toegangstoken niet via een vraagparameter overgaan.

Token voor voorbeeldtoegang via koptekst:

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Machtigingen

De Ingestie van gegevens gebruikt het zelfde toestemmingenmodel zoals Marketo REST API, en vereist geen extra speciale toestemmingen aan gebruik, hoewel de specifieke toestemmingen voor elk eindpunt worden vereist.

| Endpoint | Machtiging |
| --- | --- |
| Personen | Lead lezen |
| Aangepaste objecten | Aangepast object lezen/schrijven |
| Bedrijven | Read-Write Bedrijf |
| Programmaleden | Lead lezen |

## Ondersteunde objecttypen

| Objecttype | Ondersteunde bewerkingen |
| --- | --- |
| Personen | Upsert (invoegen of bijwerken) |
| Aangepaste objecten | Upsert (invoegen of bijwerken) |
| Bedrijven | Synchroniseren (`createOnly`, `updateOnly`, `createOrUpdate`) |
| Programmaleden | Synchroniseren (status uploaden), Verwijderen (verwijderen uit programma) |

## Kopteksten

Gegevensinname maakt gebruik van de volgende aangepaste HTTP-headers.

### Verzoek

| Sleutel | Waarde | Vereist | Beschrijving |
| --- | --- | --- | --- |
| `X-Correlation-Id` | Willekeurige tekenreeks (maximumlengte 255 tekens). | Nee | Kan worden gebruikt om verzoeken door het systeem te traceren. Zie Marketo Observability Data Stream |
| `X-Request-Source` | Willekeurige tekenreeks (maximumlengte 50 tekens). | Nee | Kan worden gebruikt om de bron van verzoeken door het systeem te vinden. Zie Marketo Observability Data Stream |

### Antwoord

| Sleutel | Waarde | Vereist |
| --- | --- | --- |
| `X-Request-Id` | Unieke aanvraag-id. | Ja |

## Verzoeken

Gebruik de POST-methode van HTTP om gegevens naar de server te verzenden.

De gegevensrepresentatie wordt in de aanvraaginstantie opgenomen als application/json.

De domeinnaam is: `mkto-ingestion-api.adobe.io`

Het pad begint met `/subscriptions/MunchkinId` waar MunchkinId specifiek is voor uw Marketo-instantie. U kunt uw identiteitskaart van Munchkin in Marketo Engage UI onder **Admin** vinden > **Mijn Rekening** > **Informatie van de Steun**.  De rest van het pad wordt gebruikt om de bron van interesse op te geven.

Voorbeeld-URL voor personen:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

Voorbeeld-URL voor aangepaste objecten:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

Voorbeeld-URL voor bedrijven:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/companies`

Voorbeeld-URL voor programmaleden:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/programmembers`

### Reacties

Alle reacties retourneren een unieke aanvraag-id via de header `X-Request-Id` .

Voorbeeld van aanvraag-id via header:

`X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Succes

Wanneer een vraag succesvol is, is een status 202 teruggekeerd.  Er wordt geen responsorgaan geretourneerd.

Voorbeeld van een geslaagde reactie:

```
HTTP/1.1 202 Accepted
X-Request-Id: e3d92152-0fb1-444a-8f8f-29d5a2338598
Content-Length: 0
Date: Wed, 18 Oct 2023 18:56:49 GMT
```

### Fout

Wanneer een vraag een fout veroorzaakt, is een status niet-202 teruggekeerd samen met een reactiekarakter met extra foutendetails. De hoofdtekst van de reactie is `application/json` en bevat één object met leden `error_code` en `message` .

Hieronder vindt u hergebruikte foutcodes van Adobe Developer Gateway.

| HTTP-statuscode | error_code | message |
| --- | --- | --- |
| 401 | 401013 | Oauth-token is ongeldig |
| 403 | 403010 | Uitvoertoken ontbreekt |
| 404 | 404040 | Bron niet gevonden |
| 429 | 429001 | Limiet voor servicegebruik bereikt |

Hieronder vindt u foutcodes die uniek zijn voor de API voor gegevensinsluiting en die bestaan uit drie segmenten.  De eerste drie cijfers zijn de status (teruggekeerd door Adobe Developer Gateway), die door nul &quot;0&quot;wordt gevolgd, door drie cijfers wordt gevolgd.

| HTTP-statuscode | error_code | message |
| --- | --- | --- |
| 400 | 4000801 | Ongeldig verzoek |
| 400 | 4000802 | Ongeldige gegevens |
| 403 | 4030801 | Ongeautoriseerd |
| 429 | 4290801 | Dagelijks quotum bereikt |
| 500 | 5000801 | Interne serverfout |

## Opnieuw

Wanneer een overgangsfout wordt ontdekt, probeert de dienst de verrichting opnieuw. De pogingen gebeuren om diverse redenen, hoofdzakelijk wanneer een afhankelijke de diensttijd uit of tijdelijk niet beschikbaar is.

Intervallen opnieuw proberen:

* Eerste bewerking en eerste poging: 5 min
* 1ste en 2de : 15 min
* 2de en 3de : 20 minuten
* 3de en 4de : 20 minuten
* 4e en 5e : 2 uur
* na 5e herpoging -> 3 uur

## Eindpunten

De eindpunten van de congestie zijn beschikbaar voor Personen, de Voorwerpen van de Douane, Bedrijven, en de Leden van het Programma.

### Personen

Eindpunt dat wordt gebruikt om persoonrecords bij te voegen.

| Methode | Pad |
| --- | --- |
| POST | /subscriptions/{munchkinId}/people |

#### Kopteksten

| Sleutel | Waarde |
| --- | --- |
| `Content-Type` | application/json |
| `X-Mkto-User-Token` | {accessToken} |

#### Aanvragingsinstantie

| Sleutel | Gegevenstype | Vereist | Waarde | Standaardwaarde |
| --- | --- | --- | --- | --- |
| `priority` | String | Nee | Prioriteit van het verzoek: normaal of hoog | normaal |
| `partitionName` | String | Nee | Naam van partitie van persoon | Standaard |
| `dedupeFields` | Object | Nee | Te dedupliceren kenmerken op. Een of twee kenmerknamen zijn toegestaan. <br/> Twee attributen worden gebruikt in een verrichting AND. Als bijvoorbeeld zowel `email` als `firstName` zijn opgegeven, worden beide gebruikt om een persoon op te zoeken met de bewerking AND. <br/> Ondersteunde kenmerken zijn: `id` , `email` , `sfdcAccountId` , `sfdcContactId` , `sfdcLeadId` `sfdcLeadOwnerId` , Aangepaste kenmerken (&quot;alleen tekenreeks&quot; en &quot;geheel getal&quot;), `email` |  |
| `persons` | Array van object | Ja | Lijst met kenmerknaam-waardeparen voor de persoon | - |

Vereiste machtigingen zijn `Read-Write Lead` .

### Voorbeeld van personen

#### Verzoek

`POST /subscriptions/{munchkinId}/persons`

#### Kopteksten

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Lichaam

```json
{
   "priority": "high",
   "partitionName": "EMEA",
   "dedupeFields": {
      "field1": "email",
      "field2": "firstName"
   },
   "persons":[
      {
         "email": "brooklyn.parker@karnv.com",
         "firstName": "Brooklyn",
         "lastName": "Parker",
         "company": "Karnv"
      },
      {
         "email": "johnny.neal@yvu30.com",
         "firstName": "Johnny",
         "lastName": "Neal",
         "company": "Acme Inc"
      }
   ]
}
```

#### Antwoord

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Aangepaste objecten

Eindpunt dat wordt gebruikt om aangepaste objectreeks bij te voegen.

| Methode | Pad |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/customobjects/{customObjectAPIName}` |

#### Kopteksten

| Sleutel | Waarde |
| --- | --- |
| `Content-Type` | application/json |
| `X-Mkto-User-Token` | {accessToken} |

#### Aanvragingsinstantie

| Sleutel | Gegevenstype | Vereist | Waarde | Standaardwaarde |
| --- | --- | --- | --- | --- |
| `priority` | String | Nee | Prioriteit van het verzoek: normaal, hoog | normaal |
| `dedupeBy` | String | Nee | Te dupliceren kenmerken op: dedupeFields, marketoGUID | dedupeFields |
| `customObjects` | Array van object | Ja | Lijst met kenmerknaam-waardeparen voor het object. | - |

De vereiste machtigingen zijn `Read-Write Custom Object` .

Als een koppelingsgebied aan een Persoon in het verzoek wordt gespecificeerd en die Persoon niet bestaat, komen verscheidene opnieuw voor. Als die persoon tijdens het venster Opnieuw proberen is toegevoegd (65 minuten), is de update gelukt. Als het koppelingsveld bijvoorbeeld `email` on Person is en Person niet bestaat, worden opnieuw pogingen uitgevoerd.

### Voorbeeld van aangepaste objecten

#### Verzoek

`POST /subscriptions/{munchkinId}/customobjects/{customObjectAPIName}`

#### Kopteksten

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Lichaam

```json
{
   "dedupeBy": "dedupeFields",
   "priority": "high",
   "customObjects": [
      {
         "email": "brooklyn.parker@karnv.com",
         "vin": "20UYA31581L000000",
         "make": "BMW",
         "model": "3-Series 330i",
         "year": 2003
      },
      {
         "email": "johnny.neal@yvu30.com",
         "vin": "19UYA31581L000000",
         "make": "BMW",
         "model": "3-Series 325i",
         "year": 1989
      }
   ]
}
```

#### Antwoord

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Bedrijven

Eindpunt dat wordt gebruikt om bedrijfsrecords te synchroniseren. Ondersteunt bewerkingen voor maken, bijwerken en uploaden met deduplicatie door een externe bedrijfs-id of een interne Marketo-id.

| Methode | Pad |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/companies` |

#### Kopteksten

| Sleutel | Waarde | Vereist |
| --- | --- | --- |
| `Content-Type` | application/json | Ja |
| `X-Mkto-User-Token` | {accessToken} | Ja |
| `X-Correlation-Id` | Willekeurige tekenreeks (maximumlengte 255 tekens) | Nee |
| `X-Request-Source` | Willekeurige tekenreeks (maximumlengte 50 tekens) | Nee |

#### Aanvragingsinstantie

| Sleutel | Gegevenstype | Vereist | Waarde | Standaardwaarde |
| --- | --- | --- | --- | --- |
| `action` | String | Nee | Handeling synchroniseren: `createOnly`, `updateOnly` of `createOrUpdate` | `createOrUpdate` |
| `dedupeBy` | String | Nee | Veld dat moet worden gedupliceerd op: `dedupeFields` of `idField` (hoofdlettergevoelig). Voor `createOnly` en `createOrUpdate` is alleen `dedupeFields` toegestaan. Voor `updateOnly` zijn beide toegestaan. | `dedupeFields` |
| `input` | Array van object | Ja | Lijst met naam-waardeparen van bedrijfskenmerken. Accepteert JSON-toets `input` of `companies` . | - |

Elk bedrijfsobject in de array `input` ondersteunt de volgende velden:

| Sleutel | Gegevenstype | Vereist | Beschrijving |
| --- | --- | --- | --- |
| `externalCompanyId` | String | Voorwaardelijk | Id van externe onderneming. Vereist als `dedupeBy` `dedupeFields` is. Niet toegestaan wanneer `dedupeBy` `idField` is. |
| `id` | Lang | Voorwaardelijk | Interne Marketo-bedrijfsnaam. Vereist als `dedupeBy` `idField` is en `action` `updateOnly` is. Niet toegestaan wanneer `dedupeBy` `dedupeFields` is. |
| `company` | String | Nee | Bedrijfsnaam. |
| (elk veld) | Alle | Nee | De extra standaard of gebieden van het douanebedrijf zoals die door [&#x200B; worden bepaald beschrijven Bedrijven &#x200B;](companies.md). Veldnamen zijn niet hoofdlettergevoelig. |

De vereiste machtigingen zijn `Read-Write Company` .

### Voorbeeld van bedrijven

#### Verzoek

`POST /subscriptions/{munchkinId}/companies`

#### Kopteksten

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Lichaam

```json
{
   "action": "createOrUpdate",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "externalCompanyId": "ext-company-001",
         "company": "Acme Corporation",
         "industry": "Technology",
         "numberOfEmployees": 5000,
         "annualRevenue": 100000000
      },
      {
         "externalCompanyId": "ext-company-002",
         "company": "Globex Industries",
         "industry": "Manufacturing",
         "numberOfEmployees": 1200
      }
   ]
}
```

#### Antwoord

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Bijwerken per id-voorbeeld voor bedrijven

```json
{
   "action": "updateOnly",
   "dedupeBy": "idField",
   "input": [
      {
         "id": 12345,
         "company": "Acme Corporation (Renamed)",
         "numberOfEmployees": 5500
      }
   ]
}
```

### Validatieregels voor bedrijven

| Regel | Detail |
| --- | --- |
| action | Moet één van zijn: `createOnly`, `updateOnly`, `createOrUpdate`. Hoofdlettergevoelig. |
| dedupeBy | Moet `dedupeFields` of `idField` (hoofdlettergevoelig) zijn. Wordt standaard ingesteld op `dedupeFields` . |
| dedupeBy + action | `createOnly` en `createOrUpdate` only allow `dedupeFields` . `updateOnly` staat zowel `dedupeFields` als `idField` toe. |
| Wanneer `dedupeBy=dedupeFields` | Elk bedrijf moet `externalCompanyId` hebben. Veld `id` mag niet aanwezig zijn. |
| Wanneer `dedupeBy=idField` | Elk bedrijf moet `id` hebben. Veld `externalCompanyId` mag niet aanwezig zijn. |
| `input` / `companies` | Mag niet null of leeg zijn. |
| Max. objecten per aanvraag | 1,000 |

### Programmaleden (synchroniseren)

Eindpunt dat wordt gebruikt om de status van het programmalid te synchroniseren, leidt tot programma&#39;s toe te voegen of hun programmastatus bij te werken.

| Methode | Pad |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/programmembers` |

#### Kopteksten

| Sleutel | Waarde | Vereist |
| --- | --- | --- |
| Inhoudstype | application/json | Ja |
| X-MKTO-User-Token | {accessToken} | Ja |
| X-correlatie-id | Willekeurige tekenreeks (maximumlengte 255 tekens) | Nee |
| X-Request-Source | Willekeurige tekenreeks (maximumlengte 50 tekens) | Nee |

#### Aanvragingsinstantie

| Sleutel | Gegevenstype | Vereist | Waarde | Standaardwaarde |
| --- | --- | --- | --- | --- |
| programma&#39;s | Array van object | Ja | Lijst van programma-activiteiten. Elk programma geeft een programma, een doelstatus en leidt tot synchronisatie. | - |

Elk object in de array `programs` bevat:

| Sleutel | Gegevenstype | Vereist | Beschrijving |
| --- | --- | --- | --- |
| programId | Lang | Ja | De Marketo-programma-id. Moet een positief geheel getal zijn. |
| status | String | Ja | De status van het programmalid die moet worden ingesteld, bijvoorbeeld `"Member"` of `"Influenced"` . Accepteert JSON-toets `statusName` of `status` . De waarde mag niet `"Not in Program"` zijn. Gebruik in plaats daarvan het verwijdereindpunt. |
| leden | Array van object | Ja | Lijst met verwijzingen naar leads die u wilt toevoegen of bijwerken in het programma. Accepteert JSON-toets `input` of `members` . |

Elk object in de array `members` bevat:

| Sleutel | Gegevenstype | Vereist | Beschrijving |
| --- | --- | --- | --- |
| leadId | Lang | Ja | De Marketo lead-id. |
| (elk veld) | Alle | Nee | Extra velden voor aangepaste programmaleden. Veldnamen zijn niet hoofdlettergevoelig. |

De vereiste machtigingen zijn `Read-Write Lead` .

### Voorbeeld van synchronisatie van programmaleden

#### Verzoek

`POST /subscriptions/{munchkinId}/programmembers`

#### Kopteksten

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Lichaam

```json
{
   "programs": [
      {
         "programId": 1001,
         "status": "Member",
         "members": [
            {
               "leadId": 10001
            },
            {
               "leadId": 10002
            }
         ]
      },
      {
         "programId": 1002,
         "status": "Influenced",
         "members": [
            {
               "leadId": 10003
            }
         ]
      }
   ]
}
```

#### Antwoord

`HTTP/1.1 202`
`X-Request-ID: e3d92152-0fb1-444a-8f8f-29d5a2338598`

### Programma-leden synchroniseren validatieregels

| Regel | Detail |
| --- | --- |
| programma&#39;s | Mag niet null of leeg zijn. |
| programId | Vereist. Moet een positief geheel getal zijn. |
| status | Vereist. Mag niet leeg zijn. Mag niet `"Not in Program"` (hoofdlettergevoelig) zijn. Gebruik in plaats hiervan het verwijdereindpunt. |
| leden | Mag niet null of leeg zijn. |
| leadId | Vereist voor elk lid in de invoerarray. |
| Max. aantal leads per aanvraag | 1.000 leden in totaal voor alle programma&#39;s. |

### Programmaleden (verwijderen)

Eindpunt dat wordt gebruikt om leads uit programma&#39;s te verwijderen. Hierdoor wordt de lidmaatschapsstatus van de lead ingesteld op `"Not in Program"` en wordt het lid uit dat programma verwijderd.

>[!NOTE]
>
>Dit eindpunt gebruikt POST eerder dan DELETE omdat het verzoek een lichaam JSON met gestructureerde gegevens vereist.

| Methode | Pad |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/programmembers/delete` |

#### Kopteksten

| Sleutel | Waarde | Vereist |
| --- | --- | --- |
| Inhoudstype | application/json | Ja |
| X-MKTO-User-Token | {accessToken} | Ja |
| X-correlatie-id | Willekeurige tekenreeks (maximumlengte 255 tekens) | Nee |
| X-Request-Source | Willekeurige tekenreeks (maximumlengte 50 tekens) | Nee |

#### Aanvragingsinstantie

| Sleutel | Gegevenstype | Vereist | Waarde | Standaardwaarde |
| --- | --- | --- | --- | --- |
| programma&#39;s | Array van object | Ja | Lijst met programma-verwijderbewerkingen. Elke methode geeft een programma op en leidt tot verwijdering. | - |

Elk object in de array `programs` bevat:

| Sleutel | Gegevenstype | Vereist | Beschrijving |
| --- | --- | --- | --- |
| programId | Lang | Ja | De Marketo-programma-id. Moet een positief geheel getal zijn. |
| leden | Array van object | Ja | Lijst met verwijzingen naar leads die u uit het programma wilt verwijderen. Accepteert JSON-toets `input` of `members` . |

Elk object in de array `members` bevat:

| Sleutel | Gegevenstype | Vereist | Beschrijving |
| --- | --- | --- | --- |
| leadId | Lang | Ja | De Marketo lead-id. |

De vereiste machtigingen zijn `Read-Write Lead` .

### Voorbeeld van verwijderen door programmaleden

#### Verzoek

`POST /subscriptions/{munchkinId}/programmembers/delete`

#### Kopteksten

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Lichaam

```json
{
   "programs": [
      {
         "programId": 1001,
         "members": [
            {
               "leadId": 10001
            },
            {
               "leadId": 10002
            }
         ]
      },
      {
         "programId": 1002,
         "members": [
            {
               "leadId": 10003
            }
         ]
      }
   ]
}
```

#### Antwoord

`HTTP/1.1 202`
`X-Request-ID: a1b2c3d4-e5f6-7890-abcd-ef1234567890`

### Programmaleden verwijderen validatieregels

| Regel | Detail |
| --- | --- |
| programma&#39;s | Mag niet null of leeg zijn. |
| programId | Vereist. Moet een positief geheel getal zijn. |
| leden | Mag niet null of leeg zijn. |
| leadId | Vereist voor elk lid in de invoerarray. |
| Max. aantal leads per aanvraag | 1.000 leden in totaal voor alle programma&#39;s. |

## Limieten

Hier volgt een bijgewerkte lijst met instructies:

* Maximale grootte van aanvraag: 1 MB
* Maximale objecten per aanvraag per objecttype: 1.000
* Maximale aanvragen per seconde per client-id: 5.000
* Maximale aantal objecten per dag: 10.000.000

Deze limieten gelden op uniforme wijze voor Personen, Aangepaste objecten, Ondernemingen en Programmaleden. Voor programmaleden is &quot;objecten per aanvraag&quot; het totale aantal verwijzingen naar leads voor alle programma&#39;s in één aanvraag.

## API voor gegevensverwerking versus REST-API

Hier volgt een lijst met verschillen tussen de API voor gegevensverwerking en andere Marketo REST-API&#39;s:

* Voor verificatie moet u toegangstoken doorgeven met de header `X-Mkto-User-Token` .
* De URL-domeinnaam is `mkto-ingestion-api.adobe.io`
* Het URL-pad begint met `/subscriptions/MunchkinId`
* Er zijn geen queryparameters
* Als de vraag succesvol is, is een status 202 teruggekeerd en het reactielichaam is leeg
* Als een vraag ontbreekt, is niet-202 status teruggekeerd en het reactiekarakter bevat `{ "error_code" : "Error Code", "message" : "Message" }`
* De aanvraag-id wordt via `X-Request-Id` header geretourneerd
