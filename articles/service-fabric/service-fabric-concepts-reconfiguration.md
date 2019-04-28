---
title: Azure Service fabric'te yeniden yapılandırma | Microsoft Docs
description: Service fabric'te bölümleri yeniden yapılandırılmasına anlama
services: service-fabric
documentationcenter: .net
author: appi101
manager: anuragg
editor: ''
ms.assetid: d5ab75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2018
ms.author: aprameyr
ms.openlocfilehash: a24aa6aa1695a3d1166816b7960bdd7b551e1a37
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60882206"
---
# <a name="reconfiguration-in-azure-service-fabric"></a>Azure Service fabric'te yeniden yapılandırma
A *yapılandırma* çoğaltmalar ve kendi rolleri için durum bilgisi olan hizmet ilişkin bir bölüm olarak tanımlanır.

A *yeniden yapılandırma* başka bir yapılandırma için bir yapılandırma taşıma işlemi gerçekleşir. Çoğaltma kümesi için bir durum bilgisi olan hizmet ilişkin bir bölüm için bir değişiklik yapar. Eski yapılandırma olarak adlandırılır *önceki yapılandırma (PC)*, ve yeni yapılandırma olarak adlandırılır *geçerli yapılandırmasını (CC)*. Azure Service Fabric yeniden yapılandırma protokolünde tutarlılığı korur ve kullanılabilirlik çoğaltma kümesine herhangi bir değişiklik sırasında tutar.

Yük Devretme Yöneticisi sistemde farklı olaylara yanıt olarak yeniden yapılandırmalar başlatır. Örneğin, birincil sonra bir yeniden yapılandırma başarısız olursa birincil etkin bir ikincil yükseltmek için başlatılır. Birincil düğüm yükseltmek için başka bir düğüme taşımak gerekli olabilir, başka bir yanıt olarak uygulama yükseltmeleri örnektir.

## <a name="reconfiguration-types"></a>Yeniden yapılandırma türleri
İki tür olarak yeniden yapılandırmalar sınıflandırılabilir:

- Birincil burada değişiyor yeniden yapılandırmalar:
    - **Yük devretme**: Yük devretme birincil çalışan bir hatanın yanıtta yeniden yapılandırmalar var.
    - **SwapPrimary**: Service Fabric bir çalışan birincil bir düğümden diğerine, Yük Dengeleme için genellikle yanıt taşımak için gereken yere yeniden yapılandırmalar veya yükseltme takasları var.

- Burada birincil değişmiyor yeniden yapılandırmalar.

## <a name="reconfiguration-phases"></a>Yeniden yapılandırma aşamaları
Bir yeniden yapılandırma birkaç aşamada çalışır:

- **Phase0**: Bu aşama, burada geçerli birincil durumuna yeni birincil ve ikincil etkin geçişleri aktarır takas birincil yeniden yapılandırmalar içinde gerçekleşir.

- **Aşama 1**: Bu aşama, birincil burada değişiyor yeniden yapılandırmalar sırasında gerçekleşir. Bu aşamada, Service Fabric geçerli çoğaltmaları arasında doğru birincil tanımlar. Yeni birincil zaten seçilmiş olduğundan bu aşama takas birincil yeniden yapılandırmalar sırasında gerekli değildir. 

- **Aşama 2**: Bu aşamada, Service Fabric, tüm veri Çoğunluk yapılandırmasına kopyasının kullanılabilir olmasını sağlar.

Yalnızca dahili kullanım olan diğer birkaç aşama vardır.

## <a name="stuck-reconfigurations"></a>Takılan yeniden yapılandırmalar
Yeniden yapılandırmalar alabilirsiniz *takılı* çeşitli nedenlerle için. Bazı yaygın nedenleri şunlardır:

- **Aşağı çoğaltmaları**: Bazı yeniden yapılandırma aşamaları kadar bir çoğaltma yapılandırmasında Çoğunluk gerektirir.
- **Ağ veya iletişim sorunları**: Yeniden yapılandırmalar farklı düğümler arasında ağ bağlantısı gerektirir.
- **API hataları**: Hizmet uygulamaları belirli API'leri son yeniden Yapılandırma Protokolü gerektirir. Örneğin, bir güvenilir hizmet iptal belirtecini uygularken değil, takılabilir SwapPrimary yeniden yapılandırmalar neden olur.

Sistem durumu raporlarının sistem bileşenlerinden kullanın, System.FM, System.RA ve System.RAP gibi bir yeniden yapılandırma burada tanılamak için takılı kalıyor. [Sistem durumu rapor sayfası](service-fabric-understand-and-troubleshoot-with-system-health-reports.md) bu sistem durumu raporları açıklar.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Reliable Services yaşam döngüsü - C#](service-fabric-reliable-services-lifecycle.md)
- [Sistem durumu raporlarını](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [Çoğaltmalar ve örnekler](service-fabric-concepts-replica-lifecycle.md)
