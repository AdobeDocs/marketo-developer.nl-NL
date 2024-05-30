---
title: "Diepe koppelingen inschakelen"
feature: "Mobile Marketing"
description: "Instructies voor het inschakelen van Diepe koppelingen"
source-git-commit: cb000968c78e062b3c17be7d0faa6236c73e7358
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---


# Diepe koppelingen inschakelen

Met Diepe koppeling kunt u personen omleiden naar specifieke inhoud (bronnen) in uw app. Wanneer iemand bijvoorbeeld op een mobiel pushbericht klikt dat adverteert met een paarse t-shirt, kunt u de app rechtstreeks openen op de paarse t-shirtinhoud (in plaats van op de startpagina).

Het proces werkt als volgt:

1. De Marketo-gebruiker plaatst een aangepaste URI in de actie Tap voor zijn pushbericht.
1. Wanneer iemand op het pushbericht tikt op het apparaat, activeert de Marketo MME SDK een gebeurtenis met de aangepaste URI.
1. Uw app verwerkt de gebeurtenis en leidt de persoon om naar de juiste inhoud in uw app.

Hiervoor moet u een aangepaste URI-structuur voor uw app definiëren, het schema in het manifest van uw app registreren en vervolgens code toevoegen om deep link-gebeurtenissen te verwerken en naar de juiste locatie in uw app te gaan.

Raadpleeg voor iOS de documentatie bij Apple op [Een aangepast URL-schema voor uw app definiëren](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app).

Raadpleeg voor Android de documentatie bij Google op [Diepe koppelingen inschakelen voor App Content](https://developer.android.com/training/app-links/deep-linking).

Voor PhoneGap-apps is deep linking niet zo direct voorwaarts als bij native iOS- of Android-apps, maar er zijn insteekmodules waarmee uw hybride app kan reageren op deep link-aangepaste URL-schema&#39;s en Universal/App-koppelingen op zowel iOS als Android. Overweeg [deze plug-ins](https://cordova.apache.org/plugins/?q=deeplink).

Wanneer u deep linking hebt ingeschakeld in uw app, deelt u uw aangepaste URI&#39;s met uw Marketo-gebruikers zodat deze ze kunnen invoegen in de Tap-actie voor pushberichten.

Marketo gebruikt een vooraf gedefinieerde URI-structuur bij het instellen van testapparaten. Raadpleeg het gedeelte &quot;Testapparaten&quot; van het dialoogvenster [Installatiehandleiding](installation.md) voor meer informatie .

## Aanbevolen procedures voor het definiëren van een URI-structuur

Als uw merk een bestaande mobiele site heeft, kunt u het beste de URL-structuur voor de diepe koppeling-URI volgen. Als `https://myappname.com/products/purple-shirt` is uw websiteadres voor het product in kwestie, dan `myappname://products/purple-shirt` Dit is een goede URI-structuur voor een diepe koppeling die u in uw app kunt gebruiken.

In het algemeen moeten uw schema&#39;s uniek zijn voor uw merk. Hoewel er momenteel geen regelgeving is om regelingen wereldwijd uniek te maken, kunt u er zeker van zijn dat uw schema&#39;s uniek zijn, bijvoorbeeld door uw domeinnaam om te keren (bijvoorbeeld `org.companyname`).
