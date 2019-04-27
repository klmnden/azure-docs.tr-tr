---
title: Olağanüstü durum kurtarma ve coğrafi dağıtım, dayanıklı işlevler - Azure
description: Olağanüstü durum kurtarma ve coğrafi dağıtım, dayanıklı işlevler hakkında bilgi edinin.
services: functions
author: MS-Santi
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: 1363dd3c620789b9f3c8ce1dbe0892ee61d66051
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60741377"
---
# <a name="disaster-recovery-and-geo-distribution"></a>Olağanüstü durum kurtarma ve coğrafi dağıtım

## <a name="overview"></a>Genel Bakış

Dayanıklı işlevler içinde tüm durumu, Azure Depolama'da kalıcı hale getirilir. A [görev hub](durable-functions-task-hubs.md) düzenlemeleri için kullanılan Azure Storage kaynakları için mantıksal bir kapsayıcıdır. Aynı görev hub'ına ait oldukları zaman orchestrator ve etkinlik işlevleri yalnızca birbiriyle etkileşim kurabilir.
Açıklanan senaryoları kullanılabilirliğini artırmak ve olağanüstü durum kurtarma etkinlikleri sırasında kapalı kalma süresini en aza indirmek için dağıtım seçenekleri önerin.

Azure Depolama'nın kullanımı kılavuzluk edilir olduğundan bu senaryolar Aktif-Pasif yapılandırmada, temel dikkat edin önemlidir. Bu düzen, bir yedekleme (pasif) işlev uygulaması farklı bir bölgeye dağıtmaya dayalıdır. Traffic Manager birincil (etkin) işlev uygulaması için kullanılabilirlik izleyeceksiniz. Birincil başarısız olursa üzerinde yedekleme işlev uygulaması ile başarısız olur. Daha fazla bilgi için [Traffic Manager](https://azure.microsoft.com/services/traffic-manager/)'s [öncelik trafik yönlendirme yöntemi.](../../traffic-manager/traffic-manager-routing-methods.md#priority-traffic-routing-method)

>[!NOTE]
>
> - Önerilen Aktif-Pasif yapılandırmayı bir istemci her zaman HTTP üzerinden yeni düzenlemeleri tetiklemeniz mümkün olmasını sağlar. Ancak, aynı depolama alanı paylaşımı iki işlev uygulamaları sahip söz konusu kümelerdeki arka plan işlemesi her ikisi de aynı kuyruk iletileri için rekabet arasında dağıtılır. Bu yapılandırma, ikincil bir işlev uygulaması için ek kullanım maliyetleri doğurur.
> - Temel alınan depolama hesabı ve görev hub birincil bölgede oluşturulur ve her iki işlev uygulamaları tarafından paylaşılır.
> - Nedenle dağıtılan tüm işlev uygulamalarını HTTP üzerinden etkinleştirilmekte olan söz konusu olduğunda aynı işlevi erişim anahtarlarını paylaşın gerekir. İşlevler çalışma zamanı gösteren bir [yönetim API'si](https://github.com/Azure/azure-functions-host/wiki/Key-management-API) tüketiciler programlı olarak ekleme, silme ve işlev tuşlarını güncelleştirmek etkinleştirir.

## <a name="scenario-1---load-balanced-compute-with-shared-storage"></a>Senaryo 1 - paylaşılan depolama ile işlem yük dengeli

Azure bilgi işlem altyapısı başarısız olursa, işlev uygulamasını kullanılamaz hale gelebilir. Tür kapalı kalma süresi olasılığını en aza indirmek için bu senaryo için farklı bölgelere dağıtılan iki işlev uygulamaları kullanır.
Traffic Manager, birincil işlev uygulamasında sorunları algılar ve otomatik olarak ikincil bölgedeki işlev uygulaması için trafiği yönlendirmek için yapılandırılır. Bu işlev uygulaması, aynı Azure depolama hesabını ve görev Hub paylaşır. Bu nedenle, işlev uygulamalarını durumunu kaybolmaz ve normal olarak çalışmaya devam. Azure Traffic Manager sistem durumu birincil bölgeye geri yüklendikten sonra bu işlev uygulaması yönlendirme isteklerini otomatik olarak başlatılacak.

![Diyagram gösteren Senaryo 1.](./media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario01.png)

Bu dağıtım senaryosu kullanırken çeşitli avantajları vardır:

- Bilgi işlem altyapısı başarısız olursa, iş durumu kaybı olmadan bölge üzerinden başarısız devam edebilir.
- Traffic Manager otomatik yük devretme sağlıklı bir işlev uygulaması için otomatik olarak yapar.
- Kesinti çözümlendikten sonra traffic Manager otomatik olarak birincil işlev uygulaması için trafiği yeniden oluşturur.

Ancak, bu senaryoyu kullanarak göz önünde bulundurun:

- İşlev uygulamasını özel bir App Service planı kullanarak dağıtılmışsa, veri merkezi bilgi işlem altyapısı, başarısız çoğaltma maliyetleri artırır.
- Bu senaryo, bilgi işlem altyapısı sırasında kesintileri kapsar, ancak depolama hesabı ' % s'işlev uygulaması için hata tek noktasını olmaya devam eder. Bir depolama kesintisi ise, uygulama bir kapalı kalma süresi düşer.
- İşlev uygulaması, yükü devredilirse bölgeler arasında kendi depolama hesabına erişir olduğundan daha yüksek gecikme süresiyle olacaktır.
- Farklı bir bölgede olduğu depolama hizmetine erişim bulunduğu ağa çıkış trafiği nedeniyle daha yüksek maliyet doğurur.
- Bu senaryo, Traffic Manager'ı bağlıdır. Dikkate [Traffic Manager nasıl çalışır](../../traffic-manager/traffic-manager-how-it-works.md), süre kadar dayanıklı işlevi kullanan bir istemci uygulaması, işlev uygulaması adresi trafik Yöneticisi'nden yeniden sorgula gerekiyor olabilir.

## <a name="scenario-2---load-balanced-compute-with-regional-storage"></a>Senaryo 2 - bölgesel depolama ile işlem yük dengeli

Yukarıdaki senaryo, yalnızca büyük küçük bilgi işlem altyapısı hata kapsar. Depolama hizmeti başarısız olursa, işlev uygulamasının bir kesintisi neden olur.
Dayanıklı işlevler sürekli çalışmasını sağlamak için bu senaryo, her bir bölgeye işlev uygulamalarını dağıtılan sanal yerel depolama hesabı kullanır.

![Diyagram gösteren Senaryo 2.](./media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario02.png)

Bu yaklaşım önceki senaryoya göre geliştirmeleri içerir:

- İşlev uygulaması başarısız olursa, Traffic Manager, ikincil bölgeye yük devretme üstlenir. Bununla birlikte, işlev uygulaması, kendi depolama hesabında kullandığından, dayanıklı işlevler çalışmaya devam.
- Bir yük devretme sırasında yoktur hiçbir ek gecikme başarısız bölge, işlev uygulaması ve depolama hesabı aynı konumda olduğundan.
- Depolama katmanı hatası hataları, başarısız bir yeniden yönlendirme bölge sırayla tetikleyecek dayanıklı işlevler neden olur. İşlev uygulaması ve depolama bölge başına yalıtılmış olduğundan, yeniden dayanıklı işlevler çalışmaya devam eder.

Bu senaryo için önemli noktalar:

- İşlev uygulamasını özel bir uygulama hizmeti planı kullanarak dağıtılmışsa, veri merkezi bilgi işlem altyapısı, başarısız çoğaltma maliyetleri artırır.
- Geçerli durumu, yürütmek anlamına gelir yük devretti değildir ve belirttiğinizde işlevleri başarısız olur. Bu yeniden deneme/yeniden çalışma için istemci uygulaması olan.

## <a name="scenario-3---load-balanced-compute-with-grs-shared-storage"></a>Senaryo 3 - GRS paylaşılan depolama ile işlem yük dengeli

Bu senaryo bir paylaşılan depolama hesabı uygulama ilk senaryoda değişikliktir. Coğrafi çoğaltmanın etkinleştirilmiş olduğu depolama hesabı oluşturduğunuz ana fark.
İşlevsel olarak, bu senaryo ıaas'nin Senaryo 1 sağlar, ancak ek veriler kurtarma avantajları sağlar:

- Coğrafi olarak yedekli depolama (GRS) ve okuma erişimli GRS (RA-GRS), depolama hesabınız için kullanılabilirliği en üst düzeye.
- Bir bölgede kesinti depolama hizmetinin ise, olasılık depolama ikincil bölgeye yük devretti gerekir, veri merkezi işlemlerini belirleyin biridir. Bu durumda, depolama hesabı erişim coğrafi olarak çoğaltılmış kopyalama depolama hesabının, kullanıcı müdahalesi olmadan şeffaf bir şekilde yönlendirilirsiniz.
- Bu durumda, birkaç dakikada bir gerçekleşir depolama hesabının son çoğaltma kadar dayanıklı işlevler durumu korunur.

Diğer bir senaryolarında olduğu gibi önemli noktalar vardır:

- Yük devretme çoğaltma, veri merkezi işleci tarafından gerçekleştirilir ve biraz zaman alabilir. O zamana kadar işlev uygulaması, bir kesinti düşer.
- Coğrafi çoğaltmalı depolama hesapları kullanmaya yönelik daha yüksek bir maliyet yoktur.
- GRS zaman uyumsuz olarak gerçekleşir. En son işlem bazıları çoğaltma işleminin gecikme nedeniyle kaybolmuş olabilir.

![Diyagram gösteren Senaryo 3.](./media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario03.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinebilirsiniz [tasarlama yüksek oranda kullanılabilir RA-GRS'yi kullanarak uygulamaları](../../storage/common/storage-designing-ha-apps-with-ragrs.md)
