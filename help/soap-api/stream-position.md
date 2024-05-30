---
title: "Stroompositie"
feature: SOAP
description: "Overzicht van de stuurpositie"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---


# Stroompositie

De elementen van de stroompositie bevatten een positieverwijzing voor één of meerdere logische stromen van tijd geordende gegevens. De positieverwijzing kan een benaderende externe specificatie zoals een timestamp, of een ondoorzichtige interne specificatie van positie zijn die door een vorige API vraag is teruggekeerd. Stroomposities kunnen worden gedefinieerd als een complex type met meerdere elementen of kunnen een tekenreeks zijn.

De stroompositie wordt gebruikt om gegevens in partijen terug te winnen, en staat de bezoeker toe om door het resultaat te pagineren. De streampositie die binnen een API-aanvraag wordt doorgegeven, is de waarde van de streampositie die in de vorige reactie is geretourneerd. Het wordt niet aanbevolen de streampositie te wijzigen die door de vorige API-aanroep is geretourneerd, en dit kan leiden tot onverwacht API-gedrag.

## API&#39;s die de stroompositie ondersteunen

- [getCustomObjects](getcustomobjects.md)
- [getLeadChanges](getleadchanges.md)
- [getLeadActivity](getleadactivity.md)
- [getMObjects](getmobjects.md)
- [getMultipleLeads](getmultipleleads.md)

## Eenvoudige streampositie

```
<streamPosition>8UJZetaMb1V6uUZl+L7DcPP2jG+PMmtpF</streamPosition>
```

## Complexe streampositie

```xml
<startPosition>
  <latestCreatedAt  />
  <oldestCreatedAt>2013-08-01T00:13:13+00:00</oldestCreatedAt>
  <activityCreatedAt  />
  <offset>ID:1086173</offset>
</startPosition>
```
