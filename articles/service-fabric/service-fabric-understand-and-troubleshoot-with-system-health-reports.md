---
title: "Sistem durumu raporları ile ilgili sorunları giderme | Microsoft Docs"
description: "Azure Service Fabric bileşenleri ve bunların kullanım tarafından gönderilen sorun giderme küme veya uygulama sorunları için sistem durumu raporları açıklar"
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 52574ea7-eb37-47e0-a20a-101539177625
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: oanapl
ms.openlocfilehash: b02b1260cedcade9bf69a99453ab0f5aa2c3c7b1
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="use-system-health-reports-to-troubleshoot"></a>Sorun gidermek için sistem durum raporlarını kullanma
Azure Service Fabric bileşenleri kutunun sağ dışında kümedeki tüm varlıklar üzerinde sistem durumu raporları sağlar. [Sistem durumu deposu](service-fabric-health-introduction.md#health-store) oluşturur ve sistem raporlarına dayalı varlıklar siler. Bu da onları varlık etkileşimleri yakalayan bir hiyerarşide düzenler.

> [!NOTE]
> Sistem durumu ile ilgili kavramları anlamanız için hakkında daha fazla okuma [Service Fabric sistem durumu modeli](service-fabric-health-introduction.md).
> 
> 

Sistem durumu raporlarını küme ve uygulama işlevselliğini ve bayrağı sorunları görünürlük sağlar. Uygulamalar ve hizmetler için sistem durumu raporlarını varlıkları uygulanır ve Service Fabric açısından doğru davranmakta olduğunu doğrulayın. Raporları, tüm sistem durumu hizmetinin iş mantığı veya askıdaki işlemleri algılamak izleme sağlamaz. Kullanıcı Hizmetleri kendi mantığı özgü bilgileri sistem durumu verilerle zenginleştirmek.

> [!NOTE]
> Kullanıcı watchdogs tarafından gönderilen durum raporları görünür yalnızca *sonra* sistem bileşenleri bir varlık oluşturun. Bir varlık silindiğinde, sistem sağlığı deposunu ilişkili tüm sistem durumu raporları otomatik olarak siler. Varlık yeni bir örneğini oluştururken, aynı durum geçerlidir, örneğin, yeni bir durum bilgisi olan kalıcı hizmet çoğaltma örneği oluşturulur. Eski örneği ile ilişkili tüm raporlar silinir ve Mağaza'dan temizlendi.
> 
> 

Sistem bileşeni raporları ile başlayan kaynak tarafından tanımlanır "**sistem.**" önek. Geçersiz parametreler raporlarla reddedildi olarak watchdogs için kaynakları, aynı öneke kullanamazsınız.

Bazı sistem raporları neyin tetikleyeceğini anlamak ve bunlar temsil eden olası sorunları gidermek nasıl öğrenmek için bakalım.

> [!NOTE]
> Kümede neler olduğunu içine görünürlüğünü artırmak koşullara ilgi rapor eklemek Service Fabric devam eder ve varolan raporları uygulamaları daha hızlı sorun gidermenize yardımcı olması için daha fazla ayrıntı geliştirilebilir.
> 
> 

## <a name="cluster-system-health-reports"></a>Sistem durumu raporlarını küme
Küme durumu varlık health store içinde otomatik olarak oluşturulur. Her şeyi düzgün çalışmıyorsa, sistemi raporu yok.

### <a name="neighborhood-loss"></a>Komşuları kaybı
**System.Federation** Komşuları kaybı algıladığında bir hata bildirir. Tek tek düğümlerden rapordur ve düğüm kimliği özellik adında dahil edilir. Bir Komşuları tüm Service Fabric halka kaybederseniz boşluk rapor her iki tarafında temsil eden iki olay genellikle bekleyebilirsiniz. Daha fazla Semt kaybolursa, daha fazla olay vardır.

Rapor kira genel zaman aşımı yaşam süresi (TTL) belirtir. Koşul etkin kaldığı sürece raporu her TTL süresinin yarısı gönderilir. Olayın süresi dolduğunda, otomatik olarak kaldırılır. Raporlama düğümü çalışmıyor olsa bile rapor health Store'dan doğru temizlenir olduğunu Kaldır zaman süresi davranış sağlar.

* **SourceId**: System.Federation
* **Özellik**: ile başlayan **Komşuları** ve düğüm bilgiler yer almaktadır.
* **Sonraki adımlar**: neden Komşuları örneğin çalındığında araştırmak, küme düğümleri arasındaki iletişimi denetleyin.

## <a name="node-system-health-reports"></a>Düğüm sistem durumu raporları
**System.FM**, Yük Devretme Yöneticisi hizmeti temsil eden durumda küme düğümleri hakkında bilgi yönetir yetkilidir. Her düğüm System.FM durumunu gösteren bir raporu olması gerekir. Düğümün varlık, düğüm durumu kaldırıldığında kaldırılır. Daha fazla bilgi için bkz: [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync).

### <a name="node-updown"></a>Düğüm yukarı/aşağı
Düğüm (Bu da çalışır durumda) halkası katıldığında System.FM Tamam bildirir. Düğüm halka departs zaman bir hata bildiriyor (aşağı, yükseltme ya da yalnızca bozulmuş başarısız oldu çünkü). Sistem durumu mağaza tarafından oluşturulmuş sistem durumu hiyerarşi bağıntı System.FM düğüm raporları ile birlikte dağıtılan varlıklar üzerinde çalışır. Tüm dağıtılan varlıkların sanal üst düğümü dikkate alır. Düğüm olarak en fazla bildirilmezse bu düğümde dağıtılan varlıklar sorguları sunulan varlıkla ilişkilendirilen örnekle aynı örneği içeren System.FM tarafından. System.FM bildirdiğinde düğümü çalışmıyor veya yeni bir örneği olarak yeniden sistem sağlığı deposunu otomatik olarak yalnızca aşağı düğüm veya düğüm önceki örneği üzerinde bulunabilir dağıtılan varlıklar temizler.

* **SourceId**: System.FM
* **Özellik**: durumu.
* **Sonraki adımlar**: düğüm için yükseltme çalışmıyorsa, yükseltme yapıldıktan sonra geri gelmesi. Bu durumda, sistem durumunu Tamam olarak geçer. Düğüm geri gelmez veya bu başarısız olursa, sorunu daha fazla araştırma gerekir.

Aşağıdaki örnek Tamam düğümü için bir sistem durumu System.FM olayla gösterir:

```PowerShell
PS C:\> Get-ServiceFabricNodeHealth  _Node_0

NodeName              : _Node_0
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 8
                        SentAt                : 7/14/2017 4:54:51 PM
                        ReceivedAt            : 7/14/2017 4:55:14 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```


### <a name="certificate-expiration"></a>Sertifika süre sonu
**System.FabricNode** düğüm tarafından kullanılan sertifikalar sona erme olduğunda bir uyarı bildirir. Düğüm başına üç sertifika vardır: **Certificate_cluster**, **Certificate_server**, ve **Certificate_default_client**. Geçerlilik süresi en az iki hafta sonra rapor sistem durumu Tamam olur. Sona erme iki hafta içinde olduğunda, rapor türü bir uyarıdır. Bu olayların TTL sonsuzdur ve kümenin bir düğümü ayrıldığında, bunlar kaldırılır.

* **SourceId**: System.FabricNode
* **Özellik**: ile başlayan **sertifika** ve sertifika türü hakkında daha fazla bilgi içerir.
* **Sonraki adımlar**: sona erme olmaları durumunda sertifikaları güncelleştirin.

### <a name="load-capacity-violation"></a>Yük kapasite ihlali
Service Fabric yük dengeleyici düğüm kapasitesi ihlaline neden algıladığında bir uyarı bildirir.

* **SourceId**: System.PLB
* **Özellik**: ile başlayan **kapasite**.
* **Sonraki adımlar**: sağlanan ölçümler denetleyin ve düğüm üzerinde geçerli kapasitesini görüntüleyin.

### <a name="node-capacity-mismatch-for-resource-governance-metrics"></a>Kaynak İdaresi ölçümleri için düğüm kapasitesi uyuşmazlığı
Bir düğüm kapasiteleri küme bildiriminde tanımlanmışsa uyarı System.Hosting raporları gerçek düğümü kapasiteler kaynak İdaresi ölçümleri (bellek ve cpu çekirdekleri) için daha büyük. Sistem Durumu raporu gösterilecek ilk hizmet paketi kullanan [kaynak İdaresi](service-fabric-resource-governance.md) belirtilen bir düğümde kaydeder.

* **SourceId**: System.Hosting
* **Özellik**: ResourceGovernance
* **Sonraki adımlar**: yöneten hizmet paketleri uygulanmaz, beklendiği gibi bu bir sorun olabilir ve [kaynak İdaresi](service-fabric-resource-governance.md) düzgün şekilde çalışmaz. Bu ölçümler için doğru düğümü kapasiteler ile küme bildirimini güncelleştirmek veya değil hiç belirtin ve kullanılabilir kaynakları otomatik olarak algılamak için Service Fabric olanak tanır.

## <a name="application-system-health-reports"></a>Uygulama sistem durumu raporları
**System.CM**, Küme Yöneticisi hizmeti temsil eden bir uygulamayla ilgili bilgileri yöneten yetkili değil.

### <a name="state"></a>Durum
Uygulama oluşturulmuş veya güncelleştirilmiş System.CM olarak Tamam bildirir. Uygulama silindiğinde, böylece deposundan kaldırılabilir sistem sağlığı deposunu bildirir.

* **SourceId**: System.CM
* **Özellik**: durumu.
* **Sonraki adımlar**: uygulama oluşturulmuş veya güncelleştirilmiş yüklüyse, Küme Yöneticisi sistem durumu raporu içermelidir. Aksi takdirde, sorgu, örneğin, PowerShell cmdlet'ini vererek uygulama durumunu denetleyin **Get-ServiceFabricApplication - ApplicationName** *applicationName*.

Aşağıdaki örnek durum olayı gösterir **fabric: / WordCount** uygulama:

```PowerShell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/14/2017 4:55:10 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="service-system-health-reports"></a>Hizmet sistem durumu raporları
**System.FM**, Yük Devretme Yöneticisi hizmeti temsil eden durumda hizmetleri hakkında bilgi yönetir yetkilidir.

### <a name="state"></a>Durum
Hizmet oluşturduğunuzda System.FM olarak Tamam bildirir. Hizmet silindiğinde varlık health Store'dan siler.

* **SourceId**: System.FM
* **Özellik**: durumu.

Aşağıdaki örnek, üzerinde hizmet durumu olay gösterir **fabric: / WordCount/WordCountWebService**:

```PowerShell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountWebService -ExcludeHealthStatistics


ServiceName           : fabric:/WordCount/WordCountWebService
AggregatedHealthState : Ok
PartitionHealthStates : 
                        PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
                        AggregatedHealthState : Ok
                        
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 14
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="service-correlation-error"></a>Hizmet bağıntı hatası
**System.PLB** bir hizmeti güncelleştirme bir benzeşim zinciri oluşturur başka bir hizmetle ilişkili olduğunu algıladığında bir hata bildirir. Başarılı bir güncelleştirme olduğunda rapor temizlenir.

* **SourceId**: System.PLB
* **Özellik**: ServiceDescription.
* **Sonraki adımlar**: bağlantılı hizmeti açıklamaları denetleyin.

## <a name="partition-system-health-reports"></a>Bölüm sistem durumu raporları
**System.FM**, Yük Devretme Yöneticisi hizmeti temsil eden durumda hizmet bölümleri hakkında bilgi yönetir yetkilidir.

### <a name="state"></a>Durum
Bölüm oluşturulup oluşturulmadığını ve iyi durumda olduğunda System.FM olarak Tamam bildirir. Bölüm silindiğinde varlık health Store'dan siler.

Minimum yineleme sayısı bölüm bir hata bildirir. Bölüm minimum yineleme sayısı değil, ancak hedef çoğaltma sayı değil, bir uyarı bildirir. Bölüm çekirdek kaybında System.FM bir hata bildirir.

Diğer önemli olayları yeniden beklenenden ve yapı beklenenden daha uzun sürerse uzun sürerse bir uyarı içerir. Beklenen kez yeniden yapılandırma ve yapı yapılandırılabilir için bağlı hizmet senaryoları. Örneğin, bir hizmet durumu, Azure SQL veritabanı gibi bir terabayt varsa yapı durumu küçük miktarda bir hizmet için daha uzun sürer.

* **SourceId**: System.FM
* **Özellik**: durumu.
* **Sonraki adımlar**: sistem durumu iyi değil, bazı çoğaltmaları değil oluşturulmuş, açılan veya birincil veya ikincil doğru yükseltilmiş olduğunu mümkündür. 

Açıklama çekirdek kayıp tanımlıyorsa, kapalı çoğaltmalar için ayrıntılı durum raporu incelenerek ve bölüm getirmek için geri yardımcı getirilmesi sonra çevrimiçi olarak yedekleyin.

Açıklama olarak takılmış bir bölüm tanımlarsa [yeniden yapılandırma](service-fabric-concepts-reconfiguration.md), birincil çoğaltmadaki sistem durumu raporu ek bilgi sağlar.

Diğer System.FM sistem durumu raporlarının çoğaltmaları veya bölüm ya da diğer sistem bileşenlerini hizmetinden raporlarda olacaktır. 

Aşağıdaki örnekler, bu raporları bazıları açıklanmaktadır. 

Aşağıdaki örnek, sağlıklı bir bölüm gösterir:

```PowerShell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountWebService | Get-ServiceFabricPartitionHealth -ExcludeHealthStatistics -ReplicasFilter None

PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 70
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Aşağıdaki örnek, hedef çoğaltma sayısı bir bölüm durumunu gösterir. Sonraki adım nasıl yapılandırıldığını gösterir bölüm açıklaması elde etmektir: **MinReplicaSetSize** üç ve **TargetReplicaSetSize** yedi değil. Sonra beş bu durumda olan kümedeki düğüm sayısını alın. Çoğaltma hedef sayısı kullanılabilir düğüm sayısından daha yüksek olduğundan, bu durumda, iki çoğaltma, yerleştirilemez.

```PowerShell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None -ExcludeHealthStatistics


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 123
                        SentAt                : 7/14/2017 4:55:39 PM
                        ReceivedAt            : 7/14/2017 4:55:44 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/S Ready _Node_2 131444422260002646
                          N/S Ready _Node_4 131444422293113678
                          N/S Ready _Node_3 131444422293113679
                          N/S Ready _Node_1 131444422293118720
                          N/P Ready _Node_0 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5)
                        
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:55:44 PM, LastOk = 1/1/0001 12:00:00 AM
                        
                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131445250939703027
                        SentAt                : 7/14/2017 4:58:13 PM
                        ReceivedAt            : 7/14/2017 4:58:14 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        Secondary replica could not be placed due to the following constraints and properties:  
                        TargetReplicaSetSize: 7
                        Placement Constraint: N/A
                        Parent Service: N/A
                        
                        Constraint Elimination Sequence:
                        Existing Secondary Replicas eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        Existing Primary Replica eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.
                        
                        Nodes Eliminated By Constraints:
                        
                        Existing Secondary Replicas -- Nodes with Partition's Existing Secondary Replicas/Instances:
                        --
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        
                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None
                        
                        
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:56:14 PM, LastOk = 1/1/0001 12:00:00 AM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | select MinReplicaSetSize,TargetReplicaSetSize

