---
title: getLeadChanges
feature: SOAP
description: getLeadChanges SOAP-aanroepen
exl-id: 23445684-d8d9-407b-8f19-cb69e806795c
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# getLeadChanges

Deze API is vergelijkbaar met `getLeadActivity` , maar werkt op meerdere leads tegelijk. De bewerking controleert op nieuwe leads die zijn gemaakt, updates voor leidende velden en andere activiteiten.

Het resultaat bevat activiteiten die de verandering samen met a [ stroompositie ](stream-position.md) om door grote resultaatreeksen te pagineren veroorzaakten.

U moet een invoerparameter opnemen die aangeeft welke activiteitsfilters u in het resultaat wilt retourneren. Als u alle activiteiten wilt, kan een lege waarde worden overgegaan. Geef voor meer dan één activiteitsfilter een lijst met activiteitsfilters door.

Voorbeelden van activiteitstypen zijn: &#39;Bezoek webpagina&#39;, &#39;Formulier invullen&#39; en &#39;Koppeling klikken&#39;.

Na SOAP API versie 2_2 kunt u een `leadSelector` opnemen.

Voor `LastUpdateAtSelector` zou de `oldestUpdatedAt` -waarde overeenkomen met de `oldestCreatedAt` -waarde in de `startPosition` . De waarde `latestUpdatedAt` komt overeen met de waarde `latestCreatedAt` in de `startPosition` .

Opmerking: het limietaantal leads dat wordt ondersteund in a `LeadKeySelector` is 100. Als het aantal leads groter is dan 100, genereert de API een ongeldige parameteruitzondering en retourneert deze een SOAP-fout.

## Verzoek

| Veldnaam | Vereist/optioneel | Beschrijving |
| --- | --- | --- |
| activityFilter->includeAttributes->activityType | Optioneel (afgekeurd) Gebruik `activityNameFilter` in plaats hiervan | Beperkt de reactie om slechts die gespecificeerde activiteitstypes op te nemen. Zie WSDL voor alle activiteitstypen. |
| activityFilter->excludeAttributes->activityType | Optioneel | Hiermee wordt de reactie beperkt om de opgegeven activiteitstypen uit te sluiten. Zie WSDL voor alle activiteitstypen. NOTA: U kunt niet zowel `includeAttributes` als `excludeAttributes` binnen de zelfde vraag specificeren. |
| activityNameFilter | Optioneel | Beperkt de reactie zodat alleen de opgegeven activiteitsfilters worden opgenomen. |
| batchSize | Optioneel | Maximumaantal records dat moet worden geretourneerd. Systeem beperkt tot 1.000 of `batchSize`, afhankelijk van welke waarde het minst is. |
| startPosition | Vereist | Wordt gebruikt om een groot aantal reacties op de activiteit te pagineren. |
| startPosition->offset | Optioneel | De verschuivingswaarde wordt geretourneerd door het vorige veld voor aanroepreactie newStartPosition->offset. |
| startPosition->oldestCreatedAt | Optioneel | De tijdstempel die wordt gebruikt om resultaten te filteren, resulteert alleen in leads die zijn gemaakt sinds de oudsteCreatedAt. NOTA: U kunt `LastUpdateAtSelector->oldestUpdatedAt` timestamp gebruiken om `oldestCreatedAt` te specificeren. |
| startPosition->activityCreatedAt | Optioneel | De tijdstempel die wordt gebruikt om resultaten te filteren, resulteert alleen in het opnemen van leads met activiteit sinds activityCreatedAt. NOTA: U kunt `LastUpdateAtSelector->latestUpdatedAt` timestamp gebruiken om `activityCreatedAt` te specificeren. |
| leadSelector | Optioneel | Kan een van de volgende drie typen hebben: `LeadKeySelector`, `StaticListSelector`, `LastUpdateAtSelector` |
| LeadKeySelector: leadSelector->keyType | Vereist | Het id-type waarop u een query wilt uitvoeren. Waarden zijn `IDNUM`, `COOKIE`, `EMAIL`, `LEADOWNEREMAIL`, `SFDCACCOUNTID`, `SFDCCONTACTID`, `SFDCLEADID`, `SFDCLEADOWNERID`, `SFDCOPPTYID` . |
| LeadKeySelector: leadSelector->keyValues->stringItem | Vereist | Lijst met sleutelwaarden. Dat is &quot;lead@email.com&quot; |
| StaticListSelector: leadSelector->staticListName | Optioneel wanneer `leadSelector->staticListId` aanwezig is | De naam van de statische lijst |
| StaticListSelector: leadSelector->staticListId | Optioneel wanneer `leadSelector->staticListName` aanwezig is | De id van de statische lijst |

