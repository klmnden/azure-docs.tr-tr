---
title: Web API Arabirimi - bilgi keşfetme hizmeti API'si
titlesuffix: Azure Cognitive Services
description: Web API Arabirimi bilgi keşfetme hizmeti (KES içinde) API zengin ve anlamsal arama deneyimi oluşturmak için kullanın.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 970db5984eedebf98bbb087cfd0b4a35a21a0f54
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60814420"
---
# <a name="web-api-interface"></a>Web API Arabirimi

Bilgi keşfetme hizmeti tarafından oluşturulan model dosyaları barındırılır ve bir dizi web API'leri erişilebilir.  API'leri kullanarak yerel makinede barındırılabileceği [ `host_service` ](CommandLine.md#host_service-command) komutunu ya da bir Azure bulut hizmetini kullanmayı dağıtılabilir [ `deploy_service` ](CommandLine.md#deploy_service-command) komutu.  Her iki tekniği aşağıdaki API uç noktalarını kullanıma sunar:

* [*Yorumlar* ](interpretMethod.md) – doğal dil sorgu dizesini yorumlar. Ek açıklamalı yorumlar döndürerek, kullanıcının ne yazmakta olduğunun tahmin edildiği zengin bir arama kutusu otomatik tamamlama deneyimi sağlar.
* [*Değerlendirme* ](evaluateMethod.md) – Evaluates ve çıktıyı bir yapılandırılmış sorgu ifadesi döndürür.
* [*calchistogram* ](calchistogramMethod.md) – bir yapılandırılmış sorgu ifadesi tarafından döndürülen nesnelere ait öznitelik değerleriyle histogramını hesaplar.

Birlikte kullanıldığında, bu API yöntemleri bir anlam zengin arama deneyimi oluşturulmasına izin verin.  Doğal dil sorgu dizesi verilmiş *yorumlama* yöntemi açıklamalı giriş sorgusu ile yapılandırılmış sorgu ifadeleri, temel dil bilgisi ve dizin verileri temel sürümleri sağlar.  *Değerlendirmek* yöntemi yapılandırılmış sorgu ifadesini değerlendirir ve görüntülemek için eşleşen dizin nesnelerini döndürür.  *Calchistogram* yöntemi, filtreleme ve iyileştirme etkinleştirmek için öznitelik değeri dağıtımları hesaplar.

**Örnek**

Kullanıcı ' % s'dizesi "görünmeyen s" yazarsa bir akademik yayınlar etki alanındaki *yorumlama* yöntemi, bir dizi kullanıcı "gizli anlam analizi" anahtar sözcüğü arama önerme dereceli yorumlaması sağlayabilir görünmeyen yapısı başlığı "analiz" veya "ile görünmeyen s" ile başlayan diğer ifadeler.  Bu bilgiler kullanıcıyı hızla istenen arama sonuçlarına ulaştırmak için kullanılabilir.

Bu etki alanı için *değerlendirmek* yöntemi, bir dizi yayınlar akademik dizinden eşleşen almak için kullanılabilir ve *calchistogram* yöntemi, öznitelik dağıtımını hesaplamak için kullanılabilir Daha fazla filtre uygulamak için kullanılan ve arama sonuçlarını daraltmak eşleşen yayınlar için değerler.

Örneklerin daha kolay okunmasını sağlamak için REST API çağrılarında URL'de kodlanmamış olan karakterlerin (boşluk gibi) bulunduğuna dikkat edin. Kodunuzda uygun URL kodlama işlemlerini gerçekleştirmeniz gerekir.
