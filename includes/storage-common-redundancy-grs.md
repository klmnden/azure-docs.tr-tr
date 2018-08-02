---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/26/2018
ms.author: jeking
ms.custom: include file
ms.openlocfilehash: f5b6e0e74bbab33b9ae6fbacca5c55ea434d3e41
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399966"
---
Coğrafi olarak yedekli depolama (GRS), en az % 99,99999999999999'si sağlamak için tasarlanmıştır (16 9) mil birincil bölgeden yüzlerce ikincil bir bölgeye veri çoğaltma ile belirli bir yıl boyunca nesnelerin dayanıklılık. Etkin GRS depolama hesabınızın sahip, verilerinizi tam bölgesel bir kesinti veya birincil bölgeye kurtarılabilir değil bir olağanüstü durum olması durumunda bile kalıcı olur.

GRS için kullanmayı seçerseniz, aralarından seçim yapabileceğiniz ilgili iki seçeneğiniz vardır:

* Veriler yalnızca Microsoft ikincil bölgeye birincil bir yük devretme başlatır, okumak kullanılabilir ancak bu GRS, verilerinizi ikincil bir bölgede başka bir veri merkezine çoğaltır. 
* GRS okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) temel alır. RA-GRS, verilerinizi ikincil bir bölgede başka bir veri merkezine çoğaltılır ve ayrıca ikincil bölgeden okumak için seçeneği sağlar. RA-GRS ile mi Microsoft ikincil birincil bir yük devretme başlatır bağımsız olarak ikincil konumdan edinebilirsiniz. 

GRS veya RA-GRS etkin olan bir depolama hesabı için tüm verileri ilk kez çoğaltılır yerel olarak yedekli depolama (LRS). Bir güncelleştirme, öncelikle birincil konuma taahhüt eder ve LRS kullanılarak çoğaltılır. Güncelleştirmeyi zaman uyumsuz olarak GRS kullanarak ikincil bir bölgeye çoğaltılır. İkincil konuma veri yazıldığında de LRS kullanarak içinde bu konuma çoğaltılır. 

Birincil ve ikincil bölgeler, çoğaltmalar ayrı hata etki alanları arasında yönetme ve yükseltme etki alanları içindeki bir depolama ölçek birimi. Depolama ölçek birimi, veri merkezi içinde temel çoğaltma birimidir. Bu düzeyde çoğaltma LRS tarafından sağlanır; Daha fazla bilgi için [yerel olarak yedekli depolama (LRS): Azure depolama için düşük maliyetli veri yedekliği](../articles/storage/common/storage-redundancy-lrs.md).

Hangi çoğaltma seçeneği kullanmak için karar verirken, aşağıdaki noktaları göz önünde bulundurun:

* Bölgesel olarak yedekli depolama (ZRS), yüksek oranda kullanılabilirlik ile zaman uyumlu çoğaltma sağlar ve bazı senaryolar için GRS veya RA-GRS daha iyi bir seçim olabilir. ZRS hakkında daha fazla bilgi için bkz. [ZRS](../articles/storage/common/storage-redundancy-zrs.md).
* Zaman uyumsuz çoğaltma bir gecikme gerektirdiğinden, bölgesel bir olağanüstü durumda birincil bölgeden verilerin kurtarılamaması durumunda ikincil bölgeye henüz çoğaltılmamış değişiklikler kaybolacak mümkündür.
* Microsoft ikincil bölgeye yük devretme başlatır sürece çoğaltma GRS ile kullanılamaz. Microsoft ikincil bölgeye yük devretme başlatmak ise, okuma ve bu verileri yük devretmeden sonra yazma erişimi tamamlandı. Daha fazla bilgi için lütfen bkz [olağanüstü durum kurtarma Kılavuzu](../articles/storage/common/storage-disaster-recovery-guidance.md).
* İkincil bölgeden okumak uygulamanız gerekiyorsa, RA-GRS etkinleştirin.

## <a name="read-access-geo-redundant-storage"></a>Okuma erişimli coğrafi olarak yedekli depolama

Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) depolama hesabınız için kullanılabilirliği en üst düzeye çıkarır. RA-GRS iki bölgede coğrafi çoğaltma ek olarak, ikincil konumdaki verilere salt okunur erişim sağlar.

Yalnızca okuma erişimi verilerinizi ikincil bölgeye etkinleştirdiğinizde, depolama hesabınızın birincil uç noktaya yanı sıra ikincil uç noktaya verilerinizin kullanılabilir. İkincil uç birincil uç noktaya benzer, ancak son ekine ekler `–secondary` hesap adı. Örneğin, Blob hizmetinin birincil uç noktanızı ise `myaccount.blob.core.windows.net`, ikincil uç noktanız ise `myaccount-secondary.blob.core.windows.net`. Depolama hesabınızın erişim anahtarlarını birincil ve ikincil uç için aynıdır.

