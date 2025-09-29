---
title: Omleiden
description: Voer Redirect API RTP uit om gesegmenteerde bezoekers naar gerichte URLs te verzenden gebruikend gebieden zoals ABM, organisatie, plaats, en segmenten, met voorbeelden en uiteinden.
feature: Javascript
exl-id: bbf91245-42e5-47ae-a561-e522cc65ff49
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 1%

---

# Omleiden

Met de RTP Redirect-API kunt u gesegmenteerde doelgroepen omleiden naar een doel-URL.

- U moet een klant van Personalization van het Web worden en de [ markering hebben RTP die ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) op uw plaats wordt opgesteld voorafgaand aan het gebruiken van de Context API van de Gebruiker.
- RTP ondersteunt geen accountgebaseerde marketing met benoemde accountlijsten. ABM-lijsten en -code hebben alleen betrekking op de geüploade accountlijsten (CSV-bestanden) die in RTP worden beheerd.

## Gebruik

`rtp('send' , 'redirect' , 'field_name' , [ 'values_array' , '...' , '...' ] , 'www.redirect_url.com' , true/false )`

| Parameter | Optioneel/vereist | Type | Beschrijving |
|---------------------------|-------------------|---------|-----------------------------|
| &#39;send&#39; | Vereist | String | Methode, actie. |
| &#39;redirect&#39; | Vereist | String | Naam van methode. |
| field_name | Vereist | String | Veldnaam die moet worden vergeleken met. Voorbeeld: &#39;abm.name&#39; (zie hieronder). |
| values_array | Vereist | Array | Lijst met waarden die met het veld moeten worden vergeleken (niet hoofdlettergevoelig). |
| redirect_url | Vereist | String | Geef de URL op om bezoekers die aan de voorwaarde voldoen, om te leiden. |
| redirect_matching_bezoekers | Optioneel | Boolean | Indien waar (true), worden overeenkomende bezoekers omgeleid. Indien onwaar, worden bezoekers zonder voorwaarden omgeleid. Standaard: true. |

Organisatie, Industrie, de Lijsten ABM, Plaats, ISP, Gelijke Segmenten

