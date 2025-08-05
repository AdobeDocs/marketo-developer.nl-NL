---
title: Web Personalization
description: Web Personalization
feature: Web Personalization, Javascript
exl-id: b2c26b28-e9bf-4faf-8b6e-c102f41aeaa1
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 1%

---

# Web Personalization

De Web Personalization JavaScript API breidt het geautomatiseerde verpersoonlijkingsvermogen van het platform uit. Zo kunt u gebeurtenissen bijhouden en een webpagina dynamisch aanpassen. De extra mogelijkheden: [ Gebeurtenissen van de Gegevens van de Douane ](custom-data-events.md), [ Dynamische Inhoud ](web-personalization.md), [ krijgen de Gegevens van de Bezoeker ](get-visitor-data.md), [ uitsluiten markering voor Specifieke Bots ](#exclude_tag_for_specific_bots).

- U moet een klant van Personalization van het Web worden en de [ markering hebben RTP die ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) op uw plaats wordt opgesteld alvorens de Context API van de Gebruiker te gebruiken.
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

Als u specifieke browsers wilt uitsluiten van het verzenden van gegevens naar het Web Personalization-platform (in het geval van bepaalde bots), voegt u de volgende IF-instructie toe aan het tagscript.

In het onderstaande codevoorbeeld wordt &quot;Googlebot|msnbot&quot; gebruikt als beide voorbeelden om activiteiten van Web Personalization uit te sluiten.

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

## JavaScript Calls Verklaard

Beschrijving van JavaScript die aan een website wordt toegevoegd wanneer Web Personalization en Predictive Content worden gebruikt.

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
| ga-integration-2.0.1.js | Wordt gebruikt als de integratie Google Analytics/Facebook/SiteCatalyst is ingeschakeld | Onder zeggenschap van Marketo |
| insightera-bar-2.1.js | Wordt gebruikt als de aanbevolen balk voor voorspellende inhoud is ingeschakeld | Onder zeggenschap van Marketo |
| froogaloop2.min.js | Wordt gebruikt als het bijhouden van inhoud is ingeschakeld en de Vimeo-speler op de pagina aanwezig is | - |
| iframe-api-v1.js | Wordt gebruikt als het bijhouden van inhoud is ingeschakeld en de YouTube-speler op de pagina aanwezig is | - |
