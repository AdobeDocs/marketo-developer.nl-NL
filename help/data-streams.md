---
title: Gegevensstromen
description: Overzicht van gegevensstromen
exl-id: 5617b6a5-ebc8-4d97-a290-e3b87f83e360
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '1596'
ht-degree: 0%

---

# Gegevensstromen

>[!NOTE]
> De huidige informatie over gegevensstromen wordt nu gevonden bij [ Gebruikend de Streams van Gegevens ](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams/).
>

De marketingorganisaties van onze klanten vertrouwen op tijdige en gerichte marketingcampagnes om op hun bedrijf te blijven en concurrerend te zijn. Om snelle beslissingen te ondersteunen en strategische veranderingen op snelheid mogelijk te maken, is het belangrijk dat u over gegevens beschikt om die belangrijke beslissingen die gerichte en gerichte campagnes leveren, te ondersteunen en te sturen. Er zijn ook klanten die marketinginspanningen uitvoeren op het niveau van hun klantensegmenten, zowel binnen als buiten Marketo Engage. Om deze verschillende inspanningen te ondersteunen, heeft Marketo de mogelijkheid gecreëerd om grote hoeveelheden gegevens te verkrijgen in bijna realtime via gegevensstreams.

Naast de voordelen van gegevens in real time, zijn er productgerelateerde voordelen:

- Hiermee wordt het knelpunt van API-limieten hersteld, omdat in plaats daarvan streaming wordt gebruikt.
- Vermindert het scenario van API grenzen, die minder waakzaam overseinen produceren.
- U hoeft geen grote hoeveelheden te exporteren om gegevens te extraheren vanwege de mogelijkheid voor gegevensstreaming.

De Streams van gegevens zijn beschikbaar aan die die het Pakket van de Rij van de Prestaties van a [ Marketo Engage ](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835) hebben gekocht.

## Overzicht van gegevensstroom voor hoofdactiviteit

De gegevensstroom van de Activiteit van de lood verstrekt bijna in real time het stromen van controle het volgen van Activiteiten waar de grote volumes van de Activiteiten van de Leiding aan het externe systeem van een klant kunnen worden verzonden. Met streams kunnen klanten gebeurtenissen met betrekking tot leads, gebruikspatronen op effectieve wijze controleren, weergaven bieden voor wijzigingen in leads en processen en workflows activeren op basis van de verschillende typen lead Events. Er zijn meer dan 144 types van Activiteit die aan kunnen worden geabonneerd en door de stroom worden verzonden.

Typen gegevensstroom voor lead:

1. Wijzigingen voor lead - alle wijzigingen in alle velden en nieuwe leads
1. Loodactiviteiten - alle soorten leidingactiviteiten die in het document worden beschreven
1. Verwijderde leads
1. Alle aangepaste objecten op de lead (indien aangevraagd). Op dit moment is het allemaal of niets.

Door meningen in de veranderingen van het Lood te verstrekken, staat dit klanten toe om snellere besluiten over hun algemene marketing strategieën te nemen en gerichtere gerichte campagnes te creëren. Sommige populaire gebruiksgevallen zijn:

- Aangepaste waarschuwingen: wanneer bepaalde leads worden gevonden met inconsistente voorwaarden, kunnen deze worden toegevoegd aan de lijst. Activiteitstromen kunnen deze ophalen en de activiteit &quot;Toevoegen aan lijst&quot; voor klanten naar elke volgende actie duwen.
- De motor van de Modellen van ML: Sommige klanten zijn van plan om het schrapen modellen te bouwen die de inzichten van de Activiteit gebruiken en hen terug naar Marketo of gebruik in hun eigen interne het scoremodellen zoals gewenst terugvoeren. Door een lead te scoren, kunnen klanten Marketo dan informeren om klanten toe te voegen aan cursussen voor verpleegkundigen en hun scoring verhogen.

Lijst met gestreamde activiteiten:

