---
title: Azure Service Fabric'e otomatik ölçeklendirme Hizmetleri ve kapsayıcıları | Microsoft Docs
description: Azure Service Fabric, otomatik ölçeklendirme Hizmetleri ve kapsayıcıları için ilkeler ayarlamanızı sağlar.
services: service-fabric
documentationcenter: .net
author: radicmilos
manager: ''
editor: nipuzovi
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/17/2018
ms.author: miradic
ms.openlocfilehash: 8e57c071c9fd93a8581d574aeec2b23b38b3ab95
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60844032"
---
# <a name="introduction-to-auto-scaling"></a>Otomatik ölçeklendirme giriş
Otomatik ölçeklendirme Hizmetleri raporladığınız ya da kendi kaynak kullanımını temel alarak yük hizmetlerinizi dinamik olarak ölçeklendirmeniz mi ek bir Service fabric özelliktir. Otomatik ölçeklendirme, büyük esneklik sağlar ve ek örnekler ya da isteğe bağlı hizmetinizin bölümler sağlama sağlar. Tüm otomatik ölçeklendirme işlemi, otomatik ve şeffaf ve bir hizmette ilkelerinizi ayarladıktan sonra hizmet düzeyinde el ile ölçeklendirme işlemleri için gerek yoktur. Otomatik ölçeklendirme üzerinde hizmet oluşturma zamanında veya herhangi bir zamanda hizmet güncelleştirerek kapatılabilir.

Otomatik ölçeklendirme kullanışlı olduğu bir yaygın bir senaryo, belirli bir hizmet üzerindeki yükü zamanla değişir andır. Örneğin, bir ağ geçidi ölçeklendirebilirsiniz gibi bir hizmet gelen istekleri işlemek için gerekli kaynaklar miktarına göre. Ölçeklendirme kuralları aşağıdaki gibi görünebilir örneği bir göz atalım:
* Ağ tüm örneklerini ikiden fazla çekirdek ortalama kullanıyorsanız, ağ geçidi hizmetini bir daha fazla örnek ekleyerek ölçeklendirin. Her saat bunu ancak hiçbir zaman toplam sekizden fazla örneğe sahip.
* Ağ tüm örneklerini az 0,5 çekirdek ortalama kullanıyorsanız, hizmette bir örnek kaldırarak ölçeklendirin. Her saat bunu ancak hiçbir zaman toplam üçten daha az sayıda örneğe sahip.

Otomatik ölçeklendirme, kapsayıcılar ve normal bir Service Fabric Hizmetleri için desteklenir. Otomatik ölçeklendirme kullanmak için 6.2 sürümü veya üzeri çalıştırması gerekir Service Fabric çalışma zamanı. 

Bu makalenin geri kalanında, yolları etkinleştirmek veya otomatik ölçeklendirme, devre dışı bırakmak için ölçeklendirme ilkeleri tanımlar ve bu özelliği kullanmak örnekler sağlar.

## <a name="describing-auto-scaling"></a>Otomatik ölçeklendirme açıklayan
Bir Service Fabric kümesindeki her bir hizmet için otomatik ölçeklendirme ilkeleri tanımlanabilir. Her bir ölçeklendirme İlkesi iki bölümden oluşur:
* **Tetikleyici ölçeklendirme** hizmetini ölçekleme ne zaman gerçekleştirilecek açıklar. Tetikleyicinin içinde tanımlanmış koşullar, bir hizmet veya daraltılacağı olmadığını belirlemek için düzenli olarak denetlenir.

* **Ölçeklendirme mekanizması** tetiklendiğinde nasıl ölçekleme gerçekleştirileceğini açıklar. Tetikleyici koşulları karşılandığında mekanizması yalnızca uygulanır.

Şu anda desteklenen tüm tetikleyiciler ile birlikte çalışma [mantıksal yükleme ölçümleri](service-fabric-cluster-resource-manager-metrics.md), veya CPU veya bellek kullanımı gibi fiziksel ölçümlerle. Her iki durumda da, Service Fabric ölçüm için bildirilen yük izler ve düzenli aralıklarla ölçeklendirme gerekip gerekmediğini belirlemek için tetikleyici olarak değerlendirilir.

