---
title: En iyi yöntemler için maliyeti ve iş yüklerini boyutlandırma için Azure geçişi | Microsoft Docs
description: Boyutlandırma iş yüklerini Azure'a geçişi ve maliyeti için en iyi alın.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/08/2018
ms.author: raynew
ms.openlocfilehash: 48ce99bd830d2c35e5cb9703d2ef754a602d534b
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64926139"
---
# <a name="best-practices-for-costing-and-sizing-workloads-migrated-to-azure"></a>Maliyet ve boyutlandırma iş yükleri için en iyi uygulamaları için Azure geçişi

Düşünüyorsanız ve geçiş için tasarım maliyetlerinden odaklanarak Azure geçişinizin uzun süreli başarının sağlar. Geçiş projesi sırasında tüm takımlar kritik (Finans, yönetimi, uygulama ekiplerinin vb.) ilişkili maliyetleri anlama.
- Geçiş işleminden önce geçişinizi tahmin harcaması, bir taban için aylık, üç aylık, ve yıllık bütçe hedefleri başarısı için çok önemli.
- Geçişten sonra maliyetleri uygun duruma getirmek, iş yüklerinin sürekli olarak izlemeniz ve gelecekteki kullanım modellerini planlayın. Geçirilen kaynakların tek bir iş yükü türü olarak başlar ancak başka bir türe kullanımı, maliyetleri ve kaydırma iş gereksinimlerine göre zamanla evrim olabilir.

Bu makalede, önce ve boyutlandırma ve maliyet geçişten sonra en iyi uygulamaları açıklar.  

> [!IMPORTANT]
> En iyi yöntemler ve bu makalede açıklanan fikirlerini Azure platformunda yer alan ve özelliklerin makalenin yazıldığı sırada hizmet. Özellikler ve yetenekler zamanla değişir. Tüm öneriler dağıtımınız için uygun, sizin için uygun şekilde seçin.


## <a name="before-migration"></a>Geçiş işleminden önce 

İş yüklerinizi buluta taşımadan önce bunları Azure'da çalışan aylık maliyetini tahmin etmek. Bulut maliyetlerinizi proaktif bir şekilde yönetme, işletim masrafları (OpEx) bütçenize uygun yardımcı olur. Bütçe sınırlı ise, bu geçiş işleminden önce dikkate alın.  Azure sunucusuz teknolojilerini iş yüklerini dönüştürme, maliyetleri azaltmak uygun bir yerlerde düşünün.

Bu bölümde en iyi kullanımı tahmini maliyetinizi hesaplayın, VM ve depolama için doğru boyutlandırma gerçekleştirmek, Azure hibrit avantajları yararlanın, ayrılmış Vm'leri kullanın ve bulut abonelikler arasında harcama tahmini yardımcı olur.



## <a name="best-practice-estimate-monthly-workload-costs"></a>En iyi yöntem: Aylık iş yükü kullanımı tahmini maliyetinizi hesaplayın
 
Geçirilen iş yükleri için aylık faturanızı tahmin etmek için kullanabileceğiniz araçlar vardır.

- **Azure fiyatlandırma hesaplayıcısı**: Tahmin etmek için örneğin VM ve depolama istediğiniz ürünleri seçin. Maliyetleri tahmin oluşturmak için fiyatlandırma hesaplayıcısını girdiğiniz.

  ![Azure fiyatlandırma hesaplayıcısı](./media/migrate-best-practices-costs/pricing.png) *Azure fiyatlandırma hesaplayıcısı*

- **Azure geçişi**: Maliyetlerini tahmin etmek için gözden geçirin ve Azure'da iş yüklerinizi çalıştırmak için gereken kaynaklar için hesabı gerekir. Bu verileri almak için sunucu, VM'ler, veritabanları ve depolama da dahil olmak üzere varlıklarınızı envanterini oluşturun. Bu bilgileri toplamak için Azure Geçişi'ı kullanabilirsiniz.

  - Azure geçişi bulur ve bir envanterini sağlamak üzere şirket içi ortamınızdaki değerlendirir.
  - Azure geçişi, eşleme ve tam resmini sahip sanal makineler arasındaki bağımlılıkları gösterir.
  - Azure geçişi değerlendirmesi tahmini maliyetini içerir.
    - İşlem maliyetleri: Değerlendirme oluşturduğunuzda önerilen Azure VM boyutu kullanarak, Azure geçişi faturalandırma API'si tahmini aylık VM maliyetleri hesaplamak için kullanır. Tahmin işletim sistemi, Yazılım Güvencesi, ayrılmış örnekler, VM çalışma süresi, konum ve para birimi ayarları göz önünde bulundurur. Tüm VM'lerin değerlendirmede maliyeti toplar ve bir toplam aylık işlem maliyeti hesaplar.
    - Depolama maliyeti: Azure geçişi, aylık toplam depolama maliyetlerini değerlendirme içindeki tüm sanal makineleri depolama maliyetlerini toplayarak hesaplar. Aylık depolama maliyeti belirli bir makine için aylık maliyeti bağlı tüm diskleri kullanarak hesaplayabilirsiniz. 

    ![Azure geçişi](./media/migrate-best-practices-costs/assess.png)
    *Azure geçişi değerlendirmesi*