## XML aanvragen

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809934544BFABAE58E5D27</mktowsUserId>
      <requestSignature>59e88471266a7495590941586ef6a96849ea8e11</requestSignature>
      <requestTimestamp>2013-08-02T17:22:53-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsGetLeadChanges>
      <startPosition>
        <oldestCreatedAt>2013-08-01T00:13:13+00:00</oldestCreatedAt>
      </startPosition>
      <activityNameFilter>
        <stringItem>Visit Webpage</stringItem>
        <stringItem>Click Link</stringItem>
      </activityNameFilter>
      <batchSize>10</batchSize>
    </ns1:paramsGetLeadChanges>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## XML van antwoord

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successGetLeadChanges>
      <result>
        <returnCount>10</returnCount>
        <remainingCount>576</remainingCount>
        <newStartPosition>
          <latestCreatedAt xsi:nil="true" />
          <oldestCreatedAt>2013-08-01T00:13:13+00:00</oldestCreatedAt>
          <activityCreatedAt xsi:nil="true" />
          <offset>ID:1086173</offset>
        </newStartPosition>
        <leadChangeRecordList>
          <leadChangeRecord>
            <id>1037189</id>
            <activityDateTime>2013-08-01T00:00:09-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Lead Score</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Lead Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>402</attrValue>
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>403</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Changed by Smart Campaign Japanese form fillout.test time zone action Change Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Step ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>19582</attrValue>
              </attribute>
              <attribute>
                <attrName>Choice Number</attrName>
                <attrType xsi:nil="true" />
                <attrValue>0</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>84105</attrValue>
              </attribute>
            </activityAttributes>
            <campaign>Japanese form fillout.test time zone</campaign>
            <mktPersonId>84105</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1037190</id>
            <activityDateTime>2013-08-01T02:37:17-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Marketo Social Facebook Reach</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Marketo Social Facebook Reach</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>574</attrValue>
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>571</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Form fill-out, URL: <unknown></attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1086478</attrValue>
              </attribute>
            </activityAttributes>
            <mktPersonId>1086478</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1037193</id>
            <activityDateTime>2013-08-01T11:19:26-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Lead Score</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Lead Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue />
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Changed by Smart Campaign Visit any web page action Change Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Step ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>11156</attrValue>
              </attribute>
              <attribute>
                <attrName>Choice Number</attrName>
                <attrType xsi:nil="true" />
                <attrValue>0</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1090235</attrValue>
              </attribute>
            </activityAttributes>
            <campaign>Visit any web page</campaign>
            <mktPersonId>1090235</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1082831</id>
            <activityDateTime>2013-08-01T13:10:17-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>First Name</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>First Name</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>George</attrValue>
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Travis</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>SOAP API</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1089965</attrValue>
              </attribute>
            </activityAttributes>
            <mktPersonId>1089965</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1082843</id>
            <activityDateTime>2013-08-01T13:10:19-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Last Name</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Last Name</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>of the Jungle</attrValue>
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Kaufman</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>SOAP API</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1089965</attrValue>
              </attribute>
            </activityAttributes>
            <mktPersonId>1089965</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1086153</id>
            <activityDateTime>2013-08-01T13:21:51-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Urgency</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Urgency</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue />
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>25</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Changed by Smart Campaign Operational Campaign - ML action Change Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Source</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Lead update</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1090236</attrValue>
              </attribute>
            </activityAttributes>
            <mktPersonId>1090236</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1086166</id>
            <activityDateTime>2013-08-01T13:21:53-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Relative Urgency</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Relative Urgency</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue />
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Changed by Smart Campaign Operational Campaign - ML action Change Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Source</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Lead update</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1090236</attrValue>
              </attribute>
            </activityAttributes>
            <mktPersonId>1090236</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1086167</id>
            <activityDateTime>2013-08-01T13:21:53-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Relative Score</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Relative Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue />
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Changed by Smart Campaign Operational Campaign - ML action Change Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Source</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Lead update</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1090236</attrValue>
              </attribute>
            </activityAttributes>
            <mktPersonId>1090236</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1086169</id>
            <activityDateTime>2013-08-01T13:21:53-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Priority</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Priority</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue />
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>70</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Changed by Smart Campaign Operational Campaign - ML action Change Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Source</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Lead update</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1090236</attrValue>
              </attribute>
            </activityAttributes>
            <mktPersonId>1090236</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1086173</id>
            <activityDateTime>2013-08-01T13:21:53-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Lead Score</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Lead Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue />
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>25</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Changed by Smart Campaign Operational Campaign - ML action Change Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Step ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>12259</attrValue>
              </attribute>
              <attribute>
                <attrName>Choice Number</attrName>
                <attrType xsi:nil="true" />
                <attrValue>0</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1090236</attrValue>
              </attribute>
            </activityAttributes>
            <campaign>Operational Campaign - ML</campaign>
            <mktPersonId>1090236</mktPersonId>
          </leadChangeRecord>
        </leadChangeRecordList>
      </result>
    </ns1:successGetLeadChanges>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Voorbeeldcode - PHP

