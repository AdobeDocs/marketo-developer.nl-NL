---
title: "Gegevensinname"
description: "Overzicht van API voor gegevensinsluiting"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 2%

---


# Gegevensinname

De API voor gegevensinsluiting is een service met een hoog volume, lage latentie en hoge beschikbaarheid die is ontworpen om inname van grote hoeveelheden gegevens van personen en personen efficiënt en met minimale vertragingen te verwerken. 

Gegevens worden opgenomen door aanvragen in te dienen die asynchroon worden uitgevoerd. De status van het verzoek kan worden opgehaald door zich te abonneren op gebeurtenissen van de [Marketo Observability Data Stream](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup/). &#x200B;

Interfaces worden aangeboden voor twee objecttypen: Personen, Aangepaste objecten. De recordbewerking is alleen &quot;invoegen of bijwerken&quot;.

De API voor gegevensinsluiting bevindt zich in een persoonlijke bètaversie. Genodigden moeten aanspraak kunnen maken op [Marketo Engage Performance Tier Package](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Verificatie

De API voor gegevensverwerking gebruikt dezelfde OAuth 2.0-verificatiemethode als Marketo REST API om een toegangstoken te genereren, maar het toegangstoken moet via HTTP-header worden doorgegeven `X-Mkto-User-Token`. U kunt het toegangstoken niet via een vraagparameter overgaan.

Token voor voorbeeldtoegang via koptekst:

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Machtigingen

De Ingestie van gegevens gebruikt het zelfde toestemmingenmodel zoals Marketo REST API, en vereist geen extra speciale toestemmingen aan gebruik, hoewel de specifieke toestemmingen voor elk eindpunt worden vereist.

| Endpoint | Machtiging |
|---|---|
| Personen | Lead lezen |
| Aangepaste objecten | Aangepast object lezen/schrijven |

## Kopteksten

Gegevensinname maakt gebruik van de volgende aangepaste HTTP-headers.

### Verzoek

| Sleutel | Waarde | Vereist | Beschrijving |
|---|---|---|---|
| X-correlatie-id | Willekeurige tekenreeks (maximumlengte 255 tekens). | Nee | Kan worden gebruikt om verzoeken door systeem te volgen. Zie Marketo Observability Data Stream |
| X-Request-Source | Willekeurige tekenreeks (maximumlengte 50 tekens). | Nee | Kan worden gebruikt om bron van verzoeken door systeem te vinden. Zie Marketo Observability Data Stream |

### Antwoord

| Sleutel | Waarde | Vereist | Beschrijving |
|---|---|---|---|
| X-aanvraag-id | Unieke aanvraag-id. | Ja | |

## Verzoeken

Gebruik de methode van de POST van HTTP om gegevens naar de server te verzenden.

De gegevensrepresentatie wordt in de aanvraaginstantie opgenomen als application/json.

De domeinnaam is: `mkto-ingestion-api.adobe.io`

Het pad begint met `/subscriptions/_MunchkinId_` waar `_MunchkinId_` is specifiek voor je Marketo-exemplaar. U vindt uw Munchkin-id in de gebruikersinterface van het Marketo Engage onder **Beheerder** >**Mijn account** > **Ondersteuningsinformatie**. De rest van het pad wordt gebruikt om de bron van interesse op te geven.

Voorbeeld-URL voor personen:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

Voorbeeld-URL voor aangepaste objecten:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

## Reacties

Alle reacties retourneren een unieke aanvraag-id via de `X-Request-Id` header.

Voorbeeld van aanvraag-id via header:

`X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Succes

Wanneer een vraag succesvol is, is een status 202 teruggekeerd. Er wordt geen responsorgaan geretourneerd.

Voorbeeld van een geslaagde reactie:

`HTTP/1.1 202 Accepted` `X-Request-Id: e3d92152-0fb1-444a-8f8f-29d5a2338598` `Content-Length: 0` `Date: Wed, 18 Oct 2023 18:56:49 GMT`

### Fout

Wanneer een vraag een fout veroorzaakt, is een status niet-202 teruggekeerd samen met een reactiekarakter met extra foutendetails. De hoofdtekst van de reactie is application/json en bevat één object met leden `error_code` en `message`.

Hieronder vindt u hergebruikte foutcodes van Adobe Developer Gateway.

| HTTP-statuscode | error_code | bericht |
|--- |--- |--- |
| 401 | 401013 | Oauth-token is ongeldig |
| 403 | 403010 | Uitvoertoken ontbreekt |
| 404 | 404040 | Bron niet gevonden |
| 429 | 429001 | Limiet voor servicegebruik bereikt |

Hieronder vindt u foutcodes die uniek zijn voor de API voor gegevensinsluiting en die bestaan uit drie segmenten. De eerste drie cijfers zijn de status (teruggekeerd door de Gateway van Adobe IO), die door nul &quot;0&quot;wordt gevolgd, door drie cijfers wordt gevolgd.

| HTTP-statuscode | error_code | bericht |
|--- |--- |--- |
| 400 | 4000801 | Ongeldig verzoek |
| 400 | 4000802 | Ongeldige gegevens |
| 403 | 4030801 | Ongeautoriseerd |
| 429 | 4290801 | Dagelijks quotum bereikt |
| 500 | 5000801 | Interne serverfout |

Voorbeeld van een foutreactie:

`HTTP/1.1 403 Forbidden` `Content-Type: application/json` `Content-Length: 54` `Date: Wed, 18 Oct 2023 19:10:07 GMT` `X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw` `{"error_code":"403010","message":"Oauth token is missing"}`

## Opnieuw

Wanneer een overgangsfout wordt ontdekt, probeert de dienst de verrichting drie keer opnieuw. De eerste keer wordt opnieuw geprobeerd na een wachttijd van 5 minuten, de tweede na 30 minuten en ten slotte de derde na 30 minuten. De pogingen gebeuren om diverse redenen, hoofdzakelijk wanneer een afhankelijke de diensttijd uit of tijdelijk niet beschikbaar is.

## Eindpunten

Ingestieeindpunten zijn beschikbaar voor Personen en Aangepaste objecten.

### Personen

Eindpunt dat wordt gebruikt om persoonrecords bij te voegen.

| Methode |
|---|
| POST |

| Pad |
|---|
| /abonnements/{munchkinId}/personen |

| HeadersKey | Waarde |
|---|---|
| Inhoudstype | application/json |
| X-MKTO-User-Token | {accessToken} |

Indieningsinstantie

| Sleutel | Gegevenstype | Vereist | Waarde | Standaardwaarde |
|---|---|---|---|---|
| prioriteit | String | Nee | Prioriteit van het verzoek:normaal hoog | normaal |
| partitionName | String | Nee | Naam van partitie van persoon | Standaard |
| dedupeFields | Object | Nee | Te dedupliceren kenmerken op. Een of twee kenmerknamen zijn toegestaan. Twee attributen worden gebruikt in een verrichting AND. Als beide `email` en `firstName` worden gespecificeerd, worden zij allebei gebruikt om op een persoon te zoeken die de EN verrichting gebruikt. Ondersteunde kenmerken zijn:`idemail`, `sfdcAccountId`, `sfdcContactId`, `sfdcLeadId`, `sfdcLeadOwnerIdCustom` attributes (&quot;string&quot; en &quot;integer&quot;) | email |
| personen | Array van object | Ja | Lijst met kenmerknaam-waardeparen voor de persoon | - |

| Machtiging |
|---|
| Lead lezen |

#### Voorbeeld van personen

```
POST /subscriptions/{munchkinId}/persons
```

```
Content-Type: application/json
X-Mkto-User-Token: {accessToken}
```

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

```
HTTP/1.1 202
X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw
```

### Aangepaste objecten

Eindpunt dat wordt gebruikt om aangepaste objectreeks bij te voegen.

| Methode |
|---|
| POST |

| Pad |
|---|
| /abonnements/{munchkinId}/customobjects/{customObjectAPIName} |

Kopteksten

| Sleutel | Waarde |
|---|---|
| Inhoudstype | application/json |
| X-MKTO-User-Token | {accessToken} |

Aanvragingsinstantie | Sleutel | Gegevenstype | Vereist | Waarde | Standaardwaarde | |—|—|—|—|—| | prioriteit | String | Nee | Prioriteit van het verzoek:normaal hoog | normaal | | dedupeBy | String | Nee | Attributen die moeten worden gededupliceerd op:dedupeFieldsmarttoGUID | dedupeFields | | customObjects | Array van object | Ja | Lijst met kenmerknaam-waardeparen voor het object. | - |

| Machtiging |
|---|
| Aangepast object lezen/schrijven |

#### Persoon niet aanwezig

Als een koppelingsgebied aan een Persoon in het verzoek wordt gespecificeerd en die Persoon niet bestaat, komen verscheidene opnieuw voor. Als die persoon tijdens het venster Opnieuw proberen is toegevoegd (65 minuten), is de update gelukt. Als het koppelingsveld bijvoorbeeld `email` op Persoon, en Persoon bestaat niet, dan komen opnieuw proberen voor.

#### Voorbeeld van aangepaste objecten

```
POST /subscriptions/{munchkinId}/customobjects/{customObjectAPIName}
```

```
Content-Type: application/json
X-Mkto-User-Token: {accessToken}
```

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

```
HTTP/1.1 202
X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw
```

## Limieten

Hier volgt een overzicht van het gebruik van hulplijnen:

- Maximale grootte van aanvraag: 1 MB
- Maximale objecten per aanvraag per objecttype: 1.000
- Maximale aanvragen per seconde per client-id: 5.000
- Maximale aantal objecten per dag: 10.000.000

## API voor gegevensverwerking versus REST-API

Hier volgt een lijst met verschillen tussen de API voor gegevensverwerking en andere Marketo REST-API&#39;s:

- Dit is geen volledige CRUD interface, het steunt &quot;upsert&quot;slechts
- Om voor authentiek te verklaren, moet u toegangstoken overgaan gebruikend `X-Mkto-User-Token` header
- De URL-domeinnaam is `mkto-ingestion-api.adobe.io`
- Het URL-pad begint met `/subscriptions/_MunchkinId_`
- Er zijn geen queryparameters
- Als de vraag succesvol is, is een status 202 teruggekeerd en het antwoordlichaam leeg is
- Als de vraag ontbreekt, is een status niet-202 teruggekeerd en het reactiekader bevat `{ "error_code" : "_Error Code_", "message" : "_Message_" }`
- De aanvraag-id wordt geretourneerd via `X-Request-Id` header
