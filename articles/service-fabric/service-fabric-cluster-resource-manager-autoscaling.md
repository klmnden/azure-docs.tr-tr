---
title: Azure Service Fabric otomatik ölçeklendirme Hizmetleri ve kapsayıcıları | Microsoft Docs
description: Azure Service Fabric, hizmetleri ve kapsayıcıları için ilkeler ölçeklendirme otomatik ayarlamanızı sağlar.
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
ms.openlocfilehash: cd19c0e51ca1ac7863058d7c3944400719508f9b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34213206"
---
# <a name="introduction-to-auto-scaling"></a>Otomatik ölçeklendirme giriş
Otomatik ölçeklendirme hizmetlerinizi Hizmetleri raporlama olan ya da kendi kaynak kullanımına bağlı yükü göre dinamik olarak ölçeklendirmek için ek bir Service Fabric, yetenektir. Otomatik ölçeklendirme büyük esneklik sağlar ve ek örnekler ya da isteğe bağlı hizmet bölümlerini sağlama sağlar. Tüm otomatik ölçekleme işlemi otomatik ve şeffaf ve bir hizmet ilkelerinizi ayarladıktan sonra hizmet düzeyinde el ile ölçeklendirme işlemleri için gerek yoktur. Otomatik ölçeklendirme üzerinde hizmet oluşturma zamanında veya herhangi bir anda hizmeti güncelleştirerek açılabilir.

Otomatik ölçeklendirme yararlı olduğu yaygın bir senaryo, belirli bir hizmet üzerindeki yükü zaman içinde değişir durumdur. Örneğin, bir ağ geçidi ölçeklendirebilirsiniz gibi bir hizmet gelen istekleri işlemek için gerekli kaynaklar miktarına göre. Bu ölçeklendirme kurallar gibi görünebilir örneği bir göz atalım:
* My ağ geçidi tüm örneklerini ikiden fazla çekirdek ortalama kullanıyorsanız, ağ geçidi hizmeti çıkışı bir daha fazla örneğini ekleyerek ölçeklendirin. Her saat bunu, ancak hiçbir zaman birden fazla yedi örnekleri toplam yoktur.
* My ağ geçidi tüm örneklerini az 0,5 çekirdek ortalama kullanıyorsanız hizmetinde bir örnek kaldırarak ölçeklendirin. Her saat bunu, ancak hiçbir zaman toplam üçten örneğe sahip.

Otomatik ölçeklendirme kapsayıcıları ve normal Service Fabric Hizmetleri için desteklenir. Bu makalenin geri kalanında yolları etkinleştirmek veya otomatik ölçeklendirmeyi, devre dışı bırakmak için ölçeklendirme ilkelerini açıklar ve bu özelliği kullanmak hakkında örnekler verilmektedir.

## <a name="describing-auto-scaling"></a>Otomatik ölçeklendirme açıklayan
İlkeleri ölçeklendirme otomatik bir Service Fabric kümesindeki her bir hizmet için tanımlanabilir. Her bir ölçeklendirme İlkesi iki bölümden oluşur:
* **Tetikleyici ölçeklendirme** hizmetini ölçeklendirme ne zaman gerçekleştirilecek açıklar. Tetikleyici içinde tanımlanmış koşullar, bir hizmet veya genişletilip varsa belirlemek için düzenli olarak denetlenir.

* **Mekanizması ölçeklendirme** onu tetiklendiğinde nasıl ölçeklendirme gerçekleştirilir açıklar. Tetikleyici koşulları sağlandığında mekanizması yalnızca uygulanır.

Şu anda desteklenen tüm tetikleyicileri ile ya da iş [mantıksal yük ölçümleri](service-fabric-cluster-resource-manager-metrics.md), veya CPU veya bellek kullanımı gibi fiziksel ölçümlerle. Her iki durumda da, Service Fabric ölçümü için bildirilen yük izler ve düzenli aralıklarla ölçeklendirme gerekip gerekmediğini belirlemek için tetikleyici değerlendirilir.

Otomatik ölçeklendirme için şu anda desteklenen iki mekanizma vardır. Birinci kapsayıcıları burada otomatik ölçeklendirme gerçekleştirilir ekleyerek veya kaldırarak veya durum bilgisi olmayan hizmetler için tasarlanmıştır [örnekleri](service-fabric-concepts-replica-lifecycle.md). Durum bilgisi olan ve durum bilgisi olmayan hizmetler için otomatik ölçeklendirme de ekleyerek veya gerçekleştirilebilir kaldırma adlı [bölümleri](service-fabric-concepts-partitioning.md) hizmetinin.

> [!NOTE]
> Şu anda yalnızca bir ölçeklendirme İlkesi her hizmet için desteği yoktur.

## <a name="average-partition-load-trigger-with-instance-based-scaling"></a>Ortalama bölüm yükü tetikleyici dayanan örnek ölçeklendirme
İlk tür tetikleyici örneklerinin bir durum bilgisi olmayan bölüme yük temel alır. Ölçüm yükleri bir bölüm, her örneği için yük almak için önce düzleştirilmiş ve ardından bu değerleri bölüm tüm örneklerinde ortalaması alınır. Hizmet zaman ölçeklendirilir belirlemek üç faktöre vardır:

