---
title: Azure faturalama Kurumsal API'leri | Microsoft Docs
description: Raporlama sağlayan kurumsal Azure müşterileri, tüketim verilerini programlı olarak çekmek API'leri hakkında bilgi edinin.
services: ''
documentationcenter: ''
author: mumami
manager: mumami
editor: ''
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: erikre
ms.openlocfilehash: 5722e05e5a5e3a57b4d12b70b14f8674364f824b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244827"
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a>Kurumsal müşteriler için raporlama API'lerini genel bakış
Raporlama API'lerini program aracılığıyla tercih edilen veri analizi Araçları Kullanım ve faturalandırma veri çekmek Kurumsal Azure müşterilerine etkinleştirin. Kurumsal müşteriler işaretli bir [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) anlaşma parasal taahhütler olun ve Azure kaynakları için özel fiyatlandırma için erişim kazanmak için Azure ile.

## <a name="enabling-data-access-to-the-api"></a>API veri erişimini etkinleştirme
* **Oluşturma veya API anahtarını alma** -Enterprise Portal'da oturum açın ve raporlara gidin > kullanımı indir > API anahtarını almak veya oluşturmak için API erişim anahtarı.
* **API anahtarları geçirme** -API anahtarı kimlik doğrulaması ve yetkilendirme her çağrı için geçirilmesi gerekiyor. Şu özellik için HTTP üstbilgileri olması gerekiyor

|İstek üst bilgisi anahtarı | Değer|
|-|-|
|Yetkilendirme| Değeri şu biçimde belirtin: **taşıyıcı {apı_key}** <br/> Örnek: taşıyıcı eyr....09| 

## <a name="consumption-apis"></a>Tüketim API'leri
Swagger uç noktası kullanılabilir [burada](https://consumption.azure.com/swagger/ui/index) kolay iç denetim API'si ve istemci SDK'ları kullanarak oluşturma yeteneği etkinleştirmelisiniz API'leri açıklanan yönelik [AutoRest](https://github.com/Azure/AutoRest) veya [Swagger CodeGen](https://swagger.io/swagger-codegen/). 1 Mayıs 2014'ten itibaren veriler, bu API aracılığıyla kullanılabilir. 

* **Bakiye ve Özet** - [bakiyenizi ve Özet API](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) aylık bakiyelerini, yeni satın alma işlemleri, Azure Market hizmeti ücreti, ayarlamalar ve fazla kullanım ücretleri bilgilerin özetini sunar.

* **Kullanım ayrıntılarını** - [kullanım ayrıntısı API'si](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) Tüketilen Miktar ve bir kayıt tahmini ücretleri günlük bir dökümünü sağlar. Sonuç, ayrıca örnekler, Ölçümler ve bölümleri hakkında bilgi içerir. API, fatura dönemi veya belirtilen başlangıç ve bitiş tarihi sorgulanabilir. 

* **Market Store ücret** - [Market Store ücret API](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) (bir kez ücretleri dahil değildir) başlangıç ve bitiş tarihleri ve belirtilen fatura dönemi için günlük kullanım tabanlı Market ücretleri dökümünü döndürür.

* **Fiyat listesi** - [fiyat listesi API'si](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) verilen kayıt ve faturalama dönemi için geçerli hızı için her bir ölçüm sağlar.

* **Ayrılmış örnek ayrıntıları** - [ayrılmış örnek kullanımını API](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) kullanımı, ayrılmış örnek satın döndürür. [Ayrılmış örnek API ücretleri](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) yapılan faturalandırma işlemlerini gösterir. 

## <a name="data-freshness"></a>Veri güncelliği
Yukarıdaki API yanıtında Etag'ler döndürülür. Etag bir değişiklik verilerin yenilenmiş gösterir.  Sonraki çağrılarında aynı parametreleri kullanarak aynı API anahtarı "If-None-Match" http isteği üstbilgisi ile yakalanan Etag geçirin. Verileri başka yenilenmemiş ve hiçbir veri döndürülmez yanıt durum kodu "NotModified" olacaktır. API etag herhangi bir değişiklik olduğunda gereken süre için tam veri kümesi döndürür.

## <a name="helper-apis"></a>Yardımcı API'leri
 **Liste faturalama dönemleri** - [faturalandırma dönemlerini API'si](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) ters kronolojik sırada belirtilen kayıt tüketim verilerini elde faturalandırma listesini döndürür. Her dönem veri - BalanceSummary, UsageDetails, Market ücretlerini ve fiyat dört kümeleri API yolunu işaret eden bir özellik içerir.


## <a name="api-response-codes"></a>API yanıt kodları   
|Yanıt durum kodu|`Message`|Açıklama|
|-|-|-|
|200| Tamam|Hata yok|
|401| Yetkilendirilmemiş| API anahtarı bulunamadı, geçersiz, vb. süresi doldu.|
|404| Kullanılamaz| Rapor bitiş noktası bulunamadı|
|400| Bozuk İstek| Geçersiz params – tarih aralıkları EA numaralarını vb.|
|500| Sunucu hatası| İstek işlenirken beklenmeyen hata oluştu| 









