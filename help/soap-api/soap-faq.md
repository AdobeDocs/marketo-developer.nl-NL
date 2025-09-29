---
title: Veelgestelde vragen over SOAP
feature: SOAP
description: Leer hoe u programma's kunt weergeven met getMObjects, getMultipleLeads kunt optimaliseren, mogelijkheden kunt creëren en persoonlijke e-mails kunt verzenden of plannen via de Marketo SOAP API.
exl-id: a2d8f144-cd5f-41bc-8231-29c42af935b8
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '353'
ht-degree: 0%

---

# Veelgestelde vragen over SOAP

**Q:** Hoe kan ik een lijst van alle Programma&#39;s binnen Marketo samen met hun meta-gegevens krijgen?

**A:** om een lijst van alle programma&#39;s terug te winnen, kunt u [ getMObjects ](./getmobjects.md) gebruiken die het type evenaart aan &quot;Programma&quot;en plaatsen includeDetails aan waar.

**Q:** is er een manier om de prestaties van getMultipleLeads te versnellen?

**A:** Er zijn een paar opties om de prestaties van de vraag te versnellen getMultipleLeads. De eerste moet batchSize verminderen u voor elke vraag vraagt. De aanbevolen batchgrootte is 200. De tweede optie is het opgeven van de velden waarin u interesse hebt voor het gebruik van het filter includeAttributes. Dit versnelt de vraag en vermindert de lading van de reactie u ontvangt. De definitieve benadering is LastUpdateAtSelector te gebruiken en oldestUpdatedAt en latestUpdatedAt te specificeren. U kunt verschillende datumbereiken specificeren en dan veelvoudige verzoeken gelijktijdig verbinden. Als het gebruiken van de verbonden benadering ervoor zorgt dat uw cliënt SOAP/WSDL [ blijvende verbindingen ](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html) steunt.

**Q:** Hoe kan ik Kansen via SOAP API tot stand brengen wanneer niet geïntegreerd met CRM zoals Salesforce of Microsoft Dynamics?

**A:** u kunt Kansen tot stand brengen gebruikend SOAP API gebruikend de [ syncMObjects ](syncmobjects.md) vraag die aan OpportunityPersonRole en Opportunity [ MObject ](marketo-objects.md) types schrijft.

**Q:** Kan ik programmatically een e-mail van Marketo verzenden? Zo ja, hoe kan ik aangepaste inhoud voor elke e-mailontvanger verzenden?

**A:** Absoluut. U kunt om een e-mail verzoeken om van Marketo worden verzonden gebruikend of [ requestCampaign ](requestcampaign.md) of combinatie [ importToList ](importtolist.md) en [ planningCampaign ](schedulecampaign.md) SOAP APIs. Om een e-mail naar één of meerdere mensen onmiddellijk te verzenden, zou u [ requestCampaign ](requestcampaign.md) gebruiken. Als u een e-mail wilt plannen die op een gespecificeerde datum en tijd worden verzonden u [ importToList ](importtolist.md) zou gebruiken om de ontvangers van e-mail te specificeren, en [ planningCampaign ](schedulecampaign.md) om te specificeren wanneer die ontvangers die e-mail zullen worden verzonden.

Als u de inhoud voor elke e-mailontvanger wilt aanpassen, kunt u dit doen door de waarden van [ programmatokens ](../rest-api/tokens.md) met voeten te treden die binnen het e-mailmalplaatje worden geplaatst.
