---
title: Service Fabric Küme Kaynak Yöneticisi - Yönetim tümleştirme | Microsoft Docs
description: Cluster Resource Manager ve Service Fabric yönetim arasında tümleştirme noktaları genel bakış.
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: 956cd0b8-b6e3-4436-a224-8766320e8cd7
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: c201945e94474d54b8a19918f3b55a0b40995a97
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670328"
---
# <a name="cluster-resource-manager-integration-with-service-fabric-cluster-management"></a>Service Fabric küme yönetimi ile Küme Kaynak Yöneticisi tümleştirme
Service Fabric Küme Kaynak Yöneticisi, Service Fabric'te yükseltmeleri sürücü değil, ancak söz konusu. Cluster Resource Manager ile yönetimi yardımcı olan ilk kümeyi ve içindeki Hizmetleri istenen durumunu izleyerek yoludur. İstenen yapılandırma ile küme konulamıyor olduğunda küme kaynak yöneticisi sistem durumu raporlarını gönderir. Örneğin, yeterli kapasite yoksa küme kaynak yöneticisi sistem durumu uyarıları ve hataları ilgili bir sorunu belirten out gönderir. Yükseltmeler nasıl ile yapmak başka bir tümleştirme vardır. Küme Kaynak Yöneticisi davranışını biraz yükseltmeler sırasında değiştirir.  

## <a name="health-integration"></a>Sistem durumu tümleştirme
Küme Kaynak Yöneticisi, hizmetlerinizi yerleştirmek için tanımlanan kuralların sürekli olarak izler. Bu da kalan kapasite her ölçüm için düğümlerde ve küme ve kümedeki bir bütün olarak izler. Bu kurallar gerçekleştiremiyor veya yeterli kapasite yoksa, sistem durumu uyarıları ve hataları gönderilir. Örneğin, bir düğüm kapasitesinden fazla olduğunda gerçekleşir ve Küme Kaynak Yöneticisi Hizmetleri taşıyarak bu durumu düzeltmek çalışacaktır. Durum düzeltemezsiniz değilse hangi ölçümlerin yanı sıra, kapasite üzerinden hangi düğümün olduğunu gösteren bir sistem durumu uyarısı gösterir.

Başka bir Resource Manager'ın sistem durumu uyarıları yerleştirme kısıtlamaları ihlal örneğidir. Örneğin, bir yerleşim kısıtlaması tanımladıysanız (gibi `“NodeColor == Blue”`) ve Resource Manager kısıtlamayı ihlal algılar, bir sistem durumu uyarı verir. Bu, özel kısıtlamalar ve varsayılan kısıtlamalar (örneğin, hata etki alanı ve yükseltme etki alanı kısıtlamaları) için geçerlidir.

Böyle bir durum raporu örneği aşağıda verilmiştir. Bu durumda, sistem durumu raporu, sistem hizmetin bölümleri için biridir. Bu bölüm çoğaltmalarını çok az sayıda yükseltme etki alanlarına geçici olarak paketlenir sistem durumu ileti gösterir.

```posh
PS C:\Users\User > Get-ServiceFabricPartitionHealth -PartitionId '00000000-0000-0000-0000-000000000001'


PartitionId           : 00000000-0000-0000-0000-000000000001
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB', Property='ReplicaConstraintViolation_UpgradeDomain', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 130766528804733380
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577821
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528854889931
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577822
                        AggregatedHealthState : Ok

                        ReplicaId             : 130837073190680024
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.PLB
                        Property              : ReplicaConstraintViolation_UpgradeDomain
                        HealthState           : Warning
                        SequenceNumber        : 130837100116930204
                        SentAt                : 8/10/2015 7:53:31 PM
                        ReceivedAt            : 8/10/2015 7:53:33 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer has detected a Constraint Violation for this Replica: fabric:/System/FailoverManagerService Secondary Partition 00000000-0000-0000-0000-000000000001 is
                        violating the Constraint: UpgradeDomain Details: UpgradeDomain ID -- 4, Replica on NodeName -- Node.8 Currently Upgrading -- false Distribution Policy -- Packing
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Ok->Warning = 8/10/2015 7:13:02 PM, LastError = 1/1/0001 12:00:00 AM
```

Ne bu durum iletisini bize olduğunu söylüyor aşağıda verilmiştir:

