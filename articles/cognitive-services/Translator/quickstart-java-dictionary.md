---
title: "Hızlı Başlangıç: İki dilli sözlük, Java - Translator metin çevirisi API'si ile sözcük araması"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir dönem için alternatif Çevirileri Alma öğreneceksiniz ve ayrıca bu kullanım örneklerini Çeviriler, Java ve Translator Text API kullanarak diğer.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: erhopf
ms.openlocfilehash: 027e895ffbeb3cc0ff5b3348c2d7a8b76b930cf3
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66514819"
---
# <a name="quickstart-look-up-words-with-bilingual-dictionary-using-java"></a>Hızlı Başlangıç: Java kullanarak iki dilli sözlük ile sözcük arayın

Bu hızlı başlangıçta, bir dönem için alternatif Çevirileri Alma öğreneceksiniz ve ayrıca bu kullanım örneklerini Çeviriler, Java ve Translator Text API kullanarak diğer.

Bu hızlı başlangıç, Translator Metin Çevirisi kaynağına sahip bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerektirir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* [JDK 7 veya üzeri](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Gradle](https://gradle.org/install/)
* Translator Metin Çevirisi için Azure abonelik anahtarı

## <a name="initialize-a-project-with-gradle"></a>Gradle ile bir proje başlatın

Bu proje için çalışan bir dizine oluşturarak başlayalım. Komut satırını (veya terminal), şu komutu çalıştırın:

```console
mkdir alt-translation-sample
cd alt-translation-sample
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
    mainClassName = "AltTranslation"
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
mkdir -p src\main\java
```

Ardından, bu klasörde, adlı bir dosya oluşturun `AltTranslation.java`.

## <a name="import-required-libraries"></a>Gerekli kitaplıkları içeri aktarma

Açık `AltTranslation.java` ve bu içeri aktarma deyimleri:

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
public class AltTranslation {
  // All project code goes here...
}
```

Bu satırları ekleyin `AltTranslation` sınıfı. İle birlikte olduğunu fark edeceksiniz `api-version`, iki ek parametreler eklenmiş için `url`. Bu parametreler, giriş ve çıkış çeviri ayarlamak için kullanılır. Bu örnekte, bu İngilizce (`en`) ve İspanyolca (`es`).

```java
String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
String url = "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es";
```

## <a name="create-a-client-and-build-a-request"></a>Bir istemci oluşturup bir istek oluşturun

Bu satırı `AltTranslation` sınıfı örneğini oluşturmak için `OkHttpClient`:

```java
// Instantiates the OkHttpClient.
OkHttpClient client = new OkHttpClient();
```

Ardından, bir POST isteği oluşturalım. Çeviri için metni değiştirme çekinmeyin.

```java
// This function performs a POST request.
public String Post() throws IOException {
    MediaType mediaType = MediaType.parse("application/json");
    RequestBody body = RequestBody.create(mediaType,
            "[{\n\t\"Text\": \"Pineapples\"\n}]");
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
        AltTranslation altTranslationRequest = new AltTranslation();
        String response = altTranslationRequest.Post();
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

```json
[
  {
    "normalizedSource": "pineapples",
    "displaySource": "pineapples",
    "translations": [
      {
        "normalizedTarget": "piñas",
        "displayTarget": "piñas",
        "posTag": "NOUN",
        "confidence": 0.7016,
        "prefixWord": "",
        "backTranslations": [
          {
            "normalizedText": "pineapples",
            "displayText": "pineapples",
            "numExamples": 5,
            "frequencyCount": 158
          },
          {
            "normalizedText": "cones",
            "displayText": "cones",
            "numExamples": 5,
            "frequencyCount": 13
          },
          {
            "normalizedText": "piña",
            "displayText": "piña",
            "numExamples": 3,
            "frequencyCount": 5
          },
          {
            "normalizedText": "ganks",
            "displayText": "ganks",
            "numExamples": 2,
            "frequencyCount": 3
          }
        ]
      },
      {
        "normalizedTarget": "ananás",
        "displayTarget": "ananás",
        "posTag": "NOUN",
        "confidence": 0.2984,
        "prefixWord": "",
        "backTranslations": [
          {
            "normalizedText": "pineapples",
            "displayText": "pineapples",
            "numExamples": 2,
            "frequencyCount": 16
          }
        ]
      }
    ]
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Çeviri ve başka alfabeye çevirme gibi bu ve diğer hızlı başlangıçlardaki örnek kodlarla birlikte GitHub’daki diğer örnek Translator Metin Çevirisi projelerini keşfedin.

> [!div class="nextstepaction"]
> [GitHub’da Java örneklerini keşfedin](https://aka.ms/TranslatorGitHub?type=&language=java)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin çevirme](quickstart-java-translate.md)
* [Metni başka dilde yazma](quickstart-java-transliterate.md)
* [Girişe göre dili belirleyin](quickstart-java-detect.md)
* [Desteklenen dillerin listesini alma](quickstart-java-languages.md)
* [Girişten tümce uzunluklarını belirleme](quickstart-java-sentences.md)
