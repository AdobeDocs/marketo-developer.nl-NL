---
title: Gegevensopname
feature: REST API, Dynamic Content
description: Gegevens gebruiken met Marketo API's.
exl-id: 1d501916-53ac-42d8-a804-abb4ab01c7e8
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '965'
ht-degree: 3%

---

# Data Ingestie-API

De API voor gegevensinsluiting is een service met een hoog volume, lage latentie en hoge beschikbaarheid die is ontworpen om de inname van grote hoeveelheden gegevens van personen en personen efficiënt en met minimale vertragingen te verwerken.

Gegevens worden opgenomen door aanvragen in te dienen die asynchroon worden uitgevoerd. De status van het verzoek kan worden teruggewonnen door aan gebeurtenissen van de [ Stream van de Gegevens van de Waarneming van Marketo ](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup) in te tekenen. &#x200B;

Interfaces worden aangeboden voor twee objecttypen: Personen, Aangepaste objecten. De recordbewerking is alleen &quot;invoegen of bijwerken&quot;.

De API voor gegevensinname bevindt zich momenteel in een persoonlijke bètaversie.  Invitees worden vereist om een recht voor [ de Rij van de Prestaties van Marketo Engage ](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835) Pakket te hebben.

## Verificatie

De API voor gegevensverwerking gebruikt dezelfde OAuth 2.0-verificatiemethode als de Marketo REST API om een toegangstoken te genereren, maar het toegangstoken moet via HTTP-header `X-Mkto-User-Token` worden doorgegeven. U kunt het toegangstoken niet via een vraagparameter overgaan.

Token voor voorbeeldtoegang via koptekst:

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Machtigingen

De Ingestie van gegevens gebruikt het zelfde toestemmingenmodel zoals Marketo REST API, en vereist geen extra speciale toestemmingen aan gebruik, hoewel de specifieke toestemmingen voor elk eindpunt worden vereist.

| Endpoint | Machtiging |
|-|-|
| Personen | Lead lezen |
| Aangepaste objecten | Aangepast object lezen/schrijven |

## Kopteksten

Gegevensinname maakt gebruik van de volgende aangepaste HTTP-headers.

### Verzoek

| Sleutel | Waarde | Vereist | Beschrijving |
| - | - | - | - |
| X-correlatie-id | Willekeurige tekenreeks (maximumlengte 255 tekens). | Nee | Kan worden gebruikt om verzoeken door het systeem te traceren.  Zie Marketo Observability Data Stream |
| X-Request-Source | Willekeurige tekenreeks (maximumlengte 50 tekens). | Nee | Kan worden gebruikt om de bron van verzoeken door het systeem te vinden.  Zie Marketo Observability Data Stream |

### Antwoord

| Sleutel | Waarde | Vereist |
| - | - | - |
| X-aanvraag-id | Unieke aanvraag-id. | Ja |

## Verzoeken

Gebruik de POST-methode van HTTP om gegevens naar de server te verzenden.

De gegevensrepresentatie wordt in de aanvraaginstantie opgenomen als application/json.

De domeinnaam is: `mkto-ingestion-api.adobe.io`

Het pad begint met `/subscriptions/MunchkinId` waar MunchkinId specifiek is voor uw Marketo-instantie. U kunt uw identiteitskaart van Munchkin in Marketo Engage UI onder **Admin** vinden > **Mijn Rekening** > **Informatie van de Steun**.  De rest van het pad wordt gebruikt om de bron van interesse op te geven.

Voorbeeld-URL voor personen:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

Voorbeeld-URL voor aangepaste objecten:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

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

Wanneer een vraag een fout veroorzaakt, is een status niet-202 teruggekeerd samen met een reactiekarakter met extra foutendetails.  De hoofdtekst van de reactie is application/json en bevat één enkel voorwerp met leden error_code en bericht.

Hieronder vindt u hergebruikte foutcodes van Adobe Developer Gateway.

| HTTP-statuscode | error_code | bericht |
| - | - | - |
| 401 | 401013 | Oauth-token is ongeldig |
| 403 | 403010 | Uitvoertoken ontbreekt |
| 404 | 404040 | Bron niet gevonden |
| 429 | 429001 | Limiet voor servicegebruik bereikt |

Hieronder vindt u foutcodes die uniek zijn voor de API voor gegevensinsluiting en die bestaan uit drie segmenten.  De eerste drie cijfers zijn de status (teruggekeerd door Adobe Developer Gateway), die door nul &quot;0&quot;wordt gevolgd, door drie cijfers wordt gevolgd.

| HTTP-statuscode | error_code | bericht |
| - | - | - |
| 400 | 4000801 | Ongeldig verzoek |
| 400 | 4000802 | Ongeldige gegevens |
| 403 | 4030801 | Ongeautoriseerd |
| 429 | 4290801 | Dagelijks quotum bereikt |
| 500 | 5000801 | Interne serverfout |

