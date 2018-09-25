---
title: 'Uygulama yükseltme: yükseltme parametreleri | Microsoft Docs'
description: Service Fabric uygulaması gerçekleştirmek için sistem durumu denetimleri ve otomatik olarak yükseltme geri almak için ilkeler de dahil olmak üzere, yükseltmeyle ilgili parametreler açıklanmıştır.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: a4170ac6-192e-44a8-b93d-7e39c92a347e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/17/2018
ms.author: subramar
ms.openlocfilehash: f3f381fddee9c1830202854f02556f73b5aeed23
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47055586"
---
# <a name="application-upgrade-parameters"></a>Uygulama yükseltme parametreleri
Bu makalede, Azure Service Fabric uygulaması yükseltme sırasında geçerli olan çeşitli parametreler açıklanmaktadır. Uygulama yükseltme parametreleri zaman aşımları ve yükseltme sırasında uygulanan sistem durumu denetimlerini denetleme ve bunlar yükseltme başarısız olduğunda uygulanmalıdır ilkeleri belirtin.

Uygulama parametreleri, PowerShell veya Visual Studio kullanarak yükseltme uygulanır. PowerShell ve/veya Visual Studio geçerli gerekli ve isteğe bağlı parametreleri gibi gerekli parametreler ve isteğe bağlı parametreler tablolarda açıklanmıştır.

Uygulama yükseltmede üç kullanıcı tarafından seçilebilen yükseltme modlarından birini başlatılır. Kendi uygulama parametrelerinin her modu vardır:
- İzleniyor
- İzlenmeyen otomatik
- İzlenmeyen el ile

PowerShell kullanarak Service Fabric uygulama yükseltme [başlangıç ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade) komutu. Yükseltme modu ya da geçirerek seçili **izlenen**, **UnmonitoredAuto**, veya **UnmonitoredManual** parametresi [ Başlangıç ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade).