MinReplicaSetSize TargetReplicaSetSize
----------------- --------------------
                2                    7                        

PS C:\> @(Get-ServiceFabricNode).Count
5
```

Aşağıdaki örnek takıldı bir bölüm durumunu yeniden yapılandırma nedeniyle iptal uygularken değil kullanıcı belirteci içinde gösterir **RunAsync** yöntemi. Birincil olarak işaretlenen herhangi bir çoğaltma (P), sistem durumu raporu araştırmaya yardımcı incelemek için aşağı sorunla ilgili daha fazla.

```PowerShell
PS C:\utilities\ServiceFabricExplorer\ClientPackage\lib> Get-ServiceFabricPartitionHealth 0e40fd81-284d-4be4-a665-13bc5a6607ec -ExcludeHealthStatistics 


PartitionId           : 0e40fd81-284d-4be4-a665-13bc5a6607ec
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', 
                        ConsiderWarningAsError=false.
                                               
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 7
                        SentAt                : 8/27/2017 3:43:09 AM
                        ReceivedAt            : 8/27/2017 3:43:32 AM
                        TTL                   : Infinite
                        Description           : Partition reconfiguration is taking longer than expected.
                        fabric:/app/test1 3 1 0e40fd81-284d-4be4-a665-13bc5a6607ec
                          P/S Ready Node1 131482789658160654
                          S/P Ready Node2 131482789688598467
                          S/S Ready Node3 131482789688598468
                          (Showing 3 out of 3 replicas. Total available replicas: 3)                        
                        
                        For more information see: http://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 8/27/2017 3:43:32 AM, LastError = 1/1/0001 12:00:00 AM
