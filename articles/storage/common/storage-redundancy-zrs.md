---
title: Bölgesel olarak yedekli depolama (ZRS) Azure depolama yüksek oranda kullanılabilir uygulamalar oluşturun | Microsoft Docs
description: Bölgesel olarak yedekli depolama (ZRS), yüksek kullanılabilirliğe sahip uygulamalar oluşturmak için basit bir yol sunar. ZRS, veri merkezindeki donanım arızalarına karşı ve bazı bölgesel felaketlere karşı korur.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/24/2018
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 111167584fb2e0e2ee5977e0e24b3ebf07b170c1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66237999"
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
- Birleşik Krallık Güney
- ABD Orta
- ABD Doğu
- ABD Doğu 2
- ABD Batı 2

Microsoft, ek Azure bölgelerinde ZRS etkinleştirmek devam ediyor. Denetleme [Azure hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/) düzenli olarak yeni bölgeler hakkında daha fazla bilgi için.

## <a name="what-happens-when-a-zone-becomes-unavailable"></a>Bir bölge kullanılamaz duruma geldiğinde ne olur?
Verilerinizi hem de okuma ve yazma işlemleri bile bir bölge kullanılamaz hale gelmesi için hala erişilebilir. Microsoft, geçici hata işlemeye yönelik uyarsanız devam etmesini önerir. Bu yöntemler, üstel geri alma ile yeniden deneme ilkelerini uygulayan içerir.

Bir bölge kullanılamadığında, Azure DNS repointing gibi ağ güncelleştirmeleri üstlendiği. Güncelleştirmeleri tamamlanmadan verilerinizi sağlıyorsanız bu güncelleştirmeler, uygulamanızın etkileyebilir.

ZRS, verilerinizin nerede birden çok bölge kalıcı olarak etkilenen bölgesel bir olağanüstü durum karşı koruma. Bunun yerine, geçici olarak kullanılamaz hale gelirse ZRS, verilerinizin dayanıklılık sunar. Bölgesel felaketlere karşı koruma için coğrafi olarak yedekli depolama (GRS) kullanarak Microsoft önerir. GRS hakkında daha fazla bilgi için bkz: [coğrafi olarak yedekli depolama (GRS): Azure depolama için bölgeler arası çoğaltma](storage-redundancy-grs.md).

## <a name="converting-to-zrs-replication"></a>ZRS çoğaltmalı dönüştürme
LRS, GRS ve RA-GRS gelen veya geçirme adımları oldukça kolaydır. Azure portalı veya depolama kaynak sağlayıcısı API'si, hesabınızın yedeklilik türünü değiştirmek için kullanın. Azure, verilerinizi daha sonra uygun şekilde çoğaltır. 

ZRS veri geçiriyorsanız, farklı bir strateji gerektirir. ZRS geçişi, bir bölgede birden fazla damga tek bir depolama damga verileri fiziksel hareketini içerir.

Geçiş için ZRS iki birincil seçenek vardır: 

- Elle kopyalamanız veya veri, var olan bir hesabı yeni bir ZRS hesabına taşıyın.
- Dinamik geçiş isteyin.

Microsoft, el ile geçiş gerçekleştirmek kesinlikle önerir. El ile geçiş, dinamik geçiş göre daha fazla esneklik sağlar. El ile geçiş ile zamanlama, Denetim sizdedir.

El ile geçiş gerçekleştirmek için seçeneğiniz vardır:
- AzCopy, Azure Storage istemcisi kitaplıklarını veya güvenilir bir üçüncü taraf araçları gibi araçları kullanın.
- Hadoop veya HDInsight ile sahibiyseniz eklemek hem kaynak hem de hedef (ZRS) hesabı kümenize. Ardından, veri kopyalama işlemini DistCp gibi bir araç ile paralel hale getirmek.
- Azure depolama istemci kitaplıklarından birini kullanarak kendi araçları oluşturun.

El ile geçiş uygulama kapalı kalma süresi neden olabilir. Uygulamanıza yüksek kullanılabilirlik gerekiyorsa, Microsoft, ayrıca bir dinamik geçiş seçeneği sunar. Dinamik geçiş, bir yerinde geçiş olur. 

