---
title: "Aangepaste gegevensgebeurtenissen"
description: "Custom Data Events API"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---


# Aangepaste gegevensgebeurtenissen

Deze methode verzendt douanegebeurtenissen voor het volgen en verpersoonlijking in real time. Deze kan worden gebruikt om gegevens van derden te verzenden of om uw eigen aangepaste gebeurtenis te activeren op basis van het gedrag van de bezoeker. Aangepaste gegevensgebeurtenissen worden één keer geteld in de sessie van een bezoeker.

U moet een klant van de Personalisatie van het Web worden en hebben [RTP-tag geïmplementeerd](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) op uw site voordat u de Context-API van de gebruiker gebruikt.

| Parameter | Optioneel/vereist | Type | Beschrijving |
|---|---|---|---|
| `send` | Vereist | String | Methode, actie. |
| `event` | Vereist | String | Naam van methode. |
| `customData` | Vereist | Tekenreeks of array | Aangepaste gegevens. |

## Voorbeelden

### Gebeurtenis verzenden met String voor aangepaste gegevens

```javascript
var customData = {value: 'MyEvent'};
rtp('send', 'event', customData);
```

### Gebeurtenis verzenden met Array van tekenreeksen voor aangepaste gegevens

De aangepaste gegevensarray kan maximaal vier elementen bevatten.  Als u meer dan vier elementen moet verzenden, roept u de API voor verzending van gebeurtenissen herhaaldelijk aan (met een maximum van vier items) totdat alle items zijn verzonden.

```javascript
var customData = {value: ['MyEvent', 'download - example whitepaper']};
rtp('send', 'event', customData);
```

### Gebeurtenis verzenden op basis van klikken op knop

Marketo past de inhoud van hun website aan webbezoekers die een bepaald artikel downloaden. Ze doen dit door de bezoeker op de knop voor het downloaden van witboeken te laten klikken, die een gebeurtenis met aangepaste gegevens verzendt. RTP segmenteert in real time alle bezoekers die de knoop van het downloadWitboek klikte, die elke bezoeker een gepersonaliseerde campagne tonen die 2 kliks later aanbiedt. Dit wordt bereikt door een ander stuk inhoud weer te geven dat betrekking heeft op het gedownloade witboek.

```html
<button id="download-whitepaper" onclick="rtp('send', 'event', {value :'download - example whitepaper'})">Download</button>
```
