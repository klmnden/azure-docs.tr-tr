---
title: 'Hızlı başlangıç: Görsel arama sorgusu oluşturma, Python - Bing Görsel Arama'
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'sine görüntü yüklemeyi ve görüntü hakkında içgörü almayı gösterir.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 3a0d92e42eed097e244118a60ec0a4223c9cedf5
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52440950"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-python"></a>Hızlı başlangıç: Python'da ilk Bing Görsel Arama sorgunuz

Bing Görsel Arama API'si, verdiğiniz bir görüntü hakkında bilgi döndürür. Bir URL veya bir içgörü belirteci kullanarak ya da karşıya resim yükleyerek görüntüyü verebilirsiniz. Bu seçenekler hakkında bilgi için bkz. [Bing Görsel Arama API’si nedir?](../overview.md) Bu makalede, karşıya görüntü yükleme gösterilmektedir. Karşıya resim yüklemek, mobil bir cihazla tanınmış bir yerin resmini çekip bu yer hakkında bilgi almak istediğiniz bir durumda kullanışlı olabilir. Örneğin, içgörüler bu yer hakkındaki önemsiz küçük ayrıntıları içerebilir. 

Aşağıda, yerel bir görüntüyü karşıya yükleyeceğiniz zaman POST’un gövdesine dahil etmeniz gereken form verileri gösterilmektedir. Form verileri, Content-Disposition üst bilgisini içermelidir. `name` parametresi, "image" olarak, `filename` parametresi ise herhangi bir dize olarak ayarlanmalıdır. Formun içerikleri, görüntünün ikili verisidir. Karşıya yükleyebileceğiniz maksimum görüntü boyutu 1 MB’tır. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Bu makale, bir Bing Görsel Arama API'si isteği gönderen ve JSON arama sonuçlarını görüntüleyen basit bir konsol uygulamasını içermektedir. Bu uygulama Python ile yazılmış olmakla birlikte API HTTP istekleri gönderebilen ve JSON ayrıştırabilen her programlama diliyle uyumlu bir RESTful Web hizmetidir. 

## <a name="prerequisites"></a>Önkoşullar

Bu kodu çalıştırmak için [Python 3](https://www.python.org/) sürümü gerekir.

Bu hızlı başlangıçta bir abonelik S9 fiyat katmanı gösterildiği gibi başlatmanız gerekecek [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/search-api/). 

Azure portalında bir abonelik başlatmak için:
1. 'BingSearchV7' ifadesini içeren Azure portalının üst metin kutusuna girin `Search resources, services, and docs`.  
2. Aşağı açılan listesinde Marketi bölümünde seçin `Bing Search v7`.
3. Girin `Name` yeni kaynak için.
4. Seçin `Pay-As-You-Go` abonelik.
5. Seçin `S9` fiyatlandırma katmanı.
6. Tıklayın `Enable` abonelik başlatmak için.

## <a name="running-the-walkthrough"></a>Adımları çalıştırma

Bu uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

1. Sık kullandığınız IDE'de veya düzenleyicide yeni bir Python projesi oluşturun.
2. visualsearch.py adlı bir dosya oluşturun ve bu hızlı başlangıçta gösterilen kodu ekleyin.
3. `SUBSCRIPTION_KEY` değerini abonelik anahtarınızla değiştirin.
3. `imagePath` değerini, karşıya yüklenecek görüntünün yoluyla değiştirin.
4. Programı çalıştırın.



Aşağıda metin Python'da çok bölümlü form verileri kullanarak ileti göndermeyi göstermektedir.

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

[İçgörü belirteci kullanarak bir görüntü ile ilgili içgörüler elde edin](../use-insights-token.md)  
[Bing Görsel Arama görüntü karşıya yükleme öğreticisi](../tutorial-visual-search-image-upload.md)
[Bing Görsel Arama tek sayfalı uygulama öğreticisi](../tutorial-bing-visual-search-single-page-app.md)  
[Bing Görsel Arama’ya genel bakış](../overview.md)  
[Deneyin](https://aka.ms/bingvisualsearchtryforfree)  
[Ücretsiz deneme erişim anahtarı alın](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Görsel Arama API’si başvurusu](https://aka.ms/bingvisualsearchreferencedoc)
