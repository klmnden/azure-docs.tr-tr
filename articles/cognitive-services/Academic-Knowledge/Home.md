---
title: Akademik bilgi API'si Microsoft akademik grafik için | Microsoft Docs
description: Akademik bilgi API'si, kullanıcı sorgularının yorumlar ve Microsoft Bilişsel hizmetler akademik grafikte zengin bilgi almak için kullanın.
services: cognitive-services
author: mvorvoreanu
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: mivorvor
ms.openlocfilehash: e241f9a87cd58b62eafd754bd3cb4283aa0a1e92
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352343"
---
# <a name="academic-knowledge-api"></a>Akademik Bilgi API'si

Akademik bilgi API'si hoşgeldiniz. Bu hizmet ile kullanıcı sorgularındaki akademik amacı yorumlayabilir ve Microsoft Academic Graph’ten (MAG) buna uygun kapsamlı bilgiler getirebilirsiniz. MANYETİK Bilgi Bankası bilimsel etkinlikleri modellemek varlıklarının oluşan bir web ölçekli heterojen varlık grafiktir: incelemesi, yazar, kurum, kağıt, salonundan ve olay alanı. 

MANYETİK veri Bing web dizini ve bunun yanı sıra şirket içi bir Bilgi Bankası ' bing'den incelenmis. Bing dizin oluşturma devam eden sonucu olarak, bu API bulma izleyerek ve Bing tarafından dizin Web'den yeni bilgileri içerir. Bu veri kümesine bağlı olarak, akademik bilgi API'leri reaktif arama öngörülü öneri deneyimleri, zengin araştırma kağıt grafik arama sonuçları ve histogram dağıtımlarını ile sorunsuz bir şekilde bir araya getiren bir Bilgi Bankası temelli, etkileşimli iletişim kutusu sağlar öznitelik değerlerini yazıları ve ilişkili varlık kümesi.

Microsoft akademik Graph hakkında daha fazla bilgi için bkz: [ http://aka.ms/academicgraph ](http://aka.ms/academicgraph).

Akademik bilgi API'si Bilişsel hizmetler Önizlemesi'nden Bilişsel hizmetler Labs taşınmıştır. Proje için yeni giriş sayfasıdır: [ https://labs.cognitive.microsoft.com/en-us/project-academic-knowledge ](https://labs.cognitive.microsoft.com/en-us/project-academic-knowledge). Varolan API anahtarınıza 24 Mayıs 2018 kadar çalışmaya devam eder. Bu tarihten sonra lütfen yeni bir API anahtarı oluşturun. Lütfen varolan anahtarınızı sona erdikten sonra Ücretli Önizleme artık kullanılabilir unutmayın. Ücretsiz katmanı API amaçlarınız için yeterli değilse ekibimiz temasa geçin. 

## <a name="features"></a>Özellikler
Akademik bilgi API'si dört ilgili REST uç noktaları oluşur:  
  1. **Yorumlar** – doğal dil kullanıcı sorgu dizesi yorumlar. Ek açıklamalı yorumlar döndürerek, kullanıcının ne yazmakta olduğunun tahmin edildiği zengin bir arama kutusu otomatik tamamlama deneyimi sağlar.  
  2. **Değerlendirme** – bir sorgu ifadesi değerlendirir ve akademik bilgi varlık sonuçları döndürür.  
  3. **calchistogram** – bir histogram öznitelik değerleri verilen yazar yıla göre alıntıları dağıtımını gibi bir sorgu ifadesi tarafından döndürülen akademik varlıklar için dağıtım hesaplar.  
  4. **Grafik arama** – belirli bir grafik arar desen ve eşleşen varlık sonuçları döndürür.

Birlikte kullanıldığında, bu API yöntemlerini zengin anlamsal arama deneyimi oluşturmanızı sağlar. Bir kullanıcı sorgu dizesi verilen **yorumlama** yöntemi bir sürümüyle açıklamalı sorgu ve isteğe bağlı olarak kullanıcının sorgu temel akademik semantiği tabanlı Tamamlanıyor çalışırken bir yapılandırılmış sorgu ifadesi, sağlar veriler. Örneğin, bir kullanıcı dize türleri *görünmeyen s*, **yorumlama** yöntemi, bir kullanıcı çalışma alanı için arama öneren dereceli yorumlar kümesi sağlayabilir  *görünmeyen semantik analizi*, kağıt *görünmeyen yapısı analiz*, veya ile başlayan diğer varlık ifadeler *görünmeyen s*. Bu bilgiler, kullanıcının istenen arama sonuçları hızlı bir şekilde kılavuzluk etmesi için kullanılabilir.

**Değerlendirmek** yöntemi, akademik Bilgi Bankası kağıt varlıklardan eşleşen kümesi almak için kullanılabilir ve **calchistogram** yöntemi, öznitelik değerleri dağıtımını hesaplamak için kullanılabilir Daha fazla arama sonuçlarını filtrelemek için kullanılan kağıt varlık kümesi için.        

**Grafik arama** yöntemi iki modu vardır: *json* ve *lambda*. *Json* modu grafik desen bir JSON nesnesi tarafından belirtilen grafik desenlerine göre eşleştirme gerçekleştirebilir. *Lambda* modu, kullanıcı tarafından belirtilen lambda ifadeleri göre grafik çapraz geçişlerine sırasında sunucu tarafı hesaplamalar gerçekleştirebilir.

## <a name="getting-started"></a>Başlarken 
Ayrıntılı belgeler için soldaki konuları bakın.  Örnekler okunabilirliğini artırmak için REST API çağrıları, URL kodlanmış edilmemiş (alanları gibi) karakterden olduğunu unutmayın.  Kodunuzu uygun URL Kodlamalar uygulamak gerekir.
