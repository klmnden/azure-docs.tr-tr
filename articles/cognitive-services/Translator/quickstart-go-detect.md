---
title: 'Hızlı Başlangıç: Metinden dili tanımlama - Translator Metin Çevirisi, Go'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Go ile Translator Metin Çevirisi API’sini kullanarak kaynak metnin dilini tanımlayacaksınız.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/29/2018
ms.author: nolachar
ms.openlocfilehash: 25834fae2ce60d93ebeb3a25298b97294748bf9b
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129718"
---
# <a name="quickstart-identify-language-from-text-with-go"></a>Hızlı Başlangıç: Go ile metinden dil tanımlama

Bu hızlı başlangıçta, Translator Metin Çevirisi API'sini kullanarak kaynak metnin dilini tanımlayacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu çalıştırmak için [Go dağıtımını](https://golang.org/doc/install) yüklemeniz gerekir. Örnek kod yalnızca **çekirdek** kitaplıklarını kullandığından, dış bağımlılıkları yoktur.

Translator Metin Çevirisi API'sini kullanmak için, ayrıca abonelik anahtarınızın olması gerekir; bkz. [Translator Metin Çevirisi API'sine kaydolma](translator-text-how-to-signup.md).

## <a name="detect-request"></a>Algılama isteği

Aşağıdaki kod, [Algılama](./reference/v3-0-detect.md) yöntemini kullanarak kaynak metnin dilini tanımlar.

1. Sık kullandığınız kod düzenleyicisinde yeni bir Go projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `subscriptionKey` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Dosyayı '.go' uzantısıyla kaydedin.
5. Go yüklü bir bilgisayarda bir komut istemi açın.
6. Dosyayı oluşturun, örneğin: 'go build quickstart-detect.go'.
7. Dosyayı çalıştırın, örneğin: 'quickstart-detect'.

```golang
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
    "strings"
    "time"
)

func main() {
    // Replace the subscriptionKey string value with your valid subscription key
    const subscriptionKey = "<Subscription Key>"

    const uriBase = "https://api.cognitive.microsofttranslator.com"
    const uriPath = "/detect?api-version=3.0"

    const uri = uriBase + uriPath

    const text = "Salve, mondo!"

    r := strings.NewReader("[{\"Text\" : \"" + text + "\"}]")

    client := &http.Client{
        Timeout: time.Second * 2,
    }

    req, err := http.NewRequest("POST", uri, r)
    if err != nil {
        fmt.Printf("Error creating request: %v\n", err)
        return
    }

    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.FormatInt(req.ContentLength, 10))
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)

    resp, err := client.Do(req)
    if err != nil {
        fmt.Printf("Error on request: %v\n", err)
        return
    }
    defer resp.Body.Close()

    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        fmt.Printf("Error reading response body: %v\n", err)
        return
    }

    var f interface{}
    json.Unmarshal(body, &f)

    jsonFormatted, err := json.MarshalIndent(f, "", "  ")
    if err != nil {
        fmt.Printf("Error producing JSON: %v\n", err)
        return
    }
    fmt.Println(string(jsonFormatted))
}
```

## <a name="detect-response"></a>Algılama yanıtı

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
[
  {
    "alternatives": [
      {
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "pt",
        "score": 1
      },
      {
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "en",
        "score": 1
      }
    ],
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "language": "it",
    "score": 1
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Bilişsel Hizmetler API'leri için GitHub’da [Go için Azure SDK](https://github.com/Azure/azure-sdk-for-go)’dan Go paketlerini keşfedin.

> [!div class="nextstepaction"]
> [GitHub’da Go paketlerini keşfedin](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)
