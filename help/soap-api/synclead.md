---
title: syncLead
feature: SOAP
description: Leer hoe u Marketo SOAP syncLead kunt gebruiken om één lead in te voegen of bij te werken, id's en werkruimten te verwerken met aanvraagvelden, XML- en PHP-voorbeelden.
exl-id: e6cda794-a9d4-4153-a5f3-52e97a506807
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '540'
ht-degree: 0%

---

# syncLead

Deze functie voegt één lead record in of werkt deze bij. Bij het bijwerken van een bestaande lead wordt de lead aangeduid met een van de volgende toetsen:

- Marketo-id
- Id van extern systeem (geïmplementeerd als `foreignSysPersonId`)
- Marketo Cookie (gemaakt door Munchkin JS-script)
- E-mail

Als een bestaande gelijke wordt gevonden, voert de vraag een update uit. Als dat niet het geval is, wordt er een lead ingevoegd en gemaakt. Anonieme leads kunnen worden bijgewerkt met de Marketo Cookie-id en worden bij update bekend.

Met uitzondering van E-mail worden al deze id&#39;s beschouwd als unieke sleutels. De Marketo-id heeft voorrang op alle andere toetsen. Als zowel `foreignSysPersonId` als de Marketo-id aanwezig zijn in de lead record, heeft de Marketo-id voorrang en wordt de `foreignSysPersonId` bijgewerkt voor die lead. Als de enige `foreignSysPersonId` is gegeven, wordt deze gebruikt als een unieke id. Als zowel `foreignSysPersonId` als E-mail aanwezig zijn maar de Marketo-id niet aanwezig is, heeft `foreignSysPersonId` voorrang en wordt de e-mail voor die lead bijgewerkt.

Desgewenst kan een Context Header worden opgegeven om de doelwerkruimte een naam te geven.

Wanneer de werkruimten van Marketo worden toegelaten en de kopbal wordt gebruikt, worden de volgende regels toegepast:

- Als de toewijzingsregels worden geplaatst en een nieuwe lood voor om het even welke gevormde regels kwalificeert, dan worden de nieuwe lood gecreeerd in de verdeling die door de toewijzingsregel wordt bepaald. Anders worden nieuwe leads gemaakt in de primaire partitie van de benoemde werkruimte.
- Leads die overeenkomen met de Marketo Lead-id, een externe systeem-id of een Marketo Cookie, moeten voorkomen in de primaire partitie van de benoemde werkruimte. Als dit niet het geval is, wordt een fout geretourneerd
- Als een bestaande lead via e-mail wordt gekoppeld, wordt de benoemde werkruimte genegeerd en wordt de lead bijgewerkt in de huidige partitie van de benoemde werkruimte

Wanneer de werkruimten van Marketo worden toegelaten en de kopbal NIET wordt gebruikt, worden de volgende regels toegepast:

- Als de toewijzingsregels worden geplaatst en een nieuwe lood voor om het even welke gevormde regels kwalificeert, dan worden de nieuwe lood gecreeerd in de verdeling die door de toewijzingsregel wordt bepaald. Anders worden nieuwe leads gemaakt in de primaire partitie van de standaardwerkruimte.
- Bestaande leads worden bijgewerkt in hun huidige partitie

Als Marketo-werkruimten NIET zijn ingeschakeld, MOET de werkruimte Standaard de werkruimte Standaard zijn. Het is niet nodig om de koptekst door te geven.

## Verzoek

| Veldnaam | Vereist/optioneel | Beschrijving |
| --- | --- | --- |
| leadRecord->Id | Vereist - Alleen wanneer e-mail of `foreignSysPersonId` niet aanwezig is | De Marketo-id van de lead record |
| leadRecord->Email | Vereist - Alleen wanneer id of `foreignSysPersonId` niet aanwezig is | Het e-mailadres dat is gekoppeld aan de lead record |
| leadRecord->`foreignSysPersonId` | Vereist - Alleen wanneer id of e-mail niet aanwezig is | De id van het externe systeem die is gekoppeld aan de loodrecord |
| leadRecord->foreignSysType | Optioneel - Alleen vereist als `foreignSysPersonId` aanwezig is | Het type extern systeem. Mogelijke waarden: CUSTOM, SFDC, NETSUITE |
| leadRecord->leadAttributeList->attribute->attrName | Vereist | The name of the lead attribute you want to update the value of. |
| leadRecord->leadAttributeList->attribute->attrValue | Vereist | De waarde die u wilt instellen op het kenmerk lead dat in attrName is opgegeven. |
| returnLead | Vereist | Indien waar (true), wordt de volledige bijgewerkte lead record geretourneerd bij update. |
| marketoCookie | Optioneel | Het [ javascript van Munchkin ](../javascript-api/lead-tracking.md) koekje |

## XML aanvragen

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>bigcorp1_461839624B16E06BA2D663</mktowsUserId>
      <requestSignature>92f05a7be4838ae1c0e5aafe814891ee72968a08</requestSignature>
      <requestTimestamp>2013-07-31T12:38:47-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsSyncLead>
      <leadRecord>
        <Email>t@t.com</Email>
        <leadAttributeList>
          <attribute>
            <attrName>FirstName</attrName>
            <attrValue>George</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrValue>of the Jungle</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
      <returnLead>false</returnLead>
    </ns1:paramsSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## XML van antwoord

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncLead>
      <result>
        <leadId>1089965</leadId>
        <syncStatus>
          <leadId>1089965</leadId>
          <status>UPDATED</status>
          <error xsi:nil="true" />
        </syncStatus>
        <leadRecord xsi:nil="true" />
      </result>
    </ns1:successSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Voorbeeldcode - PHP

