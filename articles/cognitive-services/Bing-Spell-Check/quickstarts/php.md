---
title: "Hızlı Başlangıç: Bing yazım denetimi API'si, PHP"
titlesuffix: Azure Cognitive Services
description: Bing Yazım Denetimi API'sini kısa sürede kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri alın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 09/14/2017
ms.author: aahi
ms.openlocfilehash: abce882edb9b6c97ea3b8a91219f8e2887cfb78d
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55862318"
---
# <a name="quickstart-for-bing-spell-check-api-with-php"></a>Hızlı başlangıç: PHP ile Bing Yazım Denetimi API'si 

Bu makalede nasıl kullanılacağını gösterir [Bing yazım denetimi API'si](https://azure.microsoft.com/services/cognitive-services/spell-check/) PHP ile. Yazım Denetimi API'si tanınmayan sözcüklere ek olarak değişiklik önerileri döndürür. Genellikle bu API'ye metin gönderip önerilen değişiklikleri metne uygular veya uygulamanızın kullanıcısına göstererek değişikliklerin yapılıp yapılmayacağına karar vermelerini sağlayabilirsiniz. Bu makalede "Hollo, wrld!" metnini içeren bir istek gönderme adımları gösterilmiştir. Önerilen değişiklikler "Hello" ve "world" şeklindedir.

## <a name="prerequisites"></a>Önkoşullar

Bu kodu çalıştırmak için [PHP 5.6.x](http://php.net/downloads.php) sürümüne ihtiyacınız vardır.

**Bing Yazım Denetimi API'si v7** sürümüne sahip bir [Bilişsel Hizmetler API hesabınız](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/#lang) bu hızlı başlangıç için yeterlidir. Ücretsiz denemenizi etkinleştirdiğinizde verilen erişim anahtarınız olması veya Azure panonuzdan ücretli bir abonelik anahtarı kullanmanız gerekir.  Ayrıca bkz: [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="get-spell-check-results"></a>Yazım Denetimi sonuçlarını alma

1. Sık kullandığınız IDE ile yeni bir PHP projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `subscriptionKey` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Programı çalıştırın.

```php
<?php

// NOTE: Be sure to uncomment the following line in your php.ini file.
// ;extension=php_openssl.dll

// These properties are used for optional headers (see below).
// define("CLIENT_ID", "<Client ID from Previous Response Goes Here>");
// define("CLIENT_IP", "999.999.999.999");
// define("CLIENT_LOCATION", "+90.0000000000000;long: 00.0000000000000;re:100.000000000000");

$host = 'https://api.cognitive.microsoft.com';
$path = '/bing/v7.0/spellcheck?';
$params = 'mkt=en-us&mode=proof';

$input = "Hollo, wrld!";

$data = array (
    'text' => urlencode ($input)
);

// NOTE: Replace this example key with a valid subscription key.
$key = 'ENTER KEY HERE';

// The following headers are optional, but it is recommended
// that they are treated as required. These headers will assist the service
// with returning more accurate results.
//'X-Search-Location' => CLIENT_LOCATION
//'X-MSEdge-ClientID' => CLIENT_ID
//'X-MSEdge-ClientIP' => CLIENT_IP

$headers = "Content-type: application/x-www-form-urlencoded\r\n" .
    "Ocp-Apim-Subscription-Key: $key\r\n";

// NOTE: Use the key 'http' even if you are making an HTTPS request. See:
// http://php.net/manual/en/function.stream-context-create.php
$options = array (
    'http' => array (
        'header' => $headers,
        'method' => 'POST',
        'content' => http_build_query ($data)
    )
);
$context  = stream_context_create ($options);
$result = file_get_contents ($host . $path . $params, false, $context);

if ($result === FALSE) {
    /* Handle error */
}

$json = json_encode(json_decode($result), JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
echo $json;
?>
```

**Yanıt**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Yazım Denetimi öğreticisi](../tutorials/spellcheck.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Bing Yazım Denetimi'ne genel bakış](../proof-text.md)
- [Bing Yazım Denetimi API’si v7 Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
