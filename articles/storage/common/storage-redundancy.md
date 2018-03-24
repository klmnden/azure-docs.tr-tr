---
title: Azure Storage veri çoğaltması | Microsoft Docs
description: Microsoft Azure depolama hesabınızdaki veriler dayanıklılık ve yüksek kullanılabilirlik için çoğaltılır. Çoğaltma seçenekleri yerel olarak yedekli depolama (LRS), bölge olarak yedekli depolama (ZRS), coğrafi olarak yedekli depolama (GRS) ve okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) içerir.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.workload: storage
ms.topic: article
ms.date: 01/21/2018
ms.author: tamram
ms.openlocfilehash: 600b66af3b7da24c5a40d09d5cdf76f2d5be67ac
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-storage-replication"></a>Azure Storage çoğaltma

Microsoft Azure Depolama hesabınızdaki veriler, dayanıklılık ve yüksek kullanılabilirlik sağlamak için her zaman çoğaltılır. Böylece uygulama yukarı zamanı koruma geçici donanım hatalarından korumalı çoğaltma verilerinizi kopyalar. 

Verilerinizi aynı veri merkezi içinde aynı bölge içinde veri merkezleri arasında veya bölgeler arasında çoğaltma seçebilirsiniz. Verilerinizin birden çok veri merkezleri arasında veya bölgeler arasında çoğaltılır, tek bir konumda yıkıcı bir hatadan da korunur.

