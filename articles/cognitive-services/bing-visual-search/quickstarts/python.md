---
title: "Hızlı Başlangıç: Bing görsel arama REST API'si ve Python kullanarak görüntü Öngörüler elde edin"
titleSuffix: Azure Cognitive Services
description: Bing görsel arama API'sine bir görüntüyü karşıya yükleme ve ilgili Öngörüler alma hakkında bilgi edinin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 4/02/2019
ms.author: scottwhi
ms.openlocfilehash: 7ec37b4c3bdeb924b3e35dbcb5d07a478611f631
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60510866"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-python"></a>Hızlı Başlangıç: Bing görsel arama REST API'si ve Python kullanarak görüntü Öngörüler elde edin

Bu hızlı başlangıçta, ilk sonuçlarını görüntülemek ve Bing görsel arama API'sine çağrı yapmak için kullanın. Bu Python uygulaması, API için bir görüntüyü karşıya yükler ve döndürdüğü bilgileri görüntüler. Bu uygulama Python'da yazılmıştır ancak çoğu programlama dilleri ile uyumlu bir RESTful Web hizmeti API'dir.

Yerel bir görüntüyü karşıya yüklediğinizde, form verilerini içermelidir `Content-Disposition` başlığı. Ayarlamanız gerekir, `name` "image" ve parametre ayarlayabilirsiniz `filename` herhangi bir dize parametresi. Form içeriğini görüntünün ikili verileri içerir. Karşıya yüklediğiniz en yüksek görüntü boyutu 1 MB'dir.

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

1. Sık kullandığınız IDE veya düzenleyici yeni bir Python dosyası oluşturun ve aşağıdakini ekleyin `import` deyimi:

    ```python
    import requests, json
    ```

2. Abonelik anahtarınız, uç noktayı ve görüntünün karşıya yüklemekte yolu için değişkenler oluşturun:

    ```python

    BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'
    SUBSCRIPTION_KEY = 'your-subscription-key'
    imagePath = 'your-image-path'
    ```

3. İsteğiniz ait üst bilgi bilgileri tutmak için bir sözlük nesnesi oluşturun. Abonelik anahtarınızı dizeye bağlama `Ocp-Apim-Subscription-Key`, aşağıda gösterildiği gibi:

    ```python
    HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}
    ```

4. Başka bir sözlük açılır ve isteği gönderdiğinizde karşıya görüntünüzü içerecek şekilde oluşturun:

    ```python
    file = {'image' : ('myfile', open(imagePath, 'rb'))}
    ```

## <a name="parse-the-json-response"></a>JSON yanıtı ayrıştırılamadı

1. Adlı bir yöntem oluşturma `print_json()` API yanıtında alıp JSON'ı yazdırmak için:

    ```python
    def print_json(obj):
        """Print the object as json"""
        print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))
    ```

## <a name="send-the-request"></a>İsteği Gönder

1. Kullanım `requests.post()` Bing görsel arama API'sine bir istek gönderebilirsiniz. Dize, uç nokta, başlığı ve dosya bilgileri içerir. Yazdırma `response.json()` ile `print_json()`:

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
> [Görsel arama tek sayfa web uygulaması oluşturma](../tutorial-bing-visual-search-single-page-app.md)