| Voorwaarde | Gegevenshiërarchie | Voorbeeld |
|-------------------------------------------------|----------------------|------------------------------------------------------------------------------------------------------------------|
| Overeenkomende segmenten (werkt alleen na eerste klik) | matchedSegments.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchingSegments.name&#39; , [&#39;Fortune 1,000&#39; , &#39;Enterprise&#39;] , &#39;<http://www.marketo.com>&#39;); |
| Overeenkomende segmenten (werkt alleen na eerste klik) | matchedSegments.id | rtp ( &quot;verzend&quot;, &quot;redirect&quot;, &quot;matchingSegments.id&quot;, [ 106, 107, 190 ] , &quot;<http://www.marketo.com>&quot;); |
| ABM-lijsten | abm.name | rtp( &#39;send&#39;, &#39;redirect&#39;, &#39;abm.name&#39;, [&#39;top_key_accounts&#39;, &#39;active_customer&#39;] , &#39;<http://www.marketo.com>&#39;); |
| ABM-lijsten | abm.code | rtp ( &quot;verzend&quot;, &quot;redirect&quot;, &quot;abm.code&quot;, [ 13, 15 ] , &quot;<http://www.marketo.com>&quot;); |
| Organisaties | org | rtp ( &quot;send&quot;, &quot;redirect&quot;, &quot;org&quot;, [ &quot;ebay&quot;], &quot;<http://www.marketo.com>&quot;); |
| Locatie | location.country | rtp( &#39;send&#39;, &#39;redirect&#39;, &#39;location.country&#39;, [&#39;United States&#39; ] , &#39;<http://www.marketo.com>&#39;); |
| Locatie | location.state | rtp( &#39;send&#39;, &#39;redirect&#39;, &#39;location.state&#39;, [&#39;ca&#39;], &#39;<http://www.marketo.com>&#39;); |
| Locatie | location.city | rtp ( &quot;send&quot;, &quot;redirect&quot;, &quot;location.city&quot;, [ &quot;San Mateo&quot;], &quot;<http://www.marketo.com>&quot;); |
| Industrie | industrieën | rtp ( &quot;send&quot;, &quot;redirect&quot;, &quot;industries&quot;, [ &quot;Education&quot;], &quot;<http://www.marketo.com>&quot;); |
| ISP | isp | rtp ( &quot;send&quot;, &quot;redirect&quot;, isp, [ &quot;False&quot;], &quot;<http://www.marketo.com>&quot;); |

## Notities

- Als de omleidingsregel/-voorwaarde is gebaseerd op Firmographics (bedrijf, industrie, locatie), kunt u de omleidingscode invoegen vóór rtp(&#39;send&#39;, &#39;view&#39;) en de rtp(&#39;get&#39;,&#39;campagne&#39;) om de latentie te verminderen.
- Omleiden via JavaScript is een omleiding aan de browserzijde en is afhankelijk van het laden en optimaliseren van de website om de maximale snelheid te bereiken.
- U kunt het beste de omleidingscode direct na de tag rtp instellen en deze in de koptekst plaatsen.
- Zorg ervoor u geen autoomleiding in werking stelt (er is een veiligheidsnet in rtp om cyclische omleidingsvraag te blokkeren).

```html
<!DOCTYPE html>
<html lang="en-US">
<head>
<!-- RTP tag -->
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?rh='+c.location.hostname+'&aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//xyz.marketo.com/rtp-api/v1/rtp.js","xyz");

// START REDIRECT EXAMPLE
//   - Using a helper redirect function
//   - Redirect based on named account
rtp('send','redirect','org', ['microsoft'],'http://www.marketo.com');

// Redirect based on named account list (ABM)
rtp('send','redirect','abm.name', {
    // Redirect visitors that match 'first_abm' list to www.marketo.com
    'http://www.marketo.com' : ['first_abm'],
    // Redirect visitors that match 'second_abm' list to blog.marketo.com
    'http://blog.marketo.com' : ['second_abm']
});
// END REDIRECT EXAMPLE
rtp('send','view');
rtp('get','campaign');
</script>
<!-- End of RTP tag -->
```

## Hoe te om Getraceerde Bezoekers om te leiden

1. Een parameter toevoegen aan het einde van de doel-URL: &lt;www.marketo.com?rtp=redirect>
1. Creeer een segment genoemd - &quot;opnieuw geleid door RTP&quot;
1. Gebruik de parameter &#39;Specifieke pagina&#39;s&#39; als doel voor bezoekers die pagina&#39;s weergeven met de onderstaande parameter.

![ het volgen-redirected-vistors ](assets/tracking-redirected-vistors.png)

## Meer dan één voorwaarde definiëren met verschillende doel-URL&#39;s

De omleidingsvraag steunt veelvoudige vraag. Hierdoor kunt u omleiden met meerdere velden en complexe omstandigheden met verschillende URL&#39;s en waarden creëren.

### Gebruik

`rtp('send', 'redirect', field_name, url_values_map);`

| Parameter | Optioneel/vereist | Type | Beschrijving |
|---|---|---|---|
| &#39;send&#39; | Vereist | String | Methode, actie. |
| &#39;redirect&#39; | Vereist | String | Naam van methode. |
| field_name | Vereist | String | Veldnaam die moet worden vergeleken met. Voorbeeld: &#39;abm.name&#39; (zie hierboven). |
| url_values_map | Vereist | Object | Kaart tussen omleiding URL en lijst van waarden. Voorbeeld:&lbrace;&#39;<http://marketo.com>&#39; : [&#39;first_abm&#39;, &#39;second_abm&#39;] |

#### Voorbeeld

```javascript
rtp('send','redirect','abm.name', {
    // Redirect visitors that match 'first_abm' list to www.marketo.com
    'http://www.marketo.com' : ['first_abm'],
    // Redirect visitors that match 'second_abm' list to blog.marketo.com
    'http://blog.marketo.com' : ['second_abm']
});
rtp('send','redirect','org', {
    // Redirect visitors from 'Microsoft' to www.marketo.com/enterprise
    'http://www.marketo.com/enterprise' : ['microsoft']
});
```