```php
<?php
$debug = true;
$marketoSoapEndPoint    = "";  // CHANGE ME
$marketoUserId      = "";  // CHANGE ME
$marketoSecretKey   = ""; // CHANGE ME
$marketoNameSpace   = "http://www.marketo.com/mktows/";

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
$options = array("connection_timeout" => 600, "location" => $marketoSoapEndPoint);
if ($debug) {
  $options["trace"] = 1;
}

// Create Request
$params = new stdClass();
$filter = new stdClass();

$keyValues = array("Visit Webpage", "Click Link");
$filter->stringItem = $keyValues;
$params->activityNameFilter = $filter;

$streamPos = new stdClass();
$streamPos->oldestCreatedAt = "2013-08-01T00:13:13+00:00";
$params->startPosition = $streamPos;
$params->batchSize = 10;
$authHdr = new SoapHeader($marketoNameSpace, 'AuthenticationHeader', $attrs);
$client = new SoapClient($marketoSoapEndPoint ."?WSDL", $options);
$doPage = true;
while($doPage) {
  $success = $client->__soapCall('getLeadChanges', array($params), null, $authHdr);
  $result = $success->result;
  $doPage = ($result->remainingCount > 0);
  $streamPos = $result->newStartPosition;
  $params->startPosition = $streamPos;
  if ($debug) {
    print "RAW request:\n" .$client->__getLastRequest() ."\n";
    print "RAW response:\n" .$client->__getLastResponse() ."\n";
  }
}
return;
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
import javax.xml.datatype.XMLGregorianCalendar;
import javax.xml.datatype.DatatypeFactory;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
import java.util.GregorianCalendar;


public class GetLeadChanges {

    public static void main(String[] args) {
        System.out.println("Executing Get Lead Changes");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";

            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();

            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
            String encryptString = requestTimestamp + marketoUserId;

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
            ParamsGetLeadChanges request = new ParamsGetLeadChanges();

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> batchSize = objectFactory.createParamsGetLeadActivityBatchSize(10);
            request.setBatchSize(batchSize);

            ArrayOfString activities = new ArrayOfString();
            activities.getStringItems().add("Visit Webpage");
            activities.getStringItems().add("Click Link");

            JAXBElement<ArrayOfString> activityFilter = objectFactory.createParamsGetLeadChangesActivityNameFilter(activities);
            request.setActivityNameFilter(activityFilter);

            // Create oldestCreateAt timestamp from 5 days ago
            GregorianCalendar gc = new GregorianCalendar();
            gc.setTimeInMillis(new Date().getTime());
            gc.add( GregorianCalendar.DAY_OF_YEAR, -5);

            DatatypeFactory factory = DatatypeFactory.newInstance();
            JAXBElement<XMLGregorianCalendar> oldestCreateAtValue =objectFactory.createStreamPositionOldestCreatedAt(factory.newXMLGregorianCalendar(gc));

            StreamPosition sp = new StreamPosition();
            sp.setOldestCreatedAt(oldestCreateAtValue);
            request.setStartPosition(sp);

            SuccessGetLeadChanges result = port.getLeadChanges(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessGetLeadChanges.class);
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
    'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,
    "requestTimestamp"  => requestTimestamp
    }
}

client = Savon.client(wsdl: 'http://app.marketo.com/soap/mktows/2_3?WSDL', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

#Create Request
request = {
    :start_position => {
        :oldest_created_at => "2013-08-01T00:13:13+00:00" },
    :activity_name_filter => {
        :stringItem => ["Visit Webpage", "Click Link"] },
    :batch_size => "10"
}

response = client.call(:get_lead_changes, message: request)

puts response
```
