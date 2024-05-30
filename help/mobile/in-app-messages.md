---
title: "In-app berichten"
feature: "Mobile Marketing"
description: "Overzicht van In-App-berichten"
source-git-commit: e8bb45a7b3bee71c3d0ab6117296a75c8959d72e
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---


# In-app berichten

Als u de communicatiemogelijkheden in de app van Marketo wilt gebruiken, moet u de volgende stappen uitvoeren:

1. Installeer de Marketo Mobile SDK zoals beschreven in het dialoogvenster [Mobiele installatie](installation.md).
1. Uw mobiele app toevoegen aan Marketo zoals beschreven in [Een mobiele app toevoegen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app).
1. Voeg desgewenst code toe aan uw mobiele app om vast te leggen [Aangepaste handelingen](custom-actions.md).

Nadat u de Marketo Mobile SDK hebt geïnstalleerd en uw app in Marketo hebt toegevoegd, kunt u in de app berichten verzenden die worden weergegeven wanneer een gebruiker uw app opent.

Standaard worden in-app berichten geactiveerd wanneer uw app wordt geopend. Als u in-app berichten voor andere gebeurtenissen wilt teweegbrengen, zoals wanneer een specifieke pagina wordt bekeken, of wanneer een specifieke knoop wordt geduwd, moet u douaneacties aan uw code toevoegen. Zie [Aangepaste handelingen](custom-actions.md) voor codevoorbeelden hiervan.

## Problemen oplossen

**Bericht in de app wordt niet weergegeven**

Marketo reageert pas op triggers van apps nadat de Marketo Mobile SDK is geïnitialiseerd met het Marketo-platform. Het initialisatieproces treedt op wanneer u de app voor het eerst installeert en opent. Aangezien de initialisatie plaatsvindt nadat de eerste app is geopend, wordt de gebeurtenis &quot;App Open&quot; pas geactiveerd wanneer de app een tweede keer wordt geopend. Sluit de app en open deze opnieuw en er verschijnt een bericht dat wordt geactiveerd door App Open op uw apparaat.

Aangepaste gebeurtenissen worden geactiveerd door gebruikersinteractie nadat de app is geopend. Aangepaste gebeurtenissen worden door Marketo tijdens de eerste sessie herkend.

**In-app Tap Activity Tracking**

Zorg ervoor dat u een actie naast &quot;sluiten&quot; toewijst aan een van de primaire of secundaire knoppen om de tikactiviteiten te volgen en basisweergavefrequenties te gebruiken op basis van het aantal tikken.

Zie voor meer informatie de [In-app berichten](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message) in de productdocumentatie.
