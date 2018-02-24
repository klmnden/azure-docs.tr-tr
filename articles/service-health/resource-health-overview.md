---
title: "Azure kaynak sistem durumu genel bakış | Microsoft Docs"
description: "Azure kaynak durumu genel bakış"
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/01/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: 9912911d6ed43a70a7a0f9316079d8f66d063f64
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="azure-resource-health-overview"></a>Azure kaynak sistem durumu genel bakış
 
Kaynak Durumu, bir Azure sorunu kaynaklarınızı etkilediğinde bunu tanılamanıza ve destek almanıza yardımcı olur. Kaynaklarınızın güncel ve geçmiş durumu hakkında bilgiler sağlar ve sorunları azaltmaya yardımcı olur. Kaynak Durumu, Azure hizmet sorunları ile ilgili yardıma ihtiyacınız olduğunda teknik destek sağlar.

Oysa [Azure durum](https://status.azure.com) Azure müşterilerine geniş bir dizi etkileyen hizmeti sorunları hakkında bilgilendirir, kaynak durumu kaynaklarınızın sistem durumunu içeren bir kişiselleştirilmiş Pano sağlar. Kaynak durumu kaynaklarınızı kullanılamayan bir SLA ihlal edildiğini anlamak için basit yaparak Azure hizmeti sorunları nedeniyle geçmişte her zaman gösterir. 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a>Bir kaynak olarak kabul ve kaynak sağlıklı olup olmadığını karar nasıl kaynak durumu mu?
Örneğin belirli bir örnek bir Azure hizmetini bir kaynak değildir: bir sanal makine, bir web uygulaması veya bir SQL veritabanı.

Kaynak durumu sinyalleri üzerinde farklı Azure Hizmetleri tarafından yayılan bir kaynak sağlıklı olup olmadığını değerlendirmek için kullanır. Bir kaynak sağlıksız ise, kaynak durumu sorunun kaynağını belirlemek için ek bilgiler analiz eder. Ayrıca Microsoft, sorunu düzeltmek için sürüyor eylemleri tanımlar veya hangi eylemleri sorunun nedenini gidermek için atabileceğiniz. 

Kaynak türleri tam listesini gözden geçirin ve sistem durumu denetler [Azure kaynak durumu](resource-health-checks-resource-types.md) sistem durumu nasıl değerlendirildiği hakkında ek bilgi.

## <a name="health-status-provided-by-resource-health"></a>Kaynak durumu tarafından sağlanan sistem durumu
Bir kaynak sistem durumu aşağıdaki durumlardan biri:

### <a name="available"></a>Kullanılabilir
Hizmet kaynak durumunu etkileyen herhangi bir olayı algılandı kurmadı. Burada kaynak kurtarıldı Planlanmamış kapalı kalma süresi son 24 saatte durumlarda gördüğünüz **kısa süre önce kurtarıldıysa** bildirim.

![Kaynak sistem durumu kullanılabilir sanal makine](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Kullanılamaz
Hizmet bir devam eden platform veya kaynak sağlığını etkileyen olmayan platform olayı algıladı.

#### <a name="platform-events"></a>Platform olayları
Bu olaylar, birden çok Azure altyapı bileşenleri tarafından tetiklenir. Zamanlanmış Eylemler (örn. planlı bakım) ve beklenmeyen olaylar (örneğin bir planlanmamış ana bilgisayar yeniden başlatma) içerirler.

Kaynak durumu olayı ve kurtarma işlemi hakkında ek ayrıntılar sağlar. Ayrıca, etkin bir Microsoft sözleşmesi destek yüklü olmasa bile, desteği ile iletişim sağlar.

![Kaynak durumu kullanılamıyor sanal makine platform olayı nedeniyle](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Platform olmayan olaylar
Bu olaylar, kullanıcılar tarafından gerçekleştirilen eylemler tarafından tetiklenir. Örneğin: bir sanal makineyi durdurmak veya bağlantı bir Redis önbelleğine bağlantısı sayısı.

![Kaynak durumu kullanılamıyor sanal makine olmayan platform olayı nedeniyle](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Bilinmiyor
Bu sistem durumu, kaynak durumu 10 dakikadan bu kaynak hakkındaki bilgileri kurmadı aldığını gösterir. Bu durum kaynak durumunun kesin bir gösterge olmasa da, bir sorun giderme sürecini önemli veri noktası verilmiştir:
* Kaynak beklendiği gibi çalışıyorsa, kaynak durumu için kullanılabilen birkaç dakika sonra güncelleştirin.
* Kaynak sorunlarla karşılaşıyorsanız, bilinmeyen sistem durumunu kaynak Platform bir olay tarafından etkilenen önerebilir.

![Kaynak sistem durumu bilinmeyen sanal makine](./media/resource-health-overview/Unknown.png)

### <a name="degraded"></a>Düşürüldü
Kullanımı için hala mevcut olmasına rağmen kaynak, performans kaybı algıladı bu sistem durumu gösterir.
Bir kaynak düşürülmüş belirttiğinizde farklı kaynaklar için kendi ölçüt sahiptir.

![Sanal makine kaynak durumu düşürülmüş](./media/resource-health-overview/degraded.png)

## <a name="report-an-incorrect-status"></a>Yanlış bir durum raporu
Herhangi bir noktada, geçerli sistem durumunu hatalı olduğunu düşünüyorsanız, bize tıklayarak bilebilir **yanlış sistem durumu raporu**. Burada bir Azure sorundan etkilenen durumlarda kaynak durumu desteğine başvurma öneririz. 

![Kaynak durumu rapor yanlış durumu](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a>Geçmiş bilgileri
Sistem durumu geçmişini 14 güne kadar tıklatarak erişebilirsiniz **geçmişini görüntüleme** kaynak durumu içinde. 

![Kaynak durumu rapor geçmişi](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a>Başlarken
Bir kaynak için kaynak durumu açmak için
1.  Azure portalında oturum açın.
2.  Kaynağınıza gidin.
3.  Sol tarafta bulunan kaynak menüye tıklayın **kaynak durumu**.

![Kaynak durumu kaynak görünümünden açın](./media/resource-health-overview/from-resource-blade.png)

Kaynak durumu tıklatarak da erişebilirsiniz **tüm hizmetleri**, yazarak **kaynak durumu** açmak için filtre metni kutusuna **Yardım + Destek** dikey. Son olarak tıklatın [ **kaynak durumu**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).

![Tüm hizmetinden açık kaynak durumu](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Sonraki adımlar

Kaynak durumu hakkında daha fazla bilgi için şu kaynaklara bakın:
-  [Kaynak türleri ve sistem durumu denetler Azure kaynak durumu](resource-health-checks-resource-types.md)
-  [Azure kaynak durumu hakkında sık sorulan sorular](resource-health-faq.md)