| AchieveGoalInReferral | ClickPredictiveContent | ReceivedForwardToFriendEmail |
|--- |--- |--- |
| AddToList | ClickRTPCallToAction | ReceiveSalesEmail |
| AddToNurture | ClickSalesEmail | ReferToSocialApp |
| AddToOpportunity | ClickSharedLink | RemoveFromList |
| AddToSalesCampaign | ConvertLead | RemoveFromOpportunity |
| CallWebhaak | DeleteLead | RequestCampaign |
| ChangeDataValue | DiskwalificeerSweepstakes | SalesEmailBounce |
| ChangeLeadPartition | EarnEntryInSocialApp | SendAlert |
| ChangeNurtureCadence | EmailBted | SendEmail |
| ChangeNurtureTrack | EmailBounceSoft | SendSalesEmail |
| ChangeOwner | EmailDelivered | SentForwardToFriendEmail |
| ChangeProgramData | EnrichWithDataDotCom | SFDCActivity |
| ChangeProgramMemberData | EnterSweepstakes | ShareContent |
| ChangeRevenueStage | FillOutFacebookLeadAdsForm | SignUpForReferralOffer |
| ChangeRevenueStageManually | FillOutForm | SyncLeadToMicrosoft |
| ChangeScore | InterestingMoment | SyncLeadToSFDC |
| ChangeSegment | MergeLeads | UnsubscribeEmail |
| ChangeStatusInProgression | NewLead | UpdateOpportunity |
| ChangeStatusInSalesCampaign | OpenEmail | VisitWebPage |
| ClickEmail | OpenSalesEmail | StemmingInPoll |
| ClickLink | PushLeadToMarketo | WinSweepstakes |

Als aangepaste objecten moeten worden gestreamd, moet dit alle hoofdobjecten zijn. Op dit moment is er geen enkele manier om te kiezen welke er worden gewenst.

## Overzicht van gegevensstroom door gebruikerscontrole

De Stream van de Gegevens van de Controle van de gebruiker verstrekt dichtbij controle in real time het volgen van activaveranderingen door gebruikers &#x200B;. Dit laat een klant toe om activagebeurtenissen effectief te controleren, een mening in gebruikersveranderingen te verstrekken, en processen of werkschema&#39;s teweeg te brengen die op verschillende soorten controlegebeurtenissen worden gebaseerd. Bijna worden de activa in real time verzonden via de gebeurtenissen van Adobe I/O naar een configureerbaar eindpunt. Controlegebeurtenissen worden uitgesplitst naar type element en kunnen worden geabonneerd op controlegebeurtenissen die voor hen van belang zijn.

Een goede reden om een abonnement te nemen op deze stream is:

- Wijzigingen bijhouden bij het gebruik van meerdere marketingsystemen: er zijn klanten die ook een bepaald niveau van marketingactiviteiten uitvoeren in een ander systeem, zoals een CRM, zoals Salesforce, en vervolgens de lead doorgeven aan Marketo. De lead wordt soms bijgewerkt en gesynchroniseerd, dus het is belangrijk om te volgen welk systeem recente wijzigingen heeft aangebracht.

Lijst met gestreamde gebruikersauditgebeurtenissen:

