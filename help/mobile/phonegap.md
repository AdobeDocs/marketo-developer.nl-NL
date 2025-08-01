---
title: PhoneGap
feature: Mobile Marketing
description: PhoneGap gebruiken met Marketo op mobiele apparaten
exl-id: 99f14c76-9438-4942-9309-643bca434d07
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 0%

---

# PhoneGap

Integratie van Marketo PhoneGap-insteekmodule

## Vereisten

1. [ voeg een toepassing in Marketo Admin ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) toe (verkrijg uw toepassing Geheime Sleutel en identiteitskaart van Munchkin).
1. Push van de opstelling Meldingen ([ iOS ](push-notifications.md) | [ Android ](push-notifications.md)).
1. [ installeer PhoneGap/Cordova CLI ](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Installatie-instructies

1. Marketo PhoneGap-insteekmodule instellen

   Ervan uitgaande dat Cordova CLI is geïnstalleerd, ga naar de toepassingsmap van PhoneGap en voer de volgende opdracht uit om de Marketo-insteekmodule aan uw toepassing toe te voegen:

   `$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. De FCM-insteekmodule installeren

   `$ cordova plugin add cordova-plugin-fcm`

   Voer de volgende opdracht uit om te bevestigen dat de plug-in aan de toepassing is toegevoegd en controleer of

   `$ cordova plugin ls com.marketo.plugin 0.X.0 "MarketoPlugin" cordova-plugin-fcm 2.1.2 "FCMPlugin"`

**migreer aan Nieuwere (Facultatieve) Versie**

Voer de volgende opdracht uit om een bestaande plug-in te verwijderen:

`$ cordova plugin remove com.marketo.plugin`

Voer de volgende opdracht uit om de plug-in opnieuw toe te voegen:

`$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

**versie 8.0.0 van Cordova (Cordova@Android7.0.0) en hierboven**

Nadat het Cordova Android-platform is gemaakt, opent u de app met Android Studio en werkt u de `dirs` -waarde bij van het `Marketo.gradle` -bestand in de map `com.marketo.plugin` .

```
repositories{
  jcenter()
  flatDir{
      dirs '../app/src/main/aar'
   }
}
```

De platforms toevoegen die voor de app moeten worden gebruikt `$cordova platform add android` `$ cordova platform add ios`

Lijst met toegevoegde platforms controleren `$cordova platform ls`

1. Ondersteuning voor Firebase Cloud Messaging

1. Firebase-app configureren op Firebase-console.
   1. Creeer/voeg een Project op [ ](https://console.firebase.google.com/) de Console van de Vuurbasis toe.
      1. In de [ console van de Vuurbasis ](https://console.firebase.google.com/), uitgezochte **[!UICONTROL Add Project]**.
      1. Selecteer uw GCM-project in de lijst met bestaande Google Cloud-projecten en selecteer **[!UICONTROL Add Firebase]** .
      1. Selecteer Firebase toevoegen aan uw Android-toepassing in het welkomstscherm van Firebase.
      1. Geef de pakketnaam en SHA-1 op en selecteer **[!UICONTROL Add App]** . Er wordt een nieuw `google-services.json` -bestand voor de Firebase-app gedownload.
   1. Navigeren naar **[!UICONTROL Project Settings]** in [!UICONTROL Project Overview]
      1. Klik op de tab **[!UICONTROL General]** . Download het bestand &#39;google-services.json&#39;.
      1. Klik op de tab **[!UICONTROL Cloud Messaging]** . Kopieer [!UICONTROL Server Key] &amp; [!UICONTROL Sender ID] . Geef deze [!UICONTROL Server Key] &amp; [!UICONTROL Sender ID] door aan Marketo.
   1. FCM-wijzigingen configureren in PhoneGap-app
      1. Verplaats het gedownloade bestand &#39;google-services.json&#39; naar de hoofdmap van de module Phonegap-app
      1. Verwijder het bestand &#39;&#39;MyFirebaseInstanceIDService&#39;&#39; van de locatie `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` (Afgekeurd)
      1. Wijzig het bestand &#39;MyFirebaseMessagingService&#39; op de locatie `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` als volgt:

         ```
         import com.marketo.Marketo;
         
         public class MyFirebaseMessagingService extends FirebaseMessagingService{
         
         @Override
         public void onNewToken(String s){
           super.onNewToken(s);
           MarketoExtension.setPushNotificaitonTokens(s);
           //Add your code here
         }
         
         @Override
         public void onMessageReceived(RemoteMessage remoteMessage) {
           MarketoExtension.showPushNotificaiton(remoteMessage);
           //Add your code here
         }
         }
         ```

         1. Wijzig het bestand fcm_config_files_process.js als volgt in locatie-insteekmodules/cordova-plugin-fcm/scripts

            ```
            //change
            var strings = fs.readFileSync("platforms/android/res/values/strings.xml").toString();
            //to
            var strings = fs.readFileSync("platforms/android/app/src/main/res/values/strings.xml").toString();
            
            //AND change
            fs.writeFileSync("platforms/android/res/values/strings.xml", strings);
            //to
            fs.writeFileSync("platforms/android/app/src/main/res/values/strings.xml", strings);
            ```


### 3. Enable Push Notifications in xCode

Schakel de functie voor pushmeldingen in in het xCode-project.

### &#x200B;4. Pushmeldingen bijhouden

Plak de volgende code in de functie `application:didFinishLaunchingWithOptions:` .

>[!BEGINTABS]

>[!TAB  Doelstelling C ]

Werk de methode `applicationDidBecomeActive` hieronder bij

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB  Swift ]

Werk de methode `applicationDidBecomeActive` hieronder bij

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotification(launchOptions)
```

>[!ENDTABS]

### &#x200B;5. Marketo Framework initialiseren

Om ervoor te zorgen dat het Marketo-framework wordt gestart bij het opstarten van de app, voegt u de volgende code toe onder de functie `onDeviceReady` in het JavaScript-hoofdbestand.

We moeten `phonegap` doorgeven als het frameworktype voor PhoneGap-apps.

### Syntaxis

```
// This method will Initialize the Marketo Framework using Your MunchkinId and Secret Key
marketo.initialize(
  function() { console.log("MarketoSDK Init done."); },
  function(error) { console.log("an error occurred:" + error); },
  'YOUR_MUNCHKIN_ID',
  'YOUR_SECRET_KEY',
  'FRAMEWORK_TYPE'
);

// For session tracking, add following.
marketo.onStart(
  function(){ console.log("onStart."); },
  function(error){ console.log("Failed to report onStart." + error); }
);
```

### Parameters

- Callback met succes: functie die moet worden uitgevoerd als het Marketo-framework correct is geïnitialiseerd.
- Callback van mislukking: functie uit te voeren als het Marketo-framework niet kan worden geïnitialiseerd.
- MUNCHKIN-id: Munchkin-id ontvangen van Marketo op het moment van registratie.
- SECRET KEY : Geheime sleutel die bij de registratie van Marketo is ontvangen.

### &#x200B;6. Initialiseer Marketo-pushmelding

Om ervoor te zorgen dat de Marketo-pushmelding wordt gestart, voegt u de volgende code toe na de initialisatiefunctie in het JavaScript-hoofdbestand.

### Syntaxis

```
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

### Parameters

- Callback met succes: functie die moet worden uitgevoerd als de Marketo-pushmelding correct is geïnitialiseerd.
- Callback van mislukking: functie uit te voeren als de pushmelding van Marketo niet kan worden geïnitialiseerd.
- GCM_PROJECT_ID: identiteitskaart van het Project GCM die in [ wordt gevonden de Console van Ontwikkelaars van Google ](https://console.developers.google.com/) na het creëren van app.

Het token kan ook bij afmelden niet worden geregistreerd.

```
marketo. uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Associate Lead

U kunt een Marketo Lead maken door de associatedLead-functie aan te roepen.

### Syntaxis

```
marketo.associateLead(
  function(){ console.log("MarketoSDK : Lead Added"); },
  function(error){ console.log("an error occurred:" + error); },
  'Lead_Data_JSON_String'
);
```

### Parameters

- Callback met succes: functie uit te voeren als het Marketo-framework de lead correct koppelt.
- Callback van mislukking: functie om uit te voeren als het kader van Marketo er niet in slaagt om het lood te associëren.
- Lead Data: lead data in JSON-tekenreeksindeling.

### Voorbeeld

```
// First create a lead as shown below
var lead = {};
lead[marketo.KEY_FIRST_NAME] = "Phone";
lead[marketo.KEY_LAST_NAME] = "Gap";
lead[marketo.KEY_EMAIL] = email;
lead[marketo.KEY_ADDRESS] = "demo address";
lead[marketo.KEY_CITY] = "city";
lead[marketo.KEY_STATE] = "state";
lead[marketo.KEY_COUNTRY] = "country";
lead[marketo.KEY_POSTAL_CODE] = "postalCode";
lead[marketo.KEY_GENDER] = "gender";
// To use lead custom field, use the REST API NAME as key
lead["REST API NAME"] = "value";

// Use associateLead function to associate it.
marketo.associateLead(
  function() { console.log("MarketoSDK : Lead Associated"); },
  function(error) { console.log("an error occurred:" + error); },
  JSON.stringify(lead)
);
```

## Handeling rapporteren

U kunt elke door de gebruiker uitgevoerde actie rapporteren door de functie `reportaction` aan te roepen.

### Syntaxis

```
marketo.reportaction(
  function(){ console.log("MarketoSDK : New event sent "); },
  function(error){ console.log("an error occurred:" + error); },
  'Action_Name',
  'Action_Data_JSON_String'
);
```

### Parameters

- Callback met succes: functie die wordt uitgevoerd als Marketo-framework de handeling meldt.
- Callback van mislukking: functie uit te voeren als het kader van Marketo geen actie meldt.
- Naam van handeling: naam van handeling.
- Action Data: actiegegevens in JSON-tekenreeksindeling.

### Voorbeeld

```
// First create an event as below
var event = {
    "Action Type":"Add To Cart",
    "Action Details":"Adding Product in cart",
    "Action Metric":"10",
    "Action Length":"1"
}

marketo.reportaction(
    function(){ console.log("Reported action successfully."); },
    function(error){ console.log("Failed to report action." + error); },
    "Add To Cart",
    JSON.stringify(event)
);
```

## Sessierapportage

Bind de gebeurtenistypen &quot;pause&quot; en &quot;resume&quot;, zoals hieronder wordt weergegeven, om gebeurtenissen Start en Stop te melden.  Dit wordt gebruikt om de tijd te volgen die in uw mobiele toepassing wordt doorgebracht. Opmerking: dit is verplicht in Android.

```
//Add the following code in your www/js/index.js

bindEvents: function() {
   document.addEventListener('pause', this.onStop, false);
   document.addEventListener('resume', this.onStart, false);
},
onStop: function() {
   marketo.onStop(
       function(){ console.log("onStop"); },
       function(error){ console.log("Failed to report onStop." + error); }
   );
},
onStart: function() {
   marketo.onStart(
       function(){ console.log("onStart."); },
       function(error){console.log( "Failed to report onStart." + error); }
   );
},
```

## Leads maken

Er zijn drie manieren om leads te maken van een hybride app:

1. Marketo MME SDK
1. MARKETO REST API
1. Formulier verzenden

Afhankelijk van de gebruikte methode wordt een nieuw gemaakte lead herkend door verschillende triggers en filters. Leads die zijn gemaakt met de MME SDK- of REST-API, worden weergegeven in de triggers en filters voor &#39;Lead gemaakt&#39;. Leads die zijn gemaakt door het verzenden van een formulier, worden weergegeven in de triggers en filters Formulier invullen.

De beste praktijken moeten verenigbaar met de methode blijven die door Web wordt gebruikt app wanneer het creëren van lood. Als u al een Web-app hebt die formulierverzending als mechanisme voor het maken van leads gebruikt, gebruikt u dat mechanisme bij het maken van leads in uw hybride app. Als u al een Web-app hebt die onze REST API als mechanisme gebruikt om verbindingen tot stand te brengen, dan gebruik dat zelfde mechanisme wanneer het creëren van lood in uw hybride app. Als u geen formulierverzending of REST API gebruikt als mechanisme voor het maken van leads in uw webapp, kunt u overwegen om de MME SDK te gebruiken voor het maken van leads in Marketo.
