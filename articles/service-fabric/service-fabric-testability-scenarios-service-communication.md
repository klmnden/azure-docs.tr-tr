---
title: "Test Edilebilirlik: Hizmet iletişimi | Microsoft Docs"
description: "Hizmet hizmet iletişimi, Service Fabric uygulaması kritik tümleştirme noktasıdır. Bu makalede, tasarım konuları ve test teknikleri anlatılmaktadır."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 017557df-fb59-4e4a-a65d-2732f29255b8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: c87b5d82b6eef2b1d28a3280cc2fa07c28084f90
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="service-fabric-testability-scenarios-service-communication"></a>Service Fabric Test Edilebilirlik senaryoları: hizmet iletişimi
Mikro hizmetler ve hizmet odaklı mimari stilleri yüzey doğal olarak Azure Service Fabric. Bu dağıtılmış mimariler türlerinde bileşenlerden oluşan mikro hizmet uygulamaları genelde birbirleriyle iletişim kurmalarını gerek birden çok hizmet oluşur. En basit durumlarda bile, genellikle en az bir durum bilgisi olmayan web hizmeti ve iletişim kurması gereken bir durum bilgisi olan veri depolama hizmeti vardır.

Hizmet hizmet iletişimi kritik tümleştirme noktası bir uygulamanın çünkü her hizmetin diğer hizmetlere uzak bir API sunar. Genellikle g/ç içeren bir dizi API sınırları çalışmak bazı dikkatli iyi miktarda sınama ve doğrulama gerektirir.

Bu hizmet sınırları birlikte dağıtılmış bir sistemde kablolu sağlamak için çeşitli noktalar vardır:

* *Aktarım Protokolü*. HTTP artan birlikte çalışabilirliğini ya da özel bir ikili protokol için en yüksek verimlilik kullanacaksınız?
* *Hata işleme*. Kalıcı ve geçici hataları nasıl işleneceğini? Bir hizmet farklı bir düğüme taşındığında ne?
* *Zaman aşımları ve gecikme süresi*. Çok uygulamalarda her hizmet katmanı gecikme yığın aracılığıyla ve kullanıcıya nasıl işler mi?

Service Fabric tarafından sağlanan yerleşik hizmet iletişimi bileşenleri birini kullanın ya da kendi hizmetlerinizi arasındaki etkileşimler sınama yapı uygulamanızda dayanıklılık sağlamak için önemlidir.

## <a name="prepare-for-services-to-move"></a>Taşıma hizmetleri için hazırlama
Hizmet örnekleri zaman içinde hareket etme. Bu, özellikle özel uyarlanmış en iyi kaynak Dengeleme için yük ölçümlerle yapılandırıldığında geçerlidir. Service Fabric yükseltmelerinin, yük devretme, genişleme ve dağıtılmış bir sistemde ömrü boyunca gerçekleşen diğer durumlarda sırasında bile bunların kullanılabilirliğini en üst düzeye çıkarmak, hizmet örnekleri taşır.

Kümedeki hizmetleri taşımak gibi istemcilerinizi ve diğer hizmetleri bir hizmete konuşurken iki senaryo işlemek için hazırlanması gerekir:

* Hizmet örneği veya bölüm çoğaltma için açıklandı en son ne zaman bu yana taşınmıştır. Bu hizmet yaşam döngüsü normal bir parçası olan ve uygulamanızın ömrü boyunca gerçekleşmesi beklenmelidir.
* Hizmet örneği veya bölüm çoğaltma geçme işleminde özelliğidir. Bir hizmetin bir düğümden diğerine yük devretme Service Fabric çok hızlı bir şekilde oluşur ancak bir gecikme olabilir kullanılabilirlik hizmetinizin iletişim bileşeni başlatmak yavaş ise.

Bu senaryolar düzgün biçimde işleme düzgün çalışmasını sistemi için önemlidir. Bunu yapmak için unutmayın:

