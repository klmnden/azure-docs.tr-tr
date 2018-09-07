---
title: Bölgesel olarak yedekli depolama (ZRS) Azure depolama yüksek oranda kullanılabilir uygulamalar oluşturun | Microsoft Docs
description: Bölgesel olarak yedekli depolama (ZRS), yüksek kullanılabilirliğe sahip uygulamalar oluşturmak için basit bir yol sunar. ZRS, veri merkezindeki donanım arızalarına karşı ve bazı bölgesel felaketlere karşı korur.
services: storage
author: tolandmike
ms.service: storage
ms.topic: article
ms.date: 03/20/2018
ms.author: jeking
ms.component: common
ms.openlocfilehash: 0bca4825c757604ab15838aac585603be0616582
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44025344"
---
# <a name="zone-redundant-storage-zrs-highly-available-azure-storage-applications"></a>Bölgesel olarak yedekli depolama (ZRS): Azure depolama yüksek kullanılabilirliğe sahip uygulamalar
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-zrs.md)]

## <a name="support-coverage-and-regional-availability"></a>Destek kapsamı ve bölgesel kullanılabilirlik
ZRS şu anda standart destekler [genel amaçlı v2 (GPv2)](storage-account-options.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#general-purpose-v2-accounts) hesap türleri. ZRS, blok blobları, disk olmayan sayfa blobları, dosyalar, tablolar ve Kuyruklar için kullanılabilir. Ayrıca, tüm, [depolama analizi](storage-analytics.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) günlükleri ve [depolama ölçümleri](storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)

ZRS şu bölgelerde genel kullanıma sunulmuştur:

- ABD Doğu
- ABD Doğu 2
- ABD Batı 2
- ABD Orta
- Kuzey Avrupa
- Batı Avrupa
- Fransa Orta
- Güneydoğu Asya

Microsoft, ek Azure bölgelerinde ZRS etkinleştirmek devam ediyor. Denetleme [Azure hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/) düzenli olarak yeni bölgeler hakkında daha fazla bilgi için.

## <a name="what-happens-when-a-zone-becomes-unavailable"></a>Bir bölge kullanılamaz duruma geldiğinde ne olur?
Verilerinizi bir bölge kullanılamaz duruma gelirse dayanıklı kalır. Microsoft, geçici hata işleme, üstel geri alma ile yeniden deneme ilkelerini uygulayan gibi yöntemleri izlemek için devam edin önerir. Bir bölge kullanılamadığında, Azure DNS repointing gibi ağ güncelleştirmeleri üstlendiği. Bunlar tamamlanmadan verilerinizi sağlıyorsanız bu güncelleştirmeler, uygulamanızın etkileyebilir.

ZRS, verilerinizin nerede birden çok bölge kalıcı olarak etkilenen bölgesel bir olağanüstü durum karşı koruma. Bunun yerine, geçici olarak kullanılamaz hale gelirse ZRS, verilerinizin dayanıklılık sunar. Bölgesel felaketlere karşı koruma için coğrafi olarak yedekli depolama (GRS) kullanarak Microsoft önerir. GRS hakkında daha fazla bilgi için bkz: [coğrafi olarak yedekli depolama (GRS): Azure depolama için bölgeler arası çoğaltma](storage-redundancy-grs.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

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
> Microsoft, kullanımdan ve 31 Mart 2021 üzerinde ZRS Klasik hesapları geçirme. ZRS Klasik müşterilere kullanımdan önce daha fazla ayrıntı sağlanacaktır. 
>
> ZRS olduğunda [sunuldu](#support-coverage-and-regional-availability) bir bölgede müşteriler artık ZRS Klasik hesapları bu bölgede portalından oluşturmak mümkün olacaktır. ZRS Klasik kullanım dışı kadar ZRS Klasik hesapları oluşturmak için Microsoft PowerShell ve Azure CLI kullanarak desteklenir.

ZRS Klasik zaman uyumsuz olarak verileri bir veya iki bölgedeki veri merkezleri arasında çoğaltır. Çoğaltma, Microsoft ikincil birime yük devretme işlemini başlatana kadar kullanılamayabilir. ZRS Klasik, kullanılabilir yalnızca **blok blobları** içinde [genel amaçlı V1 (GPv1)](storage-account-options.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#general-purpose-v1-accounts) depolama hesapları. ZRS Klasik hesabı, LRS veya GRS’ye iki yönlü olarak dönüştürülemez ve ölçüm veya günlüğe kaydetme özelliklerine sahip değildir.

ZRS Klasik hesapları için veya LRS, GRS veya RA-GRS dönüştürülemez. ZRS Klasik hesapları ölçüm veya günlük da desteklemez.

El ile ya da bir LRS, ZRS Klasik, GRS veya RA-GRS hesabı'ndan ZRS hesap verileri geçirmek için AzCopy, Azure Depolama Gezgini, Azure PowerShell veya Azure CLI'yı kullanın. Ayrıca, kendi geçiş çözümüyle Azure depolama istemci kitaplıklarından birini de oluşturabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.
- [Azure Depolama çoğaltması](storage-redundancy.md)
- [Yerel olarak yedekli depolama (LRS): Azure depolama için düşük maliyetli veri yedekliği](storage-redundancy-lrs.md)
- [Coğrafi olarak yedekli depolama (GRS): Azure depolama için bölgeler arası çoğaltma](storage-redundancy-grs.md)