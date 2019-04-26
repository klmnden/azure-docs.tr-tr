---
title: Dynamics 365 müşteri katılımı teknik bilgileri sekmesi - Azure Marketi için | Microsoft Docs
description: Dynamics 365 Customer Engagement uygulaması için teknik bilgi AppSource Market'te belirleme.
services: Dynamics 365 for Customer Engagement Offer, Azure, Marketplace, Cloud Partner Portal, AppSource
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 03/05/2019
ms.author: pbutlerm
ms.openlocfilehash: 1dd488c2eb419b5e210a48d7a94f7d0bb423a2b1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60332897"
---
# <a name="dynamics-365-for-customer-engagement-technical-info-tab"></a>Dynamics 365 müşteri katılımı teknik bilgileri sekmesi için

**Teknik bilgi** sekmesinde **yeni teklif** sayfası, Dynamics 365 Customer Engagement uygulaması, CRM paket ve pazarlama logosu varlıklar dahil olmak üzere ilgili ayrıntılı bilgileri belirtmenize imkan tanır.  Bu sekme, dört bölümlere ayrılmıştır: **Uygulama bilgileri**, **CRM paket**, **CRM paket kullanılabilirlik**, ve **Yapıtları pazarlama**. Bir alan adı eklenmiş bir yıldız (*) gerekli olduğunu gösterir. 


## <a name="application-info-section"></a>Uygulama bilgileri bölümü

Bu bölümde, Dynamics 365 uygulama hakkında ayrıntıları sağlayacaktır.

![Uygulama bilgileri bölümünden teknik bilgi sekmesi](./media/dynce-technical-info-tab1.png)

Aşağıdaki tabloda, bu alanlar açıklanır.

|      Alan                    |    Açıklama                  |
|    ---------                  |  ---------------                |
|   Temel lisans modeli          |  Lisans modeli, müşterilerin uygulamanızı Dynamics 365 Yönetim merkezinde nasıl atanacağını belirler. **Kaynak** lisanslama örnek tabanlı ise **kullanıcı** lisansları, Kiracı başına bir atanır.  |
|  Giden S2S & CRM güvenli Store erişimi |  Yapılandırma CRM güvenli Store veya sunucudan sunucuya (S2S) giden erişim sağlar. *Bu özellik, Dynamics 365 ekibinden özel durum sertifika aşaması sırasında gerektirir.* Microsoft bu özelliği desteklemek için ek adımları tamamlamak için sizinle iletişime geçecektir.  |
| CRM yaşam döngüsü olaylarına abone olma | Dynamics 365 yaşam döngüsü olayları ile tümleştirme, Microsoft ile özel bir sözleşme aracılığıyla kaydedilen adanmış bir hizmet sağlamak gerektirir. *Bu özellik, Dynamics 365 ekibinden özel durum sertifika aşaması sırasında gerektirir.* Bu özelliği destekleyen eksiksiz ek adımları kurulur.  |
| Uygulama Yapılandırma URL'si | Uygulamayı yapılandırmak kullanıcının sağlayan web sayfasının URL'si |
| İlgili Dynamics 365 ürünleri  | Bu teklif için geçerli bir Dynamics 365 ürünleri seçin. Bu teklif seçili ürünlerin appsource'ta görünür.  |
| Değişiklik yalnızca pazarlama         | Bu seçeneği Evet gösterir için var olan bir teklif için yalnızca pazarlama/açıklayıcı değişiklikler yapıldı ayarlanıyor.  Bu değişikliklere izin verecek sertifika ve sağlama aşamaları atlamak bir teklif.  |
|  |  |


## <a name="crm-package-section"></a>CRM paket bölümü

Bu bölümde, AppSource paket dosyası hakkında ayrıntılar sağlayacaktır.  Bu bilgiler, Dynamics 365 doğrulama ve sertifika ekipleri tarafından kullanılır.

![CRM paketin bölümünü teknik bilgi sekmesi](./media/dynce-technical-info-tab2.png)

Aşağıdaki tabloda, bu alanlar açıklanır.

|      Alan                    |    Açıklama                  |
|    ---------                  |  ---------------                |
|  Paketinin dosya adı     |  Kullanımınızın paket (.zip) dosya adı.  Bu ad *değil* genel ve Dynamics 365 sertifika ekibi tarafından dahili olarak kullanılır.  |
|  Url                          |  Karşıya yüklenen paket dosyası içeren bir Azure depolama hesabı URL'si. Bu URL'yi paketiniz için doğrulama alması için ekibimize izin vermek için bir salt okunur SAS anahtarını içermesi gerekir.  |
| Birden fazla crm paket     | Crm farklı paketleri ile birden çok sürümünü destekleniyorsa Evet sadece'i seçin.  Her sürüm ayrı ayrı oluşturmanız gerekir, karşılık gelen paket dosyası tht sahip olur.  |
| Senaryo ve kullanım örneği varlık   | Dynamics 365 doğrulama ekibi tarafından kullanılmak üzere uygulamanız için işlev belirtimi belgenin bir karşıya yükleme sağlar.  Bu belirtim tercih edilen biçimi [E2E kullanıcı senaryosu şablonu](https://isvdocumentation.blob.core.windows.net/d365documentation/Power%20Platform%20E2E%20document.docx).  |
|  |  |


## <a name="crm-package-availability-section"></a>CRM paket kullanılabilirlik bölümü

Bu bölümde, uygulamanızın hangi coğrafi bölgeler müşterilerine olacağını seçin.  Aşağıdaki bağımsız bölgeler için dağıtmadan *özel izinler ve doğrulama gerektiren* sertifika işlemi sırasında: [Almanya](https://docs.microsoft.com/azure/germany/), [ABD kamu Bulutu](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome)ve ipucu.


## <a name="marketing-artifacts-section"></a>Pazarlama Yapıtları bölümü

Bu bölümde, paketinizin Appsource'ta Market'te temsil etmek için kullanılacak bir uygulama logonuzu gerektirir.  Logo resmi PNG biçiminde olmalı ve boyutu 255 x 115 piksel olması gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Tamamlayarak uygulamanızı Tanıtımı teklif öneririz [Test Sürüşü sekmesi](./cpp-testdrive-tab.md)
