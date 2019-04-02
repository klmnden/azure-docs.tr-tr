---
title: Uygulama yükseltme sorunlarını giderme | Microsoft Docs
description: Bu makalede, Service Fabric uygulaması ve bunların nasıl çözüleceğine yükseltme etrafında bazı yaygın sorunlar yer almaktadır.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: 19ad152e-ec50-4327-9f19-065c875c003c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: e393eb92e11dc8dc296f1dc5f1c0036566c285c5
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58792459"
---
# <a name="troubleshoot-application-upgrades"></a>Uygulama yükseltme ile ilgili sorunları giderme

Bu makalede bir Azure Service Fabric uygulama ve bunların nasıl çözüleceğine yükseltme etrafında yaygın sorunlardan bazılarını ele alınmıştır.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Başarısız uygulama yükseltme sorunlarını giderme

Yükseltme başarısız olduğunda, çıktısını **Get-ServiceFabricApplicationUpgrade** komut hatası hata ayıklama için ek bilgiler içerir.  Aşağıdaki listede, ek bilgilerin nasıl kullanılabileceğini belirtir:

1. Hata türü tanımlar.
2. Hatanın nedenini belirleyin.
3. Daha fazla araştırma için bir veya daha fazla başarısız olan bileşenleri yalıtın.

Service Fabric bağımsız olarak hatası algıladığında, bu bilgi kullanılabilir **FailureAction** geri alma veya yükseltme askıya alın.

### <a name="identify-the-failure-type"></a>Hata türünü tanımlayın

Çıktısındaki **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** başlangıçtan bir yükseltme hatası Service Fabric tarafından algılandığı zaman damgası (UTC) tanımlar ve  **FailureAction** tetiklendi. **FailureReason** hata üst düzey üç olası nedenlerden biri tanımlar:

1. UpgradeDomainTimeout - gösteren belirli bir yükseltme etki alanı tamamlanması çok uzun sürdü ve **UpgradeDomainTimeout** süresi doldu.
2. OverallUpgradeTimeout - gösteren genel yükseltmenin tamamlanması çok uzun sürdü ve **UpgradeTimeout** süresi doldu.
3. HealthCheck - bir güncelleştirme etki alanına yükselttikten sonra uygulamanın belirtilen sistem ilkelerine göre sağlıksız kaldığını olduğunu gösterir ve **HealthCheckRetryTimeout** süresi doldu.

Yükseltme başarısız olur ve geri alma başlar bu girdiler yalnızca çıktıda gösterilir. Daha fazla bilgi, hatanın türüne bağlı olarak görüntülenir.

### <a name="investigate-upgrade-timeouts"></a>Yükseltme zaman aşımı araştırın

Hataları en yaygın olarak hizmet kullanılabilirliği sorunlarını tarafından neden olduğu zaman aşımı yükseltin. Bu paragrafın aşağıdaki çıkış burada yeni kod sürümünde başlatmak hizmet çoğaltmalar veya örnekleri başarısız yükseltme işlemleri sırasında görülen bir durumdur. **UpgradeDomainProgressAtFailure** alan, hata anında tüm bekleyen yükseltme çalışmanın anlık görüntüsünü yakalar.

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/DemoApp
```

```Output
ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
```

Bu örnekte, yükseltme başarısız oldu yükseltme etki alanı *MYUD1* ve iki bölüm (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* ve *4b43f4d8-b26b-424e-9307-7a7a62e79750*) takılı. Çalışma zamanı birincil çoğaltmalara yerleştirmek başlatamadığından bölümleri takılı (*WaitForPrimaryPlacement*) hedef düğümlerde *Düğüm1* ve *Düğüm4*.

**Get-ServiceFabricNode** komutu, bu iki düğüm yükseltme etki alanında olduğunu doğrulamak için kullanılabilir *MYUD1*. *UpgradePhase* diyor *PostUpgradeSafetyCheck*, yükseltme etki alanındaki tüm düğümleri yükseltme işlemi tamamlandıktan sonra bu güvenlik denetimleri gerçekleşen anlamına gelir. Bu bilgiler, uygulama kodu yeni sürümü ile olası bir sorunu işaret eder. Yaygın sorunların çoğunu, açık veya yükseltme işlemi birincil kod yolları için hizmet hatalardır.

Bir *UpgradePhase* , *PreUpgradeSafetyCheck* gerçekleştirildiği için yükseltme etki alanı hazırlama sorunlar oluştu anlamına gelir. Yaygın sorunların çoğunu bu durumda hizmet kapatın veya birincil kod yollarından indirgeme hatalardır.

Geçerli **UpgradeState** olduğu *RollingBackCompleted*, özgün yükseltmeyi geri alma ile gerçekleştirilen gerekir böylece **FailureAction**, otomatik olarak toplama Yükseltme hatası durumunda yedekleyin. Özgün yükseltme el ile gerçekleştirdiyseniz **FailureAction**, sonra da yükseltme yerine canlı izin vermek için askıya alınmış durumda uygulamayı hata ayıklama.

Nadiren de olsa, **UpgradeDomainProgressAtFailure** alan sadece sistemin geçerli yükseltme etki alanı için tüm iş tamamlanınca genel yükseltme zaman aşımına uğrarsa boş olabilir. Böyle bir durumda artırmayı deneyin **UpgradeTimeout** ve **UpgradeDomainTimeout** yükseltme parametre değerleri ve yükseltmeyi yeniden deneyin.

### <a name="investigate-health-check-failures"></a>Sistem durumu denetimi hataları Araştır

Sistem durumu denetimi hataları bir yükseltme etki alanındaki tüm düğümleri tüm güvenlik denetimleri geçirme ve yükseltme işlemini tamamladıktan sonra oluşabilecek çeşitli sorunları tarafından tetiklenebilir. Bu paragrafın aşağıdaki çıktı, yükseltme bir hata nedeniyle başarısız sistem durumu denetimleri normaldir. **UnhealthyEvaluations** alan zaman belirtilen göre yükseltme başarısız oldu. sistem durumu denetimleri anlık görüntüsünü yakalar [sistem durumu ilkesi](service-fabric-health-introduction.md).

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/DemoApp
```

