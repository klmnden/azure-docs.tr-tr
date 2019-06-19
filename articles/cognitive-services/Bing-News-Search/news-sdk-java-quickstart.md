---
title: "Hızlı Başlangıç: Java için Bing haber arama SDK'sını kullanarak bir haber arama yapın"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Java için Bing haber arama SDK'sını kullanarak haber aramak için kullanın ve işlem yanıt.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 06/18/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: d84b47feb91a9165a4bc03b20b0b7d079aa8f6ae
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203364"
---
# <a name="quickstart-search-for-news-with-the-bing-news-search-sdk-for-java"></a>Hızlı Başlangıç: Java için Bing haber arama SDK'sı ile haberler için arama

Bu hızlı başlangıçta, Java için Bing haber arama SDK'sı ile haberler için aramaya başlamak için kullanın. Bing haber arama çoğu programlama dilleri ile uyumlu bir REST API olsa da SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingNewsSearch).

## <a name="prerequisites"></a>Önkoşullar

Maven, Gradle veya başka bir bağımlılık yönetim sistemi kullanarak Bing haber arama SDK bağımlılıklarını yükleyin. Maven POM dosyası şu bildirimi gerektirir:

```xml
    <dependencies>
    <dependency>
        <groupId>com.microsoft.azure.cognitiveservices</groupId>
        <artifactId>azure-cognitiveservices-newssearch</artifactId>
        <version>0.0.1-beta-SNAPSHOT</version>
    </dependency>
    </dependencies>
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.

```java
import com.microsoft.azure.cognitiveservices.newssearch.*;
import com.microsoft.azure.cognitiveservices.newssearch.implementation.NewsInner;
import com.microsoft.azure.cognitiveservices.newssearch.implementation.NewsSearchAPIImpl;
import com.microsoft.azure.cognitiveservices.newssearch.implementation.TrendingTopicsInner;
import com.microsoft.rest.credentials.ServiceClientCredentials;
import okhttp3.Interceptor;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;
import java.io.IOException;
```

## <a name="create-a-search-client-and-store-credentials"></a>Bir arama istemci ve depolama kimlik bilgileri oluştur

1. Adlı bir yöntem oluşturma `getClient()` yeni döndüren `NewsSearchAPIImpl` arama istemci. İlk parametre olarak yeni uç noktanızı eklemeyi`NewsSearchAPIImpl` nesne ve yeni bir `ServiceClientCredentials` kimlik bilgilerinizi depolamak için nesne.

    ```java
    public static NewsSearchAPIImpl getClient(final String subscriptionKey) {
        return new NewsSearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
                new ServiceClientCredentials() {
                });
    }
    ```

2. Oluşturulacak `ServiceClientCredentials` nesne, geçersiz kılma `applyCredentialsFilter()` işlevi. Başarılı bir `OkHttpClient.Builder` yöntemi ve kullanım Oluşturucu 's `addNetworkInterceptor()` SDK çağrısı için kimlik bilgilerinizi oluşturmak için yöntemi.

    ```java
    new ServiceClientCredentials() {
        @Override
        public void applyCredentialsFilter(OkHttpClient.Builder builder) {
            builder.addNetworkInterceptor(
                    new Interceptor() {
                        @Override
                        public Response intercept(Chain chain) throws IOException {
                            Request request = null;
                            Request original = chain.request();
                            // Request customization: add request headers.
                            Request.Builder requestBuilder = original.newBuilder()
                                    .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
                            request = requestBuilder.build();
                            return chain.proceed(request);
                        }
                    });
        }
    });
    ```

## <a name="send-and-receive-a-search-request"></a>Arama isteği gönderip

1. Çağıran bir yöntem oluşturma `getClient()` ve Bing haber arama hizmetine arama isteği gönderir. Arama filtre *Pazar* ve *sayısı* parametreleri, ilk haber sonucu ile ilgili bilgiler daha sonra yazdırma: ad, URL, yayın tarihi, açıklama, sağlayıcı adı ve toplam sayısı tahmini Aramanız için eşleşmeleri.

    ```java
    public static void newsSearch(String subscriptionKey)
    {
        NewsSearchAPIImpl client = getClient(subscriptionKey);
        String searchTerm = "Quantum Computing";
    
        NewsInner newsResults = client.searchs().list(searchTerm, null, null, null,
                null, null, 100, null, "en-us",
                null, null, null, null, null,
                null, null);
    
        if (newsResults.value().size() > 0)
        {
            NewsArticle firstNewsResult = newsResults.value().get(0);
    
            System.out.println(String.format("TotalEstimatedMatches value: %d", newsResults.totalEstimatedMatches()));
            System.out.println(String.format("News result count: %d", newsResults.value().size()));
            System.out.println(String.format("First news name: %s", firstNewsResult.name()));
            System.out.println(String.format("First news url: %s", firstNewsResult.url()));
            System.out.println(String.format("First news description: %s", firstNewsResult.description()));
            System.out.println(String.format("First news published time: %s", firstNewsResult.datePublished()));
            System.out.println(String.format("First news provider: %s", firstNewsResult.provider().get(0).name()));
        }
        else
        {
            System.out.println("Couldn't find news results!");
        }
    
    }
    
    ```

2. Arama yönteminize ekleyin bir `main()` kod yürütmek için yöntemi.

    ```java 
    public static void main(String[] args) {
        String subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
        NewsSearchSDK.newsSearch(subscriptionKey);
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](tutorial-bing-news-search-single-page-app.md)
