---
title: Market teklifleri durumunu görüntüleme | Azure Market
description: Azure ve AppSource bulut iş ortağı portalını kullanarak Marketlerden teklifler durumunu görüntüleyin
services: Azure, AppSource, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: pabutler
ms.openlocfilehash: fff89dd8a17aaf6d45462edeaa22f1d2efc8d02b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064312"
---
# <a name="view-the-publishing-status-of-azure-marketplace-and-appsource-offers"></a>Azure Market ve AppSource teklifleri Yayımlama durumunu görüntüleyin

Sonra teklif oluşturma ve yayımlama işlemi sırasında özellikle bulut iş ortağı Portalı'nda teklifinizi durumunu görüntüleyebilirsiniz.  Yayımlama durumu genel kullanıma sunulmuştur [ **tüm sunar** ](../portal-tour/cpp-all-offers-page.md) ve [ **onaylar** ](../portal-tour/cpp-approvals-page.md) Portal sayfaları.  Aşağıdaki durum göstergeleri biri, her bir teklifin görüntülenmelidir.  

|            Durum              |   Açıklama                                                           |
|            ------              |   -----------                                                           |
| **-**                          | Teklif oluşturuldu ancak değil yayımlama işlemi başladı.            |
| **Yayımlama devam ediyor**        | Teklif, yayımlama işleminin adımlarını yolu çalışmaktadır.   |
| **Yayımlama başarısız oldu**             | Kritik bir sorunu doğrulama veya gözden geçirme sırasında Microsoft tarafından bulundu. |
| **Yayımlama iptal edildi**           | Yayımcı, teklif yayımlama işlemi iptal etti.  Bu durum, mevcut bir teklifi Market'te delist değil. | 
| **Bekleniyor yayımcı Oturumu Kapat** | Teklif, Microsoft tarafından gözden geçirildi ve artık yayımcı tarafından son bir doğrulama bekler. |
| **Delisted**                   | Daha önce yayımlanan bir teklifi Market'te kaldırıldı.      | 
|  |  |


## <a name="publishing-status-details"></a>Yayımlama durumu ayrıntıları 

Teklif durumu hakkında daha fazla ayrıntı yayımlama sürecinde geçebileceği bulunan **durumu** sekmesinde **yeni teklif** sayfası.  Bu sayfada Teklif türü için tüm yayımlama adımları listelenir.  *Sayısını ve belirli adımları genellikle teklif türleri arasında farklı olduğunu unutmayın.*  Bu sayfa ayrıca Microsoft doğrulama ve yayımlama işlemine devam etmeden önce genellikle yayımcı tarafından eylem gerektiren gözden geçirme adımları tarafından oluşturulan tüm bekleyen sorunları gösterir.  Örneğin, aşağıdaki gösterir görüntüde **durumu** yeni bir sanal makine teklifi için sekmesinde. 

![VM teklifi için sekmesinde durumu](./media/vm-offer-pub-steps1.png)

Sonraki örnekte **durumu** sağlama yönetimlerini raporlanan bir hata gösteren bir danışmanlık hizmeti için sekmesinde.  Yayımlama devam etmeden önce sağlama Yönetimi danışmanlık hizmetleri için gerekli olduğundan, bu hata düzeltilmelidir.

![Danışmanlık hizmeti gösteren hata durumu sekmesi](./media/consulting-service-error.png)

Azure uygulaması son örnek durumu kritik bir Microsoft gözden geçirme sorunu gösterir.  Bu gözden geçirme sorun hakkında ayrıntılı bilgi içeren Azure DevOps öğeyi etkin bağlantı var.  Daha fazla bilgi için [Azure yayımlama uygulaması teklif](cpp-publish-offer.md).

![Gözden geçirme sorunu gösteren bir Azure uygulamasına yönelik durumu sekmesi](../azure-applications/media/status-tab-ms-review.png)


## <a name="next-steps"></a>Sonraki adımlar

Bekleyen sorunları düzeltin veya teklif ayarlarını güncelleştirmek için şunları yapmanız gerekir [bir teklifi güncelleştirme](./cpp-update-offer.md). 
