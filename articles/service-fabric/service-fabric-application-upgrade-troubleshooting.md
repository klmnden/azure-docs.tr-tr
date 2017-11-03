---
title: "Uygulama yükseltme sorunlarını giderme | Microsoft Docs"
description: "Bu makale, Service Fabric uygulaması ve bunların nasıl çözüleceği yükseltme geçici bazı yaygın sorunları ele alır."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 19ad152e-ec50-4327-9f19-065c875c003c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/03/2017
ms.author: subramar
ms.openlocfilehash: acfd26674aafab4ed1925d6b33967f917058b1be
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-application-upgrades"></a>Uygulama yükseltme ile ilgili sorunları giderme
Bu makalede bazı Azure Service Fabric uygulama ve bunların nasıl çözüleceği yükseltme geçici yaygın sorunların çoğunu kapsar.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Başarısız uygulama yükseltme sorunlarını giderme
Yükseltme başarısız olduğunda, çıktısını **Get-ServiceFabricApplicationUpgrade** komutu hata ayıklama için ek bilgiler içerir.  Aşağıdaki listede, ek bilgiler nasıl kullanılabileceğini belirtir:

1. Hata türünü tanımlayın.
2. Hatanın nedenini belirleyin.
3. Daha fazla araştırma için bir veya daha fazla başarısız olan bileşenleri yalıtma.

Service Fabric mi bakılmaksızın hatası algıladığında, bu bilgiler kullanılabilir **FailureAction** geri alma veya yükseltme askıya alma.

### <a name="identify-the-failure-type"></a>Hata türünü tanımlayın
Çıktısı olarak **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** , bir yükseltme hatasına Service Fabric tarafından algılandı zaman damgası (UTC) olarak tanımlar ve  **FailureAction** tetiklendi. **FailureReason** hatanın olası üç üst düzey nedenlerden biri tanımlar:

1. UpgradeDomainTimeout - gösteren belirli bir yükseltme etki alanı tamamlanması çok uzun sürdü ve **UpgradeDomainTimeout** süresi doldu.
2. OverallUpgradeTimeout - gösterir genel yükseltmenin tamamlanması çok uzun sürdü ve **UpgradeTimeout** süresi doldu.
3. Durum denetimi - bir güncelleştirme etki alanını yükselttikten sonra uygulamaya göre belirtilen sistem durumu ilkeleri sağlıksız kalan olduğunu gösterir ve **HealthCheckRetryTimeout** süresi doldu.

Yükseltme başarısız olur ve geri alma başladığında bu girişler yalnızca çıktıda gösterilir. Daha fazla bilgi hata türüne bağlı olarak görüntülenir.

### <a name="investigate-upgrade-timeouts"></a>Yükseltme zaman aşımı araştırın
Zaman aşımı hataları tarafından hizmet kullanılabilirliği sorunlarını yaygın hatalardır yükseltin. Bu paragraf aşağıdaki çıktı burada yeni kod sürümü başlatmak hizmet çoğaltmaları veya örnekleri başarısız yükseltme normaldir. **UpgradeDomainProgressAtFailure** alan tüm bekleyen yükseltme çalışmanın hatanın zamanında anlık görüntüsünü yakalar.

```
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

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

Bu örnekte, yükseltme başarısız yükseltme etki alanında *MYUD1* ve iki bölüm (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* ve *4b43f4d8-b26b-424e-9307-7a7a62e79750*) takılmış. Çalışma zamanı birincil çoğaltmalara yerleştiremedi çünkü bölümleri takılmış (*WaitForPrimaryPlacement*) hedef düğümleri üzerinde *Düğüm1* ve *Düğüm4*.

**Get-ServiceFabricNode** komutu, bu iki düğüm yükseltme etki alanında olduğunu doğrulamak için kullanılabilir *MYUD1*. *UpgradePhase* diyor *PostUpgradeSafetyCheck*, yükseltme etki alanındaki tüm düğümleri yükseltme işlemi tamamlandıktan sonra bu güvenlik denetimleri yaşanan anlamına gelir. Bu bilgiler, uygulama kodu yeni sürümü ile olası bir sorunu işaret eder. En yaygın sorunları, açın veya birincil kod yollarının yükseltmesine hizmet hatalardır.

Bir *UpgradePhase* , *PreUpgradeSafetyCheck* gerçekleştirilip gerçekleştirilmediğine önce yükseltme etki alanını hazırlama sorunları vardı anlamına gelir. En yaygın sorunları bu durumda hizmet kapatma ya da birincil kod yollarından indirgeme hatalardır.

Geçerli **UpgradeState** olan *RollingBackCompleted*, özgün yükseltmeyi geri alma ile gerçekleştirilen gerekir böylece **FailureAction**, hangi otomatik olarak toplama hata sonrasında yükseltme yedekleyin. Özgün yükseltme el ile gerçekleştirdiyseniz **FailureAction**, sonra da yükseltme yerine canlı izin vermek için askıya alınmış durumda uygulamayı hata ayıklama.

Nadir durumlarda **UpgradeDomainProgressAtFailure** alan yalnızca sistem geçerli yükseltme etki alanı için tüm iş tamamladıkça genel yükseltme zaman aşımına uğrarsa boş olabilir. Bu durumda, artırmayı deneyin **UpgradeTimeout** ve **UpgradeDomainTimeout** yükseltme parametre değerlerini ve yükseltmeyi yeniden deneyin.

### <a name="investigate-health-check-failures"></a>Sistem durumu denetimi hatalarını Araştır
Sistem durumu denetimi hataları bir yükseltme etki alanındaki tüm düğümleri yükseltme ve tüm güvenlik denetimleri geçirme işlemini tamamladıktan sonra oluşabilecek çeşitli sorunları tarafından tetiklenebilir. Bu paragraf aşağıdaki çıktı yükseltme bir hata nedeniyle başarısız durumu denetimleri normaldir. **UnhealthyEvaluations** alan belirtilen göre yükseltme zamanda başarısız durumu denetimleri görüntüsünü yakalar [sistem durumu ilkesi](service-fabric-health-introduction.md).

```
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

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

