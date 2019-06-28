---
title: "Hızlı Başlangıç: Java için Bing Video arama SDK'sını kullanarak video Ara"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Java için Bing Video arama SDK'sını kullanarak video arama istekleri göndermek için kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 06/26/2019
ms.author: aahi
ms.openlocfilehash: 3051f663f277c216fe18513b816bb86478a2efbd
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447000"
---
# <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-java"></a>Hızlı Başlangıç: Java için bir video arama Bing Video arama SDK ile gerçekleştirme

Bu hızlı başlangıçta, Java için Bing Video arama SDK'sı ile haberler için aramaya başlamak için kullanın. Bing Video arama çoğu programlama dilleri ile uyumlu bir REST API olsa da SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingVideoSearch)ilave ek açıklamaların ve özellikleri.

## <a name="prerequisites"></a>Önkoşullar

* [Java geliştirme Kit(JDK)](https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html)

* [Gson kitaplığı](https://github.com/google/gson)

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

Bing Video Arama SDK'sı bağımlılık dosyalarını Maven, Gradle veya başka bir bağımlılık dosyası yönetim sistemini kullanarak yükleyin. Maven POM dosyası şu bildirimi gerektirir:

```xml
  <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.cognitiveservices</groupId>
      <artifactId>azure-cognitiveservices-videosearch</artifactId>
      <version>0.0.1-beta-SNAPSHOT</version>
    </dependency>
  </dependencies> 
```

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma


Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.

    ```java
    import com.microsoft.azure.cognitiveservices.videosearch.*;
    import com.microsoft.azure.cognitiveservices.videosearch.VideoObject;
    import com.microsoft.rest.credentials.ServiceClientCredentials;
    import okhttp3.Interceptor;
    import okhttp3.OkHttpClient;
    import okhttp3.Request;
    import okhttp3.Response;
    import java.io.IOException;
    import java.util.ArrayList;
    import java.util.List; 
    ```

## <a name="create-a-search-client"></a>Bir arama istemcisi oluşturma

1. Uygulama `VideoSearchAPIImpl` API uç noktanız ve bir örneğini gerektirir ve istemci `ServiceClientCredentials` sınıfı.

    ```java
    public static VideoSearchAPIImpl getClient(final String subscriptionKey) {
        return new VideoSearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
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

## <a name="send-a-search-request-and-receive-the-response"></a>Arama isteği gönderme ve yanıt 

1. Çağrılan bir işlev oluşturma `VideoSearch()` , abonelik anahtarınız bir dize olarak alır. Daha önce oluşturduğunuz arama istemci örneği oluşturun.
    
    ```java
    public static void VideoSearch(String subscriptionKey){
        VideoSearchAPIImpl client = VideoSDK.getClient(subscriptionKey);
        //...
    }
    ```
2. İçinde `VideoSearch()`, istemci kullanarak bir video arama isteği Gönder `SwiftKey` arama terimi. Video arama API'si bir sonuç döndürdü, ilk sonucu alın ve kimliği, adı ve URL, döndürülen videoları toplam sayısını yazdırın. 
    
    ```java
    VideosInner videoResults = client.searchs().list("SwiftKey");

    if (videoResults == null){
        System.out.println("Didn't see any video result data..");
    }
    else{
        if (videoResults.value().size() > 0){
            VideoObject firstVideoResult = videoResults.value().get(0);

            System.out.println(String.format("Video result count: %d", videoResults.value().size()));
            System.out.println(String.format("First video id: %s", firstVideoResult.videoId()));
            System.out.println(String.format("First video name: %s", firstVideoResult.name()));
            System.out.println(String.format("First video url: %s", firstVideoResult.contentUrl()));
        }
        else{
            System.out.println("Couldn't find video results!");
        }
    }
    ```

3. Ana yönteminiz arama yöntemi çağırın.

    ```java
    public static void main(String[] args) {
        VideoSDK.VideoSearch("YOUR-SUBSCRIPTION-KEY");
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı bir web uygulaması oluşturma](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

* [Bing Video arama API'si nedir?](../overview.md)
* [Bilişsel Hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)