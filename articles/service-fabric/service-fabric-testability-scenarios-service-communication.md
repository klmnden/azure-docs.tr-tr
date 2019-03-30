---
title: 'Test Edilebilirlik: Hizmet iletişimi | Microsoft Docs'
description: Hizmetten hizmete iletişimi, bir Service Fabric uygulamasının bir kritik tümleştirme noktasıdır. Bu makalede, tasarım konuları ve test teknikleri açıklar.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 017557df-fb59-4e4a-a65d-2732f29255b8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 529c8d74b6e0a63a7969f31d5b5e8073ecb79411
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58665755"
---
# <a name="service-fabric-testability-scenarios-service-communication"></a>Service Fabric Test Edilebilirlik senaryoları: Hizmet iletişimleri
Mikro hizmetler ve hizmet odaklı mimari stilleri yüzeyini doğal olarak Azure Service Fabric. Bu dağıtılmış mimariler türlerinde bileşenlerden mikro hizmet uygulamaları genellikle birbirine konuşmak için gereken birden çok hizmet oluşur. En basit durumlarda bile, genellikle en az bir durum bilgisi olmayan web hizmeti ve iletişim kurması gereken bir durum bilgisi olan veri depolama hizmeti vardır.

Her hizmetin diğer hizmetlere yönelik bir uzak API'ye kullanıma sunduğundan hizmetten hizmete iletişimi bir uygulamanın kritik tümleştirme noktasıdır. Genel olarak g/ç içeren bir dizi API sınırları ile çalışma bazı dikkatli bir iyi miktarda sınama ve doğrulama gerektirir.

Bu hizmet sınırları birlikte dağıtılmış bir sistemde kablolu alırken vermeniz gereken çok sayıda önemli noktalar vardır:

* *Aktarım Protokolü*. En fazla aktarım hızı için HTTP artan birlikte çalışabilirliğini ya da özel ikili protokolü kullanacak mısınız?
* *Hata işleme*. Kalıcı ve geçici hataları nasıl işleneceğini? Bir hizmet farklı bir düğüme hareket ettiğinde ne?
* *Zaman aşımları ve gecikme süresi*. Çok verili uygulamalarda, gecikme süresi yığını aracılığıyla ve kullanıcıyı her hizmet katmanı nasıl işleyeceğini?

Service Fabric tarafından sağlanan yerleşik hizmet iletişimi bileşenlerini birini kullanın veya kendi hizmetlerinizi arasındaki etkileşimler test derleme uygulamanızdaki dayanıklılık sağlamak için önemlidir.

## <a name="prepare-for-services-to-move"></a>Hizmetleri taşımak için hazırlama
Hizmet örnekleri zaman içinde hareket ettirin. Özel olarak tasarlanmış en iyi kaynak Dengeleme için yükleme ölçümleri ile yapılandırıldığında bu özellikle doğrudur. Service Fabric service örneklerinizi yükseltmeleri, yük devretme işlemleri, Ölçek genişletme ve dağıtılmış bir sistemin ömrü boyunca gerçekleşen diğer durumlarda sırasında bile, kullanılabilirliği en üst düzeye taşır.

Hizmetleri kümedeki yerleri gibi istemcilerinizi ve diğer hizmetlere bunlar bir hizmetinizle iletişim kurmasına, iki senaryo işlemeye hazırlıklı olmalıdır:

* Hizmet örneği veya bölüm çoğaltma için açıklandı en son ne zaman beri taşınmıştır. Bu hizmet yaşam döngüsü normal bir parçası olan ve uygulama ömrü boyunca gerçekleşmesi beklenmelidir.
* Hizmet örneği veya bölüm geçme işleminde yinelemedir. Bir hizmetin bir düğümden diğerine yük devretme Service Fabric'te çok hızlı bir şekilde meydana gelse bir gecikme olabilir kullanılabilirlik hizmetinizin iletişim bileşeni başlatmak yavaş ise.

Bu senaryolar düzgün bir şekilde işlemek için düzgün çalışmasını sistemi önemlidir. Bunu yapmak için unutmayın:

