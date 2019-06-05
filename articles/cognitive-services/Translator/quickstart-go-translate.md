---
title: "Hızlı Başlangıç: Metni Çevir Git - Translator metin çevirisi API'si"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, metin bir dilden diğerine Translator metin çevirisi API'si, 10 dakikadan az Go kullanarak çevir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: erhopf
ms.openlocfilehash: cf0a7598d7af583e3339c511556a121523d12a7a
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66514882"
---
# <a name="quickstart-use-the-translator-text-api-to-translate-a-string-using-go"></a>Hızlı Başlangıç: Go kullanarak bir dize çevirmek için Translator Text API kullanın

Bu hızlı başlangıçta, İtalyanca ve Almanca Go ve Translator Text REST API kullanarak bir metin dizesi İngilizce'den Çevir öğreneceksiniz.

Bu hızlı başlangıç, Translator Metin Çevirisi kaynağına sahip bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerektirir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Go](https://golang.org/doc/install)
* Translator Metin Çevirisi için Azure abonelik anahtarı

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

Sık kullandığınız IDE veya düzenleyiciyi kullanarak yeni bir Git projesi oluşturun. Ardından bu kod parçacığını projenizde `translate-text.go` adlı bir dosyaya kopyalayın.

```go
package main

import (
    "bytes"
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
     * want to use env variables. If so, be sure to delete the "os" import.
     */
    subscriptionKey := os.Getenv("TRANSLATOR_TEXT_KEY")
    if subscriptionKey == "" {
       log.Fatal("Environment variable TRANSLATOR_TEXT_KEY is not set.")
    }
    /*
     * This calls our translate function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    translate(subscriptionKey)
}
```

## <a name="create-a-function-to-translate-text"></a>Metni çevirmek için bir işlev oluşturma

Metni çevirmek için bir işlev oluşturalım. Bu işlev, tek bir bağımsız değişken, Translator metin çevirisi abonelik anahtarınızı olacaktır.

```go
func translate(subscriptionKey string) {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

Ardından, şimdi URL'sini oluşturun. URL kullanılarak oluşturulan `Parse()` ve `Query()` yöntemleri. Parametreler ile eklenen fark edeceksiniz `Add()` yöntemi. Bu örnekte, İngilizce ' Almanca ve İtalyanca çevirme: `de` ve `it`.

Bu kodu kopyalayın `translate` işlevi.

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse("https://api.cognitive.microsofttranslator.com/translate?api-version=3.0")
q := u.Query()
q.Add("to", "de")
q.Add("to", "it")
u.RawQuery = q.Encode()
```

>[!NOTE]
> Uç noktaları, yollar ve istek parametreleri hakkında daha fazla bilgi için bkz: [Translator Text API 3.0: Çevirme](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate).

## <a name="create-a-struct-for-your-request-body"></a>Bir yapı, istek gövdesi için oluşturma

Ardından, istek gövdesi için anonim bir yapı oluşturmak ve JSON olarak kodlama `json.Marshal()`. Bu kodu ekleyin `translate` işlevi.

```go
// Create an anonymous struct for your request body and encode it to JSON
body := []struct {
    Text string
}{
    {Text: "Hello, world!"},
}
b, _ := json.Marshal(body)
```

## <a name="build-the-request"></a>Derleme isteği

İstek gövdesi JSON olarak kodlanmış, derleme, bir POST isteği ve Translator Text API çağrısı.

```go
// Build the HTTP POST request
req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
if err != nil {
    log.Fatal(err)
}
// Add required headers to the request
req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
req.Header.Add("Content-Type", "application/json")

// Call the Translator Text API
res, err := http.DefaultClient.Do(req)
if err != nil {
    log.Fatal(err)
}
```

## <a name="handle-and-print-the-response"></a>İşlemek ve yanıt yazdırma

Bu kodu ekleyin `translate` işlevi JSON yanıt kodunu çözme ve sonra biçimlendirmek ve sonucu yazdırır.

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
go run translate-text.go
```

Kodunuzu bizimkiyle karşılaştırmak isterseniz, tam örnek kodu [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go)’da bulabilirsiniz.

## <a name="sample-response"></a>Örnek yanıt

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "translations": [
      {
        "text": "Hallo Welt!",
        "to": "de"
      },
      {
        "text": "Salve, mondo!",
        "to": "it"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Bilişsel hizmetler API'leri için go örnekleri keşfedin [Go için Azure SDK](https://github.com/Azure/azure-sdk-for-go) GitHub üzerinde.

> [!div class="nextstepaction"]
> [Github'da go örnekleri keşfedin](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)

## <a name="see-also"></a>Ayrıca bkz.

Translator metin çevirisi API'si için kullanmayı öğrenin:

* [Metni başka dilde yazma](quickstart-go-transliterate.md)
* [Girişe göre dili belirleyin](quickstart-go-detect.md)
* [Alternatif çeviriler edinme](quickstart-go-dictionary.md)
* [Desteklenen dillerin listesini alma](quickstart-go-languages.md)
* [Girişten tümce uzunluklarını belirleme](quickstart-go-sentences.md)