| COMPONENT | LIJST MET GEBEURTENISTYPEN |
|--- |--- |
| Standaardprogramma | klonen, maken, verwijderen, kanaal bewerken, exporteren, programmainstellingen wijzigen, programmatietoken wijzigen, naam wijzigen |
| E-mail | goedkeuren, klonen, maken, verwijderen, bewerken, verplaatsen, hernoemen, niet goedkeuren |
| Batchprogramma voor e-mail | goedkeuren, childUpdate, kloon, creeer, schrap, geef kanaal uit, wijzig programmaschema, wijzig programmaopstelling, wijzig programmateken, hernoem, unkeuren |
| E-mailsjabloon | goedkeuren, klonen, maken, verwijderen, conceptMaken, conceptNegeren, bewerken, hernoemen, niet goedkeuren |
| Programma voor betrokkenheid | klonen, maken, verwijderen, kanaal bewerken, programmainstellingen wijzigen, programmastroom wijzigen, programmatietoken wijzigen, naam wijzigen |
| Gebeurtenisprogramma | klonen, maken, verwijderen, kanaal bewerken, programmaschema wijzigen, programmainstellingen wijzigen, programmatietoken wijzigen, naam wijzigen |
| Map | maken, verwijderen, bewerken, naam wijzigen |
| Formulier | goedkeuren, klonen, maken, verwijderen, conceptMaken, bewerken, verplaatsen, naam wijzigen |
| Formulier -> Pagina-formulier voor landing | maken, klonen, bewerken, verwijderen, goedkeuren, naam wijzigen |
| Openingspagina | goedkeuren, klonen, maken, verwijderen, conceptNegeren, bewerken, hernoemen, niet goedkeuren |
| Landingspagina-sjabloon | goedkeuren, klonen, maken, verwijderen, conceptMaken, conceptNegeren, bewerken, hernoemen, niet goedkeuren |
| Slimme lijst | klonen, maken, verwijderen, bewerken, exporteren, wijzigen, instellen van lijst vervangen, naam wijzigen |
| Marketingmap | maken, bewerken, verwijderen |
| Nuratieprogramma | klonen, maken, verwijderen, kanaal bewerken, programmainstellingen wijzigen, programmastroom wijzigen, programmatietoken wijzigen, naam wijzigen |
| Segment | maken, verwijderen, bewerken, naam wijzigen |
| Segmentatie | goedkeuren, maken, verwijderen, conceptCreated, conceptDiscarded, naam wijzigen, niet goedkeuren |
| Slimme campagne | afbreken, activeren, klonen, maken, deactiveren, verwijderen, bewerken, campagnereschema wijzigen, stap wijzigen, instellen van slimme lijst wijzigen, verplaatsen, naam wijzigen |
| Fragment | goedkeuren, goedkeuren zonder ontwerp, klonen, creëren, schrappen, uitgeven, anders noemen, unkeuren |
| Admin UI -> Launchpoint -> Integration | maken, verwijderen, bewerken |
| Gebruikersinterface beheerder -> Gebruiker | maken, bewerken en verwijderen (alleen voor de API-gebruiker) |
| Aanmelding beheerder -> Gebruiker | aanmelden geslaagd, aanmelden mislukt |
| Programma -> Batch-programma voor e-mail | Asset API bewerken (voor wijzigen geselecteerd e-mailadres) |
| Programma -> Marketing Program | maken, klonen |

Voorbeeld van een controlegebeurtenis gebruiker:

```json
{
    "event_id": "a1b2c3d4-zyxw-9876-9z8y-a1b2c3d4e5f6",
    "event": {
        "specversion": "1.0",
        "id": "b77c743a-8e28-40f2-8aab-9541bbc85e68",
        "type": "com.adobe.platform.marketo.audit.user.email",
        "source": "https://www.marketo.com",
        "time": "2020-05-28T19:20:47.28Z",
        "datacontenttype": "application/json",
        "dataschema": "V1.0",
        "data": {
            "componentId": 232459,
            "componentType": "Email",
            "eventAction": "approve",
            "munchkinId": "123-ABC-456",
            "imsOrgId": "ADOBEORGID@AdobeOrg",
            "user": 253,
            "userId": "example@marketo.com"
        }
    }
}
```

## Gegevensstroomoverzicht van kennisgeving

Gegevensstroom voor meldingen is beschikbaar als onderdeel van het prestatieniveauaanbod van Marketo Engage.