1. Tüm çoğaltmaların kendilerini sağlıklı şunlardır: Her AggregatedHealthState vardır: Tamam
2. Yükseltme etki alanı dağıtım kısıtlaması şu anda ihlal. Bu, belirli bir yükseltme etki alanı bu bölüm gerekenden daha fazla çoğaltmalardan sahip olduğu anlamına gelir.
3. Hangi düğümün ihlali neden çoğaltmayı içerir. Bu durumda, "Node.8" adını içeren düğüme olur.
4. Bu bölüm için ("şu anda yükseltme--false") mi yükseltme şu anda oluyor
5. Bu hizmet için dağıtım İlkesi: "Paket dağıtım İlkesi--". Bu tabidir `RequireDomainDistribution` [yerleştirme İlkesi](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing). "Paket" DomainDistribution bu durumda olup olmadığını gösteren _değil_ , biz bu hizmet için yerleştirme ilkesi belirtilmedi bilmeleri gerekmez. 
6. Ne zaman rapor oldu - 8/10/2015 19:13:02: 00

Bir sorun oluştu ve algılamak ve hatalı yükseltmeleri durdurmak için de kullanılır size bildirmek için üretim ortamında tetiklenen bu powers uyarılar gibi bilgileri. Bu durumda, biz biz neden Resource Manager çoğaltmaları, yükseltme etki alanına paketi gerekiyordu kullanıma şekil varsa öğrenmek istersiniz. Örneğin diğer yükseltme etki düğümler aşağı oldukları için genellikle paketleme geçici olduğu.

Diyelim ki Küme Kaynak Yöneticisi, bazı hizmetler yerleştirmek çalışıyor, ancak iş herhangi bir çözüm yok. Hizmetleri yerleştirilemez genellikle aşağıdaki nedenlerden biri olur:

1. Bazı geçici koşullar Bu hizmet örneği veya çoğaltma doğru yerleştirmek imkansız yaptı
2. Hizmet yerleştirme unsatisfiable gereksinimleridir.

Bu durumlarda, sistem durumu raporlarının Küme Kaynak Yöneticisi'nden hizmet neden yerleştirilemez belirlemenize yardımcı. Bu işlem kısıtlaması eleme dizisi diyoruz. Bunu sırasında sistem kayıtları ve hizmet ne ortadan etkileyen yapılandırılmış kısıtlamaları size yol gösterir. Hizmetleri yerleştirilmesi mümkün değildir, bu şekilde, hangi düğümleri ortadan görebilirsiniz ve neden.

## <a name="constraint-types"></a>Kısıtlama türleri
Her biri bu sistem durumu raporlarının farklı kısıtlamalar söz edelim. Çoğaltmaları yerleştirildiğinde bu kısıtlamalarıyla ilgili durum iletilerini görürsünüz.

