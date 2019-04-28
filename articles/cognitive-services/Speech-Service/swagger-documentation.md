---
title: Swagger belgeleri - konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Swagger belgeler, çeşitli programlama dillerini için SDK'ları otomatik olarak oluşturmak için kullanılabilir. Tüm işlemler hizmetimizdeki Swagger tarafından desteklenmektedir.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 04/12/2019
ms.author: erhopf
ms.openlocfilehash: 29fcbd058651c428b488f5ce1c767105cb7921a6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61461672"
---
# <a name="swagger-documentation"></a>Swagger belgeleri

Birkaç veri alma, modeller oluşturun, model doğruluğunu test, özel uç noktalar oluşturmak, batch döküm ' sıraya ve abonelikleri yönetmek için kullanılan REST API ile etkileşim kurmak için bir Swagger belirtimi konuşma hizmetleri sunar. Özel konuşma tanıma Portalı aracılığıyla sağlanan işlemlerinin çoğu, bu API'leri kullanarak programlı olarak tamamlanabilir. 

> [!NOTE]
> Konuşmayı metne hem okuma işlemleri olarak Swagger belirtimi sırayla belgelenen REST API'leri desteklenir.

## <a name="generating-code-from-the-swagger-specification"></a>Swagger belirtiminden kod oluşturma

[Swager belirtimi](https://cris.ai/swagger/ui/index) , hızlı bir şekilde test etmek için çeşitli yollara izin verecek bir seçenek vardır. Ancak, bazı durumlarda, tüm yolları, gelecekteki çözümler dayandırabilir çağrıları tek bir kitaplık oluşturmak için kod oluşturmak için tercih edilir. Python kitaplığı oluşturmak için işlem bir göz atalım.

Konuşma hizmeti aboneliğinizle aynı bölgeye Swagger'ı ayarlamanız gerekir. Konuşma Hizmetleri kaynağınızı altında bölgeniz Azure portalında doğrulayabilirsiniz. Desteklenen bölgelerin tam listesi için bkz. [bölgeleri](regions.md).

1. Şuraya gidin: https://editor.swagger.io
2. Tıklayın **dosya**, ardından **içeri aktarma**
3. Konuşma Hizmetleri aboneliğinize için bölge dahil olmak üzere swagger URL'si girin `https://<your-region>.cris.ai/docs/v2.0/swagger`
4. Tıklayın **oluşturmak istemci** ve Python'ı seçin
5. İstemci Kitaplığı'nı Kaydet

Python kitaplığı ile oluşturulan kullanabileceğiniz [konuşma Hizmetleri samples github'da](https://aka.ms/csspeech/samples).

## <a name="reference-docs"></a>Başvuru belgeleri

* [REST (Swagger): Batch tanıma ve özelleştirme](https://westus.cris.ai/swagger/ui/index)
* [REST API: Konuşma metin](rest-speech-to-text.md)
* [REST API: Metin okuma](rest-text-to-speech.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma Hizmetleri samples github'da](https://aka.ms/csspeech/samples).
* [Bir konuşma Hizmetleri abonelik anahtarı ücretsiz olarak edinin](get-started.md)
