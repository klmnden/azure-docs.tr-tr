---
title: Özel Karar Alma Hizmeti nedir?
titlesuffix: Azure Cognitive Services
description: Bu makalede, Özel Karar Alma Hizmeti’ne genel bir bakış sunulmaktadır.
services: cognitive-services
author: alekh
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-decision-service
ms.topic: overview
ms.date: 05/08/2018
ms.author: slivkins
ms.openlocfilehash: 273f2965a0fcaaa729175c5232da1aba69589eec
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364220"
---
# <a name="what-is-custom-decision-service"></a>Özel Karar Alma Hizmeti nedir?

Tipik bir web uygulamasında veya mobil uygulamada ön sayfa bağlantısı birçok makaleye veya diğer içerik türlerine bağlantı sunar. Ön sayfa yüklendikçe Özel Karar Alma Hizmeti’nin ön sayfada yer alan makaleleri derecelendirmesini isteyebilir. Bu nedenle, bir kullanıcı üzerine tıklayarak bir makaleyi seçtiğinde, Özel Karar Alma Hizmeti’ne ikinci bir istek gönderilebilir ve böylece o kullanıcı kararının çıktısı günlüğe kaydedilir.

Özel Karar Alma Hizmeti yalnızca içeriğiniz için bir RSS akışı ve uygulamanıza birkaç JavaScript satırının eklenmesini gerektirdiğinden, kullanımı kolaydır.

Özel Karar Alma Hizmeti, içeriğinizi makine öğrenmesi için özelliklere dönüştürür. Sistem; metin, görüntüler, videolar ve genel yaklaşım açısından içeriğinizi anlamak için bu özellikleri kullanır. [Varlık Bağlama](../entitylinking/home.md), [Metin Analizi](../text-analytics/overview.md), [Duygu Tanıma](../emotion/home.md) ve [Görüntü İşleme](../computer-vision/home.md) gibi diğer birçok [Microsoft Bilişsel Hizmetleri](https://www.microsoft.com/cognitive-services)’ni kullanır.

Özel Karar Alma için bazı yaygın kullanım örnekleri şunlardır:

* Haber web sitesindeki makaleleri kişiselleştirme
* Medya portalındaki video içeriğini kişiselleştirme
* Reklamın yönlendirdiği web sayfalarını veya reklam yerleşimlerini iyileştirme
* Alışveriş web sitesindeki önerilen ürünleri derecelendirme.

Özel Karar Alma Hizmeti şu anda *ücretsiz genel önizleme aşamasındadır*. Bir web sitesindeki veya uygulamadaki makalelerin listesini kişiselleştirebilir. Özellik ayıklama, İngilizce dilindeki içerikle en iyi şekilde çalışır. İspanyolca, Fransızca, Almanca, Portekizce ve Japonca gibi diğer diller için [sınırlı işlevsellik](../text-analytics/overview.md) sunulur. Yeni işlevsellik kullanılabilir oldukça bu belge de revize edilecektir.

Özel Karar Alma Hizmeti, içerik kişiselleştirme etki alanında bulunmayan uygulamalarda kullanılabilir. Bu uygulamalar, özel önizleme için ideal olabilir. Daha fazla bilgi için [bize ulaşın](https://azure.microsoft.com/overview/sales-number/).

## <a name="api-usage-modes"></a>API kullanımı modları

Özel Karar Alma Hizmeti hem web sayfalarına hem de mobil uygulamalara uygulanabilir. API’ler bir tarayıcıdan veya uygulamadan çağrılabilir. API kullanımı her ikisine benzer, ancak bazı ayrıntılar farklıdır.

## <a name="glossary-of-terms"></a>Terimler sözlüğü

Bu belgede bazı terimler sık sık geçmektedir:

* **Eylem kümesi**: Özel Karar Alma Hizmeti için derecelendirilecek içerik öğeleri kümesi. Bu küme bir *RSS* veya *Atom* uç noktası olarak belirtilebilir.
* **Derecelendirme**: Özel Karar Alma Hizmeti’ne yapılan her istek bir veya daha fazla eylem kümesini belirtir. Sistem bu kümelerden tüm içerik seçeneklerini seçerek yanıt verir ve bunları sıralı bir düzende döndürür.
* **Geri çağırma işlevi**: Belirttiğiniz bu işlev, kullanıcı arabiriminizde içeriği işler. İçerik, Özel Karar Alma Hizmeti tarafından döndürülen sıralı düzene göre sıralanır.
* **Ödül**: Kullanıcının işlenen içeriğe nasıl yanıt verdiğine yönelik bir ölçü. Özel Karar Alma Hizmeti, tıklamaları kullanarak kullanıcı yanıtını ölçer. Tıklamalar, uygulamanıza eklenen özel kod kullanılarak sisteme bildirilir.

## <a name="next-steps"></a>Sonraki adımlar

* Özel Karar Alma Hizmeti’ne [uygulamanızı kaydetme](custom-decision-service-get-started-register.md)
* [Bir web sayfasını](custom-decision-service-get-started-browser.md) veya [akıllı telefon uygulamasını](custom-decision-service-get-started-app.md) iyileştirmeye başlayın.
* Sağlanan işlevler hakkında daha fazla bilgi edinmek için [API başvurusu](custom-decision-service-api-reference.md)’na bakın.