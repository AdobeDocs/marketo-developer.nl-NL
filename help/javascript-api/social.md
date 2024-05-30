---
title: Social
description: "Sociaal"
feature: Social, Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '776'
ht-degree: 0%

---


# Social

[Marketo Social Marketing](https://business.adobe.com/products/marketo/social-marketing.html) Hiermee kunnen marketers sociale widgets insluiten in websites en pagina&#39;s landen. Sociale widgets zijn onder andere opiniepeilingen, knoppen voor sociaal delen, video&#39;s, zweepslagen en speciale acties, zoals de verwijzingsaanbiedingen.

## Voorbeeld van ingesloten deelwidget

```html
<!-- Marketo Widget Loader Script --> 

<script type="text/javascript" src="//b2c-mlm.marketo.com/jsloader/271d8232-1500-4305-b7ed-05d451b9ee0c/loader.php.js">
</script>

 <!-- The Location of the Social Widget --> 

<divclass='cf_widgetloader cf_w_245d8f3c0955454cbd26abc39d0d874c'="" options="{&quot;outerHeight&quot;:400, &quot;outerWidth&quot;:600}">
</divclass='cf_widgetloader'>
```

![social-share-widget](assets/social-share-widget.png)

Er zijn twee basismethoden voor het aanpassen van een sociale widget:

1. Gebruikend de normale UI van het product en het vastmaken van gebeurtenisluisteraars om worden geïnformeerd wanneer bepaalde acties in UI zijn gebeurd om extra bedrijfslogica uit te voeren.
1. U vervangt de normale gebruikersinterface van het product door een aangepaste gebruikersinterface en activeert desgewenst de pop-upstadia van de gebruikersinterface.

## Gebeurtenissen koppelen aan de normale gebruikersinterface

Er zijn twee manieren om een abonnement te nemen op gebeurtenissen in de CF JavaScript-bibliotheek, globaal of voor één widget. Gebeurtenissen worden hieronder beschreven in de gebeurtenissentabel.

### Global Event Subscription

```html
<script>
cf_scripts.afterload(function(){
    CF.events.listen("event_name_here",
        function(event, arg1){
            //Your code to do something on the event goes here.
            //It will be fired whenever ANY widget fires the event "event_name_here".
        }
    );
});
</script>
```

### Per-Widget-gebeurtenisabonnement

```html
<script>
cf_scripts.afterload(function(){
    CF.widget.listen("widget_name_here", "event_name_here",
        function(event, arg1){
            //Your code to do something on the event goes here.
            //It will be fired whenever the widget named "widget_name_here" fires the event "event_name_here".
        }
    );
});
</script>
```

## Een voorbeeld

In dit voorbeeld wordt een eerder verborgen element getoond met de id &quot;signedUp&quot; nadat een gebruiker een aanbiedingsinschrijving voor een widget met de naam &quot;referral_SignUp&quot; heeft voltooid.

```html
<div id='signedUp'style='display:none; color:green;'>This is a custom message to let you know that you signed up!</div>
<div class='cf_widgetLoader cf_w_referral_SignUp'></div>

<script>
    cf_scripts.afterload(function(){
        CF.widget.listen("referral_SignUp", "offer_enrolled", function(){
        cf_jq("#signedUp").show();
    });
});
</script>
```

## Tabel met basisgebeurtenissen

| Gebeurtenisnaam | Beschrijving | Widgets die deze gebeurtenis gebruiken | Ondersteunde argumenten (doorgegeven aan callback-functie voor gebeurtenissen) |
| --- | --- | --- | --- | 
| share_sent | Wordt geactiveerd telkens wanneer een aanvraag voor delen naar de server wordt verzonden voor verwerking | Alle widgets die de mogelijkheid hebben te delen | 1.&quot;share_sent&quot; (String)<br>2. Verzonden parameters (Object) |
| share_success | Wordt geactiveerd wanneer de aanvraag voor delen is verwerkt. | Alle widgets die de mogelijkheid hebben te delen. | 1.&quot;share_success&quot; (String)<br>2. Reactieobject delen, met bericht verzonden en verkorte URL (Object) |
| voice_success | Wordt geactiveerd wanneer een gebruiker met succes heeft gestemd in een opiniepeiling. | Opiniepeiling, VS, Stemwidgets | 1. &quot;voice_success&quot; (String)<br>2. Punt waarvoor gestemd is, met inbegrip van titel, beschrijving, entiteitsidentificatie (Object) |
| voorstel_ingeschreven | Wordt geactiveerd wanneer een gebruiker zich met succes heeft ingeschreven voor een aanbieding | Alle aanbiedingswidgets | 1.&quot;aanbieding_ingeschreven&quot; (tekenreeks)<br>2. Gewijzigde gebruikerseigenschappen (Object)<br>3. Gewijzigde gebruikerskenmerken (Object) |
| profile_saved | Wordt geactiveerd wanneer een gebruiker zijn profiel heeft bijgewerkt via het vastleggen van profielen | Alle niet-aangeboden widgets waarvoor het vastleggen van profielen is ingeschakeld | 1.&quot;profile_saved&quot; (String)<br>2. Gewijzigde gebruikerseigenschappen (Object)<br>3. Gewijzigde gebruikerskenmerken (Object) |
| video_loaded | Wordt geactiveerd wanneer een ingesloten video volledig is geladen en geïnitialiseerd. | VideoShare-widget | 1. &quot;video_loaded&quot; (String) 2. &quot;.cf_videoshare_wrap&quot; Element dat de video bevat (jQuery Object) |

## De interface vervangen door een aangepaste interface

Als u de interface wilt vervangen door een aangepaste UI, moet u eerst de normale UI uitschakelen. Dit gebeurt door de optie in te stellen _popupUIOnly_ tot _true_. Als deze optie is ingesteld, wordt de standaard-UI niet gerenderd bij het laden van de pagina. De widget haalt de gegevens op en wacht tot u een van de pop-upfasen start door de _CF.widget.activate_ en biedt opties voor wat het moet doen.

Hier is een voorbeeld van het creëren van een douaneknoop die de verwijzingsaanbieding inschrijvingsstroom voor een verwijzingsaanbieding genoemde widget lanceert _verwijzing_SignUp_.

```html
<button id="myNewSignUpButton">My newSign Up button</button>

<!-- Turn off the defaultreferral offer UI by setting popupUIOnly to true-->
<div class="cf_widgetLoader cf_w_referral_SignUp" options="{popupUIOnly:true}"></div>

<script>
cf_scripts.afterload(function($, CF){
    // After the cf script library has loaded, find the button with
    // id="myNewSignUpButton", and attach a click listener to it.
    $("#myNewSignUpButton").click(function(){
        // When it is clicked, activate the popup widget flow for the referral,
        // asking it to point to the clicked button.
        CF.widget.activate("referral_SignUp", {pointTo:$(this)});
    });
});
</script>
```

Omdat het toevoegen van klikmanagers gemeenschappelijk is, is er een kortere wegmethode om hen toe te voegen. Het volgende is functioneel equivalent aan het voorafgaande voorbeeld.

```html
<button id="myNewSignUpButton">My newSign Up button</button>
<div class="cf_widgetLoader cf_w_referral_SignUp" options="{popupUIOnly:true}"></div>

<script>
cf_scripts.afterload(function($, CF){
    // Use the addClickActivate convenience method, which will
    // automatically make the popup point at the clicked item with id myNewSignUpButton.
    CF.widget.addClickActivate("#myNewSignUpButton", "referral_SignUp", {});
});
</script>
```

## Widget UI-gegevens ophalen om in de vervangende gebruikersinterface te plaatsen

Als u gegevens over de widget nodig hebt om de vervangende UI te tekenen, kunt u de gegevens ophalen van de speciale gebeurtenis _ui_data_. U kunt naar deze gebeurtenis luisteren met de normale `CF.widget.listen` Als u dit doet, kan dit echter een potentiële rassenvoorwaarde veroorzaken waarbij uw gebeurtenislistener wordt toegevoegd nadat de widget al de gebeurtenis_ui_data_ heeft geactiveerd, zodat u nooit gegevens ontvangt. Om dit ras te vermijden, gebruik `CF.widget.uiData_ method instead, which will give you the most recent available _ui_data_, and listen for all future updates as well. The _ui_data` gebeurtenis wordt geactiveerd wanneer een actie is uitgevoerd die ertoe zou hebben geleid dat de standaardinterface van de widget opnieuw zou zijn getekend, zelfs als u die ui hebt uitgeschakeld `popupUIOnly` -optie.

Een voorbeeld dat `uiData` functie om het aantal items weer te geven dat een gebruiker heeft voor een transporten met een widgetnaam _sweeps_Sweepstakes_.

```html
<span>You have <span id="entryCount">?</span> entries.</span>

<div class="cf_widgetLoader cf_w_sweeps_Sweepstakes"></div>

<button id='myNewSweepsButton'>New Sweeps Up Button!</button>

<script>
cf_scripts.afterload(function($, CF){
    CF.widget.uiData("sweeps_Sweepstakes", function(uiData){
        if(uiData.user && uiData.userStatus && uiData.userEntries){
            $("entryCount").html(""+ uiData.userEntries);
        }
        else{
            $("entryCount").html("0");
        }
    });
});
</script>
```

## Referral-aanbod voor aanmelding UI-gegevensreferentie

| Type | Beschrijving |
|---------------|----------------------------------------------------|
| date | Datum en waarde van de notatie &quot;jjjj-MM-dd&quot; |
| getal | Een geheel getal of een drijvende-kommagetal |
| rijke tekst | Een HTML-tekenreeks |
| score | Een 32-bits geheel getal met teken |
| sfdc-campagne | Gebruikt in de integratie van Salesforce-campagnebeheer |
| text | Een tekstreeks |

## Referral Offset TrackProgress UI-gegevensverwijzing

| Type | Beschrijving |
|---------------|----------------------------------------------------|
| date | Datum en waarde van de notatie &quot;jjjj-MM-dd&quot; |
| getal | Een geheel getal of een drijvende-kommagetal |
| rijke tekst | Een HTML-tekenreeks |
| score | Een 32-bits geheel getal met teken |
| sfdc-campagne | Gebruikt in de integratie van Salesforce-campagnebeheer |
| text | Een tekstreeks |

## Sweepstakes UI Data Reference (voor Sociale Campagne Sweepstakes, niet LM Sweepstakes)

| Type | Beschrijving |
|---------------|----------------------------------------------------|
| date | Datum en waarde van de notatie &quot;jjjj-MM-dd&quot; |
| getal | Een geheel getal of een drijvende-kommagetal |
| rijke tekst | Een HTML-tekenreeks |
| score | Een 32-bits geheel getal met teken |
| sfdc-campagne | Gebruikt in de integratie van Salesforce-campagnebeheer |
| text | Een tekstreeks |

## Referentie voor gegevens voor sociaal aanmelden (widget Formuliervulling)

| Type | Beschrijving |
|---------------|----------------------------------------------------|
| date | Datum en waarde van de notatie &quot;jjjj-MM-dd&quot; |
| getal | Een geheel getal of een drijvende-kommagetal |
| rijke tekst | Een HTML-tekenreeks |
| score | Een 32-bits geheel getal met teken |
| sfdc-campagne | Gebruikt in de integratie van Salesforce-campagnebeheer |
| text | Een tekstreeks |

```javascript
{
"alt_id": "http://www.facebook.com/profile.php?id=1526228678",
"provider_name": "facebook",
"default_photo_url": "https://graph.facebook.com/1526228678/picture?type=large",
"email": "ian.b.taylor@gmail.com",
"verified_email": "ian.b.taylor@gmail.com",
"gender": "male",
"preferred_user_name": "IanTaylor",
"display_name": "Ian Taylor",
"birth_date": 343954800000,
"first_name": "Ian",
"last_name": "Taylor",
"city": null,
"state": null,
"region": null,
"postal_code": null,
"country": null,
"time_zone": null,
"connection_count": 0,
"credentials": {
"uid": "1526228678",
"scopes": "publish_actions",
"expires": "1371994082",
"accessToken": "BAAGFJ0KUFpcBABuNMptmYY...",
"type": "Facebook"
},
"about_me": null,
"cur_pos_title": "Senior Staff Engineer",
"phone_number": null,
"company": "Marketo",
"cur_pos_start_date": 1333231200000,
"cur_pos_summary": null
}
```
