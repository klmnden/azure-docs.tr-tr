---
title: İş sürekliliği - Microsoft Genomics | Microsoft Docs
titleSuffix: Azure
description: Bu genel bakışta, iş sürekliliği ve olağanüstü durum kurtarma için Microsoft Genomics sağlayan özellikleri açıklar. Veri kaybına neden olabilecek bir Azure bölgesinde kesinti gibi hizmet kesintilerine yol açacak olaylardan kurtarmak için seçenekler hakkında bilgi edinin.
keywords: İş sürekliliği, olağanüstü durum kurtarma
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: article
ms.date: 04/06/2018
ms.openlocfilehash: 7a51477dbbf6f4e50959a6d979342961c7e49ad9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60641118"
---
# <a name="overview-of-business-continuity-with-microsoft-genomics"></a>Microsoft Genomics iş sürekliliğine genel bakış
Bu genel bakışta, iş sürekliliği ve olağanüstü durum kurtarma için Microsoft Genomics sağlayan özellikleri açıklar. Veri kaybına neden olabilecek bir Azure bölgesinde kesinti gibi hizmet kesintilerine yol açacak olaylardan kurtarmak için seçenekler hakkında bilgi edinin. 


## <a name="microsoft-genomics-features-that-support-business-continuity"></a>Microsoft Genomics söz konusu destek iş sürekliliği özellikleri 
Ender olsa bir Azure veri merkezinde bir kesinti birkaç dakika ile birkaç saat sürebilecek bir iş kesinti olabilir. Bir kesinti oluştuğunda, veri merkezinde şu anda çalışan tüm işleri başarısız olur ve Microsoft Genomics hizmetine otomatik olarak ikincil bir bölgeye sunmaları değil. 

* Veri Merkezi kesintisi veri merkezi tekrar çevrimiçi duruma gelmesini beklemek bittikten bir seçenektir. Kesinti kısaysa, Microsoft Genomics hizmeti başarısız işleri otomatik olarak algılar ve iş akışını otomatik olarak yeniden başlatılır.

* Başka bir iş akışında başka bir veri bölgesi proaktif bir şekilde göndermek için bir seçenektir. Bazı durumlarda Microsoft Genomics dağıtır [Azure bölgeleri](https://azure.microsoft.com/regions/services/), ve her bir bölgeye özgü örneği bağımsızdır. Microsoft Genomics örneklerinden birinde bir bölge yerel hatasıyla karşılaşırsanız, Microsoft Genomics örneklerini çalıştıran diğer işleri işlemek devam edecektir. Bu aktarım için alternatif bölgelere kullanıcının ve kullanılabilir altında herhangi bir zamanda denetimidir.


### <a name="manually-failover-microsoft-genomics-workflows-to-another-region"></a>El ile yük devretme Microsoft Genomics iş akışları başka bir bölgede
Bölgesel veri merkezi kesintisi durumunda ikincil bir bölgede Microsoft Genomics iş akışlarını tek tek veri egemenliği ve iş sürekliliği gereksinimlerinize göre gönderme katılmak bırakmayı. El ile yük devretme Microsoft Genomics iş akışları için farklı bir bölgeye özel kullanmanız gerekir. Genomiks hesabı ve ilgili bölgeye özgü Genomiks ve depolama hesabı kimlik bilgileri ile iş gönderme.

Özellikle, yapmanız gerekir:
* Genomiks hesabı, Azure portalını kullanarak ikincil bölgede oluşturun. 
* Girişinizi ikincil bölgedeki bir depolama hesabına geçirin ve ikincil bölgedeki bir çıkış klasörü ayarlayın.
* İkincil bölgedeki bir iş akışı gönderin.

Özgün bölge geri yüklendiğinde, Microsoft Genomics hizmeti verileri ikincil bölgeden özgün bölgeye geri geçirmez. Girişi Taşı katılmak bırakmayı ve özgün bölgeye ikincil bölgeye çıktı dosyaları yedekleyin.  Veri taşıma verilerini taşımak seçilmesi, Genomiks hizmeti dışında budur ve ilgili tüm ücretleri sizin sorumluluğunuzdadır olacaktır. 

### <a name="preparing-for-a-possible-region-specific-outage"></a>Olası bir bölgeye özgü kesinti için hazırlanma
Daha hızlı kurtarma veri merkezi kesintisi durumunda ilgili endişeleriniz varsa, Microsoft Genomics akışlarınızı ikincil bir bölgeye el ile yeniden göndermek için gereken süreyi azaltmak için yapabileceğiniz birkaç adım vardır:

* Uygun bir ikincil bölgeye tanımlamak ve profesyonel etkin olarak bu bölgede Genomiks hesabı oluşturma
* Böylece, verilerinizi ikincil bölge'de hemen kullanılabilir birincil ve ikincil bölgede çoğaltın. Bu el ile yapılan veya kullanarak olabilir [coğrafi olarak yedekli depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy) özelliğini Azure depolamada kullanılabilir. 

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, iş sürekliliği ve olağanüstü durum kurtarma seçenekleriniz hakkında Microsoft Genomics hizmeti kullanırken öğrendiniz. İş sürekliliği ve olağanüstü durum kurtarma azure'da genel hakkında daha fazla bilgi için bkz: [Azure dayanıklılık teknik Kılavuzu.](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region) 