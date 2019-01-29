---
title: "Hızlı Başlangıç: Bing görsel arama REST API'si ve Python kullanarak görüntü Öngörüler elde edin"
titleSuffix: Azure Cognitive Services
description: Bing görsel arama API'sine bir görüntüyü karşıya yükleme ve ilgili Öngörüler alma hakkında bilgi edinin.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 37fef6100d78b46d0fb52e486f1788eb78ea2578
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55193769"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-python"></a>Hızlı Başlangıç: İlk Bing görsel arama sorgunuzda Python

Bu hızlı başlangıçta, ilk Bing görsel arama API'sine çağrı yapmak ve arama sonuçlarını görüntülemek için kullanın. Bu basit bir JavaScript uygulama API için bir görüntüyü karşıya yükler ve bu konuda döndürülen bilgileri görüntüler. Bu uygulamanın, JavaScript'te yazılmış olsa da çoğu programlama dilleri ile uyumlu bir RESTful Web hizmeti API'dir.

Yerel bir görüntüyü karşıya yüklenirken gönderme form verisi içerik düzeni üstbilgisini içermelidir. `name` parametresi, "image" olarak, `filename` parametresi ise herhangi bir dize olarak ayarlanmalıdır. Formun içerikleri, görüntünün ikili verisidir. Karşıya yükleyebileceğiniz maksimum görüntü boyutu 1 MB’tır.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Önkoşullar

* [Python 3.x](https://www.python.org/)


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Uygulamayı Başlat

1. Sık kullandığınız IDE veya düzenleyici yeni bir Python dosyası oluşturun ve aşağıdaki içeri aktarma deyimini ekleyin.

    ```python
    import requests, json
    ```

2. Abonelik anahtarınız, uç noktayı ve görüntünün karşıya yüklemekte yolu için değişkenler oluşturun.

    ```python

    BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'
    SUBSCRIPTION_KEY = 'your-subscription-key'
    imagePath = 'your-image-path'
    ```

3. İsteklerinizi üst bilgi bilgileri tutmak için bir sözlük nesnesi oluşturun. Abonelik anahtarınızı dizeye bağlama `Ocp-Apim-Subscription-Key`, aşağıda gösterildiği gibi.

    ```python
    HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}
    ```

4. Başka bir sözlük açılır ve isteği gönderdiğinizde karşıya görüntünüzü içerecek şekilde oluşturun. 

    ```python
    file = {'image' : ('myfile', open(imagePath, 'rb'))}
    ```

## <a name="parse-the-json-response"></a>JSON yanıtı ayrıştırılamadı

1. Adlı bir yöntem oluşturma `print_json()` API yanıtında alıp JSON'ı yazdırmak için.

    ```python
    def print_json(obj):
        """Print the object as json"""
        print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))
    ```

## <a name="send-the-request"></a>İsteği Gönder

1. Kullanım `requests.post()` Bing görsel arama API'sine bir istek gönderebilirsiniz. Dize, uç nokta, başlığı ve dosya bilgileri içerir. Yazdırma `response.json()` ile `print_json()`

    ```python
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())
    
    except Exception as ex:
        raise ex
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir özel arama web uygulaması derleme](../tutorial-bing-visual-search-single-page-app.md)
