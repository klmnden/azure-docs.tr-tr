---
title: Olağanüstü durum kurtarma ve coğrafi dağıtım dayanıklı işlevlerinde - Azure
description: Olağanüstü durum kurtarma ve coğrafi dağıtım dayanıklı işlevlerinde hakkında bilgi edinin.
services: functions
author: MS-Santi
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: 8eb42a60045304416ec6aa1099a84b1e264c692d
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
ms.locfileid: "33207107"
---
# <a name="disaster-recovery-and-geo-distribution"></a>Olağanüstü durum kurtarma ve coğrafi dağılımı

## <a name="overview"></a>Genel Bakış
Azure dayanıklı işlevlerde tüm durum Azure Depolama'da kalıcı. A [görev hub](durable-functions-task-hubs.md) düzenlemelerin için kullanılan Azure Storage kaynakları için mantıksal bir kapsayıcısıdır. Aynı görevi hub'ına ait olduğunda orchestrator ve etkinlik işlevleri yalnızca birbirleriyle etkileşim kuramazlar.
Açıklanan senaryoları kullanılabilirliğini artırmak ve olağanüstü durum kurtarma eylemleri sırasında kapalı kalma süresini en aza indirmek için dağıtım seçenekleri önerin.
Azure depolama kullanımını kılavuzluk edilir beri bu senaryoları Aktif-Pasif yapılandırmaları, temel fark önemlidir. Bu desen bir yedekleme (pasif) işlev uygulaması farklı bir bölgeye dağıtmayı. Trafik Yöneticisi kullanılabilirlik için birincil (etkin) işlev uygulaması izlenir. Birincil başarısız olursa üzerinden yedekleme işlevi uygulamaya başarısız olur. Daha fazla bilgi için bkz: [trafik Yöneticisi](https://azure.microsoft.com/services/traffic-manager/)'s [öncelik trafik yönlendirme yöntemi.](../traffic-manager/traffic-manager-routing-methods.md#a-name--priorityapriority-traffic-routing-method)


>[!NOTE]
>- Önerilen Aktif-Pasif yapılandırması bir istemci her zaman yeni düzenlemelerin HTTP üzerinden tetiklemek mümkün olmasını sağlar. Ancak, aynı depolama paylaşımı iki işlev uygulamalarının sahip gruplarındaki sonucu olarak, arka plan işlemesi her ikisi de aynı sıralarda iletileri için rekabet arasında dağıtılır. Bu yapılandırma bu ikincil işlev uygulaması için eklenen çıkışı maliyeti doğurur.
>- Temel alınan depolama hesabı ve görev hub birincil bölgede oluşturulur ve her iki işlevi uygulamalar tarafından paylaşılır.
>- Nedenle dağıtılan tüm işlevi uygulamalar HTTP üzerinden etkinleştirilmekte olan söz konusu olduğunda aynı işlevi erişim tuşları paylaşmak gerekir. İşlevler çalışma zamanı kullanıma sunan bir [yönetim API'si](https://github.com/Azure/azure-functions-host/wiki/Key-management-API) programlı olarak ekleme, silme ve işlev tuşlarını güncelleştirme tüketicilerin sağlar.

## <a name="scenario-1---load-balanced-compute-with-shared-storage"></a>Senaryo 1 - paylaşılan depolama ile işlem yük dengeli
Azure işlem altyapısındaki başarısız olursa, işlev uygulaması kullanılamaz hale gelebilir. Bu tür kapalı kalma süresi olasılığını en aza indirmek için farklı bölgelere dağıtılan iki işlevi uygulamalar bu senaryo kullanır. Trafik Yöneticisi birincil işlevi uygulamada sorunları algılamak ve otomatik olarak ikincil bölge işlevi uygulamada trafiği yönlendirmek üzere yapılandırılmış. Bu işlev uygulaması aynı Azure Storage hesabını ve görev Hub paylaşır. Bu nedenle, işlev uygulamalarının durumunu kaybı olmadığından ve normal şekilde çalışmaya devam. Azure Traffic Manager sistem durumu birincil bölgesine geri yüklendikten sonra bu işlev uygulaması yönlendirme istekleri otomatik olarak başlatılacak.


![Diyagram gösteren Senaryo 1.](media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario01.png)

Bu dağıtım senaryosu kullanırken çeşitli avantajları vardır:
- Bilgi işlem altyapısı başarısız olursa, iş durumu kaybı olmadan bölge üzerinden başarısız devam edebilirsiniz.
- Trafik Yöneticisi sağlıklı işlev uygulaması için otomatik yük devretme otomatik olarak ilgilenir.
- Kesinti çözümlendikten sonra trafik Yöneticisi otomatik olarak birincil işlev uygulaması trafiği yeniden kurar.

Ancak, bu senaryoyu kullanarak göz önünde bulundurun:
- Adanmış bir uygulama hizmeti planı kullanarak işlev uygulaması dağıtılmışsa, veri merkezi başarısız işlem altyapısında çoğaltma maliyetleri artırır.
- Bu senaryo, bilgi işlem altyapısı sırasında kesintileri kapsar, ancak tek uygulama işlevi için hata noktası olması depolama hesabı sürdürür. Bir depolama kesinti ise, uygulama bir kapalı kalma süresi düşer.
- İşlev uygulaması üzerinden başarısız olursa, depolama hesabı bölgeler arasında erişecek olduğundan daha yüksek gecikme süresi olacaktır.
- Depolama hizmetine olduğu farklı bir bölgeye erişim bulunduğu ağ çıkış trafiği nedeniyle daha yüksek maliyet doğurur.
- Bu senaryo, trafik Yöneticisi bağlıdır. Dikkate [trafik Yöneticisi nasıl çalışır](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), işlev uygulaması adresi trafik Yöneticisi'nden yeniden sorgulamak sağlam bir işlev tüketen bir istemci uygulaması gerekiyor kadar bazı süresi olabilir. 


## <a name="scenario-2---load-balanced-compute-with-regional-storage"></a>Senaryo 2 - bölgesel depolama ile işlem yük dengeli
Önceki senaryo, yalnızca bilgi işlem altyapısı hatası durumunda kapsar. Depolama hizmetinin başarısız olursa, bir işlev uygulaması kesinti içinde neden olur.
Dayanıklı işlevlerin sürekli çalışmasını sağlamak için bu senaryo işlev uygulamalarının dağıtılan her bölge için yerel depolama hesabı kullanır.

![Diyagram gösteren Senaryo 2.](media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario02.png)

Bu yaklaşım, önceki senaryoya geliştirmeleri içerir:
- İşlev uygulaması başarısız olursa, trafik Yöneticisi yapabilmesini ikincil bölge ilgilenir. Ancak, işlev uygulaması kendi depolama hesabında kullandığından, dayanıklı işlevleri çalışmaya devam.
- Sırasında bir yük devretme yoktur hiçbir ek gecikme başarısız bölge işlev uygulaması ve depolama hesabı birlikte bulunan olduğundan.
- Depolama katmanı başarısızlığını hataları, başarısız bir yeniden yönlendirme bölge içinde sırayla tetikleyecek dayanıklı işlevlerinde neden olur. Yeniden, işlev uygulaması ve depolama bölge başına yalıtılmış olduğundan, dayanıklı işlevleri çalışmaya devam eder.
 
Bu senaryo için önemli noktalar:
- Ayrılmış bir uygulama hizmeti planı kullanarak işlev uygulaması dağıtılmışsa, başarısız işlem altyapısında veri merkezi çoğaltma maliyetleri artırır.
- Geçerli durumu, yürütme anlamına gelir yük değil ve belirttiğinizde işlevleri başarısız olur. Bu iş yeniden deneme/yeniden başlatma için istemci uygulaması olan.

## <a name="scenario-3---load-balanced-compute-with-grs-shared-storage"></a>Senaryo 3 - GRS paylaşılan depolama ile işlem yük dengeli
Bu senaryo bir paylaşılan depolama hesabı uygulama ilk senaryoda değişikliktir. Temel fark coğrafi çoğaltmanın etkinleştirilmiş ile depolama hesabı oluşturulur.
İşlevsel olarak, bu senaryo Senaryo 1 olarak aynı avantajları sağlar, ancak ek veri kurtarma avantajları sağlar:
- Coğrafi olarak yedekli depolama (GRS) ve okuma erişimi GRS (RA-GRS), depolama hesabınız için kullanılabilirliği en üst düzeye.
- Bir bölge kesinti depolama hizmetinin ise, olasılık veri merkezi işlemlerinin ikincil bölge'ye yük gerekir olduğunu belirlemek biridir. Bu durumda, depolama hesabı erişim coğrafi olarak çoğaltılmış bir kopyasına depolama hesabı, kullanıcı müdahalesi olmadan şeffaf bir şekilde yönlendirilir.
- Bu durumda, dayanıklı işlevleri durumunu birkaç dakikada bir gerçekleşir depolama hesabı son çoğaltma kadar korunur.
Diğer bir senaryolarında olduğu gibi önemli noktalar vardır:
- Yük devretme çoğaltması datacenter işleçleri tarafından yapılır ve biraz zaman alabilir. O zamana kadar işlev uygulaması kesinti düşer.
- Coğrafi olarak çoğaltılmış depolama hesapları kullanmaya yönelik daha yüksek bir maliyeti yoktur.
- GRS zaman uyumsuz olarak gerçekleşir. En son işlemleri bazıları çoğaltma işlemi gecikme nedeniyle kaybolmuş olabilir.

![Diyagram gösteren Senaryo 3.](media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario03.png)


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinebilirsiniz [tasarlama yüksek oranda kullanılabilir RA-GRS kullanan uygulamalar](../storage/common/storage-designing-ha-apps-with-ragrs.md)