```
Bu sistem durumu raporu yeniden yapılandırma altında bölüm kopyalarını durumunu gösterir: 

```
  P/S Ready Node1 131482789658160654
  S/P Ready Node2 131482789688598467
  S/S Ready Node3 131482789688598468
```

Her çoğaltma için sistem durumu raporu içerir:
- Önceki yapılandırma rolü
- Geçerli yapılandırma rolü
- [Çoğaltma durumu](service-fabric-concepts-replica-lifecycle.md)
- Çoğaltma üzerinde çalıştığı düğüm
- Çoğaltma Kimliği

Örnek gibi bir durumda, daha fazla araştırma gereklidir. Sistem durumu olarak işaretlenmiş çoğaltmaları başlayarak tek tek her çoğaltmanın araştırmak `Primary` ve `Secondary` (131482789658160654 ve 131482789688598467) önceki örnekte.

### <a name="replica-constraint-violation"></a>Çoğaltma kısıtlama ihlali
**System.PLB** çoğaltma kısıtlama ihlali algılar ve tüm çoğaltmalarını yerleştirilemiyor varsa bir uyarı bildirir. Hangi kısıtlamaları rapor ayrıntıları göster ve çoğaltma yerleştirme özellikleri engelliyor.

* **SourceId**: System.PLB
* **Özellik**: ile başlayan **ReplicaConstraintViolation**.

## <a name="replica-system-health-reports"></a>Çoğaltma sistem durumu raporları
**System.RA**, yeniden yapılandırma aracı bileşeni temsil eden durumda çoğaltma durumu için yetkilidir.

### <a name="state"></a>Durum
Çoğaltma oluşturduğunuzda System.RA Tamam bildirir.

* **SourceId**: System.RA
* **Özellik**: durumu.

Aşağıdaki örnek, sağlıklı bir çoğaltma gösterir:

```PowerShell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422293118721
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131445248920273536
                        SentAt                : 7/14/2017 4:54:52 PM
                        ReceivedAt            : 7/14/2017 4:55:13 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_0
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:13 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="replicaopenstatus-replicaclosestatus-replicachangerolestatus"></a>ReplicaOpenStatus, ReplicaCloseStatus, ReplicaChangeRoleStatus
Bu özellik, bir çoğaltma açın, bir çoğaltma kapatmak veya bir rol bir çoğaltmadan başka bir geçiş çalışırken uyarılar veya hatalar belirtmek için kullanılır. Daha fazla bilgi için bkz: [çoğaltma yaşam döngüsü](service-fabric-concepts-replica-lifecycle.md). Hataları API çağrıları ya da bu süre boyunca, hizmet ana bilgisayar işleminin kilitlenme oluşturulan özel durumlar olabilir. C# kodundan API çağrıları nedeniyle hataları için Service Fabric sistem durumu raporu yığın izlemesi ve özel durum ekler.

