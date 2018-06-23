---
title: Çağrı ve yanıt - Azure Bilişsel hizmetler, Bing Web arama API için PHP hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get Bing Web arama API Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki kullanmaya başlayın.
services: cognitive-services
documentationcenter: ''
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 9/18/2017
ms.author: v-jerkin
ms.openlocfilehash: 2e54db4ba59d89271077d243589243768bf8b7fc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352384"
---
# <a name="call-and-response-your-first-bing-web-search-query-in-php"></a>Çağrı ve yanıt: PHP, ilk Bing Web arama sorgusu

Bing Web arama API Bing belirler arama sonuçları kullanıcının sorgu ile ilgili döndürerek Bing.com/Search için benzer bir deneyim sağlar. Sonuçları, Web sayfaları, resim, video, haber ve varlıklar, ilgili arama sorguları, yazım düzeltmeleri, saat dilimleri, birim dönüştürme, çeviri ve hesaplamalar birlikte içerebilir. Tür sonuç elde ilgileri ve abone olduğunuz Bing arama API'leri katmanını temel alır.

Bu makalede bir Bing Web arama API sorgu gerçekleştirir ve JSON biçiminde döndürülen ham arama sonuçlarını görüntüleyen basit bir konsol uygulaması içerir. Bu uygulama, PHP ile yazılmış olsa da, HTTP isteği yapmak ve JSON ayrıştırma programlama dili ile uyumlu bir RESTful Web hizmeti API'dir. 

## <a name="prerequisites"></a>Önkoşullar

