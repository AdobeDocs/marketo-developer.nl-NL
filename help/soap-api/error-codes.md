---
title: Foutcodes
feature: SOAP
description: Referentiegids voor Marketo SOAP API-foutcodes met berichten en notities, die betrekking heeft op verificatiefouten, frequentie- en gelijktijdige limieten en aanvraagproblemen.
exl-id: 71796520-7bd6-4a37-94e7-b073d17df06f
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 6%

---

# Foutcodes

Wanneer het ontwikkelen voor Marketo, is het zeer belangrijk dat de verzoeken en de reacties het programma worden geopend wanneer een onverwachte uitzondering wordt ontmoet.  Terwijl bepaalde soorten uitzonderingen, zoals verlopen authentificatie veilig door re-authentificatie kunnen worden behandeld, kunnen anderen steuninteractie vereisen, en de verzoeken en de reacties zullen altijd in dit scenario worden gevraagd.

Hieronder vindt u een lijst met SOAP API-foutcodes.

| Code | Bericht | Notities |
|--- |--- |--- |
| 10001 | Interne fout | Ernstig systeemfalen |
| 20011 | Interne fout | API-servicefout |
| 20012 | Verzoek niet begrepen | Onverwacht SOAP-bericht |
| 20013 | Toegang geweigerd | Client blokkeert API-toegang |
| 20014 | Verificatie mislukt | Client heeft geen geldige gegevens opgegeven |
| 20015 | Limiet verzoek overschreden | Het aantal gesprekken heeft vandaag de quota van het abonnement overschreden. Het standaardabonnementsquotum is 10.000 per dag. |
| 20016 | Verzoek verlopen | Handtekening aanvragen is te oud. De opgegeven tijdstempel en aanvraaghandtekening bevinden zich in het verleden en zijn niet meer geldig. Het verzoek kan opnieuw worden uitgevoerd met een nieuw gegenereerde tijdstempel en handtekening. |
| 20017 | Ongeldig verzoek | Verzoek ontbreekt een verwachte parameter |
| 20019 | Niet-ondersteunde bewerking | De aangeroepen bewerking is niet gedefinieerd in de Marketo API WSDL |
| 20022 | Tijdbereik opgegeven in queryfilter overschrijdt limiet | Het aantal dagen dat is verstreken tussen de velden &quot;oldestUpdatedAt&quot; en &quot;latestUpdatedAt&quot; was groter dan 30 |
| 20023 | Snelheidslimiet overschreden | Het aantal vraag in de afgelopen 20 seconden was groter dan 100 |
| 20024 | Gelijktijdige limiet overschreden | Het aantal gezamenlijke vraag was groter dan 10 |
| 20101 | Leadtoets vereist | LeadKey is vereist maar is niet opgegeven |
| 20102 | Leadsleutel is onjuist | LeadKeyType is niet geldig |
| 20103 | Geen lood gevonden | De LeadKey-waarde komt niet overeen met een lead |
| 20104 | Leadgegevens vereist | LeadRecord is vereist maar is niet opgegeven |
| 20105 | Leadkenmerk onjuist | LeadRecord bevat een kenmerk met een ongeldige naam |
| 20106 | Synchronisatie van lead mislukt | LeadRecord kan niet worden bijgewerkt of gemaakt |
| 20107 | Activiteitensleutel is onjuist | LeadActivityFilter bevat een type slechte activiteit |
| 20108 | Eigenaar van lead niet gevonden | LeadKey geeft een eigenaar van een lead aan die niet bestaat |
| 20109 | Parameter vereist | Parameterwaarde is null of ontbreekt |
| 20110 | Ongeldige parameter | Een parameterwaarde is ongeldig |
| 20111 | Lijst niet gevonden | ListKey geeft een lijst aan die niet bestaat |
| 20113 | Campagne niet gevonden | Campagne bestaat niet |
| 20114 | Ongeldige parameter | Parameterwaarde is ongeldig |
| 20122 | Ongeldige streampositie | Stroompositie is ongeldig |
| 20123 | Streamen aan einde | De stroompositie geeft aan dat er geen records meer beschikbaar zijn |