Bu sistem durumu uyarıları, yerel olarak bazı sayısı (ilke) bağlı olarak eylem denedikten sonra ortaya çıkar. Service Fabric eylem maksimum eşik kadar yeniden dener. Maksimum eşiğine ulaşıldıktan sonra durumu düzeltmek için görev yapması deneyebilirsiniz. Bu deneme bu düğümde eylemi vazgeçmeden olarak işaretli bu uyarıları neden olabilir. Örneğin, bir düğümde açmak bir çoğaltma başarısız olduysa, Service Fabric sistem durumu uyarısı başlatır. Çoğaltma açmak başarısız olmaya devam ederse, kendi kendine onarmak için Service Fabric yapar. Bu eylem, başka bir düğümde aynı işlemi çalışırken gerektirebilir. Bu, bu çoğaltma temizlenecek yükseltilmiş uyarı neden olur. 

* **SourceId**: System.RA
* **Özellik**: **ReplicaOpenStatus**, **ReplicaCloseStatus**, ve **ReplicaChangeRoleStatus**.
* **Sonraki adımlar**: işlemi başarısız neden belirlemek için hizmet kodu ya da kilitlenme dökümleri araştırın.

Aşağıdaki örnek, atma bir çoğaltma durumunu gösterir. `TargetInvocationException` açık yönteminden. Açıklama hata noktasını içeren **IStatefulServiceReplica.Open**, özel durum türü **TargetInvocationException**ve yığın izleme.