Şu anda otomatik ölçekleme için desteklenen iki mekanizma vardır. İlk otomatik ölçeklendirme gerçekleştirildiği ekleyerek veya kaldırarak kapsayıcılar veya durum bilgisi olmayan hizmetler için tasarlanmıştır [örnekleri](service-fabric-concepts-replica-lifecycle.md). Durum bilgisi olan ve durum bilgisi olmayan hizmetler için otomatik ölçeklendirme da ekleyerek gerçekleştirilebilir veya kaldırma adlı [bölümleri](service-fabric-concepts-partitioning.md) hizmeti.

> [!NOTE]
> Şu anda hizmet başına yalnızca bir ölçeklendirme İlkesi ve ölçeklendirme İlkesi başına yalnızca bir ölçeklendirme tetikleyici desteği yoktur.

## <a name="average-partition-load-trigger-with-instance-based-scaling"></a>Ortalama bölüm yük tetikleyici bağlı olarak örnek ölçeklendirme
İlk tetikleyici türünü bir durum bilgisi olmayan hizmet bölümdeki örneklerinin yük temel alır. Ölçüm yükleri bir bölüm, her örneği için yük almak için ilk düzleştirilmiş ve ardından bu değerleri bölüm tüm örneklerinde ortalaması alınır. Hizmet zaman ölçeklendirileceği belirlemek üç faktöre vardır:

* _Alt yükleme eşiği_ hizmeti ne zaman olacağını belirleyen bir değer **ölçeği**. Ortalama yük tüm örneklerinin bölüm bu değerden düşükse, hizmet olarak ayarlanacaktır.
* _Üst yükleme eşiği_ hizmeti ne zaman olacağını belirleyen bir değer **ölçeği**. Ortalama yük tüm örneklerinin bölüm bu değerden daha yüksek ise, hizmetin kullanıma ayarlanacaktır.
* _Ölçeklendirme aralığı_ tetikleyici ne sıklıkla kontrol edileceği belirler. Ölçeklendirme gerekliyse tetikleyici iade edildikten sonra mekanizması uygulanır. Ölçeklendirme gerekmiyorsa, hiçbir işlem yapılmadı. Yeniden ölçeklendirme aralığı sona ermeden önce her iki durumda da, tetikleyici yeniden denetlenmez.

Bu tetikleyici yalnızca durum bilgisi olmayan hizmetler ile (durum bilgisi olmayan kapsayıcılar veya Service Fabric Hizmetleri) kullanılabilir. Ne zaman bir hizmet birden çok bölüm olması durumunda, tetikleyici her bölüm için ayrı olarak değerlendirilir ve her bölüm bağımsız olarak uygulanan belirtilen mekanizmasına sahip olur. Bu nedenle, bu durumda, bazı hizmet bölümlerini kullanıma ölçeklendirileceği, içindeki bazı ölçeğinin azaltılıp ve bazı hiç aynı zamanda, kendi iş yüküne göre ölçeği olmaz mümkündür.

Bu tetikleyici ile kullanılabilecek tek PartitionInstanceCountScaleMechanism mekanizmadır. Bu mekanizma nasıl uygulanacağını belirlemek üç faktöre vardır:
* _Ölçeği artırma_ kaç tane eklenecek veya mekanizması tetiklendiğinde kaldırıldı belirler.
* _En fazla örnek sayısı_ ölçeklendirmeye yönelik üst sınır tanımlar. Örneklerinin bölüm sayısı bu sınırı ulaşırsa, ardından hizmeti, bağımsız olarak yük ölçeklenmez. -1 değerini belirterek bu sınırı çıkarın ve hizmetin çalışması ölçeği, mümkün olduğunca (sınır kümedeki kullanılabilir düğüm sayısını yoktur) out mümkündür.
* _En az örnek sayısı_ ölçeklendirme için alt sınırı tanımlar. Örneklerinin bölüm sayısı bu sınırı ulaşırsa, ardından hizmeti, bağımsız olarak yük ölçeklenmez.

## <a name="setting-auto-scaling-policy"></a>Otomatik ölçeklendirme İlkesi ayarlama

