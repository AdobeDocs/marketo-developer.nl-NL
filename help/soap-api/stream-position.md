---
title: Stroompositie
feature: SOAP
description: Verklaart stroompositie voor het pagineren van tijd rangschikte gegevens in SOAP, eenvoudige en complexe formaten, en gebruik in getLeadChanges, getLeadActivity, en meer
exl-id: c3a3fc1e-086b-4822-b2c7-2a7959db557c
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '156'
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
