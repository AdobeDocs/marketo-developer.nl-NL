---
title: Aangepaste handelingen
feature: Mobile Marketing
description: Leer aangepaste acties te verzenden en te melden met de Marketo Mobile SDK for iOS en Android, offline in de wachtrij te plaatsen, Slimme campagnes te activeren en aan de 20-lettertekens te voldoen...
exl-id: 8c2698ce-4e39-4b2b-9d36-0864c55be17a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 0%

---

# Aangepaste handelingen

U kunt gebruikersinteractie bijhouden door aangepaste handelingen te verzenden. Wanneer uw mobiele app de Marketo SDK aanroept om een aangepaste handeling te verzenden, wordt de aangepaste handeling eerst opgeslagen op het apparaat. De Marketo SDK controleert vervolgens of er voldoende internetverbinding is voordat de aangepaste actie wordt verzonden. Het gevolg is dat er mogelijk een vertraging optreedt tussen het moment dat de aangepaste handeling wordt verzonden en het moment dat de aangepaste handeling door Marketo wordt ontvangen.

Aangepaste acties kunnen worden gebruikt als triggers en filters in slimme campagnes. Voor meer informatie, zie [&#x200B; Mobiele Activiteit van de Toepassing &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

## Aangepaste acties verzenden op iOS

Aangepaste actie verzenden.

>[!BEGINTABS]

>[!TAB  Doelstelling C ]

```
Marketo *sharedInstance = [Marketo sharedInstance];
[sharedInstance reportAction:@"Login" withMetaData:nil];
```

>[!TAB  Swift ]

```
sharedInstance.reportAction("Login", withMetaData:nil);
```

>[!ENDTABS]

Aangepaste actie verzenden met metagegevens.

>[!BEGINTABS]

>[!TAB  Doelstelling C ]

```
MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
[meta setType:@"Shopping"];
[meta setDetails:@"RedShirt"];
[meta setLength:20];
[meta setMetric:30];

[sharedInstance reportAction:@"Bought Shirt" withMetaData:meta];
```

>[!TAB  Swift ]

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

>[!TAB  Doelstelling C ]

```
[sharedInstance reportAll];
```

>[!TAB  Swift ]

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

Het instellen van aangepaste mobiele acties is eenvoudig, maar er gelden beperkingen voor het aantal tekens dat u kunt verzenden van de Mobile SDK naar Marketo. Zorg ervoor dat al uw aangepaste handelingen die via de mobiele SDK worden gemeld, minder dan 20 tekens lang zijn.

**Nota op multi-user gebruiksgevallen op een gedeeld apparaat:** wanneer een gebruiker zich in een mobiele toepassing aanmeldt die met Marketo SDK wordt geïntegreerd, wordt de eerste vraag gemaakt om het lood met toepassing te associëren installeert. Nadat deze aanroep is voltooid, kunnen verdere gebruikersactiviteiten in de app worden weergegeven in het activiteitenlogboek van de lead. Nota, aangezien dit een asynchrone vraag is als er om het even welke douaneacties die onmiddellijk na login worden geregistreerd kunnen zij met de gebruiker worden geassocieerd die eerder het programma werd geopend tot de associatieve vraag slaagt.