Çoğaltma işlemi, hata durumunda bile depolama hesabınızın [Depolama için Hizmet Düzeyi Sözleşmesi'ne (SLA)](https://azure.microsoft.com/support/legal/sla/storage/) uymasını sağlar. Azure Depolama'nın dayanıklılık ve kullanılabilirlikle ilgili sağladığı garantiler hakkında bilgi edinmek için SLA'ya göz atın.

Bir depolama hesabı oluşturduğunuzda şu çoğaltma seçeneklerinden birini seçebilirsiniz:

* [Yerel olarak yedekli depolama (LRS)](#locally-redundant-storage)
* [Bölgesel olarak yedekli depolama (ZRS)](#zone-redundant-storage)
* [Coğrafi olarak yedekli depolama (GRS)](#geo-redundant-storage)
* [Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)](#read-access-geo-redundant-storage)

Bir depolama hesabı oluşturduğunuzda, yerel olarak yedekli depolama (LRS) varsayılan seçenektir.

Aşağıdaki tabloda LRS, ZRS, GRS ve RA-GRS arasındaki farklar hızlı bir bakış sağlar. Bu makalenin sonraki bölümlerinde daha ayrıntılı çoğaltma her türünü adres.

| Çoğaltma stratejisi | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Veriler birden çok veri merkezi arasında çoğaltılır. |Hayır |Evet |Evet |Evet |
| Verileri ikincil bir konuma yanı sıra birincil konumda okuyabilir. |Hayır |Hayır |Hayır |Evet |
| Belirli bir yılda nesnelerin ___ dayanıklılık sağlamak üzere tasarlanmıştır. |en az %99.999999999 (11 9'ın)|en az %99.9999999999 (12 9'ın)|en az %99.99999999999999 (16 9'ın)|en az %99.99999999999999 (16 9'ın)|

Bkz: [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) farklı artıklık seçenekleri için fiyatlandırma bilgilerini için.

> [!NOTE]
> Premium depolama yalnızca yerel olarak yedekli depolama (LRS) destekler. Premium depolama hakkında daha fazla bilgi için bkz: [Premium Storage: Azure sanal makine iş yükleri için yüksek performanslı depolama](../../virtual-machines/windows/premium-storage.md).
>

## <a name="locally-redundant-storage"></a>Yerel olarak yedekli depolama
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

## <a name="zone-redundant-storage"></a>Bölge olarak yedekli depolama
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="zrs-classic-accounts"></a>ZRS Klasik hesapları

Artık mevcut ZRS özelliği için ZRS Klasik adlandırılır. ZRS Klasik hesapları yalnızca blok blob’larına ve genel amaçlı V1 depolama hesaplarında sunulur. 

ZRS Klasik, verileri zaman uyumsuz olarak bir veya iki bölge içindeki veri merkezleri arasında çoğaltır. Çoğaltma, Microsoft ikincil birime yük devretme işlemini başlatana kadar kullanılamayabilir. 

ZRS Klasik hesapları için veya LRS, GRS veya RA-GRS dönüştürülemez. ZRS Klasik hesapları, ölçümleri veya günlük kaydı da desteklemez.   

ZRS bir bölgede genellikle kullanılabilir olduğunda, artık bu bölgede portalından ZRS klasik bir hesap oluşturmak mümkün olacaktır, ancak arasında başka yollarla bir tane oluşturabilirsiniz.  
Bir otomatik geçiş işleminden ZRS Klasik ZRS için gelecekte sağlanır.

El ile ya da bir LRS, ZRS Klasik, GRS ve RAGRS hesaptan ZRS hesap verileri geçirebilirsiniz. AzCopy, Azure Storage Gezgini, Azure PowerShell, Azure CLI veya Azure Storage istemci kitaplıklarından birini kullanarak bu el ile geçiş işlemi gerçekleştirebilirsiniz.

> [!NOTE]
> ZRS Klasik hesaplarını kullanımdan kaldırma ve 31 Mart 2021 üzerinde gerekli geçiş için planlanmıştır. Microsoft, kullanımdan önce ZRS Klasik müşteriler için daha fazla bilgi gönderir.

Ek soruları ele içinde [sık sorulan sorular](#frequently-asked-questions) bölümü. 

## <a name="geo-redundant-storage"></a>Coğrafi Olarak Yedekli Depolama
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="read-access-geo-redundant-storage"></a>Coğrafi olarak yedekli depolamaya okuma erişimi
Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) depolama hesabınız için kullanılabilirliği en üst düzeye çıkarır. RA-GRS iki bölgede coğrafi çoğaltma ek olarak ikincil konum verilerine salt okunur erişim sağlar.

İkincil bölge verilerinize salt okunur erişimi etkinleştirdiğinizde, verilerinizi ve depolama hesabınız için birincil noktadaki aynı zamanda bir ikincil uç noktası kullanılabilir. İkincil uç birincil uç noktasına benzer, ancak son ekine ekler `–secondary` hesap adı. Örneğin, Blob Hizmeti uç noktanızı birincil ise `myaccount.blob.core.windows.net`, ikincil uç noktanız ise `myaccount-secondary.blob.core.windows.net`. Erişim tuşları depolama hesabınız için birincil ve ikincil uç için aynıdır.

RA-GRS kullanırken göz önünde bulundurmanız gereken bazı noktalar:

* Bunu ile RA-GRS kullanırken etkileşimde hangi uç noktaya yönetmek, uygulamanızın sahiptir.
* Zaman uyumsuz çoğaltma bir gecikme gerektirdiğinden, veri birincil bölgesinden örneğin bölgesel bir olağanüstü durumda kurtarılamazsa, ikincil bölge'ye henüz çoğaltılmamış değişiklikler kaybolabilir.
* Microsoft ikincil bölgeye yük devretme durumunda başlatır, okuduğunuz ve yük devretme sonrasında bu verilere yazma erişimi tamamlandı. Daha fazla bilgi için bkz: [olağanüstü durum kurtarma Kılavuzu](../storage-disaster-recovery-guidance.md).
* RA-GRS, yüksek kullanılabilirlik sağlamak için tasarlanmıştır. Ölçeklenebilirlik rehberlik için gözden [performans denetim listesi](../storage-performance-checklist.md).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

<a id="howtochange"></a>
#### <a name="1-how-can-i-change-the-geo-replication-type-of-my-storage-account"></a>1. Depolama Hesabımı coğrafi çoğaltma türü nasıl değiştirebilir miyim?

   Depolama hesabınız coğrafi çoğaltma türünü kullanarak değiştirebilirsiniz [Azure portal](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md), ya da Azure Storage istemci kitaplıklarından birini.

   > [!NOTE]
   > ZRS hesapları dönüştürülmüş LRS veya GRS olamaz. Benzer şekilde, varolan LRS veya GRS hesabı ZRS hesabına dönüştürülemiyor.
    
<a id="changedowntime"></a>
#### <a name="2-does-changing-the-replication-type-of-my-storage-account-result-in-down-time"></a>2. My depolama hesabı sonucu kesinti çoğaltma türünü değiştirme mu?

   Hayır, depolama hesabınız çoğaltma türünü değiştirme süresini neden değil.

<a id="changecost"></a>
#### <a name="3-are-there-additional-costs-to-changing-the-replication-type-of-my-storage-account"></a>3. Depolama Hesabımı çoğaltma türünü değiştirmek için ek ücretler var mı?

   Evet. Depolama hesabınız için GRS (veya RA-GRS) LRS'den değiştirirseniz, varolan verilerin birincil konumdan ikincil konuma kopyalanması söz konusu çıkışı için ek bir ücret doğurur. İlk veri kopyalandıktan sonra coğrafi çoğaltma olarak birincil kopyadan ikincil konuma için hiçbir ek ek çıkış ücretlerini vardır. Bant genişliği ücretleri hakkında daha fazla bilgi için bkz: [Azure depolama Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/storage/blobs/).

   LRS için GRS değiştirirseniz, ek bir maliyet yoktur, ancak, verileri ikincil konumdan silinir.

<a id="ragrsbenefits"></a>
#### <a name="4-how-can-ra-grs-help-me"></a>4. RA-GRS bana nasıl yardımcı?

   GRS depolama çoğaltmayı birincil sunucudan verileriniz birincil bölge çıktığınızda mil yüzlerce olan ikincil bir bölgeye sağlar. GRS ile bile tam bölgesel bir kesintinin veya bir olağanüstü durumda birincil bölge kurtarılabilir değil durumunda verilerinizi sağlam değil. RA-GRS depolama GRS çoğaltma sunar ve verileri ikincil konumdan okuma özelliği ekler. Yüksek kullanılabilirlik için tasarım konusunda daha fazla bilgi için bkz: [tasarlama yüksek oranda kullanılabilir RA-GRS depolama kullanan uygulamalar](../storage-designing-ha-apps-with-ragrs.md).

<a id="lastsynctime"></a>
#### <a name="5-is-there-a-way-to-figure-out-how-long-it-takes-to-replicate-my-data-from-the-primary-to-the-secondary-region"></a>5. Verilerimi birincil sunucudan ikincil bölge'ye çoğaltmak için gereken süreyi şekilde yolu var mı?

   RA-GRS depolama kullanıyorsanız, depolama hesabınıza son eşitleme zamanı kontrol edebilirsiniz. Son eşitleme saati GMT tarih/saat değeridir. Son eşitleme zamanı önce tüm birincil yazma başarıyla ikincil konumdan okumak kullanılabilir olduğu anlamına gelir ve ikincil konum için yazılmıştır. Son eşitleme süresi okumalar henüz kullanılabilir durumda olmayabilir veya sonra birincil yazar. Bu değeri kullanarak sorgulama yapabilirsiniz [Azure portal](https://portal.azure.com/), [Azure PowerShell](storage-powershell-guide-full.md), veya bir Azure Storage istemcisi kitaplıklarını.

<a id="outage"></a>
#### <a name="6-if-there-is-an-outage-in-the-primary-region-how-do-i-switch-to-the-secondary-region"></a>6. Birincil bölgede bir kesinti durumunda nasıl ikincil bölge'ye geçiş?

   Daha fazla bilgi için bkz: [bir Azure Storage kesinti oluşursa yapmanız gerekenler](../storage-disaster-recovery-guidance.md).

<a id="rpo-rto"></a>
#### <a name="7-what-is-the-rpo-and-rto-with-grs"></a>7. RPO ve GRS ile RTO nedir?

   **Kurtarma noktası hedefi (RPO):** GRS ve RA-GRS depolama hizmet zaman uyumsuz olarak coğrafi çoğaltır verilerini birincil ve ikincil konum. Birincil bölgede bir ana bölgesel olağanüstü durumda Microsoft ikincil bölge için bir yük devretme gerçekleştirir. Bir yük devretme durumda coğrafi olarak çoğaltılmış henüz edilmemiş son değişiklikler kaybolabilir. Kayıp olası veri dakika sayısını RPO adlandırılır ve verilerin kurtarılabilmesini zamanında noktasını belirtir. Azure depolama genellikle, 15 dakikadan kısa bir RPO'ya sahip, olmasına rağmen şu anda hiçbir SLA üzerinde ne kadar süreyle coğrafi çoğaltma alır.

   **Kurtarma süresi hedefi (RTO):** RTO depolama hesabı çevrimiçine alın ve yük devretme gerçekleştirmek için gereken süreyi ölçüsüdür. Yük devretme gerçekleştirmek için gereken süre aşağıdaki eylemleri içerir:

   * Saat veri birincil konumda kurtarılabilir olup olmadığını veya bir yük devretme gerekli olup olmadığını belirlemek için Microsoft gerektirir.
   * Depolama hesabı için yük devretme ikincil konumunu gösterecek şekilde birincil DNS girdilerini değiştirerek gerçekleştirmek için geçen süre.

   Microsoft, verilerinizin ciddi koruma sorumluluğunu alır. Birincil bölge içinde veri kurtarma herhangi olasılığı varsa, Microsoft yük devretme gecikme ve verilerinizi kurtarma odaklanabilirsiniz. Hizmet gelecek bir sürümünde, böylece kendiniz RTO kontrol edebilirsiniz hesap düzeyinde bir yük devretme tetiklemek izin verir.

#### <a name="8-what-azure-storage-objects-does-zrs-support"></a>8. Hangi Azure Storage nesneleri ZRS destekliyor mu? 
Blok blobları, sayfa blobları (dışında bu yedekleme VM diskleri), tablolar, dosyalar ve sıralar. 

#### <a name="9-does-zrs-also-include-geo-replication"></a>9. ZRS, ayrıca coğrafi çoğaltma içeriyor mu? 
Şu anda ZRS coğrafi çoğaltma desteklemiyor. Senaryonuz olağanüstü durum kurtarma amacıyla bölgeler arası çoğaltma gerektiriyorsa, bir GRS veya RA-GRS depolama hesabı kullanın.   

#### <a name="10-what-happens-when-one-or-more-zrs-zones-go-down"></a>10. Bir veya daha fazla ZRS bölgeleri Git aşağı ne olur? 
İlk bölgeye azaldığında ZRS bölgede iki kalan bölgeler arasında verilerin çoğaltmalarının yazmaya devam eder. İkinci bir saat dilimi devre dışı kalırsa, okuma ve yazma erişimi kullanılamıyor kadar en az iki bölgeleri yeniden kullanılabilir. 

## <a name="next-steps"></a>Sonraki adımlar
* [RA-GRS depolama kullanarak yüksek oranda kullanılabilir uygulamalar tasarlama](../storage-designing-ha-apps-with-ragrs.md)
* [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/)
* [Azure storage hesapları hakkında](../storage-create-storage-account.md)
* [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md)
* [Microsoft Azure Depolama artıklık seçenekleri ve okuma erişimi coğrafi olarak yedekli depolama ](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
* [SOSP belgesi - Azure Storage: Yüksek oranda kullanılabilir depolama sahip bulut hizmeti güçlü tutarlılık](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
