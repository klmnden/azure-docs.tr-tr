---
title: "Service Fabric Küme Kaynak Yöneticisi - Yönetim tümleştirme | Microsoft Docs"
description: "Küme Kaynak Yöneticisi ve hizmet doku Yönetimi arasında tümleştirme noktaları genel bakış."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 956cd0b8-b6e3-4436-a224-8766320e8cd7
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 70c0cc37a1d362c937ab86bd630c5ab051e63870
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="cluster-resource-manager-integration-with-service-fabric-cluster-management"></a>Service Fabric kümesi yönetimi ile Küme Kaynak Yöneticisi tümleştirme
Service Fabric kümesi Kaynak Yöneticisi Service Fabric yükseltmelerinin sürücü değil, ancak söz konusu. Küme Resource Manager ile yönetimi yardımcı olan ilk istenen durumu kümeyi ve hizmetlerin içindeki izleme tarafından yoludur. Küme Kaynak Yöneticisi'ni, küme istenen yapılandırma yerleştirin sistem durumu raporlarını gönderir. Örneğin, yeterli kapasitesi varsa küme Resource Manager sistem durumu uyarıları ve hataları sorunu belirten gönderir. Başka bir parçasını tümleştirme yükseltme nasıl çalışır ile ilgilidir. Küme Kaynak Yöneticisi'ni davranışını biraz yükseltmeler sırasında değiştirir.  

## <a name="health-integration"></a>Sistem durumu tümleştirme
Küme Kaynak Yöneticisi'ni hizmetlerinizi yerleştirmek için tanımladığınız kuralları sürekli olarak izler. Bu ayrıca kalan kapasite her ölçümü için düğümlerde ve küme ve kümedeki bir bütün olarak izler. Bu kurallar gerçekleştiremiyor veya yeterli kapasitesi varsa, sistem durumu uyarıları ve hataları gösterilen. Örneğin, bir düğüm üzerinde kapasite ise ve Hizmetleri taşıyarak durumu düzeltmek Küme Kaynak Yöneticisi'ni çalışacaktır. Durum düzeltemezsiniz, hangi düğümün hangi ölçümleri ve kapasite üzerinden olduğunu belirten bir sistem durumu uyarısı yayar.

Başka bir Resource Manager'ın sistem durumu uyarıları kısıtlamalarından ihlalleri örnektir. Örneğin, bir yerleştirme kısıtlaması tanımladıysanız (gibi `“NodeColor == Blue”`) ve Kaynak Yöneticisi'ni kısıtlamayı ihlal algılarsa, sistem durumu uyarısı yayar. Bu, özel kısıtlamalar ve varsayılan kısıtlamalar (gibi hata etki alanı ve yükseltme etki alanı kısıtlamaları) için geçerlidir.

Böyle bir durum raporu bir örneği burada verilmiştir. Bu durumda, sistem durumu raporu sistem hizmetin bölümleri için biridir. Bu bölüm kopyalarını geçici olarak çok az yükseltme etki alanlarına paketlenmiş sistem iletisi gösterir.

