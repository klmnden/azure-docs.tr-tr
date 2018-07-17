---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/03/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a88588497919d6cce17ced6d94de3bcbbb6a3019
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39069639"
---
Bölgesel olarak yedekli depolama (ZRS), verilerinizi tek bir bölgede üç depolama kümeleri arasında eşzamanlı olarak çoğaltır. Her Depolama kümesi diğerlerinden fiziksel olarak ayrılır ve kendi kullanılabilirlik bölgesinde (AZ) yer alıyor. Her kullanılabilirlik alanı ve ZRS küme içindeki ayrı yardımcı programları ve ağ özellikleri ile otonom.

Verilerinizi depolanması bir ZRS hesabına, mümkün erişimin ve bölge kullanılamaz hale gelmesi durumunda, verilerinizi yönetin sağlar. ZRS, üstün performans ve düşük gecikme sağlar. ZRS sunar aynı [ölçeklenebilirlik hedefleri](../articles/storage/common/storage-scalability-targets.md) olarak [yerel olarak yedekli depolama (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

ZRS bir bölgesel veri merkezinde bir kesinti veya doğal afetler kullanılamaz işler olsa bile, güçlü tutarlılık, güçlü dayanıklılık ve yüksek kullanılabilirlik gerektiren senaryolar için göz önünde bulundurun. ZRS, en az % 99,9999999999 depolama nesneleri için dayanıklılık sunar (12 9) belirli bir yıl boyunca.

Kullanılabilirlik alanları hakkında daha fazla bilgi için bkz. [kullanılabilirlik alanlarına genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview).

## <a name="support-coverage-and-regional-availability"></a>Destek kapsamı ve bölgesel kullanılabilirlik
ZRS şu anda standart destekler [genel amaçlı v2 (GPv2)](../articles/storage/common/storage-account-options.md#general-purpose-v2-accounts) hesap türleri. ZRS, blok blobları, disk olmayan sayfa blobları, dosyalar, tablolar ve Kuyruklar için kullanılabilir. Ayrıca, tüm, [depolama analizi](../articles/storage/common/storage-analytics.md) günlükleri ve [depolama ölçümleri](../articles/storage/common/storage-enable-and-view-metrics.md)

ZRS şu bölgelerde genel kullanıma sunulmuştur:

- ABD Doğu 2
- ABD Orta
- Kuzey Avrupa
- Batı Avrupa
- Fransa Orta
- Güneydoğu Asya

Microsoft, ek Azure bölgelerinde ZRS etkinleştirmek devam ediyor. Denetleme [Azure hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/) düzenli olarak yeni bölgeler hakkında daha fazla bilgi için.

## <a name="what-happens-when-a-zone-becomes-unavailable"></a>Bir bölge kullanılamaz duruma geldiğinde ne olur?

Verilerinizi bir bölge kullanılamaz duruma gelirse dayanıklı kalır. Microsoft, geçici hata işleme, üstel geri alma ile yeniden deneme ilkelerini uygulayan gibi yöntemleri izlemek için devam edin önerir. Bir bölge kullanılamadığında, Azure DNS repointing gibi ağ güncelleştirmeleri üstlendiği. Bunlar tamamlanmadan verilerinizi sağlıyorsanız bu güncelleştirmeler, uygulamanızın etkileyebilir.

ZRS, verilerinizin nerede birden çok bölge kalıcı olarak etkilenen bölgesel bir olağanüstü durum karşı koruma. Bunun yerine, geçici olarak kullanılamaz hale gelirse ZRS, verilerinizin dayanıklılık sunar. Bölgesel felaketlere karşı koruma için coğrafi olarak yedekli depolama (GRS) kullanarak Microsoft önerir. GRS hakkında daha fazla bilgi için bkz: [coğrafi olarak yedekli depolama (GRS): Azure depolama için bölgeler arası çoğaltma](../articles/storage/common/storage-redundancy-grs.md).

## <a name="converting-to-zrs-replication"></a>ZRS çoğaltmalı dönüştürme
LRS, GRS ve RA-GRS depolamadan geçirdiğiniz sürece Bugün, Azure portalında veya depolama kaynak sağlayıcısı API'si, hesabınızın yedeklilik türünü değiştirmek için kullanabilirsiniz. Fiziksel veri hareketi tek depolama damgası bir bölgede birden fazla damga içerdiğinden ZRS ile ancak geçiş olarak açık değildir. 

İçin iki birincil seçenek geçiş için veya ZRS var. Elle kopyalamanız veya veri, var olan hesabınızı yeni bir ZRS hesabına taşıyın. Dinamik geçiş da isteyebilirsiniz. Microsoft, dinamik geçiş tamamlandığında dair bir garanti olduğundan el ile geçiş gerçekleştirmek kesinlikle önerir. El ile geçiş yolu, dinamik geçiş yapar ve zamanlama geçiş denetimi sizdedir daha fazla esneklik sağlar.

El ile geçiş gerçekleştirmek için çeşitli seçenekler vardır:
- AzCopy, depolama SDK'sı, güvenilir bir üçüncü taraf araçları vb. gibi araçları kullanın.
- Hadoop veya HDInsight ile bilginiz varsa, hem kaynak ekleyebilirsiniz ve hedef (ZRS) hesabı kümenize ve DistCp gibi yüksek düzeyde veri kopyalama işlemini paralel hale getirmek için kullanın
- Bir depolama SDK'sı örneğinizin yararlanarak kendi araçları oluşturun

Ancak, bazı uygulama kapalı kalma süresi el ile geçiş neden olur ve sizin, devralarak belirleyemiyoruz Microsoft dinamik geçiş seçeneği sağlar. Dinamik geçiş, verilerin kaynak ve hedef depolama Damgalar arasında geçirildiğine sırasında var olan depolama hesabınızı kullanmaya devam etmenize olanak tanıyan bir yerinde geçiş olur. Geçiş sırasında her zamanki gibi aynı düzeyde dayanıklılık ve kullanılabilirlik SLA'sı hala gerekir.

Dinamik geçiş, ancak bazı kısıtlamalar ile geliyor. Aşağıda listelenmiştir.

- Microsoft, dinamik geçiş isteğinizi en kısa sürede değerlendirecektir olsa da geçiş tamamlandığında dair bir garanti yoktur. Verilerinizi tarafından belirli bir süre içinde ZRS olması gerekiyorsa, el ile geçiş yapmanız gerekir. Genellikle, hesabınızda bir daha fazla veriniz uzun, verileri geçirmek için sürer. 
- LRS veya GRS çoğaltma'yı kullanarak bir hesaptan yalnızca dinamik geçiş gerçekleştirmek. RA-GRS hesabınızın kullanıyorsa, bu çoğaltma türlerden devam etmeden önce ilk geçirmek gerekir. Bu ara adım, RA-GRS geçişten önce kaldırılır sağlayan ikincil salt okunur uç sağlar.
- Hesabınıza veri içermesi gerekir.
- Yalnızca bölge içi geçişler desteklenir. Veri kaynağı hesaptan farklı bir bölgede bulunan bir ZRS hesabına geçirmek istiyorsanız, el ile geçiş gerçekleştirmeniz gerekir.
- Yalnızca standart depolama hesabı türleri desteklenir. Bir premium depolama hesabından geçiremezsiniz.

Dinamik geçiş istekleri, Azure destek portalı üzerinden gider. Portalda, ZRS için dönüştürmek istediğiniz depolama hesabını seçin.
1. Tıklayın **yeni destek isteği**
2. Temel bilgileri doğrulayın. **İleri**’ye tıklayın. 
3. Üzerinde **sorun** bölüm 
    - Önem derecesine sahip olarak bırakın-olduğu.
    - Sorun türü = **veri geçişi**
    - Kategori = **ZRS bir bölge içinde geçiş**
    - Başlık = **ZRS hesap geçiş** (veya gerçekleşmediğini tanımlayıcı)
    - Ayrıntılar = [LRS'den, GRS] için ZRS geçirmek ___ bölgede istiyorum. 
4. **İleri**’ye tıklayın.
5. Kişi bilgileri kişi bilgileri dikey penceresinde doğru olduğundan emin olun.
6. Tıklayın **gönderme**.

Destek ekibiyle sonra sizinle iletişime geçeceğiz olur. Söz konusu kişinin gerektirebilecek herhangi bir Yardım sağlamak kullanılabilir. 

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>ZRS Klasik: Eski bir seçenek blok blobları artıklığı
> [!NOTE]
> ZRS Klasik hesapları kullanımdan kaldırma ve 31 Mart 2021 gerekli geçiş duyurulacaktır. Microsoft, kullanımdan önce ZRS Klasik müşterilere daha ayrıntılı bilgi gönderir. Microsoft Otomatik geçiş işlemi ZRS Klasik ZRS için gelecekte sağlamayı planlamaktadır.

>[!NOTE]
> ZRS olduğunda [sunuldu](#support-coverage-and-regional-availability) bir bölgede, artık ZRS Klasik hesabı, aynı bölgedeki portalından oluşturmak mümkün olmayacak. ZRS Klasik kullanım dışı kadar ancak hala başka bir yolla Microsoft PowerShell ve Azure CLI aracılığıyla başka bir deyişle, oluşturabilirsiniz.

ZRS Klasik zaman uyumsuz olarak verileri bir veya iki bölgedeki veri merkezleri arasında çoğaltır. Çoğaltma, Microsoft ikincil birime yük devretme işlemini başlatana kadar kullanılamayabilir. ZRS Klasik, kullanılabilir yalnızca **blok blobları** içinde [genel amaçlı V1 (GPv1)](../articles/storage/common/storage-account-options.md#general-purpose-v1-accounts) depolama hesapları. ZRS Klasik hesabı, LRS veya GRS’ye iki yönlü olarak dönüştürülemez ve ölçüm veya günlüğe kaydetme özelliklerine sahip değildir.

ZRS Klasik hesapları için veya LRS, GRS veya RA-GRS dönüştürülemez. ZRS Klasik hesapları ölçüm veya günlük da desteklemez.

El ile ya da bir LRS, ZRS Klasik, GRS veya RA-GRS hesabı'ndan ZRS hesap verileri geçirmek için AzCopy, Azure Depolama Gezgini, Azure PowerShell veya Azure CLI'yı kullanın. Ayrıca, kendi geçiş çözümüyle Azure depolama istemci kitaplıklarından birini de oluşturabilirsiniz.
