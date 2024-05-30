---
title: Aan de slag
description: "Aan de slag met Marketo API's"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '1239'
ht-degree: 0%

---


# Aan de slag

Marketo is een platform voor marketingautomatisering dat marketers in staat stelt gepersonaliseerde meerkanaalprogramma&#39;s en -campagnes te beheren voor vooruitzichten en klanten. Het Marketo-platform kan worden uitgebreid met behulp van integratiepunten. Hieronder vind je de kernentiteiten en hun relaties.

De volgende objecten zijn niet beschikbaar via REST API als native synchronisatie is ingeschakeld: Bedrijf, Opportunity, Opportunity Role, Sales Person

![Gegevensmodel](assets/data_model.png)

## Persoon (leads)

Mensen vormen de basis van elk platform voor marketingautomatisering. In Marketo worden alle niet-verkooppersoonrecords vanuit verkoopperspectief als leads aangeduid, ongeacht of ze zijn aangewezen als leads, vooruitzichten, verdachten, contactpersonen, enzovoort. Het hoofdobject wordt geleverd met een set [standaardvelden](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadFieldsUsingGET) zoals e-mail, voornaam en achternaam. U kunt extra velden toevoegen aan het type hoofdobject om de typen informatie die aan records in het systeem zijn gekoppeld, uit te breiden. Aangepaste kenmerken kunnen worden gelezen en geschreven als standaardvelden. Een volledige lijst met velden is te vinden in de Marketo **[!UICONTROL Admin]** > **[!UICONTROL Field Management]** -menu. Leads worden in Marketo op unieke wijze geïdentificeerd door het id-veld. Andere unieke toetsen moeten extern van het systeem worden afgedwongen.

Verwante API&#39;s: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads), [SOAP](soap-api/leads.md), [JavaScript](javascript-api/lead-tracking.md#lead-tracking-api)

## Activiteiten

Leads onderhouden op een paar manieren met uw organisatie. Een lead kan een pagina op de website van uw bedrijf bezoeken, een exporteerprogramma bijwonen of een whitepaper downloaden. Elk van deze acties kan binnen Marketo worden gevangen om een marktleider te helpen beter begrijpen welke activiteiten een lood deed en wanneer zodat kunnen zij geschikte en relevante mededelingen coördineren. Activiteiten zijn altijd gerelateerd aan leads door leadId.

U kunt uw eigen aangepaste activiteiten definiëren. Nadat u een aangepaste activiteit hebt gemaakt en gepubliceerd, kunt u aangepaste activiteiten toevoegen via de Marketo API. Meer informatie over aangepaste activiteiten vindt u [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-activities/understanding-custom-activities).

Verwante API&#39;s: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities), [SOAP](soap-api/activities.md), [JavaScript](javascript-api/lead-tracking.md#munchkin-behavior)

## Programma&#39;s en campagnes

Een programma is het mechanisme waarmee een marktleider al zijn verschillende soorten marketinginspanningen van één centrale locatie organiseert. Een voorbeeld van een programma is een e-mailexplosie. Een lead kan meerdere acties/activiteiten in verband met een bepaald programma uitvoeren die bij het programma horen. Dit wordt doorbloeding genoemd. Een voorbeeldprogressie van een e-mailbladerprogramma zou registreren wanneer een lood een e-mail wordt verzonden, wanneer de persoon e-mail opende of zij door een verbinding in e-mail klikten.

Campagnes worden gecreeerd om een specifiek doel en een specifiek doel binnen een Programma te dienen. Een voorbeeld van een campagne zou kunnen zijn om een groep van lood te versmallen en hen de e-mailontploffing te verzenden, of een verkoopvertegenwoordiger voor follow-up op de hoogte te brengen als een lood door een verbinding binnen het e-mailblastageprogramma klikt.

Verwante API&#39;s: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns), [SOAP](soap-api/getcampaignsforsource.md)

## Tags

Met tags kunt u gegevens groeperen voor rapportagedoeleinden. Deze herkenningstekens verstrekken de capaciteit om gegevens te categoriseren en te bepalen hoe u over uw Programma wilt rapporteren om de doeltreffendheid van het Programma en ROI te begrijpen.

Als Marketo-beheerder kunt u vereiste en optionele labeltypen maken die beschikbaar zijn voor selectie wanneer een Marketo-gebruiker een programma maakt. Mogelijke waarden voor elk van deze typen tags worden door u gedefinieerd en geven aan hoe uw bedrijf aangepaste tags wil gebruiken voor rapportagedoeleinden.

U kunt bijvoorbeeld een aangepast type ‘Regio’ maken met meerdere tagwaarden (bijvoorbeeld Noordoost, Zuidoost), zodat u kunt analyseren welk gebied de meeste leads genereert. Of u kunt bijvoorbeeld een type code &#39;Eigenaar&#39; maken, waarmee u kunt beoordelen en begrijpen welke programmaeigenaars (bijvoorbeeld Maria, David of John) het meeste effect hebben op het creëren van kansen en kansen. Meer informatie over tags vindt u [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/understanding-tags).

Verwante API&#39;s: [REST](https://developer.adobe.com/marketo-apis/api/asset/), [SOAP](soap-api/gettags.md)