```PowerShell
PS C:\> Get-ServiceFabricReplicaHealth -PartitionId 337cf1df-6cab-4825-99a9-7595090c0b1b -ReplicaOrInstanceId 131483509874784794


PartitionId           : 337cf1df-6cab-4825-99a9-7595090c0b1b
ReplicaId             : 131483509874784794
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.RA', Property='ReplicaOpenStatus', HealthState='Warning', 
                        ConsiderWarningAsError=false.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : ReplicaOpenStatus
                        HealthState           : Warning
                        SequenceNumber        : 131483510001453159
                        SentAt                : 8/27/2017 11:43:20 PM
                        ReceivedAt            : 8/27/2017 11:43:21 PM
                        TTL                   : Infinite
                        Description           : Replica had multiple failures during open on _Node_0 API call: IStatefulServiceReplica.Open(); Error = System.Reflection.TargetInvocationException (-2146232828)
Exception has been thrown by the target of an invocation.
   at Microsoft.ServiceFabric.Replicator.RecoveryManager.d__31.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Replicator.LoggingReplicator.d__137.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Replicator.DynamicStateManager.d__109.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Replicator.TransactionalReplicator.d__79.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Replicator.StatefulServiceReplica.d__21.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Services.Runtime.StatefulServiceReplicaAdapter.d__0.MoveNext()

    For more information see: http://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/27/2017 11:43:21 PM, LastOk = 1/1/0001 12:00:00 AM                        
```

Aşağıdaki örnek, kapatma sırasında sürekli olarak kilitlenen bir çoğaltma gösterir:

```PowerShell
C:>Get-ServiceFabricReplicaHealth -PartitionId dcafb6b7-9446-425c-8b90-b3fdf3859e64 -ReplicaOrInstanceId 131483565548493142


PartitionId           : dcafb6b7-9446-425c-8b90-b3fdf3859e64
ReplicaId             : 131483565548493142
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.RA', Property='ReplicaCloseStatus', HealthState='Warning', 
                        ConsiderWarningAsError=false.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : ReplicaCloseStatus
                        HealthState           : Warning
                        SequenceNumber        : 131483565611258984
                        SentAt                : 8/28/2017 1:16:01 AM
                        ReceivedAt            : 8/28/2017 1:16:03 AM
                        TTL                   : Infinite
                        Description           : Replica had multiple failures during close on _Node_1. The application 
                        host has crashed.
                        
                        For more information see: http://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 1:16:03 AM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="reconfiguration"></a>Yeniden yapılandırma
Bu özellik bir çoğaltma yaparken göstermek için kullanılan bir [yeniden yapılandırma](service-fabric-concepts-reconfiguration.md) yeniden durmuş veya takılmış olduğunu algılar. Bu sistem durumu raporu nerede etkin birincil ikincil indirgenir çoğaltmasında olabilir geçerli rolünü birincil dışında bir takas birincil yapılandırması durumlarda çoğaltma olabilir.

Yeniden yapılandırma aşağıdaki nedenlerden birinden dolayı takılmış:

- Bir eylem aynı çoğaltma yeniden yapılandırma gerçekleştirme biri olarak yerel çoğaltmasında Tamamlanıyor değil. Bu durumda, diğer bileşenler'den Bu çoğaltma sistem durumu raporları araştırma, System.RAP veya System.RE, ek bilgiler sağlar.

- Uzak bir çoğaltma üzerinde bir eylem Tamamlanıyor değil. Eylemler bekleyen çoğaltmaları sistem durumu raporu listelenir. Uzak yinelemeler için sistem durumu raporları hakkında daha fazla araştırma yapılması gerekir. Bu düğüm ve uzak düğümün arasında iletişim sorunları olabilir.

Nadir durumlarda, iletişim veya bu düğüm ve Yük Devretme Yöneticisi hizmeti arasındaki diğer sorunları nedeniyle yeniden takılabilir.

* **SourceId**: System.RA
* **Özellik**: **yeniden yapılandırma**.
* **Sonraki adımlar**: sistem durumu raporu açıklaması bağlı olarak yerel veya uzak çoğaltmaları araştırın.

Aşağıdaki örnek, bir yeniden yapılandırma yerel kopyada burada takıldı bir sistem durumu raporu gösterir. Bu örnekte, bunu bir hizmet nedeniyle iptal belirteci uygularken değil.

```PowerShell
PS C:\> Get-ServiceFabricReplicaHealth -PartitionId 9a0cedee-464c-4603-abbc-1cf57c4454f3 -ReplicaOrInstanceId 131483600074836703


PartitionId           : 9a0cedee-464c-4603-abbc-1cf57c4454f3
ReplicaId             : 131483600074836703
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.RA', Property='Reconfiguration', HealthState='Warning', 
                        ConsiderWarningAsError=false.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : Reconfiguration
                        HealthState           : Warning
                        SequenceNumber        : 131483600309264482
                        SentAt                : 8/28/2017 2:13:50 AM
                        ReceivedAt            : 8/28/2017 2:13:57 AM
                        TTL                   : Infinite
                        Description           : Reconfiguration is stuck. Waiting for response from the local replica
                        
                        For more information see: http://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 2:13:57 AM, LastOk = 1/1/0001 12:00:00 AM