* Her hizmet için bağlı olan bir *adresi* , (örneğin, HTTP veya WebSockets) dinler. Bir hizmet örneği veya bölüm hareket ettirdiğinde adresi bitim değiştirir. (Bu farklı bir IP adresi ile farklı bir düğüme taşınır.) Yerleşik iletişim bileşenleri kullanıyorsanız, bunlar yeniden çözümleme hizmeti adresleri sizin için işler.
* Olabilir hizmet örneği başlatır, dinleyici yukarı hizmet gecikme süresine geçici bir artış yeniden. Bu hizmet örneği taşındıktan sonra ne kadar hızlı hizmet dinleyici açılır bağlıdır.
* Herhangi bir mevcut bağlantı kapatılıp yeni bir düğüm üzerinde hizmet açıldıktan sonra gerekir. Normal bir düğümün kapanması veya yeniden başlatma düzgün biçimde kapatılması varolan bağlantılar için zaman verir.

### <a name="test-it-move-service-instances"></a>Test: Hizmet örnekleri Taşı
Service Fabric'in Test Edilebilirlik araçlarını kullanarak, bu durumlar farklı şekilde test etmek için bir test senaryosu yazabilirsiniz:

1. Bir durum bilgisi olan hizmetin birincil çoğaltma taşıyın.
   
    Durum bilgisi olan hizmet bölüm birincil çoğaltması, birkaç nedenden için taşınabilir. Bu, hizmetlerinizi taşımak için oldukça denetimli bir biçimde nasıl tepki görmek için belirli bir bölüme birincil çoğaltmasını hedeflemek için kullanın.
   
    ```powershell
   
    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService
   
    ```
2. Bir düğümü durdurun.
   
    Bir düğüm durdurulduğunda, Service Fabric hizmet örneği veya o düğümle kümedeki kullanılabilir diğer düğümlerden birine olan bölümleri tümünün taşır. Bir durum burada kümenizi ve tüm hizmet örneklerinin bir düğüm kaybolur ve çoğaltmaları bu düğüme taşımak zorunda test etmek için bunu kullanın.
   
    PowerShell kullanarak bir düğümü durdurabilirsiniz **Stop-ServiceFabricNode** cmdlet:
   
    ```powershell
   
    PS > Stop-ServiceFabricNode -NodeName Node_1
   
    ```

## <a name="maintain-service-availability"></a>Hizmet kullanılabilirliği sürdürmek
Bir platform, Service Fabric, hizmetlerinizi yüksek kullanılabilirlik sağlamak için tasarlanmıştır. Ancak olağanüstü durumlarda, temel alınan altyapı yine de kullanım dışı kalması sorunlara yol açabilir. Bu senaryolar için de test önemlidir.

Durum bilgisi olan hizmetler çekirdek tabanlı bir sistem durumu yüksek kullanılabilirlik için çoğaltma için kullanın. Bu, bir çekirdeği yazma işlemleri gerçekleştirmek kullanılabilir olması gerektiğini anlamına gelir. Yaygın bir donanım hatası gibi nadir durumlarda bir çekirdeği kullanılamıyor olabilir. Bu gibi durumlarda, yazma işlemleri gerçekleştirmek mümkün olmayacaktır, ancak yine de okuma işlemleri gerçekleştirmek mümkün olacaktır.

### <a name="test-it-write-operation-unavailability"></a>Test: Yazma işlemi olarak kullanım dışı kalması
Service Fabric'te Test Edilebilirlik Araçları'nı kullanarak bir test olarak çekirdek kayıp sevk eden bir hata ekleyebilir. Bu tür bir senaryonun ender olsa da, istemciler ve durum bilgisi olan bir hizmete bağlı hizmetler nerede bunlar yazma istekleri yapamaz durumları işlemek için hazır olduğunu önemlidir. Durum bilgisi olan hizmet bu olasılığını bilmektedir ve arayanlara düzgün bir şekilde iletişim kurmak önemlidir.

PowerShell kullanarak çekirdek kayıp zorlarsınız **Invoke-ServiceFabricPartitionQuorumLoss** cmdlet:

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

Bu örnekte, ayarladığımız `QuorumLossMode` için `QuorumReplicas` tüm çoğaltmaları duruma geçirmek zorunda kalmadan çekirdek kayıp anlamına istediğimizi belirtmek için. Bu şekilde, okuma işlemleri hala yapılabilir. Bölüm kullanılamaz olduğu bir senaryoyu test etmek için bu anahtarı ayarlayabilirsiniz `AllReplicas`.

## <a name="next-steps"></a>Sonraki adımlar
[Test Edilebilirlik eylemleri hakkında daha fazla bilgi edinin](service-fabric-testability-actions.md)

[Test Edilebilirlik senaryoları hakkında daha fazla bilgi edinin](service-fabric-testability-scenarios.md)

