---
title: "Redirect"
description: "Redirect"
feature: Javascript
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 0%

---


# Omleiden

Met de RTP Redirect-API kunt u gesegmenteerde doelgroepen omleiden naar een doel-URL.

- U moet een klant van de Personalisatie van het Web worden en hebben [RTP-tag geïmplementeerd](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) op uw site voordat u de Context-API van de gebruiker gebruikt.
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
| Overeenkomende segmenten (werkt alleen na eerste klik) | matchedSegments.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchSegments.name&#39; , [&quot;Fortune 1.000&quot; , &quot;Enterprise&quot;] , &quot;http://www.marketo.com&#39;); |
| Overeenkomende segmenten (werkt alleen na eerste klik) | matchedSegments.id | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchSegments.id&#39; , [106, 107, 190] , &quot;http://www.marketo.com&#39;); |
| ABM-lijsten | abm.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;abm.name&#39; , [&#39;top_key_accounts&#39;, &#39;active_customer&#39;] , &quot;http://www.marketo.com&#39;); |
| ABM-lijsten | abm.code | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;abm.code&#39; , [13.] , &quot;http://www.marketo.com&#39;); |
| Organisaties | org | rtp(&#39;send&#39;, &#39;redirect&#39;, &#39;org&#39;, [&#39;ebay&#39;], &quot;http://www.marketo.com&#39;); |
| Locatie | location.country | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.country&#39; , [&#39;Verenigde Staten&#39;], &quot;http://www.marketo.com&#39;); |
| Locatie | location.state | rtp( &#39;send&#39;, &#39;redirect&#39;, &#39;location.state&#39;, [&#39;ca&#39;], &quot;http://www.marketo.com&#39;); |
| Locatie | location.city | rtp(&#39;send&#39;, &#39;redirect&#39;, &#39;location.city&#39;, [&#39;San Mateo&#39;], &quot;http://www.marketo.com&#39;); |
| Industrie | industrieën | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;industries&#39; , [&quot;Onderwijs&quot;], &quot;http://www.marketo.com&#39;); |
| ISP | isp | rtp( &#39;send&#39;, &#39;redirect&#39; , isp , [&#39;False&#39;], &quot;http://www.marketo.com&#39;); |


## Notities

- Als de omleidingsregel/-voorwaarde is gebaseerd op Firmographics (bedrijf, industrie, locatie), kunt u de omleidingscode invoegen vóór rtp(&#39;send&#39;, &#39;view&#39;) en de rtp(&#39;get&#39;,&#39;campagne&#39;) om de latentie te verminderen.
- Omleiding via JavaScript is een omleiding aan de browserzijde en is afhankelijk van het laden en optimaliseren van de website om de maximale snelheid te bereiken.
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

1. Een parameter toevoegen aan het einde van de doel-URL: www.marketo.com?rtp=redirect
1. Creeer een segment genoemd - &quot;opnieuw geleid door RTP&quot;
1. Gebruik de parameter &#39;Specifieke pagina&#39;s&#39; als doel voor bezoekers die pagina&#39;s weergeven met de onderstaande parameter.

![tracking-redirected-vistors](assets/tracking-redirected-vistors.png)

## Meer dan één voorwaarde definiëren met verschillende doel-URL&#39;s

De omleidingsvraag steunt veelvoudige vraag. Hierdoor kunt u omleiden met meerdere velden en complexe omstandigheden met verschillende URL&#39;s en waarden creëren.

### Gebruik

`rtp('send', 'redirect', field_name, url_values_map);`

| Parameter | Optioneel/vereist | Type | Beschrijving |
|---|---|---|---|
| &#39;send&#39; | Vereist | String | Methode, actie. |
| &#39;redirect&#39; | Vereist | String | Naam van methode. |
| field_name | Vereist | String | Veldnaam die moet worden vergeleken met. Voorbeeld: &#39;abm.name&#39; (zie hierboven). |
| url_values_map | Vereist | Object | Kaart tussen omleiding URL en lijst van waarden. Voorbeeld:{&#39;http://marketo.com&#39;: [&#39;first_abm&#39;, &#39;second_abm&#39;]} |


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