* **ReplicaExclusionStatic** ve **ReplicaExclusionDynamic**: Bu kısıtlamaları bir çözüm, iki hizmet nesneleri aynı bölümden aynı düğüme yerleştirilmesine yeterli olacağından reddedildiğini gösterir. Bunun nedeni izin verilmiyor sonra o düğümde hata aşırı bölümü etkiler. ReplicaExclusionStatic ve ReplicaExclusionDynamic neredeyse aynı kural ve farkları gerçekten önemli değildir. ReplicaExclusionStatic veya ReplicaExclusionDynamic kısıtlaması içeren bir kısıtlama eleme dizisi görüyorsanız, Küme Kaynak Yöneticisi yeterli düğümleri olmayan düşünüyor. Bu izin verilmez, bu geçersiz yerleşimi kullanmak için geri kalan çözümleri gerektirir. Dizideki diğer kısıtlamaları genellikle bize neden düğümleri ilk başta gideriliyor söyleyecektir.
* **PlacementConstraint**: Bu iletiyi görürseniz, bu hizmetin yerleştirme kısıtlamaları ile eşleşmedi çünkü biz bazı düğümler ortadan anlamına gelir. Biz bu iletiyi bir parçası olarak yapılandırılmış yerleştirme kısıtlamaları izleme. Tanımlanan bir yerleşim kısıtlaması varsa, bu normaldir. Ancak, yerleştirme kısıtlaması ortadan çok fazla düğüm yanlış neden olup olmadığını nasıl fark etmesi budur.
* **NodeCapacity**: Bu kısıtlama, kapasite aşımı koyabilirsiniz çünkü Küme Kaynak Yöneticisi çoğaltmaları belirtilen düğümler üzerinde yerleştirmenizi uygulanamadı anlamına gelir.
* **Benzeşim**: Bu sınırlama, benzeşim kısıtlamanın ihlalini neden olduğundan şu çoğaltma etkilenen düğümlerinde yerleştirmenizi uygulanamadı gösterir. Benzeşimi hakkında daha fazla bilgi yer [bu makalede](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
* **FaultDomain** ve **UpgradeDomain**: Belirtilen düğümler üzerinde yineleme yerleştirme belirli bir hata veya yükseltme etki alanı içinde neden olacaksa, bu kısıtlama düğümleri ortadan kaldırır. Konu başlığı altında sunulur çeşitli örnekleri bu kısıtlamayı görüştükten [hata ve yükseltme etki alanı kısıtlamaları ve bunun sonucunda oluşan davranışı](service-fabric-cluster-resource-manager-cluster-description.md)
* **PreferredLocation**: Normalde, bu kısıtlama, varsayılan olarak bir iyileştirme çalıştığından, düğüm çözümden kaldırmak görmeyeceksiniz. Tercih edilen konum kısıtlaması, yükseltmeler sırasında da mevcuttur. Yükseltme sırasında yükseltme başlatıldığında geri nerede oldukları için Hizmetleri taşımak için kullanılır.

## <a name="blocklisting-nodes"></a>Blocklisting düğümleri
Başka bir sistem durumu iletisi Küme Kaynak Yöneticisi raporları olduğunda blocklisted düğümlerdir. Blocklisting sizin için otomatik olarak uygulanan geçici bir sınırlama olarak düşünebilirsiniz. Bunlar, hizmet türünün örneğini başlatırken yinelenen hatalar yaşansa düğümleri blocklisted alın. Düğüm başına-hizmet-türü temelinde blocklisted ' dir. Bir düğüm için bir hizmet türünün blocklisted olabilir, ancak başka bir değil. 

Geliştirme sırasında genellikle etkisini göstermeye blocklisting görürsünüz: bazı hata hizmet ana başlangıçta çökmesine neden neden olur. Service Fabric hizmet ana bilgisayarı birkaç kez oluşturmayı dener ve hatanın gerçekleşmesini tutar. Birkaç denemeden sonra blocklisted düğümünü alır ve Küme Kaynak Yöneticisi hizmeti başka bir yerde oluşturmayı dener. Birden çok düğümde bu hata gerçekleştiği tutar, tüm küme sonlandırmayı geçerli düğümlerin engellenen mümkündür. Blocklisting ayrıca çok fazla düğüm yeterli başarıyla istenen ölçeği karşılamak üzere hizmetini başlatabilirsiniz kaldırabilirsiniz. Ek hata veya istenen çoğaltma veya örnek sayısı, hem de ilk blocklisting için baştaki hata nedir gösteren durum iletilerinin hizmet aşağıdadır gösteren Küme Kaynak Yöneticisi'nden uyarı genellikle görürsünüz yerleştirin.

Blocklisting kalıcı bir durum değil. Birkaç dakika sonra düğüm engelleme listesi ' kaldırılır ve Service Fabric Hizmetleri bu düğümde yeniden etkinleştirebilirsiniz. Hizmetleri başarısız olmaya devam ederse, bu hizmet türünün blocklisted yeniden düğümüdür. 

### <a name="constraint-priorities"></a>Kısıtlama öncelikleri

> [!WARNING]
> Kısıtlama önceliklerini değiştirmek önerilmez ve kümenizde önemli olumsuz etkileri olabilir. Varsayılan kısıtlama öncelikleri ve bunların davranışlarını başvurusu için aşağıdaki bilgileri sağlanır. 
>

Tüm bu kısıtlamalar, "Hey – hata etki alanı kısıtlamaları sistemimin, en önemli şey olduğunu düşünüyorum. düşünmek Hata etki alanı kısıtlamasını ihlal olmadığından emin olmak için diğer kısıtlamaları ihlal etmek istiyorum."

Kısıtlamalar farklı öncelik düzeyleri ile yapılandırılabilir. Bunlar:

   - "sabit" (0)
   - “soft” (1)
   - “optimization” (2)
   - “off” (-1). 
   
Kısıtlamalar çoğu sabit kısıtlama olarak varsayılan olarak yapılandırılır.

Kısıtlama önceliğini değiştirme seyrek olur. Kısıtlama öncelikleri, genellikle bazı diğer hata veya ortam etkiliyordu davranışı geçici olarak çalışacak şekilde değiştirmek için gerekmesi kez olmuştur. Genel kısıtlama önceliğini altyapı esnekliği çok iyi çalıştı, ancak genellikle gerekli değildir. Çoğu zaman, her şeyi varsayılan öncelikleri bulunur. 

Öncelik düzeyleri, belirli bir kısıtlama gelmez _olacak_ ihlal, ya da her zaman karşılanması. Kısıtlama öncelikleri kısıtlamaları zorunlu bir sipariş tanımlayın. Tüm kısıtlamaları karşılamak mümkün olduğunda öncelikleri ödün tanımlayın. Genellikle tüm kısıtlamalar olmadığı sürece başka ortamlarında sürüp giden bir şeyler karşılanabilir. Bazı örnekler çakışan kısıtlamalarını sabiti ihlallerini önünü açacak senaryoları biri ya da çok sayıda eşzamanlı hataları.

Gelişmiş durumlarda kısıtlaması önceliklerini değiştirebilirsiniz. Örneğin, benzeşimini her zaman düğüm kapasite sorunlarını çözmek için gerekli olduğunda ihlal edilebilir emin olmak istediğinizi düşünelim. Bunu başarmak için "yumuşak" (1) için benzeşim kısıtlama önceliğini ayarlayın ve "sabit" için ayarlanmış kapasite kısıtlaması (0) bırakın.

Farklı kısıtlamaları için varsayılan öncelik değerleri aşağıdaki yapılandırmada belirtilir:

ClusterManifest.xml

```xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PlacementConstraintPriority" Value="0" />
            <Parameter Name="CapacityConstraintPriority" Value="0" />
            <Parameter Name="AffinityConstraintPriority" Value="0" />
            <Parameter Name="FaultDomainConstraintPriority" Value="0" />
            <Parameter Name="UpgradeDomainConstraintPriority" Value="1" />
            <Parameter Name="PreferredLocationConstraintPriority" Value="2" />
        </Section>
```

tek başına dağıtımlarında ClusterConfig.json veya Azure için Template.json aracılığıyla kümeleri barındırılan:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PlacementConstraintPriority",
          "value": "0"
      },
      {
          "name": "CapacityConstraintPriority",
          "value": "0"
      },
      {
          "name": "AffinityConstraintPriority",
          "value": "0"
      },
      {
          "name": "FaultDomainConstraintPriority",
          "value": "0"
      },
      {
          "name": "UpgradeDomainConstraintPriority",
          "value": "1"
      },
      {
          "name": "PreferredLocationConstraintPriority",
          "value": "2"
      }
    ]
  }
]
```

## <a name="fault-domain-and-upgrade-domain-constraints"></a>Hata etki alanı ve yükseltme etki alanı kısıtlamaları
Küme Kaynak Yöneticisi, hata ve yükseltme etki alanları arasında dağılmış Hizmetleri tutmak istiyor. Bu küme Kaynak Yöneticisi'nin altyapısının içinde bir kısıtlama olarak modeller. Bunların nasıl kullanıldığı hakkında daha fazla bilgi ve belirli davranışları için makalesine göz atın [küme yapılandırması](service-fabric-cluster-resource-manager-cluster-description.md#fault-and-upgrade-domain-constraints-and-resulting-behavior).

Küme Kaynak Yöneticisi, yükseltmeler, hataları veya diğer bir kısıtlama ihlali uğraşmak için bir yükseltme etki alanına birkaç yineleme paketi yüklemeniz gerekebilir. Yalnızca birkaç hataları veya diğer değişim sıklığı doğru yerleştirme önleme sistemde hatası veya yükseltme etki alanlarına normalde paketleme olur. Bu gibi durumlarda bile sırasında paketleme önlemek istiyorsanız, kullanabilir `RequireDomainDistribution` [yerleştirme İlkesi](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing). Bu hizmet kullanılabilirliği ve güvenilirliği bir yan etkisi olarak etkiler, bu nedenle dikkatlice düşünün dikkat edin.

Ortamı doğru şekilde yapılandırıldıysa, tüm kısıtlamalar tam olarak, hatta yükseltmeler sırasında dikkate alınır. Küme Kaynak Yöneticisi için kısıtlamaları izliyor, anahtar şeydir. Bir ihlali algıladığında hemen rapor ve sorunu gidermek çalışır.

## <a name="the-preferred-location-constraint"></a>Tercih edilen konum kısıtlaması
İki farklı kullanım içerdiğinden PreferredLocation kısıtlaması biraz farklıdır. Bir kısıtlamanın uygulama yükseltmeleri sırasında kullanılır. Küme Kaynak Yöneticisi, bu kısıtlama yükseltmeler sırasında otomatik olarak yönetir. Yükseltme tamamlandığında çoğaltmaları ilk konumlarına geri dönüş emin olmak için kullanılır. Diğer PreferredLocation kısıtlaması için kullanımıdır [ `PreferredPrimaryDomain` yerleştirme İlkesi](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md). Bunların her ikisi de en iyi duruma getirme ve bu nedenle PreferredLocation kısıtlaması tek kısıtlama "İyileştirme için" varsayılan olarak ayarlayın.

## <a name="upgrades"></a>Yükseltme
Küme Kaynak Yöneticisi ayrıca uygulama ve bu sırada iki işi vardır, Küme yükseltme sırasında yardımcı olur:

* Kural kümesinin tehlikeye emin olun.
* sorunsuz yükseltmeye yardımcı olmak için deneyin

### <a name="keep-enforcing-the-rules"></a>Kuralları zorunlu tutun
Dikkat edilmesi gereken ana – katı kısıtlamaları yerleştirme kısıtlamaları ve kapasiteler gibi - kuralları yükseltmeler sırasında zorunlu tutulmaz şeydir. Yerleştirme kısıtlamaları nerede bunlar, hatta yükseltmeler sırasında izin verilen iş yüklerinizi yalnızca çalıştırdığınızdan emin olun. Hizmetler yüksek oranda kısıtlanmış, yükseltmeleri uzun sürebilir. Hizmet veya üzerinde çalıştığı düğüm için bir güncelleştirme getirilir, burada gidebilirsiniz için birkaç seçenek olabilir.

### <a name="smart-replacements"></a>Akıllı değişiklik
Yükseltme başladığında, kaynak yöneticisi geçerli düzenleme kümenin bir anlık görüntüsünü alır. Her yükseltme etki alanı tamamladıkça, kendi özgün düzenleme, yükseltme etki alanında hizmetler döndürmeyi dener. Bu şekilde var. en fazla iki geçişi bir hizmet için yükseltme sırasında Birini Taşı geri ve etkilenen düğümünün dışarı bir taşıma yoktur. Yükseltmeden önce nasıl olduğu için Küme veya hizmetin döndüren ayrıca yükseltme kümenin düzeni etkilemez sağlar. 

### <a name="reduced-churn"></a>Azaltılmış karmaşıklık
Yükseltmeler sırasında gerçekleşen başka bir şey Dengeleme kapalı Küme Kaynak Yöneticisi'ni açar olmasıdır. Dengeleme önleme gereksiz tepkiler gibi hizmetleri yükseltme için Boşaltılan düğümleri taşınmasını yükseltme kendisini engeller. Bir küme yükseltmesi yükseltme söz konusu ise, tüm küme yükseltme sırasında dengelenir değil. Kısıtlama denetimleri etkin kalır, tek taşıma hakkında proaktif Dengeleme ölçümlerini temel devre dışı bırakıldı.

### <a name="buffered-capacity--upgrade"></a>Arabelleğe alınan kapasite & yükseltme
Genellikle, küme kısıtlı olsa bile veya tam yakın tamamlanması için istediğiniz. Küme kapasitesi yönetme yükseltmeler sırasında normalden daha da önemlidir. Yükseltme dağıtılırken aracılığıyla küme olarak yükseltme etki alanlarının sayısına bağlı olarak, kapasite yüzde 20'si ile 5 arasında geçirilmesi gerekir. Bu iş, bir yere gitmek vardır. Burada kavramı [kapasiteler arabelleğe](service-fabric-cluster-resource-manager-cluster-description.md#buffered-capacity) yararlıdır. Normal işlem sırasında arabelleğe alınan kapasite uyulduğundan. Küme Kaynak Yöneticisi (arabellek kullanan), toplam kapasite en fazla düğüm gerekirse yükseltmeler sırasında doldurabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* En baştan başlatmak ve [için Service Fabric Küme Kaynak Yöneticisi giriş yapın](service-fabric-cluster-resource-manager-introduction.md)
