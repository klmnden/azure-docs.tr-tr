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
ms.openlocfilehash: 0347d6ca951b75c385b138487f58b85a809c6805
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34076672"
---
Bölge olarak yedekli depolama (ZRS) verilerinizi tek bir bölgede depolama kümeleri üç (3) arasında zaman uyumlu olarak çoğaltır. Her Depolama kümesi diğerlerinden fiziksel olarak ayrılır ve kendi kullanılabilirlik bölgesinde (AZ) bulunur. Her kullanılabilirlik bölge, ZRS küme, içinde ise ayrı yardımcı programları ve ağ özellikleri otonom.

ZRS hesabında verilerinizi depolamak, mümkün erişimi olması ve bir bölge kullanılamaz hale gelmesi durumunda, verilerinizi yöneten sağlar. ZRS, mükemmel performans ve son derece düşük gecikme sağlar. Aslında, ZRS aynı olan [ölçeklenebilirlik hedefleri](../articles/storage/common/storage-scalability-targets.md) LRS olarak.

ZRS bir kesinti veya doğal afet zonal veri merkezi kullanılabilir işler olsa bile, güçlü tutarlılık, güçlü dayanıklılık ve yüksek kullanılabilirlik gerektiren senaryolar için göz önünde bulundurun. ZRS depolama nesneler en az %99.9999999999 için dayanıklılık sunar (12 9'in) belirli bir yılın üstünde.

Kullanılabilirlik bölgeler hakkında daha fazla bilgi için bkz: [kullanılabilirlik bölgeleri genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview).

## <a name="support-coverage-and-regional-availability"></a>Destek kapsamı ve bölgesel kullanılabilirliği
ZRS şu anda destekler [ **standart, genel amaçlı v2 (GPv2)** ](../articles/storage/common/storage-account-options.md#general-purpose-v2) hesap türleri. ZRS, blok blobları, disk olmayan sayfa BLOB'ları, dosyaları, tablolar ve Kuyruklar için kullanılabilir. Ayrıca, tüm, [depolama çözümlemeleri](../articles/storage/common/storage-analytics.md) günlükleri ve [Storage ölçümleri](../articles/storage/common/storage-enable-and-view-metrics.md)

ZRS olan **genel olarak kullanılabilir** aşağıdaki bölgelerde:

- ABD Doğu 2
- ABD Orta
- Kuzey Avrupa
- Batı Avrupa
- Fransa Orta
- Güneydoğu Asya

Microsoft Azure diğer bölgelerdeki ZRS etkinleştirmek devam eder ve oluştuğunda bu listesini güncelleştirir. Ayrıca standart kanalları üzerinden böyle duyuruları gibi yapacağız [Azure hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/) Azure abonelik sahipleri ve yöneticileri sayfası ve e-posta bildirimleri.

## <a name="what-happens-when-a-zone-becomes-unavailable"></a>Bir bölge kullanılamaz duruma geldiğinde ne olur?

Verilerinizi bir bölge kullanılamaz hale gelirse dayanıklı kalır. Microsoft, geçici hata işleme, yeniden deneme ilkelerini üstel geri alma ile uygulama gibi için uygulamaları izlemek devam önerir. Bir bölge kullanılamadığında Azure DNS repointing gibi ağ güncelleştirmeleri undertakes. Bunlar tamamladınız önce verilerinizi erişiyorsanız bu güncelleştirmeler, uygulamanızın etkileyebilir.

ZRS, verilerinizin nerede birden fazla bölge kalıcı olarak etkilenen bölgesel bir olağanüstü durum karşı koruma sağlamaz. Bunun yerine, ZRS geçici olarak kullanım dışı kalması durumunda, verileriniz için esneklik sunar. Bölgesel afetler karşı koruma için Microsoft önerir kullanarak [coğrafi olarak yedekli depolama (GRS): Azure Storage için çapraz bölge çoğaltma](../articles/storage/common/storage-redundancy-grs.md).

## <a name="converting-to-zrs-replication"></a>ZRS çoğaltma dönüştürme
Bugün, LRS, GRS ve RA-GRS arasında değiştirmek oldukça basittir - portalı veya API yararlanın. Bir bölge içinde birden çok Damgalar'tek bir depolama damgası fiziksel veri taşımayı içerdiğinden ZRS ile ancak olarak açık değil. Bu nedenle, birincil için iki seçeneğiniz vardır - el ile kopyalama/verileri yeni bir ZRS hesabına varolan hesabınızdan taşıma veya bir dinamik geçiş isteyin. Dinamik geçişi tamamladıktan sonra garanti alamıyoruz olduğundan el ile geçiş gerçekleştirmek öneririz; doğrudan ve dolaylı olarak bir geçiş işi tamamlandığında etkileyen pek çok etken vardır. 

El ile geçiş gerçekleştirmek için birçok seçeneğiniz vardır:
- AzCopy, depolama SDK'sı, güvenilir üçüncü taraf araçları, vb. gibi varolan araçları kullanın.
- Hadoop veya Hdınsight ile bilginiz varsa, her iki kaynak ekleyebilirsiniz ve hedef (ZRS) kümenize hesap ve Distcp'yi gibi yüksek düzeyde veri kopyalama işlemi paralel hale kullanın
- Bir depolama SDK'sı örneğinizin yararlanarak kendi araç derleme

Daha önce belirtildiği gibi bir dinamik geçiş yapar daha fazla esneklik verdiğinden el ile geçiş rota Git öneririz. Ayrıca geçiş gerçekleştiğinde, toplam denetiminde var.

Ancak, el ile geçiş bazı uygulama kapalı kalma sürelerine neden ve end öğesinde almak amacıyla yapamıyorsunuz varsa, size bir dinamik geçiş seçeneği sağlar. Dinamik geçiş, verilerinizi kaynak ve hedef depolama Damgalar arasında geçişi sırasında var olan depolama hesabınızı kullanmaya devam etmek izin veren bir yerinde geçiş değil. Geçiş sırasında normalde yaptığınız gibi aynı düzeyde dayanıklılık ve kullanılabilirlik SLA hala gerekir.

Dinamik geçiş, ancak bazı kısıtlamalar ile geliyor. Aşağıda listelenmiştir.

- Dinamik geçiş isteğiniz derhal ele alınacaktır olsa da, geçiş gerçekten olur, biz garanti edemez tamamlayın. Verilerinizi tarafından belirli bir süre içinde ZRS olması gerekiyorsa, el ile geçiş yapmanız gerekir. Genellikle, daha fazla veri hesabınızda, sahip uzun, bu verileri geçirmek için zaman alır. 
- LRS ve GRS çoğaltma sahip bir hesaba yalnızca canlı geçirebilirsiniz. RA-GRS varsa, devam etmeden önce bu çoğaltma türlerinden birine ilk değiştirmeniz gerekir. Bu ara adım sağlar, RA-GRS olan sağlayan ikincil salt okunur uç ne zaman kaldırılan hazırsınız.
- Hesabınızın boş olması gerekir.
- Yalnızca içi bölge geçişler desteklenir. Kaynak hesaptan farklı bir bölgede bulunan bir ZRS hesabı, verileri geçirmek istiyorsanız, el ile geçiş gerçekleştirmeniz gerekir.
- Yalnızca standart depolama hesabı türleri için. Premium depolama hesabından geçiremezsiniz.

Dinamik geçiş istekleri Azure destek portalına gidin. Portal, ZRS için dönüştürmek istediğiniz depolama hesabını seçin.
1. Tıklatın **yeni destek isteği**
2. Temel bilgileri doğrulayın. **İleri**’ye tıklayın. 
3. Üzerinde **sorun** bölümünde, 
    - Önem derecesine sahip olarak bırakın-değil.
    - Sorun türü = **veri geçişi**
    - Kategori = **ZRS bir bölge içinde geçiş**
    - Başlık = **ZRS hesap geçiş** (veya bir şey açıklayıcı)
    - Ayrıntılar = [LRS, GRS] ZRS için geçiş ___ bölgede istiyorum. 
4. **İleri**’ye tıklayın.
5. Kişi bilgileri kişi bilgileri dikey penceresinde doğru olduğundan emin olun.
6. Tıklatın **gönderme**.

Bir Destek ekibiyle iletişim kurmayan, ardından olacaktır. Bu kişi gerektirebilecek Yardım sağlamak kullanılabilir. 

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>ZRS Klasik: Eski bir seçeneği blok blobları artıklığı
> [!NOTE]
> ZRS Klasik hesaplarını kullanımdan kaldırma ve 31 Mart 2021 üzerinde gerekli geçiş için planlanmıştır. Microsoft, kullanımdan önce ZRS Klasik müşteriler için daha fazla bilgi gönderir. Otomatik geçiş işlemi ZRS Klasik ZRS için gelecekte sağlamak Microsoft planlar.

>[!NOTE]
> ZRS olduğunda [genel olarak kullanılabilir](#support-coverage-and-regional-availability) bir bölgede, artık aynı bölgede portalından ZRS klasik bir hesap oluşturmak mümkün olmayacaktır. ZRS Klasik kullanım dışıdır kadar ancak hala arasında başka yollarla Microsoft PowerShell ve Azure CLI gibi diğer bir deyişle, oluşturmayı seçebilirsiniz.

ZRS Klasik zaman uyumsuz olarak veri bir veya iki bölgeleri içindeki veri merkezleri arasında çoğaltılır. Çoğaltma, Microsoft ikincil birime yük devretme işlemini başlatana kadar kullanılamayabilir. ZRS Klasik yüklenebilir yalnızca **blok blobları** içinde [genel amaçlı V1 (GPv1)](../articles/storage/common/storage-account-options.md#general-purpose-v1) depolama hesapları. ZRS Klasik hesabı, LRS veya GRS’ye iki yönlü olarak dönüştürülemez ve ölçüm veya günlüğe kaydetme özelliklerine sahip değildir.

ZRS Klasik hesapları için veya LRS, GRS veya RA-GRS dönüştürülemez. ZRS Klasik hesapları, ölçümleri veya günlük kaydı da desteklemez.

El ile ya da bir LRS, ZRS Klasik, GRS veya RA-GRS hesabı'ndan ZRS hesap verilerini geçirmek için AzCopy, Azure Storage Gezgini, Azure PowerShell veya Azure CLI kullanın. Kendi geçiş çözümünüzle birlikte Azure Storage istemci kitaplıklarından birini de oluşturabilirsiniz.