### <a name="using-application-manifest"></a>Uygulama bildirimi kullanarak
``` xml
<LoadMetrics>
<LoadMetric Name="MetricB" Weight="High"/>
</LoadMetrics>
<ServiceScalingPolicies>
<ScalingPolicy>
    <AveragePartitionLoadScalingTrigger MetricName="MetricB" LowerLoadThreshold="1" UpperLoadThreshold="2" ScaleIntervalInSeconds="100"/>
    <InstanceCountScalingMechanism MinInstanceCount="3" MaxInstanceCount="4" ScaleIncrement="1"/>
</ScalingPolicy>
</ServiceScalingPolicies>
```
### <a name="using-c-apis"></a>C# API'lerini kullanma
```csharp
FabricClient fabricClient = new FabricClient();
StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
//set up the rest of the ServiceDescription
AveragePartitionLoadScalingTrigger trigger = new AveragePartitionLoadScalingTrigger();
PartitionInstanceCountScaleMechanism mechanism = new PartitionInstanceCountScaleMechanism();
mechanism.MaxInstanceCount = 3;
mechanism.MinInstanceCount = 1;
mechanism.ScaleIncrement = 1;
trigger.MetricName = "servicefabric:/_CpuCores";
trigger.ScaleInterval = TimeSpan.FromMinutes(20);
trigger.LowerLoadThreshold = 1.0;
trigger.UpperLoadThreshold = 2.0;
ScalingPolicyDescription policy = new ScalingPolicyDescription(mechanism, trigger);
serviceDescription.ScalingPolicies.Add(policy);
//as we are using scaling on a resource this must be exclusive service
//also resource monitor service needs to be enabled
serviceDescription.ServicePackageActivationMode = ServicePackageActivationMode.ExclusiveProcess
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```
### <a name="using-powershell"></a>PowerShell'i kullanma
```posh
$mechanism = New-Object -TypeName System.Fabric.Description.PartitionInstanceCountScaleMechanism
$mechanism.MinInstanceCount = 1
$mechanism.MaxInstanceCount = 6
$mechanism.ScaleIncrement = 2
$trigger = New-Object -TypeName System.Fabric.Description.AveragePartitionLoadScalingTrigger
$trigger.MetricName = "servicefabric:/_CpuCores"
$trigger.LowerLoadThreshold = 0.3
$trigger.UpperLoadThreshold = 0.8
$trigger.ScaleInterval = New-TimeSpan -Minutes 10
$scalingpolicy = New-Object -TypeName System.Fabric.Description.ScalingPolicyDescription
$scalingpolicy.ScalingMechanism = $mechanism
$scalingpolicy.ScalingTrigger = $trigger
$scalingpolicies = New-Object 'System.Collections.Generic.List[System.Fabric.Description.ScalingPolicyDescription]'
$scalingpolicies.Add($scalingpolicy)
#as we are using scaling on a resource this must be exclusive service
#also resource monitor service needs to be enabled
Update-ServiceFabricService -Stateless -ServiceName "fabric:/AppName/ServiceName" -ScalingPolicies $scalingpolicies
```

## <a name="average-service-load-trigger-with-partition-based-scaling"></a>Ortalama hizmet yük tetikleyici tabanlı bölüm ölçeklendirme
İkinci tetikleyici, bir hizmetin tüm bölümlerin yükünü alır. Ölçüm yükleri, her çoğaltma veya örnek bir bölümü için yük almak için ilk düzleştirilmiş. Durum bilgisi olan hizmetler için yük bölümün durum bilgisi olmayan hizmetler için ortalama yük tüm örneklerinin bölüm bölümün yük olsa birincil çoğaltmanın yükünü olarak değerlendirilir. Bu değerler, hizmetin tüm bölümler ortalaması alınır ve bu değer, otomatik ölçeklendirmeyi tetiklemek için kullanılır. Önceki mekanizması olduğu gibi aynı hizmet ölçeklendirildiğinde belirleyen üç faktör vardır:

