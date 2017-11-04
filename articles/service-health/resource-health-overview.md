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
ms.openlocfilehash: 040d58a81a9b41fe660e4276d698bf884f90bb6c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-resource-health-overview"></a>Azure kaynak sistem durumu genel bakış
 
Kaynak Durumu, bir Azure sorunu kaynaklarınızı etkilediğinde bunu tanılamanıza ve destek almanıza yardımcı olur. Kaynaklarınızın güncel ve geçmiş durumu hakkında bilgiler sağlar ve sorunları azaltmaya yardımcı olur. Kaynak Durumu, Azure hizmet sorunları ile ilgili yardıma ihtiyacınız olduğunda teknik destek sağlar.

Oysa [Azure durum](https://status.azure.com) Azure müşterilerine geniş bir dizi etkileyen hizmeti sorunları hakkında bilgilendirir, kaynak durumu kaynaklarınızın sistem durumunu içeren bir kişiselleştirilmiş Pano sağlar. Kaynak durumu kaynaklarınızı Azure hizmeti sorunları nedeniyle geçmişte kullanılamaz her zaman gösterir. Bu, sizin için bir SLA ihlal edildiğini anlamak basit kolaylaştırır. 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a>Bir kaynak olarak kabul ve kaynak sağlıklı olup olmadığını karar nasıl kaynak durumu mu?
Örneğin bir Azure hizmeti aracılığıyla Azure Resource Manager tarafından sunulan bir kaynak türünün bir örneği bir kaynaktır: bir sanal makine, bir web uygulaması veya bir SQL veritabanı.

Kaynak durumu sinyalleri üzerinde farklı Azure Hizmetleri tarafından yayılan bir kaynak sağlıklı olup olmadığını değerlendirmek için kullanır. Bir kaynak sağlıksız ise, kaynak durumu sorunun kaynağını belirlemek için ek bilgiler analiz eder. Ayrıca Microsoft, sorunu düzeltmek için sürüyor eylemleri tanımlar veya hangi eylemleri sorunun nedenini gidermek için atabileceğiniz. 

Kaynak türleri tam listesini gözden geçirin ve sistem durumu denetler [Azure kaynak durumu](resource-health-checks-resource-types.md) sistem durumu nasıl değerlendirildiği hakkında ek bilgi.

## <a name="health-status-provided-by-resource-health"></a>Kaynak durumu tarafından sağlanan sistem durumu
Bir kaynak sistem durumu aşağıdaki durumlardan biri:

### <a name="available"></a>Kullanılabilir
Hizmet kaynak durumunu etkileyen herhangi bir olayı algılamadı. Burada kaynak kurtarıldı Planlanmamış kapalı kalma süresi son 24 saatte durumlarda görürsünüz **kısa süre önce kurtarıldıysa** bildirim.

![Kaynak sistem durumu kullanılabilir sanal makine](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Kullanılamıyor
Hizmet bir devam eden platform veya kaynak sağlığını etkileyen olmayan platform olayı algıladı.

#### <a name="platform-events"></a>Platform olayları
Bu olaylar, birden çok Azure altyapı bileşenleri tarafından tetiklenen ve hem planlı bakım gibi zamanlanmış Eylemler ve planlanmamış ana bilgisayar yeniden başlatma gibi beklenmeyen olaylar içerir.

Kaynak sistem durumu kurtarma işlemi olayına ek ayrıntılar sağlar ve etkin bir Microsoft sözleşmesi destek yüklü olmasa bile, desteğe başvurun olanak tanır.

![Kaynak durumu kullanılamıyor sanal makine platform olayı nedeniyle](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Platform olmayan olaylar
Bu olaylar, örneğin bir sanal makineyi durdurmak veya bir Redis önbelleğine bağlantı sayısını ulaşmasını kullanıcılar tarafından gerçekleştirilen eylemler tarafından tetiklenir.

![Kaynak durumu kullanılamıyor sanal makine olmayan platform olayı nedeniyle](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Bilinmeyen
Bu sistem durumu, kaynak durumu 10 dakikadan bu kaynak hakkındaki bilgileri almadığını gösterir. Bu durum, kaynak durumunun kesin bir gösterge olmamasına karşın, bir sorun giderme sürecini önemli veri noktası verilmiştir:
* Kaynak kaynağının durumunu beklendiği gibi çalışıyorsa kullanılabilir için birkaç dakika sonra güncelleştirin.
* Kaynak sorunlarla karşılaşıyorsanız, bilinmeyen sistem durumunu kaynak Platform bir olay tarafından etkilenen önerebilir.

![Kaynak sistem durumu bilinmeyen sanal makine](./media/resource-health-overview/Unknown.png)

## <a name="report-an-incorrect-status"></a>Yanlış bir durum raporu
Herhangi bir noktada, geçerli sistem durumunu hatalı olduğunu düşünüyorsanız, bize tıklayarak bilebilir **yanlış sistem durumu raporu**. Burada bir Azure sorundan etkilenen durumlarda kaynak sistem durumu dikey penceresinden desteği ile iletişim öneririz. 

![Kaynak durumu rapor yanlış durumu](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a>Geçmiş bilgileri
14 gün geçmiş durumu verilerinin tıklatarak erişebilirsiniz **geçmişini görüntüleme** kaynak sistem durumu dikey. 

![Kaynak durumu rapor geçmişi](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a>Başlarken
Bir kaynak için kaynak durumu açmak için
1.  Azure portalında oturum açın.
2.  Kaynağınıza gidin.
3.  Sol tarafta bulunan kaynak menüye tıklayın **kaynak durumu**.

![Kaynak dikey penceresinden açık kaynak durumu](./media/resource-health-overview/from-resource-blade.png)

Kaynak durumu tıklatarak da erişebilirsiniz **daha fazla hizmet**, yazarak **kaynak durumu** açmak için filtre metni kutusuna **Yardım + Destek** dikey. Son olarak tıklatın [ **kaynak durumu**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).

![Daha fazla hizmetinden açık kaynak durumu](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Sonraki adımlar

Kaynak durumu hakkında daha fazla bilgi için şu kaynaklara bakın:
-  [Kaynak türleri ve sistem durumu denetler Azure kaynak durumu](resource-health-checks-resource-types.md)
-  [Azure kaynak durumu hakkında sık sorulan sorular](resource-health-faq.md)




