---
title: Diepe koppelingen inschakelen
feature: Mobile Marketing
description: Leer hoe u uitgebreide koppelingen in uw app voor Marketo-pushberichten inschakelt met behulp van aangepaste URI-schema's, iOS, Android en PhoneGap-richtlijnen en tips en trucs.
exl-id: c3647416-d81d-4f15-b660-bcb3e54cb9bc
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 0%

---

# Diepe koppelingen inschakelen

Met Diepe koppeling kunt u personen omleiden naar specifieke inhoud (bronnen) in uw app. Wanneer iemand bijvoorbeeld op een mobiel pushbericht klikt dat adverteert met een paarse t-shirt, kunt u de app rechtstreeks openen op de paarse t-shirtinhoud (in plaats van op de startpagina).

Het proces werkt als volgt:

1. De Marketo-gebruiker plaatst een aangepaste URI in de actie Tap voor zijn pushbericht.
1. Wanneer iemand op het pushbericht tikt op het apparaat, activeert de Marketo MME SDK een gebeurtenis met de aangepaste URI.
1. Uw app verwerkt de gebeurtenis en leidt de persoon om naar de juiste inhoud in uw app.

Hiervoor moet u een aangepaste URI-structuur voor uw app definiëren, het schema in het manifest van uw app registreren en vervolgens code toevoegen om deep link-gebeurtenissen te verwerken en naar de juiste locatie in uw app te gaan.

Voor iOS, verwijs naar de documentatie van Apple op [ die een Regeling van Douane URL voor Uw App ](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app) bepaalt.

Voor Android, verwijs naar de documentatie van Google op [ toelatend Diepe Verbindingen voor de Inhoud van de Toepassing ](https://developer.android.com/training/app-links/deep-linking).

Voor PhoneGap-apps is deep linking niet zo direct voorwaarts als bij native iOS- of Android-apps, maar er zijn plug-ins waarmee uw hybride app kan reageren op deep link-aangepaste URL-schema&#39;s en Universal/App-koppelingen op zowel iOS als Android. Overweeg [ deze plug-ins ](https://cordova.apache.org/plugins/?q=deeplink).

Wanneer u deep linking hebt ingeschakeld in uw app, deelt u uw aangepaste URI&#39;s met uw Marketo-gebruikers zodat deze ze kunnen invoegen in de Tap-actie voor pushberichten.

Marketo gebruikt een vooraf gedefinieerde URI-structuur bij het instellen van testapparaten. Verwijs naar de &quot;sectie van de Apparaten van de Test&quot;van de [ Gids van de Installatie ](installation.md) voor meer informatie.

## Aanbevolen procedures voor het definiëren van een URI-structuur

Als uw merk een bestaande mobiele site heeft, kunt u het beste de URL-structuur voor de diepe koppeling-URI volgen. Als `https://myappname.com/products/purple-shirt` bijvoorbeeld uw websiteadres voor het product in kwestie is, zou `myappname://products/purple-shirt` een goede structuur van diepe verbindingsURI zijn om in uw app te gebruiken.

In het algemeen moeten uw schema&#39;s uniek zijn voor uw merk. Hoewel er momenteel geen regels zijn om regelingen wereldwijd uniek te maken, kunt u er zeker van zijn dat uw schema&#39;s uniek zijn door uw domeinnaam om te keren (bijvoorbeeld `org.companyname` ).