Visual Studio Service Fabric uygulama yükseltme parametreleri, Visual Studio Yükseltme Ayarları iletişim kutusu ayarlanır. Visual Studio yükseltme modu kullanılarak seçilen **yükseltme modu** ya da açılan kutuda **izlenen**, **UnmonitoredAuto**, veya **UnmonitoredManual** . Daha fazla bilgi için [Visual Studio'da bir Service Fabric uygulamasının yükseltmesini yapılandırma](service-fabric-visualstudio-configure-upgrade.md).

## <a name="required-parameters"></a>Gerekli Parametreler
(PowerShell, VS PS = Visual Studio =)

| Parametre | Şunun İçin Geçerli | Açıklama |
| --- | --- | --- |
ApplicationName |PS| Yükseltilmekte olan uygulamanın adı. Örnekler: fabric: / VisualObjects, fabric: / ClusterMonitor. |
ApplicationTypeVersion|PS|Uygulama sürümü türü yükseltme hedefler. |
FailureAction |PS, VS|İzin verilen değerler **geçersiz**, **geri alma**, ve **el ile**. Yükseltme başarısız olduğunda, Service Fabric tarafından gerçekleştirilecek eylem. Uygulama güncelleştirme öncesi sürüm (Geri Al) geri alınması veya yükseltme sırasında geçerli yükseltme etki alanı durdurulmuş olabilir. İkinci durumda, yükseltme modu da değiştirilir **el ile**.|
İzleniyor |PS|Yükseltme modu izlenip izlenmediğini gösterir. Service Fabric, yükseltme etki alanı ve küme durumunu tanımladığınız sistem durumu ilkeleri karşılıyorsanız cmdlet'i bir yükseltme etki alanı için bir yükseltme tamamlandıktan sonra sonraki yükseltme etki alanı yükseltir. Yükseltme etki alanı ya da küme sistem durumu ilkeleri karşılamak başarısız olursa, yükseltme başarısız olur ve Service Fabric geri yükseltme etki alanı için yükseltme yapar veya belirtilen ilke başına el ile moduna döner. Bir üretim ortamında uygulama yükseltmeleri için önerilen mod budur. |
UpgradeMode | VS | İzin verilen değerler **izlenen** (varsayılan), **UnmonitoredAuto**, veya **UnmonitoredManual**. Ayrıntılar için bu makaledeki her modu için PowerShell parametreleri bakın. |
UnmonitoredAuto | PS | Yükseltme modu izlenmeyen otomatik olduğunu gösterir. Service Fabric, bir yükseltme etki alanını yükseltildikten sonra Service Fabric uygulaması sistem durumu bağımsız olarak bir sonraki yükseltme etki alanı yükseltir. Bu mod, üretim için tavsiye edilmez ve yalnızca bir uygulamanın geliştirilmesi sırasında yararlıdır. |
UnmonitoredManual | PS | Yükseltme modu izlenmeyen el ile olduğunu gösterir. Service Fabric, bir yükseltme etki alanını yükseltildikten sonra sonraki yükseltme etki alanı kullanarak yükseltmek bekleyeceği *sürdürme ServiceFabricApplicationUpgrade* cmdlet'i. |

## <a name="optional-parameters"></a>İsteğe bağlı parametreler

Sistem durumu değerlendirme parametreler isteğe bağlıdır. Yükseltme başladığında sistem durumu değerlendirme ölçütleri belirtilmezse, Service Fabric Uygulama örneğinin ApplicationManifest.xml içinde belirtilen uygulama sistem durumu ilkeleri kullanır.

Yatay kaydırma çubuğu, tablonun alt kısmındaki tam açıklama alanı görüntülemek için kullanın.

(PowerShell, VS PS = Visual Studio =)

| Parametre | Şunun İçin Geçerli | Açıklama |
| --- | --- | --- |
| ApplicationParameter |PS, VS| Uygulama parametreleri geçersiz kılmalarını belirtir.<br>PowerShell applcation parametreleri hashtable ad/değer çiftleri belirtilir. Örneğin, @{"VotingData_MinReplicaSetSize" = "3"; "VotingData_PartitionCount" = "1"}.<br>Visual Studio uygulama parametreleri, Service Fabric uygulamasını Yayımla iletişim belirtilebilir **uygulama parametreleri dosyası** alan.
| Onayla |PS| İzin verilen değerler **True** ve **False**. Cmdlet'i çalıştırmadan önce onay ister. |
| ConsiderWarningAsError |PS, VS |İzin verilen değerler **True** ve **False**. Varsayılan ayar, **False** değeridir. Uygulama için uyarı sistem durumu olayları, yükseltme sırasında durumunu değerlendirilirken hata olarak değerlendir. Varsayılan olarak, Service Fabric uyarı olayları olsa bile, yükseltme devam edebilmeniz için hatası (hata) gibi uyarı sistem durumu olayları değerlendirmez. |
| DefaultServiceTypeHealthPolicy | PS, VS |Sistem durumu ilkesi biçiminde MaxPercentUnhealthyPartitionsPerService, MaxPercentUnhealthyReplicasPerPartition, MaxPercentUnhealthyServices izlenen yükseltme için kullanılacak varsayılan hizmet türünün belirtir. Örneğin, aşağıdaki değerleri 5,10,15 gösterir: MaxPercentUnhealthyPartitionsPerService = 5, MaxPercentUnhealthyReplicasPerPartition = 10, MaxPercentUnhealthyServices = 15. |
| Zorla | PS, VS | İzin verilen değerler **True** ve **False**. Yükseltme işlemi uyarı iletisi atlar ve sürüm numarasını bile değişmediğinden yükseltme zorlar gösterir. Bu, yerel olarak test etmek için kullanışlıdır ancak kapalı kalma süresini ve olası veri kaybına neden olan mevcut dağıtımı kaldırma gerektirdiği bir üretim ortamında kullanım için önerilmez. |
| ForceRestart |PS, VS |Hizmet kodu güncelleştirmeden bir yapılandırma veya veri paketi güncelleştirirseniz yalnızca ForceRestart özelliği ayarlanmışsa hizmet yeniden **True**. Güncelleştirme tamamlandığında, Service Fabric hizmeti, yeni yapılandırma paketi veya veri paketi kullanılabilir olduğunu bildirir. Değişiklikleri uygulamak için sorumlu hizmettir. Gerekirse, hizmetin kendisini yeniden başlatabilirsiniz. |
| HealthCheckRetryTimeoutSec |PS, VS |Süre (saniye cinsinden), yükseltme başarısız olarak bildirme önce sistem durumu değerlendirmesi gerçekleştirmek, Service Fabric devam eder. 600 saniye varsayılandır. Bu süre sonra başlar *HealthCheckWaitDurationSec* ulaşıldı. Bu *HealthCheckRetryTimeout*, Service Fabric uygulama sistem durumunun birden çok sistem durumu denetimleri gerçekleştirmek. Varsayılan değer 10 dakikadır ve uygulamanız için uygun biçimde özelleştirilmelidir. |
| HealthCheckStableDurationSec |PS, VS |Süre (saniye cinsinden) uygulamanın sonraki yükseltme etki alanına taşıma veya yükseltmeyi tamamlamadan önce kararlı olduğunu doğrulayın. Bu bekleme süresi, sistem durumu denetimini gerçekleştirildikten sonra sistem durumunda algılanmayan değişiklikler önlemek için kullanılır. Varsayılan değer 120 saniyedir ve uygulamanız için uygun biçimde özelleştirilmelidir. |
| HealthCheckWaitDurationSec |PS, VS | Süre (saniye cinsinden) yükseltme etki alanında önce Service Fabric yükseltmesi tamamlandıktan sonra uygulama durumunu değerlendirir. Bu süre de sağlıklı edilebilmelerini önce uygulamanın çalıştırılması gerektiği zaman olarak kabul edilebilir. Sistem durumu denetimi başarılı olursa, yükseltme işlemi, bir sonraki yükseltme etki alanına devam eder.  Service Fabric sistem durumu denetimi başarısız olursa bekler [UpgradeHealthCheckInterval](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-fabric-settings#clustermanager) sistem yeniden denemeden önce kadar yeniden denetle *HealthCheckRetryTimeoutSec* ulaşıldı. Varsayılan ve önerilen değer olan 0 saniye. |
| MaxPercentUnhealthyDeployedApplications|PS, VS |Varsayılan ve önerilen değer: 0. Dağıtılan uygulamalar maksimum sayısını belirtin (bkz [durumu bölümünün](service-fabric-health-introduction.md)) olabilecek kötü önce uygulama durumu kötü olarak değerlendirilir ve yükseltme başarısız olur. Bu parametre, düğüm üzerinde uygulama durumunu tanımlar ve yükseltme sırasında sorunları algılamaya yardımcı olur. Genellikle, uygulamanın çoğaltmaların iyi ve bu nedenle devam etmek yükseltme izin görüntülenecek uygulama sağlayan diğer düğüm, yük dengeli alın. Bir katı belirterek *MaxPercentUnhealthyDeployedApplications* sistem durumu, Service Fabric uygulama paketi ile ilgili bir sorun hızlı bir şekilde algılayın ve hızlı yükseltme yük devretme elde etmeye yardımcı. |
| MaxPercentUnhealthyServices |PS, VS |Bir parametre *DefaultServiceTypeHealthPolicy* ve *ServiceTypeHealthPolicyMap*. Varsayılan ve önerilen değer: 0. Uygulama durumu kötü olarak değerlendirilir ve yükseltme başarısız olduğunda önce iyi durumda olmayan uygulama örneğinde Hizmetleri en fazla sayısını belirtin. |
| MaxPercentUnhealthyPartitionsPerService|PS, VS |Varsayılan ve önerilen değer: 0. En yüksek bölüm sayısı, hizmet sistem durumu kötü olarak kabul edilmeden önce iyi durumda olmayan bir hizmet belirtin. |
| MaxPercentUnhealthyReplicasPerPartition|PS, VS |Varsayılan ve önerilen değer: 0. Bölüm iyi durumda olmadığı kabul edebilmesi iyi durumda olmayan bölüm çoğaltmaları sayısı belirtin. |
| ServiceTypeHealthPolicyMap | PS, VS | Bir hizmet türüne ait hizmetlerin durumunu değerlendirmek için kullanılan sistem durumu ilkesi temsil eder. Şu biçimde bir karma tablo girişi alır: @ {"Servicetypename birlikte belirtilemez": "MaxPercentUnhealthyPartitionsPerService, MaxPercentUnhealthyReplicasPerPartition, MaxPercentUnhealthyServices"} Örneğin: @{"ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5"} |
| TimeoutSec | PS, VS | İşlemi için saniye cinsinden zaman aşımı süresini belirtir. |
| UpgradeDomainTimeoutSec |PS, VS |Tek bir yükseltme etki alanını yükseltmek için en uzun süreyi (saniye cinsinden). Bu zaman aşımı ulaşılırsa yükseltme durdurur ve ayarını temel alınarak geçer *FailureAction*. Varsayılan değer hiçbir zaman: (sonsuz) ve uygulamanız için uygun biçimde özelleştirilmelidir. |
| UpgradeReplicaSetCheckTimeoutSec |PS, VS |**Durum bilgisi olmayan hizmet**--hizmetin ek örneklerini kullanılabilir olmasını sağlamak Service Fabric tek bir yükseltme etki alanında çalışır. Service Fabric hedef örnek sayısı, birden fazla ise, birden fazla örneğini bir maksimum zaman aşımı değeri en fazla kullanılabilir olmasını bekler. Bu zaman aşımı kullanılarak belirtilen *UpgradeReplicaSetCheckTimeoutSec* özelliği. Zaman aşımı süresi dolarsa, Service Fabric service örnek sayısından bağımsız olarak yükseltme işlemine devam eder. Hedef örnek sayısını ise, Service Fabric beklememeyi ve hemen yükseltme işlemine devam eder.<br><br>**Durum bilgisi olan hizmet**--çoğaltma kümesine bir çekirdek olduğundan emin olmak Service Fabric tek bir yükseltme etki alanında çalışır. Service Fabric bir çekirdek bir maksimum zaman aşımı değeri en fazla kullanılabilir olmasını bekler (tarafından belirtilen *UpgradeReplicaSetCheckTimeoutSec* özelliği). Zaman aşımı süresi dolarsa, Service Fabric çekirdek bakılmaksızın yükseltme işlemine devam eder. Bu ayar kümesi olarak hiçbir zaman (sonsuz) İleri sarmadır olduğunda ve 1200 saniye olan zaman geri alınıyor. |
| UpgradeTimeoutSec |PS, VS |Tüm yükseltme için geçerli bir zaman aşımı (saniye cinsinden). Bu zaman aşımı ulaşılırsa, yükseltme durdurulur ve *FailureAction* tetiklenir. Varsayılan değer hiçbir zaman: (sonsuz) ve uygulamanız için uygun biçimde özelleştirilmelidir. |
| WhatIf | PS | İzin verilen değerler **True** ve **False**. Cmdlet çalıştırılıyorsa ne olacağını gösterir. Cmdlet çalıştırılmaz. |

*MaxPercentUnhealthyServices*, *MaxPercentUnhealthyPartitionsPerService*, ve *MaxPercentUnhealthyReplicasPerPartition* ölçüt başına belirtilebilir bir uygulama örneği için hizmet türü. Bu parametreleri başına hizmet ayarı sağlayan bir uygulamanın farklı değerlendirme ilkeleriyle farklı Hizmetleri türler bulunur. Örneğin, bir durum bilgisi olmayan bir ağ geçidi hizmet türü olabilir bir *MaxPercentUnhealthyPartitionsPerService* belirli bir uygulama örneği için bir durum bilgisi olan Altyapısı hizmeti türünden farklı.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamayı kullanarak Visual Studio yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltmesi size yol gösterir.

[Uygulama Powershell kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltmesi size yol gösterir.

[Linux üzerinde Service Fabric CLI kullanarak uygulamanızı yükseltmek](service-fabric-application-lifecycle-sfctl.md#upgrade-application) Service Fabric CLI kullanarak bir uygulama yükseltmesi size yol gösterir.

[Service Fabric Eclipse eklentisi kullanarak uygulamanızı yükseltme](service-fabric-get-started-eclipse.md#upgrade-your-service-fabric-java-application)

Nasıl kullanılacağını öğrenerek, uygulama yükseltmeleri uyumlu olma [veri seri hale getirme](service-fabric-application-upgrade-data-serialization.md).

Uygulamanızı yükseltilirken başvurarak gelişmiş işlevselliği kullanmanıza öğrenin [Gelişmiş konular](service-fabric-application-upgrade-advanced.md).

Yaygın sorunlar uygulama yükseltmeleri adımları başvurarak düzeltme [uygulama yükseltme sorunlarını giderme](service-fabric-application-upgrade-troubleshooting.md).
