---
title: "Hızlı Başlangıç: Bing haber arama REST API'si ve Git kullanarak haberleri alın"
titleSuffix: Azure Cognitive Services
description: Bing haber arama API'si haber sonuçları almayı öğrenin.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 2/21/2019
ms.author: rosh
ms.openlocfilehash: 295c32c1e14dc6a69a37040f92d27a6862359228
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60861361"
---
# <a name="quickstart-get-news-results-using-the-bing-news-search-rest-api-and-go"></a>Hızlı Başlangıç: Bing haber arama REST API'si ve Git kullanarak haber sonuçlar elde edin

Bu hızlı başlangıçta, Bing haber arama API'sini çağırmak için Go dilini kullanır. Sonuçlar adları ve URL sorgu dizesi tarafından tanımlanan haber kaynakları içerir.

## <a name="prerequisites"></a>Önkoşullar
* Yükleme [Git ikili dosyaları](https://golang.org/dl/)
* Git-çıktısının kitaplığı için sonuçları görüntülemek için asıl yazıcı yüklemeniz
    * Bu kitaplığı yükleyin: `$ go get -u https://github.com/davecgh/go-spew`

[!INCLUDE [bing-web-search-quickstart-signup](../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="create-a-project-and-import-libraries"></a>Proje oluşturma ve kitaplıkları içeri aktarma

Yeni bir Git proje, IDE'niz veya düzenleyicinizle oluşturun. Ardından içeri `net/http` istekleri `ioutil` yanıt okumak için ve `encoding/json` JSON metnini sonuçları işlemek için. Git-çıktısının kitaplığı, JSON ayrıştırmak için gereklidir. 

```
package main

import (
    "fmt"
    "net/http"
    "io/ioutil"
    "encoding/json"
    "github.com/davecgh/go-spew/spew"
)

```

## <a name="create-a-struct-to-format-the-news-search-results"></a>Haber araması sonuçları biçimlendirmek için bir yapı oluşturma

`NewsAnswer` yapısı, yanıtta sağlanan verileri biçimlendirir. Yanıt üst üste ve oldukça karmaşık JSON.  Aşağıdaki uygulama temelleri ele alınmaktadır.

```
// This struct formats the answer provided by the Bing News Search API.
type NewsAnswer struct {
    ReadLink       string `json: "readLink"` 
    QueryContext   struct {
        OriginalQuery   string   `json: "originalQuery"`
        AdultIntent     bool        `json: "adultIntent"`
    } `json: "queryContext"`
    TotalEstimatedMatches   int  `json: totalEstimatedMatches"` 
    Sort  []struct {
        Name    string  `json: "name"`
        ID       string    `json: "id"`
        IsSelected       bool  `json: "isSelected"`
        URL      string   `json: "url"`
    }  `json: "sort"` 
    Value   []struct   {
        Name     string   `json: "name"`
        URL   string    `json: "url"`
        Image   struct   {
            Thumbnail   struct  {
                ContentUrl  string  `json: "thumbnail"`
                Width   int  `json: "width"`
                Height  int   `json: "height"`
            } `json: "thumbnail"` 
            Description  string  `json: "description"`
            Provider  []struct   {
                Type   string    `json: "_type"`
                Name  string     `json: "name"`
            } `json: "provider"` 
            DatePublished   string   `json: "datePublished"`
        } `json: "image"` 
    } `json: "value"` 
}

```

## <a name="declare-the-main-function-and-define-variables"></a>Ana işlevi ve değişkenleri tanımlama  

Aşağıdaki kod ana işlevini bildirir ve gerekli değişkenleri atar. Uç noktasının geçerli olduğunu doğrulayın ve `token` değerini Azure hesabınızdan geçerli bir abonelik anahtarı ile değiştirin.

```
func main() {
    // Verify the endpoint URI and replace the token string with a valid subscription key.  
    const endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
    token := "YOUR-ACCESS-KEY"
    searchTerm := "Microsoft Cognitive Services"

    // Declare a new GET request.
    req, err := http.NewRequest("GET", endpoint, nil)
    if err != nil {
        panic(err)
    }

    // The rest of the code in this example goes here in the main function.
}
```

## <a name="query-and-header"></a>Sorgu ve üst bilgisi

Sorgu dizesi ve erişim anahtarı üstbilgisi Ekle

```
// Add the query to the request.  
param := req.URL.Query()
param.Add("q", searchTerm)
req.URL.RawQuery = param.Encode()

// Insert the subscription-key header.  
req.Header.Add("Ocp-Apim-Subscription-Key", token)

```

## <a name="get-request"></a>Alma isteği

İstemci oluşturmak ve Get isteği gönderin. 

```
// Instantiate a client.  
client := new(http.Client)

// Send the request to Bing.  
resp, err := client.Do(req)
if err != nil {
    panic(err)
}

```

## <a name="send-the-request"></a>İsteği Gönder

İstek göndermek ve sonuçları kullanarak okuma `ioutil`.

```
resp, err := client.Do(req)
    if err != nil {
        panic(err)
}

// Close the connection.    
defer resp.Body.Close()

// Read the results
resbody, err := ioutil.ReadAll(resp.Body)
if err != nil {
    panic(err)
}

```

## <a name="handle-the-response"></a>Yanıtı işleme

`Unmarshall` İşlevi, haber arama API'si tarafından döndürülen JSON metninde bilgileri ayıklar.  Düğümleri kullanarak sonuçları görüntüleyebilirsiniz sonra `go-spew` güzel yazıcı.

```
// Create a new answer object 
ans := new(NewsAnswer)
err = json.Unmarshal(body, &ans)
if err != nil {
    fmt.Println(err)
}
    
fmt.Print("Output of BingAnswer: \r\n\r\n")
    
// Iterate over search results and print the result name and URL.
for _, result := range ans.Value{
spew.Dump(result.Name, result.URL)
}

```

## <a name="results"></a>Sonuçlar

Sonuçları adını ve her sonuç URL'sini içerir.

```
(string) (len=91) "Cognitive Services Market: Global Industry Analysis and Opportunity Assessment, 2019 - 2025"
(string) (len=142) "https://www.marketwatch.com/press-release/cognitive-services-market-global-industry-analysis-and-opportunity-assessment-2019---2025-2019-02-21"
(string) (len=104) "Microsoft calls for greater collaboration to harness the power of AI to empower people with disabilities"
(string) (len=115) "https://indiaeducationdiary.in/microsoft-calls-greater-collaboration-harness-power-ai-empower-people-disabilities-2/"
(string) (len=70) "Microsoft 'Intelligent Cloud Hub' to build AI-ready workforce in India"
(string) (len=139) "https://cio.economictimes.indiatimes.com/news/cloud-computing/microsoft-intelligent-cloud-hub-to-build-ai-ready-workforce-in-india/67187807"
(string) (len=81) "Skills shortage is stopping many Asian companies from embracing A.I., study shows"
(string) (len=106) "https://www.cnbc.com/2019/02/20/microsoft-idc-study-skills-shortages-stopping-companies-from-using-ai.html"
(string) (len=143) "Cognitive Computing in Healthcare Market Emerging Top Key Vendors- Apixio, MedWhat, Healthcare X.0, Apple, Google, Microsoft, and IBM 2017-2025"
(string) (len=40) "http://www.digitaljournal.com/pr/4163064"
(string) (len=49) "Microsoft launches AI skills initiative in Brazil"
(string) (len=80) "https://www.zdnet.com/article/microsoft-launches-ai-skills-initiative-in-brazil/"
(string) (len=45) "Kuwait's CITRA and Microsoft host AI OpenHack"
(string) (len=70) "http://www.itp.net/618639-kuwaits-citra-and-microsoft-host-ai-openhack"
(string) (len=51) "CITRA and Microsoft collaborate to host AI workshop"
(string) (len=123) "https://www.zawya.com/mena/en/press-releases/story/CITRA_and_Microsoft_collaborate_to_host_AI_workshop-ZAWYA20190212105751/"

```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing haber arama nedir](search-the-web.md)
