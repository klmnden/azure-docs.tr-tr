---
title: Bing Visual arama API için Python hızlı başlangıç | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Bing Visual arama API görüntüyü karşıya yükleme ve görüntü ile ilgili Öngörüler ulaşırsınız gösterilmektedir.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: a520466825eb429e45e0500b52bd7af502c0a38c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355073"
---
# <a name="your-first-bing-visual-search-query-in-python"></a>İlk Python Bing Visual arama sorgusu

Bing Visual arama API sağlayan bir görüntü ile ilgili bilgileri döndürür. Belirteç, ya da görüntüyü karşıya yükleyerek bir Öngörüler resmin URL'sini kullanarak görüntü sağlayabilir. Bu seçenekler hakkında daha fazla bilgi için bkz: [Bing Visual arama API nedir?](../overview.md) Bu makalede, görüntüyü karşıya gösterilmektedir. Görüntüyü karşıya burada iyi bilinen bir yer işareti resmini alın ve ilgili bilgileri dönmek mobil senaryolarda yararlı olabilir. Örneğin, Öngörüler yer işareti hakkında trivia içerebilir. 

Yerel görüntü yüklerseniz, aşağıdaki form verilerini POST gövdesinde içermelidir gösterir. Form verileri içerik düzeni üstbilgisini eklemeniz gerekir. Kendi `name` parametresini ayarlayın, görüntü"için" ve `filename` parametresi için herhangi bir dize ayarlanmış olabilir. Form içeriğini ikili görüntünün olur. Karşıya yükleme en büyük görüntü boyutu 1 MB'tır. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Bu makalede bir Bing Visual arama API isteği gönderir ve JSON arama sonuçlarını görüntüleyen basit bir konsol uygulaması içerir. Bu uygulama, Python içinde yazılmış olsa da, HTTP isteği yapmak ve JSON ayrıştırma programlama dili ile uyumlu bir RESTful Web hizmeti API'dir. 

## <a name="prerequisites"></a>Önkoşullar

Gereksinim duyduğunuz [Python 3](https://www.python.org/) bu kodu çalıştırmak için.

Bu Hızlı Başlangıç için kullandığınız bir [ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonelik anahtar veya Ücretli abonelik anahtarı.

## <a name="running-the-walkthrough"></a>İzlenecek yol çalıştırma

Bu uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

1. Sık kullanılan IDE veya Düzenleyicisi içinde yeni bir Python projesi oluşturun.
2. Visualsearch.PY adlı bir dosya oluşturun ve bu hızlı başlangıcı gösterilen kodu ekleyin.
3. Değiştir `SUBSCRIPTION_KEY` abonelik anahtarınızı değerle.
3. Değiştir `imagePath` karşıya yüklemek için resminin yolunu içeren değer.
4. Programını çalıştırın.



Çok bölümlü form verilerinin Python içinde kullanarak ileti göndermek nasıl gösterir.

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

[Öngörüler belirteci kullanarak bir görüntü ile ilgili Öngörüler alın](../use-insights-token.md)  
[Bing Visual arama tek sayfa uygulaması Öğreticisi](../tutorial-bing-visual-search-single-page-app.md)  
[Bing Visual arama genel bakış](../overview.md)  
[Deneyin](https://aka.ms/bingvisualsearchtryforfree)  
[Ücretsiz deneme erişim anahtarı alma](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Visual arama API Başvurusu](https://aka.ms/bingvisualsearchreferencedoc)
