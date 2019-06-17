---
title: Service Fabric uygulaması yükseltme | Microsoft Docs
description: Bu makalede, Service Fabric uygulaması yükseltme seçme modları ve sistem durumu denetimleri gerçekleştirmek de dahil olmak üzere, yükseltme için bir giriş sağlar.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: 803c9c63-373a-4d6a-8ef2-ea97e16e88dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: e2b407733bcab7bc854e8e3703e53eb474f3425b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60615060"
---
# <a name="service-fabric-application-upgrade"></a>Service Fabric uygulaması yükseltme
Azure Service Fabric uygulama hizmetleri koleksiyonudur. Yükseltme sırasında Service Fabric yeni karşılaştırır [uygulama bildirimini](service-fabric-application-and-service-manifests.md) önceki sürümle ve hangi uygulama iste güncelleştirmeleri Hizmetleri belirler. Service Fabric, önceki sürümde sürüm numaraları ile sayılar hizmetinde bildirimlerini sürüm karşılaştırır. Bu hizmet, hizmet değişmemişse yükseltilmez.

## <a name="rolling-upgrades-overview"></a>Sıralı yükseltmeler genel bakış
Bir sıralı uygulama yükseltmesi, yükseltmeyi aşamalı olarak gerçekleştirilir. Her aşamada bir güncelleme etki alanı adı, kümedeki düğümler kümesini yükseltme uygulanır. Sonuç olarak, uygulama yükseltme kullanılmaya devam edilebilir. Yükseltme sırasında küme, eski ve yeni sürümleri bir karışımını içerebilir.

Bu nedenle, iki sürüm ileriye ve geriye dönük uyumlu. Uyumlu değillerse, kullanılabilirliği sürdürmek için birden çok aşama yükseltme hazırlama için uygulama Yöneticisi sorumludur. Bir birden çok aşama yükseltmesinin ilk adımı bir önceki sürümüyle uyumlu uygulama ara bir sürümüne yükseltirken lütfen bekleyin. İkinci adım, güncelleştirme öncesi sürümüyle uyumluluğu keser, ancak ara sürümü ile uyumlu olan en son sürüme yükseltin sağlamaktır.

Kümesini yapılandırırken güncelleştirme etki alanları küme bildiriminde belirtilir. Güncelleştirme etki alanları, belirli bir sırada güncelleştirmeleri almaz. Bir güncelleme etki alanı için bir uygulama dağıtımının mantıksal bir birimdir. Güncelleme etki alanı Hizmetleri, yükseltme sırasında yüksek kullanılabilirlik kalmasına izin verin.

Olmayan sıralı yükseltmeler, yükseltme, söz konusu uygulamanın yalnızca bir güncelleme etki alanı olduğunda olduğu kümedeki tüm düğümlere uygulanırsa mümkündür. Bu yaklaşım önerilmez, bu hizmetin çökmesi ve yükseltme sırasında kullanılamaz. Ayrıca, yalnızca bir güncelleme etki alanı ile bir küme ayarlandığında Azure hiçbir garanti sağlamaz.

Tüm yükseltme başarılı olursa, hizmetleri ve replicas(instances) aynı sürüm-ör kalın yükseltme, tamamlandıktan sonra yeni sürüme güncelleştirilir; yükseltme başarısız olur ve geri alınamaz, bunlar eski sürüme geri.

## <a name="health-checks-during-upgrades"></a>Yükseltme sırasında sistem durumu denetimleri
Yükseltme için sistem durumu ilkeleri ayarlamak sahip (veya varsayılan değerleri kullanılan) kullanın. Yükseltme tüm güncelleştirme etki alanları belirtilen zaman aşımlarını yükseltildiğinde ve tüm güncelleştirme etki alanları sağlıklı olarak kabul edilen başarılı olarak adlandırılır.  Sağlıklı bir güncelleştirme etki alanı, güncelleştirme etki sağlık ilkesinde belirtilen tüm sistem durumu denetimleri geçirilen anlamına gelir. Örneğin, bir sistem durumu ilkesi uygulama örneğini içindeki tüm hizmetleri olmalıdır kıldığı *sağlıklı*gibi sistem durumu, Service Fabric tarafından tanımlanır.

