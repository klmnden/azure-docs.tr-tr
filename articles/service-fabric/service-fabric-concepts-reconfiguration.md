---
title: Azure Service Fabric yeniden | Microsoft Docs
description: Service Fabric bölüm yeniden yapılandırma anlama
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
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34212815"
---
# <a name="reconfiguration-in-azure-service-fabric"></a>Azure Service Fabric içinde yeniden yapılandırma
A *yapılandırma* çoğaltmaları ve kendi rolleri için durum bilgisi olan hizmet bölümü olarak tanımlanır.

A *yeniden yapılandırma* başka bir yapılandırma için bir yapılandırma taşıma işlemidir. Çoğaltma durum bilgisi olan bir hizmet için bir bölüm kümesi için bir değişiklik yapar. Eski yapılandırma olarak adlandırılır *önceki yapılandırma (PC)*, ve yeni yapılandırmayı adlı *geçerli yapılandırma (CC)*. Azure Service Fabric yeniden yapılandırma protokolünde tutarlılık korur ve kullanılabilirlik çoğaltma kümesi herhangi bir değişiklik sırasında korur.

Yük Devretme Yöneticisi sistemde farklı olaylarına yanıt olarak yeniden yapılandırmaların başlatır. Örneğin, birincil sonra bir yeniden yapılandırma başarısız olursa birincil etkin bir ikincil yükseltmek için başlatılır. Birincil düğüm yükseltmek için başka bir düğüme taşımak gerekli olabilir, başka bir uygulama yükseltme yanıt örnektir.

## <a name="reconfiguration-types"></a>Yeniden yapılandırma türleri
Yeniden yapılandırmaların iki türlerine sınıflandırılabilir:

- Burada birincil değiştirme yeniden yapılandırmaların:
    - **Yük devretme**: yük devretme işlemlerini olan birincil çalışan bir hata yanıtı yeniden yapılandırmaların.
    - **SwapPrimary**: Takasları olduğunuz nerede Service Fabric gereken çalışmasını birincil bir düğümden diğerine, Yük Dengeleme için genellikle yanıt taşımak için yeniden yapılandırmaların veya yükseltme.

- Burada birincil değiştirmeden yeniden yapılandırmaların.

## <a name="reconfiguration-phases"></a>Yeniden yapılandırma aşamaları
Bir yeniden yapılandırma birkaç evrede çalışır:

- **Phase0**: Bu aşama burada geçerli birincil aktarır durumuna etkin ikincil geçişleri ve yeni birincil takas birincil yeniden yapılandırmaların içinde gerçekleşir.

- **Aşama 1**: Bu aşama birincil burada değiştiriyor yeniden yapılandırmaların sırasında olur. Bu aşamada, Service Fabric geçerli çoğaltmalar arasında doğru birincil tanımlar. Yeni birincil zaten seçilmiş olan çünkü bu aşaması sırasında takas birincil yeniden yapılandırmaların gerekli değildir. 

- **Aşama 2**: Bu aşamada, Service Fabric tüm verileri çoğunu geçerli yapılandırma, çoğaltmaları kullanılabilir olmasını sağlar.

Yalnızca dahili kullanım içindir birkaç aşamadan vardır.

## <a name="stuck-reconfigurations"></a>Takılan yeniden yapılandırmaların
Yeniden yapılandırmaların alabilirsiniz *takılmış* çeşitli nedenlerle için. Bazı yaygın nedenler şunlardır:

- **Aşağı çoğaltmaları**: bazı yeniden yapılandırma aşamaları yukarı olmasını yapılandırmasında çoğaltmaları çoğunu gerektirir.
- **Ağ veya iletişimi sorunları**: yeniden yapılandırmaların farklı düğümler arasında ağ bağlantısı gerektirir.
- **API hataları**: hizmet uygulamaları belirli API'leri son yeniden yapılandırma protokol gerektirir. Örneğin, güvenilir bir hizmette iptal belirteci uygularken değil Kutusu'nda SwapPrimary yeniden yapılandırmaların neden olur.

Sistem bileşenleri sistem durumu raporlarını kullanmak için System.FM, System.RA ve System.RAP gibi bir yeniden yapılandırma burada tanılamak için takıldı. [Sistem durumu raporu sayfası](service-fabric-understand-and-troubleshoot-with-system-health-reports.md) bu sistem durumu raporları açıklar.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Reliable Services yaşam döngüsü - C#](service-fabric-reliable-services-lifecycle.md)
- [Sistem durumu raporları](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [Çoğaltmalar ve örnekler](service-fabric-concepts-replica-lifecycle.md)
