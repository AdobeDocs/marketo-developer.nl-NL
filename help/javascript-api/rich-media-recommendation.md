---
title: Aanbevelingen voor rijke media
description: Aanbevelingen voor rijke media
feature: Javascript
exl-id: ee92e46d-e529-40a2-a0d0-ee233916f004
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 1%

---

# Aanbevelingen voor rijke media

De volgende tags en API-aanroepen moeten zijn ingesteld op de pagina die u wilt weergeven als sjabloon voor veelzijdige mediaconitentie.

1. In de paginakoptekst
   1. De RTP-tag geïnstalleerd laten
   1. Voeg de GET-aanroep aan de pagina toe om de aanbevelingen in te vullen
   1. Voeg de vraag van de REEKS toe om het malplaatje te vormen
1. In de pagina
   1. Plaats de sjabloontag (klasse div) op de locatie waar u de sjabloon wilt weergeven

Meer informatie is beschikbaar [ hier ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/predictive-content/enabling-predictive-content/enable-predictive-content-for-web-rich-media).

## Sjabloonlabel

| Kenmerk | Optioneel/vereist | Beschrijving |
|---|---|---|
| class | Vereist | Specificeer dat dit div HTML element RTP aanbeveling div is. |
| data-rtp-template-id | Vereist | De sjabloon-id. Dit bepaalt de groepering van uw aanbeveling. Gebruik &#39;template1&#39; voor horizontale uitlijning, &#39;template2&#39; voor verticale uitlijning of &#39;template3&#39; voor verticale uitlijning die alleen titel en beschrijving bevat. Het script voert de overeenkomstige sjabloon in deze `div.Permissible` waarden in: template1, template2, template3. |

### Voorbeelden

Gebruik &#39;&#39;template1&#39;&#39; om uw aanbevelingen in horizontale uitlijning weer te geven.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
```

Gebruik &quot;template2&quot; om uw aanbevelingen in verticale uitlijning weer te geven.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template2"></div>
```

