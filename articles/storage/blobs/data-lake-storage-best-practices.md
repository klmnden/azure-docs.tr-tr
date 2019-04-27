---
title: Azure Data Lake depolama Gen2 kullanmak için en iyi yöntemler | Microsoft Docs
description: Veri alımı, tarih güvenlik ve Azure Data Lake depolama Gen2'ye (daha önce Azure Data Lake Store da bilinir) kullanımıyla ilgili performans ile ilgili en iyi uygulamaları öğrenin
services: storage
author: sachinsbigdata
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: article
ms.date: 12/06/2018
ms.author: sachins
ms.openlocfilehash: e371ac848eff0e66390fe17bc23934725fca35f9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60708541"
---
# <a name="best-practices-for-using-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2 kullanmak için en iyi uygulamalar

Bu makalede, en iyi uygulamalar ve Azure Data Lake depolama 2. nesil ile çalışma konuları hakkında bilgi edinin. Bu makale, güvenlik, performans, dayanıklılık ve Data Lake depolama 2. nesil için izlemeyi geçici olarak bilgi sağlar. Data Lake depolama Gen2 önce Azure HDInsight gibi hizmetleri gerçekten büyük verilerle çalışma karmaşıktı. Böylece Petabayt depolama ve uygun ölçekte, en iyi performans elde edilebilir parça verileri birden çok Blob Depolama hesabı arasında vardı. Data Lake depolama 2. nesil ile Boyut ve performans için sabit sınırlar çoğunu kaldırılır. Ancak, yine de Data Lake depolama Gen2 ile'en iyi performansı elde etmek, bu makalede kapsayan bazı noktalar vardır.

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Azure Data Lake depolama Gen2, Azure Active Directory (Azure AD) kullanıcıları, grupları ve hizmet sorumluları için POSIX erişim denetimleri sağlar. Bu erişim denetimleri, var olan dosyalar ve dizinler için ayarlanabilir. Erişim denetimleri otomatik olarak yeni dosyalar veya dizinler için uygulanabilir varsayılan izinleri oluşturmak için de kullanılabilir. Data Lake depolama Gen2 ACL'ler hakkında daha fazla ayrıntı kullanılabilir [Azure Data Lake depolama Gen2'deki erişim denetimi](storage-data-lake-storage-access-control.md).

### <a name="use-security-groups-versus-individual-users"></a>Bireysel kullanıcılar ve güvenlik grupları kullanın

Data Lake depolama Gen2 büyük verilerle çalışmaya her yerindeki olası bir hizmet sorumlusu verilerle çalışmak için Azure HDInsight gibi hizmetlerin izin vermek için kullanılır. Bununla birlikte, burada bireysel kullanıcılar verileri de erişmesi gereken durumlar olabilir. Her durumda, Azure Active Directory'yi kullanarak türü kesin düşünün [güvenlik grupları](../common/storage-auth-aad.md) dizinler ve dosyalar için tek tek kullanıcı atama yerine.

