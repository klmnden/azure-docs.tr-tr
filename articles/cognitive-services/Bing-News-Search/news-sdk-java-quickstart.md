---
title: Bing Haberler arama SDK Java hızlı başlangıç | Microsoft Docs
description: Bing Haberler arama SDK konsol uygulama ayarlama öğrenin.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 02/16/2018
ms.author: v-gedod
ms.openlocfilehash: a6d4baf307fa3edcc0886d32204f2872fe310ce2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355265"
---
# <a name="bing-news-search-sdk-java-quickstart"></a>Bing Haberler arama SDK Java hızlı başlangıç

Bing Haberler arama SDK'sı haber sorgular ve ayrıştırma sonuçları için REST API işlevsellik sağlar. 

[Kaynak kodu Java Bing Haberler arama SDK örnekleri için](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingNewsSearch) Git hub'da kullanılabilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları
Alma bir [Bilişsel hizmetler erişim tuşu](https://azure.microsoft.com/try/cognitive-services/) altında **arama**. Bing Haberler arama SDK bağımlılığı Maven, Gradle veya başka bir bağımlılık yönetimi sistemi kullanarak yükleyin. Maven POM dosya bildirimi gerektirir:
```
  <dependencies>
    <dependency>
        <groupId>com.microsoft.azure.cognitiveservices</groupId>
        <artifactId>azure-cognitiveservices-newssearch</artifactId>
        <version>0.0.1-beta-SNAPSHOT</version>
    </dependency>
  </dependencies>
```
## <a name="news-search-client"></a>Haber Arama İstemcisi
İçeri aktarmalar için sınıf uygulamasını ekleyin.
```
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
Uygulama **NewsSearchAPIImpl** bir örneğini gerektirir istemci **ServiceClientCredentials** sınıfı.
```
public static NewsSearchAPIImpl getClient(final String subscriptionKey) {
    return new NewsSearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
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
}


```
Tek bir sorgu "hisse hesaplanıyor." ile haber arayın Arama ile filtre *Pazar* ve *sayısı* parametreleri. Sonuç sayısını doğrulayın. İlk haber sonucu hakkındaki bilgileri yazdırma: ad, URL, yayın tarihi, açıklama, sağlayıcı adı ve tahmini eşleşme toplam sayısı.
```
public static void newsSearch(String subscriptionKey)
{
    NewsSearchAPIImpl client = getClient(subscriptionKey);

    try
    {
        NewsInner newsResults = client.searchs().list("Quantum  Computing", null, null, null,
                null, null, 100, null, "en-us",
                null, null, null, null, null,
                null, null);

        System.out.println("\r\nSearch news for query \"Quantum  Computing\" with market and count");

        if (newsResults == null)
        {
            System.out.println("Didn't see any news result data..");
        }
        else
        {
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
    }

    catch (Exception ex)
    {
        System.out.println("Encountered exception. " + ex.getLocalizedMessage());
    }
}

```
"Yapay zeka." hakkındaki son haberleri arayın Arama ile filtre *yenilik* ve *sortBy* parametreleri. Sonuç sayısını doğrulayın. İlk haber sonucu hakkındaki bilgileri yazdırma: ad, URL, yayın tarihi, açıklama, sağlayıcı adı ve tahmini eşleşme toplam sayısı.
```
/**
 * Search recent news for (Artificial Intelligence) with the freshness and sortBy parameters.
 * Verify the number of results. Print the totalEstimatedMatches, name, url, description,
 * published time, and provider name for the first news result.
 * @param subscriptionKey cognitive services subscription key
 */
public static void newsSearchWithFilters(String subscriptionKey)
{
    NewsSearchAPIImpl client = getClient(subscriptionKey);

    try
    {
        NewsInner newsResults = client.searchs().list("Artificial Intelligence", null, null, null, null, null,
                    null, Freshness.WEEK, "en-us", null, null, null,
                    null, "Date", null, null);
        System.out.println("\r\nSearch most recent news for query \"Artificial Intelligence\" with freshness and sortBy");

        if (newsResults == null)
        {
            System.out.println("Didn't see any news result data..");
        }
        else
        {
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
    }

    catch (Exception ex)
    {
        System.out.println("Encountered exception. " + ex.getLocalizedMessage());
    }
}

```
Haber arama **kategori** için *film ve TV eğlence* konuları ve kullanım *güvenli arama* özelliği. Sonuç sayısını doğrulayın. Kategori, ad, URL, açıklama, yayın tarihi ve ilk haber sonuç için sağlayıcı adı yazdırın.
```
/**
 * Search the news category for (movie and TV entertainment) with safe search. Verify the number of results. 
 * Print the category, name, url, description, published time, and provider name for the first news result.
 * @param subscriptionKey cognitive services subscription key
 */
public static void newsCategory(String subscriptionKey)
{
    NewsSearchAPIImpl client = getClient(subscriptionKey);

    try
    {
        NewsInner newsResults = client.categorys().list(null, null, null, null, null, "Entertainment_MovieAndTV",
                null, null, "en-us", null, null, SafeSearch.STRICT,
                null, null, null);
        System.out.println("\r\nSearch category news for movie and TV entertainment with safe search");

        if (newsResults == null)
        {
            System.out.println("Didn't see any news result data..");
        }
        else
        {
            if (newsResults.value().size() > 0)
            {
                NewsArticle firstNewsResult = newsResults.value().get(0);

                System.out.println(String.format("News result count: %d", newsResults.value().size()));
                //System.out.println(String.format("First news category: %d", firstNewsResult.category()));
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
    }

    catch (Exception ex)
    {
        System.out.println("Encountered exception. " + ex.getLocalizedMessage()
        );
    }
}

```
Haberlerin eğilimleri belirlemek için arama yapın. Sonuç sayısını doğrulayın. Ad, sorgu metni, web arama URL'si ve ilk haber sonuç haber arama URL'sini yazdırın.
```
public static void trendingTopics(String subscriptionKey)
{
    NewsSearchAPIImpl client = getClient(subscriptionKey);

    try
    {
        TrendingTopicsInner trendingTopics = client.trendings().list(null, null, null, null, null, null,
                "en-us", null, null, null, null, null, null, null);
        System.out.println("\r\nSearch news trending topics in Bing");

        if (trendingTopics == null)
        {
            System.out.println("Didn't see any news trending topics..");
        }
        else
        {
            if (trendingTopics.value().size() > 0)
            {
                NewsTopic firstTopic = trendingTopics.value().get(0);

                System.out.println(String.format("Trending topics count: %s", trendingTopics.value().size()));
                System.out.println(String.format("First topic name: %s", firstTopic.name()));
                System.out.println(String.format("First topic query: %s", firstTopic.query().text()));
                System.out.println(String.format("First topic image url: %s", firstTopic.image().url()));
                System.out.println(String.format("First topic webSearchUrl: %s", firstTopic.webSearchUrl()));
                System.out.println(String.format("First topic newsSearchUrl: %s", firstTopic.newsSearchUrl()));
            }
            else
            {
                System.out.println("Couldn't find news trending topics!");
            }
        }
    }

    catch (Exception ex)
    {
        System.out.println("Encountered exception. " + ex.getLocalizedMessage());
    }
}
```
Bir sınıfa kod yürütmek için ana işleviyle bu makalede açıklanan yöntemleri ekleyin.
```
package javaNewsSDK;
import com.microsoft.azure.cognitiveservices.newssearch.*;

public class NewsSearchSDK {
    

    public static void main(String[] args) {
        String subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
        NewsSearchSDK.newsSearch("YOUR-SUBSCRIPTION-KEY");
        NewsSearchSDK.newsSearchWithFilters("YOUR-SUBSCRIPTION-KEY");
        NewsSearchSDK.newsCategory("YOUR-SUBSCRIPTION-KEY");
        NewsSearchSDK.trendingTopics("YOUR-SUBSCRIPTION-KEY");
    }

    // Include the methods described in this article.
}
```
## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler Java SDK'sı örneği](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)


