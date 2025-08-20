---
title: Marketo-objecten
feature: Email Programs
description: Een overzicht van het gebruik van Marketo-objecten met snelheidsscripts
source-git-commit: 3ccb27a0d184e0c1314979d404022bc4e0794f7b
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# Marketo-objecten

De implementatie van Marketo-snelheid kan worden uitgevoerd op gegevens uit verschillende bronnen in Marketo: leads, Opportunity, Custom Objects, Mobile App, Mobile App Installation.

## Velden laden

Als u een veld voor gebruik in een script wilt laden, moet dat veld worden gecontroleerd onder de corresponderende lijst in de scripttakeneditor.

Als u geen veld laadt en er in het script naar wordt verwezen, mislukt de uitvoering van het script tijdens de runtime. U kunt velden van het veldmenu naar het script slepen. Hierdoor kunnen ze worden geladen en wordt een verwijzing naar het veld bij de cursor toegevoegd.

## Lijst met mogelijkheden en aangepaste objecten

Wanneer het terugwinnen van Kansen of de Voorwerpen van de Douane, slechts worden de 10 onlangs bijgewerkte voorwerpen van een type geladen. Het aantal beschikbare aangepaste objecten kan worden verhoogd door de hier beschreven stappen te volgen. Deze worden gegeven als een lijst, met de naam `<objectName>List` en worden bevolen van het laatst bijgewerkte verslag. Als u dus toegang wilt krijgen tot het veld Bedrag bij de gelegenheid die het laatst is bijgewerkt, gebruikt u het volgende:

`${OpportunityList.get(0).Amount}`

In dit voorbeeld, verwijst u naar het voorwerp OpportunityList, gebruikt get methode om tot het verslag toegang te hebben dat bij 0 wordt geïndexeerd, en dan het bezit van het Bedrag van het teruggekeerde voorwerp terugwinnen. Als u een veld van een Opportunity of Aangepast object naar de editor sleept, wordt het veld automatisch opgehaald uit de record die is geïndexeerd op 0.

## Aangepaste SFDC-objectrelaties

Een aangepast SFDC-object kan alleen worden gebruikt als het een relatie heeft met de Marketo-lead. Objecten worden vaak gekoppeld via zowel de contactpersoon als de account. Het is daarom belangrijk dat objecten alleen worden gesynchroniseerd met Marketo als de contact-/hoofdrelatie is ingeschakeld.

## Objecten activeren

Wanneer een campagne wordt geactiveerd via de methode Toevoegen aan opportunity, Opportunity wordt bijgewerkt of Toegevoegd aan `<Custom Object Name>` -triggers, wordt een speciale variabele beschikbaar in Scripttokens die worden uitgevoerd in de context van de triggercampagne: `$TriggerObject ` (niet ondersteund voor `<Custom Object Name>` wordt bijgewerkt).  Als een token met een `$TriggerObject` -verwijzing wordt gebruikt in een batchcampagne, mislukt het verzenden van de e-mail, omdat dit object niet beschikbaar is in batchcampagnes van welke aard dan ook.  Dit is een verwijzing naar het object dat de campagne heeft gestart. Het object bevat alle gegevens die de record heeft wanneer deze via een andere variabelenaam wordt benaderd.

Als een campagne bijvoorbeeld via een aangepast object voor een productvolgorde is geactiveerd, wordt de volgorde waaraan de lead is toegevoegd, weergegeven in de variabele `$TriggerObject` .

Hier volgt een voorbeeldscript voor een e-mail met follow-upinstructies voor bestellingen:

```html
<div>
<strong>Your order information:</strong>
##pull information from the Triggering Order and format it in a list
<ul>
<li>Product Ordered: $!{TriggerObject.ProductName}</li>
<li>Product Quantity: $!{TriggerObject.Quanitity}</li>
<li>Shipping Address: $!{TriggerObject.ShippingAddress}</li>
<li>Billing Address: $!{TriggerObject.BillingAddress}</li>
<li>Order Total: $!{TriggerObject.Amount}</li>
</ul>
<p><a href="$!{TriggerObject.OrderURL}">View Your Order Online</a></p>
</div>
```

Het voordeel van de variabele `$TriggerObject` is dat u geen code hoeft te wijden om te bepalen uit welke van de beschikbare objecten u uw lokale gegevens wilt ophalen.  Het object wordt bepaald door de activerende handeling. Dit is de meest expliciete manier om een object te kiezen waarnaar moet worden verwezen en moet worden gebruikt wanneer dit beschikbaar en passend is.

Opmerking: als u `$TriggerObject` gebruikt, moeten velden in het bewerkingsvenster worden gecontroleerd voordat het object beschikbaar wordt gemaakt voor het script.

Opmerking 2: `$TriggerObject` werkt alleen voor &#39;Toegevoegde&#39; triggers en niet voor &#39;Bijgewerkte&#39; triggers.
