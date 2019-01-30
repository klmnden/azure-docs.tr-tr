---
title: "Hızlı Başlangıç: Desteklenen diller, Git - Translator metin çevirisi API'si"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Go ile Translator Metin Çevirisi API’sini kullanarak çeviri, başka alfabeye çevirme ve sözlük arama için desteklenen dillerin ve örneklerin bir listesini alacaksınız.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 12/05/2018
ms.author: erhopf
ms.openlocfilehash: 45dcd87910e0dbfc57aa09751cbdaa7a043d7cf1
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226664"
---
# <a name="quickstart-use-the-translator-text-api-to-get-a-list-of-supported-languages-using-go"></a>Hızlı Başlangıç: Translator metin çevirisi API'si, Go kullanarak desteklenen dillerin listesini almak için kullanın

Bu hızlı başlangıçta, Git ve Translator Text REST API kullanarak desteklenen dillerin listesi döndüren bir GET isteğinde bulunmak öğreneceksiniz.

Bu hızlı başlangıç, Translator Metin Çevirisi kaynağına sahip bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerektirir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Go](https://golang.org/doc/install)
* Translator Metin Çevirisi için Azure abonelik anahtarı

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

Sık kullandığınız IDE veya düzenleyiciyi kullanarak yeni bir Git projesi oluşturun. Ardından bu kod parçacığını projenizde `get-languages.go` adlı bir dosyaya kopyalayın.

```go
package main

import (
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
    "os"
)
```

## <a name="create-the-main-function"></a>Main işlevi oluşturma

Bu örnek, `TRANSLATOR_TEXT_KEY` ortam değişkeninden Translator Metin Çevirisi abonelik anahtarınızı okumaya çalışır. Ortam değişkenlerini bilmiyorsanız, `subscriptionKey` öğesini dize olarak ayarlayabilir ve koşul deyimini açıklama satırı yapabilirsiniz.

Bu kodu projenize kopyalayın:

```go
func main() {
    /*
     * Read your subscription key from an env variable.
     * Please note: You can replace this code block with
     * var subscriptionKey = "YOUR_SUBSCRIPTION_KEY" if you don't
     * want to use env variables.
     */
    subscriptionKey := os.Getenv("TRANSLATOR_TEXT_KEY")
    if subscriptionKey == "" {
       log.Fatal("Environment variable TRANSLATOR_TEXT_KEY is not set.")
    }
    /*
     * This calls our getLanguages function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    getLanguages(subscriptionKey)
}
```

## <a name="create-a-function-to-get-a-list-of-supported-languages"></a>Desteklenen dillerin listesini almak için bir işlev oluşturma

Desteklenen dillerin listesini almak için bir işlevi oluşturalım. Bu işlev, tek bir bağımsız değişken, Translator metin çevirisi abonelik anahtarınızı olacaktır.

```go
func getLanguages(subscriptionKey string) {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

Ardından, şimdi URL'sini oluşturun. URL kullanılarak oluşturulan `Parse()` ve `Query()` yöntemleri.

Bu kodu kopyalayın `getLanguages` işlevi.

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse("https://api.cognitive.microsofttranslator.com/languages?api-version=3.0")
q := u.Query()
u.RawQuery = q.Encode()
```

>[!NOTE]
> Uç noktaları, yollar ve istek parametreleri hakkında daha fazla bilgi için bkz: [Translator Text API 3.0: Diller](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages).

## <a name="build-the-request"></a>Derleme isteği

İstek gövdesi JSON olarak kodlanmış, derleme, bir POST isteği ve Translator Text API çağrısı.

```go
// Build the HTTP GET request
req, err := http.NewRequest("GET", u.String(), nil)
if err != nil {
    log.Fatal(err)
}
// Add required headers
req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
req.Header.Add("Content-Type", "application/json")

// Call the Translator Text API
res, err := http.DefaultClient.Do(req)
if err != nil {
    log.Fatal(err)
}
```

## <a name="handle-and-print-the-response"></a>İşlemek ve yanıt yazdırma

Bu kodu ekleyin `getLanguages` işlevi JSON yanıt kodunu çözme ve sonra biçimlendirmek ve sonucu yazdırır.

```go
// Decode the JSON response
var result interface{}
if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
    log.Fatal(err)
}
// Format and print the response to terminal
prettyJSON, _ := json.MarshalIndent(result, "", "  ")
fmt.Printf("%s\n", prettyJSON)
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

İşte bu kadar! Translator Metin Çevirisi API'sini çağıran ve bir JSON yanıtı döndüren basit bir program oluşturdunuz. Artık programınızı çalıştırmak zamanı geldi:

```console
go run get-languages.go
```

Kodunuzu bizimkiyle karşılaştırmak isterseniz, tam örnek kodu [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go)’da bulabilirsiniz.

## <a name="sample-response"></a>Örnek yanıt

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
{
  "dictionary": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
...
  },
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    },
...
  },
  "transliteration": {
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "scripts": [
        {
          "code": "Arab",
          "name": "Arabic",
          "nativeName": "العربية",
          "dir": "rtl",
          "toScripts": [
            {
              "code": "Latn",
              "name": "Latin",
              "nativeName": "اللاتينية",
              "dir": "ltr"
            }
          ]
        },
        {
          "code": "Latn",
          "name": "Latin",
          "nativeName": "اللاتينية",
          "dir": "ltr",
          "toScripts": [
            {
              "code": "Arab",
              "name": "Arabic",
              "nativeName": "العربية",
              "dir": "rtl"
            }
          ]
        }
      ]
    },
...
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bilişsel Hizmetler API'leri için GitHub’da [Go için Azure SDK](https://github.com/Azure/azure-sdk-for-go)’dan Go paketlerini keşfedin.

> [!div class="nextstepaction"]
> [GitHub’da Go paketlerini keşfedin](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)

## <a name="see-also"></a>Ayrıca bkz.

Translator metin çevirisi API'si için kullanmayı öğrenin:

* [Metin çevirme](quickstart-go-translate.md)
* [Metni başka dilde yazma](quickstart-go-transliterate.md)
* [Girişe göre dili belirleyin](quickstart-go-detect.md)
* [Alternatif çeviriler edinme](quickstart-go-dictionary.md)
* [Girişten tümce uzunluklarını belirleme](quickstart-go-sentences.md)