Sistem durumu ilkeleri ve denetimleri tarafından Service Fabric yükseltme sırasında hizmet ve uygulama bağımsızdır. Diğer bir deyişle, hizmete özgü test gerçekleştirilir.  Örneğin, hizmetiniz bir aktarım hızı gereksinim olabilir, ancak Service Fabric aktarım hızı denetlemek için bilgi yok. Başvurmak [sistem durumu makaleleri](service-fabric-health-introduction.md) gerçekleştirilen denetimler için. Örneği başlatıldı olmadığını gerçekleşen uygulama paketi doğru olarak kopyalanmış olması için bir yükseltme INCLUDE testleri sırasında denetimleri ve benzeri.

Uygulama, uygulamanın alt varlıklar toplamını durumudur. Kısacası, Service Fabric uygulaması üzerinde bildirilen sistem üzerinden uygulama durumunu değerlendirir. Ayrıca bu şekilde uygulama için tüm hizmetlerin durumunu değerlendirir. Service Fabric daha fazla hizmet çoğaltması gibi çocuklarına durumunu toplayarak uygulama hizmetlerin durumunu değerlendirir. Uygulama durumu ilkesi tatmin olduktan sonra yükseltme devam edebilirsiniz. Sistem durumu ilkesi ihlal edilirse uygulama yükseltme başarısız olur.

## <a name="upgrade-modes"></a>Yükseltme modu
Uygulama yükseltmesi için önerdiğimiz modu yaygın olarak kullanılan mod izlenen modudur. İzlenen modu bir güncelleme etki alanında yükseltmeyi gerçekleştirir ve tüm sistem durumu denetimleri, geçişi (belirtilen ilke başına), geçer Sonraki güncelleştirme etki alanına otomatik olarak.  Sistem durumu denetimi başarısız ve/veya zaman aşımları ulaşıldığında, yükseltme ya da geri güncelleştirme etki alanı için alınır veya modu izlenmeyen el ile olarak değişir. Yükseltme başarısız yükseltmeleri için bu iki moddan birini yapılandırabilirsiniz. 

İzlenmeyen el ile modu, sonraki güncelleştirme etki alanı yükseltmeyi hız kazandırın için bir güncelleştirme etki alanındaki her yükseltme sonrasında el ile müdahale gerekir. Herhangi bir Service Fabric sistem durumu denetimi gerçekleştirilir. Yönetici sonraki güncelleştirme etki alanında Yükseltmeyi başlatmadan önce sistem durumu veya durumu denetimleri gerçekleştirir.

## <a name="upgrade-default-services"></a>Varsayılan hizmetler yükseltme
Tanımlanan bazı varsayılan hizmet parametreleri [uygulama bildirimini](service-fabric-application-and-service-manifests.md) uygulama yükseltme işleminin bir parçası olarak da yükseltilebilir. Aracılığıyla değiştirilmesini destekleyen hizmet parametreleri [güncelleştirme ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/update-servicefabricservice?view=azureservicefabricps) yükseltme işleminin bir parçası olarak değiştirilebilir. Uygulama yükseltme sırasında varsayılan hizmetler değiştirme davranış aşağıdaki gibidir:

1. Kümede bulunmayan yeni bir uygulama bildirimi varsayılan hizmetler oluşturuluyor.
2. Her iki önceki ve yeni uygulama bildirimleri mevcut varsayılan hizmetler güncelleştirilir. Yeni uygulama bildiriminde varsayılan hizmet parametreleri mevcut hizmet parametreleri üzerine yazın. Uygulama yükseltmesi geri alır. otomatik olarak varsayılan hizmet güncelleştirme başarısız olursa.
3. Kümede varsa yeni uygulama bildiriminde yok varsayılan hizmetler silinir. **Varsayılan hizmet silme tüm hizmet silme sonuçlanacağını unutmayın, kullanıcının durumu ve geri alınamaz.**

