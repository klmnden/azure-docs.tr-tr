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
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30919378"
---
Coğrafi olarak yedekli depolama (GRS), en az %99.99999999999999 sağlamak için tasarlanmıştır (16 9'in) dayanıklılık verileriniz birincil bölge çıktığınızda mil yüzlerce olan bir ikincil bölge çoğaltma tarafından verilen bir yıl içinde nesne. Etkin GRS depolama hesabınız varsa, verilerinizi bile tam bölgesel bir kesintinin veya bir olağanüstü durumda birincil bölge kurtarılabilir değil söz konusu olduğunda dayanıklı.

GRS için seçerseniz, seçebileceğiniz iki ilgili seçeneğiniz vardır:

* Veriler yalnızca Microsoft bir yük devretme olarak birincil kopyadan ikincil bölge başlatır, okumak kullanılabilir ancak bu GRS, verilerinizin ikincil bir bölgede başka bir veri merkezine çoğaltır. 
* Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) üzerinde GRS temel alır. RA-GRS ikincil bir bölgede başka bir veri merkezine verilerinizi yinelenir ve ayrıca, ikincil bölge ' okumak için seçeneği sağlar. RA-GRS ile bir yük devretme olarak birincil kopyadan ikincil Microsoft olup başlatır bağımsız olarak ikincil okuyabilir. 

GRS veya RA-GRS etkin olan bir depolama hesabı için tüm verileri ilk yinelendiğini yerel olarak yedekli depolama (LRS). Bir güncelleştirme ilk birincil konumuna taahhüt eder ve LRS kullanarak çoğaltılır. Güncelleştirmeyi zaman uyumsuz olarak GRS kullanarak ikincil bölge'ye çoğaltılır. Verileri ikincil konumu yazıldığında de LRS kullanarak içinde bu konuma çoğaltılır. 

Birincil ve ikincil bölgeler çoğaltmalar ayrı hata etki alanlarında yönetmek ve depolama ölçek birimi etki alanlarını yükseltme. Depolama ölçek birimi, veri merkezi içinde temel çoğaltma birimdir. Bu düzeyde çoğaltma LRS tarafından sağlanır; Daha fazla bilgi için bkz: [yerel olarak yedekli depolama (LRS): Azure Storage için düşük maliyetli veri artıklığı](../articles/storage/common/storage-redundancy-lrs.md).

Hangi çoğaltma seçeneği kullanacağınıza karar verirken aşağıdaki noktaları göz önünde bulundurun:

* Bölge olarak yedekli depolama (ZRS) zaman uyumlu çoğaltma ile yüksek oranda kullanılabilirlik sağlar ve bazı senaryolar için GRS veya RA-GRS daha iyi bir seçim olabilir. ZRS hakkında daha fazla bilgi için bkz: [ZRS](../articles/storage/common/storage-redundancy-zrs.md).
* Zaman uyumsuz çoğaltma bir gecikme içerdiğinden, bölgesel bir olağanüstü durumda birincil bölgesinden veri kurtarılamazsa, ikincil bölge'ye henüz çoğaltılmamış değişiklikler kaybolacak mümkündür.
* Microsoft yük devretme ikincil bölge başlatır sürece çoğaltma GRS ile kullanılamaz. Microsoft bir yük devretme ikincil bölge başlatın varsa, okuduğunuz ve yük devretme sonrasında bu verilere yazma erişimi tamamlandı. Daha fazla bilgi için lütfen bkz [olağanüstü durum kurtarma Kılavuzu](../articles/storage/common/storage-disaster-recovery-guidance.md).
* Uygulamanızı ikincil bölgesinden okumak gerekirse, RA-GRS etkinleştirin.

## <a name="read-access-geo-redundant-storage"></a>Coğrafi olarak yedekli depolamaya okuma erişimi

Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) depolama hesabınız için kullanılabilirliği en üst düzeye çıkarır. RA-GRS iki bölgede coğrafi çoğaltma ek olarak ikincil konum verilerine salt okunur erişim sağlar.

İkincil bölge verilerinize salt okunur erişimi etkinleştirdiğinizde, verilerinizi ve depolama hesabınız için birincil noktadaki aynı zamanda bir ikincil uç noktası kullanılabilir. İkincil uç birincil uç noktasına benzer, ancak son ekine ekler `–secondary` hesap adı. Örneğin, Blob Hizmeti uç noktanızı birincil ise `myaccount.blob.core.windows.net`, ikincil uç noktanız ise `myaccount-secondary.blob.core.windows.net`. Erişim tuşları depolama hesabınız için birincil ve ikincil uç için aynıdır.

