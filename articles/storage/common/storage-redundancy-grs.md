---
title: Bölgeler arası dayanıklılık Azure depolama için coğrafi olarak yedekli depolama (GRS) | Microsoft Docs
description: Coğrafi olarak yedekli depolama (GRS) verilerinizi yüzlerce mil uzaklıkta bulunan iki bölge arasında çoğaltır. GRS, bölgesel bir olağanüstü yanı sıra veri merkezi donanım arızalarına karşı korur.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/20/2018
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 09b553f3ca64d8f5217f023c776ec848215366f9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65151002"
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
* Bir hesap yük devretme (Önizleme) ikincil bölgeye bir GRS veya RA-GRS hesabı'ı başlattığınızda, yük devretme tamamlandıktan sonra bu hesaba yazma erişimi geri yüklenir. Daha fazla bilgi için [olağanüstü durum kurtarma ve depolama hesabı yük devretme (Önizleme)](storage-disaster-recovery-guidance.md).
* RA-GRS, yüksek kullanılabilirlik sağlamak için tasarlanmıştır. Ölçeklenebilirlik kılavuzu için gözden [performans denetim](storage-performance-checklist.md).
* RA-GRS ile yüksek kullanılabilirlik için tasarlama konusunda daha fazla bilgi için bkz: [tasarlama yüksek oranda kullanılabilir RA-GRS depolama kullanan uygulamalar](storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>GRS ile RTO ve RPO nedir?

**Kurtarma noktası hedefi (RPO):** GRS ve RA-GRS depolama hizmeti zaman uyumsuz olarak coğrafi-çoğaltır birincil ikincil konumdaki verileri. Birincil bölge kullanılamaz duruma gelirse, olay, bir hesap yük devretme (Önizleme)'ikincil bölgeye gerçekleştirebilirsiniz. Bir yük devretme başlatın, coğrafi olarak çoğaltılmış henüz yapmadıysanız son değişiklikler kaybolabilir. Kayıp olası veri dakika sayısını RPO bilinir. RPO, verilerin kurtarılabilir zaman noktası gösterir. Azure depolama, genellikle 15 dakikadan kısa bir RPO sahip, rağmen şu anda SLA üzerinde ne kadar süreyle coğrafi çoğaltma olur.

**Kurtarma süresi hedefi (RTO):** RTO, bir yük devretme gerçekleştirin ve depolama hesabı çevrimiçine almak için ne kadar sürer, bir ölçüdür. Yük devretme gerçekleştirmek için zaman, aşağıdaki eylemleri içerir:

   * Müşterinin birincil depolama hesabından ikincil bölgeye yük devretme başlatana kadar geçen süre.
   * Azure tarafından ikincil konuma işaret edecek şekilde birincil DNS girişlerini değiştirerek yük devretme gerçekleştirmek için gereken süre.

## <a name="paired-regions"></a>Eşleştirilmiş bölgeler 
Bir depolama hesabı oluşturduğunuzda, hesap için birincil bölge seçin. Eşleştirilmiş ikincil bölgede, birincil bölgeye göre belirlenir ve değiştirilemez. Azure tarafından desteklenen bölgeler hakkında güncel bilgiler için bkz: [iş sürekliliği ve olağanüstü durum kurtarma (BCDR): Azure eşleştirilmiş bölgeleri](../../best-practices-availability-paired-regions.md).

## <a name="see-also"></a>Ayrıca bkz.
- [Azure Depolama çoğaltması](storage-redundancy.md)
- [Yerel olarak yedekli depolama (LRS): Azure depolama için düşük maliyetli veri yedekliği](storage-redundancy-lrs.md)
- [Bölgesel olarak yedekli depolama (ZRS): Azure depolama yüksek kullanılabilirliğe sahip uygulamalar](storage-redundancy-zrs.md)
