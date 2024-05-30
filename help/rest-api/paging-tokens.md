---
title: "Paging Tokens"
feature: REST API
description: "Gegevens voor paginerende tokens weergeven."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 0%

---


# Pagingtokens

Marketo biedt paginatokens om de resultaten te doorbladeren of gegevens op te halen die zijn bijgewerkt ten opzichte van een bepaalde gegevens.

In bepaalde gevallen kunnen lange paginering tokentekenreeksen worden geretourneerd. Hierdoor kan een HTTP 414-foutcode optreden. Meer informatie over hoe deze te verwerken zijn [fouten](error-codes.md).

## Typen token

Er zijn twee verwante, maar verschillende typen paginatokens die Marketo biedt:

- Op datum gebaseerd
- Op positie gebaseerd

## Op datum gebaseerd

Het eerste is een paginatoken die een datum vertegenwoordigt. Deze worden gebruikt om activiteiten, veranderingen van de gegevenswaarde, en geschrapte lood terug te winnen die na de datum voorkwamen die door het pagineren teken wordt vertegenwoordigd. Dit type het pagineren teken wordt geproduceerd door het roepen van [Paginasken ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET) en inclusief een datetime.

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

Het formaat van de `sinceDateTime` parameter moet voldoen aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) standaarddatumnotatie. Voor beste resultaten, gebruik een volledige datetime die de tijdstreek omvat. De tijdzone kan als of een compensatie van GMT worden vertegenwoordigd gebruikend het volgende formaat:

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

Of het gebruiken van een hoofdletter &quot;Z&quot;als steno om UTC als dit te vertegenwoordigen:

`yyyy-mm-ddThh:mm:ssZ`

Voorbeelden

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

Omdat `sinceDateTime` Is een vraagparameter, moet het URL gecodeerd zijn.

De `nextPageToken` tekenreeks wordt vervolgens aan een [Leadactiviteiten ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET), [Wijzigingen in lead ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET), of [Verwijderde leads ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) De vraag, en de activiteiten worden teruggewonnen van na datetime die aan Get het Pagineren Symbolische API wordt verstrekt.

```
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## Op positie gebaseerd

Het tweede type van het pagineren teken kan door om het even welke vraag van de partijherwinning aan een gegevensbestand API van het Lood zijn teruggekeerd. Dit type het pagineren teken is gelijkaardig in concept aan een gegevensbestandcurseur die verkeer van verslagen toelaat. Bijvoorbeeld, kan een Get Leidingen door de vraag van het Type van Filter een reeks groter dan de bepaalde partijgrootte vertegenwoordigen, gewoonlijk maximum en gebrek van 300. Als er meer resultaten zijn, is het veld moreResult waar in de reactie en wordt een `nextPageToken` wordt geretourneerd. Om de extra verslagen in de resultaatreeks terug te winnen, omvat een extra vraag `nextPageToken` met de ontvangen waarde van de vorige reactie in de nieuwe vraag. De resulterende reactie retourneert de volgende pagina in de resultaatset.
