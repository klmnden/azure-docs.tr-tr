---
title: 'Uygulama yükseltme: yükseltme parametreleri | Microsoft Docs'
description: Service Fabric uygulaması gerçekleştirmek için sistem durumu denetimleri ve otomatik olarak yükseltme geri almak için ilkeler de dahil olmak üzere, yükseltmeyle ilgili parametreler açıklanmıştır.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: a4170ac6-192e-44a8-b93d-7e39c92a347e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/08/2018
ms.author: subramar
ms.openlocfilehash: 9a93c0993ee45e72b11b023982dfbbe8c6528272
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60614390"
---
# <a name="application-upgrade-parameters"></a>Uygulama yükseltme parametreleri
Bu makalede, Azure Service Fabric uygulaması yükseltme sırasında geçerli olan çeşitli parametreler açıklanmaktadır. Uygulama yükseltme parametreleri zaman aşımları ve yükseltme sırasında uygulanan sistem durumu denetimlerini denetleme ve bunlar yükseltme başarısız olduğunda uygulanmalıdır ilkeleri belirtin. Uygulama parametreleri kullanarak yükseltmeleri için geçerlidir:
- PowerShell
- Visual Studio
- SFCTL
- [REST](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-startapplicationupgrade)

Uygulama yükseltmede üç kullanıcı tarafından seçilebilen yükseltme modlarından birini başlatılır. Kendi uygulama parametrelerinin her modu vardır:
- İzlenen
- İzlenmeyen otomatik
- İzlenmeyen el ile

Uygun gerekli ve isteğe bağlı parametreleri gibi her bölümde açıklanmıştır.

## <a name="visual-studio-and-powershell-parameters"></a>Visual Studio ve PowerShell parametreleri

PowerShell kullanarak Service Fabric uygulama yükseltme [başlangıç ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade) komutu. Yükseltme modu ya da geçirerek seçili **izlenen**, **UnmonitoredAuto**, veya **UnmonitoredManual** parametresi [ Başlangıç ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade).

