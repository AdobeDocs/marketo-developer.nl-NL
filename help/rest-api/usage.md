---
title: Gebruik
feature: REST API
description: Het gebruik van de Marketo REST API en fouten met de eindpunten van de dagelijkse en laatste 7 dagen stats, inclusief tellingen per gebruiker en getallen van foutcode controleren.
source-git-commit: 73fa4c85ecabd4cfd24bc6591aad11dc4e75010a
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 5%

---

# Gebruik

[Gebruik eindpuntverwijzing](https://developer.adobe.com/marketo-apis/api/mapi#tag/Usage)

De gebruikers-API&#39;s bevatten een overzicht van het gebruik van de REST API en de foutactiviteit van uw abonnement. Deze eindpunten zijn nuttig om integratie te controleren, dagelijks vraagvolume te volgen, en foutentendensen in tijd te identificeren.

De gegevens van het gebruik omvatten een totaal aantal API vraag en een per-gebruikersuitsplitsing. Foutgegevens bevatten een totaal aantal fouten en een indeling naar foutcode.

De Gebruiks-API&#39;s gebruiken dezelfde verificatiemethode als andere Marketo REST-API&#39;s. Geef het toegangstoken door in de header `Authorization: Bearer {accessToken}` .

## Eindpunten

| Methode | Pad | Beschrijving |
| --- | --- | --- |
| GET | `/rest/v1/stats/usage.json` | Hiermee wordt het API-gebruik voor de huidige dag opgehaald. |
| GET | `/rest/v1/stats/usage/last7days.json` | Hiermee wordt het API-gebruik voor de laatste 7 dagen opgehaald. |
| GET | `/rest/v1/stats/errors.json` | Hiermee worden API-fouten voor de huidige dag opgehaald. |
| GET | `/rest/v1/stats/errors/last7days.json` | Hiermee worden API-fouten van de laatste 7 dagen opgehaald. |

## Dagelijks gebruik

Hiermee wordt het API-gebruik voor de huidige dag opgehaald.

```
GET /rest/v1/stats/usage.json
```

```json
{
   "requestId": "5f7f#17d7d8f2b6f",
   "success": true,
   "result": [
      {
         "date": "2015-10-17",
         "total": 1120,
         "users": [
            {
               "userId": "some.body@yahoo.com",
               "count": 200
            },
            {
               "userId": "some.body@marketo.com",
               "count": 200
            },
            {
               "userId": "some.body@gmail.com",
               "count": 720
            }
         ]
      }
   ]
}
```

Elk object in de array `result` bevat een gebruiksduur van één dag en een uitsplitsing per gebruiker.

## Gebruik van de laatste 7 dagen

Hiermee wordt het API-gebruik voor de laatste 7 dagen opgehaald. Elk element in de array `result` vertegenwoordigt één dag.

```
GET /rest/v1/stats/usage/last7days.json
```

## Dagelijkse fouten

Hiermee worden API-fouten voor de huidige dag opgehaald.

```
GET /rest/v1/stats/errors.json
```

```json
{
   "requestId": "5f7f#17d7d8f2b6f",
   "success": true,
   "result": [
      {
         "date": "2015-10-17",
         "total": 73,
         "errors": [
            {
               "errorCode": "604",
               "count": 1
            },
            {
               "errorCode": "609",
               "count": 56
            },
            {
               "errorCode": "610",
               "count": 16
            }
         ]
      }
   ]
}
```

Elk object in de array `result` bevat een dag met fouttotalen en een verdeling op basis van foutcode.

## Fouten van afgelopen 7 dagen

Hiermee worden API-fouten van de laatste 7 dagen opgehaald. Elk element in de array `result` vertegenwoordigt één dag.

```
GET /rest/v1/stats/errors/last7days.json
```

## Antwoordleden

### Gebruiksresultaat-object

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| `date` | String | De datum voor het gebruiksoverzicht in `YYYY-MM-DD` formaat. |
| `total` | Geheel | Het totale aantal API-aanroepen voor die dag. |
| `users` | Array | Lijst van het aantal per-gebruikersgebruik tellingen voor die dag. |

### Gebruikersobject gebruiken

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| `userId` | String | API-gebruikersnaam. |
| `count` | Geheel | Aantal API vraag die door die gebruiker voor de dag wordt gemaakt. |

### Foutresultaatobject

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| `date` | String | De datum voor de foutensamenvatting in `YYYY-MM-DD` formaat. |
| `total` | Geheel | Het totale aantal API-fouten voor die dag. |
| `errors` | Array | Lijst van per-fout-code tellingen voor die dag. |

### Foutobject

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| `errorCode` | String | Marketo-foutcode. |
| `count` | Geheel | Aantal keren dat die fout voor de dag voorkwam. |

## Notities

Elk van uw API-gebruikers wordt afzonderlijk vermeld in de gebruiksreactie. Door de integratie van verschillende API-gebruikers te splitsen, kunt u gemakkelijker zien welke service quota verbruikt en fouten veroorzaakt.
