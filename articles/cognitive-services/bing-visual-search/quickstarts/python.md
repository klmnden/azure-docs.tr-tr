---
title: Bing görsel arama API'si için Python hızlı başlangıç | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Bing görsel arama API'sine bir görüntüyü karşıya yükleme ve görüntü ile ilgili Öngörüler geri alma işlemi gösterilmektedir.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 96bd94e37c75d10726245fbcea7044d4ae2ed07e
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39070384"
---
# <a name="your-first-bing-visual-search-query-in-python"></a>İlk Bing görsel arama sorgunuzda Python

Bing görsel arama API'sine sağlayan bir görüntü ile ilgili bilgi döndürür. Belirteç, ya da bir görüntü yükleyerek bir ınsights görüntünün URL'sini kullanarak görüntü sağlayabilir. Bu seçenekler hakkında daha fazla bilgi için bkz: [Bing görsel arama API'si nedir?](../overview.md) Bu makalede, bir resim karşıya gösterilmektedir. Görüntüyü karşıya yükleme, iyi bilinen bir yer işareti resmini Al ve geri hakkında bilgi alın mobil senaryolarda yararlı olabilir. Örneğin, öngörüleri, yer işareti hakkında bilgi dahil olabilir. 

Yerel bir görüntüyü karşıya yükleme, aşağıdaki form verilerini POST gövdesinde içermelidir gösterir. Form verileri içerik düzeni üstbilgisini içermelidir. Kendi `name` parametresi ayarlanması gerekir "Görüntü" ve `filename` parametreyi bir dizeye ayarlayın. Form içeriğini ikili görüntünün olur. Karşıya yükleyebilirsiniz en yüksek görüntü boyutu 1 MB'dir. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Bu makale, Bing görsel arama API'sine bir istek gönderir ve JSON arama sonuçlarını görüntüleyen basit bir konsol uygulaması içerir. Bu uygulama Python'da yazılmıştır, ancak HTTP istekleri ve JSON Ayrıştır programlama dili ile uyumlu bir RESTful Web hizmeti API'dir. 

## <a name="prerequisites"></a>Önkoşullar

Gereksinim duyduğunuz [Python 3](https://www.python.org/) bu kodu çalıştırmak için.

Bu hızlı başlangıçta, kullanabileceğiniz bir [ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonelik anahtarı veya Ücretli abonelik anahtarı.

## <a name="running-the-walkthrough"></a>İzlenecek yol çalıştırma

Bu uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

1. Yeni Python projesi, sık kullandığınız IDE veya düzenleyici oluşturun.
2. Visualsearch.PY adlı bir dosya oluşturun ve bu hızlı başlangıçta gösterilen kodu ekleyin.
3. Değiştirin `SUBSCRIPTION_KEY` abonelik anahtarınız ile değeri.
3. Değiştirin `imagePath` görüntünün karşıya yükleme yolunu içeren değer.
4. Programı çalıştırın.



Çok bölümlü form verilerinin Python kullanarak ileti gönderme işlemini gösterir.

```python
"""Bing Visual Search upload image example"""

# Download and install Python at https://www.python.org/
# Run the following in a command console window
# pip3 install requests

import requests, json


BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'

SUBSCRIPTION_KEY = '<yoursubscriptionkeygoeshere>'

HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}

imagePath = '<pathtoyourimagetouploadgoeshere>'

file = {'image' : ('myfile', open(imagePath, 'rb'))}

def main():
    
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())

    except Exception as ex:
        raise ex


def print_json(obj):
    """Print the object as json"""
    print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))



# Main execution
if __name__ == '__main__':
    main()
```


## <a name="next-steps"></a>Sonraki adımlar

[Insights belirteci kullanarak bir görüntü ile ilgili Öngörüler elde edin](../use-insights-token.md)  
[Bing görsel arama görüntüsünü karşıya yükleme Öğreticisi](../tutorial-visual-search-image-upload.md)
[Bing görsel arama tek sayfalı uygulama Öğreticisi](../tutorial-bing-visual-search-single-page-app.md)  
[Bing görsel arama genel bakış](../overview.md)  
[Deneyin](https://aka.ms/bingvisualsearchtryforfree)  
[Ücretsiz deneme erişim anahtarını alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing görsel arama API'si başvurusu](https://aka.ms/bingvisualsearchreferencedoc)
