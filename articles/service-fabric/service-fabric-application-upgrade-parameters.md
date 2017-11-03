---
title: "Uygulama yükseltme: yükseltme parametreleri | Microsoft Docs"
description: "Sistem durumu denetimleri gerçekleştirmek ve otomatik olarak yükseltmeyi geri alma ilkeleri dahil olmak üzere bir Service Fabric uygulaması yükseltmeyle ilgili parametreler açıklanmaktadır."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: a4170ac6-192e-44a8-b93d-7e39c92a347e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: f09dad590f32c10f75484bba9afb7ea60f29d81e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="application-upgrade-parameters"></a>Uygulama yükseltme parametreleri
Bu makalede Azure Service Fabric uygulama yükseltme sırasında geçerli çeşitli parametreler açıklanmaktadır. Parametreleri adını ve uygulamanın sürümünü içerir. Zaman aşımları ve yükseltme sırasında uygulanan sistem durumu denetimlerini denetleme düğmelerini oldukları ve bir yükseltme başarısız olduğunda uygulanması gereken ilkeleri belirtin.

<br>

| Parametre | Açıklama |
| --- | --- |
| ApplicationName |Yükseltilmekte olan uygulamanın adı. Örnekler: fabric: / VisualObjects, fabric: / ClusterMonitor |
| TargetApplicationTypeVersion |Uygulama sürümü yazın yükseltme hedefler. |
| FailureAction |Yükseltme başarısız olduğunda Service Fabric tarafından gerçekleştirilecek eylem. Uygulama (rollback) güncelleştirme öncesi sürümüne geri alınması veya yükseltme geçerli yükseltme etki alanında durdurulmuş olabilir. İkinci durumda, yükseltme modu da el ile olarak değişir. Geri alma ve el ile izin verilen değerleri şunlardır. |
| HealthCheckWaitDurationSec |Service Fabric önce yükseltme etki alanı yükseltme tamamlandıktan sonra (saniye cinsinden) beklenecek süreyi uygulamanın durumunu değerlendirir. Bu sürenin sağlıklı değerlendirilebilir önce uygulamanın çalıştırılması zaman olarak kabul edilebilir. Sistem durumu denetimi başarılı olursa, sonraki yükseltme etki alanı yükseltme işlemine devam eder.  Sistem durumu denetimi başarısız olursa, Service Fabric (UpgradeHealthCheckInterval) için bir aralığı HealthCheckRetryTimeout ulaşılana kadar sistem durumu denetimi yeniden denemeden önce bekler. Varsayılan ve önerilen değer 0 ise saniyeyi ifade eder. |
| HealthCheckRetryTimeoutSec |Süre (saniye cinsinden) yükseltme başarısız olarak bildirme önce sistem durumu değerlendirmesi gerçekleştirmek, Service Fabric devam eder. 600 saniye varsayılandır. Bu süre HealthCheckWaitDuration ulaşıldıktan sonra başlar. Bu HealthCheckRetryTimeout içinde Service Fabric uygulama sistem durumunun birden fazla sistem durumu denetimi gerçekleştirebilir. Varsayılan değer 10 dakikadır ve uygulamanız için uygun şekilde özelleştirilmiş. |
| HealthCheckStableDurationSec |Süre (saniye cinsinden) uygulamayı bir sonraki yükseltme etki alanına taşıma veya yükseltme işlemini tamamladıktan önce tutarlı olduğunu doğrulamak için. Bu bekleme süresi, sistem durumu denetimi gerçekleştirildikten sonra sistem durumu sağ algılanmayan değişiklikleri önlemek için kullanılır. Varsayılan değer 120 saniye ve uygulamanız için uygun şekilde özelleştirilmiş. |
| UpgradeDomainTimeoutSec |Tek bir yükseltme etki alanına yükseltmek için en uzun süre (saniye cinsinden). Bu zaman aşımı ulaştıysanız, yükseltme durdurur ve UpgradeFailureAction ayarını temel alınarak devam eder. Varsayılan değer hiçbir zaman'dir (sonsuz) ve uygulamanız için uygun şekilde özelleştirilebilir. |
| UpgradeTimeout |Tüm yükseltme için geçerli bir zaman aşımı (saniye cinsinden). Bu zaman aşımı ulaştıysanız yükseltme durur ve UpgradeFailureAction tetiklenir. Varsayılan değer hiçbir zaman'dir (sonsuz) ve uygulamanız için uygun şekilde özelleştirilebilir. |
| UpgradeHealthCheckInterval |Sistem durumu denetlenir sıklığı. Bu parametre ClusterManager bölümünde belirtilen *küme* *bildirim*ve yükseltme cmdlet'inin bir parçası olarak belirtilmemiş. Varsayılan değer 60 saniyedir. |

<br>

## <a name="service-health-evaluation-during-application-upgrade"></a>Uygulama yükseltme sırasında hizmet sistem durumu değerlendirmesi
<br>
Sistem durumu değerlendirme ölçütleri isteğe bağlıdır. Yükseltme başladığında, sistem durumu değerlendirme ölçütleri belirtilmezse, Service Fabric uygulaması örneği ApplicationManifest.xml içinde belirtilen uygulama sistem durumu ilkeleri kullanır.

<br>

