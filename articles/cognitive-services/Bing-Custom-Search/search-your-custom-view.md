---
title: ': Bing özel özel bir görünüm araması | Microsoft Docs'
description: Bir özel görünümde Web ara açıklar
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 75f6c8d299c7eed901dda0631fca74b040f72e30
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352852"
---
# <a name="call-your-custom-search"></a>Özel aramanızı çağırın
Örneğiniz için arama sonuçları almak için ilk çağrıda özel arama API yapmadan önce Bilişsel hizmetler abonelik anahtarı edinmeniz gerekir. Özel arama API için bir anahtar almak için bkz: [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).

> [!NOTE]
> Veya bu tarihten önce 15 Ekim 2017 sağlanan bir önizleme anahtara sahip varolan Bing özel arama müşteriler kendi anahtarları 30 Kasım 2017 kadar veya sayısı izin verilen sorgular tüketmiş kadar kullanmanız mümkün olacaktır. Daha sonra Azure ile ilgili genel olarak kullanılabilir sürümüne geçirmek gerekir.

## <a name="try-it-out"></a>Deneyin
Özel arama deneyiminiz yapılandırdıktan sonra özel arama portal dahilinde yapılandırmasından test edebilirsiniz. Oturum [özel arama](https://customsearch.ai)özel arama örneği ve öğesini tıklatın **üretim** sekmesi. **Uç noktaları** sekmesi görüntülenir. Aboneliğinizi denemek için bkz: hangi uç noktaları kullanılabilir belirleyecek [sayfaları fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/). Bir uç nokta sınamak için açılan listeden seçin ve ilişkili yapılandırma seçeneklerini ayarlayın. 

Mevcut seçenekler şunlardır:

- **Sorgu**: aramak için arama terimi. Yalnızca Web, görüntü ve Autosuggest uç noktaları için kullanılabilir.
- **Özel yapılandırma kimliği**: seçilen özel arama örneğinin yapılandırması kimliği. Bu alan salt okunur.
- **Pazar**: sonuçları yeri Pazar. Yalnızca Web, görüntü ve barındırılan UI uç noktaları için kullanılabilir.
- **Abonelik anahtarı**: test etmek için abonelik anahtarı. Aşağı açılır listeden bir anahtar seçin veya el ile girebilirsiniz.
- **Güvenli arama**: Web sayfaları yetişkinlere yönelik içeriğe için filtre uygulamak için kullanılan bir filtre. Yalnızca Web, görüntü ve barındırılan UI uç noktaları için kullanılabilir.
- **Count**: yanıtta döndürmek için arama sonuçları sayısı. Yalnızca Web ve görüntü uç noktaları için kullanılabilir.
- **Uzaklık**: yanıtta döndürmek için arama sonuçları sayısı. Yalnızca Web ve görüntü uç noktaları için kullanılabilir.

Web, görüntü veya Autosuggest için gerekli tüm seçenekler belirttikten sonra tıklatın **çağrısı** JSON yanıt sağ bölmede görüntülemek için. 

UI barındırılan uç noktası seçerseniz, arama deneyimi sağ bölmesinde test edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [C# ile özel görünümünüzde çağırın](./call-endpoint-csharp.md)
- [Java ile özel görünümünüzde çağırın](./call-endpoint-java.md)
- [Özel görünümünüzde NodeJs ile çağırın](./call-endpoint-nodejs.md)
- [Python ile özel görünümünüzde çağırın](./call-endpoint-python.md)