Sistem durumu denetimi hataları araştırılıyor ilk Service Fabric sistem durumu modeli bilinmesini gerektirir. Ancak bile bu tür bir ayrıntılı anlama olmadan, iki hizmet sağlıksız olduğunu görebiliriz: *fabric: / DemoApp/Svc3* ve *fabric: / DemoApp/Svc2*, hata sistem durumu raporlarının ("InjectedFault" ile birlikte Bu durumda). Bu örnekte, iki dışı dört Hizmetleri %0 sağlıksız varsayılan hedef olduğu sağlıksız, (*MaxPercentUnhealthyServices*).

Yükseltme sırasında başarısız olan belirterek askıya alındı bir **FailureAction** yükseltme başlatırken elle. Bu mod başka eylemi gerçekleştirmeden önce başarısız durumda Canlı sistem araştırmanıza olanak tanır.

### <a name="recover-from-a-suspended-upgrade"></a>Askıya alınmış bir yükseltmeyi kurtarmak
Bir geri alma ile **FailureAction**, yükseltme otomatik olarak geri başarısız olan temel alındığında olmadığından gerekli hiçbir kurtarma yok. El ile **FailureAction**, çeşitli kurtarma seçenekler vardır:

1.  Tetikleyici bir geri alma
2. Yükseltme geri kalanı el ile devam edin
3. İzlenen yükseltmeyi sürdürün

**Başlangıç ServiceFabricApplicationRollback** komutu uygulama geri başlatmak için herhangi bir zamanda kullanılabilir. Komut başarıyla döndüğünde, geri alma isteği sistemde kayıtlı ve kısa süre içinde bundan sonra başlatır.

**Resume-ServiceFabricApplicationUpgrade** komutu, yükseltme geri kalanı el ile devam etmek için kullanılabilir bir seferde bir yükseltme etki alanı. Bu modda, yalnızca güvenlik denetimleri sistem tarafından gerçekleştirilir. Daha fazla sistem durumu denetimleri yapılır. Bu komut yalnızca olabilir kullanılır *UpgradeState* gösterir *RollingForwardPending*, başka bir deyişle, geçerli yükseltme etki alanı yükseltmeyi tamamladı. ancak bir sonraki (Bekleyen) başlatılmadı.

**Güncelleştirme ServiceFabricApplicationUpgrade** komutu, her iki güvenlik izlenen yükseltmeye devam etmek için kullanılabilir ve sistem durumu gerçekleştirilen denetler.

```
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

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

PS D:\temp>
```

Yükseltme devam eder burada son askıya alındı yükseltme etki alanı ve aynı parametreleri ve önce sistem durumu ilkeleri olarak yükseltme kullanın. Yükseltme devam ettiğinde gerekirse, tüm yükseltme parametreleri ve yukarıdaki çıktıda gösterilen sistem durumu ilkeleri aynı komutta değiştirilebilir. Bu örnekte, izlenen modunda, parametreleri ve değişmeden sistem durumu ilkeleri ile yükseltme devam ediyor.

## <a name="further-troubleshooting"></a>Daha fazla sorun giderme
### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Service Fabric belirtilen sistem durumu ilkeleri aşağıdaki değil
Olası neden 1:

Service Fabric sistem durumu değerlendirmesi için (örneğin, çoğaltmalar, bölümler ve Hizmetler) varlıkların gerçek sayılar tüm yüzdeleri çevirir ve her zaman tüm varlıklara yukarı yuvarlar. Örneğin, maksimum *MaxPercentUnhealthyReplicasPerPartition* % 21 olduğunu ve beş çoğaltmalar, en fazla iki sağlıksız çoğaltmaları Service Fabric sağlar (diğer bir deyişle,`Math.Ceiling (5*0.21)`). Bu nedenle, sistem durumu ilkeleri uygun şekilde ayarlamanız gerekir.