| Parametre | Açıklama |
| --- | --- |
| ConsiderWarningAsError |Varsayılan değer False olur. Uygulama için uyarı sistem durumu olayları, yükseltme sırasında uygulama durumunu değerlendirilirken hata olarak işler. Varsayılan olarak, Service Fabric uyarı olayları olsa bile yükseltme devam edebilmeniz için hatası (hata) gibi uyarı sistem durumu olayları değerlendirmez. |
| MaxPercentUnhealthyDeployedApplications |Varsayılan ve önerilen değer: 0. Dağıtılan uygulamalar en fazla sayısını belirtin (bkz [sistem bölümü](service-fabric-health-introduction.md)), olabilir sağlıksız önce uygulama kötü olarak değerlendirilir ve yükseltme başarısız olur. Bu parametre uygulama sistem durumu düğümünde tanımlar ve yükseltme sırasında sorunlarını algılamaya yardımcı olur. Genellikle, yük dengeli sağlıklı, böylece devam etmek için Yükselt sağlar görünmesi uygulama sağlayan başka bir düğüme, uygulama, çoğaltmaları alın. Service Fabric, katı MaxPercentUnhealthyDeployedApplications sağlık durumunu belirterek, uygulama paketi ile ilgili bir sorun hızla algılayabilir ve başarısız hızlı yükseltme oluşturulmasını sağlamak. |
| MaxPercentUnhealthyServices |Varsayılan ve önerilen değer: 0. Uygulama kötü olarak değerlendirilir ve yükseltme başarısız olduğunda önce düzgün çalışmayan uygulama örneğinde Hizmetleri en fazla sayısını belirtin. |
| MaxPercentUnhealthyPartitionsPerService |Varsayılan ve önerilen değer: 0. En çok bölüm sayısı hizmet sağlıksız kabul edilmeden önce sağlıksız bir hizmet belirtin. |
| MaxPercentUnhealthyReplicasPerPartition |Varsayılan ve önerilen değer: 0. Çoğaltmaları sayısının bölüm sağlıksız kabul edilmeden önce sağlıksız bölümünde belirtin. |
| UpgradeReplicaSetCheckTimeout |**Durum bilgisiz hizmet**--tek bir yükseltme etki alanında ek hizmet örneklerini kullanılabilir olduğundan emin olmak Service Fabric çalışır. Hedef örnek sayısı birden fazla ise, Service Fabric kadar en büyük zaman aşımı değeri kullanılabilir olması birden fazla örneği bekler. Bu zaman aşımı UpgradeReplicaSetCheckTimeout özelliğini kullanarak belirtilir. Zaman aşımı süresi dolarsa, Service Fabric hizmet örneği sayısından bağımsız olarak yükseltme devam eder. Hedef örnek sayısı ise, Service Fabric beklememek ve hemen yükseltme işlemine devam eder. **Durum bilgisi olan hizmet**--çoğaltma kümesi çekirdeği olduğundan emin olmak Service Fabric tek bir yükseltme etki alanında çalışır. Service Fabric (UpgradeReplicaSetCheckTimeout özelliği tarafından belirtilen) en büyük zaman aşımı değerine kadar kullanılabilir olması bir çekirdek bekler. Zaman aşımı süresi dolarsa, Service Fabric çekirdek bağımsız olarak yükseltme devam eder. Bu ayar kümesi olarak hiçbir zaman (İleri sunulurken sonsuz) ve 900 saniye olan zaman geri alınıyor. |
| ForceRestart |Hizmet koduna güncelleştirmeden yapılandırma veya veri paketi güncelleştirirseniz ForceRestart özelliği yalnızca ayarlanmışsa, hizmet yeniden true. Güncelleştirme tamamlandığında, Service Fabric hizmeti, yeni bir yapılandırma paketi veya veri paketi kullanılabilir olduğunu bildirir. Hizmet, değişiklikleri uygulamak için sorumludur. Gerekirse, hizmetin kendisini yeniden başlatabilirsiniz. |

<br>
<br>
Hizmet türü için bir uygulama örneği başına MaxPercentUnhealthyServices, MaxPercentUnhealthyPartitionsPerService ve MaxPercentUnhealthyReplicasPerPartition ölçütlerini belirtilebilir. Bu parametreleri başına hizmet ayarlamayı sağlar farklı değerlendirme ilkeleriyle farklı Hizmetleri türlerini içeren bir uygulama için. Örneğin, bir durum bilgisi olmayan ağ geçidi hizmet türü belirli uygulama örneği için bir durum bilgisi olan Altyapısı hizmeti türünden farklı bir MaxPercentUnhealthyPartitionsPerService sahip olabilir.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama kullanarak Visual Studio yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltme yol gösterir.

[Uygulama Powershell kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltme yol gösterir.

[Linux'ta Service Fabric CLI kullanarak uygulamanızı yükseltme](service-fabric-application-lifecycle-sfctl.md#upgrade-application) Service Fabric CLI kullanarak bir uygulama yükseltme yoluyla anlatılmaktadır.

[Service Fabric Eclipse eklentisi kullanarak uygulamanızı yükseltme](service-fabric-get-started-eclipse.md#upgrade-your-service-fabric-java-application)

Uygulama yükseltme nasıl kullanılacağını öğrenerek uyumlu hale getirmek [veri seri hale getirme](service-fabric-application-upgrade-data-serialization.md).

Gelişmiş işlevselliği başvurarak uygulamanızı yükseltirken kullanmayı öğrenin [Gelişmiş konular](service-fabric-application-upgrade-advanced.md).

Adımlarına bakarak uygulama yükseltmeleri sık karşılaşılan sorunları düzeltmek [sorun giderme uygulama yükseltmelerini](service-fabric-application-upgrade-troubleshooting.md).