* _Alt yükleme eşiği_ hizmetin ne zaman olacağını belirleyen bir değer **içinde ölçeklendirilmiş**. Tüm bölümleri örneklerinin ortalama yük bu değerden daha düşük ise, hizmet olarak ölçeklendirilir.
* _Üst yük eşiği_ hizmetin ne zaman olacağını belirleyen bir değer **çıkışı ölçeklendirilmiş**. Bölüm tüm örneklerinin ortalama yük bu değerden daha düşük olması durumunda, hizmet çıkışı ölçeklendirilir.
* _Ölçeklendirme aralığı_ tetikleyici ne sıklıkta denetlenecek belirler. Ölçeklendirme gerekirse tetikleyici iade edildikten sonra mekanizması uygulanır. Ölçeklendirme gerekmiyorsa, hiçbir eylem ulaşabilirsiniz. Ölçeklendirme aralığı yeniden süresi dolmadan önce her iki durumda da, tetikleyici yeniden denetlenmez.

Bu tetikleyici yalnızca durum bilgisi olmayan hizmetler ile (durum bilgisiz kapsayıcıları veya Service Fabric Hizmetleri) kullanılabilir. Bir hizmet birden çok bölüm olduğunda durumunda, tetikleyici her bölüm için ayrı olarak değerlendirilir ve her bölüm için bağımsız olarak uygulanan belirtilen mekanizması gerekir. Bu nedenle, bu durumda, bazı hizmet bölümlerini kullanıma ölçeklendirilir, bazı içinde ölçeklendirilir ve bazı hiç aynı anda kendi yüküne göre ölçeklendirilmesi olmaz mümkündür.

Bu tetikleyici ile kullanılabilen yalnızca PartitionInstanceCountScaleMechanism mekanizmadır. Bu mekanizma nasıl uygulanacağını belirleyen üç faktöre vardır:
* _Ölçeği artırma_ kaç tane örnek eklenir veya mekanizması tetiklendiğinde kaldırılır belirler.
* _En fazla örnek sayısı_ ölçekleme için üst sınır tanımlar. Bölümün örnek sayısı bu sınırı ulaşırsa, ardından hizmeti, bağımsız olarak yük ölçeklenmez. Bu sınır -1 değeri belirterek atlayın ve durum hizmet ölçeklendirilir, mümkün olduğunca (kümede bulunan düğüm sayısı sınırı:) out mümkündür.
* _En düşük örnek sayısı_ ölçeklendirmeye yönelik alt sınır tanımlar. Bölümün örnek sayısı bu sınırı ulaşırsa, ardından hizmeti, bağımsız olarak yük ölçeklenmez.

## <a name="setting-auto-scaling-policy"></a>Otomatik ölçeklendirme İlkesi ayarlama

### <a name="using-application-manifest"></a>Uygulama bildirimini kullanma
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
### <a name="using-c-apis"></a>C# API'lerini kullanarak
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

## <a name="average-service-load-trigger-with-partition-based-scaling"></a>Ortalama hizmet yük tetikleyici bölüm göre ölçeklendirme
İkinci tetikleyici bir hizmetin tüm bölümlerin yükü temel alır. Ölçüm yükleri yük her çoğaltma veya bir bölümünün bir örneği elde etmek için ilk düzleştirilmiş. Durum bilgisi olan hizmetler için bölüm yükünü durum bilgisi olmayan hizmetler için yük bölümün bölüm tüm örneklerinin ortalama yük olsa da birincil çoğaltma yükünü olarak kabul edilir. Bu değerleri hizmetinin tüm bölümler ortalaması alınır ve bu değeri otomatik ölçeklendirme tetiklemek için kullanılır. Önceki mekanizması olduğu gibi aynı hizmet ölçeklendirilir gerektiğini belirleyen üç etken vardır:

* _Alt yükleme eşiği_ hizmetin ne zaman olacağını belirleyen bir değer **içinde ölçeklendirilmiş**. Ortalama yük hizmetinin tüm bölümlerin bu değerden daha düşük ise, hizmet olarak ölçeklendirilir.
* _Üst yük eşiği_ hizmetin ne zaman olacağını belirleyen bir değer **çıkışı ölçeklendirilmiş**. Ortalama yük hizmetinin tüm bölümlerin bu değerden daha düşük ise, hizmet çıkışı ölçeklendirilir.
* _Ölçeklendirme aralığı_ tetikleyici ne sıklıkta denetlenecek belirler. Ölçeklendirme gerekirse tetikleyici iade edildikten sonra mekanizması uygulanır. Ölçeklendirme gerekmiyorsa, hiçbir eylem ulaşabilirsiniz. Ölçeklendirme aralığı yeniden süresi dolmadan önce her iki durumda da, tetikleyici yeniden denetlenmez.

