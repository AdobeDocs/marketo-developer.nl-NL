---
title: Triggers
description: Triggers
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 2%

---

# Triggers

Hiermee voegt u de mogelijkheid toe om functies te activeren op een bepaalde status van het algemene rtp-object.

U moet een klant van Personalization van het Web zijn en de [ markering hebben RTP die ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) op uw plaats wordt opgesteld alvorens de Context API van de Gebruiker te gebruiken.

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
