---
title: Azure Data Lake depolama Gen1 kullanmak için en iyi yöntemler | Microsoft Docs
description: En iyi veri alımı, tarih güvenlik ve Azure Data Lake depolama Gen1 (daha önce Azure Data Lake Store da bilinir) kullanımıyla ilgili performans hakkında bilgi edinin
services: data-lake-store
documentationcenter: ''
author: sachinsbigdata
manager: jhubbard
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: sachins
ms.openlocfilehash: 37af9007a1572bf2f297909fe1351d7f122405e3
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297031"
---
# <a name="best-practices-for-using-azure-data-lake-storage-gen1"></a>Azure Data Lake depolama Gen1 kullanmak için en iyi uygulamalar

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Bu makalede, en iyi uygulamalar ve Azure Data Lake Store ile çalışma konuları hakkında bilgi edinin. Bu makalede, güvenlik, performans, dayanıklılık ve Data Lake Store için izleme bilgileri sağlar. Önce Data Lake Store, Azure HDInsight gibi hizmetleri gerçekten büyük verilerle çalışma karmaşıktı. Böylece Petabayt depolama ve uygun ölçekte, en iyi performans elde edilebilir parça verileri birden çok Blob Depolama hesabı arasında vardı. Data Lake Store ile Boyut ve performans için sabit sınırlar çoğunu kaldırılır. Ancak, yine de Data Lake Store ile en iyi performansı elde etmek, bu makalede kapsayan bazı noktalar vardır. 

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Azure Data Lake Store, POSIX erişim denetimleri ve Azure Active Directory (Azure AD) kullanıcıları, grupları ve hizmet sorumluları için ayrıntılı sunar. Bu erişim denetimleri, mevcut dosya ve klasörler için ayarlanabilir. Erişim denetimleri, yeni dosyalar veya klasörler için uygulanan varsayılan değerleri oluşturmak için de kullanılabilir. Var olan klasörler ve alt nesneleri için ayarlanmış olan izinlere, izinleri yayılan yinelemeli olarak her nesne üzerinde olmanız gerekir. Çok sayıda dosya izinleri yayma, uzun bir zaman var. alabilir. Geçen süre, saniye başına işlenen 30-50 nesneler arasında değişebilir. Bu nedenle, klasör yapısını ve kullanıcı gruplarını uygun şekilde planlayın. Verilerinizle çalışırken Aksi takdirde, beklenmedik gecikmeler ve sorunları neden olabilir. 