Bu tetikleyici olabilir hem durum bilgisi olan ve durum bilgisiz Hizmetleri ile kullanılır. Bu tetikleyici ile kullanılabilen yalnızca AddRemoveIncrementalNamedParitionScalingMechanism mekanizmadır. Yeni bir bölüm eklendikten sonra hizmet çıkışı ölçeklenir ve hizmet varolan bölümleri birinde ölçeklendirilir zamanı kaldırılır. Hizmet oluşturulmuş veya güncelleştirilmiş ve bu koşullar karşılanmazsa hizmet oluşturma/güncelleştirme yapamaz denetlenir kısıtlamaları vardır:
* Adlandırılmış bölüm düzeni hizmet için kullanılması gerekir.
* Bölüm adları ardışık tamsayı sayı olmalıdır, olduğu gibi "0", "1",...
* İlk bölüm adı "0" olmalıdır.

Örneğin, bir hizmet başlangıçta ile üç bölüm oluşturduysanız, yalnızca geçerli olasılığı bölüm adları için "0", "1" ve "2" olur.

Gerçekleştirilen işlem ölçeklendirme gerçek otomatik de bu adlandırma şeması uyar:
* Hizmetin geçerli bölümleri "0", "1" ve "2" adında, ölçeğini için eklenecek bölüm "3" adlandırılacaktır.
* Geçerli hizmet bölümlerini "0", "1" ve "2" adında, ardından içinde ölçekleme için kaldırılacak bölüm "2" adıyla bölümdür.

Aynı ekleyerek veya kaldırarak örnekleri ölçeklendirme kullanan mekanizması olarak ile Bu mekanizma nasıl uygulanacağını belirleyen üç parametre vardır:
* _Ölçeği artırma_ kaç bölümleri eklenemez veya mekanizması tetiklendiğinde kaldırılamaz belirler.
* _En fazla bölüm sayısı_ ölçekleme için üst sınır tanımlar. Hizmet bölüm sayısı bu sınırı ulaşırsa, ardından hizmeti, bağımsız olarak yük ölçeklenmez. Bu sınır -1 değeri belirterek atlayın ve durum hizmet ölçeklendirilir, mümkün olduğunca (küme gerçek kapasitesini sınır yoktur) out mümkündür.
* _En düşük örnek sayısı_ ölçeklendirmeye yönelik alt sınır tanımlar. Hizmet bölüm sayısı bu sınırı ulaşırsa, ardından hizmeti, bağımsız olarak yük ölçeklenmez.

## <a name="setting-auto-scaling-policy"></a>Otomatik ölçeklendirme İlkesi ayarlama

### <a name="using-application-manifest"></a>Uygulama bildirimini kullanma
``` xml
<ServiceScalingPolicies>
    <ScalingPolicy>
        <AverageServiceLoadScalingTrigger MetricName="servicefabric:/_MemoryInMB" LowerLoadThreshold="300" UpperLoadThreshold="500" ScaleIntervalInSeconds="600"/>
        <AddRemoveIncrementalNamedParitionScalingMechanism MinPartitionCount="1" MaxPartitionCount="3" ScaleIncrement="1"/>
    </ScalingPolicy>
</ServiceScalingPolicies>
```
### <a name="using-c-apis"></a>C# API'lerini kullanarak
```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceUpdateDescription serviceUpdate = new StatefulServiceUpdateDescription();
AveragePartitionLoadScalingTrigger trigger = new AverageServiceLoadScalingTrigger();
PartitionInstanceCountScaleMechanism mechanism = new AddRemoveIncrementalNamedParitionScalingMechanism();
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
$mechanism = New-Object -TypeName System.Fabric.Description.AddRemoveIncrementalNamedParitionScalingMechanism
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

## <a name="auto-scaling-based-on-resources"></a>Otomatik ölçeklendirme kaynaklara bağlı

Ölçeklendirmek kaynak izleme hizmeti etkinleştirmek için gerçek kaynaklara bağlı

``` json
"fabricSettings": [
...      
],
"addonFeatures": [
    "ResourceMonitorService"
],
```
Gerçek fiziksel kaynakları temsil eden iki ölçüm vardır. Bunlardan biri olan servicefabric: / (yarım çekirdek 0,5 gösterir şekilde) gerçek cpu kullanımı ve diğer temsil eden _CpuCores olan servicefabric: / MB bellek kullanımında temsil eden _MemoryInMB.
ResourceMonitorService kullanıcı Hizmetleri cpu ve bellek kullanımını izlemek için sorumludur. Bu hizmet ağırlıklı hareketli ortalama için olası kısa süreli ani hesabı için geçerli olur. Kaynak İzleme kapsayıcılı ve kapsayıcılı olmayan uygulamalar Windows ve Linux üzerinde kapsayıcılı olanlar için desteklenir. Kaynakları ölçeklendirme otomatik olarak hizmetler etkinleştirilmiş için yalnızca etkin [özel işlem modeli](service-fabric-hosting-model.md#exclusive-process-model).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [uygulama ölçeklenebilirlik](service-fabric-concepts-scalability.md).