* _Alt yükleme eşiği_ hizmeti ne zaman olacağını belirleyen bir değer **ölçeği**. Ortalama yük hizmetinin tüm bölümlerin bu değerden düşükse, hizmet olarak ayarlanacaktır.
* _Üst yükleme eşiği_ hizmeti ne zaman olacağını belirleyen bir değer **ölçeği**. Ortalama yük hizmetinin tüm bölümlerin bu değerden daha yüksek ise, hizmetin kullanıma ayarlanacaktır.
* _Ölçeklendirme aralığı_ tetikleyici ne sıklıkla kontrol edileceği belirler. Ölçeklendirme gerekliyse tetikleyici iade edildikten sonra mekanizması uygulanır. Ölçeklendirme gerekmiyorsa, hiçbir işlem yapılmadı. Yeniden ölçeklendirme aralığı sona ermeden önce her iki durumda da, tetikleyici yeniden denetlenmez.

Bu tetikleyiciyi olabilir hem de durum bilgisi olan ve olmayan Hizmetleri ile kullanılır. Bu tetikleyici ile kullanılabilecek tek AddRemoveIncrementalNamedPartitionScalingMechanism mekanizmadır. Yeni bir bölüm eklendikten sonra hizmeti kullanıma ölçeklenir ve hizmet var olan bölümleri birinde ölçeklendirildiğinde kaldırılır. Hizmet oluşturulduğunda veya güncelleştirildiğinde ve hizmet oluşturma/güncelleştirme başarısız olur, bu koşullar karşılanmazsa denetlenecek kısıtlamaları vardır:
* Adlandırılmış bölüm düzeni, hizmet için kullanılmalıdır.
* Bölüm adları olmalıdır ardışık tamsayı numaraları gibi "0", "1"...
* İlk bölüm adı "0" olmalıdır.

Bir hizmet ilk üç bölümlü oluşturduysanız, örneğin, yalnızca geçerli bölüm adları için "0", "1" ve "2" bir olasılıktır.

Gerçek otomatik ölçeklendirme, gerçekleştirilen işlemleri de bu adlandırma şeması uyar:
* Hizmet, geçerli bölümlerini "0", "1" ve "2" adlı, Ölçek genişletme için eklenecek bölüm "3" şeklinde ilerler.
* Hizmet, geçerli bölümlerini "0", "1" ve "2" adlı, ölçeklendirme için kaldırılacak bölüm adı "2" bölümüne yoktur.

Kullanan örnekleri ekleyerek veya kaldırarak tarafından ölçeklendirme mekanizması olarak ile aynı, bu mekanizma nasıl uygulanacağını belirleyen üç parametre vardır:
* _Ölçeği artırma_ kaç bölümleri eklendi veya mekanizması tetiklendiğinde kaldırıldı belirler.
* _En yüksek bölüm sayısı_ ölçeklendirmeye yönelik üst sınır tanımlar. Hizmet bölüm sayısı bu sınırı ulaşırsa, ardından hizmeti, bağımsız olarak yük ölçeklenmez. -1 değerini belirterek bu sınırı çıkarın ve hizmetin çalışması ölçeği, mümkün olduğu kadar (kümeyi gerçek kapasitesi sınırı kadar) out mümkündür.
* _En az örnek sayısı_ ölçeklendirme için alt sınırı tanımlar. Hizmet bölüm sayısı bu sınırı ulaşırsa, ardından hizmeti, bağımsız olarak yük ölçeklenmez.

> [!WARNING] 
> Durum bilgisi olan hizmetlerle AddRemoveIncrementalNamedPartitionScalingMechanism kullanıldığında, Service Fabric ekleme veya kaldırma bölümlerini **bildirim veya uyarısı olmadan**. Ölçeklendirme mekanizması tetiklendiğinde verilerin yeniden bölümlenmesi gerçekleştirilmeyecek. Durumda ölçeği artırma işlemi, yeni bölümler boş olur ve ölçeği azaltma işlemi durumunda **bölüm içerdiği tüm verilerle birlikte silinecek**.

## <a name="setting-auto-scaling-policy"></a>Otomatik ölçeklendirme İlkesi ayarlama