* Her hizmet için bağlı olan bir *adresi* , (örneğin, HTTP veya WebSockets) dinler. Bir hizmet örneği veya bölüm taşındığında, adresi uç noktasında değiştirir. (Bunu farklı bir IP adresi ile farklı bir düğüme taşınır.) Yerleşik iletişim bileşenleri kullanıyorsanız, bunlar yeniden çözümleme hizmeti adresleri sizin için işler.
* Olabilir hizmet gecikme hizmet örneği başlatır, dinleyicisi yukarı olarak geçici bir artış yeniden. Bu hizmet örneği taşındıktan sonra ne kadar hızlı hizmet dinleyici üzerinde bağlıdır.
* Var olan tüm bağlantıları kapatılıp yeniden açılmasını yeni bir düğüm üzerinde hizmet açıldıktan sonra gerekir. Normal düğümün kapanması veya yeniden başlatma düzgün biçimde kapatılması varolan bağlantılar için zaman sağlar.

### <a name="test-it-move-service-instances"></a>Test: taşıma hizmet örnekleri
Service Fabric'ın Test Edilebilirlik araçlarını kullanarak, bu gibi durumlarda farklı şekillerde test etmek için bir test senaryosu yazabilirsiniz:

1. Bir durum bilgisi olan hizmetin birincil çoğaltma taşıyın.
   
    Bir durum bilgisi olan hizmet bölüm birincil çoğaltmasını birkaç nedenden dolayı için taşınabilir. Bu, hizmetlerinizi taşımak için çok denetimli bir şekilde nasıl tepki görmek için belirli bir bölüm birincil çoğaltmasını hedeflemek için kullanın.
   
    ```powershell
   
    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService
   
    ```
2. Bir düğüm durdurun.
   
    Bir düğüm durdurulduğunda, Service Fabric tüm hizmet örneği ya da bu düğümde kümedeki kullanılabilir diğer düğümlerden biri olan bölümleri taşır. Burada kümenizi ve tüm hizmet örneklerinin bir düğüm kaybolur ve çoğaltmaları bu düğüme taşımak sahip bir durum sınamak için bunu kullanın.
   
    PowerShell kullanarak bir düğümü durdurabilirsiniz **Stop-ServiceFabricNode** cmdlet:
   
    ```powershell
   
    PS > Stop-ServiceFabricNode -NodeName Node_1
   
    ```

## <a name="maintain-service-availability"></a>Hizmet kullanılabilirliği sürdürmek
Bir platform Service Fabric, hizmetlerin yüksek kullanılabilirlik sağlamak için tasarlanmıştır. Ancak olağanüstü durumlarda, temel alınan altyapı hala kullanılamazlık sorunlara yol açabilir. Bu senaryolar için çok sınamak önemlidir.

Durum bilgisi olan hizmetler çekirdek tabanlı bir sistem durumu yüksek kullanılabilirlik için çoğaltmak için kullanın. Bu çekirdek çoğaltmalarının yazma işlemleri gerçekleştirmek kullanılabilir olması gerektiği anlamına gelir. Nadir durumlarda, yaygın donanım arızası gibi bir çekirdek çoğaltmalarının kullanılamayabilir. Bu durumda, yazma işlemleri gerçekleştirmek mümkün olmaz ancak hala okuma işlemleri açamaz.

### <a name="test-it-write-operation-unavailability"></a>Test: yazma işlemi kullanılamazlık
Service Fabric Test Edilebilirlik araçlarını kullanarak, bir test olarak çekirdek kayıp gerektiriyorsa bir arıza ekleyemezsiniz. Böyle bir senaryo ender olsa da, istemciler ve durum bilgisi olan bir hizmete bağlı hizmetler bunlar yazma isteklerine burada yapamazsınız durumları işlemek için hazırlanan önemlidir. Durum bilgisi olan hizmet bu olasılığını bilmektedir ve düzgün biçimde arayanlara iletişim kurabildiğini önemlidir.

Çekirdek kayıp PowerShell kullanarak anlamına **Invoke-ServiceFabricPartitionQuorumLoss** cmdlet:

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

Bu örnekte, ayarlarız `QuorumLossMode` için `QuorumReplicas` tüm çoğaltmaları bırakmadan çekirdek kayıp anlamına istediğimizi belirtmek için. Bu şekilde, okuma işlemleri hala mümkündür. Bölümünün tamamını nerede kullanılamıyorsa bir senaryoyu test etmek için bu anahtarı ayarlayabilirsiniz `AllReplicas`.

## <a name="next-steps"></a>Sonraki adımlar
[Test Edilebilirlik eylemler hakkında daha fazla bilgi edinin](service-fabric-testability-actions.md)

[Test Edilebilirlik senaryoları hakkında daha fazla bilgi edinin](service-fabric-testability-scenarios.md)