Gereksinim duyduğunuz [PHP 5.6.x](http://php.net/downloads.php) bu kodu çalıştırmak için.

Bilmeniz gereken bir [Bilişsel Hizmetleri API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Bing arama API'leri**. [Ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) Bu Hızlı Başlangıç için yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirmek ya da Ücretli abonelik anahtarı Azure panonuza kullanabilir sağlanan erişim anahtarı gerekir.

## <a name="running-the-application"></a>Uygulamayı çalıştırma

Bu uygulamayı çalıştırmak için aşağıdaki adımları izleyin.

1. Güvenli HTTP desteği etkinleştirildiğinden emin olun, `php.ini` kod açıklamada açıklandığı gibi. Bu dosyanın bulunduğu Windows üzerinde `C:\windows`.
2. Sık kullanılan IDE veya Düzenleyicisi içinde yeni bir PHP projesi oluşturun.
3. Sağlanan kod ekleyin.
4. Değiştir `accessKey` aboneliğiniz için geçerli bir erişim anahtarı ile değer.
5. Programını çalıştırın.

```php
<?php

// NOTE: Be sure to uncomment the following line in your php.ini file.
// ;extension=php_openssl.dll

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the accessKey string value with your valid access key.
$accessKey = 'enter key here';

// Verify the endpoint URI.  At this writing, only one endpoint is used for Bing
// search APIs.  In the future, regional endpoints may be available.  If you
// encounter unexpected authorization errors, double-check this value against
// the endpoint for your Bing Web search instance in your Azure dashboard.
$endpoint = 'https://api.cognitive.microsoft.com/bing/v7.0/search';

$term = 'Microsoft Cognitive Services';

function BingWebSearch ($url, $key, $query) {
    // Prepare HTTP request
    // NOTE: Use the key 'http' even if you are making an HTTPS request. See:
    // http://php.net/manual/en/function.stream-context-create.php
    $headers = "Ocp-Apim-Subscription-Key: $key\r\n";
    $options = array ('http' => array (
                          'header' => $headers,
                           'method' => 'GET'));

    // Perform the Web request and get the JSON response
    $context = stream_context_create($options);
    $result = file_get_contents($url . "?q=" . urlencode($query), false, $context);

    // Extract Bing HTTP headers
    $headers = array();
    foreach ($http_response_header as $k => $v) {
        $h = explode(":", $v, 2);
        if (isset($h[1]))
            if (preg_match("/^BingAPIs-/", $h[0]) || preg_match("/^X-MSEdge-/", $h[0]))
                $headers[trim($h[0])] = trim($h[1]);
    }

    return array($headers, $result);
}

if (strlen($accessKey) == 32) {

    print "Searching the Web for: " . $term . "\n";
    
    list($headers, $json) = BingWebSearch($endpoint, $accessKey, $term);
    
    print "\nRelevant Headers:\n\n";
    foreach ($headers as $k => $v) {
        print $k . ": " . $v . "\n";
    }
    
    print "\nJSON Response:\n\n";
    echo json_encode(json_decode($json), JSON_PRETTY_PRINT);

} else {

    print("Invalid Bing Search API subscription key!\n");
    print("Please paste yours into the source code.\n");

}
?>
```

## <a name="json-response"></a>JSON yanıtı

Örnek yanıt izler. JSON uzunluğunu sınırlamak için yalnızca tek bir sonuç gösterilir ve diğer bölümleri yanıtının kesildi. 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "Microsoft Cognitive Services"
  },
  "webPages": {
    "webSearchUrl": "https://www.bing.com/search?q=Microsoft+cognitive+services",
    "totalEstimatedMatches": 22300000,
    "value": [
      {
        "id": "https://api.cognitive.microsoft.com/api/v7/#WebPages.0",
        "name": "Microsoft Cognitive Services",
        "url": "https://www.microsoft.com/cognitive-services",
        "displayUrl": "https://www.microsoft.com/cognitive-services",
        "snippet": "Knock down barriers between you and your ideas. Enable natural and contextual interaction with tools that augment users' experiences via the power of machine-based AI. Plug them in and bring your ideas to life.",
        "deepLinks": [
          {
            "name": "Face API",
            "url": "https://azure.microsoft.com/services/cognitive-services/face/",
            "snippet": "Add facial recognition to your applications to detect, identify, and verify faces using a Face API from Microsoft Azure. ... Cognitive Services; Face API;"
          },
          {
            "name": "Text Analytics",
            "url": "https://azure.microsoft.com/services/cognitive-services/text-analytics/",
            "snippet": "Cognitive Services; Text Analytics API; Text Analytics API . Detect sentiment, ... you agree that Microsoft may store it and use it to improve Microsoft services, ..."
          },
          {
            "name": "Computer Vision API",
            "url": "https://azure.microsoft.com/services/cognitive-services/computer-vision/",
            "snippet": "Extract the data you need from images using optical character recognition and image analytics with Computer Vision APIs from Microsoft Azure."
          },
          {
            "name": "Emotion",
            "url": "https://www.microsoft.com/cognitive-services/en-us/emotion-api",
            "snippet": "Cognitive Services Emotion API - microsoft.com"
          },
          {
            "name": "Bing Speech API",
            "url": "https://azure.microsoft.com/services/cognitive-services/speech/",
            "snippet": "Add speech recognition to your applications, including text to speech, with a speech API from Microsoft Azure. ... Cognitive Services; Bing Speech API;"
          },
          {
            "name": "Get Started for Free",
            "url": "https://azure.microsoft.com/services/cognitive-services/",
            "snippet": "Add vision, speech, language, and knowledge capabilities to your applications using intelligence APIs and SDKs from Cognitive Services."
          }
        ]
      }
    ]
  },
  "relatedSearches": {
    "id": "https://api.cognitive.microsoft.com/api/v7/#RelatedSearches",
    "value": [
      {
        "text": "microsoft bot framework",
        "displayText": "microsoft bot framework",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+bot+framework"
      },
      {
        "text": "microsoft cognitive services youtube",
        "displayText": "microsoft cognitive services youtube",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+youtube"
      },
      {
        "text": "microsoft cognitive services search api",
        "displayText": "microsoft cognitive services search api",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+search+api"
      },
      {
        "text": "microsoft cognitive services news",
        "displayText": "microsoft cognitive services news",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+news"
      },
      {
        "text": "ms cognitive service",
        "displayText": "ms cognitive service",
        "webSearchUrl": "https://www.bing.com/search?q=ms+cognitive+service"
      },
      {
        "text": "microsoft cognitive services text analytics",
        "displayText": "microsoft cognitive services text analytics",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+text+analytics"
      },
      {
        "text": "microsoft cognitive services toolkit",
        "displayText": "microsoft cognitive services toolkit",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+toolkit"
      },
      {
        "text": "microsoft cognitive services api",
        "displayText": "microsoft cognitive services api",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+api"
      }
    ]
  },
  "rankingResponse": {
    "mainline": {
      "items": [
        {
          "answerType": "WebPages",
          "resultIndex": 0,
          "value": {
            "id": "https://api.cognitive.microsoft.com/api/v7/#WebPages.0"
          }
        }
      ]
    },
    "sidebar": {
      "items": [
        {
          "answerType": "RelatedSearches",
          "value": {
            "id": "https://api.cognitive.microsoft.com/api/v7/#RelatedSearches"
          }
        }
      ]
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Web arama tek sayfa uygulaması Öğreticisi](../tutorial-bing-web-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Bing Web araması genel bakış](../overview.md)  
[Deneyin](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/)  
[Ücretsiz deneme erişim anahtarı alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api)  
[Bing Web arama API Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
