---
title: Bölgesel olarak yedekli depolama (ZRS) Azure depolama yüksek oranda kullanılabilir uygulamalar oluşturun | Microsoft Docs
description: Bölgesel olarak yedekli depolama (ZRS), yüksek kullanılabilirliğe sahip uygulamalar oluşturmak için basit bir yol sunar. ZRS, veri merkezindeki donanım arızalarına karşı ve bazı bölgesel felaketlere karşı korur.
services: storage
author: tolandmike
ms.service: storage
ms.topic: article
ms.date: 10/24/2018
ms.author: jeking
ms.component: common
ms.openlocfilehash: 776feba239af5f2cafaf7229554960fab3417943
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55093368"
---
# <a name="zone-redundant-storage-zrs-highly-available-azure-storage-applications"></a>Bölgesel olarak yedekli depolama (ZRS): Azure depolama yüksek kullanılabilirliğe sahip uygulamalar
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-zrs.md)]

## <a name="support-coverage-and-regional-availability"></a>Destek kapsamı ve bölgesel kullanılabilirlik
ZRS şu anda standart genel amaçlı v2 hesabı türünü destekler. Depolama hesabı türleri hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](storage-account-overview.md).

ZRS, blok blobları, disk olmayan sayfa blobları, dosyalar, tablolar ve Kuyruklar için kullanılabilir.

ZRS şu bölgelerde genel kullanıma sunulmuştur:

- Güneydoğu Asya
- Batı Avrupa
- Kuzey Avrupa
- Fransa Orta
- Japonya Doğu
- ABD Doğu
- ABD Doğu 2
- ABD Batı 2
- ABD Orta

Microsoft, ek Azure bölgelerinde ZRS etkinleştirmek devam ediyor. Denetleme [Azure hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/) düzenli olarak yeni bölgeler hakkında daha fazla bilgi için.

## <a name="what-happens-when-a-zone-becomes-unavailable"></a>Bir bölge kullanılamaz duruma geldiğinde ne olur?
Verilerinizi hem de okuma ve yazma işlemleri bile bir bölge kullanılamaz hale gelmesi için hala erişilebilir. Microsoft, geçici hata işlemeye yönelik uyarsanız devam etmesini önerir. Bu yöntemler, üstel geri alma ile yeniden deneme ilkelerini uygulayan içerir.

Bir bölge kullanılamadığında, Azure DNS repointing gibi ağ güncelleştirmeleri üstlendiği. Güncelleştirmeleri tamamlanmadan verilerinizi sağlıyorsanız bu güncelleştirmeler, uygulamanızın etkileyebilir.

ZRS, verilerinizin nerede birden çok bölge kalıcı olarak etkilenen bölgesel bir olağanüstü durum karşı koruma. Bunun yerine, geçici olarak kullanılamaz hale gelirse ZRS, verilerinizin dayanıklılık sunar. Bölgesel felaketlere karşı koruma için coğrafi olarak yedekli depolama (GRS) kullanarak Microsoft önerir. GRS hakkında daha fazla bilgi için bkz: [coğrafi olarak yedekli depolama (GRS): Azure depolama için bölgeler arası çoğaltma](storage-redundancy-grs.md).

## <a name="converting-to-zrs-replication"></a>ZRS çoğaltmalı dönüştürme
LRS, GRS ve RA-GRS gelen veya geçirme adımları oldukça kolaydır. Azure portalı veya depolama kaynak sağlayıcısı API'si, hesabınızın yedeklilik türünü değiştirmek için kullanın. Azure, verilerinizi daha sonra uygun şekilde çoğaltır. 

Veri geçişi ZRS gelen veya farklı bir strateji gerektirir. ZRS geçişi, bir bölgede birden fazla damga tek bir depolama damga verileri fiziksel hareketini içerir.

İçin iki birincil seçenek geçiş için veya ZRS vardır: 

- Elle kopyalamanız veya veri, var olan bir hesabı yeni bir ZRS hesabına taşıyın.
- Dinamik geçiş isteyin.

Microsoft, el ile geçiş gerçekleştirmek kesinlikle önerir. El ile geçiş, dinamik geçiş göre daha fazla esneklik sağlar. El ile geçiş ile zamanlama, Denetim sizdedir.

El ile geçiş gerçekleştirmek için seçeneğiniz vardır:
- AzCopy, Azure Storage istemcisi kitaplıklarını veya güvenilir bir üçüncü taraf araçları gibi araçları kullanın.
- Hadoop veya HDInsight ile sahibiyseniz eklemek hem kaynak hem de hedef (ZRS) hesabı kümenize. Ardından, veri kopyalama işlemini DistCp gibi bir araç ile paralel hale getirmek.
- Azure depolama istemci kitaplıklarından birini kullanarak kendi araçları oluşturun.

El ile geçiş uygulama kapalı kalma süresi neden olabilir. Uygulamanıza yüksek kullanılabilirlik gerekiyorsa, Microsoft, ayrıca bir dinamik geçiş seçeneği sunar. Dinamik geçiş, bir yerinde geçiş olur. 