Visual Studio Service Fabric uygulama yükseltme parametreleri, Visual Studio Yükseltme Ayarları iletişim kutusu ayarlanır. Visual Studio yükseltme modu kullanılarak seçilen **yükseltme modu** ya da açılan kutuda **izlenen**, **UnmonitoredAuto**, veya **UnmonitoredManual** . Daha fazla bilgi için [Visual Studio'da bir Service Fabric uygulamasının yükseltmesini yapılandırma](service-fabric-visualstudio-configure-upgrade.md).

### <a name="required-parameters"></a>Gerekli Parametreler
(PowerShell, VS PS = Visual Studio =)

| Parametre | İçin geçerlidir | Açıklama |
| --- | --- | --- |
ApplicationName |PS| Yükseltilmekte olan uygulamanın adı. Örnekler: fabric: / VisualObjects, fabric: / ClusterMonitor. |
ApplicationTypeVersion|PS|Uygulama sürümü türü yükseltme hedefler. |
FailureAction |PS, VS|İzin verilen değerler **geri alma**, **el ile**, ve **geçersiz**. Ne zaman telafi gerçekleştirilecek bir *izlenen* ilke ihlallerini ilke veya sistem durumu izleme karşılaştığı yükseltin. <br>**Geri alma** yükseltme otomatik olarak yükseltme öncesi sürüme geri döner olduğunu belirtir. <br>**El ile** yükseltme geçer gösterir *UnmonitoredManual* yükseltme modu. <br>**Geçersiz** hatası eylemi geçersiz olduğunu gösterir.|
İzlenen |PS|Yükseltme modu izlenip izlenmediğini gösterir. Service Fabric, yükseltme etki alanı ve küme durumunu tanımladığınız sistem durumu ilkeleri karşılıyorsanız cmdlet'i bir yükseltme etki alanı için bir yükseltme tamamlandıktan sonra sonraki yükseltme etki alanı yükseltir. Yükseltme etki alanı ya da küme sistem durumu ilkeleri karşılamak başarısız olursa, yükseltme başarısız olur ve Service Fabric geri yükseltme etki alanı için yükseltme yapar veya belirtilen ilke başına el ile moduna döner. Bir üretim ortamında uygulama yükseltmeleri için önerilen mod budur. |
UpgradeMode | VS | İzin verilen değerler **izlenen** (varsayılan), **UnmonitoredAuto**, veya **UnmonitoredManual**. Ayrıntılar için bu makaledeki her modu için PowerShell parametreleri bakın. |
UnmonitoredAuto | PS | Yükseltme modu izlenmeyen otomatik olduğunu gösterir. Service Fabric, bir yükseltme etki alanını yükseltildikten sonra Service Fabric uygulaması sistem durumu bağımsız olarak bir sonraki yükseltme etki alanı yükseltir. Bu mod, üretim için tavsiye edilmez ve yalnızca bir uygulamanın geliştirilmesi sırasında yararlıdır. |
UnmonitoredManual | PS | Yükseltme modu izlenmeyen el ile olduğunu gösterir. Service Fabric, bir yükseltme etki alanını yükseltildikten sonra sonraki yükseltme etki alanı kullanarak yükseltmek bekleyeceği *sürdürme ServiceFabricApplicationUpgrade* cmdlet'i. |

### <a name="optional-parameters"></a>İsteğe bağlı parametreler

Sistem durumu değerlendirme parametreler isteğe bağlıdır. Yükseltme başladığında sistem durumu değerlendirme ölçütleri belirtilmezse, Service Fabric Uygulama örneğinin ApplicationManifest.xml içinde belirtilen uygulama sistem durumu ilkeleri kullanır.

Yatay kaydırma çubuğu, tablonun alt kısmındaki tam açıklama alanı görüntülemek için kullanın.

(PowerShell, VS PS = Visual Studio =)

| Parametre | İçin geçerlidir | Açıklama |
| --- | --- | --- |
| ApplicationParameter |PS, VS| Uygulama parametreleri geçersiz kılmalarını belirtir.<br>PowerShell uygulama parametreleri hashtable ad/değer çiftleri belirtilir. Örneğin, @{"VotingData_MinReplicaSetSize" = "3"; "VotingData_PartitionCount" = "1"}.<br>Visual Studio uygulama parametreleri, Service Fabric uygulamasını Yayımla iletişim belirtilebilir **uygulama parametreleri dosyası** alan.
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
| MaxPercentUnhealthyPartitionsPerService|PS, VS |Bir parametre *DefaultServiceTypeHealthPolicy* ve *ServiceTypeHealthPolicyMap*. Varsayılan ve önerilen değer: 0. En yüksek bölüm sayısı, hizmet sistem durumu kötü olarak kabul edilmeden önce iyi durumda olmayan bir hizmet belirtin. |
| MaxPercentUnhealthyReplicasPerPartition|PS, VS |Bir parametre *DefaultServiceTypeHealthPolicy* ve *ServiceTypeHealthPolicyMap*. Varsayılan ve önerilen değer: 0. Bölüm iyi durumda olmadığı kabul edebilmesi iyi durumda olmayan bölüm çoğaltmaları sayısı belirtin. |
| ServiceTypeHealthPolicyMap | PS, VS | Bir hizmet türüne ait hizmetlerin durumunu değerlendirmek için kullanılan sistem durumu ilkesi temsil eder. Şu biçimde bir karma tablo girişi alır: @ {"Servicetypename birlikte belirtilemez": "MaxPercentUnhealthyPartitionsPerService, MaxPercentUnhealthyReplicasPerPartition, MaxPercentUnhealthyServices"} Örneğin: @{"ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5"} |
| TimeoutSec | PS, VS | İşlemi için saniye cinsinden zaman aşımı süresini belirtir. |
| UpgradeDomainTimeoutSec |PS, VS |Tek bir yükseltme etki alanını yükseltmek için en uzun süreyi (saniye cinsinden). Bu zaman aşımı ulaşılırsa yükseltme durdurur ve ayarını temel alınarak geçer *FailureAction*. Varsayılan değer hiçbir zaman: (sonsuz) ve uygulamanız için uygun biçimde özelleştirilmelidir. |
| UpgradeReplicaSetCheckTimeoutSec |PS, VS |Saniye cinsinden ölçülür.<br>**Durum bilgisi olmayan hizmet**--hizmetin ek örneklerini kullanılabilir olmasını sağlamak Service Fabric tek bir yükseltme etki alanında çalışır. Service Fabric hedef örnek sayısı, birden fazla ise, birden fazla örneğini bir maksimum zaman aşımı değeri en fazla kullanılabilir olmasını bekler. Bu zaman aşımı kullanılarak belirtilen *UpgradeReplicaSetCheckTimeoutSec* özelliği. Zaman aşımı süresi dolarsa, Service Fabric service örnek sayısından bağımsız olarak yükseltme işlemine devam eder. Hedef örnek sayısını ise, Service Fabric beklememeyi ve hemen yükseltme işlemine devam eder.<br><br>**Durum bilgisi olan hizmet**--çoğaltma kümesine bir çekirdek olduğundan emin olmak Service Fabric tek bir yükseltme etki alanında çalışır. Service Fabric bir çekirdek bir maksimum zaman aşımı değeri en fazla kullanılabilir olmasını bekler (tarafından belirtilen *UpgradeReplicaSetCheckTimeoutSec* özelliği). Zaman aşımı süresi dolarsa, Service Fabric çekirdek bakılmaksızın yükseltme işlemine devam eder. Bu ayar kümesi olarak hiçbir zaman (sonsuz) İleri sarmadır olduğunda ve 1200 saniye olan zaman geri alınıyor. |
| UpgradeTimeoutSec |PS, VS |Tüm yükseltme için geçerli bir zaman aşımı (saniye cinsinden). Bu zaman aşımı ulaşılırsa, yükseltme durdurulur ve *FailureAction* tetiklenir. Varsayılan değer hiçbir zaman: (sonsuz) ve uygulamanız için uygun biçimde özelleştirilmelidir. |
| WhatIf | PS | İzin verilen değerler **True** ve **False**. Cmdlet çalıştırılıyorsa ne olacağını gösterir. Cmdlet çalıştırılmaz. |

*MaxPercentUnhealthyServices*, *MaxPercentUnhealthyPartitionsPerService*, ve *MaxPercentUnhealthyReplicasPerPartition* ölçüt başına belirtilebilir bir uygulama örneği için hizmet türü. Bu parametreleri başına hizmet ayarı sağlayan bir uygulamanın farklı değerlendirme ilkeleriyle farklı Hizmetleri türler bulunur. Örneğin, bir durum bilgisi olmayan bir ağ geçidi hizmet türü olabilir bir *MaxPercentUnhealthyPartitionsPerService* belirli bir uygulama örneği için bir durum bilgisi olan Altyapısı hizmeti türünden farklı.

## <a name="sfctl-parameters"></a>SFCTL parametreleri

Service Fabric CLI kullanarak Service Fabric uygulama yükseltme [sfctl uygulama yükseltmesi](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-upgrade) aşağıdaki gerekli ve isteğe bağlı parametreler birlikte komutu.

### <a name="required-parameters"></a>Gerekli Parametreler

| Parametre | Açıklama |
| --- | --- |
| Uygulama Kimliği  |Yükseltilmekte olan uygulama kimliği. <br> Bu genellikle uygulamayı olmadan tam adı, ' fabric:' URI şeması. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış '\~' karakter. Örneğin, uygulama adı ise ' fabric: / myapp/app1 ', uygulama kimliği olur ' Uygulamam\~app1' 6.0 + ve ' myapp/app1' in önceki sürümlerindeki.|
Uygulama sürümü |Uygulama sürümü türü yükseltme hedefler.|
parametreler  |Uygulama yükseltme sırasında uygulanacak parametr aplikace JSON olarak kodlanmış listesi geçersiz kılar.|

### <a name="optional-parameters"></a>İsteğe bağlı parametreler

| Parametre | Açıklama |
| --- | --- |
Varsayılan-hizmet-sistem durumu ilkesi | [JSON](https://docs.microsoft.com/rest/api/servicefabric/sfclient-model-servicetypehealthpolicy) kodlanmış varsayılan olarak bir hizmet türünün durumunu değerlendirmek için kullanılan sistem durumu ilkesi belirtimi. Harita, varsayılan olarak boştur. |
Hata eylemi | İzin verilen değerler **geri alma**, **el ile**, ve **geçersiz**. Ne zaman telafi gerçekleştirilecek bir *izlenen* ilke ihlallerini ilke veya sistem durumu izleme karşılaştığı yükseltin. <br>**Geri alma** yükseltme otomatik olarak yükseltme öncesi sürüme geri döner olduğunu belirtir. <br>**El ile** yükseltme geçer gösterir *UnmonitoredManual* yükseltme modu. <br>**Geçersiz** hatası eylemi geçersiz olduğunu gösterir.|
zorla-yeniden başlatma | Hizmet kodu güncelleştirmeden bir yapılandırma veya veri paketi güncelleştirirseniz yalnızca ForceRestart özelliği ayarlanmışsa hizmet yeniden **True**. Güncelleştirme tamamlandığında, Service Fabric hizmeti, yeni yapılandırma paketi veya veri paketi kullanılabilir olduğunu bildirir. Değişiklikleri uygulamak için sorumlu hizmettir. Gerekirse, hizmetin kendisini yeniden başlatabilirsiniz. |
Sistem durumu-onay-yeniden-zaman aşımı | Uygulama veya küme önce sistem durumu kötü olduğunda sistem durumu değerlendirmesi yeniden denemek için süreyi *FailureAction* yürütülür. Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. Varsayılan: PT0H10M0S. |
Sistem durumu denetimi kararlı süresi | Süreyi sonraki yükseltme etki alanına yükseltmeye devam etmeden önce uygulama veya kümenin sağlıklı kalmasını gerekir. Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. Varsayılan: PT0H2M0S. |
Sistem durumu denetimi bekleme süresi | Sistem durumu ilkeleri uygulanmadan önce bir yükseltme etki alanını tamamladıktan sonra beklenecek süre miktarı. Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. Varsayılan: 0.|
en fazla sağlıksız-uygulamalar | Varsayılan ve önerilen değer: 0. Dağıtılan uygulamalar maksimum sayısını belirtin (bkz [durumu bölümünün](service-fabric-health-introduction.md)) olabilecek kötü önce uygulama durumu kötü olarak değerlendirilir ve yükseltme başarısız olur. Bu parametre, düğüm üzerinde uygulama durumunu tanımlar ve yükseltme sırasında sorunları algılamaya yardımcı olur. Genellikle, uygulamanın çoğaltmaların iyi ve bu nedenle devam etmek yükseltme izin görüntülenecek uygulama sağlayan diğer düğüm, yük dengeli alın. Bir katı belirterek *en fazla sağlıksız-apps* sistem durumu, Service Fabric uygulama paketi ile ilgili bir sorun hızlı bir şekilde algılayın ve hızlı yükseltme yük devretme elde etmeye yardımcı. 0 ile 100 arasında bir sayı olarak temsil edilir. |
mode | İzin verilen değerler **izlenen**, **UpgradeMode**, **UnmonitoredAuto**, **UnmonitoredManual**. Varsayılan değer **UnmonitoredAuto**. Visual Studio ve PowerShell *gerekli parametreleri* bölümünde bu değerlerin açıklaması.|
çoğaltma-kümesi-onay-zaman aşımı |Saniye cinsinden ölçülür. <br>**Durum bilgisi olmayan hizmet**--hizmetin ek örneklerini kullanılabilir olmasını sağlamak Service Fabric tek bir yükseltme etki alanında çalışır. Service Fabric hedef örnek sayısı, birden fazla ise, birden fazla örneğini bir maksimum zaman aşımı değeri en fazla kullanılabilir olmasını bekler. Bu zaman aşımı kullanılarak belirtilen *çoğaltma-kümesi-onay-zaman aşımı* özelliği. Zaman aşımı süresi dolarsa, Service Fabric service örnek sayısından bağımsız olarak yükseltme işlemine devam eder. Hedef örnek sayısını ise, Service Fabric beklememeyi ve hemen yükseltme işlemine devam eder.<br><br>**Durum bilgisi olan hizmet**--çoğaltma kümesine bir çekirdek olduğundan emin olmak Service Fabric tek bir yükseltme etki alanında çalışır. Service Fabric bir çekirdek bir maksimum zaman aşımı değeri en fazla kullanılabilir olmasını bekler (tarafından belirtilen *çoğaltma-kümesi-onay-zaman aşımı* özelliği). Zaman aşımı süresi dolarsa, Service Fabric çekirdek bakılmaksızın yükseltme işlemine devam eder. Bu ayar kümesi olarak hiçbir zaman (sonsuz) İleri sarmadır olduğunda ve 1200 saniye olan zaman geri alınıyor. |
Hizmet sistem durumu ilkesi | JSON hizmet türü sistem durumu ilkesi başına hizmet türü adı eşlemeyle kodlanmış. Bir eşlem boşsa, varsayılan olarak. [Parametre JSON biçimi. ](https://docs.microsoft.com/rest/api/servicefabric/sfclient-model-applicationhealthpolicy#servicetypehealthpolicymap). "Value" bölümü için JSON içeriyor **MaxPercentUnhealthyServices**, **MaxPercentUnhealthyPartitionsPerService**, ve **MaxPercentUnhealthyReplicasPerPartition**. Bu parametre açıklamaları için Visual Studio ve PowerShell isteğe bağlı parametreler bölümüne bakın.
timeout | İşlemi için saniye cinsinden zaman aşımı süresini belirtir. Varsayılan: 60. |
Yükseltme etki alanı timeout | Önce tamamlamak, her bir yükseltme etki alanı süreyi sahip *FailureAction* yürütülür. Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. Varsayılan değer hiçbir zaman: (sonsuz) ve uygulamanız için uygun biçimde özelleştirilmelidir. Varsayılan: P10675199DT02H48M05.4775807S. |
Yükseltme zaman aşımı | Önce tamamlamak, her bir yükseltme etki alanı süreyi sahip *FailureAction* yürütülür. Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. Varsayılan değer hiçbir zaman: (sonsuz) ve uygulamanız için uygun biçimde özelleştirilmelidir. Varsayılan: P10675199DT02H48M05.4775807S.|
warning-as-error | İzin verilen değerler **True** ve **False**. Varsayılan ayar, **False** değeridir. İçinde bir bayrak olarak geçirilebilir. Uygulama için uyarı sistem durumu olayları, yükseltme sırasında durumunu değerlendirilirken hata olarak değerlendir. Varsayılan olarak, Service Fabric uyarı olayları olsa bile, yükseltme devam edebilmeniz için hatası (hata) gibi uyarı sistem durumu olayları değerlendirmez. |

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamayı kullanarak Visual Studio yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltmesi size yol gösterir.

[Uygulama Powershell kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltmesi size yol gösterir.

[Linux üzerinde Service Fabric CLI kullanarak uygulamanızı yükseltmek](service-fabric-application-lifecycle-sfctl.md#upgrade-application) Service Fabric CLI kullanarak bir uygulama yükseltmesi size yol gösterir.

[Service Fabric Eclipse eklentisi kullanarak uygulamanızı yükseltme](service-fabric-get-started-eclipse.md#upgrade-your-service-fabric-java-application)

Nasıl kullanılacağını öğrenerek, uygulama yükseltmeleri uyumlu olma [veri seri hale getirme](service-fabric-application-upgrade-data-serialization.md).

Uygulamanızı yükseltilirken başvurarak gelişmiş işlevselliği kullanmanıza öğrenin [Gelişmiş konular](service-fabric-application-upgrade-advanced.md).

Yaygın sorunlar uygulama yükseltmeleri adımları başvurarak düzeltme [uygulama yükseltme sorunlarını giderme](service-fabric-application-upgrade-troubleshooting.md).
