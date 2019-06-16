---
title: Olağanüstü durum kurtarma ve depolama hesabı yük devretme (Önizleme) - Azure depolama
description: Azure depolama hesabı yük devretme (Önizleme), coğrafi olarak yedekli depolama hesapları için destekler. Hesap yük devretme ile birincil uç noktaya kullanılamaz duruma gelirse, depolama hesabınız için yük devretme işlemini başlatabilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/25/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: f9d68af12f6b2e98c77d0bd1b65a82c69588f203
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65147626"
---
# <a name="disaster-recovery-and-storage-account-failover-preview-in-azure-storage"></a>Olağanüstü durum kurtarma ve depolama hesabı yük devretme (Önizleme) Azure Depolama'daki

Microsoft Azure hizmetlerini her zaman kullanılabilir olmasını sağlamak çalışır. Ancak, hizmet planlanmamış kesintiler meydana gelebilir. Uygulamanıza esneklik gerekiyorsa, böylece verilerinizi ikinci bir bölgeye çoğaltılır Microsoft coğrafi olarak yedekli depolama kullanmanızı önerir. Ayrıca, müşteriler bölgesel hizmet kesintisi işlemek için yerinde planlama bir olağanüstü durum kurtarma olması gerekir. Birincil uç noktaya kullanılamaz hale gelmesi durumunda, ikincil uç noktaya yük devretmek önemli bir olağanüstü durum kurtarma planının parçası hazırlanıyor. 

Azure depolama hesabı yük devretme (Önizleme), coğrafi olarak yedekli depolama hesapları için destekler. Hesap yük devretme ile birincil uç noktaya kullanılamaz duruma gelirse, depolama hesabınız için yük devretme işlemini başlatabilirsiniz. Depolama hesabınızın birincil uç nokta olacak ikincil uç nokta yük devretme güncelleştirir. Yük devretme işlemi tamamlandıktan sonra istemciler yeni birincil uç nokta yazma başlayabilirsiniz.

Bu makalede, kavramlar açıklanır ve işlem bir hesabı yük devretme ile ilgili ve müşteri etkisi en az miktarda ile kurtarma için depolama hesabınızın hazırlama anlatılmaktadır. Azure portal veya PowerShell içinde bir hesap yük devretme başlatma hakkında bilgi edinmek için bkz: [hesabı yük devretme (Önizleme) başlatın](storage-initiate-account-failover.md).


[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="choose-the-right-redundancy-option"></a>Doğru olarak yedeklilik seçeneği seçin

Tüm depolama hesaplarında yedeklilik için çoğaltılır. Hesabınız için hangi yedeklilik seçeneği, gereksinim duyduğunuz dayanıklılık derecesine üzerinde bağlıdır. Bölgesel kesintiler karşı koruma için coğrafi olarak yedekli depolama ile veya olmadan ikincil bölgeden okuma erişimine seçeneği seçin:  

**Coğrafi olarak yedekli depolama (GRS)** verilerinizi en az yüzlerce mil uzaklıkta bulunan zaman uyumsuz olarak iki coğrafi bölgede çoğaltır. Birincil bölgede kesinti nedeniyle düşerse, yedekli bir kaynak için verilerinizi ikincil bölgeye görür. Birincil uç noktada ikincil uç dönüştürmek için bir yük devretme başlatabilirsiniz.

**Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)** coğrafi olarak yedekli depolama okuma erişimi ikincil uç ek avantajı sağlar. Birincil uç noktasında bir kesinti oluşursa, RA-GRS için yapılandırılmış ve yüksek kullanılabilirlik için tasarlanmış uygulamalar ikincil uç noktadan okumaya devam edebilirsiniz. Microsoft, uygulamalarınız için en yüksek dayanıklılık için RA-GRS önerir.

Verilerinizi tek bir bölgede kullanılabilirlik alanları genelinde çoğaltır bölgesel olarak yedekli depolama (ZRS) ve tek bir bölgedeki tek bir veri merkezi içinde verilerinizin yerel olarak yedekli depolama (LRS), diğer Azure depolama yedekliliği seçenekleri içerir. Depolama hesabınızı ZRS veya LRS için yapılandırılmışsa, GRS veya RA-GRS kullanmak için bu hesabı dönüştürebilirsiniz. Hesabınız için coğrafi olarak yedekli depolama yapılandırma ek maliyetler doğurur. Daha fazla bilgi için [Azure depolama çoğaltma](storage-redundancy.md).

