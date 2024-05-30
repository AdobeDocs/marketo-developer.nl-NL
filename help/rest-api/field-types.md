---
title: "Veldtypen"
feature: REST API
description: "Een lijst met Marketo-veldtypen"
source-git-commit: fd75f60adbc4d38e4743db5447d15cf90f025e22
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 2%

---


# Veldtypen

Hier volgt een beschrijving van veldtypen in Marketo. Aanvullende informatie over veldtypen vindt u [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/custom-field-type-glossary). Aanvullende informatie over veldtypegrenzen vindt u [hier](https://nation.marketo.com/t5/knowledgebase/tkb-p/support_solutions-documents).

| Veldtype | Beschrijving | Voorbeeld |
| --- | --- | --- |
| Datumtijd | Wordt gebruikt voor het invoeren van een datum en tijd. Volgt [W3C-indeling](https://www.w3.org/TR/NOTE-datetime) (ISO 8601) De beste praktijken moeten altijd de compensatie van de tijdzone omvatten. Volledige datum plus uren en minuten: JJJJ-MM-DDTh:mm:ssTZD waarbij TZD &quot;+hh:mm&quot; of &quot;-hh:mm&quot; is Opmerking: sommige API&#39;s voor middelen retourneren &quot;Z+0000&quot; als TZD voor updatedAt en createdAt. | 2010-05-07T15:41:32-05:00 |
| E-mail | Een tekenreeksveld dat e-mailadressen accepteert | example@example.com |
| Float | Een getalveld dat echte getallen bevat en een decimale waarde kan gebruiken. | 10,4 |
| Geheel | Hele getallen | 10 |
| Formule | Velden waarvan de waarden worden gegenereerd door gegevens te manipuleren uit andere velden in een lead record. Ze worden niet geëxporteerd en kunnen niet worden gebruikt in slimme campagnes. | Zie dit [artikel](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/create-and-use-a-concatenated-string-formula-field) |
| Percentage | Een percentage uitgedrukt als een geheel getal | 30 |
| URL | Een tekstveld dat invoer beperkt tot URL&#39;s, inclusief het protocol van de URL. | http://example.com/ |
| Telefoonnummer | Telefoonnummer | 111-111-111 |
| Tekstgebied | Langere tekst. | Ondersteunt maximaal 30.000 bytes. Standaard ASCII-tekens gebruiken 1 byte per teken (maximaal 30.000 tekens). Unicode-tekens kunnen maximaal 4 bytes per teken gebruiken (waarbij het aantal toegestane tekens wordt verminderd tot minder dan 30.000 tekens). |
| String | Kortere tekst (maximaal 255 tekens) | Lorem ipsum dolor sit amet |
| Score | Een geheel-getalveld dat met de de stroomstap van de Score van de Verandering kan worden gemanipuleerd | 10 |
| Boolean (voorheen Selectievakje) | Hiermee kunnen gebruikers een waarde voor Waar (ingeschakeld) of Onwaar (uitgeschakeld) selecteren. | Waar |
| Valuta | Een zwevend veld dat staat voor het standaardvalutatype dat is geselecteerd voor het Marketo-abonnement | 10,40 |
| Datum | Wordt gebruikt voor datum. volgt de W3C-indeling. | 05-07-2010 |
| Referentie | Een tekenreeksveld met een sleutel tot een andere record (een buitenlandse sleutel). | Contactpersoon |
