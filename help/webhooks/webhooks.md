---
title: Webhaken
feature: Webhooks
description: Overzicht van webhooks
exl-id: fd283c66-05a1-4aa4-8412-0d41b8d1e3c8
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '562'
ht-degree: 0%

---

# Webhaken

Marketo staat het gebruik van Webhooks toe om met derdeWebservices te communiceren. Webhooks ondersteunen het gebruik van de HTTP-werkwoorden van de GET of POST om gegevens van een specifieke URL te verzenden of op te halen. Raadpleeg de volgende artikelen voor gedetailleerde instructies over het maken van Webhooks in toepassingen en hoe u deze aan Slimme campagnes kunt toevoegen:

- [Webhaak maken](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook)
- [Bellen Webhaak](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/call-webhook)
- [Een webhaak gebruiken in een slimme campagne](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/use-a-webhook-in-a-smart-campaign)

Elke afzonderlijke webhaak heeft de volgende eigenschappen:

- [!UICONTROL URL] - Voer de URL in die u gebruikt om uw verzoek bij de webservice in te dienen.
- [!UICONTROL Request Type] - De HTTP-methode.
- [!UICONTROL Payload Template] - Als u informatie in de tekst van de POST wilt verzenden, voert u de sjabloon in. Gebruik elke gegevensindeling die HTTP-POST ondersteunt, inclusief XML, JSON of SOAP. De rangschikkingsindeling moet dubbele aanhalingstekens om tekenreeksen toestaan. Als u een token wilt invoegen in uw sjabloon, klikt u op **[!UICONTROL Insert Token]**.  Tekenreekstype-tokens worden automatisch tussen dubbele aanhalingstekens geplaatst.
- [!UICONTROL Request Token Encoding] - Als de tokenwaarden speciale tekens bevatten (zoals een en-teken, &#39;&amp;&#39;), geeft u de indeling van uw aanvraag op (JSON of Form/Url). De juiste codering moet voor de hoofdtekst worden geselecteerd om ervoor te zorgen dat de Webhaak correct communiceert met de webservice.
- [!UICONTROL Response Type] - Selecteer de indeling van de reactie die u van de service ontvangt (JSON of XML). Het juiste reactietype moet zijn geselecteerd om de eigenschappen van het antwoord weer toe te wijzen aan loodvelden in Marketo
- [!UICONTROL Custom Headers] - Toegang verkregen via [!UICONTROL Webhooks Actions] -> [!UICONTROL Set Custom Header], staat dit menu de toevoeging van om het even welk aantal douane zeer belangrijk-Waarde paren als Kopballen van HTTP toe.

Gegevens kunnen via webservicereacties naar leads worden geschreven [Responstoewijzingen](response-mappings.md)

## Tokens

Alle uitgaande gebieden in een Webhaak (URL, Malplaatje, en Kopbal van de Douane) bevolken de inhoud van tekenen in de zelfde context van de stroomstap. Dit betekent dat de Tokens van het Lood en van het Systeem altijd beschikbaar zijn, terwijl de Tkens van de Trekker, van de Campagne, en van het Programma in hun respectieve werkingsgebied beschikbaar zijn. Zie tokenartikelen:

- [Overzicht van tokens](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/personalizing-landing-pages/tokens-overview)
- [Verklarende woordenlijst systeemtokens](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/system-tokens-glossary)
- [Tokens voor interessante momenten](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-sales-insight/msi-for-salesforce/features/tabs-in-the-msi-panel/interesting-moments/trigger-tokens-for-interesting-moments)

Een algemeen geval voor dit is wanneer een Programma of een Campagne uitdrukkelijk aan een derdemiddel in kaart wordt gebracht. Een id kan op programmaniveau als een `My Token`en wordt vervolgens als token doorgegeven aan de Webhaak-aanvraag.

## Aangepaste koppen

Met webhooks kan het gebruik van een willekeurig aantal aangepaste koptekstvelden samen met de uitgaande aanvraag worden verzonden. Deze kunnen via **[!UICONTROL Webhooks Actions]** > **[!UICONTROL Set Custom Header]**. Elke kopbal wordt geregistreerd als eenvoudig zeer belangrijk-waardepaar. Tokens kunnen in dit gebied worden gebruikt.

![Aangepaste koppen](assets/custom-headers.png)

## Tips

- De de stroomstap van Webhaak van de Vraag is slechts geldig in de campagnes van de Trekker.
- Updates via responstoewijzingen worden alleen uitgevoerd als de webservice reageert op een 2xx HTTP-antwoordcode. Andere typen codes resulteren niet in updates van de record.
- U kunt de Webdiensten gebruiken om de verrijking, bevestiging van douanegegevens, of normalisatie van de interne of externe diensten uit te voeren.
- De uitvoeringstijd van Webhaak is bij de overvloed van de reactietijd van de dienst die wordt gebruikt en kan in lange vertragingen van de campagneuitvoering resulteren. Zelfs als een dienst slechts 50 ms vergt om uit te voeren, dat is 1.5 uur wanneer uitgevoerd 100.000 keer.
- Marketo wacht tot 30 seconden op een bepaalde de dienstvraag alvorens de vraag (a.k.a. timing uit) te eindigen.
- Tekens die zijn ingesloten in het URL-veld, worden doorgegeven zoals geschreven, bijvoorbeeld &#39;&amp;&#39; wordt verzonden als &#39;&amp;&#39;, &#39;%26&#39; wordt verzonden als &#39;%26&#39;
   - Als een karakter percent-gecodeerd zou moeten zijn wanneer ontvangen door de ontvankelijke server, zou het uitdrukkelijk als koord moeten worden overgegaan die dat karakter vertegenwoordigen
