---
title: Sistem Durum raporlarıyla ilgili sorunları giderme | Microsoft Docs
description: Sorun giderme küme veya uygulama sorunları için Azure Service Fabric bileşenleri ve bunların kullanımını tarafından gönderilen sistem durumu raporları açıklar
services: service-fabric
documentationcenter: .net
author: oanapl
manager: chackdan
editor: ''
ms.assetid: 52574ea7-eb37-47e0-a20a-101539177625
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2018
ms.author: oanapl
ms.openlocfilehash: 4ece2dc1df3d29a3024c7efe15dd8cecfd9666db
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663868"
---
# <a name="use-system-health-reports-to-troubleshoot"></a>Sorun gidermek için sistem durum raporlarını kullanma
Azure Service Fabric bileşenleri çıktığı kümedeki tüm varlıklarda sistem durumu raporları sağlar. [Sistem durumu deposu](service-fabric-health-introduction.md#health-store) oluşturur ve sistem raporlarına dayalı varlıkları siler. Bu da onları varlık etkileşimleri yakalayan bir hiyerarşide düzenler.

> [!NOTE]
> Sistem durumu ile ilgili kavramları anlamak için daha fazla bilgi okuyun [Service Fabric sistem durumu modeli](service-fabric-health-introduction.md).
> 
> 

Sistem durumu raporlarını, küme ve uygulama işlevselliğini ve bayrağı sorunları görünürlük sağlar. Uygulamalar ve hizmetler için sistem durumu raporlarını varlıkları uygulanır ve Service Fabric açısından doğru davrandığını doğrulayın. Raporları, tüm sistem durumu hizmetinin iş mantığını veya askıdaki işlemleri algılamak izleme sağlaması gerekmez. Hizmetleri kendi mantığını özgü bilgileri sistem durumu verileri zenginleştirebilirsiniz.

> [!NOTE]
> Kullanıcı watchdogs tarafından gönderilen durum raporları görünür yalnızca *sonra* sistem bileşenleri bir varlık oluşturun. Bir varlık silindiğinde, sistem durumu deposu onunla ilişkili tüm sistem durumu raporları otomatik olarak siler. Bir varlığın yeni bir örneği oluşturulduğunda, aynı durum geçerlidir. Yeni bir durum bilgisi olan kalıcı hizmet çoğaltma örneği oluşturulduğunda bir örnektir. Eski örneğiyle ilişkili tüm raporlar silinir ve Mağaza'dan temizlenir.
> 
> 

Sistem bileşeni raporları ile başlayan kaynağı tarafından tanımlanır "**sistem.**" önek. Geçersiz parametreler raporlarla reddedilmiş olarak watchdogs için kaynakları, aynı ön ekini kullanamazsınız.

Bunları tetikler anlamak ve temsil ettikleri olası sorunları düzeltmek hakkında bilgi almak için bazı sistem raporları göz atalım.

> [!NOTE]
> Service Fabric kümesi ve uygulamalarda neler olduğunu içine görünürlüğünü artırmak koşullar ilgi raporlar eklemek devam eder. Var olan raporların daha hızlı bir şekilde sorun gidermeye yardımcı olmak için daha fazla ayrıntı ile geliştirilebilir.
> 
> 

## <a name="cluster-system-health-reports"></a>Küme sistem durumu raporlarını
Küme sistem durumu varlık health store içinde otomatik olarak oluşturulur. Her şeyin düzgün çalıştığından, sistem raporu yok.

### <a name="neighborhood-loss"></a>Komşu kaybı
**System.Federation** , bir komşu kaybı algıladığında bir hata bildirir. Tek tek düğümleri rapordur ve düğüm kimliği özellik adında dahil edilir. Tüm Service Fabric halka bir Komşuları kaybedildiğinde, genellikle her iki tarafında boşluk rapor temsil eden iki olay bekleyebilirsiniz. Daha fazla Semt kaybolması durumunda, daha fazla olay vardır.

Rapor yaşam süresi (TTL) kira genel zaman aşımını belirtir. Koşul etkin kaldığı sürece raporu TTL süresi boyunca her yarısında gönderilir. Belirtecin süresi dolduğunda, olayın otomatik olarak kaldırılır. Raporlama düğümü çalışmıyor olsa bile rapor health Store'dan doğru temizlenir, remove-zaman süresi davranış sağlar.

* **SourceId**: System.Federation
* **Özellik**: İle başlayan **Komşuları** ve düğüm bilgileri içerir.
* **Sonraki adımlar**: Komşu kaybı neden olduğunu araştırın. Örneğin, küme düğümler arasında iletişim bakın.

### <a name="rebuild"></a>Yeniden derle

Yük Devretme Yöneticisi'ni (FM) hizmeti, küme düğümleri hakkında bilgi yönetir. FM verilerini kaybeder ve veri kaybı gider, küme düğümleri hakkında en güncel bilgilere sahip olmasını garanti edemez. Bu durumda, sistem yeniden geçer, ve System.FM veri kümedeki tüm düğümlerden durumunu yeniden derlemek için toplar. Bazı durumlarda, ağ veya düğüm sorunları nedeniyle yeniden takılı durmuş veya. Aynı durum, Yük Devretme Yöneticisi ana (FMM) hizmetiyle meydana gelebilir. FMM FMs kümede olduğu, izleme tutan bir durum bilgisi olmayan sistemi hizmetidir. FMM'ın birincil her zaman 0 olarak en yakın kimlikli düğümüdür. Bu düğüm bırakılan, yeniden derleme tetiklenir.
Önceki koşullardan biri gerçekleştiğinde **System.FM** veya **System.FMM** bir hata raporu işaretler. Yeniden iki aşama birinde takılmış olabilir:

* **Yayın için bekleyen**: FM/FMM diğer düğümlerden yayın iletisi yanıt bekler.

  * **Sonraki adımlar**: Düğümler arasında ağ bağlantısı sorunu olup olmadığını araştırın.
* **Düğümler için bekleniyor**: FM/FMM zaten diğer düğümlerden yayın yanıtı alınan ve belirli düğümlerden yanıt bekliyor. Sistem Durumu raporu yanıt bekleyen FM/FMM için düğümleri listeler.
   * **Sonraki adımlar**: FM/FMM ve listelenen düğümleri arasındaki ağ bağlantısını inceleyin. Listelenen her düğüm için diğer olası sorunları araştırın.

* **SourceId**: System.FM veya System.FMM
* **Özellik**: Yeniden oluşturun.
* **Sonraki adımlar**: Açıklamayı sistem durumu raporu listelenen herhangi bir belirli düğümlerinin durumunu yanı sıra, düğümler arasındaki ağ bağlantısını inceleyin.

## <a name="node-system-health-reports"></a>Sistem durumu raporlarını düğümü
Yük Devretme Yöneticisi hizmeti temsil eden System.FM küme düğümleri hakkında bilgi yöneten yetkilisidir. Her bir düğümünde System.FM durumunu gösteren bir rapordan olması gerekir. Düğümün varlık, düğüm durumu kaldırıldığında kaldırılır. Daha fazla bilgi için [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync).

### <a name="node-updown"></a>Düğümü yukarı/aşağı
Düğüm (onu çalışır duruma) halkası katıldığında System.FM Tamam bildirir. Düğüm halka departs olduğunda bir hata bildirir (aşağı, yükseltme ya da yalnızca şeklindedir başarısız olması nedeniyle). Sistem durumu deposu tarafından oluşturulan durum hiyerarşisi bağıntı System.FM düğüm raporları ile birlikte dağıtılan varlıklar üzerinde çalışır. Dağıtılan tüm varlıkların sanal bir üst düğümü olarak değerlendirir. Düğümü yukarı olarak bildirilirse bu düğümde dağıtılan varlıklar sorgular üzerinden sunulan System.FM, varlıklarla ilişkili örnekle aynı örneğine göre. System.FM bildirdiğinde düğümü çalışmıyor veya yeni bir örneği yeniden sistem durumu deposu otomatik olarak yalnızca aşağı düğümde veya düğüm önceki örneği bulunabilir dağıtılan varlıklar temizler.

* **SourceId**: System.FM
* **Özellik**: Durumu.
* **Sonraki adımlar**: Düğüm için bir yükseltme çalışmıyorsa, yükseltme yapıldıktan sonra geri gelmesi. Bu durumda, sistem durumunu Tamam olarak geçer. Düğüm geri gelmeyen veya başarısız ise sorun, daha fazla incelenmesi gerekiyor.

Aşağıdaki örnekte System.FM olay Tamam düğümü için bir sistem durumu ile gösterilmektedir:

```powershell
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


### <a name="certificate-expiration"></a>Sertifika bitiş tarihi
**System.FabricNode** düğüm tarafından kullanılan sertifikalar sona erme olduğunda bir uyarı bildirir. Düğüm başına üç sertifika bulunur: **Certificate_cluster**, **Certificate_server**, ve **Certificate_default_client**. Süre en az iki hafta sonra rapor durumunun Tamam olur. Sona erme iki hafta içinde olduğunda, rapor türü bir uyarıdır. Bu olayların TTL sonsuzdur ve bir düğüm kümesi ayrıldığında kaldırılır.

* **SourceId**: System.FabricNode
* **Özellik**: İle başlayan **sertifika** ve sertifika türü hakkında daha fazla bilgi içerir.
* **Sonraki adımlar**: Sona erme olmaları durumunda, sertifikaları güncelleştirin.

### <a name="load-capacity-violation"></a>Yük kapasitesi ihlaline neden oluyor
Service Fabric yük Dngeleyici, düğüm kapasitesi ihlaline neden oluyor algıladığında bir uyarı bildirir.

* **SourceId**: System.PLB
* **Özellik**: İle başlayan **kapasite**.
* **Sonraki adımlar**: Sağlanan ölçümler denetleyin ve geçerli kapasite düğümünde görüntüleyebilirsiniz.

### <a name="node-capacity-mismatch-for-resource-governance-metrics"></a>Kaynak İdaresi ölçümleri için düğüm kapasitesi uyuşmazlığı
System.Hosting kaynak İdaresi ölçümleri (bellek ve CPU çekirdeği) için gerçek düğüm kapasiteleri büyük düğüm kapasiteleri küme bildiriminde tanımlanan bir uyarı bildirir. İlk hizmet paketi kullanan bir sistem durumu raporu görüntülendiğinde [kaynak İdaresi](service-fabric-resource-governance.md) belirtilen bir düğümde kaydeder.

* **SourceId**: System.Hosting
* **Özellik**: **ResourceGovernance**.
* **Sonraki adımlar**: Bu sorunu değerlendirip hizmet paketleri, beklendiği gibi zorunlu değildir çünkü bir sorun olabilir ve [kaynak İdaresi](service-fabric-resource-governance.md) düzgün çalışmaz. İle Bu ölçümler için doğru düğüm kapasiteleri gibi küme bildiriminin güncelleştirin veya yoksa bunları belirtin ve Service Fabric kullanılabilir kaynakları otomatik olarak algılamasını sağlar.

## <a name="application-system-health-reports"></a>Uygulama sistem durumu raporlarını
Küme Yöneticisi hizmeti temsil eden System.CM yöneten bir uygulamayla ilgili bilgileri yetkilisidir.

### <a name="state"></a>Durum
Uygulama oluşturulduğunda veya güncelleştirildiğinde System.CM olarak Tamam bildirir. Uygulama Mağazası'ndan kaldırılır, böylece silindiğinde sistem durumu deposu bildirir.

* **SourceId**: System.CM
* **Özellik**: Durumu.
* **Sonraki adımlar**: Uygulama oluşturulduğunda veya güncelleştirildiğinde, Küme Yöneticisi sistem durumu raporu eklemeniz gerekir. Aksi takdirde, bir sorgu verme tarafından uygulamanın durumunu denetleyin. Örneğin, PowerShell cmdlet kullanın **ServiceFabricApplication Get - ApplicationName** *applicationName*.

Aşağıdaki örnek durum olayı gösteren **fabric: / WordCount** uygulama:

```powershell
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

## <a name="service-system-health-reports"></a>Hizmet sistem durumu raporlarını
Yük Devretme Yöneticisi hizmeti temsil eden System.FM hizmetleri hakkında bilgi yöneten yetkilisidir.

### <a name="state"></a>Durum
Hizmet oluşturulduğunda System.FM olarak Tamam bildirir. Hizmet silindiğinde bir varlık health Store'dan siler.

* **SourceId**: System.FM
* **Özellik**: Durumu.

Aşağıdaki örnek, hizmet durumu olay gösterir **fabric: / WordCount/WordCountWebService**:

```powershell
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
**System.PLB** bir benzeşim zinciri oluşturur, başka bir hizmeti ile bir hizmeti güncelleştirme bağıntılı algıladığında bir hata bildirir. Başarılı bir güncelleştirme olduğunda rapor temizlenir.

* **SourceId**: System.PLB
* **Özellik**: **ServiceDescription**.
* **Sonraki adımlar**: Bağlantılı hizmet açıklamaları denetleyin.

## <a name="partition-system-health-reports"></a>Bölüm sistem durumu raporlarını
Yük Devretme Yöneticisi hizmeti temsil eden System.FM yöneten hizmet bölümleri hakkında bilgi yetkilisidir.

### <a name="state"></a>Durum
Bölüm oluşturuldu ve iyi durumda olduğunda System.FM olarak Tamam bildirir. Bölüm silindiğinde bir varlık health Store'dan siler.

En az yineleme sayısı bölümü bir hata bildirir. Bölümü altında en az çoğaltma sayısı değil, ancak hedef çoğaltma sayısı olduğu, bir uyarı bildirir. Bölüm çekirdek kaybında System.FM, bir hata bildirir.

Yeniden yapılandırma beklenenden ve yapı beklenenden daha uzun sürerse uzun sürerse bir uyarı diğer önemli olayları içerir. Beklenen kez derleme ve yeniden yapılandırılabilir için bağlı hizmet senaryoları. Örneğin, bir hizmet durumu, Azure SQL veritabanı gibi bir terabayt varsa, yapı durumu az miktarda bir hizmet için daha uzun sürer.

* **SourceId**: System.FM
* **Özellik**: Durumu.
* **Sonraki adımlar**: Sistem durumu iyi değil, yinelemeler değil oluşturulmuş, açık veya birincil veya ikincil doğru yükseltilen olduğunu mümkündür. 

Açıklama çekirdek kayıp tanımlıyorsa, devre dışı olan yinelemeler için ayrıntılı durum raporu inceleme ve bölüm getirmek için geri olur getirilmesi ardından çevrimiçi olarak yedekleyin.

Açıklama olarak takılı bir bölüm tanımlıyorsa [yeniden yapılandırma](service-fabric-concepts-reconfiguration.md), sistem durumu raporu birincil çoğaltmadaki ek bilgi sağlar.

Diğer System.FM sistem durumu raporlarının çoğaltmalar veya bölüm veya diğer sistem bileşenlerini hizmetinden raporlarda olacaktır. 

Aşağıdaki örnekler, bu raporlar bazılarını açıklar. 

Aşağıdaki örnek, sağlıklı bir bölümünü gösterir:

```powershell
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

Aşağıdaki örnek, hedef çoğaltma sayısı olan bir bölüm durumunu gösterir. Sonraki adım nasıl yapılandırıldığını gösteren bölüm açıklaması almaktır: **MinReplicaSetSize** üçüncü bölümüdür ve **TargetReplicaSetSize** yedidir. Ardından beş bu durumda olan kümedeki düğüm sayısını alın. Çoğaltma hedefi sayısı kullanılabilir düğüm sayısından daha yüksek olduğu halde bu durumda, yinelemeler, yerleştirilemez.

```powershell
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

Aşağıdaki örnek sistem durumunda bir bölümü yeniden yapılandırma nedeniyle iptal uygularken değil kullanıcı belirtecini gösterir **RunAsync** yöntemi. Birincil olarak işaretlenmiş tüm çoğaltma (P) sistem durumu raporu araştırmaya yardımcı detaya gitmek için aşağı sorunla daha fazla.

```powershell
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
                        
                        For more information see: https://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 8/27/2017 3:43:32 AM, LastError = 1/1/0001 12:00:00 AM
```
Bu sistem durumu raporu yeniden yapılandırma aşamasında bölüm çoğaltmalarını durumunu gösterir: 

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
- Yineleme Kimliği

Örnekte olduğu gibi bir durumda, daha fazla araştırma gereklidir. Olarak işaretlenmiş çoğaltmaları başlayarak tek tek her çoğaltma durumunu araştırmak `Primary` ve `Secondary` (131482789658160654 ve 131482789688598467) önceki örnekte.

### <a name="replica-constraint-violation"></a>Çoğaltma kısıtlama ihlali
**System.PLB** çoğaltma kısıtlama ihlali algılar ve tüm bölüm çoğaltmaları yerleştirilemiyor bir uyarı bildirir. Hangi kısıtlamaları rapor ayrıntılarını göster ve çoğaltma yerleştirme özellikleri engelliyor.

* **SourceId**: System.PLB
* **Özellik**: İle başlayan **ReplicaConstraintViolation**.

## <a name="replica-system-health-reports"></a>Çoğaltma sistem durumu raporlarını
**System.RA**, yeniden yapılandırma aracı bileşeni temsil eden çoğaltma durumu için yetkili olan.

### <a name="state"></a>Durum
Çoğaltma oluşturduğunuzda System.RA Tamam bildirir.

* **SourceId**: System.RA
* **Özellik**: Durumu.

Aşağıdaki örnek, sağlıklı bir yineleme gösterilmektedir:

```powershell
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
Bu özellik, bir çoğaltma açmak, bir çoğaltmayı kapatmak veya bir rolü çoğaltmadan diğerine geçiş çalışırken uyarılar veya hatalar belirtmek için kullanılır. Daha fazla bilgi için [çoğaltma yaşam döngüsü](service-fabric-concepts-replica-lifecycle.md). Hataları, API çağrıları ya da bu süre boyunca hizmet ana bilgisayarı işlemlerinin kilitlenmeleri oluşturulan özel durumlar olabilir. API kaynaklanan hatalar için çağıran C# kod, Service Fabric sistem durumu raporu özel durum ve yığın izlemesi ekler.

Bu sistem durumu uyarıları, yerel olarak bazı kaç kez (ilke) bağlı olarak eylemi nedeniyle yeniden denedikten sonra oluşturulur. Service Fabric, en yüksek eşik kadar eylemi yeniden dener. En fazla eşiğine ulaşıldıktan sonra bu durumu düzeltmek için görev yapacak deneyebilir. Bu deneme eylemi bu düğümdeki vazgeçmeden olarak işaretli bu uyarıları neden olabilir. Örneğin, bir düğümde açmak bir çoğaltma başarısız olduysa, Service Fabric sistem durumu uyarısı oluşturur. Service Fabric çoğaltma açmak başarısız olmaya devam ederse, kendi kendini onarmayı işlevi görür. Bu eylem, aynı işlemi başka bir düğümde çalışan gerektirebilir. Bu girişim, temizlenecek şu çoğaltma için yükseltilmiş uyarısına neden olur. 

* **SourceId**: System.RA
* **Özellik**: **ReplicaOpenStatus**, **ReplicaCloseStatus**, ve **ReplicaChangeRoleStatus**.
* **Sonraki adımlar**: Hizmet Kodu inceleme veya işlemi başarısız olmasının nedeni belirlemek için kilitlenme bilgi dökümleri.

Aşağıdaki örnek, yanlamasına ivme kazanmaz bir çoğaltma durumunu gösterir. `TargetInvocationException` kendi açık yönteminden. Açıklama, hata noktasını içeren **IStatefulServiceReplica.Open**, özel durum türü **TargetInvocationException**ve yığın izlemesi.

```powershell
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

    For more information see: https://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/27/2017 11:43:21 PM, LastOk = 1/1/0001 12:00:00 AM                        
```

Aşağıdaki örnek, kapatma sırasında sürekli olarak kilitlenen bir yineleme gösterilmektedir:

```powershell
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
                        
                        For more information see: https://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 1:16:03 AM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="reconfiguration"></a>Yeniden yapılandırma
Bu özellik bir çoğaltma yaparken göstermek için kullanılan bir [yeniden yapılandırma](service-fabric-concepts-reconfiguration.md) yeniden yapılandırma durmuş veya takılı olduğunu algılar. Bu sistem durumu raporu, burada çoğaltmadaki etkin birincil ikincil bölgeden indirgenir olabilir geçerli rolünü birincil dışında bir takas birincil yapılandırması durumlarda çoğaltma olabilir.

Yeniden yapılandırma aşağıdaki nedenlerden biri için takılabilir:

- Aynı çoğaltma yapılandırması gerçekleştiren bir yerel kopyasında bir eylem tamamlanmıyor. Bu durumda, diğer bileşenlerden Bu çoğaltma sistem durumu raporlarının araştırma, System.RAP veya System.RE, ek bilgi sağlar.

- Bir eylem, uzak bir çoğaltma üzerinde tamamlanmıyor. Eylemler bekleyen çoğaltma sistem durumu raporu listelenir. Daha fazla araştırma uzak yinelemeler için sistem durumu raporlarının yapılması gerekir. Bu düğüm ve uzak düğüm arasında iletişim sorunları olabilir.

Nadiren de olsa, iletişim veya bu düğüm ve Yük Devretme Yöneticisi hizmeti arasındaki diğer sorunları nedeniyle yeniden yapılandırma takıldı.

* **SourceId**: System.RA
* **Özellik**: Yeniden yapılandırma.
* **Sonraki adımlar**: Yerel veya uzak çoğaltma sistem durumu raporu açıklamasını bağlı olarak araştırın.

Aşağıdaki örnek, bir sistem durumu raporu yerel çoğaltmasında burada bir yeniden yapılandırma takıldı gösterir. Bu örnekte, bu hizmet nedeniyle iptal belirteci uygularken değil.

```powershell
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
                        
                        For more information see: https://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 2:13:57 AM, LastOk = 1/1/0001 12:00:00 AM
```

Aşağıdaki örnekte gösterildiği bir sistem durumu raporu bir yeniden yapılandırma nerede takılı iki uzak çoğaltma yanıt bekleniyor. Bu örnekte, geçerli birincil dahil olmak üzere bölümünde üç kopyaya vardır. 

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
                        
                        For more information see: https://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 12:07:37 PM, LastOk = 1/1/0001 12:00:00 AM
```

Bu sistem durumu raporu, yinelemeler yanıtı beklerken yeniden yapılandırma takıldı olduğunu gösterir: 

```
    P/I Down 40 131483956244554282
    S/S Down 20 131483956274972403
```

Her çoğaltma için aşağıdaki bilgileri verilir:
- Önceki yapılandırma rolü
- Geçerli yapılandırma rolü
- [Çoğaltma durumu](service-fabric-concepts-replica-lifecycle.md)
- Düğüm kimliği
- Yineleme Kimliği

Yeniden yapılandırma engelini kaldırmak için:
- **Aşağı** çoğaltmaları getirdiği. 
- **Inbuild** çoğaltmaları derlemesini tamamlama ve geçiş için hazır.

### <a name="slow-service-api-call"></a>Yavaş bir hizmeti API çağrısı
**System.RAP** ve **System.Replicator** kullanıcı hizmet kodu için bir çağrı yapılandırılmış süreden uzun sürerse bir uyarı raporu. Arama tamamlandığında uyarı temizlenir.

* **SourceId**: System.RAP veya System.Replicator
* **Özellik**: Yavaş API adı. Açıklama API uzun süredir bekleyen zaman hakkında daha fazla ayrıntı sağlar.
* **Sonraki adımlar**: Çağrı neden beklenenden uzun sürüyor araştırın.

İptal uygularken değil güvenilir bir hizmet için System.RAP aşağıdaki örnek sistem durumu olayı belirtecini gösterir **RunAsync**:

```powershell
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

Özellik ve metin, API takılı gösterir. Sonraki adımlar için farklı takılan API'leri farklıdır. Herhangi bir API'yi *IStatefulServiceReplica* veya *IStatelessServiceInstance* genellikle hizmeti kodunda bir hatadır. Aşağıdaki bölümde nasıl bunlar için çevir açıklanmaktadır [Reliable Services modeli](service-fabric-reliable-services-lifecycle.md):

- **IStatefulServiceReplica.Open**: Bu uyarı bildiren bir çağrı `CreateServiceInstanceListeners`, `ICommunicationListener.OpenAsync`, veya kılınırsa, `OnOpenAsync` takıldı.

- **IStatefulServiceReplica.Close** ve **IStatefulServiceReplica.Abort**: En yaygın çalışması için geçirilen iptal belirteci uygularken değil bir hizmettir `RunAsync`. Ayrıca, olabilir `ICommunicationListener.CloseAsync`, veya kılınırsa, `OnCloseAsync` takıldı.

- **IStatefulServiceReplica.ChangeRole (S)** ve **IStatefulServiceReplica.ChangeRole(N)**: En yaygın çalışması için geçirilen iptal belirteci uygularken değil bir hizmettir `RunAsync`.

- **IStatefulServiceReplica.ChangeRole(P)**: En yaygın durumda hizmeti bir görevden döndürmedi `RunAsync`.

Takılı kalarak diğer API çağrıları bulunan **IReplicator** arabirimi. Örneğin:

- **IReplicator.CatchupReplicaSet**: Bu uyarı, ikisinden birini gösterir. Çoğaltmaları yedeklemek için yetersiz vardır. Durumun bu olup olmadığını görmek için bölüm veya System.FM sistem durumu raporu takılan yapılandırması için çoğaltmaları çoğaltma durumunu bakın. Veya yineleme işlemleri sıkan değil. PowerShell cmdlet `Get-ServiceFabricDeployedReplicaDetail` tüm çoğaltmaların ilerlemesini belirlemek için kullanılabilir. Sorun çoğaltma ile ayarlanmış kaynaklandığını `LastAppliedReplicationSequenceNumber` değerdir birincil 's `CommittedSequenceNumber` değeri.

- **IReplicator.BuildReplica (<Remote ReplicaId>)**: Bu uyarı oluşturma işlemindeki bir sorun olduğunu gösterir. Daha fazla bilgi için [çoğaltma yaşam döngüsü](service-fabric-concepts-replica-lifecycle.md). Çoğaltıcı adresi yanlış yapılandırma nedeniyle olabilir. Daha fazla bilgi için [durum bilgisi olan Reliable Services özelliğini yapılandırma](service-fabric-reliable-services-configuration.md) ve [bir hizmet bildiriminde kaynakları belirtme](service-fabric-service-manifest-resources.md). Ayrıca, uzak düğümün bir sorun da olabilir.

### <a name="replicator-system-health-reports"></a>Çoğaltma sistem durumu raporlarını
**Çoğaltma kuyruğu dolu:**
**System.Replicator** çoğaltma kuyruğu dolu olduğunda bir uyarı bildirir. Bir veya daha fazla ikincil çoğaltma işlemleri onaylamak yavaş olduğundan, birincil çoğaltma kuyruğu genellikle tam haline gelir. Hizmet işlemleri uygulamak yavaş olduğunda ikincil, bu genellikle gerçekleşir. Sıra dolu olduğunda uyarı temizlenir.

* **SourceId**: System.Replicator
* **Özellik**: **PrimaryReplicationQueueStatus** veya **SecondaryReplicationQueueStatus**çoğaltma rolü bağlı olarak.
* **Sonraki adımlar**: Rapor birincil ise, kümedeki düğümler arasındaki bağlantıyı denetleyin. Tüm bağlantılar sağlıklı olduğunu işlemleri uygulamak için bir yüksek disk gecikme süresi ile en az bir yavaş ikincil olabilir. Rapor ikincil ise, ilk düğümü üzerindeki performans ve disk kullanımını denetleyin. Ardından birincil yavaş düğümünden giden bağlantıyı denetleyin.

**RemoteReplicatorConnectionStatus:**
**System.Replicator** ikincil (uzak) bir çoğaltma bağlantısı iyi durumda olmadığı zaman birincil Çoğaltmada bir uyarı bildirir. Uzak çoğaltıcı'nın adresi, yanlış yapılandırma geçirildiyse veya Çoğaltıcılar arasında ağ sorunları varsa algılamak daha kullanışlı hale getirir raporun iletisinde gösterilir.

* **SourceId**: System.Replicator
* **Özellik**: **RemoteReplicatorConnectionStatus**.
* **Sonraki adımlar**: Hata iletisini denetleyin ve uzak çoğaltıcı adresi doğru şekilde yapılandırıldığından emin olun. Örneğin, "localhost" dinleme adresiyle uzak çoğaltıcı açtıysanız, dışarıdan erişilebilir değildir. Adresi doğru görünüyorsa, birincil düğüm ve olası ağ sorunları bulmak için uzak adres arasındaki bağlantıyı denetleyin.

### <a name="replication-queue-full"></a>Çoğaltma kuyruğu dolu
**System.Replicator** çoğaltma kuyruğu dolu olduğunda bir uyarı bildirir. Bir veya daha fazla ikincil çoğaltma işlemleri onaylamak yavaş olduğundan, birincil çoğaltma kuyruğu genellikle tam haline gelir. Hizmet işlemleri uygulamak yavaş olduğunda ikincil, bu genellikle gerçekleşir. Sıra dolu olduğunda uyarı temizlenir.

* **SourceId**: System.Replicator
* **Özellik**: **PrimaryReplicationQueueStatus** veya **SecondaryReplicationQueueStatus**çoğaltma rolü bağlı olarak.

### <a name="slow-naming-operations"></a>Yavaş işlem adlandırma
**System.NamingService** adlandırma işlemi kabul edilebilir daha uzun sürerse, birincil Çoğaltmada durumu raporları. Adlandırma işlemlerinin örnekler [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) veya [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync). Daha fazla yöntem FabricClient altında bulunabilir. Örneğin, bunlar altında bulunabilir [hizmet yönetimi yöntemlerini](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) veya [özelliği yönetimi yöntemlerini](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).

> [!NOTE]
> Adlandırma Hizmeti hizmet adlarını bir konumu kümedeki çözümler. Kullanıcıların bu hizmet adlarına ve özellikleri yönetmek için kullanabilirsiniz. Bölümlenmiş kalıcı bir Service Fabric hizmeti var. Bölümlerinden temsil *yetki sahibi*, tüm Service Fabric adları ve Hizmetleri hakkındaki meta verileri içerir. Service Fabric adları olarak adlandırılan farklı bölümleri için eşlenen *ad sahibi* bölümleri için genişletilebilir bir hizmettir. Daha fazla bilgi edinin [service adlandırma](service-fabric-architecture.md).
> 
> 

Adlandırma işlemi beklenenden daha uzun sürerse, işlemi bir uyarı raporu adlandırma hizmeti bölümünün işlemi hizmet birincil çoğaltmadaki ile işaretlenir. İşlem başarıyla tamamlanırsa uyarı temizlenir. İşlem bir hata ile tamamlanırsa, sistem durumu raporu hatanın ayrıntılarını içerir.

* **SourceId**: System.NamingService
* **Özellik**: Önek ile başlayan "**Duration_**" ve yavaş işlemi ve işlemin uygulandığı Service Fabric adı tanımlar. Örneğin, hizmet adı oluşturma **fabric: / Uygulamam/Hizmetim** özelliği çok uzun sürer, **Duration_AOCreateService.fabric:/MyApp/MyService**. Bu ad ve işlem için adlandırma bölümün rolü "AO" işaret eder.
* **Sonraki adımlar**: Adlandırma işlem neden başarısız olmadığını denetleyin. Her işlemin farklı kök neden olabilir. Örneğin, hizmet Sil'i takılmış olabilir. Uygulama konağı bir düğümde hizmeti kodunda bir kullanıcı hatası nedeniyle kilitlenme tutar olduğundan hizmet takılmış olabilir.

Aşağıdaki örnek, bir oluşturma hizmeti işlemi gösterilmektedir. İşlemi, yapılandırılan süreden daha uzun sürdü. "AO" yeniden deneme sayısı ve iş "Hayır" olarak gönderir. Zaman AŞIMI ile son işlemi tamamlandı "Hayır". Bu durumda, aynı çoğaltma "AO" ve "Hayır" rolleri için birincil.

```powershell
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

## <a name="deployedapplication-system-health-reports"></a>Sistem durumu raporlarını DeployedApplication
**System.Hosting** dağıtılan varlıklar üzerinde yetkilisidir.

### <a name="activation"></a>Etkinleştirme
Düğüm üzerinde bir uygulama başarıyla etkinleştirildi System.Hosting olarak Tamam bildirir. Aksi takdirde bir hata bildirir.

* **SourceId**: System.Hosting
* **Özellik**: **Etkinleştirme**, ürün sürümü dahil olmak üzere.
* **Sonraki adımlar**: Uygulamanın sağlıksız olduğunu, etkinleştirme başarısız olmasının araştırın.

Aşağıdaki örnek, başarılı bir etkinleştirme gösterir:

```powershell
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
Uygulama paket indirmesi başarısız olursa System.Hosting bir hata bildirir.

* **SourceId**: System.Hosting
* **Özellik**: **İndirme**, ürün sürümü dahil olmak üzere.
* **Sonraki adımlar**: İndirme düğüm üzerinde başarısız olmasının araştırın.

## <a name="deployedservicepackage-system-health-reports"></a>Sistem durumu raporlarını DeployedServicePackage
**System.Hosting** dağıtılan varlıklar üzerinde yetkilisidir.

### <a name="service-package-activation"></a>Hizmet paketi etkinleştirme
System.Hosting Tamam olarak düğümde hizmet paketi etkinleştirme başarılı olup olmadığını bildirir. Aksi takdirde bir hata bildirir.

* **SourceId**: System.Hosting
* **Özellik**: Etkinleştirme.
* **Sonraki adımlar**: Etkinleştirme başarısız olmasının araştırın.

### <a name="code-package-activation"></a>Kod paketi etkinleştirme
System.Hosting, etkinleştirme başarılı olursa, her kod paketi için Tamam bildirir. Etkinleştirme başarısız olursa, yapılandırılan bir uyarı bildirir. Varsa **CodePackage** etkinleştirilemiyor veya yapılandırılmış büyük bir hata ile sona erer **CodePackageHealthErrorThreshold**, barındırma, bir hata bildirir. Hizmet paketi birden çok kod paketleri içeriyorsa, her biri için bir etkinleştirme raporu oluşturulur.

* **SourceId**: System.Hosting
* **Özellik**: Ön ekini kullanan **CodePackageActivation** ve kod paketi ve giriş noktası olarak adını içeren *CodePackageActivation:CodePackageName:SetupEntryPoint / EntryPoint*. Örneğin, **CodePackageActivation:Code:SetupEntryPoint**.

### <a name="service-type-registration"></a>Hizmet türü kayıt
System.Hosting hizmet türü başarıyla kaydedildi, Tamam bildirir. Bir hata bildiriyor, kayıt zamanında kullanılarak yapılandırılan bitti değildi **ServiceTypeRegistrationTimeout**. Çalışma zamanı kapalıysa, hizmet türü düğümden kaydı silinir ve barındırma bir uyarı bildirir.

* **SourceId**: System.Hosting
* **Özellik**: Ön ekini kullanan **ServiceTypeRegistration** ve hizmet türü adı içerir. Örneğin, **ServiceTypeRegistration:FileStoreServiceType**.

Aşağıdaki örnek, bir sorunsuz dağıtılan hizmet paketi gösterir:

```powershell
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
Hizmet paketin indirmesi başarısız olursa System.Hosting bir hata bildirir.

* **SourceId**: System.Hosting
* **Özellik**: **İndirme**, ürün sürümü dahil olmak üzere.
* **Sonraki adımlar**: İndirme düğüm üzerinde başarısız olmasının araştırın.

### <a name="upgrade-validation"></a>Yükseltme doğrulama
System.Hosting, yükseltme sırasında doğrulama başarısız olursa veya düğümde yükseltme başarısız olursa bir hata bildirir.

* **SourceId**: System.Hosting
* **Özellik**: Ön ekini kullanan **FabricUpgradeValidation** ve yükseltme sürümünü içerir.
* **Açıklama**: Karşılaşılan hata işaret eder.

### <a name="undefined-node-capacity-for-resource-governance-metrics"></a>Kaynak İdaresi ölçümler için tanımlanmamış düğüm kapasitesi
System.Hosting düğüm kapasiteleri küme bildiriminde tanımlanan değil ve otomatik algılama yapılandırmasını devre dışı bir uyarı bildirir. Service Fabric başlatır hizmet paketi kullanan her bir sistem durumu uyarı [kaynak İdaresi](service-fabric-resource-governance.md) belirtilen bir düğümde kaydeder.

* **SourceId**: System.Hosting
* **Özellik**: **ResourceGovernance**.
* **Sonraki adımlar**: Bu sorunu çözmek için tercih edilen yol kullanılabilir kaynakları otomatik olarak algılanmasını etkinleştirmek için küme bildirimine değiştirmektir. İle Bu ölçümler için doğru şekilde belirtilen düğüm kapasiteleri gibi küme bildiriminin güncelleştirme başka bir yoludur.

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric sistem durumu raporlarını görüntüleme](service-fabric-view-entities-aggregated-health.md)

* [Hizmet durumunu raporlama ve denetleme](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

* [Hizmetleri yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

* [Service Fabric uygulaması yükseltme](service-fabric-application-upgrade.md)