RA-GRS kullanırken akılda tutulması gereken bazı noktalar:

* Uygulamanızın, RA-GRS kullanırken etkileşim hangi uç noktaya yönetme gerekir.
* Zaman uyumsuz çoğaltma bir gecikme gerektirdiğinden, veriler birincil bölgede, örneğin bölgesel bir olağanüstü durumda kurtarılamaması durumunda ikincil bölgeye henüz çoğaltılmamış değişiklikler kaybolabilir.
* Son eşitleme zamanı depolama hesabınızın kontrol edebilirsiniz. Son eşitleme zamanı tarih/saat GMT bir değerdir. Tüm birincil yazma işlemlerini son eşitleme zamanı önce ikincil konumda ikincil konumdan okumaya kullanılabilir olduğu anlamına gelir, başarılı bir şekilde yazılmış. Son eşitleme süresi okuma için henüz kullanılamıyor olabilir veya sonra birincil yazar. Bu değeri kullanarak sorgulayabilirsiniz [Azure portalında](https://portal.azure.com/), [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), veya bir Azure depolama istemci kitaplıkları.
* Microsoft ikincil bölgeye yük devretme durumunda başlatır, okuduğunuz ve bu verileri yük devretmeden sonra yazma erişimi tamamlandı. Daha fazla bilgi için [olağanüstü durum kurtarma Kılavuzu](../articles/storage/common/storage-disaster-recovery-guidance.md).
* İkincil bölgeye geçiş yapma hakkında daha fazla bilgi için bkz: [bir Azure depolama kesinti oluşursa yapmanız gerekenler](../articles/storage/common/storage-disaster-recovery-guidance.md).
* RA-GRS, yüksek kullanılabilirlik sağlamak için tasarlanmıştır. Ölçeklenebilirlik kılavuzu için gözden [performans denetim](../articles/storage/common/storage-performance-checklist.md).
* RA-GRS ile yüksek kullanılabilirlik için tasarlama konusunda daha fazla bilgi için bkz: [tasarlama yüksek oranda kullanılabilir RA-GRS depolama kullanan uygulamalar](../articles/storage/common/storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>GRS ile RTO ve RPO nedir?
**Kurtarma noktası hedefi (RPO):** GRS ve RA-GRS depolama hizmeti zaman uyumsuz olarak coğrafi-çoğaltır birincil ikincil konumdaki verileri. Birincil bölgede bir ana bölgesel olağanüstü durumda Microsoft ikincil bölgeye yük devretme gerçekleştirir. Bir yük devretme durumda coğrafi olarak çoğaltılmış henüz verilmemiş son değişiklikler kaybolabilir. Kaybedilen potansiyel veri dakika sayısını RPO adlandırılır ve veriler kurtarılabilir zaman noktası gösterir. Azure depolama, genellikle 15 dakikadan kısa bir RPO sahip, rağmen şu anda SLA üzerinde ne kadar süreyle coğrafi çoğaltma olur.

**Kurtarma süresi hedefi (RTO):** RTO yük devretme gerçekleştirmek ve depolama hesabı çevrimiçine almak için ne kadar sürer ölçüdür. Yük devretme gerçekleştirmek için zaman, aşağıdaki eylemleri içerir:

   * Saat, birincil konumda verilerin kurtarılabilir olup olmadığını ya da bir yük devretme gerekli olup olmadığını belirlemek için Microsoft gerektirir.
   * Depolama hesabının ikincil konuma işaret edecek şekilde birincil DNS girişlerini değiştirerek yük devri için geçen süre.

   Microsoft, verilerinizin gerçekten de koruma sorumluluğunu üstlenir. Birincil bölgedeki veriler kurtarma herhangi olasılığı varsa, Microsoft yük devretme gecikme ve verilerinizi kurtarma üzerinde odaklanın. Hizmet gelecek bir sürümünde, böylece RTO kendiniz denetleyebilirsiniz hesap düzeyinde bir yük devretme tetiklemek izin verir.

## <a name="paired-regions"></a>Eşleştirilmiş bölgeler 

Bir depolama hesabı oluşturduğunuzda, hesap için birincil bölge seçin. Eşleştirilmiş ikincil bölgede, birincil bölgeye göre belirlenir ve değiştirilemez. Azure tarafından desteklenen bölgeler hakkında güncel bilgiler için bkz: [iş sürekliliği ve olağanüstü durum kurtarma (BCDR): eşleştirilmiş Azure bölgeleri](../articles/best-practices-availability-paired-regions.md).