RA-GRS kullanırken göz önünde bulundurmanız gereken bazı noktalar:

* Bunu ile RA-GRS kullanırken etkileşimde hangi uç noktaya yönetmek, uygulamanızın sahiptir.
* Zaman uyumsuz çoğaltma bir gecikme gerektirdiğinden, veri birincil bölgesinden örneğin bölgesel bir olağanüstü durumda kurtarılamazsa, ikincil bölge'ye henüz çoğaltılmamış değişiklikler kaybolabilir.
* Son eşitleme zamanı depolama hesabınızın kontrol edebilirsiniz. Son eşitleme saati GMT tarih/saat değeridir. Son eşitleme zamanı önce tüm birincil yazma başarıyla ikincil konumdan okumak kullanılabilir olduğu anlamına gelir ve ikincil konum için yazılmıştır. Son eşitleme süresi okumalar henüz kullanılabilir durumda olmayabilir veya sonra birincil yazar. Bu değeri kullanarak sorgulama yapabilirsiniz [Azure portal](https://portal.azure.com/), [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), veya bir Azure Storage istemcisi kitaplıklarını.
* Microsoft ikincil bölgeye yük devretme durumunda başlatır, okuduğunuz ve yük devretme sonrasında bu verilere yazma erişimi tamamlandı. Daha fazla bilgi için bkz: [olağanüstü durum kurtarma Kılavuzu](../articles/storage/common/storage-disaster-recovery-guidance.md).
* İkincil bölge'ye geçiş hakkında daha fazla bilgi için bkz: [bir Azure Storage kesinti oluşursa yapmanız gerekenler](../articles/storage/common/storage-disaster-recovery-guidance.md).
* RA-GRS, yüksek kullanılabilirlik sağlamak için tasarlanmıştır. Ölçeklenebilirlik rehberlik için gözden [performans denetim listesi](../articles/storage/common/storage-performance-checklist.md).
* RA-GRS ile yüksek kullanılabilirlik için tasarım konusunda daha fazla bilgi için bkz: [tasarlama yüksek oranda kullanılabilir RA-GRS depolama kullanan uygulamalar](../articles/storage/common/storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>RPO ve GRS ile RTO nedir?
**Kurtarma noktası hedefi (RPO):** GRS ve RA-GRS depolama hizmet zaman uyumsuz olarak coğrafi çoğaltır verilerini birincil ve ikincil konum. Birincil bölgede bir ana bölgesel olağanüstü durumda Microsoft ikincil bölge için bir yük devretme gerçekleştirir. Bir yük devretme durumda coğrafi olarak çoğaltılmış henüz edilmemiş son değişiklikler kaybolabilir. Kayıp olası veri dakika sayısını RPO adlandırılır ve verilerin kurtarılabilmesini zamanında noktasını belirtir. Azure depolama genellikle, 15 dakikadan kısa bir RPO'ya sahip, olmasına rağmen şu anda hiçbir SLA üzerinde ne kadar süreyle coğrafi çoğaltma alır.

**Kurtarma süresi hedefi (RTO):** RTO depolama hesabı çevrimiçine alın ve yük devretme gerçekleştirmek için gereken süreyi ölçüsüdür. Yük devretme gerçekleştirmek için gereken süre aşağıdaki eylemleri içerir:

   * Saat veri birincil konumda kurtarılabilir olup olmadığını veya bir yük devretme gerekli olup olmadığını belirlemek için Microsoft gerektirir.
   * Depolama hesabı için yük devretme ikincil konumunu gösterecek şekilde birincil DNS girdilerini değiştirerek gerçekleştirmek için geçen süre.

   Microsoft, verilerinizin ciddi koruma sorumluluğunu alır. Birincil bölge içinde veri kurtarma herhangi olasılığı varsa, Microsoft yük devretme gecikme ve verilerinizi kurtarma odaklanabilirsiniz. Hizmet gelecek bir sürümünde, böylece kendiniz RTO kontrol edebilirsiniz hesap düzeyinde bir yük devretme tetiklemek izin verir.

## <a name="paired-regions"></a>Eşleştirilmiş bölgeleri 

Bir depolama hesabı oluşturduğunuzda, hesap için birincil bölge seçin. Eşleştirilmiş ikincil bölge birincil bölgeye göre belirlenir ve değiştirilemez. Azure tarafından desteklenen bölgeler hakkında güncel bilgiler için bkz: [iş devamlılığı ve olağanüstü durum kurtarma (BCDR): Azure eşleştirilmiş bölgeleri](../articles/best-practices-availability-paired-regions.md).
