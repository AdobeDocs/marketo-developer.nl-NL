---
title: "Patroonovereenkomst"
description: "Patroonovereenkomst"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 0%

---


# Patroonovereenkomst

RTP stelt een nutsfunctie bloot om te controleren of het patroon bepaalde koord aanpast. Het nut kan niet in async worden gebruikt omdat het een aanwijzing terugkeert als er een gelijke of niet is.

U moet een klant van de Personalisatie van het Web worden en hebben [RTP-tag geïmplementeerd](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) op uw site voordat u de Context-API van de gebruiker gebruikt.

## Gebruik

> rtp.checkPattern(check_out, pattern);

| Parameter | Optioneel/vereist | Type | Beschrijving |
|---|---|---|---|
| check_out | Vereist | String | Tekenreeks die overeenkomt met het patroon. Voorbeeld: URL huidige pagina, productnaam. |
| patroon | Vereist | String | % toevoegen voor jokerteken. Het patroon kan zijn:begin met volledige overeenkomst |


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