Als u uw aanbevelingen verticaal wilt uitlijnen met alleen titel en beschrijving, gebruikt u &quot;template3&quot;.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template3"></div>
```

Zie screenshots van malplaatjeuitlijningen [ hier ](#example_of_rich_media_recommendation_template_1).

## Aanbeveling vullen

Met deze methode worden alle rich media `<divs>` op de pagina gevuld met aanbevelingen.

### Gebruik

`rtp('get', 'rcmd', 'richmedia');`

| Parameter | Optioneel/vereist | Type | Beschrijving |
|---|---|---|---|
| &#39;get&#39; | Vereist | String | Methode, actie. |
| &#39;rcmd&#39; | Vereist | String | Naam van methode. |
| &#39;richmedia&#39; | Vereist | String | Naam submethode. |

## Sjabloonconfiguratie wijzigen

Met deze methode wijzigt u de standaardconfiguratie voor een sjabloon.

Opmerking: wanneer u deze methode gebruikt, moet deze worden aangeroepen voordat rtp(&#39;get&#39;, &#39;rcmd&#39;, &#39;richmedia&#39;) wordt aangeroepen.

### Gebruik

`rtp('set', 'rcmd', 'richmedia', 'template_id', conf_obj);`

| Parameter | Optioneel/vereist | Type | Beschrijving |
|---|---|---|---|
| &#39;set&#39; | Vereist | String | Methode, actie. |
| &#39;rcmd&#39; | Vereist | String | Naam van methode. |
| &#39;richmedia&#39; | Vereist | String | Naam submethode. |
| template_id | Optioneel | String | De sjabloon-id voor configuratiewijzigingen. Gebruik deze optie als u de instellingen voor slechts één sjabloon wilt wijzigen. |
| conf_obj | Vereist | Object | De nieuwe configuratie. Het object bevat alle configuraties als sleutel-/waardepaar. |

### Voorbeelden

Dit codefragment wijzigt de titeltekst voor een sjabloon.

```javascript
rtp("set", "rcmd", "richmedia","template1",
    {
        "rcmd.title.text": "RECOMMENDED CONTENT"
    }
);
```

Dit codefragment toont het plaatsen van categorieën met veelvoudige configuraties voor een malplaatje.

```javascript
rtp("set", "rcmd", "richmedia",
    {
        "template1":
        {
            "rcmd.title.text": "RECOMMENDED CONTENT",
            "rcmd.general.font.family": "arial",
            "category":
            [
                "webinar",
                "blog posts",
                "pricing_page_category",
                "product_a_category"
            ]
        }
    }
);
```

OPMERKING: gebruik &quot;categorie&quot; om inhoud te filteren die wordt weergegeven in het resultaat van aanbevelingen voor voorspellende inhoud. Als u voorspellende inhoud wilt toepassen op alle ingeschakelde inhoudsonderdelen, laat u de &quot;categorie&quot; leeg. Als u slechts specifieke inhoud voor de output in het Rich Media malplaatje wilt adviseren, voeg een categorie voor de inhoud in de Vastgestelde inhoudspagina toe en associeer die categorie binnen de code van het aanbevelingsmalplaatje. De relevante inhoud categoriseren op basis van secties op uw website (producten of oplossingen).

Dit codefragment toont het plaatsen van veelvoudige malplaatjeconfiguraties voor een malplaatje.

```javascript
rtp("set", "rcmd", "richmedia",
    {
        "template1":
        {
            "rcmd.title.text": "RECOMMENDED CONTENT",
            "rcmd.general.font.family": "arial"
        }
    }
);
```

#### Configuratieeigenschappen

| Configuratie | Voorbeeld | Beschrijving |
|---|---|---|
| rcmd.general.font.family | &quot;rcmd.general.font.family&quot; : &quot;arial&quot; | Wijzigt de lettertypefamilie voor alle tekst in de sjabloon. Deze eigenschap ondersteunt alle CSS-waarden per browsertype. U kunt een aangepaste lettertypefamilie gebruiken als deze op de pagina aanwezig is. |
| rcmd.content.background.color | &quot;rcmd.content.background.color&quot; : &quot;black&quot; | Hiermee wijzigt u de achtergrondkleur van de binnenvakken van de sjabloon. Deze eigenschap ondersteunt alle CSS-waarden per browsertype. |
| rcmd.title.text | &quot;rcmd.title.text&quot; : &quot;AANBEVOLEN INHOUD&quot; | Hiermee wijzigt u de sjabloontitel. |
| rcmd.title.background.color | &quot;rcmd.title.background.color&quot; : &quot;blue&quot; | Hiermee wijzigt u de achtergrondkleur van het titelvak. Deze eigenschap ondersteunt alle CSS-kleurwaarden (kleurnaam, rgb, ...) |
| rcmd.title.font.size | &quot;rcmd.title.font.size&quot; : &quot;26px&quot; | Hiermee wijzigt u de tekengrootte van de titel. De eigenschap ondersteunt alle mogelijke CSS-waarden voor tekengrootten (px, em, ...) |
| rcmd.title.font.color | &quot;rcmd.title.font.color&quot; : &quot;white&quot; | Hiermee wijzigt u de kleur van het titellettertype. Deze eigenschap ondersteunt alle waarden voor fontkleuren (rgb, hex, ...) |
| rcmd.description.font.color | &quot;rcmd.description.font.color&quot; : &quot;white&quot; | Hiermee wijzigt u de fontkleur van de beschrijving. Deze eigenschap ondersteunt alle waarden voor fontkleuren (rgb, hex, ...) |
| rcmd.cta.background.color | &quot;rcmd.cta.background.color&quot; : &quot;green&quot; | Hiermee wijzigt u de achtergrondkleur van de knop. Deze eigenschap ondersteunt alle CSS-kleurwaarden (kleurnaam, rgb, ...) |
| rcmd.cta.font.color | &quot;rcmd.cta.font.color&quot; : &quot;rgb(90, 84, 164)&quot; | Hiermee wijzigt u de fontkleur van de knop. Deze eigenschap ondersteunt alle waarden voor fontkleuren (rgb, hex, ...) |
| rcmd.cta.text | &quot;rcmd.cta.text&quot; : &quot;Push&quot; | Hiermee wijzigt u de knoptekst. De tekst is hetzelfde voor alle knoppen. |
| categorie | &quot;category&quot; : [ &quot;one category&quot;] | Wijzigt de aanbevolen categorie die door deze sjabloon wordt ondersteund. Het malplaatje toont slechts aanbevelingen met één van de categorieën die door deze configuratie worden geplaatst. |

Opmerking: de configuratieondersteuning kan per sjabloon worden gewijzigd.

#### Eenvoudig voorbeeld

Dit voorbeeld heeft één sjabloon met drie aanbevelingen. Kopieer dit voorbeeld naar een HTML-pagina en vervang de RTP-tag door uw tag.

```html
<!DOCTYPE>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>RTP recommendation</title>
<!-- RTP tag -->
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i,e){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;c[a].e=e;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//example.rtp.com/rtp-api/v1/rtp.js","account_id");

// Send page view (required by  the recommendation)
rtp('send','view');
// Populate recommendation
rtp('get','rcmd', 'richmedia');
</script>
<!-- End of RTP tag -->
</head>
<body>
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
</body>
</html>
```

#### Geavanceerd voorbeeld

Dit voorbeeld heeft één sjabloon met drie aanbevelingen. De sjabloontitel is &quot;AANBEVOLEN INHOUD&quot; en de knoptekst is &quot;Meer informatie&quot;. Kopieer dit voorbeeld naar een HTML-pagina en vervang de RTP-tag door uw tag.

```html
<!DOCTYPE>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>RTP recommendation</title>
<!-- RTP tag -->
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i,e){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;c[a].e=e;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//example.rtp.com/rtp-api/v1/rtp.js","account_id");

// Send page view (required by  the recommendation)
rtp('send','view');
// Populate the recommendation zone
rtp('get', 'campaign',true);
// Change template configuration
rtp('set', 'rcmd', 'richmedia',
    {
        template1 :
        {
            "rcmd.title.text" : "RECOMMENDED CONTENT",
            "rcmd.cta.text" : "Read More"
        }
    }
);
// Populate recommendation
rtp('get','rcmd', 'richmedia');
</script>
<!-- End of RTP tag -->
</head>
<body>
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
</body>
</html>
```

#### Voorbeeld van Sjabloon 1 voor veelzijdige mediaconclusies

**Naam**: template1 **Beschrijving**: Horizontale inhoud met inbegrip van beeld, titel, en beschrijving en de knoop van call to action.

![ Rich Media malplaatje ](assets/rich-media-template1.png)

#### Voorbeeld van sjabloon #2 voor rich Media Recommendation

**Naam**: template2 **Beschrijving**: Verticale inhoud met inbegrip van beeld, titel, en beschrijving en de knoop van call to action.

![ Rich Media malplaatje ](assets/rich-media-template2.png)

#### Voorbeeld van sjabloon 3 voor rich Media Recommendation

**Naam**: template3 **Beschrijving**: Verticale inhoud met inbegrip van slechts titel en beschrijving. Bij de muisaanwijzer verandert de koptekst van kleur en is deze gekoppeld aan de URL van de inhoud. Beschrijving is ook gekoppeld aan inhoud zonder kleurwijziging. ![ Rich Media malplaatje ](assets/rich-media-template3.png)
