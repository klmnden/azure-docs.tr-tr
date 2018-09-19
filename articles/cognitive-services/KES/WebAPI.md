---
title: Web API Arabirimi - bilgi keşfetme hizmeti API'si
titlesuffix: Azure Cognitive Services
description: Web API Arabirimi bilgi keşfetme hizmeti (KES içinde) API zengin ve anlamsal arama deneyimi oluşturmak için kullanın.
services: cognitive-services
author: bojunehsu
manager: cgronlun
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 5be39e8dce6aeeef32d20273c56650620d6fe986
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46122034"
---
# <a name="web-api-interface"></a>Web API Arabirimi

Bilgi keşfetme hizmeti tarafından oluşturulan model dosyaları barındırılır ve bir dizi web API'leri erişilebilir.  API'leri kullanarak yerel makinede barındırılabileceği [ `host_service` ](CommandLine.md#host_service-command) komutunu ya da bir Azure bulut hizmetini kullanmayı dağıtılabilir [ `deploy_service` ](CommandLine.md#deploy_service-command) komutu.  Her iki tekniği aşağıdaki API uç noktalarını kullanıma sunar:

* [*Yorumlar* ](interpretMethod.md) – doğal dil sorgu dizesini yorumlar. Ek açıklamalı yorumlar döndürerek, kullanıcının ne yazmakta olduğunun tahmin edildiği zengin bir arama kutusu otomatik tamamlama deneyimi sağlar.
* [*Değerlendirme* ](evaluateMethod.md) – Evaluates ve çıktıyı bir yapılandırılmış sorgu ifadesi döndürür.
* [*calchistogram* ](calchistogramMethod.md) – bir yapılandırılmış sorgu ifadesi tarafından döndürülen nesnelere ait öznitelik değerleriyle histogramını hesaplar.

Birlikte kullanıldığında, bu API yöntemleri bir anlam zengin arama deneyimi oluşturulmasına izin verin.  Doğal dil sorgu dizesi verilmiş *yorumlama* yöntemi açıklamalı giriş sorgusu ile yapılandırılmış sorgu ifadeleri, temel dil bilgisi ve dizin verileri temel sürümleri sağlar.  *Değerlendirmek* yöntemi yapılandırılmış sorgu ifadesini değerlendirir ve görüntülemek için eşleşen dizin nesnelerini döndürür.  *Calchistogram* yöntemi, filtreleme ve iyileştirme etkinleştirmek için öznitelik değeri dağıtımları hesaplar.

**Örnek**

Kullanıcı ' % s'dizesi "görünmeyen s" yazarsa bir akademik yayınlar etki alanındaki *yorumlama* yöntemi, bir dizi kullanıcı "gizli anlam analizi" anahtar sözcüğü arama önerme dereceli yorumlaması sağlayabilir görünmeyen yapısı başlığı "analiz" veya "ile görünmeyen s" ile başlayan diğer ifadeler.  Bu bilgiler, kullanıcının istenen arama sonuçlarını hızla yol göstermesi için kullanılabilir.

Bu etki alanı için *değerlendirmek* yöntemi, bir dizi yayınlar akademik dizinden eşleşen almak için kullanılabilir ve *calchistogram* yöntemi, öznitelik dağıtımını hesaplamak için kullanılabilir Daha fazla filtre uygulamak için kullanılan ve arama sonuçlarını daraltmak eşleşen yayınlar için değerler.

Örnekler okunabilirliğini geliştirmek için REST API çağrıları URL kodlanmış olan karakterler (örneğin, boşluk) içerdiğini unutmayın. Kodunuz, uygun URL'yi Kodlamalar uygulamak gerekir.