Momenteel kan het meldingscentrum in Marketo zo worden geconfigureerd dat meldingen naar een e-mailadres worden verzonden. De Stream van de Gegevens van het bericht laat de berichten toe om rechtstreeks naar een configureerbaar eindpunt via de gebeurtenissen van Adobe I/O worden verzonden. Meldingen worden vandaag via de gebruikersinterface geleverd en kunnen worden aangehaald door de oranje bel in de rechterbovenhoek van het scherm. Deze stream neemt deze meldingen en stuurt ze door een stream.

Lijst met meldingsgebeurtenissen:

| COMPONENT | LIJST MET GEBEURTENISTYPEN |
|--- |--- |
| Melding | campagneabort, mislukking van de campagne, koeling (programma uitgeput), mislukking van de verkoopsforcatie, testgroep (testresultaat A/B), webservices (dagelijkse quota) |

Voorbeeld van een meldingsgebeurtenis:

```json
{
    "event_id": "a1b2c3d4-zyxw-9876-9z8y-a1b2c3d4e5f6",
    "event": {
        "specversion": "1.0",
        "type": "com.adobe.platform.marketo.notification.campaign_abort",
        "source": "https://www.marketo.com",
        "time": "2021-05-27T10:22:37.489-5:00",
        "datacontenttype": "application/json",
        "dataschema": "V1.0",
        "data": {
            "componentType": "campaign_abort",
            "subType": "user_campaign_abort",
            "eventAction": {
                "campaignId":1234,
                "userId":"example@marketo.com",
            }
            "imsOrgId":"ADOBEORGID@AdobeOrg",
            "munchkinId":"123-ABC-456"
        }
    }
}
```

## Technische details

Deze sectie bevat richtlijnen voor wat nodig is, hoe u verbinding kunt maken met streaming gegevens en deze kunt ontvangen voor elk van de streams. Bij elke codering en installatie is een niveau betrokken.

### Gegevensstroom leidingactiviteit

De hoofdactiviteitsstroom biedt vrijwel realtime streaming van Marketo Lead Activity-gebeurtenissen en verzendt geabonneerde wijzigingen in het type activiteit met configureerbare kenmerken:

- De frequentie van gegevens duwt elke 2 seconden door gebrek.
- Batches van 100 tot 500 per abonnement.
- De onderbreking voor de dienst van het KLEINE TUSSENRUIMTE van de klant is 20 seconden met 3 herpogingen om de 3 minuten, en auto die op succes wordt toegelaten. Anders worden ze gepauzeerd. Zodra zijn gepauzeerd, probeert de dienst opnieuw om de 3 minuten in een poging om opnieuw toe te laten tenzij de-provisioned manueel.
- Gegevens worden maximaal 7 dagen in een wachtrij bewaard.

Om de gegevensstroom van de Activiteit van de Lood uit te voeren, zijn hier de stappen voor klanten te volgen:

