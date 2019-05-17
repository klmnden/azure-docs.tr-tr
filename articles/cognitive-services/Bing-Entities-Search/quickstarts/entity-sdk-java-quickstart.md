---
title: "Hızlı Başlangıç: Java için Bing varlık arama SDK'sı ile varlıkları Ara"
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Java için Bing varlık arama SDK'sı ile varlıkları aramak için kullanın
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 15958033ef305a44f9c5254409fa111acd9d1eb5
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65813710"
---
# <a name="quickstart-send-a-search-request-with-the-bing-entity-search-sdk-for-java"></a>Hızlı Başlangıç: Java için Bing varlık arama SDK'sı ile arama isteği gönder

Bu hızlı başlangıçta, Java için Bing varlık arama SDK'sı ile varlıkları için aramaya başlamak için kullanın. Bing varlık arama çoğu programlama dilleri ile uyumlu bir REST API olsa da SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingEntitySearch).

## <a name="prerequisites"></a>Önkoşullar

* [Java geliştirme Kit(JDK)](https://www.oracle.com/technetwork/java/javase/downloads/)

* Java için SDK'sı Bing varlık arama

Bing Varlık Arama SDK'sı bağımlılık dosyalarını Maven, Gradle veya başka bir bağımlılık dosyası yönetim sistemini kullanarak yükleyin. Maven POM dosyası şu bildirimi gerektirir:

```xml
<dependency>
  <groupId>com.microsoft.azure.cognitiveservices</groupId>
  <artifactId>azure-cognitiveservices-entitysearch</artifactId>
  <version>1.0.2</version>
</dependency>
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]


## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.

    ```java
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

2. Abonelik anahtarınız için bir değişken oluşturun

    ```java
    String subscriptionKey = "your-key-here"
    ```

## <a name="create-a-search-client"></a>Bir arama istemcisi oluşturma

1. Uygulama `dominantEntityLookup` API uç noktanız ve bir örneğini gerektirir ve istemci `ServiceClientCredentials` sınıfı.

    ```java
    public static EntitySearchAPIImpl getClient(final String subscriptionKey) {
        return new EntitySearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
                new ServiceClientCredentials() {
                //...
                }
    )};
    ```

    Uygulamak için `ServiceClientCredentials`, şu adımları izleyin:

   1. geçersiz kılma `applyCredentialsFilter()` işlevi ile bir `OkHttpClient.Builder` bir parametre olarak nesne. 
        
       ```java
       //...
       new ServiceClientCredentials() {
               @Override
               public void applyCredentialsFilter(OkHttpClient.Builder builder) {
               //...
               }
       //...
       ```
    
   2. İçinde `applyCredentialsFilter()`, çağrı `builder.addNetworkInterceptor()`. Yeni bir `Interceptor` nesne ve geçersiz kılma kendi `intercept()` gerçekleştirilecek yöntemi bir `Chain` dinleyiciyi nesne.

       ```java
       //...
       builder.addNetworkInterceptor(
           new Interceptor() {
               @Override
               public Response intercept(Chain chain) throws IOException {
               //...    
               }
           });
       ///...
       ```

   3. İçinde `intercept` işlev, isteğiniz için değişkenler oluşturun. Kullanım `Request.Builder()` isteğinizi oluşturmak için. Abonelik anahtarınızı ekleme `Ocp-Apim-Subscription-Key` üst bilgi ve dönüş `chain.proceed()` istek nesnesi üzerinde.
            
       ```java
       //...
       public Response intercept(Chain chain) throws IOException {
           Request request = null;
           Request original = chain.request();
           Request.Builder requestBuilder = original.newBuilder()
                   .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
           request = requestBuilder.build();
           return chain.proceed(request);
       }
       //...
       ```
      ## <a name="send-a-request-and-receive-a-response"></a>Bir istek gönderir ve yanıt

1. Abonelik anahtarınızla arama istemci yeni bir örneğini oluşturun. kullanma `client.entities().search()` arama sorgusu için bir arama talebi göndermek için `satya nadella`ve bir yanıt alın. 
    
    ```java
    EntitySearchAPIImpl client = getClient(subscriptionKey);
    SearchResponseInner entityData = client.entities().search(
            "satya nadella", null, null, null, null, null, null, "en-us", null, null, SafeSearch.STRICT, null);
    ```

1. Herhangi bir varlık döndürülmedi, bunları bir listeye dönüştürün. Bunlar içerisinde yineleme yapmanızı ve baskın varlık yazdırın.

    ```java
    if (entityData.entities().value().size() > 0){
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
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfa uygulaması oluşturma](../tutorial-bing-entities-search-single-page-app.md)

* [Bing varlık arama API'si nedir?](../overview.md )