```Output
ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
```

Sistem durumu denetimi hataları araştırma, ilk Service Fabric sistem durumu modeli'nın bilinmesini gerektirir. Ancak bile bu tür bir derinlemesine anlamak olmadan, iki hizmet sağlıksız olduğunu görebiliriz: *fabric: / DemoApp/Svc3* ve *fabric: / DemoApp/Svc2*, ("InjectedFault" hata sistem durumu raporlarının yanı sıra Bu durumda). Bu örnekte, iki tanesi dört Hizmetleri, varsayılan hedef iyi durumda olmayan %0 sağlıksız, bağımlıdır (*MaxPercentUnhealthyServices*).

Yükseltme sırasında başarısız olan belirterek askıya alındığı bir **FailureAction** yükseltme başlatırken el. Bu mod başarısız durumdaki Canlı sistemi herhangi başka bir eylemde bulunmadan önce araştırmak sağlıyor.

### <a name="recover-from-a-suspended-upgrade"></a>Askıya alınmış bir yükseltmeyi kurtarmak

Bir geri alma ile **FailureAction**, yükseltme otomatik olarak geri başarısız üzerine dağıtılırken bu yana gerekli hiçbir kurtarma yok. El ile **FailureAction**, Kurtarma birkaç seçenek vardır:

1.  Tetikleyici bir geri alma
2. Yükseltme geri kalanında el ile devam edin
3. İzlenen yükseltme Sürdür

**Başlangıç ServiceFabricApplicationRollback** komutu uygulama geri başlatmak için herhangi bir zamanda kullanılabilir. Komut başarıyla döndüğünde, geri alma isteği sistemde kayıtlı ve kısa süre sonra başlar.

**Sürdürme ServiceFabricApplicationUpgrade** komutu, yükseltmeyi geri kalanında el ile devam etmek için kullanılabilir bir yükseltme etki alanı aynı anda. Bu modda, yalnızca güvenliği denetimlerini, sistem tarafından gerçekleştirilir. Daha fazla sistem durumu denetimleri yapılır. Bu komut yalnızca kullanılabilir olduğunda kullanılabilir *UpgradeState* gösterir *RollingForwardPending*, geçerli yükseltme etki alanı yükseltmeyi tamamladı. ancak bir sonraki (Bekleyen) başlatılmadı gösterir.

**Güncelleştirme ServiceFabricApplicationUpgrade** komutu, izlenen her iki güvenlik yükseltmeye devam etmek için kullanılabilir ve durum gerçekleştirilmekte olan denetimleri.

```powershell
Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored
```

```Output
UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
```

Yükseltme devam ediyor burada son askıya alındığı yükseltme etki alanı ve aynı parametre ve önce sistem durumu ilkeleri olarak yükseltme kullanın. Yükseltme devam ettiğinde gerekirse, tüm yükseltme parametreleri ve yukarıdaki çıktıda gösterilen sistem durumu ilkeleri aynı komutta değiştirilebilir. Bu örnekte, parametreler ve değişmeden sistem durumu ilkeleri, izlenen modunda yükseltme devam ediyor.

## <a name="further-troubleshooting"></a>Daha fazla sorun giderme

### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Service Fabric belirtilen sistem durumu ilkeleri takip edilmiyor

Olası nedeni 1:

Service Fabric sistem durumu değerlendirmesi için (örneğin, çoğaltmalar, bölümler ve Hizmetler) varlıkların gerçek sayılar tüm yüzdeleri dönüştürecektir ve her zaman tüm varlıklara yukarı yuvarlar. Örneğin, maksimum *MaxPercentUnhealthyReplicasPerPartition* % 21 ve beş çoğaltmalar, Service Fabric ikiye kadar iyi durumda olmayan çoğaltmalar sağlar (diğer bir deyişle,`Math.Ceiling (5*0.21)`). Bu nedenle, sistem durumu ilkeleri buna göre ayarlamanız gerekir.

