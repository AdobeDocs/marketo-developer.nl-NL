---
title: Pagingtokens
feature: REST API
description: Gebruik Marketo REST API-pagineringstokens om activiteiten en leads op te halen, inclusief op datum gebaseerde en op positie gebaseerde tokens, ISO 8601 sinceDatetime en 414 fouten.
exl-id: 63fbbf03-8daf-4add-85b0-a8546c825e5b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# Pagingtokens

Marketo biedt paginatokens om de resultaten te doorbladeren of gegevens op te halen die zijn bijgewerkt ten opzichte van een bepaalde gegevens.

In bepaalde gevallen kunnen lange paginering tokentekenreeksen worden geretourneerd. Hierdoor kan een HTTP 414-foutcode optreden. U kunt meer informatie vinden over hoe te om deze [ fouten ](error-codes.md) te behandelen.

Zie [ het Pagelen Token API ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET) documentatie.

## Typen token

Er zijn twee verwante, maar verschillende typen paginatokens die Marketo biedt:

- Op datum gebaseerd
- Op positie gebaseerd

## Op datum gebaseerd

Het eerste is een paginatoken die een datum vertegenwoordigt. Deze worden gebruikt om activiteiten, veranderingen van de gegevenswaarde, en geschrapte lood terug te winnen die na de datum voorkwamen die door het pagineren teken wordt vertegenwoordigd. Dit type van het pagineren van teken wordt geproduceerd door [ te roepen krijgt het Pagelen Token ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET) eindpunt en met inbegrip van datetime.

```
GET /rest/v1/activities/pagingtoken.json?sinceDatetime=2014-10-06T13:22:17-08:00
```

```json
{
    "requestId": "1607c#14884f3e74e",
    "success": true,
    "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
}
```

Het formaat van de `sinceDateTime` parameter moet [ ISO 8601 ](https://en.wikipedia.org/wiki/ISO_8601) standaarddatumaantekening in overeenstemming zijn. Voor beste resultaten, gebruik een volledige datetime die de tijdstreek omvat. De tijdzone kan als of een compensatie van GMT worden vertegenwoordigd gebruikend het volgende formaat:

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

Of het gebruiken van een hoofdletter &quot;Z&quot;als steno om UTC als dit te vertegenwoordigen:

`yyyy-mm-ddThh:mm:ssZ`

Voorbeelden

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

Omdat `sinceDateTime` een queryparameter is, moet deze URL-gecodeerd zijn.

Het `nextPageToken` koord wordt dan verstrekt aan a [ krijgt de Activiteiten van het Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET), [ krijgt de Veranderingen van het Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET), of [ krijgen Geschrapte vraag van Leads ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET), en de activiteiten worden teruggewonnen van na datetime die aan Get het Pageren Symbolische API wordt verstrekt.

```
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## Op positie gebaseerd

Het tweede type van het pagineren teken kan door om het even welke vraag van de partijherwinning aan een gegevensbestand API van het Lood zijn teruggekeerd. Dit type het pagineren teken is gelijkaardig in concept aan een gegevensbestandcurseur die verkeer van verslagen toelaat. Bijvoorbeeld, kan een Get Leidingen door de vraag van het Type van Filter een reeks groter dan de bepaalde partijgrootte vertegenwoordigen, gewoonlijk maximum en gebrek van 300. Als er meer resultaten zijn, is het veld moreResult waar in de reactie en wordt een `nextPageToken` geretourneerd. Om de extra verslagen in de resultaatreeks terug te winnen, een extra vraag die `nextPageToken` met de ontvangen waarde van de vorige reactie in de nieuwe vraag omvat. De resulterende reactie retourneert de volgende pagina in de resultaatset.
