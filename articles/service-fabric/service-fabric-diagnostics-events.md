---
title: Azure Service Fabric olayları | Microsoft Docs
description: Azure Service Fabric kümesi izlemenize yardımcı olması için kullanıma hazır sunulan Service Fabric olaylar hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: b9372c806eab1b0ca69ba078d972b076c8a7d6f6
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="service-fabric-events"></a>Service Fabric olayları 

Service Fabric platformundan kümenizi içinde gerçekleştiği anahtar işletimsel etkinlikler için birkaç yapılandırılmış olayları yazar. Küme yükseltme bu aralıktan çoğaltma yerleştirme kararları için. Her olay Service Fabric küme aşağıdaki varlıklarda birine eşlemeleri gösterir:
* Küme
* Uygulama
* Hizmet
* Bölüm
* Çoğalt 
* Kapsayıcı

-Platform tarafından kullanıma sunulan olayların tam bir listesini görmek için [Service Fabric listesi olayları](service-fabric-diagnostics-event-generation-operational.md).

Kümenizdeki olaylarını görmelisiniz önemli senaryolar bazı örnekleri aşağıda verilmiştir. 
1. Düğüm yaşam döngüsü olayları: düğümleri gündeme, Git aşağı, etkin/devre dışı veya yeniden gibi olayları neler olduğunu gösteren sunulur ve varsa makine ile bir sorun veya BT için aracılığıyla çağrıldı bir API değilse tanımlamanıza yardımcı olması bir düğümün durumunu değiştirin.
1. Yükseltme küme: kümeniz yükseltilmiş olarak (BT sürümü veya yapılandırma değişikliği), yükseltmeyi başlatmak, her, UDs alma ve tamamlama görürsünüz (veya geri alma). 
1. Uygulama yükseltme: aracılığıyla yükseltme yapar gibi olayları kapsamlı bir kümesini olduğunda, küme için benzer şekilde yükseltir. Bu olayların ne zaman yükseltme zamanlandığı, yükseltme geçerli durumunu ve olayları genel dizi anlamak yararlı olabilir. Bu geri ne başarıyla yükseltmeler çıktığından görmek arama kullanışlıdır.
1. Uygulama/hizmet dağıtımı / silme: her uygulama, hizmet ve kapsayıcı oluşturuluyor veya için olayları şunlardır.
1. Bölüm taşır (yeniden): durum bilgisi olan bir bölümü yeniden yapılandırma (Çoğaltma kümesinde bir değişiklik) geçer her bir olay kaydedilir. Bu ne sıklıkta anlamak çalışıyorsanız, bölüm çoğaltma kümesi değiştiriyor veya hangi düğümün zamanında herhangi bir noktada birincil çoğaltmanın çalışan izlemek yararlı olur.
1. Chaos olayları: Service Fabric'ın kullanırken [Chaos](service-fabric-controlled-chaos.md) hizmet, hizmet çalışmaya durduruldu veya her zaman ya da bir arıza sistemde yerleştirir olayları görürsünüz.
1. Sistem durumu olayları: Service Fabric bir uyarı veya hata durumu raporu oluşturulur, veya bir varlık için bir Tamam sistem durumu geri gider ya da bir sistem durumu raporu süresi her zaman sistem durumu olayları gösterir. Bu olaylar, bir varlık için geçmiş durumu istatistiklerini izlemek çok faydalıdır. 

## <a name="how-to-access-events"></a>Nasıl yapılır erişimi olayları

Service Fabric olayları erişilebilen birkaç farklı yolu vardır:
* işletimsel kanalı. Bunlar Azure tanılama uzantısını toplanacak ve depolama tabloya tüketimi veya OMS günlük analizi gibi bir araç içine alımı için gönderilecek. "Tanılama" bir küme için etkinleştirildiğinde, Azure Tanılama Aracı kümenizde dağıtılır ve işletimsel kanal günlüklerinden okumak için yapılandırılmış varsayılan olarak açıktır. Yapılandırma hakkında daha fazla okuma [Azure Tanılama Aracı](service-fabric-diagnostics-event-aggregation-wad.md) daha fazla günlükleri veya performans sayaçları seçmek için küme Tanılama yapılandırmasını değiştirmek için. 
* EventStore hizmetin Rest API'leri aracılığıyla, doğrudan veya Service Fabric istemci kitaplığı aracılığıyla küme sorgu olanak sağlar. Bkz: [küme olayları için sorgu EventStore API'leri](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi üzerinde kümenizi - izleme [küme ve platform izleme](service-fabric-diagnostics-event-generation-infra.md).
* EventStore hizmeti hakkında-daha fazla bilgi [EventStore hizmetine genel bakış](service-fabric-diagnostics-eventstore.md)
