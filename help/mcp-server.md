---
title: MCP-server
description: Leer hoe u een AI-assistent met Marketo verbindt via de MCP-server. Configureer Claude Desktop, Cursor, Claude Code of VS Code met uw Marketo-referenties.
hidefromtoc: true
badgeBeta: label="Beta" type="informative" tooltip="Deze functie bevindt zich momenteel in een gesloten bètaversie"
exl-id: ab446e56-6250-4af5-b03e-162991d09a5c
source-git-commit: c21ba0db3115c453f8ec35e18d4a8fd4c1ad8745
workflow-type: tm+mt
source-wordcount: '1388'
ht-degree: 0%

---

# [!DNL Marketo] MCP-server

>[!NOTE]
>
>De server MCP is momenteel in een gesloten bètaversie. Deze is momenteel niet voor alle gebruikers beschikbaar.

Het ModelProtocol van de Context (MCP) is een open norm die AI hulpmiddelen om met de externe diensten toelaat te communiceren. De [!DNL Marketo] MCP-server fungeert als een brug tussen uw AI-assistent en [!DNL Marketo] . Er worden meer dan 100 bewerkingen in verschillende formulieren, programma&#39;s, slimme campagnes, leads, e-mails, fragmenten, lijsten en mappen weergegeven.

Wanneer uw AI hulpmiddel de server MCP roept, voert de server de overeenkomstige REST API vraag op uw naam uit, gebruikend de geloofsbrieven u in elk verzoek ingaat. U hoeft geen software op de server te installeren, implementeren of uitvoeren.

>[!IMPORTANT]
>
>Het ModelContextprotocol (MCP) is een nieuwe open-bronnorm en kan veiligheid of betrouwbaarheidsrisico&#39;s voorstellen. Adobe MCP server integrations en verwante documentatie worden verstrekt &quot;zoals is,&quot;zonder enige garanties van welke aard ook.
>Het verbinden van cliënten MCP of servers met de producten van Adobe is een klant-verkozen configuratie, en de klanten zijn verantwoordelijk voor het evalueren van de veiligheid en de geschiktheid van om het even welke integratie MCP. Adobe is niet verantwoordelijk voor problemen die voortvloeien uit verkeerde configuratie, misbruik van MCP, kwetsbaarheden in derdeimplementaties, of onbedoelde acties die via MCP-Toegelaten werkschema&#39;s worden uitgevoerd.
>Om risico&#39;s te verminderen, moedigt Adobe het testen van integratie in een zandbakmilieu voorafgaand aan productief gebruik aan en zorgvuldig het herzien en het bevestigen van alle MCP-Gerichte acties en reacties alvorens hen te bevestigen of te vertrouwen.

## Vereisten

- Een [!DNL Marketo] -instantie met REST API-toegang ingeschakeld
- Beheerdersrechten kunnen API-referenties maken in [!DNL Marketo] LaunchPoint
- Één van de volgende hulpmiddelen van AI: Desktop, Cursor, de Code van Claude (CLI), of de Code van VS met Copilot van GitHub
- Netwerktoegang tot de URL van de MCP-server: `https://marketo-mcp.adobe.io/mcp`

## Marketo-referenties ophalen

U hebt de volgende waarden van uw [!DNL Marketo] -instantie nodig:

- **identiteitskaart van de Cliënt**
- **Geheim van de Cliënt**
- **identiteitskaart van de Rekening van Munchkin**

Als u reeds hen hebt, overslaan aan [ vorm uw AI hulpmiddel ](#configure-your-ai-tool).

### Client-id en clientgeheim

1. Ga naar **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]** .
1. Klik op uw API-service. Als u er geen hebt, selecteert u **[!UICONTROL New]** > **[!UICONTROL New Service]** , kiest u **[!UICONTROL Custom]** als het servicetype en wijst u een toegewijde API-gebruiker toe.
1. Klik op **[!UICONTROL View Details]** en kopieer de waarden **[!UICONTROL Client ID]** en **[!UICONTROL Client Secret]** .

### Munchkin-account-id

1. Ga naar **[!UICONTROL Admin]** > **[!UICONTROL Munchkin]** .
1. Kopieer de **[!UICONTROL Munchkin Account ID]** . De indeling is `XXX-XXX-XXX` en komt overeen met het voorvoegsel van de instantie-URL.

## Uw AI-gereedschap configureren

Elk AI hulpmiddel leest MCP serverconfiguratie van een verschillende plaats. Zoek hieronder uw gereedschap en volg de stappen om de [!DNL Marketo] MCP-server toe te voegen.

### Claude Desktop

Het configuratiebestand is `claude_desktop_config.json` . Open de toepassing op een van de volgende locaties:

- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Vensters**: `%APPDATA%\Claude\claude_desktop_config.json`
- **Linux**: `~/.config/Claude/claude_desktop_config.json`

