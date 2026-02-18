---
title: Standaardvelden
feature: REST API, Field Management
description: Blader door de volledige lijst met Marketo-standaardloodvelden met REST- en SOAP-namen, -labels en -beschrijvingen, en hoe u deze ophaalt via de Describe Lead-API.
exl-id: 147dbdff-4bc9-4ab3-8918-c4de3e1aa97a
source-git-commit: d674384b3ab979df2322ece3f02155259d05431a
workflow-type: tm+mt
source-wordcount: '727'
ht-degree: 12%

---

# Standaardvelden

Hier volgt een lijst met standaardvelden die beschikbaar zijn in Marketo en die toegankelijk zijn via de API.

U kunt de lijst van alle gesteunde gebiedsnamen terugwinnen beschikbaar op uw loodverslagen door REST [&#x200B; te gebruiken beschrijf lood &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/) eindpunt.

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