### <a name="using-application-manifest"></a>Uygulama bildirimi kullanarak
``` xml
<ServiceScalingPolicies>
    <ScalingPolicy>
        <AverageServiceLoadScalingTrigger MetricName="servicefabric:/_MemoryInMB" LowerLoadThreshold="300" UpperLoadThreshold="500" ScaleIntervalInSeconds="600"/>
        <AddRemoveIncrementalNamedPartitionScalingMechanism MinPartitionCount="1" MaxPartitionCount="3" ScaleIncrement="1"/>
    </ScalingPolicy>
</ServiceScalingPolicies>
```
### <a name="using-c-apis"></a>C# API'lerini kullanma
```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceUpdateDescription serviceUpdate = new StatefulServiceUpdateDescription();
AveragePartitionLoadScalingTrigger trigger = new AverageServiceLoadScalingTrigger();
PartitionInstanceCountScaleMechanism mechanism = new AddRemoveIncrementalNamedPartitionScalingMechanism();
mechanism.MaxPartitionCount = 4;
mechanism.MinPartitionCount = 1;
mechanism.ScaleIncrement = 1;
//expecting that the service already has metric NumberOfConnections
trigger.MetricName = "NumberOfConnections";
trigger.ScaleInterval = TimeSpan.FromMinutes(15);
trigger.LowerLoadThreshold = 10000;
trigger.UpperLoadThreshold = 20000;
ScalingPolicyDescription policy = new ScalingPolicyDescription(mechanism, trigger);
serviceUpdate.ScalingPolicies = new List<ScalingPolicyDescription>;
serviceUpdate.ScalingPolicies.Add(policy);
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), serviceUpdate);
```
### <a name="using-powershell"></a>PowerShell'i kullanma
```posh
$mechanism = New-Object -TypeName System.Fabric.Description.AddRemoveIncrementalNamedPartitionScalingMechanism
$mechanism.MinPartitionCount = 1
$mechanism.MaxPartitionCount = 3
$mechanism.ScaleIncrement = 2
$trigger = New-Object -TypeName System.Fabric.Description.AverageServiceLoadScalingTrigger
$trigger.MetricName = "servicefabric:/_MemoryInMB"
$trigger.LowerLoadThreshold = 5000
$trigger.UpperLoadThreshold = 10000
$trigger.ScaleInterval = New-TimeSpan -Minutes 25
$scalingpolicy = New-Object -TypeName System.Fabric.Description.ScalingPolicyDescription
$scalingpolicy.ScalingMechanism = $mechanism
$scalingpolicy.ScalingTrigger = $trigger
$scalingpolicies = New-Object 'System.Collections.Generic.List[System.Fabric.Description.ScalingPolicyDescription]'
$scalingpolicies.Add($scalingpolicy)
#as we are using scaling on a resource this must be exclusive service
#also resource monitor service needs to be enabled
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -TargetReplicaSetSize 3 -MinReplicaSetSize 2 -HasPersistedState true -PartitionNames @("0","1") -ServicePackageActivationMode ExclusiveProcess -ScalingPolicies $scalingpolicies
```

## <a name="auto-scaling-based-on-resources"></a>Kaynaklar temelinde otomatik ölçeklendirme

Kaynak İzleyicisi hizmeti ölçeklendirmek etkinleştirmek için gerçek kaynaklar temelinde.

``` json
"fabricSettings": [
...      
],
"addonFeatures": [
    "ResourceMonitorService"
],
```
Gerçek fiziksel kaynakları temsil eden iki ölçüm vardır. Bunlardan biri olan servicefabric: / Gerçek cpu kullanımı (yarı çekirdek 0,5 gösterir şekilde) ve diğer temsil eden _CpuCores olan servicefabric: / MB bellek kullanımında temsil eden _MemoryInMB.
ResourceMonitorService kullanıcı Hizmetleri cpu ve bellek kullanımını izlemekten sorumludur. Ağırlıklı hareketli ortalama, bu hizmet için olası kısa süreli ani hesap için geçerli olur. Kaynak İzleme kapsayıcılı ve kapsayıcılı olmayan uygulamaları Windows ve Linux'ta kapsayıcılı olanlar için desteklenir. Otomatik ölçeklendirme kaynakları, hizmetleri etkin için yalnızca etkin [özel işlem modeli](service-fabric-hosting-model.md#exclusive-process-model).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [uygulama ölçeklenebilirlik](service-fabric-concepts-scalability.md).
