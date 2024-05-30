---
title: "Veelgestelde vragen over SOAP"
feature: SOAP
description: "Veelgestelde vragen over SOAP"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---


# Veelgestelde vragen over SOAP

**V:** Hoe krijg ik een lijst van alle Programma&#39;s binnen Marketo samen met hun meta gegevens?

**A:** Als u een lijst met alle programma&#39;s wilt ophalen, kunt u [getMObjects](./getmobjects.md) Het overgaan van het type gelijk aan &quot;Programma&quot;en het plaatsen includeDetails aan waar.

**V:** Is er een manier om de prestaties van getMultipleLeads te versnellen?

**A:** Er zijn een paar opties om de prestaties van de getMultipleLeads vraag te versnellen. De eerste moet batchSize verminderen u voor elke vraag vraagt. De aanbevolen batchgrootte is 200. De tweede optie is het opgeven van de velden waarin u interesse hebt voor het gebruik van het filter includeAttributes. Dit versnelt de vraag en vermindert de lading van de reactie u ontvangt. De definitieve benadering is LastUpdateAtSelector te gebruiken en oldestUpdatedAt en latestUpdatedAt te specificeren. U kunt verschillende datumbereiken specificeren en dan veelvoudige verzoeken gelijktijdig verbinden. Als het gebruiken van de verbonden benadering ervoor zorgt dat uw cliënt SOAP/WSDL steunt [permanente verbindingen](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html).

**V:** Hoe kan ik Kansen via de ZEEP API tot stand brengen wanneer niet geïntegreerd met CRM zoals SalesForce of de Dynamica van Microsoft?

**A:** U kunt Opportunity maken met de SOAP API [syncMObjects](syncmobjects.md) het schrijven aan OpportunityPersonRole en Opportunity [MObject](marketo-objects.md) typen.

**V:** Kan ik via programmacode een e-mail sturen vanuit Marketo? Zo ja, hoe kan ik aangepaste inhoud voor elke e-mailontvanger verzenden?

**A:** Absoluut. U kunt een verzoek indienen om een e-mailbericht van Marketo te verzenden via de [requestCampaign](requestcampaign.md) of combinatie van [importToList](importtolist.md) en [planningCampaign](schedulecampaign.md) SOAP-API&#39;s. Als u onmiddellijk een e-mail naar een of meer personen wilt verzenden, gebruikt u [requestCampaign](requestcampaign.md). Als u een e-mailbericht wilt plannen dat op een bepaalde datum en tijd moet worden verzonden, gebruikt u [importToList](importtolist.md) om de ontvangers van de e-mail te specificeren, en [planningCampaign](schedulecampaign.md) om aan te geven wanneer deze ontvangers die e-mail zullen ontvangen.

Als u de inhoud voor elke e-mailontvanger wilt aanpassen, kunt u dit doen door de waarden van [programmatietokens](../rest-api/tokens.md) die zijn ingesteld in de e-mailsjabloon.
