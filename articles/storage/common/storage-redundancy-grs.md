---
title: Bölgeler arası dayanıklılık Azure depolama için coğrafi olarak yedekli depolama (GRS) | Microsoft Docs
description: Coğrafi olarak yedekli depolama (GRS) verilerinizi yüzlerce mil uzaklıkta bulunan iki bölge arasında çoğaltır. GRS, bölgesel bir olağanüstü yanı sıra veri merkezi donanım arızalarına karşı korur.
services: storage
author: tolandmike
ms.service: storage
ms.topic: article
ms.date: 10/20/2018
ms.author: jeking
ms.component: common
ms.openlocfilehash: 85d69db2f94e4bddf1258233c34c64dcf78a3eeb
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219231"
---
# <a name="geo-redundant-storage-grs-cross-regional-replication-for-azure-storage"></a>Coğrafi olarak yedekli depolama (GRS): Azure depolama için bölgeler arası çoğaltma
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-grs.md)]

## <a name="read-access-geo-redundant-storage"></a>Okuma erişimli coğrafi olarak yedekli depolama
Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) depolama hesabınız için kullanılabilirliği en üst düzeye çıkarır. RA-GRS iki bölgede coğrafi çoğaltma ek olarak, ikincil konumdaki verilere salt okunur erişim sağlar.

Yalnızca okuma erişimi verilerinizi ikincil bölgeye etkinleştirdiğinizde, depolama hesabınızın birincil uç noktaya yanı sıra ikincil uç noktaya verilerinizin kullanılabilir. İkincil uç birincil uç noktaya benzer, ancak son ekine ekler `–secondary` hesap adı. Örneğin, Blob hizmetinin birincil uç noktanızı ise `myaccount.blob.core.windows.net`, ikincil uç noktanız ise `myaccount-secondary.blob.core.windows.net`. Depolama hesabınızın erişim anahtarlarını birincil ve ikincil uç için aynıdır.

RA-GRS kullanırken akılda tutulması gereken bazı noktalar:

* Uygulamanızın, RA-GRS kullanırken etkileşim hangi uç noktaya yönetme gerekir.
* Zaman uyumsuz çoğaltma bir gecikme gerektirdiğinden, birincil bölgeden verilerin kurtarılamaması durumunda ikincil bölgeye henüz çoğaltılan henüz değişiklikler kaybolabilir.
* Son eşitleme zamanı depolama hesabınızın kontrol edebilirsiniz. Son eşitleme zamanı tarih/saat GMT bir değerdir. Tüm birincil yazma işlemlerini son eşitleme zamanı önce ikincil konumda ikincil konumdan okumaya kullanılabilir olduğu anlamına gelir, başarılı bir şekilde yazılmış. Son eşitleme süresi okuma için henüz kullanılamıyor olabilir veya sonra birincil yazar. Bu değeri kullanarak sorgulayabilirsiniz [Azure portalında](https://portal.azure.com/), [Azure PowerShell](storage-powershell-guide-full.md), veya bir Azure depolama istemci kitaplıkları.
* Microsoft ikincil bölgeye yük devretme durumunda başlatır, okuduğunuz ve bu verileri yük devretmeden sonra yazma erişimi tamamlandı. Daha fazla bilgi için [olağanüstü durum kurtarma Kılavuzu](storage-disaster-recovery-guidance.md).
* İkincil bölgeye geçiş yapma hakkında daha fazla bilgi için bkz: [bir Azure depolama kesinti oluşursa yapmanız gerekenler](storage-disaster-recovery-guidance.md).
* RA-GRS, yüksek kullanılabilirlik sağlamak için tasarlanmıştır. Ölçeklenebilirlik kılavuzu için gözden [performans denetim](storage-performance-checklist.md).
* RA-GRS ile yüksek kullanılabilirlik için tasarlama konusunda daha fazla bilgi için bkz: [tasarlama yüksek oranda kullanılabilir RA-GRS depolama kullanan uygulamalar](storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>GRS ile RTO ve RPO nedir?
**Kurtarma noktası hedefi (RPO):** GRS ve RA-GRS depolama hizmeti zaman uyumsuz olarak coğrafi-çoğaltır birincil ikincil konumdaki verileri. Birincil bölgede bir ana bölgesel olağanüstü durumda Microsoft ikincil bölgeye yük devretme gerçekleştirir. Bir yük devretme durumda coğrafi olarak çoğaltılmış henüz yapmadıysanız son değişiklikler kaybolabilir. Kayıp olası veri dakika sayısını RPO bilinir. RPO, verilerin kurtarılabilir zaman noktası gösterir. Azure depolama, genellikle 15 dakikadan kısa bir RPO sahip, rağmen şu anda SLA üzerinde ne kadar süreyle coğrafi çoğaltma olur.

**Kurtarma süresi hedefi (RTO):** RTO yük devretme gerçekleştirmek ve depolama hesabı çevrimiçine almak için ne kadar sürer ölçüdür. Yük devretme gerçekleştirmek için zaman, aşağıdaki eylemleri içerir:

   * Veriler birincil konuma geri yüklenebilir veya bir yük devretme gerekli olup olmadığını belirlemek için Microsoft süresi gerektirir
   * Depolama hesabı için yük devretme ikincil konuma işaret edecek şekilde birincil DNS girişlerini değiştirerek gerçekleştirilecek saat

Microsoft, verilerinizin gerçekten de koruma sorumluluğunu üstlenir. Birincil bölgedeki veriler kurtarma herhangi olasılığı varsa, Microsoft yük devretme geciktirir ve veri kurtarma üzerinde odaklanır. 

## <a name="paired-regions"></a>Eşleştirilmiş bölgeler 
Bir depolama hesabı oluşturduğunuzda, hesap için birincil bölge seçin. Eşleştirilmiş ikincil bölgede, birincil bölgeye göre belirlenir ve değiştirilemez. Azure tarafından desteklenen bölgeler hakkında güncel bilgiler için bkz: [iş sürekliliği ve olağanüstü durum kurtarma (BCDR): Azure eşleştirilmiş bölgeleri](../../best-practices-availability-paired-regions.md).

## <a name="see-also"></a>Ayrıca bkz.
- [Azure Depolama çoğaltması](storage-redundancy.md)
- [Yerel olarak yedekli depolama (LRS): Azure depolama için düşük maliyetli veri yedekliği](storage-redundancy-lrs.md)
- [Bölgesel olarak yedekli depolama (ZRS): Azure depolama yüksek kullanılabilirliğe sahip uygulamalar](storage-redundancy-zrs.md)