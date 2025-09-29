---
title: Standaardvelden
feature: REST API, Field Management
description: Blader door de volledige lijst met Marketo-standaardloodvelden met REST- en SOAP-namen, -labels en -beschrijvingen, en hoe u deze ophaalt via de Describe Lead-API.
exl-id: 147dbdff-4bc9-4ab3-8918-c4de3e1aa97a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1161'
ht-degree: 7%

---

# Standaardvelden

Hier volgt een lijst met standaardvelden die beschikbaar zijn in Marketo en die toegankelijk zijn via de API.

U kunt de lijst van alle gesteunde gebiedsnamen terugwinnen beschikbaar op uw loodverslagen door REST [ te gebruiken beschrijf lood ](https://developer.adobe.com/marketo-apis/api/mapi/) eindpunt.

| REST API-naam | SOAP API-naam | Friendly Label | Beschrijving |
| --- | --- | --- | --- |
| adres | Adres | Adres | Adres van lead |
| annualRevenue | Jaarlijkse inkomsten | Jaaromzet | Jaarlijkse inkomsten van de hoofdvennootschap |
| anoniemIP | AnonymousIP | Anoniem IP-adres | IP adres van het eerste geregistreerde Webbezoek van de lood |
| billingCity | BillingCity | Factuurstad | Plaats van het factuuradres van de lead |
| factureringsland | Factureringsland | Factuurland | Land van het factuuradres van de lead |
| billingPostalCode | BillingPostalCode | Factuurpostcode | Postcode van het factuuradres van de lead |
| factureringsstatus | BillingState | Factuurstaat | Staat of provincie van het factuuradres van de lead |
| factureringsstraat | BillingStreet | Factuuradres | Adres van het bedrijf van de lead op de factuuradres |
| stad | Stad | Stad | Loodstad |
| bedrijf | Bedrijf | Bedrijfsnaam | Bedrijfsnaam van Lead |
| land | Land | Land | Loodse staat |
| dateOfBirth | Geboortedatum | Geboortedatum | Geboortedatum van de leider |
| afdeling | Afdeling | Afdeling | Loodse afdeling in hun bedrijf |
| doNotCall | DoNotCall | Niet bellen | Voorkeuren voor do-not-call van lead |
| doNotCallReason | DoNotCallReason | Reden voor niet bellen | Uitleg voor do-not-call voorkeur van lood |
| email | E-mail | E-mailadres | E-mailadres van lead. Standaard Marketo-sleutelveld voor lead records |
| fax | Fax | Faxnummer | Faxnummer van de lead |
| firstName | FirstName | Voornaam | Voornaam van lead |
| industrie | Marktsegment | Marktsegment | De industrie van de leider |
| inferredCompany | InferredCompany | Afgeleid bedrijf | De naam van het bedrijf die door omgekeerde IP raadpleging van het eerste geregistreerde Webbezoek van de lood wordt afgeleid |
| inferredCountry | InferredCountry | Afgeleid land | Land dat wordt afgeleid door omgekeerde IP-zoekopdracht van het eerste opgenomen webbezoek van de lead |
| lastName | LastName | Achternaam | Achternaam van lead |
| leadRole | LeadRole | Functie | Rol van de leider in hun bedrijf |
| leadScore | LeadScore | Leadscore | Gehele score toegekend aan de leider door scènes en programma&#39;s |
| leadSource | LeadSource | Leadbron | Veld waarin wordt vastgelegd van welke bron de lead afkomstig is |
| leadStatus | LeadStatus | Leadstatus | Veld waarin de huidige marketing-/verkoopstatus van de lead wordt geregistreerd |
| mainPhone | MainPhone | Telefoon | Primair telefoonnummer van het bedrijf van de lead |
| jigsawContactId | Marketo Jigzaag-contactpersoon | MARKETO Data.com ID | De Data.com-id van de lead, indien beschikbaar |
| jigsawContactStatus | Marketo Jigzaag-contactstatus | Marketo Data.com Status | Status Data.com van lead, indien beschikbaar |
| facebookDisplayName | MarketoSocialFacebookDisplayName | Marketo Social Facebook Display Name | De Facebook-weergavenaam van de leider. Systeem gevuld tijdens aanmelden via sociale media |
| facebookId | MarketoSocialFacebookId | Marketo Social Facebook-id | Facebook-id van lead. Systeem gevuld tijdens aanmelden via sociale media |
| FacebookPhotoURL | MarketoSocialFacebookPhotoURL | Marketo Social Facebook Foto-URL | URL van Facebook-profielfoto van lead. Systeem gevuld tijdens aanmelden via sociale media |
| facebookProfileURL | MarketoSocialFacebookProfileURL | Marketo Social Facebook Profile URL | URL van Facebook-profiel van lead. Systeem gevuld tijdens aanmelden via sociale media |
| facebookReach | MarketoSocialFacebookReach | Marketo Social Facebook Reach | Het Facebook-bereik van de leider. Systeem gevuld tijdens aanmelden via sociale media |
| facebookReferredEnrollments | MarketoSocialFacebookReferredEnrollments | Inschrijvingen verwezen naar Marketo Social Facebook | Het aantal inschrijvingen waarnaar wordt verwezen in de lead via Facebook. Systeembeheer |
| facebookReferredVisits | MarketoSocialFacebookReferredVisits | Bezoeken op Marketo Social Facebook | Het aantal doorverwezen bezoeken dat via Facebook aan de lead wordt toegeschreven. Systeembeheer |
| sekse | MarketoSocialGender | Marketo Social Gender | Geslacht van de leider. Systeem gevuld tijdens aanmelden via sociale media |
| lastReferredEnrollment | MarketoSocialLastReferredEnrollment | Inschrijving als laatste verwijzing voor Marketo Social | Datum van laatste voltooide verwijzing. Systeembeheer |
| lastReferredVisit | MarketoSocialLastReferredVisit | Bezoek van laatst vermelde Marketo Social | Datum van het laatst doorverwezen bezoek. Systeembeheer |
| linkedInDisplayName | MarketoSocialLinkedInDisplayName | Naam Marketo Social LinkedIn-weergave | Weergavenaam LinkedIn van de lead. Systeem gevuld tijdens aanmelden via sociale media |
| linkedInId | MarketoSocialLinkedInId | Marketo Social LinkedIn-id | LinkedIn-id van lead. Systeem gevuld tijdens aanmelden via sociale media |
| linkedInPhotoURL | MarketoSocialLinkedInPhotoURL | Marketo Social LinkedIn-foto-URL | URL van LinkedIn-foto van lead. Systeem gevuld tijdens aanmelden via sociale media |
| linkedInProfileURL | MarketoSocialLinkedInProfileURL | Marketo URL sociaal gekoppeld profiel | LinkedIn-profiel van lead. Systeem gevuld tijdens aanmelden via sociale media |
| linkedInReach | MarketoSocialLinkedInReach | Marketo Social LinkedIn Reach | LinkedIn bereik van de lead. Systeem gevuld tijdens aanmelden via sociale media |
| linkedInReferredEnrollments | MarketoSocialLinkedInReferredEnrollments | Marketo Social LinkedIn-inschrijvingen waarnaar wordt verwezen | Het aantal referenties dat via LinkedIn aan de lead wordt toegewezen. Systeembeheer |
| linkedInReferredVisits | MarketoSocialLinkedInReferredVisits | Marketo Social LinkedIn bezoeken waarnaar wordt verwezen | Het aantal met LinkedIn verband gebrachte bezoeken dat aan de lead is toegewezen. Systeembeheer |
| syndicationId |  - | Marketo Social Syndication ID | Interne Marketo Sociale ID van lead. Systeembeheer |
| totalReferredEnrollments | MarketoSocialTotalReferredEnrollments | Marketo Social Total Vermeld inschrijvingen | Totaal aantal voltooide verwijzingsinschrijvingen toegeschreven aan de lead |
| totalReferredVisits | MarketoSocialTotalReferredVisits | Marketo Sociale Totaal Aangewezen bezoeken | Totaal aantal aan de lead toegewezen referentiebezoeken |
| twitterDisplayName | MarketoSocialTwitterDisplayName | Marketo Sociale Twitter-weergavenaam | Weergavenaam Twitter van leider. Systeem gevuld tijdens aanmelden via sociale media |
| twitterId | MarketoSocialTwitterId | Marketo Sociale Twitter-id | Twitter-id van leider. Systeem gevuld tijdens aanmelden via sociale media |
| twitterPhotoURL | MarketoSocialTwitterPhotoURL | Marketo Social Twitter-foto-URL | URL Twitter-foto van leider. Systeem gevuld tijdens aanmelden via sociale media |
| twitterProfileURL | MarketoSocialTwitterProfileURL | URL van sociaal Twitter-profiel voor Marketo | URL Twitter-profiel van leider. Systeem gevuld tijdens aanmelden via sociale media |
| twitterReach | MarketoSocialTwitterReach | Marketo Social Twitter Reach | Twitter-bereik van leider. Systeem gevuld tijdens aanmelden via sociale media |
| twitterReferredEnrollments | MarketoSocialTwitterReferredEnrollments | Inschrijvingen op sociale Twitter in Marketo | Aantal referenties dat via Twitter aan de lead is toegewezen. Systeembeheer |
| twitterReferredVisits | MarketoSocialTwitterReferredVisits | Bezoeken op sociale Twitter in Marketo | Het aantal doorverwezen bezoeken dat via Twitter aan de lead is toegeschreven. Systeembeheer |
| middleName | MiddleName | Tussenvoegsel | Middennaam van lead |
| mobilePhone | Mobiele telefoon | Mobiel | Mobiel telefoonnummer van lead |
| numberOfEmployees | NumberOfEmployees | Aantal werknemers | Aantal werknemers van hoofdbedrijf |
| telefoon | Telefoonnummer | Telefoonnummer | Telefoonnummer van lead |
| postcode | Postcode | Postcode | Postcode van lead |
| beoordeling | Classificatie | Leadrating | Marketing-/verkoopclassificatie van de lead |
| aanhef | Aanhef | Aanhef | De voorkeursalutatie van Lead, d.w.z. Mister, Misses enz. |
| sicCode | SICCode | SIC-code | Standard Industrial Classification code of the lead&#39;s company |
| site | Vestiging | Vestiging |  |
| state | Staat | Staat | Loodstaat |
| titel | Titel | Beroep | Functie van leider |
| geabonneerd | Niet geabonneerd | Niet geabonneerd | e-mailstatus van lead. Gedeeltelijk beheerd systeem. Hiermee voorkomt u dat niet-operationele e-mailberichten worden ontvangen als deze zijn ingesteld op true. |
| unsubscribedReason | UnsubscribedReason | Reden voor niet geabonneerd | Reden voor de status waarop de lead zich niet heeft geabonneerd. Gedeeltelijk beheerd systeem. Deze groep wordt gevuld met e-mailgegevens als de lead direct wordt afgemeld via een Marketo-e-mail. |
| website | Website | Website | URL van de website van het bedrijf van de lead |
| createdAt |  - | Gemaakt op | De tijd waarop de lead record voor de eerste keer is gemaakt. Systeembeheer |
| updatedAt |  - | Bijgewerkt op | De laatste keer dat de lead record is bijgewerkt. Systeembeheer |
| emailInvalid |  - | E-mail ongeldig | Ongeldige status e-mailadres. Alle e-mails naar het adres worden geblokkeerd als deze op true zijn ingesteld. Bounces die aangeven dat de e-mail ongeldig is, stellen dit veld automatisch in op true. |
| emailInvalidCase |  - | Ongeldige oorzaak e-mail | Ongeldige status van oorzaak van e-mailfout. Het aanroepende bericht wordt in dit veld opgenomen wanneer de ongeldige e-mail is ingesteld op true. |
| inferredCity |  - | Overgenomen stad | De stad van de leider die door omgekeerde IP raadpleging van het eerste geregistreerde Webbezoek van lood wordt afgeleid. |
| inferredMetropolitanArea |  - | Overgenomen metropolitaans gebied | Het metropolitane gebied van lood wordt afgeleid door omgekeerde IP raadpleging van het eerste geregistreerde Webbezoek van lood. |
| interferredPhoneAreaCode |  - | Gebiedscode afgeleide telefoon | De code van het telefoongebied van de leider die door omgekeerde IP raadpleging van het eerste geregistreerde Webbezoek van lood wordt afgeleid. |
| inferredPostalCode |  - | Postcode | De postcode van de leider die door omgekeerde IP raadpleging van het eerste geregistreerde Webbezoek van lood wordt afgeleid. |
| inferredStateRegion |  - | Gebied van de betrokken staat | De staatsregio van de leider die door omgekeerde IP raadpleging van het eerste geregistreerde Webbezoek van lood wordt afgeleid. |
| isAnonymous |  - | Is anoniem | Anonieme status van lead record. Systeem beheerd. |
| prioriteit |  - | Prioriteit | Prioriteit Verkoopmanager Insight. Systeem beheerd. |
| relativeScore |  - | Relatieve score | Relatieve score van Verkoopmanager Insight. Systeem beheerd. |
| urgentie |  - | Urgentie | Verkoopmanager Insight-urgentie. Systeem beheerd. |
