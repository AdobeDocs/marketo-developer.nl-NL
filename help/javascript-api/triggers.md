---
title: "Triggers"
description: "Triggers"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 0%

---


# Triggers

Hiermee voegt u de mogelijkheid toe om functies te activeren op een bepaalde status van het algemene rtp-object.

U moet een klant van de Personalisatie van het Web zijn en hebben [RTP-tag geïmplementeerd](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) op uw site voordat u de Context-API van de gebruiker gebruikt.

## Gebruik

`rtp('triggerName', function_to_trigger);`

| Parameter | Optioneel/vereist | Type | Beschrijving |
|---------------------|-------------------|----------|----------------------|
| &#39;triggerName&#39; | Vereist | String | Naam van methode. |
| function_to_trigger | Vereist | Functie | Functie die moet worden geactiveerd. |


### Gebruikercontext gereed voor trigger

Hiermee stelt u een aangepaste variabele in op basis van de gebruikerslocatie. Deze functie wordt aangeroepen wanneer het algemene object rtpUserContext gereed is.

```javascript
rtp('userContextReady', function() {
    if (rtpUserContext.location.state == 'CA') {
        rtp('set', 'custom1', 'productA');
    }
});
```
