---
title: "Aangepaste handelingen"
feature: "Mobile Marketing"
description: Overzicht van aangepaste handelingen
source-git-commit: c51e1b84efdf444c13714c1a08ecc4cac677f483
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---


# Aangepaste handelingen

U kunt gebruikersinteractie bijhouden door aangepaste handelingen te verzenden. Wanneer uw mobiele app naar de SDK van Marketo aanroept om een aangepaste handeling te verzenden, wordt de aangepaste handeling eerst opgeslagen op het apparaat. De SDK van Marketo controleert vervolgens of er voldoende internetverbinding is voordat de aangepaste handeling wordt verzonden. Het gevolg is dat er mogelijk een vertraging optreedt tussen het moment dat de aangepaste handeling wordt verzonden en het moment dat de aangepaste handeling door Marketo wordt ontvangen.

Aangepaste acties kunnen worden gebruikt als triggers en filters in slimme campagnes. Zie voor meer informatie [Mobiele toepassingsactiviteit](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

## Aangepaste acties verzenden op iOS

Aangepaste actie verzenden.

>[!BEGINTABS]

>[!TAB Doelstelling C]

```
Marketo *sharedInstance = [Marketo sharedInstance];
[sharedInstance reportAction:@"Login" withMetaData:nil];
```

>[!TAB Swift]

```
sharedInstance.reportAction("Login", withMetaData:nil);
```

>[!ENDTABS]

Aangepaste actie verzenden met metagegevens.

>[!BEGINTABS]

>[!TAB Doelstelling C]

```
MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
[meta setType:@"Shopping"];
[meta setDetails:@"RedShirt"];
[meta setLength:20];
[meta setMetric:30];

[sharedInstance reportAction:@"Bought Shirt" withMetaData:meta];
```

>[!TAB Swift]

```
let meta = MarketoActionMetaData()
meta.setType("Shopping");
meta.setDetails("RedShirt");
meta.setLength(20);
meta.setMetric(30);

sharedInstance.reportAction("Bought Shirt", withMetaData:meta);
```

>[!ENDTABS]

Alle handelingen direct rapporteren (alle opgeslagen handelingen verzenden).

>[!BEGINTABS]

>[!TAB Doelstelling C]

```
[sharedInstance reportAll];
```

>[!TAB Swift]

```
sharedInstance.reportAll();
```

>[!ENDTABS]

## Aangepaste acties verzenden op Android

1. Aangepaste actie verzenden.

   ```
   Marketo.reportAction("Login", null);
   ```

1. Aangepaste actie verzenden met metagegevens.

   ```
   MarketoActionMetaData meta = new MarketoActionMetaData();
   meta.setActionType("Shopping");
   meta.setActionDetails("RedShirt");
   meta.setActionLength("20");
   meta.setActionMetric("30");
   
   Marketo.reportAction("Bought Shirt", meta);
   ```

1. Alle aangepaste handelingen direct rapporteren (alle opgeslagen handelingen verzenden).

   ```
   Marketo.reportAll();
   ```

## Aangepaste acties oplossen

Aangepaste acties voor mobiele apparaten instellen is eenvoudig, maar er gelden beperkingen voor het aantal tekens dat u kunt verzenden van de SDK voor mobiele apparaten naar Marketo. Zorg ervoor dat al uw aangepaste handelingen die via de mobiele SDK aan Marketo worden gemeld, minder dan 20 tekens lang zijn.

**Opmerking over gebruiksgevallen voor meerdere gebruikers op een gedeeld apparaat:** Wanneer een gebruiker zich aanmeldt bij een mobiele toepassing die is geïntegreerd met de SDK van Marketo, wordt de eerste aanroep gedaan om de lead te koppelen aan de installatie van de app. Nadat deze aanroep is voltooid, kunnen verdere gebruikersactiviteiten in de app worden weergegeven in het activiteitenlogboek van de lead. Nota, aangezien dit een asynchrone vraag is als er om het even welke douaneacties die onmiddellijk na login worden geregistreerd kunnen zij met de gebruiker worden geassocieerd die eerder het programma werd geopend tot de associatieve vraag slaagt.