```posh
PS C:\Users\User > Get-WindowsFabricPartitionHealth -PartitionId '00000000-0000-0000-0000-000000000001'


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

İşte ne bu sistem durumu ileti bize olduğunu bildiriyor:

1. Tüm çoğaltmaların kendilerini sağlıklı: her AggregatedHealthState vardır: Tamam
2. Yükseltme etki alanı dağıtım kısıtlaması şu anda ihlal. Bu, belirli bir yükseltme etki alanı bu bölümü gerekenden daha fazla çoğaltmalardan olduğu anlamına gelir.
3. Hangi düğümün ihlali neden çoğaltma içerir. Bu durumda "Node.8" adını içeren düğüme olur
4. Bu bölüm için ("şu anda yükseltme--false") olup olmadığını yükseltme şu anda gerçekleştiriliyor
5. Bu hizmet için dağıtım İlkesi: "Dağıtım ilkesi--paketleme". Bu tabidir `RequireDomainDistribution` [yerleştirme İlkesi](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing). "Sevk" gösterir, bu durumda DomainDistribution olduğunu _değil_ yerleştirme İlkesi bu hizmet için belirtilmedi biliyoruz şekilde, gerekli. 
6. Ne zaman rapor oldu - 8/10/2015 19:13:02: 00

Bir şeyler yanlış geçti ve algılamak ve hatalı yükseltmeler durdurmak için de kullanılır size bildirmek üretimde yangın bu powers uyarıları gibi bilgiler. Bu durumda, biz biz neden Resource Manager yükseltme etki alanına çoğaltmaları paketi gerekiyordu çıkışı şekil olmadığını görmek istersiniz. Diğer yükseltme etki alanları içindeki düğümlerin aşağı, örneğin olduğundan genellikle sevk geçicidir.

Diyelim ki bazı hizmetler yerleştirmek küme kaynak yönetici çalışıyor, ancak iş çözümleri yok. Hizmetleri yerleştirilemez genellikle aşağıdaki nedenlerden birinden dolayı olur:

1. Bazı geçici koşul bu hizmet örneği veya çoğaltma doğru yerleştirmek imkansız yaptı
2. Hizmetin yerleştirme unsatisfiable gereksinimleridir.

Bu durumlarda, sistem durumu raporlarının Küme Kaynak Yöneticisi'nden service neden yerleştirilemez belirlemenize yardımcı. Bu işlem kısıtlaması eleme dizisi diyoruz. Bunu sırasında sistem ne ortadan kayıtları ve hizmet etkileyen yapılandırılmış kısıtlamaları anlatılmaktadır. Hizmetleri yerleştirilmesi için değil, bu şekilde, hangi düğümlerin ortadan görebilirsiniz ve neden.

## <a name="constraint-types"></a>Kısıtlama türleri
Bu sistem durumu raporları farklı kısıtlamalar her hakkında şimdi konuşun. Çoğaltmaları yerleştirildiğinde bu kısıtlamaların ilgili durum iletilerini görürsünüz.

* **ReplicaExclusionStatic** ve **ReplicaExclusionDynamic**: Bu kısıtlamaların gösteren bir çözüm aynı bölüm iki hizmet nesneleri aynı düğümde yerleştirilmesi zorunda olduğu için reddedildi. Daha sonra bu düğümü aşırı o bölümün etkileyebilecek bu için izin verilmiyor. ReplicaExclusionStatic ve ReplicaExclusionDynamic neredeyse aynı kural ve farkları gerçekten önemi yoktur. ReplicaExclusionStatic veya ReplicaExclusionDynamic kısıtlaması içeren bir kısıtlama eleme sırası görüyorsanız, Küme Kaynak Yöneticisi'ni yeterli düğüm olmayan düşünmektedir. Bu izin verilmiyor bu geçersiz yerleşimi kullanmak için kalan çözümleri gerektirir. Dizisindeki diğer kısıtlamaları genellikle bize neden düğümleri ilk başta ortadan söyler.
* **PlacementConstraint**: Bu iletiyi görürseniz, bu hizmetin kısıtlamalarından eşleşmedi çünkü biz bazı düğümler ortadan anlamına gelir. Biz bu iletiyi bir parçası olarak yapılandırılmış kısıtlamalarından izleme. Tanımlanan bir yerleştirme kısıtlaması varsa, bu normaldir. Ancak, yerleştirme kısıtlaması ortadan için çok fazla düğüm yanlış neden olup olmadığını nasıl fark etmesi budur.
* **NodeCapacity**: Bu kısıtlama, bunları kapasite aşımı taşmasına neden olabileceğinden Küme Kaynak Yöneticisi'ni çoğaltmaları belirtilen düğümlerinde yerleştirmenizi uygulanamadı anlamına gelir.
* **Benzeşim**: benzeşim kısıtlamayı ihlal neden olacağından bu yana biz çoğaltma etkilenen düğümlerinde yerleştirmenizi uygulanamadı bu kısıtlamayı belirtir. Benzeşimi hakkında daha fazla bilgi yer [bu makalede](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
* **FaultDomain** ve **UpgradeDomain**: Belirtilen düğümlerinde yineleme yerleştirme bir belirli hatası veya yükseltme etki alanında sevk neden olacaksa bu kısıtlamayı düğümleri ortadan kaldırır. Bu sınırlama ele çeşitli örnekler üzerinde konusunda sunulan [hata ve yükseltme etki alanı kısıtlamaları ve bunun sonucunda oluşan davranışı](service-fabric-cluster-resource-manager-cluster-description.md)
* **PreferredLocation**: varsayılan olarak bir iyileştirme çalışır olduğundan düğüm çözümden kaldırmak bu kısıtlamayı normalde görmemesi. Tercih edilen konum kısıtlaması, yükseltmeler sırasında da mevcuttur. Yükseltme sırasında yükseltme başlatıldığında geri burada oldukları için Hizmetleri taşımak için kullanılır.

## <a name="blocklisting-nodes"></a>Blocklisting düğümler
Başka bir sistem durumu Küme Kaynak Yöneticisi raporları iletisidir düğümleri blocklisted olduğunda. Blocklisting sizin için otomatik olarak uygulanan geçici bir kısıtlama olarak düşünebilirsiniz. Yinelenen hatalarının bu hizmet türünün örneklerini başlatılırken karşılaşırsanız, düğümleri blocklisted alın. Düğüm başına-service-type olarak blocklisted ' dir. Bir düğüm için bir hizmet türünün blocklisted olabilir, ancak başka bir değil. 

Genellikle geliştirme sırasında ilkenin etkisini gösterip blocklisting görürsünüz: başlangıçta çökmesine hizmet ana bilgisayarı bazı hata neden olur. Service Fabric hizmeti ana bilgisayarı birkaç kez oluşturmayı dener ve hata gerçekleşen tutar. Birkaç denemeden sonra blocklisted düğümünü alır ve başka bir hizmet oluşturmak Küme Kaynak Yöneticisi'ni çalışacaktır. Birden çok düğümde bu hata gerçekleştiği tutar, tüm küme sonlandırmayı geçerli düğüm engellenen mümkündür. Blocklisting ayrıca çok fazla sayıda düğüm yeterli başarıyla istenen ölçeği karşılamak üzere hizmetini başlatabilirsiniz kaldırabilirsiniz. Genellikle, ek hatalar veya uyarılar küme kaynak hizmet istenen çoğaltma veya örnek sayısı yanı sıra neyin başarısız olduğunu gösteren durum iletileri olduğunu belirten, blocklisting ilk başta baştaki Yöneticisi'nden de görürsünüz.

Blocklisting kalıcı bir durum değil. Birkaç dakika sonra engelleme düğüm kaldırılır ve Service Fabric Hizmetleri bu düğümde yeniden etkinleştirebilirsiniz. Hizmetleri başarısız olmaya devam ederse, bu hizmet türünün blocklisted yeniden düğümdür. 

### <a name="constraint-priorities"></a>Kısıtlama öncelikler

> [!WARNING]
> Kısıtlama önceliklerini değiştirmek önerilmez ve kümenizi önemli olumsuz etkileri olabilir. Varsayılan kısıtlama önceliklerini ve davranışlarını başvurusunu altındaki bilgi sağlanır. 
>

Tüm bu kısıtlamaların, "Merhaba – ı hata etki alanı kısıtlamaları my sisteminde en önemli şey olduğunu düşünün. düşünüyorum Hata etki alanı kısıtlaması ihlal edildi olmadığından emin olmak için diğer kısıtlamaları ihlal istiyorum."

Kısıtlamaları farklı öncelik düzeyleri ile yapılandırılabilir. Bunlar:

   - "sabit" (0)
   - "yumuşak" (1)
   - "en iyi duruma getirme" (2)
   - (-1 "kapalı"). 
   
Kısıtlamaların büyük bir bölümü sabit kısıtlamaları varsayılan olarak yapılandırılır.

Kısıtlamaları önceliğini değiştirmek seyrek olur. Burada kısıtlaması öncelikleri, genellikle bazı diğer hata veya ortam etkileyen davranış çalışacak şekilde değiştirmek için gerekli kez olmuştur. Genellikle kısıtlaması öncelik altyapı esnekliğini çok iyi çalışmıştır ancak genellikle gerekli değildir. Çoğu zaman, her şeyi varsayılan öncelikleri bulunur. 

Öncelik düzeyleri belirli bir kısıtlama anlamına yok _olacak_ ihlal, ya da her zaman karşılanması. Kısıtlama öncelikleri kısıtlamaları zorunlu tutulmaz sipariş tanımlayın. Tüm kısıtlamaları karşılamak mümkün olduğunda öncelikleri dengelemeden tanımlayın. Genellikle olmadıkça ortamda geçmeden başka bir şey tüm kısıtlamalar karşılanabilir. Çakışan kısıtlamaları için bir kısıtlama ihlali götürür senaryoları bazı örnekleri şunlardır ya da çok sayıda eş zamanlı hataları.

Gelişmiş durumlarda kısıtlaması önceliklerini değiştirebilirsiniz. Örneğin, benzeşimini her zaman düğüm kapasitesi sorunları çözmek için gerekli olduğunda ihlal edilebilir emin olmak istediğinizi varsayalım. Bunun için "yumuşak" (1) için benzeşim kısıtlaması önceliğini ayarlama ve "sabit" için ayarlanan kapasite kısıtlamasına (0) bırakın.

Farklı kısıtlamaları varsayılan öncelik değerleri aşağıdaki yapılandırma dosyasında belirtilir:

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

tek başına dağıtımlarında ClusterConfig.json ya da Azure için Template.json aracılığıyla kümeleri barındırılan:

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
Küme Kaynak Yöneticisi'ni hata ve yükseltme etki alanları arasında dağılmış Hizmetleri tutmak istiyor. Bu küme Resource Manager'ın altyapısı içinde bir kısıtlama olarak modeller. Bunların nasıl kullanıldığı hakkında daha fazla bilgi ve belirli davranışlarını için makalesine kontrol [küme yapılandırması](service-fabric-cluster-resource-manager-cluster-description.md#fault-and-upgrade-domain-constraints-and-resulting-behavior).

Küme Kaynak Yöneticisi'ni bir yükseltme etki alanına paketini yükseltmeler, hataları veya başka bir kısıtlama ihlali ile mücadele etmek için birkaç çoğaltmaları gerekebilir. Yalnızca birkaç hataları veya diğer karmaşası doğru yerleştirme önleme sistemde hatası veya yükseltme etki alanlarına normalde sevk olur. Bu durumlarda sırasında bile paketleme önlemek isterseniz, kullanabileceği `RequireDomainDistribution` [yerleştirme İlkesi](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing). Bu hizmet kullanılabilirliği ve güvenilirliği bir yan etkisi olarak etkiler, bu nedenle dikkatlice düşünün dikkat edin.

Ortam doğru şekilde yapılandırıldıysa, tüm kısıtlamalarını tam olarak, bile yükseltmeler sırasında kullanılır. Küme Kaynak Yöneticisi için kısıtlamaları izliyor, anahtar şeydir. Bir ihlali algıladığında hemen rapor ve sorunu gidermek çalışır.

## <a name="the-preferred-location-constraint"></a>Tercih edilen konum kısıtlaması
İki farklı kullanımlar taşıdığından PreferredLocation kısıtlaması biraz farklıdır. Bir kısıtlamanın uygulama yükseltmeler sırasında kullanılır. Küme Kaynak Yöneticisi, bu kısıtlamayı yükseltmeler sırasında otomatik olarak yönetir. Yükseltme tamamlandığında çoğaltmaları ilk konumlarına geri dönüş emin olmak için kullanılır. Diğer PreferredLocation kısıtlaması için kullanımıdır [ `PreferredPrimaryDomain` yerleştirme İlkesi](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md). Bunların her ikisi de en iyi duruma getirme ve bu nedenle PreferredLocation kısıtlaması "En iyi duruma getirme için" varsayılan olarak ayarlanmış yalnızca kısıtlaması.

## <a name="upgrades"></a>Yükseltme
Küme Kaynak Yöneticisi'ni de uygulama ve hangi sırasında iki iş bulunmuyor Küme yükseltme sırasında yardımcı olur:

* Kural kümesinin tehlikeye emin olun
* sorunsuz yükseltme yardımcı olmak için deneyin

### <a name="keep-enforcing-the-rules"></a>Kuralları zorunlu tutun
Dikkat edilmesi gereken ana – katı kısıtlamaları kısıtlamalarından ve kapasiteleri gibi - kuralları yükseltmeler sırasında zorunlu tutulmaz şeydir. Kısıtlamalarından iş yüklerinizi yalnızca burada bunlar, hatta yükseltmeler sırasında izin verilen çalıştırdığınızdan emin olun. Hizmetler yüksek oranda kısıtlı kullanılırken yükseltmeler uzun sürebilir. Hizmet veya üzerinde çalıştığı düğüm için bir güncelleştirme yapıldığında, gidebilecekleri için birkaç seçenek olabilir.

### <a name="smart-replacements"></a>Akıllı değişiklik
Yükseltme başladığında, kaynak yöneticisi kümenin geçerli düzenleme, bir anlık görüntüsünü alır. Her yükseltme etki alanı tamamladıkça kendi özgün düzenleme, yükseltme etki alanında olan hizmetleri döndürmeye çalışır. Bu şekilde var. en çok bir hizmet için iki geçişleri yükseltme sırasında Etkilenen düğümünün çıkışı bir taşıma yoktur ve bir geri içeri taşı. Yükseltmeden önce nasıl olduğu için Küme veya hizmetin döndürme, aynı zamanda yükseltme kümenin düzeni etkilemez sağlar. 

### <a name="reduced-churn"></a>Azaltılmış karmaşası
Yükseltme sırasında gerçekleşen başka bir şey Dengeleme kapalı Küme Kaynak Yöneticisi'ni açar olmasıdır. Dengeleme önleme Hizmetleri yükseltme için Boşaltılan düğümleri taşınmasını gibi yükseltme kendisi için gereksiz tepki engeller. Küme Yükseltme yükseltme söz konusu ise, tüm küme yükseltme sırasında dengelenir değil. Kısıtlama denetimleri etkin kalır, yalnızca taşıma öngörülü Dengeleme ölçümlerini üzerinde temel devre dışı bırakıldı.

### <a name="buffered-capacity--upgrade"></a>Arabelleğe alınan kapasite & yükseltme
Genellikle küme kısıtlı olsa bile veya tam yakın tamamlamak için yükseltme istersiniz. Küme kapasitesini yönetme yükseltmeler sırasında normalden daha da önemlidir. Yükseltme küme boyunca yapar gibi yükseltme etki alanlarının sayısına bağlı olarak, 5 ve kapasite yüzde 20 arasında geçirilmesi gerekir. Başka bir yere gitmek bu iş vardır. Bu yerdir kavramı [kapasiteleri arabelleğe](service-fabric-cluster-resource-manager-cluster-description.md#buffered-capacity) yararlıdır. Arabelleğe alınan kapasite normal işlem sırasında dikkate. Küme Kaynak Yöneticisi'ni (arabellek tüketen) kapasitelerine toplam en fazla düğüm gerekirse, yükseltmeler sırasında doldurabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* En baştan başlatın ve [bir giriş için Service Fabric kümesi Resource Manager Al](service-fabric-cluster-resource-manager-introduction.md)
