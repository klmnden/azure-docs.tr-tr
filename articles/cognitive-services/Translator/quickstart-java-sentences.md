---
title: "Hızlı Başlangıç: Translator metin çevirisi API'si cümlesi işlediklerinde, Java - al"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Java ve Translator Text API kullanarak cümle uzunluğu belirlemek öğreneceksiniz.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: e47bbd4f50a1521eebc15f3f812f31699a7ddf13
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704484"
---
# <a name="quickstart-use-the-translator-text-api-to-determine-sentence-length-using-java"></a>Hızlı Başlangıç: Translator metin çevirisi API'si, Java kullanarak cümle belirlemek için kullanın

Bu hızlı başlangıçta, Java ve Translator Text API kullanarak cümle uzunlukları belirlemek öğreneceksiniz.

Bu hızlı başlangıç, Translator Metin Çevirisi kaynağına sahip bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerektirir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

>[!TIP]
> Tek seferde tüm kodu görmek istiyorsanız, bu örnek için kaynak kodunu şurada bulunur [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Java).

## <a name="prerequisites"></a>Önkoşullar

* [JDK 7 veya üzeri](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Gradle](https://gradle.org/install/)
* Translator Metin Çevirisi için Azure abonelik anahtarı

## <a name="initialize-a-project-with-gradle"></a>Gradle ile bir proje başlatın

Bu proje için çalışan bir dizine oluşturarak başlayalım. Komut satırını (veya terminal), şu komutu çalıştırın:

```console
mkdir length-sentence-sample
cd length-sentence-sample
```

Ardından, Gradle proje başlatmak için dağıtacağız. Bu komut temel yapı dosyalarını Gradle için en önemlisi de oluşturacak `build.gradle.kts`, oluşturma ve uygulamanızı yapılandırmak için çalışma zamanında kullanılır. Çalışma dizininizdeki şu komutu çalıştırın:

```console
gradle init --type basic
```

Seçmeniz istendiğinde bir **DSL**seçin **Kotlin**.

## <a name="configure-the-build-file"></a>Derleme dosyası yapılandırma

Bulun `build.gradle.kts` ve en sevdiğiniz IDE ya da metin düzenleyici ile açın. Ardından bu yapı yapılandırması içindeki kopyalayın:

```
plugins {
    java
    application
}
application {
    mainClassName = "LengthSentence"
}
repositories {
    mavenCentral()
}
dependencies {
    compile("com.squareup.okhttp:okhttp:2.5.0")
    compile("com.google.code.gson:gson:2.8.5")
}
```

Bu örnek OkHttp HTTP isteklerini ve Gson işlemek ve JSON ayrıştırmak bağımlılıkları olduğunu not edin. Derleme yapılandırmaları hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [yeni Gradle derleme oluşturma](https://guides.gradle.org/creating-new-gradle-builds/).

## <a name="create-a-java-file"></a>Bir Java dosyası oluşturma

Şimdi örnek uygulamanız için bir klasör oluşturun. Çalışma dizininizden çalıştırın:

```console
mkdir -p src/main/java
```

Ardından, bu klasörde, adlı bir dosya oluşturun `LengthSentence.java`.

## <a name="import-required-libraries"></a>Gerekli kitaplıkları içeri aktarma

Açık `LengthSentence.java` ve bu içeri aktarma deyimleri:

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;
```


## <a name="define-variables"></a>Değişkenleri tanımlama

İlk olarak, projeniz için genel bir sınıf oluşturmanız gerekir:

```java
public class LengthSentence {
  // All project code goes here...
}
```

Bu satırları ekleyin `LengthSentence` sınıfı. İle birlikte olduğunu fark edeceksiniz `api-version`, giriş dili tanımlayabilirsiniz. Bu örnekte, İngilizce olarak.

```java
String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
String url = "https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0&language=en";
```
Bilişsel hizmetler çok hizmet aboneliği kullanıyorsanız de içermelidir `Ocp-Apim-Subscription-Region` , istek parametreleri. [Birden çok hizmet aboneliği ile kimlik doğrulaması hakkında daha fazla bilgi](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="create-a-client-and-build-a-request"></a>Bir istemci oluşturup bir istek oluşturun

Bu satırı `LengthSentence` sınıfı örneğini oluşturmak için `OkHttpClient`:

```java
// Instantiates the OkHttpClient.
OkHttpClient client = new OkHttpClient();
```

Ardından, bir POST isteği oluşturalım. Metni Değiştir çekinmeyin. Metin kaçış karakterleri eklenmelidir.

```java
// This function performs a POST request.
public String Post() throws IOException {
    MediaType mediaType = MediaType.parse("application/json");
    RequestBody body = RequestBody.create(mediaType,
            "[{\n\t\"Text\": \"How are you? I am fine. What did you do today?\"\n}]");
    Request request = new Request.Builder()
            .url(url).post(body)
            .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey)
            .addHeader("Content-type", "application/json").build();
    Response response = client.newCall(request).execute();
    return response.body().string();
}
```

## <a name="create-a-function-to-parse-the-response"></a>Yanıtları ayrıştırmak için bir işlev oluşturma

Bu basit işlev ayrıştırır ve Translator metin tanıma hizmetinden JSON yanıtı prettifies.

```java
// This function prettifies the json response.
public static String prettify(String json_text) {
    JsonParser parser = new JsonParser();
    JsonElement json = parser.parse(json_text);
    Gson gson = new GsonBuilder().setPrettyPrinting().create();
    return gson.toJson(json);
}
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Bir istekte bulunmak ve yanıt almak için son adımdır bakın. Bu satırlar, projenize ekleyin:

```java
public static void main(String[] args) {
    try {
        LengthSentence lengthSentenceRequest = new LengthSentence();
        String response = lengthSentenceRequest.Post();
        System.out.println(prettify(response));
    } catch (Exception e) {
        System.out.println(e);
    }
}
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu kadar örnek uygulamanızı çalıştırmak hazır olursunuz. Komut satırını (veya terminal oturumu), çalışma dizininizin kök dizinine gidin ve çalıştırın:

```console
gradle build
```

Derleme tamamlandığında çalıştırın:

```console
gradle run
```

## <a name="sample-response"></a>Örnek yanıt

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "sentLen": [
      13,
      11,
      22
    ]
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Translator metin çevirisi API'si ile yapabileceğiniz her şeyi anlamak için API Başvurusu göz atın.

> [!div class="nextstepaction"]
> [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin çevirme](quickstart-java-translate.md)
* [Metni başka dilde yazma](quickstart-java-transliterate.md)
* [Girişe göre dili belirleyin](quickstart-java-detect.md)
* [Alternatif çeviriler edinme](quickstart-java-dictionary.md)
* [Desteklenen dillerin listesini alma](quickstart-java-languages.md)
* [Girişten tümce uzunluklarını belirleme](quickstart-java-sentences.md)
