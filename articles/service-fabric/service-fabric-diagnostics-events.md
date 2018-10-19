---
title: Azure Service Fabric olayları | Microsoft Docs
description: Azure Service Fabric kümenizi izlemenize yardımcı olması için kullanıma hazır sağlanan Service Fabric olayları hakkında bilgi edinin.
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
ms.openlocfilehash: ca63d67f6d7c19b4ca6928c4cc0f9ccb06eace2b
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49402990"
---
# <a name="service-fabric-events"></a>Service Fabric olayları 

Service Fabric platform içinde küme'olmuyor anahtar işletimsel etkinlikler için birkaç yapılandırılmış olayları yazar. Küme yükseltme bu aralıktan çoğaltma yerleştirme kararları için. Her olay, Service Fabric kümesinde aşağıdaki varlıkları birine eşlemeleri gösterir:
* Küme
* Uygulama
* Hizmet
* Bölüm
* Çoğaltma 
* Kapsayıcı

-Platform tarafından sunulan olayların tam listesi görmek için [Service Fabric listesi olayları](service-fabric-diagnostics-event-generation-operational.md).

Önemli senaryolar kümenizde olayları görmelisiniz bazı örnekleri aşağıda verilmiştir. 
1. Düğüm yaşam döngüsü olaylarını: düğümleri gelen, aşağı git, etkin/devre dışı veya yeniden gibi olayları ne olduğunu gösteren sunulur ve varsa bir makine ile sorun veya aracılığıyla BT için çağrılan bir API olduysa belirlemenize yardımcı bir düğümün durumunu değiştirin.
1. Küme Yükseltmesi: kümenizi yükseltilmiş olarak (SF sürümü veya yapılandırma değişikliği), yükseltmeyi tamamlamak başlatmak ve her biri, UD geri görürsünüz (veya geri alma). 
1. Uygulama yükseltme: aracılığıyla yükseltme yapar gibi olayları kapsamlı bir dizi olduğunda, küme için benzer şekilde yükseltir. Bu olayların ne zaman yükseltme zamanlandı, geçerli yükseltme durumunu ve genel olaylar dizisini anlamak yararlı olabilir. Bu geri ne yükseltmelerini başarıyla piyasaya sunuluyor görmek arama kullanışlıdır.
1. Uygulama/hizmet dağıtımı / silme: olaylar vardır için her bir uygulama, hizmet ve kapsayıcı oluşturuluyor veya siliniyor.
1. Bölüm taşır (yeniden): durum bilgisi olan bir bölümü yeniden yapılandırma (çoğaltma kümesi bir değişiklik) geçer her bir olay kaydedilir. Bu ne sıklıkta anlamak çalışıyorsanız, bölüm çoğaltma kümesi değiştiriyor veya hangi düğümün zaman içinde herhangi bir noktada birincil çoğaltma çalışan izlemek yararlı olur.
1. Kaos olayları: Service Fabric'in kullanırken [Chaos](service-fabric-controlled-chaos.md) hizmet, hizmet başlatılmış veya durdurulmuşsa her zaman veya sistemdeki bir hata ekler olayları görürsünüz.
1. Sistem durumu olayları: Service Fabric her zaman bir uyarı veya hata sistem durumu raporu oluşturulur, bir varlığın bir Tamam sistem durumuna döner veya bir sistem durumu raporu süresi sistem durumu olaylarını kullanıma sunar. Bu olayları bir varlık için geçmiş sağlık istatistikleri izlemek çok yararlı olur. 

## <a name="how-to-access-events"></a>Olayları erişme

Service Fabric olayları erişilebilen birkaç farklı yolu vardır:
* işlevsel kanal. Bunlar Azure tanılama uzantısı aracılığıyla toplanacak ve bir depolama tablosuna tüketim veya Azure Log Analytics gibi bir araç içine alma için gönderilecek. "Tanılama" bir küme için etkinleştirildiğinde, Azure tanılama aracısını kümenizde dağıttığınız ve işlevsel kanal günlüklerinden de okumak için yapılandırılmış varsayılan olarak. Yapılandırma hakkında daha fazla okuma [Azure tanılama aracısını](service-fabric-diagnostics-event-aggregation-wad.md) kümenizi daha fazla günlük veya performans sayaçlarını alacak şekilde Tanılama yapılandırmasını değiştirmek için. 
* Eventstore'a hizmetin Rest API'leri aracılığıyla, doğrudan veya Service Fabric istemci kitaplığı aracılığıyla küme sorgu olanak sağlar. Bkz: [küme olayları için sorgu EventStore API'leri](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi - kümenizi izlemeye [platform ve küme izleme](service-fabric-diagnostics-event-generation-infra.md).
* Eventstore'a hizmeti hakkında-daha fazla bilgi [Eventstore'a hizmetine genel bakış](service-fabric-diagnostics-eventstore.md)
