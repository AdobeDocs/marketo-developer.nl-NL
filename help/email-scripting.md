---
title: E-mailscripting
feature: Email Programs
description: Overzicht van e-mailscripts
exl-id: ff396f8b-80c2-4c87-959e-fb8783c391bf
source-git-commit: 9012135dc7a295c2462574dad1aca2d06a9077ea
workflow-type: tm+mt
source-wordcount: '947'
ht-degree: 0%

---

# E-mailscripting

NOTA: Het wordt hoogst geadviseerd dat u de [ Gids van de Gebruiker van de Snelheid ](https://velocity.apache.org/engine/devel/user-guide.html) voor een diep-duik in het gedrag van de Taal van het Malplaatje van de Snelheid leest.

[ de Snelheid van Apache ](https://velocity.apache.org/) is een taal die op Java wordt voortgebouwd die voor het templating en het scripting van de inhoud van HTML wordt ontworpen. Marketo staat het toe om in de context van E-mail te worden gebruikt door scripting tokens te gebruiken. Hierdoor hebt u toegang tot gegevens die zijn opgeslagen in Opportunity en Custom Objects, en kunt u dynamische inhoud maken in e-mails. Snelheid biedt standaard besturingsstroming op hoog niveau met if/else, for en for each om voorwaardelijke en iteratieve manipulatie van inhoud toe te staan. Hier is een eenvoudig voorbeeld om een groet met de correcte aanhef te drukken:

```java
##check if the lead is male
#if(${lead.MarketoSocialGender} == "Male")
    ##if the lead is male, use the salutation 'Mr.'
    #set($greeting = "Dear Mr. ${lead.LastName},")
##check is the lead is female
#elseif(${lead.MarketoSocialGender} == "Female")
    ##if female, use the salutation 'Ms.'
    #set($greeting = "Dear Ms. ${lead.LastName},")
#else
    ##otherwise, use the first name
    #set($greeting = "Dear ${lead.FirstName},")
#end
##print the greeting and some content
${greeting}

    Lorem ipsum dolor sit amet...
```

## Variabelen

Variabelen worden altijd vooraf ingesteld op &#39;$&#39; en worden ingesteld en bijgewerkt met #set:

```
#set($variable = "value")
```

Hun waarden kunnen dan via verscheidene verschillende verwijzingstypes met verschillend gedrag worden teruggewonnen:

```
$variable ##outputs 'value'
$variablename ##outputs '$variablename'
${variable}name ##outputs 'valuename'
```

Er is ook een stille verwijzingsnotatie, waarbij er een `!` Opgenomen is na de `$` . Wanneer de snelheid een ongedefinieerde verwijzing tegenkomt, wordt de tekenreeks die de verwijzing vertegenwoordigt, normaal gesproken op zijn plaats gelaten. Met een rustige verwijzingsnotatie wordt, als een niet gedefiniëerde verwijzing wordt ontmoet, dan geen waarde uitgestraft:

```
##Defined Reference

#set($foo = "bar")
$foo ##outputs "bar"

##Undefined Reference

##normal
$baz ##outputs "$baz"

##quiet
$!baz ##outputs nothing
```

Voor meer informatie over hoe te om variabelen van verwijzingen te voorzien, zie de [ Gids van de Gebruiker Apache ](https://velocity.apache.org/engine/devel/user-guide.html#formal-reference-notation).

## Snelheidsgereedschappen

Het Project van de Snelheid Apache maakt functionaliteit door het gebruik van [ Hulpmiddelen van de Snelheid ](https://velocity.apache.org/tools/devel/apidocs/overview-summary.html) beschikbaar. Dit zijn eenvoudig wrappers voor voorwerpen Java en hun methodes door globale variabelen blootstellen die aan alle manuscripten ter beschikking worden gesteld.

- [ AlternatorTool ](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/AlternatorTool.html)
- [ ComparisonDateTool ](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/ComparisonDateTool.html)
- [ ConversionTool ](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/ConversionTool.html)
- [ DateTool ](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/DateTool.html)
- [ DisplayTool ](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/DisplayTool.html)
- [ MathTool ](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/MathTool.html)
- [ NumberTool ](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/NumberTool.html)
- [ EscapeTool ](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/EscapeTool.html)
- [ LoopTool ](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/LoopTool.html)

Als u bijvoorbeeld een methode van `ComparisonDateTool` wilt gebruiken, opent u de gebeurtenis `$date` via een scripttoken:

```
#set($birthday = $convert.parseDate("2015-08-07","yyyy-MM-dd"))
##use whenIs to determine how many days away it is
$date.whenIs($birthday).days ##outputs 1
```

## Een scripttoken maken

Het snelheidsscript wordt opgenomen in e-mailberichten met behulp van e-mailscripttokens. Deze kunnen in de Activiteiten van de Marketing in of een Omslag van de Marketing of een Programma worden gecreeerd. Als u een token in een e-mailbericht wilt gebruiken, moet de e-mail een onderliggend item zijn van een programma dat de token bezit of overneemt van een marketingmap. Als u een token wilt maken, navigeert u naar een map of programma en selecteert u de tab [!UICONTROL My Tokens] . Sleep vanuit het rechtermenu de optie &#39;E-mailscript&#39; naar de tokenlijst

![ Symbolisch van het Manuscript ](assets/script-token.png)

Vanaf hier kunt u de naam van het token bewerken en de editor openen via de optie [!UICONTROL Click to Edit] :

![ geef Manuscript ](assets/script-edit.png) uit

Wanneer u zich in de editor bevindt, kunt u een script maken met toegang tot alle variabelen in objecten die toegankelijk zijn voor scripts. Als u een veldverwijzing van een object wilt ophalen, sleept u deze van de rechterstructuur naar het script:

![ geef de Token van het Manuscript uit ](assets/edit-script-token.png)

## Scriptinsluiting en testen

Als u uw script hebt gedefinieerd in een Program My Token, kunt u ernaar verwijzen in een bepaalde e-mail via de e-maileditor van Marketo.

![ E-mailManuscript ](assets/email-script-marketo-email.png)

U kunt uw script testen met de e-mailactie [!UICONTROL Send Sample Email] in de Marketo-e-mailontwerper. Het script kan alleen correct worden verwerkt als u een bestaande lead selecteert om zich voor te doen in het veld [!UICONTROL Lead] . Als u met `$TriggerObject` test, kunt u het activerende voorwerp via [!UICONTROL Trigger] param selecteren. Hierbij worden de gegevens van het laatst bijgewerkte object van dat type gebruikt als de variabele `$TriggerObject` .

![ Testen E-mailmanuscript ](assets/velocity-test.png)

U kunt het script ook testen met [!UICONTROL Email Preview] . Hiervoor moet u **[!UICONTROL View As: Lead Detail]** selecteren en een lead in een beschikbare statische lijst selecteren. Dit heeft het extra voordeel om het even welke uitzonderingen uit te voeren die tijdens manuscriptuitvoering kunnen zijn voorgekomen:

![ E-mail van de Mening als ](assets/view-as.png)

## Nuttige tips

De gecombineerde lengte van alle e-mailscripttokens in een bepaalde e-mail mag niet groter zijn dan 100.000 bytes. Deze limiet heeft betrekking op de totale lengte van de tokentekenreeksen zelf (niet de totale lengte nadat tokens zijn uitgebreid).

- De variabelen waarnaar in het e-mailscript wordt verwezen, moeten in Marketo aanwezig zijn op een van de objecten die beschikbaar zijn voor het script.
- U kunt naar eerste en tweede niveaudouanevoorwerpen van verwijzingen voorzien die uit uw binnen geïntegreerde CRM voortkomen die direct met de Lood, of Contact, maar niet derdevoorwerpen van de Douane worden verbonden. Custom Objects may not be parent of the Lead or Company
- Voor aangepaste Marketo-objecten kunt u naar aangepaste objecten op het tweede niveau verwijzen met de relatie Bovenliggend-Onderliggend. Bijvoorbeeld `Lead <- Parent <- Child` . U kunt niet verwijzen naar aangepaste objecten op het tweede niveau met Edge-Bridge relatie. bijv., `Lead <- Bridge -> Edge`
- U kunt naar aangepaste objecten verwijzen die zijn verbonden met een lead, contactpersoon of account, maar niet meer dan een account.
- Naar aangepaste objecten kan slechts worden verwezen via één verbinding, lead, contactpersoon of account
- U moet het vakje in de manuscriptredacteur voor de gebieden controleren u gebruikt of zij zullen niet verwerken
- Voor elk aangepast object zijn de tien meest recente bijgewerkte records per persoon/contactpersoon beschikbaar bij uitvoering en worden deze geordend van de meest recente update (op 0) tot de oudste update (op 9). U kunt het aantal verslagen verhogen beschikbaar door [ na de instructies ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/email-setup/change-custom-object-retrieval-limits-in-velocity-scripting).
- Als u meer dan één e-mailscript in een e-mail opneemt, worden deze van boven naar beneden uitgevoerd. Het bereik van variabelen die zijn gedefinieerd in het eerste script dat moet worden uitgevoerd, is beschikbaar in volgende scripts.
- De Verwijzing van hulpmiddelen: [ https://velocity.apache.org/tools/2.0/index.html](https://velocity.apache.org/tools/2.0/index.html)
- Een opmerking over tokens die nieuwe-regeltekens &quot;\\n&quot; of &quot;\\r\\n&quot; bevatten. Wanneer een e-mail wordt verzonden via Sample verzenden of via een campagne Batch, worden nieuwe-regeltekens in tokens vervangen door spaties. Wanneer e-mail via de campagne Trigger wordt verzonden, blijven nieuwe-regeltekens ongewijzigd.
- Om ervoor te zorgen dat URL&#39;s correct worden geparseerd, moet het hele pad worden ingesteld als een variabele en vervolgens worden afgedrukt. De variabele mag niet worden afgedrukt binnen URL-referenties. Het protocol (http:// of https://) moet worden opgenomen en moet gescheiden zijn van de rest van de URL. De URL moet ook deel uitmaken van een volledig gevormde ankertag (<a>). Het script moet een volledig gevormde ankertag uitvoeren, zodat koppelingen kunnen worden bijgehouden. Koppelingen worden niet bijgehouden als deze worden uitgevoerd vanuit een lus for of foreach.

```html
<!-- Correct -->
#set($url = "www.example.com/${object.id}")
<a href="http://${url}">Link Text</a>

<!-- Correct -->
<a href="http://www.example.com/${object.id}">Link Text</a>

<!-- Incorrect -->
<a href="${url}">Link Text</a>

<!-- Incorrect -->
<a href="{{my.link}}">Link Text</a>

<!-- Incorrect -->
<a href="http://{{my.link}}">Link Text</a>
```