Olası neden 2:

Sistem durumu ilkeleri toplam Hizmetleri ve özgü olmayan hizmet örnekleri yüzdelerini bakımından belirtilir. Örneğin, bir uygulama dört varsa, yükseltme önce hizmeti D sağlıksız olduğu ancak uygulama için çok az etkisi olan hizmet örnekleri A, B, C ve D. Yükseltme sırasında bilinen sağlıksız Hizmeti D yoksay ve parametre kümesini istiyoruz *MaxPercentUnhealthyServices* yalnızca A varsayılarak % 25 B ve C sağlıklı olması gerekir.

C bozulur sırada ancak, yükseltme sırasında D sağlıklı duruma gelebilir. Yalnızca %25 Hizmetleri sağlıklı olduğundan yükseltme yine de başarılı. Ancak, beklenmedik bir şekilde yerine D. sağlıklı olmadığını C nedeniyle beklenmeyen hataları neden Bu durumda, D, A'dan farklı hizmet türü olarak modellenmesi gerekir B ve c Sistem durumu ilkeleri hizmet türü belirtilmiş olduğundan, farklı sağlıksız yüzde eşikleri farklı hizmetlerine uygulanabilir. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Uygulama yükseltme için bir sistem durumu ilkesi belirtmedi ancak yükseltme hala hiçbir zaman belirtilen bazı zaman aşımlarını başarısız
Sistem durumu ilkeleri zaman olmayan yükseltme isteği, bunlar gelen alınır sağlanan *ApplicationManifest.xml* geçerli uygulama sürümü. Örneğin, sürüm 1.0 sürüm 2.0, uygulama sistem durumu ilkeleri için sürüm 1.0 için belirtilen uygulama X yükseltiyorsanız kullanılır. Farklı sistem durumu ilkesi yükseltme için kullanılıp kullanılmayacağını ilke, uygulama yükseltme API çağrısı bir parçası olarak belirtilmesi gerekiyor. API çağrısı bir parçası olarak belirtilen ilkeleri, yalnızca yükseltme sırasında uygulanır. Yükseltme tamamlandıktan sonra ilkeleri belirtilen *ApplicationManifest.xml* kullanılır.

### <a name="incorrect-time-outs-are-specified"></a>Hatalı zaman aşımlarını belirtilen
Zaman aşımlarını tutarsız ayarladığınızda ne olduğu hakkında hiç merak ettiniz. Örneğin, olabilir bir *UpgradeTimeout* bu küçük *UpgradeDomainTimeout*. Yanıtı bir hata döndürülür. Hatalar varsa döndürülür *UpgradeDomainTimeout* toplamını'dan küçük *HealthCheckWaitDuration* ve *HealthCheckRetryTimeout*, veya  *UpgradeDomainTimeout* toplamını'dan küçük *HealthCheckWaitDuration* ve *HealthCheckStableDuration*.

### <a name="my-upgrades-are-taking-too-long"></a>My yükseltmeler çok uzun sürüyor
Zaman tamamlamak bir yükseltme için sistem durumu denetimlerinin ve belirtilen zaman aşımlarını bağlıdır. Sistem durumu denetimlerinin ve zaman aşımlarını kopyalama, dağıtma ve uygulama Sabitle için gereken süreyi bağlıdır. Nedenle ölçülü uzun zaman aşımlarını başlangıç öneririz zaman aşımlarını çok agresif olan başarısız yükseltme anlamına gelebilir.

Zaman aşımları yükseltme süreleri ile nasıl etkileşim hızlı Yenileyici şöyledir:

Bir yükseltme etki alanına tamamlayamıyor için yükseltme daha hızlı bir şekilde *HealthCheckWaitDuration* + *HealthCheckStableDuration*.

Yükseltme hatası gerçekleşemez daha hızlı bir şekilde *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.

Yükseltme etki alanı için yükseltme süresi sınırlıdır *UpgradeDomainTimeout*.  Varsa *HealthCheckRetryTimeout* ve *HealthCheckStableDuration* hem de sıfır olması ve yükseltme sonunda üzerindezamanaşımınauğruyorsonrauygulamadurumunugeriveİlerigeçiştutar*UpgradeDomainTimeout*. *UpgradeDomainTimeout* geçerli yükseltme etki alanı başlar için bir kez yükseltme sayım başlatır.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama kullanarak Visual Studio yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltme yol gösterir.

[Uygulama Powershell kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltme yol gösterir.

Uygulamanızı kullanarak yükseltme nasıl kontrol [yükseltme parametreleri](service-fabric-application-upgrade-parameters.md).

Uygulama yükseltme nasıl kullanılacağını öğrenerek uyumlu hale getirmek [veri seri hale getirme](service-fabric-application-upgrade-data-serialization.md).

Gelişmiş işlevselliği başvurarak uygulamanızı yükseltirken kullanmayı öğrenin [Gelişmiş konular](service-fabric-application-upgrade-advanced.md).
