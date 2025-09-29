---
title: Patroonovereenkomst
description: Gebruik het RTP rtp.checkPattern nut om koordpatronen met percentenvervangingen te testen, synchronisatielimieten, gebruik en URL voorbeelden, en vereiste de markeringsopstelling van RTP te zien.
feature: Javascript
exl-id: 4ebd13e3-375b-449b-850f-3b18f570ca75
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 1%

---

# Patroonovereenkomst

RTP stelt een nutsfunctie bloot om te controleren of het patroon bepaalde koord aanpast. Het nut kan niet in async worden gebruikt omdat het een aanwijzing terugkeert als er een gelijke of niet is.

U moet een klant van Personalization van het Web worden en de [ markering hebben RTP die ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) op uw plaats wordt opgesteld voorafgaand aan het gebruiken van de Context API van de Gebruiker.

## Gebruik

> rtp.checkPattern(check_out, pattern);

| Parameter | Optioneel/vereist | Type | Beschrijving |
|---|---|---|---|
| check_out | Vereist | String | Tekenreeks die overeenkomt met het patroon. Voorbeeld: URL huidige pagina, productnaam. |
| patroon | Vereist | String | % toevoegen voor jokerteken. Het patroon kan :start zonder volledige gelijke zijn |

## Voorbeelden

Stel de aangepaste variabele in index 1 in als de URL van de huidige pagina eindigt met &quot;productA&quot;.

```javascript
if (rtp.checkPattern(window.location.href, '%productA')) {
    rtp('set', 'custom1', 'productA');
}
```

Het huidige URL-pad is &#39;/products/productB&#39;. In dit voorbeeld wordt gecontroleerd of het pad &quot;producten&quot; bevat en wordt een aangepaste variabele ingesteld.

```javascript
var currentURLPath = '/products/productB';
if (rtp.checkPattern(currentURLPath, '%products%')) {
    rtp('set', 'custom1', 'products');
}
```
