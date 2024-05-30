---
title: "Web Personalization"
description: "Web Personalization"
feature: Web Personalization, Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%

---


# Webpersonalisatie

De JavaScript-API voor webpersonalisatie breidt de mogelijkheden voor geautomatiseerde personalisatie van het platform uit. Zo kunt u gebeurtenissen bijhouden en een webpagina dynamisch aanpassen. Aanvullende mogelijkheden: [Aangepaste gegevensgebeurtenissen](custom-data-events.md), [Dynamische inhoud](web-personalization.md), [Bezoekergegevens ophalen](get-visitor-data.md), [Label uitsluiten voor specifieke blokken](#exclude_tag_for_specific_bots).

- U moet een klant van de Personalisatie van het Web worden en hebben [RTP-tag geïmplementeerd](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) op uw site voordat u de Context-API van de gebruiker gebruikt.
- RTP ondersteunt geen accountgebaseerde marketing met benoemde accountlijsten. ABM-lijsten en -code hebben alleen betrekking op de geüploade accountlijsten (CSV-bestanden) die in RTP worden beheerd.

## Taginstelling

De markering RTP zou bij de kopbal van de gepersonaliseerde pagina moeten worden opgenomen.

```javascript
<!-- RTP tag --> 
<script type='text/javascript'>
(function(c,h,a,f,e,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].p=e;c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b)})
(window,document,"rtp","[rtp-js-cdn-url]","[pod-url]","[accountId]");
</script>
<!-- End of RTP tag -->
```

## Account instellen

Deze methode wordt automatisch aangeroepen op tagniveau om de relevante account-id in te stellen. U kunt de account-id instellen wanneer u wilt splitsen tussen verschillende domeinen.

| Parameter | Optioneel/vereist | Type | Beschrijving |
|--------------|-------------------|--------|--------------|
| &#39;setAccount&#39; | Vereist | String | Naam van methode. |
| accountId | Vereist | String | Account-ID. |


```javascript
var accountId = '561-HYG-937';
rtp('setAccount', accountId);
```

## Functies voor verzenden van gebeurtenissen

Deze methode verzendt een weergavegebeurtenis die wordt gebruikt voor het bijhouden van pagina&#39;s. In het onderstaande voorbeeld wordt de huidige pagina-URL bijgehouden als een weergave van de bezoekerspagina.

Door de optionele parameter &quot;page&quot; in deze methode door te geven, kan de huidige pagina worden overschreven.

| Parameter | Optioneel/vereist | Type | Beschrijving |
|-----------|-------------------|--------|---------------------------------|
| &#39;send&#39; | Vereist | String | Methode, actie. |
| &#39;view&#39; | Vereist | String | Naam van methode. |
| page | Optioneel | String | Relatief pad of URL van volledige pagina. |


```javascript
// Example for Default Page
rtp('send', 'view');

// Example for Overriding Default Page
var page = 'my-page?param=1';
rtp('send', 'view', page);
```

## Label uitsluiten voor specifieke blokken (gebruikersagenten)

Om specifieke browsers van het verzenden van gegevens naar het platform van de Personalisatie van het Web (in het geval van geïdentificeerde bots) uit te sluiten, voeg de volgende verklaring van IF aan het markeringsmanuscript toe.

In het onderstaande codevoorbeeld wordt &quot;Googlebot|msnbot&quot; gebruikt als beide voorbeelden om activiteiten op het gebied van personalisatie van het web uit te sluiten.

```javascript
<!-- RTP tag --> 
<script type='text/javascript'>
if(navigator.userAgent.match(/.(Googlebot|msnbot)./gi) == null){
    (function(c,h,a,f,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
    c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
    g.src=f+'?rh='+c.location.hostname+'&aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//[cdn-pod-X-url]/rtp-api/v1/rtp.js","[accountId]");

    rtp('send','view');
    rtp('get', 'campaign', true);
}
</script>
<!-- End of RTP tag -->
```

## JavaScript-aanroepen beschreven

Beschrijving van JavaScript dat aan een website wordt toegevoegd wanneer het gebruiken van de Personalisatie van het Web en Voorspelende Inhoud.

### Core/Afhankelijke JavaScript

| Naam | Beschrijving | Besturing |
|---------------------------|-------------|--------------------------------------------------------|
| rtp.js | - | Onder zeggenschap van Marketo |
| jquery.min.js | v1.8.3 | Kan worden uitgeschakeld door contact op te nemen met de klantenondersteuning van Marketo |
| jquery-custom-ui-min.js | v1.9.2 | Kan worden uitgeschakeld door contact op te nemen met de klantenondersteuning van Marketo |
| query-ui-1.8.17-dialog.js | v1.9.2* | Kan worden uitgeschakeld door contact op te nemen met de klantenondersteuning van Marketo |


*Wordt alleen gebruikt als dialoogvenster voor jQuery-gebruikersinterface ontbreekt

### JavaScript op aanvraag

| Naam | Beschrijving | Besturing |
|-------------------------|-----------------------------------------------------------------------|-----------------------|
| ga-integration-2.0.1.js | Wordt gebruikt als integratie tussen Googles Analytics/Facebook/SiteCatalyst is ingeschakeld | Onder zeggenschap van Marketo |
| insightera-bar-2.1.js | Wordt gebruikt als de aanbevolen balk voor voorspellende inhoud is ingeschakeld | Onder zeggenschap van Marketo |
| froogaloop2.min.js | Wordt gebruikt als het bijhouden van inhoud is ingeschakeld en de Vimeo-speler op de pagina aanwezig is | - |
| iframe-api-v1.js | Wordt gebruikt als het bijhouden van inhoud is ingeschakeld en de YouTube-speler op de pagina aanwezig is | - |