Verilerinizi, kaynak ve hedef depolama Damgalar arasında geçişi sırasında dinamik geçiş sırasında depolama hesabınızı kullanabilirsiniz. Normalde yaptığınız gibi geçiş işlemi sırasında aynı düzeyde dayanıklılık ve kullanılabilirlik SLA'sı vardır.

Dinamik geçiş üzerine aşağıdaki kısıtlamaları göz önünde bulundurun:

- Microsoft kısa süre içinde dinamik geçiş için isteğinizi işlerken, dinamik geçiş tamamlandığında dair bir garanti yoktur. Gerekirse verilerinizi ZRS için belirli bir tarihe kadar geçişi ve ardından Microsoft yerine el ile geçiş gerçekleştirmek önerir. Genellikle, hesabınızda bir daha fazla veriniz, verileri geçirmek için uzar. 
- Dinamik geçiş kullanan çoğaltma LRS veya GRS depolama hesapları için desteklenir. RA-GRS hesabınızın kullanıyorsa, ilk LRS veya GRS hesabınızın çoğaltma türü bu devam etmeden önce değiştirmeniz gerekir. Aracı bu adım, geçiş işleminden önce RA-GRS tarafından sağlanan ikincil salt okunur uç kaldırır.
- Hesabınıza veri içermesi gerekir.
- Yalnızca aynı bölgede verilerin geçirebilirsiniz. Veri kaynağı hesaptan farklı bir bölgede bulunan bir ZRS hesabına geçirmek istiyorsanız, el ile geçiş gerçekleştirmeniz gerekir.
- Yalnızca standart depolama hesabı türleri, dinamik geçiş desteği. Premium depolama hesapları el ile geçirilmesi gerekir.

Dinamik geçiş aracılığıyla isteyebilir [Azure destek portalı](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). Portalda, ZRS için dönüştürmek istediğiniz depolama hesabını seçin.
1. Seçin **yeni destek isteği**
2. Tamamlamak **Temelleri** hesap bilgilerinizi temel. İçinde **hizmet** bölümünden **depolama hesabı Yönetim** ve için ZRS dönüştürmek istediğiniz kaynak. 
3. **İleri**’yi seçin. 
4. Aşağıdaki değerleri belirtin **sorun** bölümü: 
    - **Önem derecesi**: Varsayılan değeri bırakın-olduğu.
    - **Sorun türü**: Seçin **veri geçişi**.
    - **Kategori**: Seçin **ZRS bir bölge içinde geçiş**.
    - **Başlık**: Örneğin, açıklayıcı bir başlık yazın **ZRS hesap geçiş**.
    - **Ayrıntılar**: Ek ayrıntılar yazın **ayrıntıları** kutusunda, örneğin, istediğim [LRS'den, GRS] için ZRS geçirmeyi de \_ \_ bölge. 
5. **İleri**’yi seçin.
6. Kişi bilgilerini doğru olduğundan emin olun **iletişim bilgilerini** dikey penceresi.
7. **Oluştur**’u seçin.

Destek ekibiyle sizinle iletişim kurmak ve ihtiyacınız olan tüm Yardım sağlar. 

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>ZRS Klasik: Blok blobları yedeklilik için eski bir seçeneği
> [!NOTE]
> Microsoft, kullanımdan ve 31 Mart 2021 üzerinde ZRS Klasik hesapları geçirme. ZRS Klasik müşterilere kullanımdan önce daha fazla ayrıntı sağlanacaktır. 
>
> ZRS sunulduktan sonra [sunuldu](#support-coverage-and-regional-availability) bir bölgede müşteriler ZRS Klasik hesapları bu bölgede portalından oluşturmak mümkün olmayacaktır. ZRS Klasik kullanım dışı kadar ZRS Klasik hesapları oluşturmak için Microsoft PowerShell ve Azure CLI kullanarak bir seçenektir.

ZRS Klasik zaman uyumsuz olarak verileri bir veya iki bölgedeki veri merkezleri arasında çoğaltır. Microsoft yük devretme ikincil başlatır sürece, çoğaltılan veriler kullanılamıyor olabilir. ZRS Klasik hesabı, LRS, GRS veya RA-GRS gelen veya dönüştürülemez. ZRS Klasik hesapları, ölçüm veya günlük da desteklemez.

ZRS Klasik, kullanılabilir yalnızca **blok blobları** , genel amaçlı V1 (GPv1) depolama hesapları. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](storage-account-overview.md).

El ile ya da bir LRS, ZRS Klasik, GRS veya RA-GRS hesabı'ndan ZRS hesap verileri geçirmek için aşağıdaki araçlardan birini kullanın: AzCopy, Azure Depolama Gezgini, Azure PowerShell veya Azure CLI. Ayrıca, kendi geçiş çözümüyle Azure depolama istemci kitaplıklarından birini de oluşturabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.
- [Azure Depolama çoğaltması](storage-redundancy.md)
- [Yerel olarak yedekli depolama (LRS): Azure depolama için düşük maliyetli veri yedekliği](storage-redundancy-lrs.md)
- [Coğrafi olarak yedekli depolama (GRS): Azure depolama için bölgeler arası çoğaltma](storage-redundancy-grs.md)