Uygulama yükseltme geri alınır, varsayılan hizmet parametreleri silinen Hizmetleri ile eski durumlarına yeniden oluşturulamaz ancak Yükseltmeyi başlatmadan önce eski değerlerine geri döndürülür.

> [!TIP]
> [Enabledefaultservicesupgrade özelliğini](service-fabric-cluster-fabric-settings.md) küme yapılandırma ayarı olmalıdır *true* 2 kurallarını etkinleştirmek için) ve 3) (varsayılan hizmet güncelleştirme ve silme). Bu özellik, Service Fabric sürüm 5.5 başlayarak desteklenir.

## <a name="upgrading-multiple-applications-with-https-endpoints"></a>HTTPS uç noktaları ile birden çok uygulama yükseltme
Kullanma konusunda dikkatli olmanız gerekir **aynı bağlantı noktasını** için HTTP kullanırken aynı uygulamanın farklı örneklerinde**S**. Service Fabric uygulama örneklerinden birine için sertifika yükseltme imkanınız olmayacaktır nedenidir. Örneğin, uygulama 1 ya da uygulama 2 hem kendi sertifikası 1 sertifika 2 yükseltmek istiyorsanız. Yükseltme gerçekleştiğinde, diğer uygulama hala kullanarak olsa da Service Fabric sertifikası 1 kayıt http.sys ile yukarı temizlendi. Bunu önlemek için Service Fabric, zaten başka bir uygulama örneği bağlantı noktası (http.sys) nedeniyle sertifika ile kayıtlı ve işlem başarısız algılar.

Bu nedenle Service Fabric kullanarak iki farklı hizmet yükseltmeyi desteklemez **aynı bağlantı noktasını** farklı uygulama örneklerinin içinde. Diğer bir deyişle, aynı bağlantı noktasında farklı Hizmetleri aynı sertifikayı kullanamazsınız. Aynı bağlantı noktasında paylaşılan bir sertifikanız gerekiyorsa, hizmetleri yerleştirme kısıtlamaları ile farklı makinelerde yerleştirildiğinden emin olmak gerekir. Veya Service Fabric dinamik bağlantı noktaları mümkün olduğu durumlarda her uygulama örneğinin her bir hizmet için kullanmayı düşünün. 

Https, "Windows HTTP sunucu API birden çok sertifika bir bağlantı noktasını paylaşan uygulamalar için desteklemiyor." belirten uyarı bir hata ile yükseltme bir hata görürseniz

## <a name="application-upgrade-flowchart"></a>Uygulama yükseltme akış çizelgesi
Bu paragrafın aşağıdaki akış, bir Service Fabric uygulaması yükseltme işlemini anlamanıza yardımcı olabilir. Özellikle, akışı açıklar nasıl dahil olmak üzere zaman aşımları *HealthCheckStableDuration*, *HealthCheckRetryTimeout*, ve *UpgradeHealthCheckInterval*, Yardım bir güncelleme etki alanı yükseltmesinde bir başarı veya hata olarak kabul ettiğinde denetimi.

![Bir Service Fabric uygulaması için yükseltme işlemi][image]

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamayı kullanarak Visual Studio yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltmesi size yol gösterir.

[Uygulama PowerShell kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltmesi size yol gösterir.

Uygulamanızı kullanarak yükseltme nasıl kontrol [yükseltme parametreleri](service-fabric-application-upgrade-parameters.md).

Nasıl kullanılacağını öğrenerek, uygulama yükseltmeleri uyumlu olma [veri seri hale getirme](service-fabric-application-upgrade-data-serialization.md).

Uygulamanızı yükseltilirken başvurarak gelişmiş işlevselliği kullanmanıza öğrenin [Gelişmiş konular](service-fabric-application-upgrade-advanced.md).

Yaygın sorunlar uygulama yükseltmeleri adımları başvurarak düzeltme [uygulama yükseltme sorunlarını giderme](service-fabric-application-upgrade-troubleshooting.md).

[image]: media/service-fabric-application-upgrade/service-fabric-application-upgrade-flowchart.png