```

Bir sistem durumu raporu bir yeniden yapılandırma burada aşağıdaki örnekte gösterildiği iki uzaktan çoğaltmaların yanıt beklerken takıldı. Bu örnekte, geçerli birincil dahil olmak üzere bölümünde üç çoğaltmaları vardır. 

```Powershell
PS C:\> Get-ServiceFabricReplicaHealth -PartitionId  579d50c6-d670-4d25-af70-d706e4bc19a2 -ReplicaOrInstanceId 131483956274977415


PartitionId           : 579d50c6-d670-4d25-af70-d706e4bc19a2
ReplicaId             : 131483956274977415
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.RA', Property='Reconfiguration', HealthState='Warning', ConsiderWarningAsError=false.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : Reconfiguration
                        HealthState           : Warning
                        SequenceNumber        : 131483960376212469
                        SentAt                : 8/28/2017 12:13:57 PM
                        ReceivedAt            : 8/28/2017 12:14:07 PM
                        TTL                   : Infinite
                        Description           : Reconfiguration is stuck. Waiting for response from 2 replicas
                        
                        Pending Replicas: 
                        P/I Down 40 131483956244554282
                        S/S Down 20 131483956274972403
                        
                        For more information see: http://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 12:07:37 PM, LastOk = 1/1/0001 12:00:00 AM
```

Bu sistem durumu raporu yeniden iki çoğaltmaların yanıt beklerken takıldı gösterir: 

```
    P/I Down 40 131483956244554282
    S/S Down 20 131483956274972403
```

Her çoğaltma için aşağıdaki bilgiler verilmiştir:
- Önceki yapılandırma rolü
- Geçerli yapılandırma rolü
- [Çoğaltma durumu](service-fabric-concepts-replica-lifecycle.md)
- Düğüm kimliği
- Çoğaltma Kimliği

Yeniden yapılandırma engellemesini kaldırmak için:
- **Aşağı** çoğaltmaları getirdiği. 
- **Inbuild** çoğaltmaları yapı tamamlamak ve geçiş için hazır.

### <a name="slow-service-api-call"></a>Yavaş hizmeti API çağrısı
**System.RAP** ve **System.Replicator** kullanıcı hizmet koduna bir çağrı yapılandırılmış süreden uzun sürerse bir uyarı rapor. Arama tamamlandığında uyarı temizlenir.

* **SourceId**: System.RAP veya System.Replicator
* **Özellik**: yavaş API adı. Açıklama API bekleyen kaldığı süreyi hakkında daha fazla ayrıntı sağlar.
* **Sonraki adımlar**: çağrı neden beklenenden uzun sürüyor araştırın.

İptal uygularken değil güvenilir bir hizmet için System.RAP aşağıdaki örnek sistem durumu olayı içinde belirteci gösterir **RunAsync**:

```PowerShell
PS C:\> Get-ServiceFabricReplicaHealth -PartitionId 5f6060fb-096f-45e4-8c3d-c26444d8dd10 -ReplicaOrInstanceId 131483966141404693


PartitionId           : 5f6060fb-096f-45e4-8c3d-c26444d8dd10
ReplicaId             : 131483966141404693
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.RA', Property='Reconfiguration', HealthState='Warning', ConsiderWarningAsError=false.
                        
HealthEvents          :                         
                        SourceId              : System.RAP
                        Property              : IStatefulServiceReplica.ChangeRole(S)Duration
                        HealthState           : Warning
                        SequenceNumber        : 131483966663476570
                        SentAt                : 8/28/2017 12:24:26 PM
                        ReceivedAt            : 8/28/2017 12:24:56 PM
                        TTL                   : Infinite
                        Description           : The api IStatefulServiceReplica.ChangeRole(S) on _Node_1 is stuck. Start Time (UTC): 2017-08-28 12:23:56.347.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 12:24:56 PM, LastOk = 1/1/0001 12:00:00 AM
                        
