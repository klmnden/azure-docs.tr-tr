---
title: "Service Fabric uygulama yükseltme | Microsoft Docs"
description: "Bu makalede seçme yükseltme modları ve gerçekleştirme durumu denetimleri de dahil olmak üzere bir Service Fabric uygulama yükseltme giriş bilgileri sağlar."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 803c9c63-373a-4d6a-8ef2-ea97e16e88dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 43e1a66c3aca882f8f572d2bf71976d6b65a9c68
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="service-fabric-application-upgrade"></a>Service Fabric uygulaması yükseltme
Azure Service Fabric uygulama hizmetleri koleksiyonudur. Yükseltme sırasında Service Fabric yeni karşılaştırır [uygulama bildirimi](service-fabric-application-model.md#describe-an-application) önceki sürümüyle ve hangi uygulama iste güncelleştirmeleri Hizmetleri belirler. Service Fabric önceki sürümde sürüm numaralarıyla numaraları hizmet bildirimlerini sürüm karşılaştırır. Bu hizmet, hizmet değişmemişse yükseltilmez.

## <a name="rolling-upgrades-overview"></a>Yükseltmeler genel bakış alınıyor
Çalışırken uygulama yükseltmesinde, yükseltme aşamada gerçekleştirilir. Her aşamada yükseltme bir güncelleştirme etki alanı adı verilen kümedeki düğümlerin bir alt uygulanır. Sonuç olarak, uygulama yükseltme kullanılabilir olarak kalır. Yükseltme sırasında küme eski ve yeni sürümlerin bir karışımını içerebilir.

Bu nedenle, iki sürüm ileriye ve geriye dönük uyumlu. Uyumlu değillerse, uygulama Yöneticisi kullanılabilirliğini sağlamak için birden çok aşaması yükseltme hazırlama için sorumludur. Bir birden çok aşama yükseltmesinin ilk adım, önceki sürümü ile uyumlu olan uygulama ara sürümünden yükseltiyor. Güncelleştirme öncesi sürümüyle uyumluluğu sonlarını ancak ara sürümü ile uyumlu olan en son sürümüne yükseltmek için ikinci adım olacaktır.

Kümeyi yapılandırırken güncelleştirme etki alanı küme bildiriminde belirtilir. Güncelleştirme etki alanları, belirli bir sırada güncelleştirmeleri almaz. Bir güncelleştirme etki alanı için bir uygulama dağıtımının mantıksal bir birimdir. Güncelleştirme etki alanı Hizmetleri yükseltme sırasında yüksek kullanılabilirliğine kalmasına olanak sağlar.

Yükseltme söz konusu uygulamanın yalnızca bir güncelleştirme etki alanına sahip olduğunda olduğu kümedeki tüm düğümlere uygulanırsa olmayan çalışırken yükseltme mümkündür. Bu yaklaşım önerilmez, bu hizmet arıza ve yükseltme sırasında kullanılamaz. Ayrıca, bir küme yalnızca tek bir güncelleştirme etki alanı ile ayarlandığında, Azure garanti sağlamaz.

Yükseltme, tüm yükseltme başarılı olursa, hizmetleri ve replicas(instances) aynı sürüm-yani, kalır işlemi tamamlandıktan sonra yeni sürüme güncelleştirilir; yükseltme başarısız olur ve ise geri, bunlar eski sürüme geri.

## <a name="health-checks-during-upgrades"></a>Yükseltme sırasında sistem durumu denetimleri
Bir yükseltme için sistem durumu ilkeleri ayarlanmış olması gerekir (veya varsayılan değerleri kullanılabilir) kullanın. Bir yükseltme tüm güncelleştirme etki alanları belirtilen zaman aşımlarını yükseltildiğinde ve tüm güncelleştirme etki alanları sağlıklı olarak kabul edilen başarılı olarak adlandırılır.  Sağlıklı güncelleştirme etki alanı güncelleştirme etki alanı sistem durumu İlkesi'nde belirtilen tüm sistem durumu denetimleri geçirilen anlamına gelir. Örneğin, bir sistem durumu ilkesi uygulama örneğini içindeki tüm hizmetler olmalıdır zorunlu kılabilir *sağlıklı*, sistem durumu Service Fabric tarafından tanımlanan.

Sistem durumu ilkeleri ve denetimler Service Fabric tarafından yükseltme sırasında hizmet ve uygulama bağımsızdır. Diğer bir deyişle, hizmete özgü testleri yapılır.  Örneğin, hizmetiniz bir işleme gereksinimi olabilir, ancak Service Fabric verimlilik denetlemek için bilgileri yok. Başvurmak [sistem durumu makaleleri](service-fabric-health-introduction.md) gerçekleştirilen denetimleri için. Örnek başlayıp başlamadığını uygulama paketi doğru kopyalanmıştır bir yükseltme INCLUDE testleri sırasında gerçekleşecek denetler ve benzeri.

Uygulama durumunu uygulamanın alt varlıkları toplamı olur. Kısacası, Service Fabric uygulaması üzerinde bildirilen sistem üzerinden uygulama durumunu değerlendirir. Ayrıca bu şekilde tüm hizmetler için uygulama durumunu değerlendirir. Daha fazla Service Fabric sistem durumu hizmeti çoğaltma gibi kendi alt toplayarak uygulama hizmetlerini durumunu değerlendirir. Uygulama durumu ilkesi sağlanıyorsa sonra yükseltme devam edebilirsiniz. Sistem durumu ilkesi ihlal uygulama yükseltme başarısız olur.

## <a name="upgrade-modes"></a>Yükseltme modları
Uygulama yükseltme için öneririz modu yaygın olarak kullanılan modu izlenen modudur. İzlenen modu tek bir güncelleştirme etki alanında yükseltme gerçekleştirir ve tüm sistem durumu denetliyorsa (belirtilen ilke başına), geçişi geçer Sonraki güncelleştirme etki alanına otomatik olarak.  Sistem durumu denetimi başarısız ve/veya zaman aşımları ulaşıldığında, yükseltme veya güncelleştirme etki alanı için geri veya modu izlenmeyen manuel olarak değiştirilir. Başarısız yükseltme için bu iki moddan birini seçmek için yükseltme yapılandırabilirsiniz. 

İzlenmeyen el ile moduna el ile müdahale sonraki güncelleştirme etki alanı yükseltme kapalı kazandırın için bir güncelleştirme etki alanındaki her yükseltme sonrasında gerekir. Herhangi bir Service Fabric sistem durumu denetimi gerçekleştirilir. Yönetici, sonraki güncelleştirme etki alanında Yükseltmeyi başlatmadan önce sistem durumu veya durum denetimi yapar.

## <a name="upgrade-default-services"></a>Varsayılan Hizmetleri yükseltme
Varsayılan Hizmetleri Service Fabric uygulaması içindeki bir uygulama yükseltme işlemi sırasında yükseltilebilir. Varsayılan Hizmetleri tanımlanmış [uygulama bildirimi](service-fabric-application-model.md#describe-an-application). Varsayılan Hizmetleri yükseltme standart kurallar şunlardır:

1. Varsayılan hizmetlerini yeni [uygulama bildirimi](service-fabric-application-model.md#describe-an-application) kümede olmayan oluşturulur.
> [!TIP]
> [EnableDefaultServicesUpgrade](service-fabric-cluster-fabric-settings.md) aşağıdaki kuralları etkinleştirmek için true olarak ayarlanması gerekir. Bu özellik, v5.5 desteklenir.

2. Varsayılan hem de önceki varolan hizmetlerini [uygulama bildirimi](service-fabric-application-model.md#describe-an-application) ve yeni sürümü güncelleştirilir. Hizmet açıklamasında yeni sürümü kümeye de zaten üzerine yazacak. Uygulama yükseltme varsayılan hizmet hatası güncelleştirme sırasında otomatik olarak geri alma olacaktır.
3. Varsayılan hizmetlerini önceki [uygulama bildirimi](service-fabric-application-model.md#describe-an-application) ancak yeni sürümde silinir. **Bu silme varsayılan Hizmetleri değil döndürülebilir unutmayın.**

Bir uygulama durumunda yükseltme geri, yükseltmeyi başlatmadan önce Hizmetleri durumuna geri alınır varsayılan alınır. Ancak silinmiş Hizmetleri hiçbir zaman oluşturulabilir.

## <a name="application-upgrade-flowchart"></a>Uygulama yükseltme akış çizelgesi
Bu paragraf aşağıdaki akış çizelgesi, bir Service Fabric uygulama yükseltme işlemini anlamanıza yardımcı olabilir. Özellikle, akışını açıklar nasıl dahil olmak üzere zaman aşımlarını *HealthCheckStableDuration*, *HealthCheckRetryTimeout*, ve *UpgradeHealthCheckInterval*, Yardım Yükseltme tek bir güncelleştirme etki alanındaki bir başarı veya hata olarak kabul edildiğinde denetim.

![Service Fabric uygulaması için yükseltme işlemi][image]

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama kullanarak Visual Studio yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltme yol gösterir.

[Uygulama Powershell kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltme yol gösterir.

Uygulamanızı kullanarak yükseltme nasıl kontrol [yükseltme parametreleri](service-fabric-application-upgrade-parameters.md).

Uygulama yükseltme nasıl kullanılacağını öğrenerek uyumlu hale getirmek [veri seri hale getirme](service-fabric-application-upgrade-data-serialization.md).

Gelişmiş işlevselliği başvurarak uygulamanızı yükseltirken kullanmayı öğrenin [Gelişmiş konular](service-fabric-application-upgrade-advanced.md).

Adımlarına bakarak uygulama yükseltmeleri sık karşılaşılan sorunları düzeltmek [sorun giderme uygulama yükseltmelerini](service-fabric-application-upgrade-troubleshooting.md).

[image]: media/service-fabric-application-upgrade/service-fabric-application-upgrade-flowchart.png
