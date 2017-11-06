---
title: Azure Enterprise API'leri faturalama | Microsoft Docs
description: "Raporlama Tüketim verileri programlı olarak çekmesini müşteriler kuruluş Azure sağlayan API hakkında bilgi edinin."
services: 
documentationcenter: 
author: anandedwin
manager: aedwin
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: aedwin
ms.openlocfilehash: 62a69aeb7499a961f95739fb3836942b670c7320
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a>Kurumsal müşteriler için raporlama API'leri genel bakış
Raporlama API tercih edilen veri çözümleme araçları kullanım ve fatura verileri programlı olarak çekmesini müşteriler kuruluş Azure etkinleştirin. 

## <a name="enabling-data-access-to-the-api"></a>API veri erişimini etkinleştirme
* **Oluşturmak veya API anahtarını almak** - Enterprise portal ve izleme Yardım altında öğretici için oturum açma - raporlama API'leri. Bu Yardım makalenin ilk bölümünde, oluşturmak veya belirtilen kayıt API anahtarını almak açıklanmaktadır.
* **API anahtarları geçirme** -API anahtarı kimlik doğrulaması ve yetkilendirme her çağrı için geçirilmesi gerekiyor. Aşağıdaki özellikler HTTP üstbilgilerine olması gerekir

|Üstbilgi anahtarı iste | Değer|
|-|-|
|Yetkilendirme| Değer şu biçimde belirtin: **taşıyıcı {apı_key}** <br/> Örnek: taşıyıcı eyr....09|

## <a name="consumption-apis"></a>Tüketim API'leri
Swagger uç nokta kullanılabilir [burada](https://consumption.azure.com/swagger/ui/index) API'leri altında API kolay introspection ve istemci SDK'ları oluşturma yeteneği etkinleştirmelisiniz açıklanan için [AutoRest](https://github.com/Azure/AutoRest) veya [Swagger CodeGen](http://swagger.io/swagger-codegen/). 1 Mayıs 2014 başlayan veriler bu API aracılığıyla kullanılabilir. 

* **Yük Dengeleme ve Özet** - [dengelemek ve Özet API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) aylık bakiyelerini, yeni satın alma işlemleri, Azure Marketi servis ücretleri, ayarlamalar ve fazla kullanım ücretleri bilgilerin özetini sunar.

* **Kullanım ayrıntılarını** - [kullanım ayrıntı API'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) tüketilen miktarları ve bir kayıt tarafından tahmini ücretleri günlük bir dökümü sunar. Sonuç ayrıca örnekleri, Ölçümler ve Departmanlar hakkında bilgi içerir. API, faturalama dönemi veya belirtilen başlangıç ve bitiş tarihi tarafından sorgulanabilir. 

* **Market deposu ücret** - [Market deposu ücret API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) (bir kez ücretleri olmayan dahil) başlangıç ve bitiş tarihleri ve belirtilen fatura dönemi için günlük kullanım tabanlı Market ücretleri dökümünü döndürür.

* **Fiyat listesi** - [fiyat listesi API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) verilen kayıt ve Faturalama dönem için geçerli oranı her ölçer sağlar. 

## <a name="helper-apis"></a>Yardımcısı API'ları
 **Liste faturalama nokta** - [faturalama nokta API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) ters kronolojik sırada belirtilen kayıt için verilere sahip nokta faturalama bir liste döndürür. Her dönem veri - BalanceSummary, UsageDetails, Market ücretleri ve fiyat listesi dört kümelerinin API rota işaret eden bir özellik içerir.


## <a name="api-response-codes"></a>API yanıt kodları  
|Yanıt durum kodu|İleti|Açıklama|
|-|-|-|
|200| TAMAM|Hiçbir hata|
|401| Yetkilendirilmemiş| API anahtarı bulunamadı, geçersiz, vb. süresi doldu.|
|404| Kullanılamıyor| Rapor uç noktası bulunamadı|
|400| Hatalı istek| Geçersiz params – tarih aralıkları EA numaralarını vb.|
|500| Sunucu hatası| İstek işleme Unexoected hatası| 