Verilerinizi, kaynak ve hedef depolama Damgalar arasında geçişi sırasında dinamik geçiş sırasında depolama hesabınızı kullanabilirsiniz. Geçiş işlemi sırasında normal aynı düzeyde dayanıklılık ve kullanılabilirlik SLA'sı yazarken sahip.

Dinamik geçiş üzerine aşağıdaki kısıtlamaları göz önünde bulundurun:

- Microsoft kısa süre içinde dinamik geçiş için isteğinizi işlerken, dinamik geçiş tamamlandığında dair bir garanti yoktur. Gerekirse verilerinizi ZRS için belirli bir tarihe kadar geçişi ve ardından Microsoft yerine el ile geçiş gerçekleştirmek önerir. Genellikle, hesabınızda bir daha fazla veriniz, verileri geçirmek için uzar. 
- Dinamik geçiş kullanan çoğaltma LRS veya GRS depolama hesapları için desteklenir. RA-GRS hesabınızın kullanıyorsa, ilk LRS veya GRS hesabınızın çoğaltma türü bu devam etmeden önce değiştirmeniz gerekir. Aracı bu adım, geçiş işleminden önce RA-GRS tarafından sağlanan ikincil salt okunur uç kaldırır.
- Hesabınıza veri içermesi gerekir.
- Yalnızca aynı bölgede verilerin geçirebilirsiniz. Veri kaynağı hesaptan farklı bir bölgede bulunan bir ZRS hesabına geçirmek istiyorsanız, el ile geçiş gerçekleştirmeniz gerekir.
- Yalnızca standart depolama hesabı türleri, dinamik geçiş desteği. Premium depolama hesapları el ile geçirilmesi gerekir.
- LRS, GRS veya RA-GRS ZRS Canlı geçiş desteklenmiyor. Yeni veya mevcut bir depolama hesabı için verileri el ile taşımanız gerekir.
- Yönetilen diskler yalnızca LRS için kullanılabilir ve ZRS için geçirilemez. Bkz: tümleştirme ile kullanılabilirlik kümeleri için [giriş Azure yönetilen diskler](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview#integration-with-availability-sets). Anlık görüntülerini ve görüntülerini standart SSD yönetilen diskler için standart HDD depolama alanında depolayabilirsiniz ve [LRS hem de ZRS seçenekler arasından seçim yapın](https://azure.microsoft.com/pricing/details/managed-disks/). 

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

## <a name="live-migration-to-zrs-faq"></a>Dinamik geçiş ZRS hakkında SSS

**Geçiş sırasında kapalı kalma süresi için planlama yapmalıyım?**

Geçiş tarafından nedeniyle kapalı kalma süresi yoktur. Bir dinamik geçiş sırasında verilerinizi, kaynak ve hedef depolama Damgalar arasında geçişi sırasında depolama hesabınızı kullanmaya devam edebilirsiniz. Geçiş işlemi sırasında normal aynı düzeyde dayanıklılık ve kullanılabilirlik SLA'sı yazarken sahip.

**Geçişle ilişkilendirilmiş veri kaybını var mı?**

Geçiş ile ilişkili bir veri kaybı olmadan yoktur. Geçiş işlemi sırasında normal aynı düzeyde dayanıklılık ve kullanılabilirlik SLA'sı yazarken sahip.

**Geçişi tamamlandıktan sonra uygulamaları için gerekli tüm güncelleştirmeleri misiniz?**

Geçişi tamamlandıktan sonra hesapları çoğaltma türünü "bölgesel olarak yedekli depolama (ZRS)" olarak değişecektir. Hizmet uç noktaları, erişim anahtarları, SAS ve başka bir hesap yapılandırma seçenekleri değişmeden ve değişmeden kalır.

**My genel amaçlı v1 hesapları ZRS için dinamik geçişini isteyebilir miyim?**

Göndermeden önce ZRS için dinamik geçiş için istekte bulunmak için genel amaçlı v2'ye hesaplarınıza yükselttiğinizden emin ZRS yalnızca genel amaçlı v2 hesaplarını destekler. Bkz: [Azure depolama hesabına genel bakış](https://docs.microsoft.com/azure/storage/common/storage-account-overview) ve [yükseltmek için bir genel amaçlı v2 depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) daha fazla ayrıntı için.

**My okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) hesapları ZRS için dinamik geçişini isteyebilir miyim?**