1. Maak een eindpunt van HTTP bloot dat POST- verzoeken met een lichaam JSON van openbaar Internet kan ontvangen. De Activity Push Data Stream verzendt aanvragen naar:
1. Geef Adobe het volgende op:
   1. Marketo Munchkin ID for their subscription
   1. De URL van het eindpunt in stap 1
   1. De typen activiteiten die zij willen ontvangen (volledige lijst hierboven)
   1. Een middel van authentificatie, zodat de klant kan verifiëren dat de verzoeken wettig zijn. Ofwel:
      1. Een identiteitsleverancier URL, identiteitskaart van de Cliënt, en Geheim voor de Authentificatie van de Credentials van de Cliënt OAuth [&#128279;](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/)
      1. Een API-token, dat kan worden opgenomen in aanvragen die door de Lead Activity DataStream worden verzonden in een Authorization http-header

Adobe schakelt vervolgens de gegevensstroom in, waarna klanten gegevens beginnen te ontvangen.

Het diagram van UML van een typische vraag van de Gegevens van de Activiteit van het Lood:

![ het diagram van de Gegevens van de Activiteit van de Leiding ](assets/lead-activity-data-stream.png)

Voorbeeld van het maken van URL-eindpunten:

```javascript
/*
Copyright 2022 Adobe
All Rights Reserved.

NOTICE: Adobe permits you to use, modify, and distribute this file in
accordance with the terms of the Adobe license agreement accompanying
it.
*/
constexpress=require('express')
constwinston=require('winston');
constport=3000

constapp=express().use(express.json())

constlogger=winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  defaultMeta: {service: 'activity-stream-consumer-example'},
  transports: [
    // - Write all logs with level `error` and below to `error.log`
    newwinston.transports.File({filename: 'error.log',level: 'error'}),
    // - Write all logs with level `info` and below to `combined.log`
    newwinston.transports.File({filename: 'combined.log'}),
    newwinston.transports.Console({format: winston.format.simple()})
  ],
});

app.get('/',(req,res)=>{
  logger.info(JSON.stringify(req.query))
  res.sendStatus(200)
})

app.post('/',(req,res)=>{
  logger.info(JSON.stringify(req.body))
  res.sendStatus(200)
})

app.listen(port,()=>{
  logger.info(`app listening on port ${port}`)
})
```

Een codesteekproef voor een toepassing die de stroom van de Gegevens van de Activiteit van de Lood van Marketo verbruikt kan [ hier ](https://github.com/ihgrant/activity-stream-consumer-example) worden gevonden.

### Gegevensstroom van gebruikerscontrole en gegevensstroom van berichten

De gebeurtenissen van de Controle van de gebruiker worden verzonden naar Adobe IO en kunnen door login met een Adobe ID worden verbruikt. Hier volgen de volgende stappen:

1. Klanten bieden Adobe het volgende:
   1. Adobe ID
   1. Marketo Munchkin ID for their subscription
1. De klant stelt een REST eindpunt bloot om gebeurtenissen normaal in de vorm van een webhaak te verbruiken.
1. Zodra dat wordt verstrekt, laat Adobe de stroom voor het abonnement van de klant toe.
1. De klant stelt vervolgens de stream in Adobe IO in (instructies die moeten worden verstrekt)
   1. Voor deze stap is Adobe Org vereist
   1. Adobe Org-gebruiker moet Ontwikkelaar of Systeembeheerfunctie hebben

Aan opstelling Adobe IO, zie [ de Streams van de Gegevens van de Controle van de Gebruiker van Marketo met Adobe IO ](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-user-audit-data-stream-setup/) in de Openbare sectie van de Documentatie.

### Gebruikersauditgegevensstroom instellen in Marketo

De gegevensstroom van de Controle van de Gebruiker is momenteel beschikbaar als deel van de pakketten van Prestaties samen met andere 3 Streams van Gegevens. Voor meer informatie over de Pakketten, verwijs naar de [ Pagina van de Beschrijving van het Product ](https://helpx.adobe.com/legal/product-descriptions/adobe-marketo-engage---product-description.html) voor de grenzen en de eigenschappen van het Product.

### Adobe I/O instellen

[ zie Begonnen het Worden met Adobe I/O Events ](https://developer.adobe.com/runtime/docs/guides/getting-started/)

Voor basisinstructies voor dit gebruiksgeval, die van [ console.adobe.io ](https://developer.adobe.com/console) beginnen:

Selecteer **[!UICONTROL Create New Project]** of **[!UICONTROL Add Event]** als daarom wordt gevraagd.

### Aan de slag met uw nieuwe project

Om de diensten van Adobe te beginnen te gebruiken, voeg API, gebeurtenissen of runtime toe, bekijk onze [ documentatie ](https://developer.adobe.com/runtime/docs/).

## Openbare documentatie

- [ de Streams van Gegevens van Marketo ](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams/)
- [ Intro aan de Gebeurtenissen van Adobe IO &amp; Webhooks ](https://developer.adobe.com/events/docs/guides/)
- [ Blog van de Streams van Gegevens ](https://blog.developer.adobe.com/introducing-the-adobe-marketo-engage-data-streams-61198b567fbb)