Olası neden 2:

Sistem durumu ilkeleri açısından toplam Hizmetleri ve özel olmayan hizmet örneklerinin yüzdesi olarak belirtilir. Örneğin, bir uygulamanın dört varsa yükseltme önce hizmeti D sağlıksız olduğu ancak uygulamaya çok az etkisi olan hizmet örnekleri A, B, C ve D. Yükseltme sırasında bilinen iyi durumda olmayan hizmet D yoksay ve parametre istiyoruz *MaxPercentUnhealthyServices* % 25'yalnızca bir varsayarak, B ve C iyi durumda olması gerekir.

C kötüleşir sırada ancak, yükseltme sırasında D sağlıklı hale gelebilir. Yalnızca %25 Hizmetleri iyi durumda olmayan yükseltme yine de başarılı. Ancak, C d yerine beklenmedik bir şekilde sağlıksız olması nedeniyle beklenmeyen hataları neden olabilir Bu durumda, farklı hizmet türü a, D modellensin B ve c Sistem durumu ilkeleri hizmet türü belirlendiğinden, farklı bir sağlıksız yüzde eşikleri farklı hizmetlere uygulanabilir. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Uygulama yükseltmesi için bir sistem durumu ilkesi belirtilmedi, ancak yükseltme, hiçbir zaman belirtilen bazı zaman aşımları için yine de başarısız oluyor

Sistem durumu ilkeleri zaman olmayan yükseltme isteği, bunlar alınmıştır sağlanan *ApplicationManifest.xml* geçerli uygulama sürümü. Örneğin, sürüm 1.0 sürüm 2.0, uygulama sistem durumu ilkeleri için sürüm 1.0 için belirtilen uygulama X yükseltiyorsanız kullanılır. Yükseltme için kullanılması gereken farklı sistem durumu ilkesi, ilke, uygulama yükseltme API çağrısı bir parçası olarak belirtilmesi gerekir. API çağrısının bir parçası olarak belirtilen ilkeleri yalnızca yükseltme sırasında uygulanır. Yükseltme tamamlandıktan sonra ilkeleri belirtilen *ApplicationManifest.xml* kullanılır.

### <a name="incorrect-time-outs-are-specified"></a>Hatalı zaman aşımı belirtildi

Zaman aşımlarını tutarsız olduğunda ne olduğu hakkında merak etmiş. Örneğin, olabilir bir *UpgradeTimeout* o küçüktür *UpgradeDomainTimeout*. Yanıtı bir hata döndürülür. Hatalar varsa döndürülen *UpgradeDomainTimeout* toplamından daha az *HealthCheckWaitDuration* ve *HealthCheckRetryTimeout*, veya  *UpgradeDomainTimeout* toplamından daha az *HealthCheckWaitDuration* ve *HealthCheckStableDuration*.

### <a name="my-upgrades-are-taking-too-long"></a>My yükseltmeleri çok uzun sürüyor

Yükseltmeyi tamamlamak süre, sistem durumu denetimleri ve belirtilen zaman aşımı bağlıdır. Sistem durumu denetimleri ve zaman aşımları ne kadar kopyalama, dağıtma ve uygulamanın Sabitle vereceğine bağlıdır. Zaman aşımlarını çok agresif olan ölçülü uzun zaman aşımı ile başlamanızı öneririz, böylece başarısız yükseltme anlamına gelebilir.

Zaman aşımlarını yükseltme süreleri ile nasıl etkileşime hızlı Yenileyici şöyledir:

Bir yükseltme etki alanını tamamlayamıyor. yükseltme daha hızlı bir şekilde *HealthCheckWaitDuration* + *HealthCheckStableDuration*.

Yükseltme hatası gerçekleşemez daha hızlı bir şekilde *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.

Yükseltme etki alanı için yükseltme süresi sınırlıdır *UpgradeDomainTimeout*.  Varsa *HealthCheckRetryTimeout* ve *HealthCheckStableDuration* her ikisi de sıfır dışında olan ve yükseltme sonunda üzerindezamanaşımınauğrarsonrauygulamadurumunugeriveİlerigeçiştutar*UpgradeDomainTimeout*. *UpgradeDomainTimeout* başlar geçerli yükseltme etki alanı için bir kez yükseltme saymaya başlar.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulamayı kullanarak Visual Studio yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltmesi size yol gösterir.

[Uygulama Powershell kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltmesi size yol gösterir.

Uygulamanızı kullanarak yükseltme nasıl kontrol [yükseltme parametreleri](service-fabric-application-upgrade-parameters.md).

Nasıl kullanılacağını öğrenerek, uygulama yükseltmeleri uyumlu olma [veri seri hale getirme](service-fabric-application-upgrade-data-serialization.md).

Uygulamanızı yükseltilirken başvurarak gelişmiş işlevselliği kullanmanıza öğrenin [Gelişmiş konular](service-fabric-application-upgrade-advanced.md).