---
title: Özel karar hizmeti nedir? -Azure Bilişsel hizmetler | Microsoft Docs
description: Bu makale genel bakışlar Azure özel karar hizmeti, bulut tabanlı bir API deneyimiyle keskinleştirir bağlamsal karar vermek için.
services: cognitive-services
author: alekh
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/08/2018
ms.author: slivkins;marcozo;alekh;marossi
ms.openlocfilehash: 774467446513dcd7ade7255d998b11f41824cafe
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356298"
---
# <a name="what-is-custom-decision-service"></a>Özel karar hizmeti nedir?

Tipik web veya mobil uygulama, birkaç makaleler veya diğer içerik türlerine ön sayfa bağlanır. Ön sayfa yükler gibi bu ön sayfasında içerilip derece makaleleri özel karar hizmete isteyebilir. Bu nedenle, bir kullanıcı bir makale tıklayarak seçtiğinde, ikinci bir istek özel karar hangi kullanıcı kararı sonucunu oturum hizmetine gönderilebilir.

İçeriğinizi ve birkaç satırlık uygulamanıza eklenecek JavaScript için yalnızca bir RSS akışı gerektirdiği şekilde özel karar kullanmayı, hizmetidir.

Özel karar hizmet özellikleri machine learning için içeriğinizi dönüştürür. Sistem, içeriğinizi metin, resim, video ve genel düşünceleri bakımından anlamak için bu özellikleri kullanır. Birkaç diğer kullanan [Microsoft Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services)gibi [varlık bağlama](../entitylinking/home.md), [metin analizi](../text-analytics/overview.md), [duygu](../emotion/home.md)ve [Bilgisayar görme](../computer-vision/home.md).

Özel karar hizmeti için bazı ortak kullanım durumları şunlardır:

* Kişiselleştirme makaleleri bir haber Web sitesinde
* Video içeriği medya Portal kişiselleştirme
* Ad yerleşimi veya için ad yönlendirir web sayfalarını en iyi duruma getirme
* Derecelendirme öğeleri bir alışveriş Web sitesinde önerilir.

Özel karar hizmet halen *ücretsiz genel Önizleme*. Bir Web sitesi veya uygulama makalelerin bir listesi kişiselleştirebilirsiniz. Özellik ayıklama İngilizce dil içerik en iyi şekilde çalışır. [Sınırlı işlevsellik](../text-analytics/overview.md) İspanyolca, Fransızca, Almanca, Portekizce ve Japonca gibi diğer diller için sunulur. Yeni işlevselliği kullanılabilir olduğunda bu belgeleri düzenlendi.

Özel karar hizmet içerik kişiselleştirme etki alanında olmayan uygulamalarda kullanılabilir. Bu uygulamalar özel Önizleme için iyi bir tercihtir olabilir. [Bize Ulaşın](https://azure.microsoft.com/overview/sales-number/) daha fazla bilgi için.

## <a name="api-usage-modes"></a>API kullanımı modları

Özel karar Hizmet Web sayfalarını ve mobil uygulamaları için uygulanabilir. API'ler bir tarayıcı veya bir uygulama çağrılabilir. API kullanımı hem de benzer, ancak bazı ayrıntıları farklıdır.

## <a name="glossary-of-terms"></a>Terimler sözlüğü

Bu belgede sık birkaç koşulları oluşur:

* **Eylem kümesi**: içerik öğeleri kümesi özel karar hizmeti için derece için. Bu kümesi olarak belirtilen bir *RSS* veya *Atom* uç noktası.
* **Derecelendirme**: bir veya daha fazla eylem kümelerini özel karar hizmetine her isteği belirtir. Sistem bu kümelerinden tüm içerik seçeneklerini seçerek yanıt verir ve dereceli sırada döndürür.
* **Geri çağırma işlevi**: belirlerseniz, bu işlev, UI içeriği işler. İçerik derecelendirme sıralama özel karar hizmet tarafından döndürülen tarafından sıralanır.
* **Ödül**: kullanıcı işlenmiş içeriği nasıl yanıt verdiğini ölçü. Özel karar hizmet tıklama kullanarak kullanıcı yanıtı ölçer. Tıklama uygulamanızda eklenen özel kod kullanarak sisteme raporlanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulamanızı kaydetme](custom-decision-service-get-started-register.md) özel karar hizmetiyle
* En iyi duruma getirme başlamak [bir Web sayfası](custom-decision-service-get-started-browser.md) veya [bir akıllı telefon uygulaması](custom-decision-service-get-started-app.md).
* Başvurun [API Başvurusu](custom-decision-service-api-reference.md) sağlanan işlevselliği hakkında daha fazla bilgi için.