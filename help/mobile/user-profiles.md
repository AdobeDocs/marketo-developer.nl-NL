---
title: Gebruikersprofielen
feature: Mobile Marketing, Users and Roles
description: Leer gebruikersprofielen te maken en bij te werken in Marketo Mobile SDK op iOS en Android met Objectieve C Swift en Java, standaard- en aangepaste velden, associatedLead
exl-id: 1b2cfb7f-d678-4022-8cd9-a56004a1ac46
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 0%

---

# Gebruikersprofielen

Gebruikersprofielen maken

1. [Gebruikersprofielen maken op iOS](#ios_user_profiles)
1. [Gebruikersprofielen maken op Android](#android_user_profiles)

## Gebruikersprofielen maken op iOS {#ios_user_profiles}

U kunt rijke profielen tot stand brengen door de gebruikersgebieden te verzenden zoals hieronder getoond.

```
MarketoLead *profile = [[MarketoLead alloc] init];

// Get user profile from network and populate
[profile setEmail:@"jd@makesomething.com"];
[profile setFirstName:@"John"];
[profile setLastName:@"Doe"];
[profile setAddress:@"1234KingFishSt"];
[profile setCity:@"SouthPadreIsland"];
[profile setState:@"CA"];
[profile setPostalCode:@"78596"];
[profile setCountry:@"USA"];
[profile setGender:@"male"];
[profile setLeadSource:@"_facebook_ads"];
[profile setBirthDay:@"01/01/1985"];
[profile setFacebookId:@"facebookid"];
[profile setFacebookProfileURL:@"facebook.com/profile"];
[profile setFacebookProfilePicURL:@"faceboook.com/profile/pic"];
[profile setLinkedInId:@"linkedinid"];
[profile setTwitterId:@"twitterid"];
```

```
let profile =  MarketoLead()

// Get user profile from network and populate
profile.setEmail("jd@makesomething.com")
profile.setFirstName("John")
profile.setLastName("Doe")
profile.setAddress("1234KingFishSt")
profile.setCity("SouthPadreIsland")
profile.setState("CA")
profile.setPostalCode("78596")
profile.setCountry("USA")
profile.setGender("male")
profile.setLeadSource("_facebook_ads")
profile.setBirthDay("01/01/1985")
profile.setFacebookId("facebookid")
profile.setFacebookProfileURL("facebook.com/profile")
profile.setFacebookProfilePicURL("faceboook.com/profile/pic")
profile.setLinkedInId("linkedinid")
profile.setTwitterId("twitterid")
```

Voeg meer [&#x200B; standaardgebieden &#x200B;](../rest-api/list-of-standard-fields.md) toe.

>[!BEGINTABS]

>[!TAB  Doelstelling C ]

```
// Add other custom fields
[profile setFieldName:@"mobilePhone"withValue:@"123.456.7890"];
[profile setFieldName:@"numberOfEmployees"withValue:@"10"];
[profile setFieldName:@"phone"withValue:@"123.456.7890"];
```

>[!TAB  Swift ]

```
// Add other custom fields
profile.setFieldName("mobilePhone" , withValue :"123.456.7890");
profile.setFieldName("numberOfEmployees", withValue: "10");
profile.setFieldName("phone", withValue:"123.456.7890");
```

>[!ENDTABS]

Gebruikersprofiel rapporteren.

>[!BEGINTABS]

>[!TAB  Doelstelling C ]

```
Marketo *sharedInstance = [Marketo sharedInstance];

// This method will update user profile
[sharedInstance associateLead:profile];
```

>[!TAB  Swift ]

```
let marketo = Marketo.sharedInstance()

// This method will update user profile
marketo.associateLead(profile)
```

>[!ENDTABS]

## Gebruikersprofielen maken op Android {#android_user_profiles}

1. Gebruikersprofiel maken.

   U kunt rijke profielen tot stand brengen door gebruikersgebieden te verzenden zoals hieronder getoond.

   ```java
   MarketoLead profile = new MarketoLead();
   
   // Get user profile from network and populate
   try {
       profile.setEmail("htcone3@gmail.com");
       profile.setFirstName("Mike");
       profile.setLastName("Gray");
       profile.setFacebookId("facebookid");
       profile.setAddress("1234 King Fish Blvd");
   }
   catch (MktoException e) {
       e.printStackTrace();
   }
   ```

1. Voeg meer [&#x200B; standaardgebieden &#x200B;](../rest-api/list-of-standard-fields.md) toe.

   ```java
   // Add other custom fields
   profile.setCustomField("mobilePhone", "123.456.7890");
   profile.setCustomField("numberOfEmployees", "10");
   profile.setCustomField("phone", "123.456.7890");
   profile.setCustomField("rating", "R");
   profile.setCustomField("facebookDisplayName", "mini");
   profile.setCustomField("facebookReach", "10");
   profile.setCustomField("facebookReferredEnrollments", "100");
   profile.setCustomField("facebookReferredVisits", "9998");
   profile.setCustomField("lastReferredEnrollment", "03/01/2015");
   profile.setCustomField("lastReferredVisit", "03/01/2015");
   profile.setCustomField("linkedInDisplayName", "Android");
   ```

1. Gebruikersprofiel rapporteren.

   ```java
   MarketoLead profile = new MarketoLead();
   
   // This method will update user profile
   marketoSdk.associateLead(profile);
   ```