Als het bestand al andere MCP-servers bevat, voegt u de vermelding `marketo` onder `mcpServers` toe. In het volgende voorbeeld wordt het volledige `mcpServers` -blok getoond:

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
      }
    }
  }
}
```

Sla het bestand op, sluit Claude Desktop af en open het opnieuw.

### Cursor

Als uw MCP-configuratie van Cursor al andere servers bevat, voegt u de `marketo` -vermelding toe onder `mcpServers` . In het volgende voorbeeld wordt het volledige `mcpServers` blok in **[!UICONTROL Settings]** > **[!UICONTROL MCP]** of `.cursor/mcp.json` in de projectmap getoond:

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
      }
    }
  }
}
```

Start de cursor opnieuw.

### Claude Code (CLI)

Voer het volgende bevel in uw terminal in werking, die uw geloofsbrieven vervangt:

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "X-Marketo-Client-Id: YOUR-CLIENT-ID" \
  --header "X-Marketo-Client-Secret: YOUR-CLIENT-SECRET" \
  --header "X-Marketo-Munchkin-Id: YOUR-MUNCHKIN-ID"
```

### VS-code met GitHub Copilot

Open uw VS-code `settings.json` door op **[!UICONTROL Ctrl+Shift+P]** of **[!UICONTROL Cmd+Shift+P]** op macOS te drukken en vervolgens **[!UICONTROL Preferences: Open User Settings (JSON)]** te selecteren. Voeg het volgende voorbeeld toe:

```json
{
  "mcp": {
    "servers": {
      "marketo": {
        "type": "http",
        "url": "https://marketo-mcp.adobe.io/mcp",
        "headers": {
          "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
          "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
          "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
        }
      }
    }
  }
}
```

Druk op **[!UICONTROL Ctrl+Shift+P]** (of **[!UICONTROL Cmd+Shift+P]** op macOS), typ **[!UICONTROL Reload Window]** en druk op Enter.

>[!NOTE]
>
>Voor veiligheidsdoeleinden, gebruik milieu veranderlijke interpolatie in configuratiedossiers in plaats van het kleven van geloofsbrieven direct. U kunt naar variabelen verwijzen met syntaxis als `${MARKETO_CLIENT_SECRET}` en deze in uw omgeving instellen. Dit verhindert geloofsbrieven in gewone teksten in dossiers worden opgeslagen die aan versiecontrole kunnen worden begaan.

## Beschikbare bewerkingen

Zodra verbonden, kunt u uw AI medewerker vragen om verrichtingen over de volgende categorieën uit te voeren.

### Forms

Bladeren, maken, klonen en goedkeuren van formulieren. U kunt velden toevoegen of verwijderen, zichtbaarheidsregels voor velden configureren en bepalen waar formulieren worden ingesloten.

Voorbeeld vraagt:

- &quot;Alle goedgekeurde formulieren weergeven&quot;
- &quot;Clone the Contact US form into the Q2 Campaign folder&quot;
- &quot;Voeg een gebied van het Bedrijf aan de vorm van het Verzoek van de Demo toe&quot;

### Slimme campagnes

Maak slimme campagnes, configureer filters voor slimme lijsten, voeg flowstappen toe en activeer of deactiveer campagnes.

Voorbeeld vraagt:

- &quot;Welke slimme campagnes zijn nu actief?&quot;
- &quot;Maak een nieuwe slimme campagne met de naam Update voor het weergeven van leads in de map Operations&quot;
- &quot;Toon me de stroomstappen in de Welkome E-mailcampagne&quot;

### Leads en lijsten

Leden zoeken op e-mailadres, leadrecords maken of bijwerken en lidmaatschap van statische lijsten beheren.

Voorbeeld vraagt:

- &quot;Zoek de lead met e-mail jane@example.com&quot;
- &quot;Voeg loodnummer 12345 toe aan de lijst Q2 MQL.&quot;
- &quot;Maak een nieuwe statische lijst met de naam Zomergebeurtenisdeelnemers&quot;

### Programma&#39;s

Maak, kloon en tagprogramma&#39;s. Blader door programma&#39;s op type, kanaal of datumbereik.

Voorbeeld vraagt:

- &quot;Clone the Q4 Webinar program into the 2026 Events folder&quot;
- &quot;Maak een nieuw e-mailprogramma met de naam Summer Sale in de map Campaigns&quot;
- &quot;Toon me alle programma&#39;s die als Webinar worden geëtiketteerd&quot;

### E-mails en fragmenten

Blader door e-mailberichten, maak e-mails van sjablonen, werk inhoudssecties bij en beheer herbruikbare fragmenten.

Voorbeeld vraagt:

- &quot;Alle e-mailconcepten weergeven&quot;
- &quot;De koptekstsectie van het welkomstbericht bijwerken&quot;
- &quot;Welke middelen gebruiken het fragment van de Promo van de Vakantie?&quot;

### Instantiestructuur

Blader door mappen, kanalen, typen tags en typen activiteiten om uw [!DNL Marketo] -configuratie te begrijpen.

Voorbeeld vraagt:

- &quot;Alle mappen weergeven in Marketo&quot;
- &quot;Alle beschikbare kanalen weergeven&quot;
- &quot;Welke markeringstypes worden gevormd?&quot;

### Bulkbewerkingen

Gegevens voor lead bulksgewijs exporteren en status van import- of exporttaak controleren.

Voorbeeld vraagt:

- &quot;Maak een grote export van leads die in de afgelopen 30 dagen zijn gemaakt&quot;
- &quot;De status van exporttaak xx controleren&quot;

## Problemen oplossen

| Fout | Oorzaak | Repareren |
| ------- | ------- | ----- |
| &quot;Marketo credentials not provided&quot; | Een of meer van `X-Marketo-Client-Id`, `X-Marketo-Client-Secret` of `X-Marketo-Munchkin-Id` ontbreken. | Verifieer alle vier kopballen in uw configuratie aanwezig zijn. |
| &quot;Verificatiefout&quot; | Uw referenties zijn ongeldig of verlopen. | Controleer uw client-id en clientgeheim opnieuw in **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]** . |
| &quot;403 Verboden&quot; | Je Munchkin-id staat niet op de lijst van gewenste personen van de server. | Neem contact op met de [!DNL Marketo] MCP-beheerder om uw Munchkin-id toe te voegen. |
| Time-out verbinding of geweigerd | De server MCP is onbereikbaar van uw netwerk. | Bevestig dat u de server-URL vanuit uw omgeving kunt bereiken. Controleer de VPN-vereisten, indien van toepassing. |
| De vraag van het hulpmiddel keert lege resultaten terug | De API-gebruiker heeft geen machtigingen voor het aangevraagde elementtype. | Vraag uw [!DNL Marketo] -beheerder om de API-gebruikersrol en -machtigingen te controleren. |

## Veelgestelde vragen

+++Zijn mijn gegevens veilig?

De geloofsbrieven worden overgebracht in de kopballen van HTTP met elke individuele aanvraag. De server slaat referenties tussen sessies niet op of plaatst deze in de cache en elk verzoek is volledig geïsoleerd.

+++

+++Kunnen meerdere mensen dit tegelijkertijd gebruiken?

Ja. De server is meerdere gebruikers. Elke gebruiker verbindt met zijn eigen geloofsbrieven, en de verzoeken worden geïsoleerd van elkaar.

+++

+++Wat gebeurt als mijn toegangstoken verloopt?

Wanneer u gebruikend identiteitskaart van de Cliënt en Geheim van de Cliënt voor authentiek verklaart, verfrist de serverhandvatten automatisch het teken zich. U hoeft geen actie te ondernemen.

+++

+++Moet ik iets installeren of uitvoeren?

Nee. De MCP-server wordt gehost door Adobe. U hoeft uw AI-hulpprogramma alleen te configureren om er verbinding mee te maken.

+++

+++Welke [!DNL Marketo] machtigingen heeft mijn API-gebruiker nodig?

De API-gebruiker heeft toegang nodig tot de elementtypen die u wilt beheren. Wijs minimaal een rol Alleen-lezen toe voor het bladeren door bewerkingen en een rol Lezen-Schrijven voor het maken of wijzigen van elementen. Werk met uw [!DNL Marketo] -beheerder om de juiste machtigingen toe te wijzen.

+++

+++Wat zijn de tarieflimieten?

De MCP-server overerft de API-snelheidslimieten van de [!DNL Marketo] -instantie. Gebruik een specifieke API-gebruiker om quotaverbruik te volgen en te beheren.

+++

+++Welke AI-gereedschappen worden ondersteund?

Claude Desktop, Cursor, Claude Code (CLI), en de Code van VS met GitHub Copilot. Elk AI-gereedschap dat het Model Context Protocol via HTTP ondersteunt, moet werken.

+++

+++Kan ik verbinding maken met meerdere [!DNL Marketo] -instanties?

Ja. Voeg veelvoudige ingangen in de configuratie MCP van uw AI hulpmiddel, elk met een unieke naam en geloofsbrieven voor de overeenkomstige instantie toe. U kunt `marketo-prod` en `marketo-staging` bijvoorbeeld configureren als aparte servers.

+++

## Beveiligingsoverwegingen

>[!IMPORTANT]
>
>Gebruik een specifieke API-gebruiker in [!DNL Marketo] met alleen de machtigingen die nodig zijn voor uw werk. Gebruik de beheerdersreferenties niet opnieuw voor API-toegang.

- **geloofsbrieven per-verzoek.** Clientid, clientgeheim, Munchkin-id en het REST API-eindpunt worden met elke aanvraag verzonden in HTTP-headers. Deze worden niet opgeslagen of in cache opgeslagen door de server.
- **multi-huurdersisolatie.** Elk verzoek gebruikt zijn eigen reeks geloofsbrieven. Uw gegevens hebben geen interactie met een sessie van een andere gebruiker.
- **de lijst van gewenste personen van identiteitskaart van Munchkin.** De server accepteert alleen aanvragen voor goedgekeurde [!DNL Marketo] -instanties. Aanvragen met een niet-geautoriseerde Munchkin-id worden afgewezen met een fout van 403.
- **houd geloofsbrieven uit versiecontrole.** Gebruik de omgevingsvariabele interpolatie (`${MARKETO_CLIENT_SECRET}`) als uw AI-gereedschap dit ondersteunt, zodat referenties niet worden opgeslagen in onbewerkte tekst in bestanden die zijn toegewezen aan een opslagplaats.
