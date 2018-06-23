---
title: Bing varlık arama SDK Java hızlı başlangıç | Microsoft Docs
description: Bing varlık arama SDK konsol uygulama ayarlama.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 02/19/2018
ms.author: v-gedod
ms.openlocfilehash: ebfabc00b5dc031ac4e5284450a9d639c383e78f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355240"
---
# <a name="bing-entity-search-sdk-java-quickstart"></a>Bing varlık arama SDK Java hızlı başlangıç

Bing varlık arama SDK varlık sorgular ve ayrıştırma sonuçları için REST API işlevsellik sağlar. 

[Kaynak kodu Java Bing varlık arama SDK örnekleri için](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingEntitySearch) Git hub'da kullanılabilir. 

## <a name="application-dependencies"></a>Uygulama bağımlılıkları
Alma bir [Bilişsel hizmetler erişim tuşu](https://azure.microsoft.com/try/cognitive-services/) altında **arama**. Bing varlık arama SDK bağımlılığı Maven, Gradle veya başka bir bağımlılık yönetimi sistemi kullanarak yükleyin. Maven POM dosya bildirimi gerektirir:
```
  <dependencies>
    <dependency>
        <groupId>com.microsoft.azure.cognitiveservices</groupId>
        <artifactId>azure-cognitiveservices-entitysearch</artifactId>
        <version>0.0.1-beta-SNAPSHOT</version>
    </dependency>
  </dependencies>
```
## <a name="entity-search-client"></a>Varlık arama istemci
İçeri aktarmalar için sınıf uygulamasını ekleyin.
```
import com.microsoft.azure.cognitiveservices.entitysearch.*;
import com.microsoft.azure.cognitiveservices.entitysearch.implementation.EntitySearchAPIImpl;
import com.microsoft.azure.cognitiveservices.entitysearch.implementation.SearchResponseInner;
import com.microsoft.rest.credentials.ServiceClientCredentials;
import okhttp3.Interceptor;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
```
Uygulama **EntitySearchAPIImpl** bir örneğini gerektirir istemci **ServiceClientCredentials** sınıfı.
```
public static EntitySearchAPIImpl getClient(final String subscriptionKey) {
    return new EntitySearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
            new ServiceClientCredentials() {
                @Override
                public void applyCredentialsFilter(OkHttpClient.Builder builder) {
                    builder.addNetworkInterceptor(
                            new Interceptor() {
                                @Override
                                public Response intercept(Interceptor.Chain chain) throws IOException {
                                    Request request = null;
                                    Request original = chain.request();
                                    // Request customization: add request headers
                                    Request.Builder requestBuilder = original.newBuilder()
                                            .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
                                    request = requestBuilder.build();
                                    return chain.proceed(request);
                                }
                            });
                }
            });
}

```
Tek varlık "Satya Nadella" için arama ve kısa bir açıklama yazdırın.
```
public static void dominantEntityLookup(final String subscriptionKey)
{
    try
    {
        EntitySearchAPIImpl client = getClient(subscriptionKey);
        SearchResponseInner entityData = client.entities().search(
                "satya nadella", null, null, null, null, null, null, "en-us", null, null, SafeSearch.STRICT, null);

        if (entityData.entities().value().size() > 0)
        {
            // Find the entity that represents the dominant entity
            List<Thing> entrys = entityData.entities().value();
            Thing dominateEntry = null;
            for(Thing thing : entrys) {
                if(thing.entityPresentationInfo().entityScenario() == EntityScenario.DOMINANT_ENTITY) {
                    System.out.println("\r\nSearched for \"Satya Nadella\" and found a dominant entity with this description:");
                    System.out.println(thing.description());
                    break;
                }
            }

            if(dominateEntry == null)
            {
                 System.out.println("Couldn't find main entity Satya Nadella!");
            }
        }
        else
        {
             System.out.println("Didn't see any data..");
        }
    }
    catch (ErrorResponseException ex)
    {
         System.out.println("Encountered exception. " + ex.getLocalizedMessage());
    }
}

```
Belirsiz sorgu için "William ağ geçitleri" ve tanıtıcı Kesinleştirme sonuçları arayın.
```
public static void handlingDisambiguation(String subscriptionKey)
{
    try
    {
        EntitySearchAPIImpl client = getClient(subscriptionKey);
        SearchResponseInner entityData = client.entities().search(
                "william gates", null, null, null, null, null, null, "en-us", null, null, SafeSearch.STRICT, null);
        if (entityData.entities().value().size() > 0)
        {
            // Find the entity that represents the dominant entity
            List<Thing> entrys = entityData.entities().value();
            Thing dominateEntry = null;
            List<Thing> disambigEntities = new ArrayList<Thing>();
            for(Thing thing : entrys) {
                if(thing.entityPresentationInfo().entityScenario() == EntityScenario.DOMINANT_ENTITY) {
                    System.out.println("\r\nSearched for \"William Gates\" and found a dominant entity with this description:");
                    System.out.println(String.format("Searched for \"William Gates\" and found a dominant entity with type hint \"%s\" with this description:",
                            thing.entityPresentationInfo().entityTypeDisplayHint()));
                    System.out.println(thing.description());
                }

                if(thing.entityPresentationInfo().entityScenario() == EntityScenario.DISAMBIGUATION_ITEM) {
                    disambigEntities.add(thing);
                    System.out.println("Searched for \"William Gates\" and found a dominant entity with this description:");
                    System.out.println(thing.description());
                }

            }

            if (dominateEntry == null)
            {
                 System.out.println("Couldn't find a reliable dominant entity for William Gates!");
            }

            if (disambigEntities.size() > 0)
            {
                 System.out.println();
                 System.out.println("This query is pretty ambiguous and can be referring to multiple things. Did you mean one of these: ");

                StringBuilder sb = new StringBuilder();

                for (Thing disambig : disambigEntities)
                {
                    sb.append(String.format(", or %s the %s", disambig.name(), disambig.entityPresentationInfo().entityTypeDisplayHint()));
                }

                 System.out.println(sb.toString().substring(5) + "?");
            }
            else
            {
                 System.out.println("We didn't find any disambiguation items for William Gates, so we must be certain what you're talking about!");
            }
        }
        else
        {
             System.out.println("Didn't see any data..");
        }
    }
    catch (ErrorResponseException ex)
    {
         System.out.println("Encountered exception. " + ex.getLocalizedMessage());
    }
}

```
"Microsoft depolamak" sorgu tek bir deposuyla aramak ve sonucu için telefon numarası yazdırın.
```
public static void storeLookup(String subscriptionKey)
{
    try
    {
        EntitySearchAPIImpl client = getClient(subscriptionKey);
        SearchResponseInner entityData = client.entities().search(
                "Microsoft Store", null, null, null, null, null, null, "en-us", null, null, SafeSearch.STRICT, null);

        if (entityData.places() != null && entityData.places().value().size() > 0)
        {
            // Some local entities are places, others are not. Depending on the data that you want, try to cast to the appropriate schema.
            // In this case, the returned item is technically a store, but the Place schema has the data that we want (telephone).
            Place store = (Place)entityData.places().value().get(0);

            if (store != null)
            {
                 System.out.println("\r\nSearched for \"Microsoft Store\" and found a store with this phone number:");
                 System.out.println(store.telephone());
            }
            else
            {
                 System.out.println("Couldn't find a place!");
            }
        }
        else
        {
             System.out.println("Didn't see any data..");
        }
    }
    catch (ErrorResponseException ex)
    {
         System.out.println("Encountered exception. " + ex.getLocalizedMessage());
    }
}

```
"Seattle Restoran." sorguyla Restoran listesini arayın Adları ve sonuçları için telefon numaraları yazdırın.
```
public static void multipleRestaurantLookup(String subscriptionKey)
{
    try
    {
        EntitySearchAPIImpl client = getClient(subscriptionKey);
        SearchResponseInner restaurants = client.entities().search(
                "Seattle restaurants", null, null, null, null, null, null, "en-us", null, null, SafeSearch.STRICT, null);
        
        System.out.println("\r\nSearched for \"multiple restaurants\""); 
        if (restaurants.places() != null && restaurants.places().value().size() > 0)
        {
            List<Thing> listItems = new ArrayList<Thing>();
            for(Thing place : restaurants.places().value()) {
                if (place.entityPresentationInfo().entityScenario() == EntityScenario.LIST_ITEM) {
                    listItems.add(place);
                }
            }

            if (listItems.size() > 0)
            {
                StringBuilder sb = new StringBuilder();

                for (Thing item : listItems)
                {
                    Place place = (Place)item;
                    if (place == null)
                    {
                         System.out.println(String.format("Unexpectedly found something that isn't a place named \"%s\"", place.name()));
                        continue;
                    }

                    sb.append(String.format(",%s (%s) ", place.name(), place.telephone()));
                }

                 System.out.println("Ok, we found these places: ");
                 System.out.println(sb.toString().substring(1));
            }
            else
            {
                 System.out.println("Couldn't find any relevant results for \"seattle restaurants\"");
            }
        }
        else
        {
             System.out.println("Didn't see any data..");
        }
    }
    catch (ErrorResponseException ex)
    {
         System.out.println("Encountered exception. " + ex.getLocalizedMessage());
    }
}

```
Bir sınıfa kod yürütmek için ana işleviyle bu makalede açıklanan yöntemleri ekleyin.
```
package entitySDK;
import com.microsoft.azure.cognitiveservices.entitysearch.*;

public class EntitySearchSDK {

    public static void main(String[] args) {
        
        dominantEntityLookup("YOUR-SUBSCRIPTION-KEY");
        handlingDisambiguation("YOUR-SUBSCRIPTION-KEY");
        restaurantLookup("YOUR-SUBSCRIPTION-KEY");
        multipleRestaurantLookup("YOUR-SUBSCRIPTION-KEY");

    }
    // Include the methods described in this article.
}
```
## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler Java SDK'sı örneği](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)

