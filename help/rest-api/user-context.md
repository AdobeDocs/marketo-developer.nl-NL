---
title: Gebruikerscontext
feature: REST API
description: Leer hoe u de Marketo RTP-gebruikerscontext-API kunt inschakelen en gebruiken om aangepaste variabelen in te stellen, gebruikersgegevens te lezen tijdens bezoeken en bekeken en geklikte campagnes te volgen.
exl-id: b8daace2-07a5-4621-aa3a-03fa9f66ea73
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 1%

---

# Gebruikerscontext

De context JavaScript API van de gebruiker stelt gebruiker en bezoekersniveau gegevens over veelvoudige zittingen bloot om geavanceerde verpersoonlijkingsvermogen toe te laten gebruikend historisch gebruikersgedrag en gegevens. De API gaat voorbij gegevens lezen en stelt douanevariabelen bloot die u toestaan om zinvolle gegevens en gebeurtenissen aan de achtergrond RTP voor geavanceerde segmentatie en verpersoonlijkingsdoeleinden te duwen. Extra mogelijkheden: [&#x200B; Trekkers &#x200B;](../javascript-api/triggers.md), [&#x200B; de Gelijke van het Patroon &#x200B;](../javascript-api/pattern-match.md).

- U moet een klant van Personalization van het Web worden en de [&#x200B; markering hebben RTP die &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) op uw plaats wordt opgesteld alvorens de Context API van de Gebruiker te gebruiken.
- De gebruikerscontext-API is een functie die op verzoek door Marketo Support moet worden ingeschakeld. Wanneer API wordt toegelaten, zal een userContext voorwerp onder het globale voorwerp RTP worden blootgesteld.

## Contextkenmerken gebruiker

| Naam | Type | Beschrijving |
|------------------|-------------|------|
| customVar [ 1-5 ] | String | Aangepaste gegevens opgeslagen in de gebruikerscontext. |
| viewsCampagnes | Campagne-id&#39;s als door komma&#39;s gescheiden tekenreeks | Bekeek campagnes tijdens huidige of vorige bezoeken. |
| clickedCampaigns | Campagne-id&#39;s als door komma&#39;s gescheiden tekenreeks | Klik door campagnes in huidige of vorige bezoeken. |

## Aangepaste variabelen instellen

Aangepaste gegevens toevoegen aan gebruikerscontext.

### Gebruik

`rtp('set', 'customVar'[1-5], my_custom_value);`

| Parameter | Optioneel/vereist | Type | Beschrijving |
|-----------------|-------------------|--------|-----------------|
| &#39;set&#39; | Vereist | String | Methode, actie. |
| customVar | Vereist | String | Naam van aangepaste variabele. |
| my_custom_value | Vereist | String | Aangepaste waarde om op te slaan op aangepaste variabele in index 1-5. |

Nota: De variabelen van de douane worden verzonden naar RTP slechts in meningsvraag, zodat wordt het geadviseerd om douanevariabelen te plaatsen alvorens de mening wordt geroepen. Anders, zal het slechts in volgende meningsvraag worden verzonden.

Aangepaste Var-beperkingen

- Aangepaste variabele lengte mag niet langer zijn dan 100 tekens.
- Campagnegegevens zijn beperkt tot de laatste tien bezoeken met tien campagnes per bezoek.

### Gebruik

`rtp('set', 'customVar', 'A');`

```javascript
// Set and get customVars
rtp('set', 'customVar1', 'foo');

// Read location
if (rtp.userContext.location.state == 'CA')  {
    // Do something
}

// Check if user viewed campaign id 45:
// The campaign id is exposed in the RTP UI when hovering over a campaign name.
if (rtp.userContext.viewedCampaign('45')) {
    // Do something
}
```