Bir güvenlik grubu izinlerini atandıktan sonra ekleme veya kullanıcıları gruptan kaldırma Data Lake depolama Gen2'ye herhangi bir güncelleştirme gerektirmez. Bu, aynı zamanda erişim denetimi girdileri erişim denetim listesi (ACL) başına en fazla sayısını aşmamak sağlamaya yardımcı olur. Şu anda bu (her zaman her dosya ve dizin ile ilişkili olan dört POSIX stili ACL'leri dahil), 32 sayıdır: sahip olan kullanıcı, sahip olan Grup, maske ve diğer. Her dizin ACL, erişim ACL'si ve varsayılan iki türde olabilir, 64 erişim denetimi girdileri toplam ACL. Bu ACL'ler hakkında daha fazla bilgi için bkz: [Azure Data Lake depolama Gen2'deki erişim denetimi](data-lake-storage-access-control.md).

### <a name="security-for-groups"></a>Güvenlik grupları

Sizin veya kullanıcılarınızın bir depolama hesabındaki verilere erişimin etkin hiyerarşik ad alanı ile gerektiğinde Azure Active Directory güvenlik gruplarını kullanmak en iyisidir. Bazı gruplar ile başlaması olabilir önerilen **ReadOnlyUsers**, **WriteAccessUsers**, ve **FullAccessUsers** için kök dosya sisteminin ve hatta ayırın anahtar alt dizinleri. Varsa diğer daha sonra eklenen, ancak değil tanımlanan kullanıcı henüz, beklenen grupları, belirli klasörlere erişimi işlevsiz güvenlik grupları oluşturmayı düşünebilirsiniz. Güvenlik grubu kullanarak sağlar, uzun işleme süresi önleyebilirsiniz binlerce dosya için yeni izinleri atarken.

### <a name="security-for-service-principals"></a>Hizmet sorumluları için güvenlik

Azure Active Directory Hizmet sorumluları, genellikle Data Lake depolama Gen2 verilerine erişim için Azure Databricks gibi hizmetleri tarafından kullanılır. Birçok müşteri, tek bir Azure Active Directory Hizmet sorumlusu yeterli olabilir ve Data Lake depolama 2. nesil dosya sistemini kökünde tam izinlere sahip olabilir. Diğer müşteriler, bir küme veri ve yalnızca okuma erişimi olan başka bir küme tam erişime sahip olduğu farklı hizmet sorumluları ile birden fazla küme gerektirebilir. 

### <a name="enable-the-data-lake-storage-gen2-firewall-with-azure-service-access"></a>Data Lake depolama 2. nesil güvenlik duvarı ile Azure hizmet erişimini etkinleştir

Data Lake depolama Gen2'ye bir Güvenlik Duvarı'nı ve Azure Hizmetleri için yalnızca dış saldırı vektörü sınırlanması önerilir erişimi sınırlandırma seçeneğini destekler. Güvenlik Duvarı Azure portalında bir depolama hesabı üzerinde etkinleştirilebilir **Güvenlik Duvarı** > **etkinleştir Güvenlik Duvarı (açık)** > **Azurehizmetlerineerişimeizinver** seçenekleri.

Azure Databricks kümeleri depolama güvenlik duvarı erişmesi için izin verilmiyor olabilir bir sanal ağa ekleme, bir önizleme özelliği Databricks kullanılmasını gerektirir. Bu özelliği etkinleştirmek için lütfen bir destek isteği yerleştirin.

## <a name="resiliency-considerations"></a>Dayanıklılık konuları

Data Lake depolama Gen2'ye veya herhangi bir bulut hizmeti ile bir sistem mimarisi oluşturma, kullanılabilirlik gereksinimlerinizi ve nasıl olası kesintileri hizmetinde yanıt dikkate almanız gerekir. Bir sorun belirli bir örneğine yerelleştirilmiş veya bile bölge çapında, bu nedenle her ikisi için bir planınızın olması önemlidir. İş yükünüz, SLA hedefini bağlı olarak, Kurtarma süresi hedefi ve kurtarma noktası yüksek kullanılabilirlik ve olağanüstü durum kurtarma için daha az veya fazla agresif bir stratejisi seçebilirsiniz.

### <a name="high-availability-and-disaster-recovery"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma

Özellikle veri söz konusu olduğunda her biraz farklı bir strateji olsa yüksek kullanılabilirlik (HA) ve olağanüstü durum kurtarma (DR) bazen birlikte birleştirilebilir. Data Lake depolama Gen2 yerel donanım hatalarına karşı koruma sağlamak için başlık altında 3 x çoğaltma işlemektedir. Ayrıca, ZRS gibi diğer çoğaltma seçenekleri GRS sırasında HA geliştirmek & RA-GRS DR geliştirmek. Yüksek kullanılabilirlik için bir plan oluşturma sırasında bir hizmet kesintisi olması durumunda iş yükü en son verileri mümkün olduğunca hızlı bir şekilde tarafından yerel olarak veya yeni bir bölgeye ayrı olarak çoğaltılmış bir örneğine geçişi erişmesi.

Bir DR stratejisi bir bölgede bir arıza yaşandığı nadir için hazırlamak için de GRS veya RA-GRS çoğaltma kullanarak farklı bir bölgeye çoğaltılan veriler önemlidir. Gereksinimleriniz için burada geri döner için düzenli anlık görüntüleri oluşturmak isteyebilirsiniz veri bozulması gibi istisnai durumlara de dikkate almanız gerekir. Önem derecesi ve verilerin boyutuna bağlı olarak, risk toleranslar göre 1, 6 ve 24 saatlik dönem delta anlık görüntülerini çalışırken göz önünde bulundurun.

Data Lake depolama Gen2 ile veri dayanıklılığı için verilerinizi GRS veya HA/DR gereksinimlerinizi karşılayan RA-GRS ile coğrafi olarak çoğaltmak için tavsiye edilir. Ayrıca, şekilde otomatik Tetikleyiciler izleme aracılığıyla ikincil bölgeye yük devretmek için Data Lake depolama Gen2'ı kullanarak uygulama için dikkate almanız gereken veya uzunluğunu girişimleri başarısız oldu ya da yöneticiler el ile müdahale için en az bir bildirim gönderin. Tekrar çevrimiçi duruma gelmesini karşı bir hizmet için bekleyen yük devretme, tradeoff olduğunu aklınızda bulundurun.

### <a name="use-distcp-for-data-movement-between-two-locations"></a>İki konum arasında veri taşıma için Distcp kullanma

Kısaltması dağıtılmış kopya DistCp Hadoop ile birlikte gelir ve Dağıtılmış veri taşıma iki konum arasında sağlayan bir Linux komut satırı aracıdır. Data Lake depolama Gen2, HDFS veya S3 iki konumda olabilir. MapReduce işleri, bu araç tüm düğümlerde ölçeğini genişletmek için bir Hadoop kümesi (örneğin, HDInsight) kullanır. Distcp özel ağ sıkıştırma Gereçleri olmadan büyük veri taşımak için en hızlı yolu olarak kabul edilir. Distcp yalnızca iki konum, tutamaçları otomatik yeniden denemeler, yanı sıra işlem dinamik ölçeklendirme deltaları güncelleştirmek için bir seçenek de sağlar. Bu yaklaşım, tek bir dizinde birçok büyük dosyayı olabilir Hive/Spark tablolar gibi şeyleri çoğaltmak için gelir ve yalnızca değiştirilen verileri kopyalamak istediğiniz son derece etkili olur. Bu nedenlerden dolayı Distcp büyük veri depoları arasında veri kopyalamak için en çok önerilen araçtır.

İşleri kopyalama sıklığı veya veri tetikleyicileri yanı sıra, Linux cron işleri kullanarak Apache Oozie iş akışları tarafından tetiklenebilir. Yoğun çoğaltma işleri için ayarlanmış ve özellikle kopyası işleri için ölçeği ayrı bir HDInsight Hadoop kümesinin önerilir. Bu kopyası işleri kritik işlerle engellememesini sağlar. Çoğaltma yetecek kadar geniş bir sıklık üzerinde çalışıyorsa, küme bile her iş arasında kapatılabilmesi. İkincil bölgeye yük devretmeyi, başka bir küme de geri açıldığında sonra birincil Data Lake depolama Gen2 hesap için yeni veri çoğaltmak için ikincil bölgedeki hazırlandığında emin emin olun. Distcp kullanma örnekleri için bkz: [kullanımı Azure depolama Blobları ile Data Lake depolama 2. nesil arasında veri kopyalamak için Distcp](../blobs/data-lake-storage-use-distcp.md).

### <a name="use-azure-data-factory-to-schedule-copy-jobs"></a>Azure Data Factory kopyalama iş zamanlamak için kullanabilirsiniz

Azure Data Factory kopyalama etkinliğini kullanarak kopyası işleri zamanlamak için de kullanılabilir ve hatta Kopyalama Sihirbazı aracılığıyla bir sıklık üzerinde ayarlanabilir. Azure Data Factory bulut verisi taşıma birimlerini (DMUs) sınırı vardır ve büyük veri iş yükleri için aktarım hızı/işlem sonunda caps aklınızda bulundurun. Ayrıca, Azure Data Factory şu anda dizinleri Hive tabloları gibi tam bir kopyasını çoğaltmak için gerektirecek şekilde Data Lake depolama Gen2 hesapları arasında değişim güncelleştirmeleri sunmaz. Başvurmak [data factory makale](../../data-factory/load-azure-data-lake-storage-gen2.md) Data Factory ile kopyalama hakkında daha fazla bilgi için.

## <a name="monitoring-considerations"></a>İzleme konuları

Data Lake depolama Gen2, Data Lake depolama Gen2 hesabı altında Azure portalında ve Azure İzleyici ölçümleri sağlar. Data Lake depolama Gen2 kullanılabilirliğini, Azure portalında görüntülenir. Bir Data Lake depolama Gen2 hesabı en güncel kullanılabilirliğini almak için kullanılabilirlik doğrulamak için yapay kendi testlerinizi çalıştırmanız gerekir. Toplam depolama alanı kullanımı gibi diğer ölçümleri okuma/yazma istekleri ve giriş/çıkış uygulamaları izleyerek havuzlamanızı kullanılabilir ve (örneğin, ortalama gecikme süresi veya dakika başına hatalarının sayısı) eşikler aşıldığında uyarılar tetikleyebilirsiniz.

## <a name="directory-layout-considerations"></a>Dizin düzeni etkenleri

Verileri göle veri giriş, güvenlik, bölümlendirme ve işleme olabilir. böylece veri yapısı önceden planlama önemlidir etkili bir şekilde kullanılan. Aşağıdaki önerilerin çoğu, tüm büyük veri iş yükleri için geçerlidir. Her bir iş yükünün nasıl veri kullanılır, ancak IOT ile çalışırken dikkate alınması gereken bazı ortak düzenleri altındadır ve batch senaryoları farklı gereksinimleri vardır.

### <a name="iot-structure"></a>IOT yapısı

IOT iş yüklerinde verilerin veri deposundaki çeşitli ürünleri, cihazlar, kuruluşlar ve müşteriler arasında kapsayan geldiğimizi büyük ölçüde olabilir. Kuruluşunuzun, güvenlik ve etkili bir veri işleme aşağı akış Tüketiciler için dizin düzenini önceden planlamanız gerekir. Dikkate alınması gereken bir genel şablon aşağıdaki düzen olabilir:

    {Region}/{SubjectMatter(s)}/{yyyy}/{mm}/{dd}/{hh}/

Örneğin, bir uçak motorunun Birleşik Krallık içinde telemetri giriş aşağıdaki yapısı gibi görünebilir:

    UK/Planes/BA1293/Engine1/2017/08/11/12/

Dizin yapısı sonunda tarih koymak için önemli bir neden yoktur. Belirli bir bölge veya konu önemli olan konuya kullanıcılar/gruplar için kilitleme istiyorsanız, daha sonra kolayca POSIX izinlerle bunu yapabilirsiniz. Yalnızca Birleşik Krallık verileri veya belirli düzlemleri görüntülemek için belirli bir güvenlik grubu kısıtlamak için bir gereksinimi varsa, aksi takdirde, tarih yapı ile önde ayrı bir izni her saat dizini altındaki çeşitli dizinler için gerekli olacaktır. Süresi geçti gibi ek olarak, tarih yapısı önde olan katlanarak dizinleri sayısını artırır.

### <a name="batch-jobs-structure"></a>Batch işleri yapısı

Bir üst düzey, toplu işlem yaygın olarak kullanılan bir yaklaşım bir "içeri" dizininde veri kavuşmak sağlamaktır. Ardından, veriler işlendikten sonra yeni verileri bir "dışarı olmak üzere" kullanmak aşağı akış işlemleri için dizin yerleştirin. Bu dizin yapısı, tek tek dosyalarda işleme gerektiren ve yüksek düzeyde paralel işleme, büyük veri kümelerinde gerektirmeyebilecek işleri için bazen görülür. Yukarıda önerilen IOT yapısı gibi üst düzey dizinleri bölge ve konu sorunları (örneğin, kuruluş, ürün/üretici) gibi şeyler için iyi dizin yapısı vardır. Bu yapı, iş yüklerinizi verilerinin daha iyi yönetim ve kuruluş arasında veri güvenliğini yardımcı olur. Ayrıca, tarih ve saat yapısında daha iyi bir kuruluş, filtrelenmiş aramalar, güvenlik ve Otomasyon işleme izin vermek için göz önünde bulundurun. Tarih yapısı için taneciklik düzeyini üzerinde verileri karşıya veya, gibi saatlik, günlük, ya da aylık işlenen aralığa göre belirlenir.

Bazen dosyası işleme veri bozulma veya beklenmeyen biçimleri nedeniyle başarısız. Bu gibi durumlarda, dizin yapısını kaldırmadan yararlanabilecek bir **/hatalı** klasör daha fazla inceleme için hazırlayın. Toplu işlem, ayrıca raporlama veya bunlardan bildirim işleyebilir *hatalı* dosyaları el ile müdahale için. Aşağıdaki şablon yapısını göz önünde bulundurun:

    {Region}/{SubjectMatter(s)}/In/{yyyy}/{mm}/{dd}/{hh}/
    {Region}/{SubjectMatter(s)}/Out/{yyyy}/{mm}/{dd}/{hh}/
    {Region}/{SubjectMatter(s)}/Bad/{yyyy}/{mm}/{dd}/{hh}/

Örneğin, bir pazarlama şirketi, Kuzey Amerika'da istemcilerinden müşteri güncelleştirmelerin günlük verileri ayıklayan alır. Önce ve sonra işlenen, aşağıdaki kod parçacığı gibi görünebilir:

    NA/Extracts/ACMEPaperCo/In/2017/08/14/updates_08142017.csv
    NA/Extracts/ACMEPaperCo/Out/2017/08/14/processed_updates_08142017.csv

Toplu veri doğrudan gibi Hive veritabanları veya Geleneksel SQL veritabanlarına işlenmekte olan ortak durumda ihtiyacı yoktur bir **/in** veya **/out** girer zaten çıkış olduğundan klasör bir Hive tablosu veya dış veritabanı klasörü ayırın. Örneğin, müşterilerin günlük ayıklar, ilgili klasörleri ve düzenleme gibi Azure Data Factory, Apache Oozie tarafından kavuşmak veya Apache hava akışı işlemek ve bir Hive tablosuna verileri yazmak için bir günlük Hive veya Spark işi tetikler.
