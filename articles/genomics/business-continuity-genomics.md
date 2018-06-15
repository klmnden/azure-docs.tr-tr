---
title: İş sürekliliği - Microsoft Genomics | Microsoft Docs
titleSuffix: Azure
description: Microsoft Genomics iş sürekliliği nasıl destekler? öğrenin
keywords: İş sürekliliği, olağanüstü durum kurtarma
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: article
ms.date: 04/06/2018
ms.openlocfilehash: cb3825cb89aff386c4f7c3f3b0d771d73fe637b1
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31427166"
---
# <a name="overview-of-business-continuity-with-microsoft-genomics"></a>İş sürekliliği Microsoft Genomics ile genel bakış
Bu genel bakışta, iş devamlılığı ve olağanüstü durum kurtarma için Microsoft Genomics sağlayan özellikleri açıklanmaktadır. Veri kaybına neden olabilecek bir Azure bölgesi kesinti gibi kesintiye uğratan olaylardan kurtarma seçenekleri hakkında bilgi edinin. 


## <a name="microsoft-genomics-features-that-support-business-continuity"></a>Bu destek iş sürekliliği Microsoft Genomics özellikleri 
Ender olsa bir Azure veri merkezi birkaç dakika ile birkaç saat son bir iş kesintiye neden olabilecek bir hizmet kesintisi olabilir. Bir kesinti oluşursa, şu anda veri merkezinde çalışan tüm işleri başarısız olur ve Microsoft Genomics hizmeti otomatik olarak ikincil bir bölgeye işleri yeniden gönderin. 

* Veri Merkezi kesintisinden tekrar çevrimiçi görünmesi için veri merkezi üzerinden beklenir bir seçenektir. Kesinti kısaysa, Microsoft Genomics hizmet başarısız işleri otomatik olarak algılar ve iş akışı otomatik olarak yeniden başlatılacak.

* Proaktif olarak başka bir veri bölgesi iş akışında gönderme başka bir seçenektir. Microsoft Genomics dağıtan birkaç durumlarda [Azure bölgeleri](https://azure.microsoft.com/regions/services/), ve her bölgeye özgü örneği bağımsızdır. Microsoft Genomics örneklerden birini bir bölge yerel hatasıyla karşılaşırsanız, Microsoft Genomics örneklerini çalıştıran diğer bölgelerdeki işlerini işlemeye devam eder. Bu aktarım alternatif bir bölge için kullanıcı ve kullanılabilir altında herhangi bir zamanda denetimdir.


### <a name="manually-failover-microsoft-genomics-workflows-to-another-region"></a>El ile yük devretme Microsoft Genomics iş akışlarına başka bir bölge
Bölgesel veri merkezi kesintisinden söz konusu olduğunda, Microsoft Genomics akışlarında ayrı veri egemenliği ve iş sürekliliği gereksinimlerinize göre bir ikincil bölge göndermek tercih edebilirsiniz. El ile yük devretme Microsoft Genomics iş akışları için farklı bir bölgeye özel kullanırsınız. Genomics hesap ve uygun bölgeye özgü Genomics ve depolama hesabının kimlik bilgilerini işlemiyle gönderin.

Özellikle, yapmanız gerekir:
* Azure portalını kullanarak ikincil bölge'de, bir Genomics hesabı oluşturun. 
* İkincil bölge içindeki bir depolama hesabına giriş verilerinizi taşımanız ve ikincil bölge içinde bir çıkış klasörü ayarlayın.
* İkincil bölge iş akışında gönderin.

Özgün bölge geri yüklendiğinde Microsoft Genomics hizmeti veri ikincil bölge ' özgün bölgeye geçirmez. İkincil bölge çıktı dosyaları özgün bölgesine arka ve giriş taşımak tercih edebilirsiniz.  Veri taşıma verilerini taşıma bırakmayı, bu dışında Genomics hizmetidir ve tüm ücretleri ilgili sizin sorumluluğunuzdadır şöyle olur. 

### <a name="preparing-for-a-possible-region-specific-outage"></a>Olası bir bölgeye özgü kesinti için hazırlama
Veri Merkezi kesintisinden söz konusu olduğunda daha hızlı kurtarma hakkında endişeleriniz varsa, Microsoft Genomics akışlarınızı ikincil bir bölgeye el ile yeniden gönderin gereken süreyi azaltmak için yapabileceğiniz birkaç adım vardır:

* Uygun bir ikincil bölge belirleyin ve bu bölgede etkin profesyonel bir Genomics hesabı oluşturun
* Böylece, verilerinizi ikincil bölge'de hemen kullanılabilir birincil ve ikincil bölge verilerinizi yinelenen. Bu el ile yapılan veya kullanarak olabilir [coğrafi olarak yedekli depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy) Özelliği Azure depolama alanında kullanılabilir. 

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, iş sürekliliği ve olağanüstü durum kurtarma seçenekleri hakkında Microsoft Genomics hizmetini kullanırken öğrendiniz. İş devamlılığı ve olağanüstü durum kurtarma Azure içinde genel hakkında daha fazla bilgi için bkz: [Azure dayanıklılık teknik Kılavuzu.](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region) 