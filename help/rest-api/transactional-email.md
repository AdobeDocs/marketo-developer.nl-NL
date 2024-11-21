---
title: Transactionele e-mail
feature: REST API
description: Transactiee-mails verwerken voor aanvraagcampagnes.
exl-id: 057bc342-53f3-4624-a3c0-ae619e0c81a5
source-git-commit: e7d893a81d3ed95e34eefac1ee8f1ddd6852f5cc
workflow-type: tm+mt
source-wordcount: '971'
ht-degree: 0%

---

# Transactionele e-mail

Een gemeenschappelijk gebruiksgeval voor Marketo API moet het verzenden van transactie e-mails naar specifieke verslagen via de [ vraag van de Campagne van het Verzoek ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/triggerCampaignUsingPOST) API teweegbrengen. Er zijn een paar configuratievereisten binnen Marketo om de vereiste vraag met de Marketo REST API uit te voeren.

- De ontvanger moet een record in Marketo hebben
- Er moet een Transactiee-mail zijn gemaakt en goedgekeurd in uw Marketo-exemplaar.
- Er moet een actieve triggercampagne zijn met de &quot;Campagne is Requested, 1. Source: Web Service API&quot;, die is ingesteld om de e-mail te verzenden

Eerst [ creeer en keur uw e-mail ](https://experienceleague.adobe.com/docs/marketo/using/home.html) goed. Als de e-mail echt transactie is, zult u het waarschijnlijk aan operationeel moeten plaatsen, maar zeker zijn dat het wettelijk als operationeel kwalificeert. Dit is geconfigureerd met Scherm bewerken onder E-mailhandelingen > E-mailinstellingen:

![ verzoek-campagne-e-mail-Montages ](assets/request-campaign-email-settings.png)

![ verzoek-campagne-Operationeel ](assets/request-campaign-operational.png)

Goedkeuren en we zijn klaar om onze campagne te maken:

![ RequestCampaign-Goedkeuren-Ontwerp ](assets/request-campaign-approve-draft.png)

Als u aan het creëren van campagnes nieuw bent, controle uit [ creeer een Nieuw Slimme Campagne ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/creating-a-smart-campaign/create-a-new-smart-campaign.html) artikel. Als je je campagne hebt gemaakt, moeten we deze stappen doorlopen. Vorm uw Slimme Lijst met de Campagne wordt Gevraagde trekker:

![ verzoek-Campagne-slim-Lijst ](assets/request-campaign-smart-list.png)

Nu moeten we de stroom zo configureren dat deze een stap E-mail verzenden naar onze e-mail stuurt:

![ verzoek-campagne-stroom ](assets/request-campaign-flow.png)

Voordat u de activering uitvoert, moet u een aantal instellingen opgeven op het tabblad Planning. Als deze specifieke e-mail maar één keer naar een bepaalde record moet worden verzonden, laat u de kwalificatie-instellingen ongewijzigd. Als het vereist is dat zij de e-mail veelvoudige tijden ontvangen, niettemin, wilt u dit aan of elke keer aanpassen of aan één van de beschikbare wegen:

Nu zijn we klaar om te activeren:

![ verzoek-campagne-Programma ](assets/request-campaign-schedule.png)

## Het verzenden van de API Vraag

**Nota:** In de voorbeelden hieronder van Java, gebruiken wij het [ minimum-json pakket ](https://github.com/ralfstx/minimal-json) om vertegenwoordiging JSON in onze code te behandelen.

Het eerste onderdeel van het verzenden van een transactie-e-mail via de API is ervoor zorgen dat er een record met het bijbehorende e-mailadres bestaat in uw Marketo-exemplaar en dat we toegang hebben tot de bijbehorende lead-id. In het kader van dit bericht gaan we ervan uit dat de e-mailadressen al in Marketo staan en dat we alleen de id van de record moeten ophalen. Voor dit, gebruiken wij [ krijgen Leads door de vraag van het Type van Filter ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET). Laten we kijken naar onze hoofdmethode om de campagne aan te vragen:

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App 
{
    public static void main( String[] args )
    {
        //Create an instance of Auth so that we can authenticate with our Marketo instance
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("requestCampaign.test@marketo.com");

        //Create and parameterize an instance of Leads
        //Set your email filterValue appropriately
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("test.requestCamapign@example.com");

        //Get the inner results array of the response
        JsonArray leadsResult = leadsRequest.getData().get("result").asArray();

        //Get the id of the record indexed at 0
        int lead = leadsResult.get(0).asObject().get("id").asInt();

        //Set the ID of your campaign from Marketo
        int campaignId = 0;
        RequestCampaign rc = new RequestCampaign(auth, campaignId).addLead(lead);

        //Send the request to Marketo
        rc.postData();
    }
}
```

Om deze resultaten te krijgen van de JsonObject-reactie van leadRequest, moeten we code schrijven. Om het eerste resultaat in de Serie terug te winnen, moeten wij de Serie uit JsonObject halen en het voorwerp krijgen dat bij 0 wordt geïndexeerd:

```java
JsonArray leadsResult = leadsRequest.getData().get("result").asArray();
int leadId = leadsResult.get(0).asObject().get("id").asInt();
```

Van nu af aan moeten we alleen maar de oproep tot het indienen van een campagne doen. Hiervoor zijn de vereiste parameters id in de URL van de aanvraag en een array van JSON-objecten met één lid, &#39;id&#39;. Kijk eens naar de code voor dit onderwerp:

```java
package dev.marketo.blog_request_campaign;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class RequestCampaign {
    private String endpoint;
    private Auth auth;
    public ArrayList leads = new ArrayList();
    public ArrayList tokens = new ArrayList();
    
    public RequestCampaign(Auth auth, int campaignId) {
        this.auth = auth;
        this.endpoint = this.auth.marketoInstance + "/rest/v1/campaigns/" + campaignId + "/trigger.json";
    }
    public RequestCampaign setLeads(ArrayList leads) {
        this.leads = leads;
        return this;
    }
    public RequestCampaign addLead(int lead){
        leads.add(lead);
        return this;
    }
    public RequestCampaign setTokens(ArrayList tokens) {
        this.tokens = tokens;
        return this;
    }
    public RequestCampaign addToken(String tokenKey, String val){
        JsonObject jo = new JsonObject().add("name", tokenKey);
        jo.add("value", val);
        tokens.add(jo);
        return this;
    }
    public JsonObject postData(){
        JsonObject result = null;
        try {
            JsonObject requestBody = buildRequest(); //builds the Json Request Body
            System.out.println("Executing RequestCampaign call\n" + "Endpoint: " + endpoint + "\nRequest Body:\n"  + requestBody);
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
            urlConn.setRequestMethod("POST");
            urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
            OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
            wr.write(requestBody.toString());
            wr.flush();
            InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
            Reader reader = new InputStreamReader(inStream);
            result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
            System.out.println("Result:\n" + result);
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return result;
    }
    
    private JsonObject buildRequest(){
        JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
        JsonObject input = new JsonObject();
        JsonArray leadsArray = new JsonArray();
        for (int lead : leads) {
            JsonObject jo = new JsonObject().add("id", lead);
            leadsArray.add(jo);
        }
        input.add("leads", leadsArray);
        JsonArray tokensArray = new JsonArray();
        for (JsonObject jo : tokens) {
            tokensArray.add(jo);
        }
        input.add("tokens", tokensArray);
        requestBody.add("input", input);
        return requestBody;
    }

}
```

Deze klasse heeft één aannemer die een Auth, en Id van de campagne neemt. Leads worden aan het object toegevoegd door een `ArrayList<Integer>` met de id&#39;s van de records door te geven aan setLeads of door addLead te gebruiken. Dit neemt één geheel getal en voegt dit toe aan de bestaande ArrayList in de eigenschap Leads. Om de API vraag teweeg te brengen om de belangrijkste verslagen tot de campagne over te gaan, moet postData worden geroepen, die een JsonObject terugkeert die de reactiegegevens van het verzoek bevat. Wanneer de verzoekcampagne wordt geroepen, zal elke lood die tot de vraag wordt overgegaan door de doeltrekkercampagne in Marketo worden verwerkt en zal de e-mail worden verzonden die eerder werd gecreeerd. U hebt een e-mail getriggerd via de Marketo REST API. Houd een oogje in het zeil voor Deel 2 waar wij de inhoud van een e-mail dynamisch aanpassen door de Campagne van het Verzoek.

### Uw e-mail maken

Om onze inhoud aan te passen, moeten wij eerst a [ programma ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/programs/creating-programs/create-a-program.html) en een [ e-mail ](https://experienceleague.adobe.com/docs/marketo/using/home.html) in Marketo vormen. Om onze aangepaste inhoud te genereren, moeten we tokens in het programma maken en deze in de e-mail plaatsen die we gaan verzenden. Eenvoudigheidshalve gebruiken we slechts één token in dit voorbeeld, maar u kunt elk aantal tokens in een e-mail vervangen, in het vak Van e-mail, Van naam, Antwoord naar of enig ander stuk inhoud in de e-mail. Laten we dus één token Rich Text maken voor vervanging en deze &#39;bodyReplacement&#39; noemen. Met RTF kunnen we alle inhoud van de token vervangen door willekeurige HTML die we willen invoeren.

![ nieuw-Symbolisch ](assets/New-Token.png)

Tokens kunnen niet worden opgeslagen als ze leeg zijn. Plaats hier een tijdelijke aanduiding voor tekst. Nu moeten we ons token invoegen in de e-mail:

![ toe:voegen-Token ](assets/Add-Token.png)

Dit teken zal nu voor vervanging door een vraag van de Campagne van het Verzoek toegankelijk zijn. Deze token kan zo eenvoudig zijn als een enkele tekstregel die per e-mail moet worden vervangen, of kan bijna de volledige opmaak van de e-mail bevatten.

### De code

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App 
{
    public static void main( String[] args )
    {
        //Create an instance of Auth so that we can authenticate with our Marketo instance
        Auth auth = new Auth("Client ID - CHANGE ME", "Client Secret - CHANGE ME", "Host - CHANGE ME");
        
        //Create and parameterize an instance of Leads
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("requestCampaign.test@marketo.com");
        
        //get the inner results array of the response
        JsonArray leadsResult = leadsRequest.getData().get("result").asArray();
        
        //get the id of the record indexed at 0
        int lead = leadsResult.get(0).asObject().get("id").asInt();
        
        //Set the ID of our campaign from Marketo
        int campaignId = 1578;
        RequestCampaign rc = new RequestCampaign(auth, campaignId).addLead(lead);

        //Create the content of the token here, and add it to the request
        String bodyReplacement = "<div class=\"replacedContent\"><p>This content has been replaced</p></div>";
        rc.addToken("{{my.bodyReplacement}}", bodyReplacement);
        rc.postData();
    }
}
```

Als de code er vertrouwd uitziet, is dat omdat deze slechts twee extra regels heeft van de bovenstaande hoofdmethode. Dit keer creëren wij de inhoud van ons teken in de bodyReplacement variabele en dan gebruikend de addToken methode om het aan het verzoek toe te voegen. addToken neemt een sleutel en een waarde en leidt dan tot een vertegenwoordiging JsonObject en voegt het aan de interne tokens serie toe. Dit wordt dan geserialiseerd tijdens de postData methode en leidt tot een lichaam dat als dit kijkt:

```json
{
    "input":
    {
        "leads": [
            {
                "id": 1
            }
        ],
        "tokens": [
            {
                "name": "{{my.bodyReplacement}}",
                "value": "<div class=\"replacedContent\"><p>This content has been replaced</p></div>"
            }
        ]
    }
}
```

Gecombineerd ziet onze consoleoutput als dit:

```bash
Token is empty or expired. Trying new authentication
Trying to authenticate with ...
Got Authentication Response: {"access_token":"19d51b9a-ff60-4222-bbd5-be8b206f1d40:st","token_type":"bearer","expires_in":3565,"scope":"apiuser@mktosupport.com"}
Executing RequestCampaign call
Endpoint: .../rest/v1/campaigns/1578/trigger.json
Request Body:
{"input":{"leads":[{"id":1}],"tokens":[{"name":"{{my.bodyReplacement}}","value":"<div class=\"replacedContent\"><p>This content has been replaced</p></div>"}]}}
Result:
{"requestId":"1e8d#14eadc5143d","result":[{"id":1578}],"success":true}
```

## Omloop omhoog

Deze methode kan op allerlei manieren worden uitgebreid, waarbij de inhoud in e-mails binnen afzonderlijke lay-outsecties of buiten e-mails wordt gewijzigd, zodat aangepaste waarden kunnen worden doorgegeven aan taken of interessante momenten. Overal waar een teken van binnen een programma kan worden gebruikt kan worden aangepast gebruikend deze methode. De gelijkaardige functionaliteit is ook beschikbaar met de [ vraag van de Campagne van het Programma ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/scheduleCampaignUsingPOST) die u zal toestaan om tokens over een volledige partijcampagne te verwerken. Deze kunnen niet per lood worden aangepast, maar zijn nuttig om inhoud over een brede reeks lood aan te passen.