**Daha fazla bilgi edinin:**
- [Kullanım](https://azure.microsoft.com/pricing/calculator/) Azure fiyatlandırma hesaplayıcısı.
- [Genel bakışın](https://docs.microsoft.com/azure/migrate/migrate-overview) Azure geçişi.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation) Azure geçişi değerlendirmeleri.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/dms/dms-overview) veritabanı geçiş hizmeti (DMS) hakkında.

## <a name="best-practice-right-size-vms"></a>En iyi yöntem: Doğru boyuta VM'ler

İş yüklerini desteklemek için Azure Vm'leri dağıtırken, bir dizi seçenek seçebilirsiniz. Her VM türünün belirli özellikler ve CPU, bellek ve disk farklı birleşimlerini vardır. Sanal makineleri şu şekilde gruplanır.

**Tür** | **Ayrıntılar** | **Kullanma**
--- | --- | ---
**Genel amaçlı** | Dengeli CPU/bellek. | Test ve geliştirme, küçük ve orta büyüklükte veri tabanları, orta düzey trafiğe sahip web sunucuları için düşük için iyidir.
**İşlem için iyileştirilmiş** | Yüksek CPU/bellek. | Orta trafikli web sunucusu, ağ Gereçleri, toplu işlemler, uygulama sunucuları için uygundur.
**Bellek için iyileştirilmiş** | Yüksek bellek-için-CPU. | İlişkisel veritabanları, Orta veya büyük bir önbellek, bellek içi analiz için iyidir.
**Depolama için iyileştirilmiş** | Yüksek disk aktarım hızı ve GÇ. | Büyük veri için iyi SQL ve NoSQL veritabanları.
**GPU için iyileştirilmiş** | Özel VM'ler. Tek veya birden çok GPU. | Ağır grafik ve video düzenleme.
**Yüksek performans** | Hızlı ve en güçlü CPU. İsteğe bağlı işleme düzeyi yüksek ağ arabirimleri (RDMA) ile Vm'leri | Kritik yüksek performanslı uygulamalar.

- Bu sanal makineler ve uzun vadeli bütçe etkileri arasındaki fiyat farklarını anlamak önemlidir.
- Her türde bir dizi içindeki VM serisi.
- Bir dizi içinde bir VM'yi seçtiğinizde, ayrıca, yalnızca VM ölçeğini artırıp serisi içinde ölçeklendirebilirsiniz. Örneğin, DSv2\_2 ölçeği artırma için DSv2\_Fsv2 gibi farklı bir seri için 4, ancak değiştirilemez\_2.

**Daha fazla bilgi edinin:**
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) VM türleri ve boyutlandırma ve türler için eşleme boyutları hakkında.
- [Plan](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs) VM boyutlandırma.
- [Gözden geçirme](https://docs.microsoft.com/azure/migrate/contoso-migration-assessment) kurgusal Contoso şirketi için bir örnek değerlendirme.

## <a name="best-practice-select-the-right-storage"></a>En iyi yöntem: Doğru depolama alanını seçin

Ayarlama ve şirket içi depolama (SAN ya da NAS) ve bunları desteklemek için ağları, pahalı ve zaman alıcı olabilir. Dosya (Depolama) verileri, yaygın olarak yönetim sıkıntılarına otomatik çözüm sağlamaya ve işletimsel çözmenize yardımcı buluta geçirilir. Microsoft Azure için verileri taşımak için çeşitli seçenekler sunar ve bu seçenekler hakkında kararlar almak gerekir. Kuruluşunuz, veri için doğru depolama türünü seçmeye yönelik ayda birkaç binlerce dolar kaydedebilirsiniz. Bazı önemli noktalar:

- Çok daha erişilebilir olmayan verileri ve iş açısından kritik değilse, en pahalı depolama alanında yerleştirilmesi gerekmez.
- Buna karşılık, önemli iş açısından kritik verileri daha yüksek katmanlı depolama seçenekleri bulunmalıdır.
- Planlama, geçiş sırasında Envanter verilerinin ve en uygun depolama alanına eşlemek için önem derecesine göre sınıflandırın. Bütçe ve maliyet yanı sıra performans göz önünde bulundurun. Maliyeti, ana karar verme faktörü mutlaka olmamalıdır. En ucuz seçeneği çekme, performans ve kullanılabilirlik riskleri iş yüküne kullanımına sunulmasına neden olabilir. 

### <a name="storage-data-types"></a>Depolama veri türleri

Azure depolama veri farklı türler sağlar.

| **Veri türü** | **Ayrıntılar** | **Kullanım** |
|--- | --- |  --- |
|**Bloblar** | Çok miktarda metin veya ikili veri gibi yapılandırılmamış nesne depolamak için en iyi duruma getirilmiş<br/>Verilere erişen her yerden HTTP/HTTPS üzerinden. | Akış ve rastgele erişim senaryoları için kullanın. Örneğin, görüntü ve belgelerin doğrudan bir tarayıcıya sunmak için video ve ses akışı ve yedekleme ve olağanüstü durum kurtarma veri depolayın.|
|**Dosyalar** | Yönetilen dosya paylaşımları SMB 3.0 üzerinden erişilen | Geçiş dosya paylaşımları, şirket içinde olduğunda kullanın ve birden çok erişim/dosya verileri için bağlantılar sağlar.|
|**Diskler** | Sayfa BLOB'ları üzerinde temel.<br/><br/> Disk türü (hızlı): Standart (HDD veya SSD) veya Premium (SSD).<br/><br/>Disk Yönetimi: Yönetilmeyen (yönettiğiniz disk ayarlarını ve depolama) veya yönetilen (disk türünü seçin ve Azure tarafından yönetilen disk için). | Premium diskler, VM'ler için kullanın. Basit yönetim ve ölçeklendirme için yönetilen diskleri kullanın.|
|**Kuyruklar** | Store ve çok büyük sayılarda mesajı kimliği doğrulanmış aramalar (HTTP veya HTTPS) erişilen alma | Uygulama bileşenleri ile zaman uyumsuz ileti kuyruğu bağlanın.|
|**Tabloları** | Tablolar Store. | Artık Azure Cosmos DB tablo API'si, parçası.|



### <a name="access-tiers"></a>Erişim katmanları
Azure depolama blok blob verilerine erişmek için farklı seçenekler sunar. Doğru erişim katmanı seçerek, blok blobu veri en uygun maliyetli bir şekilde depoladığınızdan emin olun yardımcı olur.

**Tür** | **Ayrıntılar** | **Kullanım**
--- | --- | ---
**Sık erişimli** | Seyrek erişimli ' maliyeti daha yüksek depolama alanı. Seyrek erişimli'den daha düşük erişim ücretleri.<br/><br/>Bu varsayılan katmandır. | Etkin kullanımda sık erişilen veriler için kullanın.
**seyrek erişimli** | Sık erişimli'den depolama maliyetini düşürür. Sık erişimli'den daha yüksek erişim ücretleri.<br/><br/> Store için en az 30 gün içinde. | Store kısa süreli, veri kullanılabilir, ancak erişilen seyrek.
**Arşiv** | Tek tek blok blobları için kullanılır.<br/><br/> En uygun maliyetli seçeneği depolama. Veri erişimi daha sıcak ve soğuk daha pahalıdır. | Sunucu saatlik alma gecikmesinden etkilenmeyecek ve katmanında en az 180 gün boyunca kalacak veriler için kullanın. 

### <a name="storage-account-types"></a>Depolama hesabı türleri

Azure, farklı türlerde depolama hesapları ve performans katmanları sağlar.

**Hesap türü** | **Ayrıntılar** | **Kullanım**
--- | --- | ---
**Genel amaçlı v2 standart** | BLOB'larını destekler (engellemek, sayfa, ekleme), dosyalar, diskler, kuyruklar ve tablolar.<br/><br/> Sık erişimli, seyrek erişimli ve Arşiv erişim katmanı destekler. ZRS desteklenir. | Çoğu senaryo ve çoğu veri türleri için kullanın. Standart depolama hesapları, HDD veya SSD tabanlı olabilir.
**Genel amaçlı v2 Premium** | BLOB Depolama verilerini (sayfa blobları) destekler. Sık erişimli, seyrek erişimli ve Arşiv erişim katmanı destekler. ZRS desteklenir.<br/><br/> SSD üzerinde depolanır. | Microsoft, tüm VM'ler için kullanılmasını önerir.
**Genel amaçlı v1** | Erişim katmanı desteklenmiyor. ZRS desteklemiyor | Uygulamaları Azure Klasik dağıtım modeli gerekiyorsa kullanın.
**Blob** | Yapılandırılmamış nesneleri depolamak için özel depolama hesabı'nı tıklatın. Blok blobları sağlar ve ekleme blobları yalnızca (hiçbir dosya, kuyruk, tablo veya Disk Depolama Hizmetleri). Aynı dayanıklılık, kullanılabilirlik, ölçeklenebilirlik ve performans genel amaçlı v2 olarak sağlar. | Sayfa bloblarını bu hesaplarda depolayamaz ve bu nedenle VHD dosyalarını da depolayamazsınız. Erişim katmanı sık erişimli veya seyrek erişimli olarak ayarlayabilirsiniz.

### <a name="storage-redundancy-options"></a>Depo yedekliliği seçenekleri

Depolama hesapları, esneklik ve yüksek kullanılabilirlik için artıklığı farklı türde kullanabilirsiniz.

**Tür** | **Ayrıntılar** | **Kullanım**
--- | --- | ---
**Yerel olarak yedekli depolama (LRS)** | Bir tek bir depolama birimine ayrı hata etki alanı ve güncelleme etki alanı içinde çoğaltarak, yerel bir kesintiye karşı korur. Bir veri merkezinde verilerinizin birden çok kopyasını tutar. En az % 99,999999999 sağlar (11 9\'s) belirli bir yıl boyunca nesnelerin dayanıklılık. | Uygulamanızı kolayca reconstructed veri depolar, göz önünde bulundurun.
**Bölgesel olarak yedekli depolama (ZRS)** | Tek bir bölgede üç depolama kümeleri arasında çoğaltarak veri merkezi kesintisi yeniden korur. Her Depolama kümesi fiziksel olarak ayrılmış ve kendi kullanılabilirlik alanı'nda bulunur. Sağlayan en az % 99,9999999999 (12 9\'s) birden çok veri merkezleri veya bölgede verilerinizin birden çok kopyasını tutarak belirli bir yıl boyunca nesnelerin dayanıklılık. | Tutarlılık, dayanıklılık ve yüksek kullanılabilirlik gerekli olup olmadığını düşünün. Birden çok bölge kalıcı olarak etkilendiğinde bölgesel bir olağanüstü durum karşı korumaz.
**Coğrafi olarak yedekli depolama (GRS)** | Yüzlerce mil uzaktaki birincil bir ikincil bölgeye veri çoğaltma yaparak bir bölgenin tamamını arızasına karşı korur. En az % 99,99999999999999 sağlar (16 9\'s) belirli bir yıl boyunca nesnelerin dayanıklılık. | Çoğaltma verilerini Microsoft ikincil bölgeye yük devretme başlatır sürece kullanılamaz. Yük devretme gerçekleşirse, okuma ve yazma erişimi yok.
**Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)** | GRS benzer. En az % 99,99999999999999 sağlar (16 9\'s) belirli bir yıl boyunca nesnelerin dayanıklılık | Sağlar ve % 99,99 kullanılabilirlik GRS için kullanılan ikinci bölgeden okuma erişimine izin vererek okuyun.

**Daha fazla bilgi edinin:**
- [Gözden geçirme](https://azure.microsoft.com/pricing/details/storage/) Azure depolama fiyatlandırması.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-import-export-service) Azure içeri/dışarı aktarma için büyük miktarda veriyi Azure BLOB'ları ve dosyaları geçiş. 
- [Karşılaştırma](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) bloblar, dosyalar ve disk depolama veri türleri.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) erişim katmanları hakkında.
- [Gözden geçirme](https://docs.microsoft.com/azure/storage/common/storage-account-overview?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) farklı türlerde depolama hesapları.
- Hakkında bilgi edinin [depolama yedekliliği](https://docs.microsoft.com/azure/storage/common/storage-redundancy), [LRS](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fqueues%2ftoc.json), [ZRS](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs?toc=%2fazure%2fstorage%2fqueues%2ftoc.json), [GRS](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs?toc=%2fazure%2fstorage%2fqueues%2ftoc.json), ve [okuma erişimli GRS](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#read-access-geo-redundant-storage).
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) Azure dosyaları hakkında.



## <a name="best-practice-leverage-azure-hybrid-benefits"></a>En iyi yöntem: Azure hibrit avantajları yararlanın

Yazılım sistemleri Windows Server ve SQL Server gibi yatırım yıllardır nedeniyle, müşterilerin bulutta, diğer bulut sağlayıcılarının mutlaka sağlayamaz önemli indirimler değer sunmak için benzersiz bir konumda Microsoft'tur. 

Tümleşik bir Microsoft şirket içi/Azure ürün Portföyü rekabetçi ve maliyet avantajları oluşturur. Şu anda bir işletim sistemini veya diğer yazılım lisanslama Yazılım Güvencesi (SA) varsa, bu lisansları sizinle için Azure hibrit Teklifi'nden yararlanarak buluta uygulayabileceğiniz.

**Daha fazla bilgi edinin:**

- [Bir göz atın](https://azure.microsoft.com/pricing/hybrid-benefit/) hibrit avantajı Tasarruf hesaplayıcısı.
- [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/hybrid-benefit/) Windows Server için hibrit Avantajı hakkında.
- [Gözden geçirme](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) SQL Server Azure Vm'leri için fiyatlandırma Kılavuzu.


## <a name="best-practice-use-reserved-vm-instances"></a>En iyi yöntem: Ayrılmış VM örnekleri kullanın

Çoğu bulut platformu, Kullandıkça Öde ayarlanır. Dinamik iş yükleri mutlaka bilmiyorsanız bu yana bu modeli sunar dezavantajları olacaktır. Bir iş yükü için NET amaçları belirttiğinizde, altyapı planlama katkıda bulunur.

Azure ayrılmış VM örnekleri kullanarak, bir ön ödeme veya üç yıllık süre VM örneği. 

- Ön ödemeli kullandığınız kaynaklar üzerinde bir indirim sağlar.
- En fazla %72 Kullandıkça Öde fiyatlarında VM, SQL veritabanı işlem, Azure Cosmos DB veya diğer kaynak maliyetleri önemli ölçüde azaltabilir. 
- Rezervasyon faturalandırma indirim sağlar ve kaynaklarınızın çalışma zamanı durumunu etkilemez.
- Ayrılmış örnekler iptal edebilirsiniz.

![Ayrılmış örnekleri](./media/migrate-best-practices-costs/reserve.png)
*Azure ayrılmış VM*

**Daha fazla bilgi edinin:**
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations) Azure ayırmalar.
- [Okuma](https://azure.microsoft.com/pricing/reserved-vm-instances/#faq) ayrılmış örnekleri hakkında SSS.
- [Fiyatlandırma Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) SQL Server Azure Vm'leri için.


## <a name="best-practice-aggregate-cloud-spend-across-subscriptions"></a>En iyi yöntem: Abonelikler arasında toplama bulut Harcamaları

Sonuçta, birden fazla Azure aboneliği gerekir, kaçınılmazdır. Örneğin, geliştirme ve üretim sınırları ayırmak için başka bir abonelik gerekebilir, veya her müşteri için ayrı bir abonelik gerektiren bir platform olabilir. Tek bir platform içinde tüm abonelikleri genelinde raporlama veri toplama yeteneğine sahip olmanın önemli bir özelliktir.

Bunu yapmak için Azure maliyet Yönetimi API'leri kullanabilirsiniz. Ardından, verileri Azure SQL gibi tek bir kaynak olarak toplama sonra toplanan verileri ortaya çıkarma Power BI gibi araçları kullanabilirsiniz. Toplanan abonelik raporları ve ayrıntılı raporlar oluşturabilirsiniz. Örneğin, maliyet Yönetimi proaktif Öngörüler isteyen kullanıcılar için belirli görünümler departman, kaynak grubu temel maliyetleri, vb. oluşturabilirsiniz. Azure faturalama veri tam erişim ile bunları girmeniz gerekmez.

**Daha fazla bilgi edinin:**

- [Genel bakışın](https://docs.microsoft.com/azure/billing/billing-consumption-api-overview) Azure tüketim API.
- [Hakkında bilgi edinin](https://docs.microsoft.com/power-bi/desktop-connect-azure-consumption-insights) Power BI Desktop'ta Azure tüketim öngörüleri bağlanma.
- [Bilgi nasıl](https://docs.microsoft.com/azure/billing/billing-manage-access) rol tabanlı erişim denetimi (RBAC) kullanarak Azure için fatura bilgilerini erişimi yönetin.
 
## <a name="after-migration"></a>Geçişten sonra

Başarılı bir geçiş iş yüklerinizin ve kullanım verilerini toplama birkaç hafta sonra kaynakları maliyetlerini hakkında NET bir fikir gerekir.
- Veri analiz ederken, bir bütçe temel Azure kaynak grubu ve kaynak oluşturmaya başlayabilirsiniz.
- Ardından, bulut bütçenizi nerede harcandığını anladığınızda, daha da maliyetlerinizi azaltmak nasıl analiz edebilirsiniz.

Bu bölümde en iyi uygulamalar, Azure maliyet Yönetimi maliyet ödemeyle ve analiz, izleme, depolama ve VM'lerin en iyi duruma getirme kaynakları izleme ve kaynak grubu bütçelerini uygulamak için kullanmayı içerir.

## <a name="best-practice-use-azure-cost-management"></a>En iyi yöntem: Azure maliyet Yönetimi'ni kullanın

Microsoft, harcama, şu şekilde izlemenize yardımcı olması için Azure maliyet yönetimi sağlar:

- İzleme ve Azure harcama denetlemek ve kaynaklarının kullanımını en iyi duruma getirme için yardımcı olur.
- Tüm aboneliğinizi ve kaynaklarının tümü inceler ve öneriler sağlar.
- Harici Araçlar ve finansal raporlama sistemleri tümleştirmek için bir tam API sağlar.
- Kaynak kullanımını izler ve tek, birleştirilmiş bir görünüm ile bulut maliyetlerinizi yönetin.
- Bilgiye dayalı kararlar almanıza yardımcı olacak zengin operasyonel ve finansal Öngörüler sağlar.

Maliyet Yönetimi'nde, şunları yapabilirsiniz:


- **Bir bütçe oluşturmak**: Finansal sorumluluk için bütçe oluşturun.
  - Hizmetleri kullanan veya belirli bir süre (aylık, üç aylık, Yıllık) ve bir kapsamın (abonelikler/kaynak grupları) için abone için hesap. Örneğin, bir aylık, üç aylık veya yıllık dönem için bir Azure aboneliği bütçe oluşturabilirsiniz.
  - Bütçe oluşturduktan sonra maliyet analizi gösterilmektedir. Geçerli harcama karşı bütçenizi görüntüleme maliyetlerinizi analiz edilirken gerektiği ve harcama ilk adımlarından biridir.
  - Bütçe eşiklerine ulaşıldığında, e-posta bildirimleri gönderilebilir.
  - Analiz için Azure depolama için maliyet Yönetimi verilerine dışarı aktarabilirsiniz.

    ![Maliyet Yönetimi bütçe](./media/migrate-best-practices-costs/budget.png)
    *Azure maliyet Yönetimi bütçe*

- **Maliyet analizi yapmak**: Keşfedin ve maliyetleri nasıl doğan, anlamanıza yardımcı olması için kuruluş maliyetlerinizi analiz etmek için bir maliyet analizi alın ve harcama eğilimleri belirleyin.
  - Maliyet analizi EA kullanıcılar tarafından kullanılabilir.
  - Kapsamlar, departman, hesap, abonelik veya kaynak grubu tarafından da dahil olmak üzere bir dizi için maliyet analizi verilerini görüntüleyebilirsiniz.
  - Geçerli aya ait toplam maliyetleri ve birikmiş günlük maliyetlerin gösteren bir maliyet analizi alabilirsiniz. 

    ![Maliyet Yönetimi çözümlemesi](./media/migrate-best-practices-costs/analysis.png)
    *Azure maliyet Yönetimi çözümleme*
- **Öneri**: Nasıl en iyi duruma getirmek ve verimliliği artırmak Göster Danışmanı önerileri alın.


**Daha fazla bilgi edinin:**

- [Genel bakışın](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyet yönetimi.
- [Bilgi edinmek için nasıl](https://docs.microsoft.com/azure/cost-management/cost-mgt-best-practices) Azure maliyet yönetimi ile bulut yatırımınızdan en iyi duruma getirme.
- [Bilgi edinmek için nasıl](https://docs.microsoft.com/azure/cost-management/use-reports) Azure maliyet Yönetimi raporlarını kullanma.
- [Bir öğretici alma](https://docs.microsoft.com/azure/cost-management/tutorial-acm-opt-recommendations?toc=/azure/billing/TOC.json) önerilerden maliyetleri en iyi duruma getirmek.
- [Gözden geçirme](https://docs.microsoft.com/rest/api/consumption/budgets) Azure tüketim API.

## <a name="best-practice-monitor-resource-utilization"></a>En iyi yöntem: Kaynak kullanımını izleme

Azure'da, kaynakları tüketilir ve bunlar değilken ödemeyin kullandıklarınız için ödeme yaparsınız. VM'ler için VM ayrılan ve bir VM serbest sonra ücretlendirilmezsiniz faturalandırma gerçekleşir. Bu aklınızda kullanımda Vm'leri izleme ve VM boyutlandırma doğrulamanız gerekir.

- VM iş yüklerinizi temelleri belirlemek için sürekli olarak değerlendirin.
- Örneğin, iş yükünüz Pazartesi-Cuma 8: 00 için 6 pm, yoğun olarak kullanılan ancak zor o saatleri dışında kullanılan, yoğun saatler dışında Vm'leri düşürme. Bu VM boyutları değiştirme anlamına gelebilir veya sanal makine ölçek kullanarak otomatik ölçeklendirme Vm'leri yukarı veya aşağı ayarlar.
- Bazı şirketler "erteleme süresi", ne zaman kullanılabilir olmaları gerektiği ve ne zaman gerekli olmadığında bir takvim üzerinde koyarak Vm'leri gerektiğini belirtir. 
- VM izlemenin yanı sıra, ExpressRoute ve altında ve kullanım için sanal ağ geçitleri gibi diğer ağ kaynakları izlemeniz gerekir.
- Çeşitli Azure maliyet yönetimi, Azure İzleyici ve Azure Danışmanı'nı da dahil olmak üzere Microsoft araçlarını kullanarak sanal makine kullanımını izleyebilirsiniz. Üçüncü taraf araçları de mevcuttur.  

**Daha fazla bilgi edinin:**
- Genel Bakış [Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/overview) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-overview).
- [Alma](https://docs.microsoft.com/azure/advisor/advisor-cost-recommendations) Advisor maliyet önerileri.
- [Bilgi nasıl [önerilerden maliyetlerini iyileştirme](https://docs.microsoft.com/azure/cost-management/tutorial-acm-opt-recommendations?toc=/azure/billing/TOC.json), ve [beklenmeyen ücretlerden](https://docs.microsoft.com/azure/billing/billing-getting-started).
- [Hakkında bilgi edinin](https://github.com/Azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit/) Azure kaynak iyileştirme (ARO) Araç Seti

## <a name="best-practice-implement-resource-group-budgets"></a>En iyi yöntem: Kaynak grubu bütçelerini uygulayın

Genellikle, kaynak grupları, maliyet sınırlarını temsil etmek için kullanılır. Bu kullanım düzeniyle birlikte Azure ekibi, kaynak kaynak grubu ve kaynakları bütçeleri oluşturun olanağı dahil olmak üzere farklı düzeylerde harcama analiz etmek ve izlemek için yeni ve geliştirilmiş yollar geliştirmeye devam eder.  

- Bir kaynak grubu bütçe bir kaynak grubu ile ilişkili maliyetleri izlemenize yardımcı olur.
- Uyarıları tetikleyin ve bütçe üst sınırına ulaştınız veya aşıldı gibi çok çeşitli playbook'ları çalıştırın. 

**Daha fazla bilgi edinin:**

- [Bilgi edinmek için nasıl](https://docs.microsoft.com/azure/billing/billing-cost-management-budget-scenario) Azure bütçe ile maliyetleri yönetme.
- [Bir öğreticiyi izleyin](https://docs.microsoft.com/azure/cost-management/tutorial-acm-create-budgets?toc=/azure/billing/TOC.json) oluşturmak ve bir Azure bütçe yönetmek için.


## <a name="best-practice-optimize-azure-monitor-retention"></a>En iyi yöntem: Azure İzleyici bekletme en iyi duruma getirme

Kaynakları Azure'a taşıyın ve bunları için tanılama günlüğünü etkinleştirme gibi çok fazla günlük verileri oluşturur. Genellikle bu günlük verileri Log Analytics çalışma alanına eşlenmiş bir depolama hesabına gönderilir.

- Artık Günlük veri saklama süresi, daha fazla veri, gerekir.
- Tüm günlük verilerini eşittir ve bazı kaynaklar diğerlerinden daha fazla günlük verileri oluşturur.
- Düzenlemeleri ve uyumluluk nedeniyle diğerlerine göre daha uzun bazı kaynaklar için günlük verileri korumak gerektiğini olasıdır.
- Günlük depolama maliyetlerinizi en iyi duruma getirme ve ihtiyacınız olan günlük verileri tutma arasında dikkatli bir satır yol.
- Değerlendirme ve harcama hiçbir önem günlük tutma para olmayan bir geçiş tamamlandıktan hemen sonra günlük ayarını öneririz.

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs) kullanım ve Tahmini maliyetler izleme.
 
## <a name="best-practice-optimize-storage"></a>En iyi yöntem: Depolama en iyi duruma getirme

Geçiş işleminden önce depolama seçmeye yönelik en iyi izlediyseniz, büyük olasılıkla bazı avantajlar apps'teki. Ancak, yine de iyileştirebilirsiniz büyük olasılıkla ek depolama alanı maliyeti yoktur. Zamanla, blobları ve dosyaları eski haline gelir. Veriler artık kullanılabilir değil, ancak yasal düzenleme gereksinimleri için belirli bir süre saklamak gerektiği anlamına gelebilir. Bu nedenle, özgün geçiş için kullanılan yüksek performanslı depolama depolamak istediğiniz gerekmeyebilir.

Tanımlama ve ucuz depolama alanlarına eski veri taşıma, aylık depolama bütçenizi muazzam bir etkisi olması ve maliyet tasarrufu. Azure belirleyin ve ardından bu eski veri depolamak yardımcı olmak için birçok yol sağlar.

- Genel amaçlı v2 depolama, daha az önemli verileri sık erişimli'den seyrek erişimli ve Arşiv katmanlara taşımak için erişim katmanı yararlanın.
- StorSimple, özelleştirilmiş ilkelerine bağlı eski veri taşımak için kullanın. 

**Daha fazla bilgi edinin:**
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) erişim katmanları hakkında.
- [Genel bakışın](https://docs.microsoft.com/azure/azure-monitor/overview) , storsimple ve [StorSimple fiyatlandırma](https://azure.microsoft.com/pricing/details/storsimple/).

## <a name="best-practice-automate-vm-optimization"></a>En iyi yöntem: VM en iyi duruma getirme otomatikleştirin

Bulutta çalışan bir VM nihai amacıyla CPU, bellek ve kullanılan disk'en üst düzeye çıkarmaktır. İçin optimize edilmediğinden Vm'leri bulmak veya Vm'leri ne zaman kullanılmaz sık sürelerine sahip, onları kapatmanız veya aşağı ölçeklemenizi mantıklıdır bunları sanal makine ölçek kümeleri kullanma.

Azure Otomasyonu, VM ölçek kümeleri, otomatik kapatma ve komut dosyalı veya 3 taraf çözümleri ile bir VM en iyi duruma getirebilirsiniz. 

**Daha fazla bilgi edinin:**

- [Bilgi edinmek için nasıl](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-vertical-scale-reprovision) dikey otomatik ölçeklendirme kullanın.
- [Zamanlama](https://azure.microsoft.com/updates/azure-devtest-labs-schedule-vm-auto-start/) VM autostart.
- [Bilgi edinmek için nasıl](https://docs.microsoft.com/azure/automation/automation-solution-vm-management) başlayın veya Azure automation'da saatleri dışında Vm'leri durdur.
- [Daha fazla bilgi edinin] [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-overview)ve [Azure kaynak iyileştirme (ARO) Araç Seti](https://github.com/Azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit/).

## <a name="best-practices-use-logic-apps-and-runbooks-with-budgets-api"></a>En iyi uygulamalar: Logic Apps ve runbook'ları bütçelerini API ile kullanma

Azure Kiracı fatura bilgilerinizi erişimi olan bir REST API'si sağlar.

- Dış sistemler ve API verileri yapı ölçümleri tarafından tetiklenen iş akışlarını tümleştirmeyi bütçelerini API'yi kullanabilirsiniz.
- Tercih edilen veri analizi araçları kullanımı ve kaynak veri çekebilirsiniz.
- Azure Kaynak Kullanımı ve RateCard API'leri maliyetlerinizi doğru tahmin etmenize ve yönetmenize yardımcı olabilir.
- API'ler bir kaynak sağlayıcısı olarak uygulanır ve Azure Resource Manager tarafından sunulan API'lerde eklenir.
- Bütçe API, Azure Logic Apps ve runbook'ları ile tümleştirilebilir.

**Daha fazla bilgi edinin:**

- [Daha fazla bilgi edinin](https://docs.microsoft.com/rest/api/consumption/budgets) bütçelerini API hakkında daha fazla.
- [Öngörüler elde edin](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) faturalandırma API'si ile Azure kullanımı hakkında.


## <a name="best-practice-implement-serverless-technologies"></a>En iyi yöntem: Sunucusuz teknolojilerini uygulamak

VM iş yükleri genellikle "olduğu gibi" geçiş kapalı kalma süresini önlemek için. Genellikle sanal makineleri çalıştırmak için kısa bir süre alma aralıklı olan görevleri veya alternatif olarak kaç saat barındırabilir. Örneğin, zamanlanmış görevler gibi Windows çalıştıran sanal makineler, Zamanlayıcı veya PowerShell betikleri görev. Bu görevleri çalıştırmadığınız olduğunda, yine de VM azaltırız ve depolama maliyetlerini disk.

Geçişten sonra bu tür görevleri tam olarak gözden geçirdikten sonra bunları Azure işlevleri veya Azure Batch işleri gibi sunucusuz teknolojilerini geçiş düşünebilirsiniz. Bu çözüm sayesinde artık yönetmek ve bir ek maliyet tasarrufu getirme Vm'leri korumak ihtiyacınız. 

**Daha fazla bilgi edinin:**
- [Hakkında bilgi edinin](https://azure.microsoft.com/services/functions/) Azure işlevleri
- [Daha fazla bilgi edinin](https://azure.microsoft.com/services/batch/) Azure Batch
  
## <a name="next-steps"></a>Sonraki adımlar 

Diğer en iyi uygulamaları gözden geçirin:

- [En iyi uygulamalar](migrate-best-practices-security-management.md) güvenlik ve yönetim geçişten sonra.
- [En iyi uygulamalar](migrate-best-practices-networking.md) geçişten sonra ağ.

