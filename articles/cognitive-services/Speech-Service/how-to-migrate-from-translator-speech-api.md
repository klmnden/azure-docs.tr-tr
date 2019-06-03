---
title: Translator konuşma API'sini konuşma Service'a dönüştürme
titleSuffix: Azure Cognitive Services
description: Uygulamalarınızı konuşma Hizmetleri için Translator konuşma tanıma API'SİNDEN geçirmeyi öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: 1ed494cea1ccf8845a25a3ab49d3194cc6a55509
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "65785672"
---
# <a name="migrate-from-the-translator-speech-api-to-the-speech-service"></a>Translator konuşma API'sini konuşma Service'a dönüştürme

Microsoft Translator konuşma tanıma API'si için uygulamalarınızı geçirmek için bu makaleyi kullanın [konuşma hizmeti](index.yml). Bu kılavuz, Translator konuşma tanıma API'si ve konuşma hizmeti arasındaki farkları özetler ve uygulamalarınızı geçiş stratejileri önerir.

> [!NOTE]
> Translator konuşma tanıma API'si abonelik anahtarınızı konuşma hizmeti tarafından kabul edilmez. Yeni bir konuşma Hizmetleri aboneliği oluşturmanız gerekir.

## <a name="comparison-of-features"></a>Özelliklerin karşılaştırması

| Özellik                                           | Translator Konuşma Çevirisi API’si                                  | Konuşma Hizmetleri | Ayrıntılar                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Metin çevirisi                               | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Konuşma çevirisi teknolojisini                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Genel uç noktası                                   | :heavy_check_mark:                                              | : heavy_minus_sign:                 | Konuşma hizmetleri genel bir uç noktası sunmamaktadır. Genel bir uç noktası otomatik olarak trafiği uygulamanızdaki gecikme süresini düşüren, bölgesel uç noktasına yakın yönlendirebilir.                                                    |
| Bölgesel uç noktaları                                | : heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Bağlantı zaman sınırı                             | 90 dakika                                               | SDK'sı ile sınırsız. 10 dakika WebSockets bağlantısı.                                                                                                                                                                                                                                                                                   |
| Üst bilgisindeki kimlik doğrulama anahtarı                                | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Tek bir istekte birden çok dil çevirisi | : heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Kullanılabilir SDK'lar                                    | : heavy_minus_sign:                                              | :heavy_check_mark:                 | Bkz: [konuşma Hizmetleri belgeleri](index.yml) kullanılabilir SDK'ları için.                                                                                                                                                    |
| WebSockets bağlantıları                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Dil API'si                                     | :heavy_check_mark:                                              | : heavy_minus_sign:                 | Konuşma Hizmetleri aynı açıklanan dilleri destekler [Translator API dil başvurusu](../translator-speech/languages-reference.md) makalesi. |
| Küfür filtresini ve işaret                       | : heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| . WAV/PCM giriş                                 | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Giriş olarak diğer dosya türleri                         | : heavy_minus_sign:                                              | : heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Kısmi sonuçlar                                   | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Zamanlama bilgisi                                       | :heavy_check_mark:                                              | : heavy_minus_sign:                 |                                                                                                                                                                 |
| Bağıntı Kimliği                                    | :heavy_check_mark:                                              | : heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Özel konuşma modelleri                              | : heavy_minus_sign:                                              | :heavy_check_mark:                 | Konuşma Hizmetleri, konuşma tanıma, kuruluşunuzun benzersiz kelime için özelleştirmenize olanak sağlayan özel konuşma modelleri sunar.                                                                                                                                           |
| Özel çeviri modellerini                         | : heavy_minus_sign:                                              | :heavy_check_mark:                 | Microsoft metin çevirisi API'sine abone kullanmanıza olanak sağlar [özel Translator](https://www.microsoft.com/translator/business/customization/) için daha doğru çevirileri kendi verilerinizi kullanmak için.                                                 |

## <a name="migration-strategies"></a>Geçiş stratejileri

Translator konuşma tanıma API'si kullanan uygulamaları geliştirme veya üretim, sizin veya kuruluşunuzun varsa, bunları konuşma hizmeti kullanacak şekilde güncelleştirmeniz gerekir. Bkz: [konuşma hizmeti](index.yml) belgeleri kullanılabilir SDK'lar, kod örnekleri ve öğreticiler. Geçirmekte olduğunuz aşağıdakileri dikkate alın:

* Konuşma hizmetleri genel bir uç noktası sunmamaktadır. Uygulamanızı verimli bir şekilde, bölgesel tek bir uç nokta tüm trafik için kullandığında işlevleri, belirleyin. Aksi durumda, coğrafi konum en verimli uç nokta belirlemek için kullanın.

* Uygulamanızı uzun süreli bağlantılar kullanır ve kullanılabilir SDK'lar kullanamazsınız, WebSockets bağlantı kullanabilirsiniz. 10 dakikalık zaman aşımı sınırı, uygun zamanlarda yeniden bağlanmayı tarafından yönetin.

* Translator konuşma tanıma API'si ve Translator Text API bir özel çeviri modellerini etkinleştirmek için uygulamanızın kullanıyorsa, konuşma hizmeti kullanarak doğrudan kategori kimlikleri ekleyebilirsiniz.

* Translator konuşma tanıma API'si, konuşma Hizmetleri tek bir istekte birden fazla dilde içine çevirileri tamamlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma Hizmetleri ücretsiz olarak deneyin](get-started.md)
* [Hızlı Başlangıç: Bir UWP uygulamasında Speech SDK'sı kullanarak konuşma tanıma](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Konuşma hizmeti nedir](overview.md)
* [Konuşma Hizmetleri ve Speech SDK'sı belgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg)