```

Özellik ve metin hangi API takılmış gösterir. Farklı takılmış API'ler sonraki adımlarınızı farklıdır. Herhangi bir API'yi *IStatefulServiceReplica* veya *IStatelessServiceInstance* genellikle hizmeti kodunda bir hatadır. Aşağıdaki bölümde nasıl bunlar için çevir açıklanmaktadır [Reliable Services modeli](service-fabric-reliable-services-lifecycle.md):

- **IStatefulServiceReplica.Open**: Bu uyarı bildiren bir çağrı `CreateServiceInstanceListeners`, `ICommunicationListener.OpenAsync`, veya kılınırsa, `OnOpenAsync` takıldı.

- **IStatefulServiceReplica.Close** ve **IStatefulServiceReplica.Abort**: en yaygın çalışması için geçirilen iptal belirteci uygularken değil bir hizmettir `RunAsync`. Ayrıca, olabilir `ICommunicationListener.CloseAsync`, veya kılınırsa, `OnCloseAsync` takıldı.

- **IStatefulServiceReplica.ChangeRole (S)** ve **IStatefulServiceReplica.ChangeRole(N)**: en yaygın çalışması için geçirilen iptal belirteci uygularken değil bir hizmettir `RunAsync`.

- **IStatefulServiceReplica.ChangeRole(P)**: en yaygın durumdur hizmet görevden döndürmedi `RunAsync`.

Takılmış diğer API çağrıları bulunan **IReplicator** arabirimi. Örneğin:

- **IReplicator.CatchupReplicaSet**: Bu uyarı ikisinden birini gösterir. Ya da vardır çoğaltmaları bölüm veya System.FM sistem durumu raporu takılmış yeniden yapılandırılması için çoğaltma durumunu bakarak belirlenebilir çoğaltmaları yukarı yetersiz. Veya çoğaltmaları işlemleri aktarımının değil. PowerShell command-let `Get-ServiceFabricDeployedReplicaDetail` tüm çoğaltmaların ilerlemesini belirlemek için kullanılabilir. Sorun yinelemelerle özelliği arasındadır `LastAppliedReplicationSequenceNumber` birincil 's `CommittedSequenceNumber`.

- **IReplicator.BuildReplica (<Remote ReplicaId>)**: Bu uyarı oluşturma işlemindeki bir sorun olduğunu gösterir. Daha fazla bilgi için bkz: [çoğaltma yaşam döngüsü](service-fabric-concepts-replica-lifecycle.md). Çoğaltıcı adresi yanlış yapılandırma nedeniyle olabilir. Daha fazla bilgi için bkz: [durum bilgisi olan güvenilir hizmetler yapılandırma](service-fabric-reliable-services-configuration.md) ve [hizmet bildiriminde kaynakları belirtme](service-fabric-service-manifest-resources.md). Uzak düğümün bir sorun da olabilir.

### <a name="replication-queue-full"></a>Çoğaltma kuyruğu dolu
**System.Replicator** çoğaltma sırası dolu olduğunda bir uyarı bildirir. Bir veya daha fazla ikincil çoğaltmaları işlemleri onaylamak yavaş olduğu için birincil, çoğaltma sırası genellikle tam haline gelir. Hizmet işlemleri uygulamak yavaş olduğunda ikincil, bu genellikle gerçekleşir. Sıra dolu olduğunda uyarı temizlenir.

* **SourceId**: System.Replicator
* **Özellik**: **PrimaryReplicationQueueStatus** veya **SecondaryReplicationQueueStatus**çoğaltma rolü bağlı olarak.

### <a name="slow-naming-operations"></a>Yavaş adlandırma işlemleri
**System.NamingService** adlandırma işlemi kabul edilebilir daha uzun sürerse, birincil Çoğaltmada durumu raporları. Adlandırma işlemleri örnekleri [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) veya [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync). Daha fazla yöntem FabricClient altında örneğin altında bulunabilir [hizmet yönetimi yöntemleri](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) veya [özellik yönetimi yöntemleri](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).

> [!NOTE]
> Adlandırma hizmeti kümedeki bir konuma hizmet adlarını çözer ve kullanıcıların hizmet adlarını ve özelliklerini yönetmenize olanak tanır. Bu bir Service Fabric bölümlenmiş kalıcı hizmetidir. Bölümlerden birini temsil eden *Authority Owner*, tüm Service Fabric adları ve Hizmetleri hakkındaki meta verileri içerir. Service Fabric adları olarak adlandırılan farklı bölümleri için eşlenen *Name Owner* bölümler için genişletilebilir bir hizmettir. Daha fazla bilgi edinin [hizmet adlandırma](service-fabric-architecture.md).
> 
> 

Bir adlandırma işlemi beklenenden daha uzun sürerse, işlemi birincil çoğaltmadaki işlemi hizmet adlandırma hizmeti bölümün uyarı raporu ile işaretlenir. İşlem başarıyla tamamlanırsa uyarı temizlenir. İşlemi bir hata ile tamamlarsa, sistem durumu raporu hatayla ilgili ayrıntılar içerir.

* **SourceId**: System.NamingService
* **Özellik**: önek ile başlayan "**Duration_**" ve yavaş işlemi ve işlem uygulandığı Service Fabric adını tanımlar. Örneğin, ise hizmet adını oluşturma **doku: / MyApp/MyService** çok uzun sürer, özelliğidir **Duration_AOCreateService.fabric:/MyApp/MyService**. Bu ad ve işlem için adlandırma bölümün rolü ise "AO" işaret ediyor.
* **Sonraki adımlar**: adlandırma işlemi neden başarısız denetleyin. Her bir işlemin farklı kök neden olabilir. Örneğin, delete hizmet takılmış olabilir. Uygulama ana bilgisayar hizmeti kodunda bir kullanıcı hata nedeniyle bir düğümde kilitlenen tutar çünkü hizmet kalmış.

Aşağıdaki örnek oluşturma hizmeti işlemi gösterir. İşlemi yapılandırılan süreden daha uzun sürdü. "AO" yeniden deneme sayısı ve iş "Hayır" olarak gönderir Son işlemi zaman AŞIMI ile tamamlandı "Hayır". Bu durumda, aynı çoğaltma "AO" ve "Hayır" rolleri için birincil özelliğidir.

```PowerShell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>DeployedApplication sistem durumu raporları
**System.Hosting** yetkilisi dağıtılan varlıklar üzerinde.