```php
 <?php

  $debug = true;

  $marketoSoapEndPoint     = "";  // CHANGE ME
  $marketoUserId           = "";  // CHANGE ME
  $marketoSecretKey        = "";  // CHANGE ME
  $marketoNameSpace        = "http://www.marketo.com/mktows/";

  // Create Signature
  $dtzObj = new DateTimeZone("America/Los_Angeles");
  $dtObj  = new DateTime('now', $dtzObj);
  $timeStamp = $dtObj->format(DATE_W3C);
  $encryptString = $timeStamp . $marketoUserId;
  $signature = hash_hmac('sha1', $encryptString, $marketoSecretKey);

  // Create SOAP Header
  $attrs = new stdClass();
  $attrs->mktowsUserId = $marketoUserId;
  $attrs->requestSignature = $signature;
  $attrs->requestTimestamp = $timeStamp;
  $authHdr = new SoapHeader($marketoNameSpace, 'AuthenticationHeader', $attrs);
  $options = array("connection_timeout" =20, "location" =$marketoSoapEndPoint);
  if ($debug) {
    $options["trace"] = true;
  }

  // Create Request
  $leadKey = new stdClass();
  $leadKey->Email = "george@jungle.com";

  // Lead attributes to update
  $attr1 = new stdClass();
  $attr1->attrName  = "FirstName";
  $attr1->attrValue = "George";

  $attr2= new stdClass();
  $attr2->attrName  = "LastName";
  $attr2->attrValue = "of the Jungle";

  $attrArray = array($attr1, $attr2);
  $attrList = new stdClass();
  $attrList->attribute = $attrArray;
  $leadKey->leadAttributeList = $attrList;

  $leadRecord = new stdClass();
  $leadRecord->leadRecord = $leadKey;
  $leadRecord->returnLead = false;
  $params = array("paramsSyncLead" =$leadRecord);

  $soapClient = new SoapClient($marketoSoapEndPoint ."?WSDL", $options);
  try {
    $result = $soapClient->__soapCall('syncLead', $params, $options, $authHdr);
  }
  catch(Exception $ex) {
    var_dump($ex);
  }

  if ($debug) {
    print "RAW request:\n" .$soapClient->__getLastRequest() ."\n";
    print "RAW response:\n" .$soapClient->__getLastResponse() ."\n";
  }
  print_r($result);

?>
```

## Voorbeeldcode - Java

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;


public class SyncLead {

    public static void main(String[] args) {
        System.out.println("Executing syncLead");
        try {
            URL marketoSoapEndPoint = new URL("https://100-AEK-913.mktoapi.com/soap/mktows/2_1" + "?WSDL");
            String marketoUserId = "demo17_1_809934544BFABAE58E5D27";
            String marketoSecretKey = "27272727aa";

            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();

            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
            String encryptString = requestTimestamp + marketoUserId ;

            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars);

            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);

            // Create Request
            ParamsSyncLead request = new ParamsSyncLead();
            LeadRecord key = new LeadRecord();

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Stringemail = objectFactory.createLeadRecordEmail("george@jungle.com");
            key.setEmail(email);
            request.setLeadRecord(key);

            Attribute attr1 = new Attribute();
            attr1.setAttrName("FirstName");
            attr1.setAttrValue("George2");

            Attribute attr2 = new Attribute();
            attr2.setAttrName("LastName");
            attr2.setAttrValue("of the Jungle");

            ArrayOfAttribute aoa = new ArrayOfAttribute();
            aoa.getAttributes().add(attr1);
            aoa.getAttributes().add(attr2);

            QName qname = new QName("http://www.marketo.com/mktows/", "leadAttributeList");
            JAXBElement<ArrayOfAttributeattrList = new JAXBElement(qname, ArrayOfAttribute.class, aoa);
            key.setLeadAttributeList(attrList);

            MktowsContextHeader headerContext = new MktowsContextHeader();
            headerContext.setTargetWorkspace("default");

            SuccessSyncLead result = port.syncLead(request, header, headerContext);

            JAXBContext context = JAXBContext.newInstance(SuccessSyncLead.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);

        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

## Voorbeeldcode - Ruby

```ruby
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "http://www.marketo.com/mktows/"

#Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

#Create SOAP Header
headers = {
    'ns1:AuthenticationHeader' ={ "mktowsUserId" =mktowsUserId, "requestSignature" =requestSignature,
    "requestTimestamp"  =requestTimestamp
    }
}

client = Savon.client(wsdl: 'http://app.marketo.com/soap/mktows/2_3?WSDL', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

#Create Request
request = {
    :lead_record ={
        :Email ="t@t.com",
          :lead_attribute_list ={
              :attribute ={
                :attr_name ="FirstName",
                :attr_value ="George" },
              :attribute! ={
                :attr_name ="LastName",
                :attr_value ="of the Jungle" }
        }
    },
    :return_lead ="false"
}

response = client.call(:sync_lead, message: request)

puts response
```