Göndermeden önce ZRS için dinamik geçiş için bir istek, uygulamaları veya workload(s) artık ikincil salt okunur uç noktasına erişmesi ve coğrafi olarak yedekli depolama (GRS) depolama hesabınızda veya hesaplarınızda çoğaltma türünü değiştirme emin olun. Bkz: [çoğaltma stratejinizi değiştirme](https://docs.microsoft.com/azure/storage/common/storage-redundancy#changing-replication-strategy) daha fazla ayrıntı için.

**Benim için ZRS depolama hesapları başka bir bölgeye dinamik geçişini isteyebilir miyim?**

Ardından, verileri kaynak hesabı bölgeden farklı bir bölgede bulunan bir ZRS hesabına geçirmek istiyorsanız, el ile geçiş gerçekleştirmeniz gerekir.

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>ZRS Klasik: Blok blobları yedeklilik için eski bir seçeneği
> [!NOTE]
> Microsoft, kullanımdan ve 31 Mart 2021 üzerinde ZRS Klasik hesapları geçirme. ZRS Klasik müşterilere kullanımdan önce daha fazla ayrıntı sağlanacaktır. 
>
> ZRS sunulduktan sonra [sunuldu](#support-coverage-and-regional-availability) bir bölgede müşteriler ZRS Klasik hesapları bu bölgede portalından oluşturmak mümkün olmayacaktır. ZRS Klasik kullanım dışı kadar ZRS Klasik hesapları oluşturmak için Microsoft PowerShell ve Azure CLI kullanarak bir seçenektir.

ZRS Klasik zaman uyumsuz olarak verileri bir veya iki bölgedeki veri merkezleri arasında çoğaltır. Microsoft yük devretme ikincil başlatır sürece, çoğaltılan veriler kullanılamıyor olabilir. ZRS Klasik hesabı, LRS, GRS veya RA-GRS gelen veya dönüştürülemez. ZRS Klasik hesapları, ölçüm veya günlük da desteklemez.

ZRS Klasik, kullanılabilir yalnızca **blok blobları** , genel amaçlı V1 (GPv1) depolama hesapları. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](storage-account-overview.md).

El ile ya da bir LRS, ZRS Klasik, GRS veya RA-GRS hesabı'ndan ZRS hesap verileri geçirmek için aşağıdaki araçlardan birini kullanın: AzCopy, Azure Depolama Gezgini, Azure PowerShell veya Azure CLI. Ayrıca, kendi geçiş çözümüyle Azure depolama istemci kitaplıklarından birini de oluşturabilirsiniz.

ZRS Portal ya da kullanarak Azure PowerShell veya Azure CLI'yı ZRS kullanılabildiği bölgelerdeki, ZRS Klasik hesapları da yükseltebilirsiniz.

Yükseltmek için portalda ZRS hesap yapılandırması bölümüne gidin ve Yükselt'i seçin:![ZRS Klasik Portalı'nda ZRS yükseltme](media/storage-redundancy-zrs/portal-zrs-classic-upgrade.jpg)

PowerShell kullanarak ZRS için yükseltmek için aşağıdaki komutu arayın:
```powershell
Set-AzStorageAccount -ResourceGroupName <resource_group> -AccountName <storage_account> -UpgradeToStorageV2
```

CLI kullanarak ZRS için yükseltmek için aşağıdaki komutu arayın:
```cli
az storage account update -g <resource_group> -n <storage_account> --set kind=StorageV2
```

## <a name="see-also"></a>Ayrıca bkz.
- [Azure Depolama çoğaltması](storage-redundancy.md)
- [Yerel olarak yedekli depolama (LRS): Azure depolama için düşük maliyetli veri yedekliği](storage-redundancy-lrs.md)
- [Coğrafi olarak yedekli depolama (GRS): Azure depolama için bölgeler arası çoğaltma](storage-redundancy-grs.md)