### <a name="activation"></a>Etkinleştirme
Bir uygulama düğümde başarıyla etkinleştirildi System.Hosting olarak Tamam bildirir. Aksi takdirde bir hata bildirir.

* **SourceId**: System.Hosting
* **Özellik**: ürün sürümü de dahil olmak üzere etkinleştirme.
* **Sonraki adımlar**: neden etkinleştirme başarısız oldu uygulama sağlıksız ise araştırın.

Aşağıdaki örnek, başarılı bir etkinleştirme gösterir:

```PowerShell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ExcludeHealthStatistics

ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_1
                                     AggregatedHealthState : Ok
                                     
HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131445249083836329
                                     SentAt                : 7/14/2017 4:55:08 PM
                                     ReceivedAt            : 7/14/2017 4:55:14 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>İndir
Uygulama paketi yükleme başarısız olursa System.Hosting bir hata bildirir.

* **SourceId**: System.Hosting
* **Özellik**: **indirin:***RolloutVersion*.
* **Sonraki adımlar**: indirme düğümde neden başarısız araştırın.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage sistem durumu raporları
**System.Hosting** yetkilisi dağıtılan varlıklar üzerinde.

### <a name="service-package-activation"></a>Hizmet paketi etkinleştirme
System.Hosting olarak Tamam düğümde hizmet paketi etkinleştirme başarılı olup olmadığını bildirir. Aksi takdirde bir hata bildirir.

* **SourceId**: System.Hosting
* **Özellik**: etkinleştirme.
* **Sonraki adımlar**: neden etkinleştirme başarısız araştırın.

### <a name="code-package-activation"></a>Kod paketi etkinleştirme
System.Hosting Tamam için her kod paketi etkinleştirme başarılı olup olmadığını bildirir. Etkinleştirme başarısız olursa, yapılandırılan bir uyarı bildirir. Varsa **CodePackage** etkinleştirilemiyor veya yapılandırılmış büyük bir hata ile sona erer **CodePackageHealthErrorThreshold**, barındırma bir hata bildirir. Bir hizmet paketi birden çok kod paketler içeriyorsa, bir etkinleştirme raporu her biri için oluşturulur.

* **SourceId**: System.Hosting
* **Özellik**: öneki kullanan **CodePackageActivation** ve kod paketi ve giriş noktası olarak adını içeren **CodePackageActivation:***CodePackageName* :*SetupEntryPoint/EntryPoint*. Örneğin, **CodePackageActivation:Code:SetupEntryPoint**.

### <a name="service-type-registration"></a>Hizmet türü kayıt
System.Hosting Tamam hizmet türü başarıyla kayıtlı olup olmadığını bildirir. Hata raporları kayıt, zaman içindeki kullanılarak yapılandırılan değildi yapıldığında **ServiceTypeRegistrationTimeout**. Çalışma zamanı kapattıysanız, hizmet türü düğümden kaydettirilmemiş ve barındırma bir uyarı bildirir.

* **SourceId**: System.Hosting
* **Özellik**: öneki kullanan **ServiceTypeRegistration** ve hizmet türü adı içerir. Örneğin, **ServiceTypeRegistration:FileStoreServiceType**.

Aşağıdaki örnek, sağlıklı dağıtılmış hizmet paketi gösterir:

```PowerShell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_1
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131445249084026346
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131445249084306362
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131445249088096842
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>İndir
Hizmet paketi indirme işlemi başarısız olursa System.Hosting bir hata bildirir.

* **SourceId**: System.Hosting
* **Özellik**: **indirin:***RolloutVersion*.
* **Sonraki adımlar**: indirme düğümde neden başarısız araştırın.

### <a name="upgrade-validation"></a>Yükseltme doğrulaması
System.Hosting, yükseltme sırasında doğrulama başarısız olursa veya düğümde yükseltme başarısız olursa bir hata bildirir.

* **SourceId**: System.Hosting
* **Özellik**: öneki kullanan **FabricUpgradeValidation** ve yükseltme sürümünü içerir.
* **Açıklama**: işaret hatayla karşılaşıldı.

### <a name="undefined-node-capacity-for-resource-governance-metrics"></a>Kaynak İdaresi ölçümleri tanımsız düğüm kapasitesi
Düğüm kapasitesi küme bildiriminde tanımlı değil ve otomatik algılama için yapılandırma kapalı System.Hosting bir uyarı bildirir. Hizmet paketi kullandığında, Service Fabric sistem durumu uyarısı Yükselt [kaynak İdaresi](service-fabric-resource-governance.md) belirtilen bir düğümde kaydeder.

* **SourceId**: System.Hosting
* **Özellik**: ResourceGovernance
* **Sonraki adımlar**: Bu sorunu çözmek için tercih edilen yolu kullanılabilir kaynakların otomatik algılamayı etkinleştirmek için küme bildirimine değiştirmektir. Başka bir yolu, bu ölçümler için doğru belirtilen düğümün kapasiteler ile küme bildiriminde güncelleştiriyor.

## <a name="next-steps"></a>Sonraki adımlar
[Service Fabric sistem durumu raporlarını görüntüle](service-fabric-view-entities-aggregated-health.md)

[Nasıl rapor ve hizmetin sistem durumunu denetle](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[İzleme ve Hizmetleri yerel olarak tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric uygulama yükseltme](service-fabric-application-upgrade.md)