## Opnieuw

Wanneer een overgangsfout wordt ontdekt, probeert de dienst de verrichting drie keer opnieuw.  De eerste keer wordt opnieuw geprobeerd na een wachttijd van 5 minuten, de tweede na 30 minuten en ten slotte de derde na 30 minuten.  De pogingen gebeuren om diverse redenen, hoofdzakelijk wanneer een afhankelijke de diensttijd uit of tijdelijk niet beschikbaar is.

## Eindpunten

Ingestieeindpunten zijn beschikbaar voor Personen en Aangepaste objecten.

### Personen

Eindpunt dat wordt gebruikt om persoonrecords bij te voegen.

| Methode | Pad |
| - | - |
| POST | /subscriptions/{munchkinId}/people |

#### Kopteksten

| Sleutel | Waarde |
| - | - |
| Inhoudstype | application/json |
| X-MKTO-User-Token | {accessToken} |

#### Aanvragingsinstantie

| Sleutel | Gegevenstype | Vereist | Waarde | Standaardwaarde |
| - | - | - | - | - |
| prioriteit | String | Nee | Prioriteit van het verzoek: normaal of hoog | normaal |
| partitionName | String | Nee | Naam van partitie van persoon | Standaard |
| dedupeFields | Object | Nee | Te dedupliceren kenmerken op. Een of twee kenmerknamen zijn toegestaan. <br/> Er worden twee kenmerken gebruikt in een AND-bewerking. Als bijvoorbeeld zowel `email` als `firstName` zijn opgegeven, worden beide gebruikt om een persoon op te zoeken met de bewerking AND. <br/> Ondersteunde kenmerken zijn: `id` , `email` , `sfdcAccountId` , `sfdcContactId` , `sfdcLeadId` `sfdcLeadOwnerId` , Aangepaste kenmerken (&quot;alleen tekenreeks&quot; en &quot;geheel getal&quot;), `email` |
| personen | Array van object | Ja | Lijst met kenmerknaam-waardeparen voor de persoon | - |

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
         "lastName": "Parker"
      },
      {
         "email": "johnny.neal@yvu30.com",
         "firstName": "Johnny",
         "lastName": "Neal"
      }
   ]
}
```

#### Antwoord

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Aangepaste objecten

Eindpunt dat wordt gebruikt voor het bijwerken van aangepaste objectrecords

| Methode | Pad |
| - | - |
| POST | `/subscriptions/{munchkinId}/customobjects/{customObjectAPIName}` |

#### Kopteksten

| Sleutel | Waarde |
| - | - |
| Inhoudstype | application/json |
| X-MKTO-User-Token | {accessToken} |

#### Aanvragingsinstantie

| Sleutel | Gegevenstype | Vereist | Waarde | Standaardwaarde |
| - |- | - | - | - |
| prioriteit | String | Nee | Prioriteit van het verzoek: normaal, hoog | normaal |
| dedupeBy | String | Nee | Te dupliceren kenmerken op: dedupeFields, marketoGUID | dedupeFields |
| customObjects | Array van object | Ja | Lijst met kenmerknaam-waardeparen voor het object. | - |

De vereiste machtigingen zijn `Read-Write Custom Object` .

Als een koppelingsgebied aan een Persoon in het verzoek wordt gespecificeerd en die Persoon niet bestaat, komen verscheidene opnieuw voor. Als die persoon tijdens het venster Opnieuw proberen is toegevoegd (65 minuten), is de update gelukt. Als het koppelingsveld bijvoorbeeld een e-mail op persoon is en Person niet bestaat, worden opnieuw pogingen uitgevoerd.

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

## Limieten

Hier volgt een lijst met het gebruik van ardrails:

* Maximale grootte van aanvraag: 1 MB
* Maximale objecten per aanvraag per objecttype: 1.000
* Maximale aanvragen per seconde per client-id: 5.000
* Maximale aantal objecten per dag: 10.000.000

## API voor gegevensverwerking versus REST-API

Hier volgt een lijst met verschillen tussen de API voor gegevensverwerking en andere Marketo REST-API&#39;s:

* Dit is geen volledige interface CRUD, het steunt slechts upsert
* Voor verificatie moet u toegangstoken doorgeven met de header `X-Mkto-User-Token` .
* De URL-domeinnaam is `mkto-ingestion-api.adobe.io`
* Het URL-pad begint met `/subscriptions/MunchkinId`
* Er zijn geen queryparameters
* Als de vraag succesvol is, is een status 202 teruggekeerd en het reactielichaam is leeg
* Als de aanroep mislukt, wordt de status non-202 geretourneerd en bevat de hoofdtekst van de reactie `{ "error_code" : "Error Code", "message" : "Message" }`
* De aanvraag-id wordt via `X-Request-Id` header geretourneerd
