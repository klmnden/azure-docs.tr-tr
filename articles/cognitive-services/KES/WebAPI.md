---
title: Bilgi Bankası araştırması hizmeti API'si Web API arabiriminde | Microsoft Docs
description: Web API Arabirimi içinde bilgi araştırması hizmet (KES) API Bilişsel Hizmetleri'nde bir zengin, semantik arama deneyimi oluşturmak için kullanın.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 16c5680eb4f249a5d37e6b90eea92cfff7090eef
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351545"
---
# <a name="web-api-interface"></a>Web API Arabirimi
Bilgi araştırması hizmeti tarafından oluşturulan model dosyalarında barındırılan ve bir dizi web API'leri erişilebilir.  Yerel makinenin kullanılması API'leri barındırılabilir [ `host_service` ](CommandLine.md#host_service-command) komutu veya bir Azure bulut hizmeti kullanmaya dağıtılabileceği [ `deploy_service` ](CommandLine.md#deploy_service-command) komutu.  Her iki tekniği aşağıdaki API uç noktaları ortaya çıkarır:
* [*Yorumlar* ](interpretMethod.md) – doğal dil sorgu dizesi yorumlar. Ek açıklamalı yorumlar döndürerek, kullanıcının ne yazmakta olduğunun tahmin edildiği zengin bir arama kutusu otomatik tamamlama deneyimi sağlar.
* [*Değerlendirme* ](evaluateMethod.md) – Evaluates ve bir yapılandırılmış sorgu ifadesi çıktısını döndürür.
* [*calchistogram* ](calchistogramMethod.md) – bir yapılandırılmış sorgu ifadesi tarafından döndürülen nesne öznitelik değerlerini Histogramı hesaplar.

Birlikte kullanıldığında, bu API yöntemlerini zengin anlamsal arama deneyimi oluşturulmasını sağlar.  Doğal dil sorgu dizesi verilen *yorumlama* yöntemi giriş sorgusu temel alınan dilbilgisi ve dizin verilere dayalı yapılandırılmış sorgu ifadelerle açıklamalı sürümlerini sağlar.  *Değerlendirmek* yöntemi yapılandırılmış sorgu ifadesi değerlendirir ve görüntülenecek eşleşen dizin nesneleri döndürür.  *Calchistogram* yöntemi, filtreleme ve iyileştirme etkinleştirmek için öznitelik değeri dağıtımları hesaplar.

**Örnek**

Kullanıcı "görünmeyen s" dizesi yazarsa akademik yayınlar etki alanında *yorumlama* yöntemi, kullanıcının "görünmeyen anlamsal analiz", anahtar sözcüğü arama öneren dereceli yorumlar kümesi sağlayabilir Başlık "görünmeyen yapısı analiz" veya "görünmeyen s" ile başlayan diğer ifadeler.  Bu bilgiler, kullanıcının istenen arama sonuçları hızlı bir şekilde kılavuzluk etmesi için kullanılabilir.

Bu etki alanı için *değerlendirmek* yöntemi, yayınlar akademik dizinden eşleşen kümesi almak için kullanılabilir ve *calchistogram* yöntemi, öznitelik dağıtımını hesaplamak için kullanılabilir Daha fazla filtre uygulamak için kullanılan ve arama sonuçlarını daraltın eşleşen yayınlar için değerler.

Örnekler okunabilirliğini artırmak için REST API çağrıları, URL kodlanmış edilmemiş (alanları gibi) karakterden olduğunu unutmayın. Kodunuzu uygun URL Kodlamalar uygulamak gerekir.
