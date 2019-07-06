---
title: "Hızlı Başlangıç: Metin içeriği python'da - Content Moderator analiz edin"
titlesuffix: Azure Cognitive Services
description: Metin içeriği için içerik Moderator SDK'sını kullanarak Python için çeşitli içeriklere analiz etme
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: 0fef3bffd30c19d0313e5fce7eb610ae7f6349f5
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607003"
---
# <a name="quickstart-analyze-text-content-for-objectionable-material-in-python"></a>Hızlı Başlangıç: Metin içeriği içeriklere python'da için analiz edin

Bu makalede bilgiler sağlar ve yardımcı olması için kod örnekleri Content Moderator SDK'sı için Python'ı kullanmaya başlayın. Uygunsuz olabilecek malzemeleri yönetmek için terim tabanlı filtreleme ve metin içeriğini sınıflandırma işlemlerini yürütmeyi öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar
- Content Moderator abonelik anahtarı. Content Moderator'a abone olmak ve anahtarınızı almak için [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yönergelerini izleyin.
- [Python 2.7+ veya 3.5+](https://www.python.org/downloads/)
- [pip](https://pip.pypa.io/en/stable/installing/) aracı

## <a name="get-the-content-moderator-sdk"></a>Content Moderator SDK'sı Al

Content Moderator Python SDK'sı, bir komut istemini açın ve aşağıdaki komutu çalıştırarak yükleyin:

```bash
pip install azure-cognitiveservices-vision-contentmoderator
```

## <a name="import-modules"></a>Modülleri içeri aktarma

Adlı yeni bir Python betiği oluşturmak _ContentModeratorQS.py_ ve SDK'yı gerekli bölümlerini içeri aktarmak için aşağıdaki kodu ekleyin.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=1-10)]

Ayrıca son çıktı işlemek için "düzgün yazdırma" işlev içeri aktarın.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=12)]


## <a name="initialize-variables"></a>Değişkenleri başlatma

Ardından, değişkenleri, Content Moderator abonelik anahtarını ve uç nokta URL'sini ekleyin. Değiştirmeniz gerekecektir `<your subscription key>` anahtarınızı değerine sahip. Değerini değiştirmeniz gerekebilir `endpoint_url` kullanmak için abonelik anahtarınızı karşılık gelen bir bölge tanımlayıcısı. Ücretsiz deneme aboneliği anahtarları oluşturulur **westus** bölge.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=14-16)]


Basitleştirmek amacıyla, doğrudan komut dosyasından metin analiz eder. Metin içeriğini denetlemek için yeni bir dize tanımlayın:

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=18-21)]


## <a name="query-the-moderator-service"></a>Denetleyici hizmetini sorgulama

Oluşturma bir **ContentModeratorClient** abonelik anahtarını ve uç nokta URL'nizi kullanarak örneği. Daha sonra kendi üyesi kullanın **TextModerationOperations** denetimi API'si çağırmak için örneği. Bkz: **[screen_text](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.operations.textmoderationoperations?view=azure-python)** başvuru belgeleri çağırmak nasıl daha fazla bilgi için.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=23-36)]

## <a name="print-the-response"></a>Yanıtı yazdırma

Son olarak, çağrısı başarıyla tamamlandı ve döndürülen denetleyin bir **ekran** örneği. Daha sonra konsolda döndürülen veriler yazdırın.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=38-39)]


Bu hızlı başlangıçta sonuçları aşağıdaki çıktıda kullanılan örnek metni:

```console
{'auto_corrected_text': '" Is this a garbage email abide@ abed. com, phone: '
                        '6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, '
                        'Redmond, WA 98052. Crap is the profanity here. Is '
                        'this information PII? phone 3144444444\\ n"',
 'classification': {'category1': {'score': 0.00025233393535017967},
                    'category2': {'score': 0.18468093872070312},
                    'category3': {'score': 0.9879999756813049},
                    'review_recommended': True},
 'language': 'eng',
 'normalized_text': '" Is this a garbage email abide@ abed. com, phone: '
                    '6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, '
                    'Redmond, WA 98052. Crap is the profanity here. Is this '
                    'information PII? phone 3144444444\\ n"',
 'original_text': '"Is this a grabage email abcdef@abcd.com, phone: '
                  '6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, '
                  'WA 98052. Crap is the profanity here. Is this information '
                  'PII? phone 3144444444\\n"',
 'pii': {'address': [{'index': 82,
                      'text': '1 Microsoft Way, Redmond, WA 98052'}],
         'email': [{'detected': 'abcdef@abcd.com',
                    'index': 25,
                    'sub_type': 'Regular',
                    'text': 'abcdef@abcd.com'}],
         'ipa': [{'index': 65, 'sub_type': 'IPV4', 'text': '255.255.255.255'}],
         'phone': [{'country_code': 'US', 'index': 49, 'text': '6657789887'},
                   {'country_code': 'US', 'index': 177, 'text': '3144444444'}]},
 'status': {'code': 3000, 'description': 'OK'},
 'terms': [{'index': 118, 'list_id': 0, 'original_index': 118, 'term': 'crap'}],
 'tracking_id': 'b253515c-e713-4316-a016-8397662a3f1a'}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, verilen metni örnek ilgili bilgileri döndürmek için Content Moderator hizmet kullanan basit bir Python betiği geliştirdik. Bundan sonra, hangi verilere ihtiyacınız olduğuna ve uygulamanızın bunları nasıl işleyeceğine karar verebilmek için farklı bayrakların ve sınıflandırmaların anlamları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Metin moderasyonu kılavuzu](text-moderation-api.md)