## Lijsten

Met lijsten kan een markering een verzameling leads ordenen. Er zijn twee typen lijsten in Marketo: statisch en slim. Een statische lijst is een vaste lijst met leads die een markator naar keuze kan toevoegen of verwijderen. Een slimme lijst is een dynamische verzameling van leads op basis van een reeks aangewezen kenmerken. Een voorbeeld van een slimme lijst zou zijn &quot;Alle leads die de prijspagina op onze website hebben bezocht.&quot; Deze slimme lijst blijft groeien naarmate meer leads de prijspagina bezoeken. Meer informatie over lijsten vindt u [hier](https://experienceleague.adobe.com/en/docs/marketo/using/home).

Verwante API&#39;s: [REST](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists), [SOAP](soap-api/getimporttoliststatus.md)

## Kansen

Marktdeelnemers leveren verkoopkansen. Een kans vertegenwoordigt een potentiële verkoopovereenkomst en wordt geassocieerd met een lood of een contact en een organisatie in Marketo. Een opportuniteitsrol is de doorsnede tussen een bepaalde leiding en een organisatie. De opportuniteitsrol heeft betrekking op de functie van een lead binnen de organisatie.

Verwante API&#39;s: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities), [SOAP](soap-api/getmobjects.md)

## Bedrijven

Een organisatie, ook wel een account in Marketo genoemd, verwijst naar de organisatie waartoe een persoon behoort. Wanneer het gebruiken van ROI rapportering in Marketo of de Analyse van de Cyclus van de Opbrengst (RCA), is het belangrijk om mensen met hun organisatie en kansen te associëren zodat kan de juiste ROI attributie worden bepaald.

Verwante API&#39;s: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies), [SOAP](soap-api/leads.md)

## Activa

Elementen verwijzen naar bestemmingspagina&#39;s, e-mails, formulieren en afbeeldingen die in een programma worden gebruikt. Elementen kunnen lokaal zijn voor een bepaald programma of wereldwijd. Globale middelen zijn beschikbaar in elk programma.

Verwante API&#39;s: [REST](https://developer.adobe.com/marketo-apis/api/asset/)

## Tokens

Met tokens kan een markeerteken berichten personaliseren met elementen en logica toevoegen binnen stroomhandelingen. Er zijn tokens voor het algemene systeem, programma&#39;s, leads en bedrijven. Een voorbeeld van een token lead is {{lead.First Name}}. Deze token kan in een e-mail worden geplaatst om de voornaam van de lead weer te geven.

Tokens die op het niveau van het Programma of van de omslag worden bepaald worden bedoeld als &quot;Mijn Tokens&quot;binnen Marketo. Mijn tokens kunnen van drie typen zijn: lokaal, overgeërfd of overschreven.

Mijn tokens die lokaal binnen een specifieke campagnemap of een specifiek programma worden gecreeerd zijn beschikbaar aan dat specifiek programma of (lokale) campagnemap. Mijn tokens die op het niveau van de campagnemap worden gecreeerd zijn beschikbaar voor gebruik over alle programma&#39;s binnen die (geërfte) campagnemap. Mijn tokens die op programmaniveau met douanewaarden worden gewijzigd veranderen niet de ouder Mijn Symbolische waarde van het teken op het niveau van de programmaomslag (met voeten getreden).

Mijn tokens gebruiken de naamgevingsconventie {{my.My Token}}, with the word "my" added to the beginning of the token name. For example, if you create a Date type My Token with the name EventDate, the name of the token is {{my.EventDate}}. Meer informatie over Mijn tokens vindt u [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program).

Verwante API&#39;s: [REST](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens), [SOAP](soap-api/getcampaignsforsource.md)

## Aangepaste objecten

Met een aangepast Marketo-object kunt u een een-op-veel- of een veel-op-veel-relatie (Edge-Bridge-Edge) maken tussen uw Marketo-leads en de aangepaste objectrapporten. Nadat u een aangepast Marketo-object hebt gemaakt en gepubliceerd, kunt u CRUD-bewerkingen op het aangepaste object uitvoeren via de Marketo API. Meer informatie over het maken van aangepaste objecten vindt u [hier](https://experienceleague.adobe.com/en/docs/marketo/using/home). Wanneer nieuwe records aan het aangepaste object worden toegevoegd, kunt u een trigger voor een slimme lijst gebruiken om te reageren. U kunt aangepaste objectgegevens ook als filter gebruiken in slimme lijsten (segmentatie) of in e-mails met [E-mailscripting](email-scripting.md).

Verwante API&#39;s: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects), [SOAP](soap-api/custom-objects.md)

## Verkopers

Verkooppersoonrecords en leadrelaties kunnen in Marketo worden beheerd als native CRM-integratie niet is ingeschakeld. Deze verslagen bevatten basisinformatie over de Persoon van de Verkoop, zoals Naam, E-mail, en de Titel van de Baan, die voor het filtreren en penningen in Marketo kan worden gebruikt wanneer een lood door wordt bezeten. De relatie met een verkooppersoon wordt beheerd op het hoofdniveau door het &quot;externalSalesPersonId&quot;gebied, dat door [Leads synchroniseren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) API.

Verwante API&#39;s: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Sales-Persons)