100.000 alt nesneleri içeren bir klasör olduğunu kabul edelim. Saniye başına işlenen 30 nesnelerin alt sınır alırsa, tüm klasörün izinlerini güncelleştirmek için bir saat sürebilir. Data Lake Store ACL'leri hakkında daha fazla ayrıntı kullanılabilir [erişim denetimi, Azure Data Lake Store](data-lake-store-access-control.md). ACL'ler yinelemeli olarak atama hakkında daha iyi performans için Azure Data Lake komut satırı aracını kullanabilirsiniz. Aracı, birden çok iş parçacığı ve hızlı bir şekilde milyonlarca dosyaya ACL uygulamak için özyinelemeli Gezinti mantığı oluşturur. Araç, Linux ve Windows, kullanılabilir ve [belgeleri](https://github.com/Azure/data-lake-adlstool) ve [indirir](http://aka.ms/adlstool-download) bu araç, Github'da bulunabilir. Bu aynı performans geliştirmeleri, Data Lake Store ile yazılmış kendi araçları tarafından etkinleştirilebilir [.NET](data-lake-store-data-operations-net-sdk.md) ve [Java](data-lake-store-get-started-java-sdk.md) SDK'ları.

### <a name="use-security-groups-versus-individual-users"></a>Bireysel kullanıcılar ve güvenlik grupları kullanın 

Büyük olasılıkla Data Lake Store, büyük veri ile çalışırken, bir hizmet sorumlusu verilerle çalışmak için Azure HDInsight gibi hizmetlerin izin vermek için kullanılır. Bununla birlikte, burada bireysel kullanıcılar verileri de erişmesi gereken durumlar olabilir. Bu gibi durumlarda, Azure Active Directory kullanmalısınız [güvenlik grupları](data-lake-store-secure-data.md#create-security-groups-in-azure-active-directory) yerine bireysel kullanıcılar klasörleri ve dosyaları atama. 

Bir güvenlik grubu izinlerini atandıktan sonra ekleme veya kullanıcıları gruptan kaldırma Data Lake Store için herhangi bir güncelleştirme gerektirmez. Bu ayrıca sınırını aşan yoksa sağlamaya yardımcı olur [32 erişim ve varsayılan ACL'leri](../azure-subscription-service-limits.md#data-lake-store-limits) (Bu, her zaman her dosya ve klasör ile ilişkili olan dört POSIX stili ACL'leri içerir: [sahip olan kullanıcı](data-lake-store-access-control.md#the-owning-user), [sahip olan grup](data-lake-store-access-control.md#the-owning-group), [maske](data-lake-store-access-control.md#the-mask)ve diğer).

### <a name="security-for-groups"></a>Güvenlik grupları 

Data Lake Store için erişim kullanıcıların gerektiğinde açıklandığı gibi Azure Active Directory güvenlik gruplarını kullanmak en iyisidir. Bazı gruplar ile başlaması olabilir önerilen **ReadOnlyUsers**, **WriteAccessUsers**, ve **FullAccessUsers** hesabının ve hatta kök anahtar için ayrı alt klasörleri. Varsa diğer daha sonra eklenen, ancak değil tanımlanan kullanıcı henüz, beklenen grupları, belirli klasörlere erişimi işlevsiz güvenlik grupları oluşturmayı düşünebilirsiniz. Güvenlik grubu kullanarak sağlar daha sonra bir uzun ihtiyacınız yoktur, binlerce dosya için yeni izinler atamak için işleme süresi. 

### <a name="security-for-service-principals"></a>Hizmet sorumluları için güvenlik 

Azure Active Directory Hizmet sorumluları, genellikle verilere Data Lake Store, Azure HDInsight gibi hizmetleri tarafından kullanılır. Birden çok iş yükleri arasında erişim gereksinimlerine bağlı olarak, içindeki ve dışındaki kuruluş güvenliğini sağlamak için bazı durumlar olabilir. Birçok müşteri, tek bir Azure Active Directory Hizmet sorumlusu yeterli olabilir ve Data Lake Store kökünde tam izinlere sahip olabilir. Diğer müşteriler, bir küme veri ve yalnızca okuma erişimi olan başka bir küme tam erişime sahip olduğu farklı hizmet sorumluları ile birden fazla küme gerektirebilir. Güvenlik gruplarıyla olduğu gibi bir Data Lake Store hesabı oluşturulduktan sonra her beklenen (okuma, yazma, tam) senaryosu için bir hizmet sorumlusu yapmayı düşünebilirsiniz. 

### <a name="enable-the-data-lake-store-firewall-with-azure-service-access"></a>Data Lake Store güvenlik duvarı ile Azure hizmet erişimini etkinleştir 

Data Lake Store, bir Güvenlik Duvarı'nı ve Azure Hizmetleri için yalnızca dış izinsiz Girişi'dan daha küçük bir saldırı vektörü için önerilen erişimi sınırlandırma seçeneğini destekler. Güvenlik Duvarı, Data Lake Store hesabında Azure portalı üzerinden etkinleştirilebilir **Güvenlik Duvarı** > **etkinleştir Güvenlik Duvarı (açık)** > **Azure hizmetlerine erişime izin ver**  seçenekleri.  

![Güvenlik Duvarı ayarları, Data Lake Store](./media/data-lake-store-best-practices/data-lake-store-firewall-setting.png "güvenlik duvarı, Data Lake Store ayarları")

Güvenlik Duvarı etkinleştirilmişse, yalnızca Azure Hizmetleri gibi HDInsight olduktan sonra Data Factory, SQL veri ambarı vb. için Data Lake Store erişimi. Nedeniyle Azure tarafından kullanılan iç ağ adresi çevirisi Data Lake Store güvenlik duvarı belirli hizmetleri IP kısıtlama desteklemez ve yalnızca şirket içi gibi Azure, dışındaki uç kısıtlamaları için tasarlanmıştır. 

## <a name="performance-and-scale-considerations"></a>Performans ve ölçek ilgili önemli noktalar

Data Lake Store en güçlü özelliklerinden veri akışındaki sabit sınırlara kaldırır biridir. Sınırları kaldırmak, müşterilerin kendi veri boyutunu büyütün sağlar ve verileri parçaya gerek kalmadan performans gereksinimleri eşlik. Data Lake Store performansı iyileştirmek için en önemli konular en iyi paralellik olduğunda verilen gerçekleştirir biridir.

### <a name="improve-throughput-with-parallelism"></a>Aktarım hızı paralelliği ile geliştirin 

Çekirdek başına 8-12 iş parçacıkları için en iyi okuma/yazma performans vermeyi düşünün. Bu engelleme okumayı/yazmayı tek bir iş parçacığında kaynaklanır ve daha fazla iş parçacığı daha yüksek Eş zamanlılık VM'de izin verebilirsiniz. Düzeyleri sağlıklı olduğunu ve paralellik artırılabilir sağlamak için sanal makinenin CPU kullanımı izleme emin olun.   

### <a name="avoid-small-file-sizes"></a>Küçük dosya boyutunu kaçının

POSIX izinleri ve Data Lake Store içinde denetim gelen bir Giderle çok sayıda küçük dosyalarıyla çalışırken belirgin olur. En iyi uygulama, Data Lake Store için binlerce veya milyonlarca küçük dosyaları yazma ile karşılaştırması büyük dosyalarına verilerinizi toplu gerekir. Küçük dosya boyutunu önleme birden çok avantaj, aşağıdaki gibi olabilir:

* Kimlik doğrulama denetimleri birden çok dosyaya azaltmayı
* Dosya Aç bağlantı azaltılmış
* Daha hızlı kopyalama/çoğaltma
* Data Lake Store POSIX izinler güncelleştirilirken işlemek için daha az sayıda dosya 

Hangi hizmet ve iş yüklerini verileri kullanarak bağlı, dosyaları dikkate almanız iyi bir boyut 256 MB olan veya büyük. Dosya boyutu, Data Lake Store içinde giriş olduğunda toplu olamaz, bu dosyalar daha büyük parçalara birleştiren bir ayrı düzenleme işi olabilir. Daha fazla bilgi ve dosya boyutları ve Data Lake Store verileri düzenleme hakkında daha fazla öneri için bkz. [Veri kümenizi yapısı](data-lake-store-performance-tuning-guidance.md#structure-your-data-set).

### <a name="large-file-sizes-and-potential-performance-impact"></a>Büyük dosya boyutlarına ve olası performans etkisini

Data Lake Store, Petabayt büyük dosya boyutu, en iyi performans için ve bağlı verilerini okuma işlemi olarak desteklese 2 GB ortalama gitmek ideal olmayabilir. Örneğin, kullanırken **Distcp** konumları veya farklı bir depolama hesabı arasında veri kopyalamak için en ince harita görevleri belirlemek için kullanılan bir ayrıntı düzeyinde dosyalarıdır. Bu nedenle, her biri 1 TB olan 10 dosya kopyalanıyorsa, en fazla 10 azaltıcının ayrılır. Atanan azaltıcının ile çok sayıda dosya varsa, ayrıca, başlangıçta azaltıcının büyük dosyaları taşımak için paralel olarak çalışır. Ancak, iş Rüzgar başlatılırken yalnızca birkaç azaltıcının ayrılmış şekilde kalır ve büyük bir dosya için atanan tek bir eşleyiciyle takılabilir. Microsoft, Hadoop sürümleri gelecekte bu sorunu gidermek için Distcp geliştirmeleri gönderdi.  

Dikkate alınması gereken başka bir Azure Data Lake Analytics'i Data Lake Store ile kullanırken örnektir. Ayıklayıcısı tarafından yapılan işleme bağlı olarak, bölünemez bazı dosyalar (örneğin, XML, JSON) 2 GB'den büyük olduğunda performans düşebilir. Burada bir ayıklayıcısı (örneğin, CSV) dosyaları bölünebilir durumlarda büyük dosyaları tercih edilir.

### <a name="capacity-plan-for-your-workload"></a>İş yükünüz kapasite planlama 

Azure Data Lake Store, Blob Depolama hesaplarında yerleştirilir sınırları sabit GÇ kaldırır. Ancak, ele alınması gereken yine soft sınırı yoktur. Varsayılan giriş/çıkış azaltma sınırları Çoğu senaryoda karşılar. İş yükünüz artırılmış sınırlar olması gerekiyorsa, Microsoft desteği ile çalışır. Ayrıca, böylece GÇ azaltma sınırları üretim sırasında ulaşılmıyor kavram kanıtı aşamasında sınırları bakın. Bu durumda Microsoft mühendislik ekibinin el ile bir artış için bekleme gerektirebilir. GÇ azaltma meydana gelirse, Azure Data Lake Store hata kodu 429 döndürür ve ideal bir uygun üstel geri alma ilkesiyle denenmesi gerekiyor. 

### <a name="optimize-writes-with-the-data-lake-store-driver-buffer"></a>"Yazar" Data Lake Store sürücü arabellek ile en iyi duruma getirme 

Performansı iyileştirmek ve IOPS için Data Lake Store, Hadoop yazarken azaltmak için Data Lake Store sürücü arabellek boyutu olarak mümkün olduğunca yakın yazma işlemleri gerçekleştirin. Arabellek boyutu boşaltma önce gibi aşmayacak şekilde deneyin Apache Storm kullanarak akış olduğunda veya Spark akışı iş yükleri. Data Lake Store için HDInsight/Hadoop yazarken, Data Lake Store 4 MB'lık arabelleği ile bir sürücü olduğunu bilmek önemlidir. Çok sayıda dosya sistemi sürücüleri gibi 4 MB boyutuna ulaşmadan önce bu arabellek el ile boşaltılabilir. Aksi halde, sonraki yazma arabelleğin en büyük boyutu aşarsa hemen depolama alanına temizlenir. Mümkün olduğunda ilke sayısı veya zaman penceresi ile eşitleme/boşaltma zaman bir taşma ya da önemli bir eksik arabellek kaçınmalısınız.

## <a name="resiliency-considerations"></a>Dayanıklılık konuları 

Data Lake Store veya herhangi bir bulut hizmeti ile bir sistem mimarisi oluşturma, kullanılabilirlik gereksinimlerinizi ve nasıl olası kesintileri hizmetinde yanıt dikkate almanız gerekir. Bir sorun belirli bir örneğine yerelleştirilmiş veya bile bölge çapında, bu nedenle her ikisi için bir planınızın olması önemlidir. Yapılandırmanıza bağlı olarak **kurtarma süresi hedefi** ve **kurtarma noktası hedefi** SLA'ları iş yükünüz için yüksek kullanılabilirlik ve olağanüstü durum kurtarma için daha az veya fazla agresif bir strateji seçmek.

### <a name="high-availability-and-disaster-recovery"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma 

Özellikle veri söz konusu olduğunda her biraz farklı bir strateji olsa yüksek kullanılabilirlik (HA) ve olağanüstü durum kurtarma (DR) bazen birlikte birleştirilebilir. Data Lake Store, zaten yerel donanım hatalarına karşı koruma sağlamak için başlık altında 3 x çoğaltma işlemini gerçekleştirir. Ancak, bölgeler arasında çoğaltma yerleşik olmayan olduğundan, bu işlemi kendiniz yönetmeniz gerekir. Yüksek kullanılabilirlik için bir plan oluşturma sırasında bir hizmet kesintisi olması durumunda iş yükü en son verileri mümkün olduğunca hızlı bir şekilde tarafından yerel olarak veya yeni bir bölgeye ayrı olarak çoğaltılmış bir örneğine geçişi erişmesi.  

Bir DR stratejisi bir bölgede bir arıza yaşandığı nadir için hazırlamak için de farklı bir bölgeye çoğaltılan veriler önemlidir. Bu verileri başlangıçta çoğaltılmış HA verileri aynı olabilir. Ancak, gereksinimleriniz için burada geri döner için düzenli anlık görüntüleri oluşturmak isteyebilirsiniz veri bozulması gibi istisnai durumlara dikkate almanız gerekir. Önem derecesi ve verilerin boyutuna bağlı olarak, risk toleranslar göre 1, 6 ve 24 saatlik dönem delta anlık görüntülerini yerel ve/veya ikincil depolama üzerinde çalışırken göz önünde bulundurun. 

Data Lake Store ile veri dayanıklılığı için verilerinizi ideal olarak her saatte HA/DR gereksinimlerinizi karşılayan sıklığa sahip ayrı bir bölgeye coğrafi olarak çoğaltmak için tavsiye edilir. Bu çoğaltma sıklığı olabilir çok büyük veri hareketleri en aza indirir rakip performans ihtiyaçları ana sistem ile daha iyi kurtarma noktası hedefi (RPO). Ayrıca, şekilde otomatik Tetikleyiciler izleme aracılığıyla ikincil hesabına yük devretmek için Data Lake Store kullanarak uygulama için dikkate almanız gereken veya uzunluğunu girişimleri başarısız oldu ya da yöneticiler el ile müdahale için en az bir bildirim gönderin. Tekrar çevrimiçi duruma gelmesini karşı bir hizmet için bekleyen yük devretme, tradeoff olduğunu aklınızda bulundurun. Veri çoğaltma işlemi tamamlanmadı, bir yük devretme olası veri kaybı, tutarsızlık ve karmaşık veri birleştirme neden olabilir. 

Data Lake Store hesapları ve bunların her birini arasındaki temel farklılıklar arasında çoğaltma işlemlerini en çok üç önerilen seçenek aşağıdadır.


|  |Distcp  |Azure Data Factory  |AdlCopy  |
|---------|---------|---------|---------|
|**Ölçek sınırları**     | Çalışan düğümleri tarafından sınırlanan        | En büyük bulut verisi taşıma birimlerini sınırlıdır        | Analiz birimi bağlı        |
|**Deltaları kopyalamayı destekler**     |   Evet      | Hayır         | Hayır         |
|**Yerleşik düzenleme**     |  Hayır (Oozie hava akışı veya sıralanmış iş kullanın)       | Evet        | Hayır (Azure Otomasyonu veya Windows Görev Zamanlayıcısı'nı kullanın)         |
|**Desteklenen dosya sistemleri**     | ADL, HDFS, WASB, S3, GS, CFS        |Çok sayıda, bkz: [Bağlayıcılar](../data-factory/connector-azure-blob-storage.md).         | ADL için ADL, WASB için ADL (yalnızca aynı bölge)        |
|**İşletim sistemi desteği**     |Hadoop çalıştıran herhangi bir işletim sistemi         | Yok          | Windows 10         |
   

### <a name="use-distcp-for-data-movement-between-two-locations"></a>İki konum arasında veri taşıma için Distcp kullanma 

Kısaltması dağıtılmış kopya Distcp Hadoop ile birlikte gelir ve Dağıtılmış veri taşıma iki konum arasında sağlayan bir Linux komut satırı aracıdır. Data Lake Store, HDFS, WASB veya S3 iki konumda olabilir. MapReduce işleri, bu araç tüm düğümlerde ölçeğini genişletmek için bir Hadoop kümesi (örneğin, HDInsight) kullanır. Distcp özel ağ sıkıştırma Gereçleri olmadan büyük veri taşımak için en hızlı yolu olarak kabul edilir. Distcp yalnızca iki konum, tutamaçları otomatik yeniden denemeler, yanı sıra işlem dinamik ölçeklendirme deltaları güncelleştirmek için bir seçenek de sağlar. Bu yaklaşım, tek bir dizinde birçok büyük dosyayı olabilir Hive/Spark tablolar gibi şeyleri çoğaltmak için gelir ve yalnızca değiştirilen verileri kopyalamak istediğiniz son derece etkili olur. Bu nedenlerden dolayı Distcp büyük veri depoları arasında veri kopyalamak için en çok önerilen araçtır. 

İşleri kopyalama sıklığı veya veri tetikleyicileri yanı sıra, Linux cron işleri kullanarak Apache Oozie iş akışları tarafından tetiklenebilir. Yoğun çoğaltma işleri için ayarlanmış ve özellikle kopyası işleri için ölçeği ayrı bir HDInsight Hadoop kümesinin önerilir. Bu kopyası işleri kritik işlerle engellememesini sağlar. Çoğaltma yetecek kadar geniş bir sıklık üzerinde çalışıyorsa, küme bile her iş arasında kapatılabilmesi. İkincil bölgeye yük devretmeyi, başka bir küme de geri açıldığında sonra birincil Data Lake Store hesabı için yeni veri çoğaltmak için ikincil bölgedeki hazırlandığında emin emin olun. Distcp kullanma örnekleri için bkz: [kullanımı Azure depolama Blobları ile Data Lake Store arasında veri kopyalamak için Distcp](data-lake-store-copy-data-wasb-distcp.md).

### <a name="use-azure-data-factory-to-schedule-copy-jobs"></a>Azure Data Factory kopyalama iş zamanlamak için kullanabilirsiniz 

Azure Data Factory de kullanılabilir kullanarak kopyası işleri zamanlamak için bir **kopyalama etkinliği**ve hatta bir sıklık üzerinde ayarlanabilir **Kopyalama Sihirbazı'nı**. Azure Data Factory bulut verisi taşıma birimlerini (DMUs) sınırı vardır ve büyük veri iş yükleri için aktarım hızı/işlem sonunda caps aklınızda bulundurun. Ayrıca, Azure Data Factory şu anda klasörleri Hive tabloları gibi tam bir kopyasını çoğaltmak için gerektirecek şekilde Data Lake Store hesapları arasında değişim güncelleştirmeleri sunmaz. Başvurmak [kopyalama etkinliği ayarlama Kılavuzu](../data-factory/copy-activity-performance.md) Data Factory ile kopyalama hakkında daha fazla bilgi için. 

### <a name="adlcopy"></a>AdlCopy

AdlCopy yalnızca aynı bölgedeki iki Data Lake Store hesabı arasında veri kopyalamanıza olanak sağlayan bir Windows komut satırı aracıdır. AdlCopy aracı, bir tek başına veya bir Azure Data Lake Analytics hesabı, bir kopyalama işi çalıştırmak için seçeneğini sağlar. İlk olarak güçlü bir çoğaltma aksine üzerine kopyalar için oluşturulmuş olsa da, Data Lake Store hesaplarında aynı bölge içinde dağıtılmış kopyalamayı yapmak için başka bir seçenek sağlar. Güvenilirlik için tüm üretim iş yükleri için premium Data Lake Analytics seçeneği kullanmak için önerilir. Tek başına sürüm meşgul yanıtlar döndürebilir ve ölçeklendirme ve izleme sınırlıdır. 

AdlCopy Distcp gibi bir şey tarafından Azure Otomasyonu veya Windows Görev Zamanlayıcısı'nı gibi düzenlenen gerekir. Data Factory gibi ile AdlCopy yalnızca güncelleştirilmiş dosyaları kopyalama desteklemiyor ancak yeniden kopyalar ve mevcut dosyaların üzerine yaz. Daha fazla bilgi ve AdlCopy kullanma örnekleri için bkz. [veri kopyalama Azure depolama Bloblarından Data Lake Store için](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="monitoring-considerations"></a>İzleme konuları 

Data Lake Store, ayrıntılı tanılama günlükleri ve denetim sağlar. Data Lake Store, Data Lake Store hesabı altında Azure portalında ve Azure İzleyici'de bazı temel ölçümleri sağlar. Data Lake Store kullanılabilirliğini, Azure portalında görüntülenir. Ancak, bu ölçüm yedi dakikada yenilendiğini ve genel olarak sunulan bir API aracılığıyla sorgulanamıyor. Bir Data Lake Store hesabı en güncel kullanılabilirliğini almak için kullanılabilirlik doğrulamak için yapay kendi testlerinizi çalıştırmanız gerekir. Giriş/Çıkış toplam depolama alanı kullanımı ve okuma/yazma istekleri gibi diğer ölçümleri yenilemek için 24 saat sürebilir. Bu nedenle, daha fazla güncel ölçümleri el ile Hadoop komut satırı araçları veya toplama günlük bilgileri hesaplanmalıdır. En son depolama alanı kullanımı almanın en hızlı yolu, bir Hadoop küme düğümünden (örneğin, baş düğüm) bu HDFS komutu çalışıyor:   

    hdfs dfs -du -s -h adl://<adls_account_name>.azuredatalakestore.net:443/

### <a name="export-data-lake-store-diagnostics"></a>Dışarı aktarma Data Lake Store tanılama 

Data Lake Store ' aranabilir günlüklerine erişmek için hızlı yollarından biridir günlük aktarma etkinleştirmek için **Log Analytics** altında **tanılama** Data Lake Store hesabı dikey penceresinde. Bu, saat ve seçenekleri (e-posta/Web kancası) içinde 15 dakikalık aralıklarla tetiklenen uyarı yanı sıra içerik filtreleri ile gelen günlükler anında erişim sağlar. Yönergeler için [Azure Data Lake Store tanılama günlüklerine erişme](data-lake-store-diagnostic-logs.md). 

Daha fazla gerçek zamanlı uyarı verme ve günlükleri yerleşmesi nerede hakkında daha fazla denetim için Azure içeriği tek tek veya bir zaman aralığında sıraya gerçek zamanlı bildirimler göndermek için edilebilecekleri eventhub günlükleri dışarı aktarma göz önünde bulundurun. Ayrı bir uygulama gibi bir [mantıksal uygulama](../connectors/connectors-create-api-azure-event-hubs.md) ardından kullanma ve Uyarıları uygun kanala iletişim kurmak, yapabilir izleme araçları NewRelic, Datadog veya AppDynamics gibi ölçümlerini gönderin. Alternatif olarak, ElasticSearch gibi bir üçüncü taraf araç kullanıyorsanız, Blob depolama alanına günlükleri verebilir ve kullanmak [Azure Logstash eklentisini](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob) Elasticsearch, Kibana ve Logstash'i (ELK) yığın halinde veri tüketmek için.

### <a name="turn-on-debug-level-logging-in-hdinsight"></a>HDInsight hata ayıklama düzeyinde oturum açın 

Data Lake Store, günlük aktarma açık değilse Azure HDInsight ayrıca açmak için bir yol sağlar, [istemci-tarafı günlüğe kaydetme için Data Lake Store](data-lake-store-performance-tuning-mapreduce.md) log4j aracılığıyla. Aşağıdaki özelliği ayarlayın gerekir **Ambari** > **YARN** > **Config** > **yarn log4j Gelişmiş yapılandırmaları**: 

    log4j.logger.com.microsoft.azure.datalake.store=DEBUG 

Özellik ayarlanmışsa ve düğümlerin yeniden sonra Data Lake Store tanılama düğümlerinde YARN günlüklere yazılır (/tmp/<user>/yarn.log), veya önemli ayrıntıları gibi hataları'yi ve azaltma (hata kodu HTTP 429) izlenebilir. Bu bilgiyi de Log Analytics veya yerde günlükleri içeri aktarılır izlenebilir [tanılama](data-lake-store-diagnostic-logs.md) Data Lake Store hesabı dikey penceresinde. En az istemci tarafı günlük kaydı açıksa veya günlük operasyonel görünürlük ve daha kolay hata ayıklama için Data Lake Store ile seçeneğini kullanmak için önerilir.

### <a name="run-synthetic-transactions"></a>Yapay işlemlerin çalıştırılması 

Şu anda, Data Lake Store için Azure portalında hizmet kullanılabilirlik ölçümü 7 dakikalık yenileme pencere içerir. Ayrıca, genel olarak sunulan bir API kullanarak sorgulanamıyor. Bu nedenle, en fazla dakika kullanılabilirlik sağlayabilir, Data Lake Store için yapay işlemler yapan temel bir uygulama oluşturmak için önerilir. Örnek bir Web işi, mantıksal uygulama veya Azure işlev uygulaması okuma gerçekleştirmek, oluşturmak, Data Lake Store karşı ve sonuçları izleme çözümünüzü göndermek oluşturmak olabilir. İşlem geçici bir klasörde bitti ve gereksinimlerine bağlı olarak her 30-60 saniye çalışabilir test sonra silinir.

## <a name="directory-layout-considerations"></a>Dizin düzeni etkenleri

Verileri göle veri giriş, güvenlik, bölümlendirme ve işleme olabilir. böylece veri yapısı önceden planlama önemlidir etkili bir şekilde kullanılan. Azure Data Lake Store, Blob Depolama veya HDFS ile olup aşağıdaki önerilerin çoğu kullanılabilir. Her bir iş yükünün nasıl veri kullanılır, ancak IOT ile çalışırken dikkate alınması gereken bazı ortak düzenleri altındadır ve batch senaryoları farklı gereksinimleri vardır.

### <a name="iot-structure"></a>IOT yapısı 

IOT iş yüklerinde verilerin veri deposundaki çeşitli ürünleri, cihazlar, kuruluşlar ve müşteriler arasında kapsayan geldiğimizi büyük ölçüde olabilir. Kuruluşunuzun, güvenlik ve etkili bir veri işleme aşağı akış Tüketiciler için dizin düzenini önceden planlamanız gerekir. Dikkate alınması gereken bir genel şablon aşağıdaki düzen olabilir: 

    {Region}/{SubjectMatter(s)}/{yyyy}/{mm}/{dd}/{hh}/ 

Örneğin, bir uçak motorunun Birleşik Krallık içinde telemetri giriş aşağıdaki yapısı gibi görünebilir: 

    UK/Planes/BA1293/Engine1/2017/08/11/12/ 

Klasör yapısı sonunda tarih koymak için önemli bir neden yoktur. Belirli bir bölge veya konu önemli olan konuya kullanıcılar/gruplar için kilitleme istiyorsanız, daha sonra kolayca POSIX izinlerle bunu yapabilirsiniz. Yalnızca Birleşik Krallık verileri veya belirli düzlemleri görüntülemek için belirli bir güvenlik grubu kısıtlamak için bir gereksinimi varsa, aksi takdirde, tarih yapısı ile önde ayrı bir izni her saat klasör altındaki çeşitli klasörler için gerekli olacaktır. Süresi geçti gibi ek olarak, tarih yapısı önde olan katlanarak klasörlerin sayısı artırır.

### <a name="batch-jobs-structure"></a>Batch işleri yapısı 

Yüksek düzeyde, bir "içeri" klasöründe veri yerleşmesi toplu işlem yaygın olarak kullanılan bir yaklaşım olan. Ardından, veriler işlendikten sonra yeni verileri kullanmak aşağı akış işlemleri bir "dışarı" klasörüne yerleştirin. Bu dizin yapısı, tek tek dosyalarda işleme gerektiren ve yüksek düzeyde paralel işleme, büyük veri kümelerinde gerektirmeyebilecek işleri için bazen görülür. Yukarıda önerilen IOT yapısı gibi üst düzey klasörler bölge ve konu sorunları (örneğin, kuruluş, ürün/üretici) gibi şeyler için iyi bir dizin yapısı vardır. Bu yapı, iş yüklerinizi verilerinin daha iyi yönetim ve kuruluş arasında veri güvenliğini yardımcı olur. Ayrıca, tarih ve saat yapısında daha iyi bir kuruluş, filtrelenmiş aramalar, güvenlik ve Otomasyon işleme izin vermek için göz önünde bulundurun. Tarih yapısı için taneciklik düzeyini üzerinde verileri karşıya veya, gibi saatlik, günlük, ya da aylık işlenen aralığa göre belirlenir. 

Bazen dosyası işleme veri bozulma veya beklenmeyen biçimleri nedeniyle başarısız. Bu gibi durumlarda, dizin yapısını kaldırmadan yararlanabilecek bir **/hatalı** klasör daha fazla inceleme için hazırlayın. Toplu işlem, ayrıca raporlama veya bunlardan bildirim işleyebilir *hatalı* dosyaları el ile müdahale için. Aşağıdaki şablon yapısını göz önünde bulundurun: 

    {Region}/{SubjectMatter(s)}/In/{yyyy}/{mm}/{dd}/{hh}/ 
    {Region}/{SubjectMatter(s)}/Out/{yyyy}/{mm}/{dd}/{hh}/ 
    {Region}/{SubjectMatter(s)}/Bad/{yyyy}/{mm}/{dd}/{hh}/ 

Örneğin, bir pazarlama şirketi, Kuzey Amerika'da istemcilerinden müşteri güncelleştirmelerin günlük verileri ayıklayan alır. Önce ve sonra işlenen, aşağıdaki kod parçacığı gibi görünebilir: 

    NA/Extracts/ACMEPaperCo/In/2017/08/14/updates_08142017.csv 
    NA/Extracts/ACMEPaperCo/Out/2017/08/14/processed_updates_08142017.csv 
 
Toplu veri doğrudan gibi Hive veritabanları veya Geleneksel SQL veritabanlarına işlenmekte olan ortak durumda ihtiyacı yoktur bir **/in** veya **/out** girer zaten çıkış olduğundan klasör bir Hive tablosu veya dış veritabanı klasörü ayırın. Örneğin, müşterilerin günlük ayıklar, ilgili klasörleri ve düzenleme gibi Azure Data Factory, Apache Oozie tarafından kavuşmak veya Apache hava akışı işlemek ve bir Hive tablosuna verileri yazmak için bir günlük Hive veya Spark işi tetikler.

## <a name="next-steps"></a>Sonraki adımlar     

* [Azure Data Lake Store'a Genel Bakış](data-lake-store-overview.md) 
* [Azure Data Lake Store erişim denetimi](data-lake-store-access-control.md) 
* [Azure Data Lake Store güvenlik](data-lake-store-security-overview.md)
* [Azure Data Lake Store için performans ayarlama](data-lake-store-performance-tuning-guidance.md)
* [Performans ayarlama Kılavuzu, Azure Data Lake Store ile HDInsight Spark kullanma](data-lake-store-performance-tuning-spark.md)
* [Performans ayarlama Kılavuzu, Azure Data Lake Store ile HDInsight Hive kullanma](data-lake-store-performance-tuning-hive.md)
* [Azure Data Lake Store için Azure Data Factory'yi kullanarak Veri Düzenlemesi](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Data Lake Store ile HDInsight kümeleri oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md) 
