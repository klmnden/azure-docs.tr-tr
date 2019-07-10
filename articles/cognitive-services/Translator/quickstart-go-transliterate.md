---
title: "Hızlı Başlangıç: Metin dönüştürme komut dosyası, Git - Translator metin çevirisi API'si"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Go ile Translator Metin Çevirisi API’sini kullanarak bir dildeki metni bir betikten diğerine dönüştüreceksiniz.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 4e91cffbc08b145e77141e6f09bbc57651b94149
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704505"
---
# <a name="quickstart-use-the-translator-text-api-to-transliterate-text-using-go"></a>Hızlı Başlangıç: Translator metin çevirisi API'si, Go kullanarak metin alfabeye için kullanın

Bu hızlı başlangıçta, Translator Metin Çevirisi API'sini kullanarak bir dildeki metni bir betikten diğerine dönüştüreceksiniz.

Bu hızlı başlangıç, Translator Metin Çevirisi kaynağına sahip bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerektirir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

>[!TIP]
> Tek seferde tüm kodu görmek istiyorsanız, bu örnek için kaynak kodunu şurada bulunur [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Go](https://golang.org/doc/install)
* Translator Metin Çevirisi için Azure abonelik anahtarı

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

Sık kullandığınız IDE veya düzenleyiciyi kullanarak yeni bir Git projesi oluşturun. Ardından bu kod parçacığını projenizde `transliterate-text.go` adlı bir dosyaya kopyalayın.

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
     * This calls our transliterate function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    transliterate(subscriptionKey)
}
```

## <a name="create-a-function-to-transliterate-text"></a>Metin alfabeye için bir işlev oluşturma

Bir işlev metin alfabeye oluşturalım. Bu işlev, tek bir bağımsız değişken, Translator metin çevirisi abonelik anahtarınızı olacaktır.

```go
func transliterate(subscriptionKey string) {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

Ardından, şimdi URL'sini oluşturun. URL kullanılarak oluşturulan `Parse()` ve `Query()` yöntemleri. Parametreler ile eklenen fark edeceksiniz `Add()` yöntemi. Bu örnekte Japonca metni Latin alfabesine dönüştürüyoruz.

Bu kodu kopyalayın `transliterate` işlevi.

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse("https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0")
q := u.Query()
q.Add("language", "ja")
q.Add("fromScript", "jpan")
q.Add("toScript", "latn")
u.RawQuery = q.Encode()
```

>[!NOTE]
> Uç noktaları, yollar ve istek parametreleri hakkında daha fazla bilgi için bkz: [Translator Text API 3.0: Alfabeye](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-transliterate).

## <a name="create-a-struct-for-your-request-body"></a>Bir yapı, istek gövdesi için oluşturma

Ardından, istek gövdesi için anonim bir yapı oluşturmak ve JSON olarak kodlama `json.Marshal()`. Bu kodu ekleyin `transliterate` işlevi.

```go
// Create an anonymous struct for your request body and encode it to JSON
body := []struct {
    Text string
}{
    {Text: "こんにちは"},
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

Bilişsel hizmetler çok hizmet aboneliği kullanıyorsanız de içermelidir `Ocp-Apim-Subscription-Region` , istek parametreleri. [Birden çok hizmet aboneliği ile kimlik doğrulaması hakkında daha fazla bilgi](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="handle-and-print-the-response"></a>İşlemek ve yanıt yazdırma

Bu kodu ekleyin `transliterate` işlevi JSON yanıt kodunu çözme ve sonra biçimlendirmek ve sonucu yazdırır.

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
go run transliterate-text.go
```

Kodunuzu bizimkiyle karşılaştırmak isterseniz, tam örnek kodu [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go)’da bulabilirsiniz.

## <a name="sample-response"></a>Örnek yanıt

```json
[
  {
    "script": "latn",
    "text": "konnichiwa"
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Translator metin çevirisi API'si ile yapabileceğiniz her şeyi anlamak için API Başvurusu göz atın.

> [!div class="nextstepaction"]
> [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)

## <a name="see-also"></a>Ayrıca bkz.

Translator metin çevirisi API'si için kullanmayı öğrenin:

* [Metin çevirme](quickstart-go-translate.md)
* [Girişe göre dili belirleyin](quickstart-go-detect.md)
* [Alternatif çeviriler edinme](quickstart-go-dictionary.md)
* [Desteklenen dillerin listesini alma](quickstart-go-languages.md)
* [Girişten tümce uzunluklarını belirleme](quickstart-go-sentences.md)
