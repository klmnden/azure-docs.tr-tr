---
title: "Azure kaynak durumu genel bakış | Microsoft Docs"
description: "Azure kaynak durumu genel bakış"
services: Resource health
documentationcenter: 
author: shawntabrizi
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/01/2017
ms.author: shawn.tabrizi
ms.openlocfilehash: 50a173a3d3a10ed59492b4a1d64173913f331639
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="azure-resource-health-overview"></a>Azure kaynak durumu genel bakış
 
Azure kaynak durumu tanılamanıza ve bir Azure hizmeti sorunu kaynaklarınızı etkilediğinde destek alma yardımcı olur. Geçerli ve geçmiş kaynaklarınızın sistem durumunu hakkında bilgilendirir. Ve sorunları azaltmaya yardımcı olmak için teknik destek sağlar.

Oysa [Azure durum](https://status.azure.com) Azure müşterilerine geniş bir dizi etkileyen hizmet sorunlar hakkında sizi bilgilendirir, kaynak durumu kaynaklarınızın sistem durumunu, kişiselleştirilmiş bir Pano sağlar. Kaynak durumu kaynaklarınızı Azure hizmeti sorunları nedeniyle geçmişte kullanılamaz her zaman gösterir. Ardından, bir SLA ihlal edildiğini anlamak için basit değildir. 

## <a name="resource-definition-and-health-assessment"></a>Kaynak tanımı ve sistem durumu değerlendirmesi
Belirli bir örnek bir Azure hizmetini bir kaynaktır: Örneğin, bir sanal makine, bir web uygulaması veya bir SQL veritabanı.

Kaynak durumu sinyalleri üzerinde farklı Azure Hizmetleri tarafından yayılan bir kaynak sağlıklı olup olmadığını değerlendirmek için kullanır. Bir kaynak sağlıksız ise, kaynak durumu sorunun kaynağını belirlemek için ek bilgiler analiz eder. Aynı zamanda Microsoft sorunu düzeltmek için sürüyor eylemler veya sorunun nedenini adres için gerçekleştirebileceğiniz eylemleri tanımlar. 

Sistem durumu nasıl değerlendirildiği, ek ayrıntılar gözden kaynak türlerinin tam listesi ve sistem durumu denetler [Azure kaynak durumu](resource-health-checks-resource-types.md).

## <a name="health-status"></a>Sistem durumu
Bir kaynak sistem durumu aşağıdaki durumlardan biri olarak görüntülenir.

### <a name="available"></a>Kullanılabilir
Durumu **kullanılabilir** hizmet kaynak durumunu etkileyen herhangi bir olayı algılandı kurmadı anlamına gelir. Burada kaynak kurtarıldı Planlanmamış kapalı kalma süresi son 24 saatte durumlarda, gördüğünüz **yakın zamanda çözülmüş** bildirim.

![Bir sanal makine için "kullanılabilir" durumunu "Yakın zamanda çözülmüş" bildirimi](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Kullanılamaz
Durumu **kullanılamıyor** hizmet devam eden platform veya kaynak sağlığını etkileyen platform olmayan olay algıladı anlamına gelir.

#### <a name="platform-events"></a>Platform olayları
Platform olayları birden çok Azure altyapı bileşenleri tarafından tetiklenir. Zamanlanmış Eylemler (örneğin, planlı bakım) ve beklenmeyen olaylar (örneğin, bir planlanmamış ana bilgisayar yeniden başlatma) içerirler.

Kaynak durumu olayı ve kurtarma işlemi hakkında ek ayrıntılar sağlar. Ayrıca, etkin bir Microsoft sözleşmesi destek yüklü olmasa bile, desteği ile iletişim sağlar.

![Platform olayı nedeniyle sanal makine için "Kullanılamıyor" durumu](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Platform olmayan olaylar
Platform olmayan olaylar kullanıcıların eylemler tarafından tetiklenir. Örnekler bir sanal makineyi durdurmak veya bir Redis önbelleğine bağlantı sayısını ulaşmasını.

!["Kullanılabilir" durumunu olmayan platform olayı nedeniyle sanal makine için](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Bilinmiyor
Sistem durumunu **bilinmeyen** kaynak durumu 10 dakikadan bu kaynak hakkındaki bilgileri kurmadı aldığını gösterir. Bu durum kaynak durumunun kesin bir gösterge olmasa da, bir sorun giderme sürecini önemli veri noktası var.

Kaynak beklendiği gibi çalışıyorsa, durumunu kaynağının değiştirir **kullanılabilir** birkaç dakika sonra.

Kaynak sorunlarla karşılaşıyorsanız **bilinmeyen** sistem durumu platform olayda kaynak söz konusu önermek.

![Bir sanal makine için "Bilinmeyen" durumu](./media/resource-health-overview/Unknown.png)

### <a name="degraded"></a>Düşürüldü
Sistem durumunu **Degraded** kullanımı için hala mevcut olmasına rağmen kaynak, performans kaybı algıladı gösterir.
Farklı kaynaklar bunlar kaynak düşer belirttiğinizde, kendi ölçütlerini sahiptir.

![Bir sanal makine için "Degraded" durumu](./media/resource-health-overview/degraded.png)

## <a name="reporting-an-incorrect-status"></a>Yanlış bir durum bildirimi
Geçerli sistem durumunu hatalı olduğunu düşünüyorsanız, bize seçerek bilebilir **yanlış sistem durumu raporu**. Bir Azure sorun, burada söz konusu durumlarda kaynak durumu desteğine başvurma öneririz. 

![Yanlış bir durumu hakkındaki bilgileri göndermek için kutusu](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a>Geçmiş bilgileri
Sistem durumu geçmişini 14 güne kadar seçerek erişebilirsiniz **geçmişini görüntüleme** kaynak durumu içinde. 

![Son iki hafta içindeki kaynak durumu olayların listesi](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a>Başlarken
Kaynak durumu için bir kaynak açmak için:
1.  Azure Portal’da oturum açın.
2.  Kaynağınızı bulun.
3.  Sol bölmede Kaynak menüsünde seçin **kaynak durumu**.

![Kaynak durumu kaynak görünümünden açma](./media/resource-health-overview/from-resource-blade.png)

Kaynak durumu seçerek de erişebilirsiniz **tüm hizmetleri** yazarak **kaynak durumu** filtre metni kutusuna. İçinde **Yardım + Destek** bölmesinde, [kaynak durumu](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).

![Kaynak durumu "Tüm hizmetleri" açma](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Sonraki adımlar

Kaynak durumu hakkında daha fazla bilgi için şu kaynaklara bakın:
-  [Kaynak türleri ve sistem durumu denetler Azure kaynak durumu](resource-health-checks-resource-types.md)
-  [Azure kaynak durumu hakkında sık sorulan sorular](resource-health-faq.md)




