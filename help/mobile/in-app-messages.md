---
title: In-app berichten
feature: Mobile Marketing
description: Stel Marketo In-App-berichten in met de Mobile SDK, configureer aangepaste gebeurtenistriggers, controleer de tikactiviteit en los de initialisatieproblemen bij het openen van de eerste app op.
exl-id: 73c9f862-d154-4b37-94ce-92311aa756e8
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 0%

---

# In-app berichten

Als u de communicatiemogelijkheden in de app van Marketo wilt gebruiken, moet u de volgende stappen uitvoeren:

1. Installeer Marketo Mobile SDK zoals die in de [&#x200B; Mobiele Installatie &#x200B;](installation.md) wordt beschreven.
1. Voeg uw mobiele app aan Marketo toe zoals die in [&#x200B; wordt beschreven een Mobiele App &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) toevoegt.
1. Naar keuze, voeg code aan uw mobiele app toe om [&#x200B; Acties van de Douane &#x200B;](custom-actions.md) te vangen.

Nadat u de Marketo Mobile SDK hebt geïnstalleerd en uw app in Marketo hebt toegevoegd, kunt u in de app berichten verzenden die worden weergegeven wanneer een gebruiker uw app opent.

Standaard worden in-app berichten geactiveerd wanneer uw app wordt geopend. Als u in-app berichten voor andere gebeurtenissen wilt teweegbrengen, zoals wanneer een specifieke pagina wordt bekeken, of wanneer een specifieke knoop wordt geduwd, moet u douaneacties aan uw code toevoegen. Zie [&#x200B; sectie van de Acties van de Douane &#x200B;](custom-actions.md) voor codesteekproeven van dit.

## Problemen oplossen

**In-App Bericht wordt niet getoond**

Marketo reageert pas op triggers van apps nadat de Marketo Mobile SDK is geïnitialiseerd met het Marketo-platform. Het initialisatieproces treedt op wanneer u de app voor het eerst installeert en opent. Aangezien de initialisatie plaatsvindt nadat de eerste app is geopend, wordt de gebeurtenis &quot;App Open&quot; pas geactiveerd wanneer de app een tweede keer wordt geopend. Sluit de app en open deze opnieuw en er verschijnt een bericht dat wordt geactiveerd door App Open op uw apparaat.

Aangepaste gebeurtenissen worden geactiveerd door gebruikersinteractie nadat de app is geopend. Aangepaste gebeurtenissen worden door Marketo tijdens de eerste sessie herkend.

**In-app het Volgen van de Activiteit van het Tikken**

Zorg ervoor dat u een actie naast &quot;sluiten&quot; toewijst aan een van de primaire of secundaire knoppen om de tikactiviteiten te volgen en basisweergavefrequenties te gebruiken op basis van het aantal tikken.

Voor extra informatie, zie de [&#x200B; in-App sectie van Berichten &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message) in onze productdocumentatie.
