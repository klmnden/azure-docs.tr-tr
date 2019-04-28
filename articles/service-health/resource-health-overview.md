---
title: Azure kaynak durumu genel bakış | Microsoft Docs
description: Azure kaynak durumu genel bakış
author: stephbaron
ms.author: stbaron
ms.topic: conceptual
ms.service: service-health
ms.workload: Supportability
ms.date: 11/16/2018
ms.openlocfilehash: d2a77e831290aa1ee0fcb6d4addf8f6e90786d52
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62119867"
---
# <a name="azure-resource-health-overview"></a>Azure kaynak durumu genel bakış
 
Azure kaynak durumu, tanılamanıza ve bir Azure hizmet sorunu kaynaklarınızı etkilediğinde destek almanıza yardımcı olur. Güncel ve geçmiş kaynaklarınızın sistem durumunu hakkında bilgilendirir. Ve sorunları azaltmaya yardımcı olmak için teknik destek sağlar.

Oysa [Azure durumu](https://status.azure.com) çok sayıda Azure müşterileri etkileyecek hizmet sorunlar hakkında sizi bilgilendirir, kaynak durumu, kaynaklarınızın durumuna kişiselleştirilmiş bir Pano sağlar. Kaynak Durumu, geçmişte Azure hizmetiyle ilgili sorunlar nedeniyle kaynaklarınızın kullanılamaz durumda olduğu zamanları gösterir. Ardından, sizin için bir SLA ihlal edilen anlamak basit değildir. 

## <a name="resource-definition-and-health-assessment"></a>Kaynak tanımı ve sistem durumu değerlendirmesi
Bir Azure hizmeti belirli bir örneğini bir kaynaktır: Örneğin, bir sanal makineyi, bir web uygulaması veya bir SQL veritabanı.

Kaynak durumu sinyal üzerinde farklı Azure Hizmetleri tarafından yayılan bir kaynağı iyi durumda olup olmadığını değerlendirmek için kullanır. Kaynak durumu, bir resource sağlam değil, sorunun kaynağını belirlemek için ek bilgileri analiz eder. Ayrıca, sorunu gidermek için Microsoft sürüyor veya sorunun nedenini düzeltmek için gerçekleştirebileceğiniz eylemler de tanımlar. 

Sistem durumu nasıl değerlendirdiğini ilgili ek ayrıntılar kaynak türlerinin tam listesini gözden geçirin ve durum denetimleri [Azure kaynak durumu](resource-health-checks-resource-types.md).

## <a name="health-status"></a>Sistem durumu
Kaynak durumu, aşağıdaki durumlardan biri olarak görüntülenir.

### <a name="available"></a>Kullanılabilir
Durumu **kullanılabilir** hizmet kaynak durumunu etkileyen herhangi bir olayı algılandı taşınmadığından anlamına gelir. Burada kaynak kurtarıldı Planlanmamış kapalı kalma süresi son 24 saat durumlarda gördüğünüz **yakın zamanda çözülen** bildirim.

![Bir sanal makine için "kullanılabilir" durumunu "Yakın zamanda çözülmüş" bildirimi](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Kullanılamaz
Durumu **kullanılamıyor** hizmeti devam eden platform ya da kaynak sağlığını etkileyen platform olmayan olayı algıladı anlamına gelir.

#### <a name="platform-events"></a>Platform etkinlikleri
Platform olayı birden çok Azure altyapısının bileşenleri tarafından tetiklenir. Bunlar, hem zamanlanmış Eylemler (örneğin, planlı bakım) hem de beklenmedik olaylar (örneğin, bir beklenmeyen ana bilgisayar yeniden başlatma) içerir.

Kaynak durumu, olay ve kurtarma işlemi hakkında ek ayrıntılar sağlar. Ayrıca destek etkin bir Microsoft olmasa bile destek ile iletişime geçmenizi sağlar.

![Platform olayı nedeniyle sanal makine için "Kullanılamıyor" durumu](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Platform olmayan olaylar
Platform olmayan olaylar kullanıcıların eylemler tarafından tetiklenir. Örnekler bir sanal makineyi durdurmak veya bir Azure önbelleği için bağlantıları Redis için en fazla sayıda.

![Bir platform olayı nedeniyle sanal makine için "Kullanılamıyor" durumu](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Bilinmeyen
Sistem durumunu **bilinmeyen** bu kaynak hakkında bilgi için 10 dakikadan daha fazla kaynak durumu henüz aldığını gösterir. Bu durum kaynak durumunun eksiksiz bir gösterimi olmasa da, bir sorun giderme işlemi önemli veri noktası var.

Kaynak beklenen şekilde çalışıyorsa, kaynak durumu değişir **kullanılabilir** birkaç dakika sonra.

Kaynak ile ilgili sorunlar yaşıyorsanız **bilinmeyen** sistem durumu platform içindeki bir olay kaynağı etkilediğini Öner.

![Bir sanal makine için "Bilinmeyen" durumu](./media/resource-health-overview/Unknown.png)

### <a name="degraded"></a>Düşürüldü
Sistem durumunu **Degraded** kullanımı için hala kullanılabilir olsa da, kaynak, performans, kayıp algıladığını belirtir.
Farklı kaynaklar için ne zaman bir kaynak düzeyinin düşürüldüğünü belirlediği kendi ölçütlerine sahiptirler.

![Bir sanal makine için "Degraded" durumu](./media/resource-health-overview/degraded.png)

## <a name="reporting-an-incorrect-status"></a>Hatalı bir durum bildirimi
Geçerli sistem durumunu hatalı olduğunu düşünüyorsanız, seçerek bize **yanlış sistem durumu raporu**. Burada bir Azure sorunundan etkileniyor durumlarda, kaynak durumu destek ile iletişime geçmenizi öneriyoruz. 

![Hatalı bir durum hakkında bilgi göndermek için arama kutusu](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a>Geçmiş bilgileri
En fazla 14 gün içinde sistem durumu geçmişi erişebileceğiniz **sistem durumu geçmişi** kaynak durumu bölümü. 

![Son iki hafta içinde kaynak durumu olayları listesi](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a>Başlarken
Bir kaynak için kaynak durumu açmak için:
1.  Azure Portal’da oturum açın.
2.  Kaynağınızı bulun.
3.  Sol bölmede Kaynak menüsünde **kaynak durumu**.

![Kaynak durumu kaynak görünümünde açma](./media/resource-health-overview/from-resource-blade.png)

Kaynak durumu seçerek de erişebilirsiniz **tüm hizmetleri** yazarak **kaynak durumu** filtre metin kutusuna. İçinde **Yardım + Destek** bölmesinde [kaynak durumu](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).

![Kaynak durumu "Tüm hizmetler" açma](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Sonraki adımlar

Kaynak durumu hakkında daha fazla bilgi için bu kaynaklara göz atın:
-  [Kaynak türleri ve sistem durumu denetimleri bulunan Azure kaynak durumu](resource-health-checks-resource-types.md)
-  [Azure kaynak durumu hakkında sık sorulan sorular](resource-health-faq.md)