> [!WARNING]
> Coğrafi olarak yedekli depolama, veri kaybı riskini taşır. Veriler birincil bölgesine yazılan veri ikincil bölge'ye zaman yazılır arasında bir gecikme olduğu anlamına ikincil bölge'ye zaman uyumsuz olarak çoğaltılır. Kesinti olması durumunda, ikincil uç noktaya henüz çoğaltılmamış birincil uç nokta işlemleri kaybolacak yazın. 

## <a name="design-for-high-availability"></a>Yüksek kullanılabilirlik için tasarlama

Yüksek kullanılabilirlik için uygulamanızı baştan tasarlayın önemlidir. Uygulamanızı tasarlama ve olağanüstü durum kurtarma için Planlama Kılavuzu için bu Azure kaynaklarına bakın:

* [Azure için dayanıklı uygulamalar tasarlama](https://docs.microsoft.com/azure/architecture/resiliency/): Azure'da yüksek oranda kullanılabilir uygulamalar tasarlamak için anahtar kavramlar genel bakış.
* [Kullanılabilirlik denetim listesi](https://docs.microsoft.com/azure/architecture/checklist/availability): Uygulamanızı yüksek kullanılabilirlik için en iyi tasarım yöntemleri uygulayan doğrulamak için bir denetim listesi.
* [RA-GRS'yi kullanarak yüksek kullanılabilirliğe sahip uygulamalar tasarlama](storage-designing-ha-apps-with-ragrs.md): RA-GRS yararlanmak için uygulamaları oluşturmaya yönelik rehberlik tasarlayın.
* [Öğretici: Blob Depolama ile yüksek oranda kullanılabilir bir uygulama derleme](../blobs/storage-create-geo-redundant-storage.md): Uç noktalar olarak hataları ve kurtarma arasında otomatik olarak geçer yüksek oranda kullanılabilir bir uygulamanın nasıl oluşturulacağını gösteren bir öğretici benzetimi. 

Ayrıca, Azure Storage verilerinizin yüksek kullanılabilirliğini sürdürmek için bu en iyi uygulamaları göz önünde bulundurun:

* **Diskler:** Kullanım [Azure Backup](https://azure.microsoft.com/services/backup/) , Azure sanal makineler tarafından kullanılan VM disklerini yedekleme için. Ayrıca kullanmayı [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) bölgesel bir olağanüstü durumda sanal makinelerinizi korumak için.
* **Blok BLOB'lar:** Açma [geçici silme](../blobs/storage-blob-soft-delete.md) nesne düzeyinde silme karşı korumak ve üzerine yazar veya blok BLOB'ları kullanarak farklı bölgede başka bir depolama hesabına kopyalayın [AzCopy](storage-use-azcopy.md), [Azure PowerShell ](storage-powershell-guide-full.md), veya [Azure veri taşıma Kitaplığı](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).
* **Dosyaları:** Kullanım [AzCopy](storage-use-azcopy.md) veya [Azure PowerShell](storage-powershell-guide-full.md) dosyalarınızı farklı bir bölgede başka bir depolama hesabına kopyalamak için.
* **Tablolar:** kullanın [AzCopy](storage-use-azcopy.md) tablo verilerini farklı bir bölgede başka bir depolama hesabına aktarın.

## <a name="track-outages"></a>Kesintiler izleyin

Müşteriler abone [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/) durumunu ve Azure depolama ve diğer Azure hizmetlerinin durumunu izlemek için.

Microsoft ayrıca uygulamanızı yazma hataları olasılığını için hazırlamak üzere tasarım önerir. Uygulamanızı yazma hatalarını birincil bölgede bir kesintisi olasılığını uyarır şekilde sunmalıdır.

## <a name="understand-the-account-failover-process"></a>Hesap yük devretme işlemini anlama

Müşteri tarafından yönetilen hesap yük devretme (Önizleme), birincil herhangi bir nedenle kullanılamaz duruma gelirse, tüm depolama hesabınızı ikincil bölgeye yük devretmek sağlar. İkincil bölgeye yük devretme zorlamak, istemcilerin yük devretme işlemi tamamlandıktan sonra verileri ikincil uç noktaya yazma başlayabilirsiniz. Yük devretme, genellikle yaklaşık bir saat sürer.

### <a name="how-an-account-failover-works"></a>Bir hesap yük devretme nasıl çalışır?

Normal koşullarda bir istemci birincil bölgede bir Azure depolama hesabına veri yazar ve bu verileri zaman uyumsuz olarak ikincil bölgeye çoğaltılır. Aşağıdaki görüntüde, birincil bölgenin kullanılabilir olduğunda senaryo gösterilmektedir:

![İstemciler, birincil bölgelerindeki depolama hesabı için veri yazma](media/storage-disaster-recovery-guidance/primary-available.png)

Birincil uç noktadan herhangi bir nedenle kullanılamaz duruma gelirse, istemci artık depolama hesabına yazma mümkün değildir. Aşağıdaki görüntüde olduğu birincil kullanılamıyor, ancak hiçbir kurtarma henüz gerçekleştirilmedi senaryo gösterilmektedir:

![İstemcilerin veri yazamaz için birincil kullanılamıyor](media/storage-disaster-recovery-guidance/primary-unavailable-before-failover.png)

Müşteri hesabı yük devretme ikincil uç başlatır. Yük devretme işlemi, depolama hesabınız için yeni birincil uç noktaya ikincil uç noktaya dönüşür aşağıdaki görüntüde gösterildiği gibi Azure Depolama tarafından sağlanan DNS girişini güncelleştirir:

![Müşteri hesabı yük devretme ikincil uç noktaya başlatır](media/storage-disaster-recovery-guidance/failover-to-secondary.png)

Yazma erişimi, GRS ve RA-GRS hesapları için DNS girişini güncelleştirildi ve yeni birincil uç noktaya istek yönlendirilmiş sonra geri yüklenir. Blobları, tablolar, kuyruklar ve dosyalar için var olan depolama hizmet uç noktası, yük devretme sonrasında aynı kalır.

> [!IMPORTANT]
> Yük devretme işlemi tamamlandıktan sonra depolama hesabı, yeni birincil uç nokta yerel olarak yedekli olacak şekilde yapılandırılır. Yeni bir ikincil çoğaltma sürdürmek için coğrafi olarak yedekli depolama tekrar (RA-GRS veya GRS) kullanmak üzere hesabı yapılandırın.
>
> Bir LRS hesabına GRS veya RA-GRS dönüştürme maliyeti doğurur göz önünde bulundurun. Bu maliyet, RA-GRS veya GRS yük devretmeden sonra kullanmak için yeni birincil bölgelerindeki depolama hesabı güncelleştirmek için geçerlidir.  

### <a name="anticipate-data-loss"></a>Veri kaybı tahmin

> [!CAUTION]
> Bir hesap yük devretme, genellikle bazı veri kaybını içerir. Bir hesap yük devretmeyi başlatmadan etkilerini anlamak önemlidir.  

Verileri, zaman uyumsuz olarak birincil bölgeden ikincil bölgeye yazıldığı için her zaman bir gecikme olur önce birincil bölgeye yazma ikincil bölgeye çoğaltılır. Birincil bölge kullanılamaz duruma gelirse, en son yazma henüz ikincil bölgeye çoğaltılmamış.

Bir yük devretmeye zorlamak, birincil bölgedeki tüm veriler kaybolur yeni birincil bölgeden ikincil bölgeye olur ve depolama hesabı, yerel olarak yedekli olacak şekilde yapılandırılır. Yük devretme gerçekleştiğinde ikincil önceden çoğaltılmış tüm verileri saklanır. Ancak, aynı zamanda ikincil veritabanına çoğaltılmamış birincil yazılan tüm veriler kalıcı olarak kaybolur. 

**Son eşitleme zamanı** özelliği gösterir en son ne zaman veri birincil bölgeden ikincil bölgeye yazılan sağlanır. Son eşitleme zamanından önce yazılan tüm veriler son eşitleme zamanı ikincil yazılmış olabilir değil ve kaybolabilir sonra yazılan veriler ikincil kullanılabilir. Bir hesap yük devretme başlatarak kaynaklanabilecek veri kaybı miktarı tahmin etmek için kesinti durumunda bu özelliği kullanın. 

Böylece son eşitleme zamanı beklenen veri kaybı değerlendirmek için kullanabileceğiniz en iyi uygulama, uygulamanızı tasarlayın. Örneğin, son yazma işlemlerinizin zaman son karşılaştırabilirsiniz sonra tüm yazma işlemleri, oturum açıyorsanız hangi yazma işlemleri için ikincil eşitlenmemiş belirlemek için zaman eşitleyin.

### <a name="use-caution-when-failing-back-to-the-original-primary"></a>Özgün birincil siteye başarısız kullanırken dikkatli olun

Birincil düğümden ikincil bölgeye yük devretme sonra depolama hesabınıza yeni bir birincil bölgede yerel olarak yedekli olacak şekilde yapılandırılır. GRS veya RA-GRS kullanacak şekilde güncelleştirerek, coğrafi olarak yedekli hesabı yeniden yapılandırabilirsiniz. Hesabı yeniden yük devretme sonrasında coğrafi olarak yedeklilik için yapılandırıldığında, özgün yük devretmeden önce birincil olan yeni ikincil bölgeye veri çoğaltma yeni birincil bölgeye hemen başlar. Ancak, bir süre önce birincil mevcut verileri yeni bir ikincil tam olarak çoğaltılır beklemeniz gerekebilir.

Depolama hesabı için coğrafi yedeklilik yapılandırılır sonra yeni ikincil yeni birincil arkasından başka bir yük devretmeyi başlatmak mümkündür. Bu durumda, özgün birincil bölgeye yük devretme öncesinde, birincil bölge tekrar olur ve yerel olarak yedekli olacak şekilde yapılandırılır. Tüm veriler yük devretme sonrasında birincil bölgede (özgün ikincil) sonra kaybolur. Depolama hesabındaki verilerin çoğunu çoğaltılmaz, yeni ikincil veritabanına geri devretmeden önce bir büyük veri kaybı yaşar. 

Büyük veri kaybını önlemek için değerini kontrol edin **son eşitleme zamanı** ilk duruma döndürmeden önce özelliği. Karşılaştırma verileri son kez son eşitleme zamanı beklenen veri kaybı değerlendirmek için yeni birincil veritabanına yazıldı. 

## <a name="initiate-an-account-failover"></a>Hesap yük devretmesi başlatma

Azure portalı, PowerShell, Azure CLI veya Azure depolama kaynak sağlayıcısı API'si bir hesabı yük başlatabilirsiniz. Bir yük devretme başlatma konusunda daha fazla bilgi için bkz. [hesabı yük devretme (Önizleme) başlatın](storage-initiate-account-failover.md).

## <a name="about-the-preview"></a>Önizleme hakkında

Hesap yük devretme, GRS veya RA-GRS ile Azure Resource Manager dağıtımlarını kullanan tüm müşteriler için önizleme modunda kullanılabilir. Genel amaçlı v1, genel amaçlı v2 ve Blob Depolama hesabı türleri desteklenir. Hesap yük devretme bu bölgede şu anda kullanılabilir:

- ABD Batı 2
- ABD Orta Batı

Önizleme yalnızca üretim dışı kullanması için tasarlanmıştır. Üretim hizmet düzeyi sözleşmeleri (SLA'lar) şu anda kullanılamıyor.

### <a name="register-for-the-preview"></a>Önizleme için kaydolun

Önizlemeye kaydolmak için PowerShell'de aşağıdaki komutları çalıştırın. Köşeli ayraçlar içindeki yer tutucusunu kendi abonelik kimliği ile değiştirdiğinizden emin olun:

```powershell
Connect-AzAccount -SubscriptionId <subscription-id>
Register-AzProviderFeature -FeatureName CustomerControlledFailover -ProviderNamespace Microsoft.Storage
```

Bu önizleme için onay almak için 1-2 gün sürebilir. Kaydınızı onaylandığını doğrulamak için aşağıdaki komutu çalıştırın:

```powershell
Get-AzProviderFeature -FeatureName CustomerControlledFailover -ProviderNamespace Microsoft.Storage
```

### <a name="additional-considerations"></a>Diğer konular 

Önizleme dönemi boyunca bir yük devretme zorunlu tutarsanız nasıl uygulamalarınızı ve hizmetlerinizi etkilenebilir anlamak için bu bölümdeki açıklanan ek konuları gözden geçirin.

#### <a name="azure-virtual-machines"></a>Azure sanal makineleri

Azure sanal makineleri (VM'ler) bir hesap yük devretme işleminin bir parçası olarak yapmıyor. Birincil bölge kullanılamaz duruma gelir ve ikincil bölgeye yük devretme, yük devretme sonrasında herhangi bir VM oluşturmanız gerekir. 

#### <a name="azure-unmanaged-disks"></a>Azure yönetilmeyen diskler

En iyi uygulama, Microsoft, yönetilmeyen diskleri yönetilen disklere dönüştürme önerir. Ancak, Azure Vm'lere bağlı yönetilmeyen diskler içeren bir hesap yük devretme gerekiyorsa, yük devretmeyi başlatmadan önce sanal makineyi gerekecektir.

Yönetilmeyen diskleri sayfa blobları Azure Depolama'da depolanır. Azure'da bir VM çalışırken VM'ye bağlı herhangi bir yönetilmeyen diskler kiralanır. Blob üzerinde kiralama olduğunda bir hesabı yük devretme devam edemiyor. Yük devretme gerçekleştirmek için şu adımları izleyin:

1. Başlamadan önce herhangi bir yönetilmeyen diskler, mantıksal birim numaralarına (LUN) ve bağlı VM adlarını not edin. Bunun yapılması, diskleri, yük devretmeden sonra VM'ye yeniden kolaylaştırır. 
2. VM'yi kapatın.
3. VM'yi silin, ancak yönetilmeyen diskler için VHD dosyalarını korur. VM sildiğiniz zaman unutmayın.
4. Bekle **son eşitleme zamanı** güncelleştirdi ve VM sildiğiniz saatten sonra. Bu adım önemlidir, çünkü yük devretme gerçekleştiğinde ikincil uç tamamen VHD dosyaları ile değil güncelleştirildiyse, ardından sanal Makineyi yeni bir birincil bölgede düzgün çalışmayabilir.
5. Hesap yük devretmeyi başlatın.
6. Hesap yük devretme işlemi tamamlandıktan ve yeni birincil bölgeden ikincil bölgeye haline gelmiştir kadar bekleyin.
7. Yeni bir birincil bölgede bir VM oluşturun ve VHD'leri yeniden bağlayın.
8. Yeni VM'yi başlatın.

VM kapatıldığında geçici bir diskte depolanan tüm veriler kaybolur aklınızda bulundurun.

### <a name="unsupported-features-or-services"></a>Desteklenmeyen özellikleri veya hizmetleri
Aşağıdaki özellikler veya hizmetleri Önizleme sürümü için hesap yük devretme için desteklenmez:

- Azure dosya eşitleme, depolama hesabı yük devretme işlemini desteklemiyor. Azure dosya eşitleme bulut uç olarak kullanılan Azure dosya paylaşımları içeren depolama hesaplarının yük devretmesi değil. Bunun yapılması ayrıca çalışma ve Mayıs durdurmak için neden Eşitleme beklenmeyen veri kaybı durumunda yeni katmanlı dosyalar neden olur.  
- Azure Data Lake depolama Gen2 hiyerarşik ad alanı'nı kullanarak depolama hesaplarında yük devredilemez.
- Arşivlenen bloblarını içeren depolama hesabı üzerinden devredilemez. Yük devretme planlamadığınız ayrı bir depolama hesabı arşivlenmiş blob'larda korur.
- Premium blok bloblarını içeren depolama hesabı üzerinden devredilemez. Coğrafi yedeklilik premium blok blobları destekleyen depolama hesapları şu anda desteklemez.
- Yük devretme işlemi tamamlandıktan sonra aşağıdaki özellikleri özgün olarak etkinleştirilirse çalışmayı durdurur: [Olay abonelikleri](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-overview), [yaşam döngüsü ilkeleri](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts), [depolama analizi günlük](https://docs.microsoft.com/rest/api/storageservices/about-storage-analytics-logging).

## <a name="copying-data-as-an-alternative-to-failover"></a>Yük devretme için alternatif olarak veri kopyalama

Ardından, depolama hesabı RA-GRS için yapılandırılmışsa, ikincil uç noktayı kullanarak verilerinize okuma erişimine sahip. Birincil bölgede bir kesinti durumunda yük devretme değil tercih ederseniz, gibi araçları kullanın [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md), veya [Azure veri taşıma Kitaplığı](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) için İkincil bölgede depolama hesabınızdan veri etkilenmeyen bir bölgedeki başka bir depolama hesabına kopyalayın. Uygulamalarınızı bu depolama hesabına işaret için hem okuma ve yazma kullanılabilirliğini.

## <a name="microsoft-managed-failover"></a>Microsoft tarafından yönetilen bir yük devretme

Bir bölge önemli bir olağanüstü durum nedeniyle kayıp olduğu olağanüstü durumlarda, Microsoft bölgesel yük devretme başlatabilir. Bu durumda, sizin herhangi bir eylemi gerekli değildir. Microsoft tarafından yönetilen bir yük devretme gerçekleştirilene kadar depolama hesabınıza yazma erişimine sahip olmaz. Depolama hesabı RA-GRS için yapılandırılmışsa, uygulamalarınızı ikincil bölgeden okuyabilirsiniz. 

## <a name="see-also"></a>Ayrıca bkz.

* [Bir hesap yük devretme (Önizleme) başlatın](storage-initiate-account-failover.md)
* [RA-GRS’yi kullanarak yüksek kullanılabilirliğe sahip uygulamalar tasarlama](storage-designing-ha-apps-with-ragrs.md)
* [Öğretici: Blob Depolama ile yüksek oranda kullanılabilir bir uygulama oluşturun](../blobs/storage-create-geo-redundant-storage.md) 
