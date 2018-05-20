---
title: En iyi uygulamalar Azure Data Lake Store kullanma | Microsoft Docs
description: En iyi yöntemleri veri alımı, tarih güvenlik ve Azure Data Lake Store kullanımıyla ilgili performans hakkında bilgi edinin
services: data-lake-store
documentationcenter: ''
author: sachinsbigdata
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 03/02/2018
ms.author: sachins
ms.openlocfilehash: ac0a01ed7a067688732aa54eb1b76e0e299e4263
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="best-practices-for-using-azure-data-lake-store"></a>Azure Data Lake Store kullanmak için en iyi uygulamalar
Bu makalede, en iyi yöntemler ve Azure Data Lake Store ile çalışma konuları hakkında bilgi edinin. Bu makale, güvenlik, performans, dayanıklılık ve Data Lake Store için izleme bilgileri sağlar. Data Lake Store önce Azure Hdınsight gibi hizmetler gerçekten büyük verilerle çalışmak karmaşıktı. Böylece Petabayt depolama ve bu ölçekte en iyi performansı elde edilebilir birden çok Blob Depolama hesapları arasında parça veri içeriyor. Data Lake Store ile boyutu ve performans için sabit sınırları çoğunu kaldırılır. Ancak, yine bu makalede yer almaktadır ve böylece Data Lake Store ile en iyi performansı elde edebilirsiniz bazı noktalar vardır. 

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Azure Data Lake Store POSIX erişim denetimleri ve Azure Active Directory (Azure AD) kullanıcıları, grupları ve hizmet asıl adı için Denetim ayrıntılı sunar. Bu erişim denetimlerini, varolan dosya ve klasörler için ayarlanabilir. Erişim denetimlerini, yeni dosyalar veya klasörler için uygulanabilir Varsayılanları oluşturmak için de kullanılabilir. İzinler mevcut klasörleri ve alt nesneleri için ayarladığınızda, izinleri her nesneyi yayılan yinelemeli olmanız gerekir. Çok sayıda dosya izinleri yayılıyor, uzun bir zaman var. alabilir Geçen süre, saniye başına işlenen 30-50 nesneler arasında değişebilir. Bu nedenle, klasör yapısı ve kullanıcı grupları uygun şekilde planlayın. Verilerinizle çalışırken Aksi takdirde, beklenmeyen gecikmeler ve sorunları neden olabilir. 

100.000 alt nesneleri içeren bir klasör olduğunu varsayın. Tüm klasör izinlerini güncelleştirmek için saniye başına işlenen 30 nesnelerin alt sınır izlerseniz, bir saat sürebilir. Veri Gölü deposu ACL'ler hakkında daha fazla ayrıntı bulunabilir [erişim denetimi Azure Data Lake Store'da](data-lake-store-access-control.md). ACL'ler yinelemeli atama iyileştirilmiş performans için Azure Data Lake komut satırı aracını kullanabilirsiniz. Aracı birden çok iş parçacığı ve hızla milyonlarca dosya için ACL uygulamak için özyinelemeli Gezinti mantığı oluşturur. Linux ve Windows için kullanılabilir bir araçtır ve [belgelerine](https://github.com/Azure/data-lake-adlstool) ve [indirmeleri](http://aka.ms/adlstool-download) bu araç, GitHub üzerinde bulunabilir. Bu aynı performans iyileştirmeleri, Data Lake Store ile yazılmış kendi araçları tarafından etkinleştirilebilir [.NET](data-lake-store-data-operations-net-sdk.md) ve [Java](data-lake-store-get-started-java-sdk.md) SDK'ları.

### <a name="use-security-groups-versus-individual-users"></a>Tek tek kullanıcılar yerine güvenlik grupları kullanın 

Olasılıkla büyük veri Data Lake Store'da ile çalışırken, bir hizmet sorumlusu verilerle çalışmak için Azure Hdınsight gibi hizmetleri izin vermek için kullanılır. Ancak, burada bireysel kullanıcılar verileri de erişmesi gereken durumlar olabilir. Böyle durumlarda, Azure Active Directory kullanmalısınız [güvenlik grupları](data-lake-store-secure-data.md#create-security-groups-in-azure-active-directory) klasörleri ve dosyaları için tek tek kullanıcılara atamak yerine. 

Bir güvenlik grubu izinleri atandığında, kullanıcı ekleme veya gruptan kaldırma Data Lake Store herhangi bir güncelleştirme gerektirmez. Bu ayrıca sınırını aşan yok sağlamaya yardımcı olur [32 erişim ve varsayılan ACL'leri](../azure-subscription-service-limits.md#data-lake-store-limits) (Bu her zaman her dosya ve klasör ile ilişkili olan dört POSIX tipi ACL'leri içerir: [sahibi olan kullanıcı](data-lake-store-access-control.md#the-owning-user), [sahibi olan grup](data-lake-store-access-control.md#the-owning-group), [maskesi](data-lake-store-access-control.md#the-mask-and-effective-permissions)ve diğer).

### <a name="security-for-groups"></a>Güvenlik grupları 

Data Lake Store'a erişmek kullanıcılar gerektiğinde açıklandığı gibi Azure Active Directory güvenlik grupları kullanmak en iyisidir. Bazı gruplar başlaması olabilir önerilen **ReadOnlyUsers**, **WriteAccessUsers**, ve **FullAccessUsers** olanları anahtarı için için kök hesabının ve hatta ayırın alt klasörler. Varsa diğer, daha sonra eklenen, ancak değil tanımlanan kullanıcılar henüz, beklenen grupları belirli klasörlere erişimi kukla güvenlik grupları oluşturma düşünebilirsiniz. Güvenlik grubu kullanmanızı sağlar daha sonra bir uzun gerekmediği binlerce dosya için yeni izinler atamak için işleme süresi. 

### <a name="security-for-service-principals"></a>Hizmet sorumluları için güvenlik 

Azure Active Directory hizmet asıl adı, genellikle Azure Hdınsight gibi hizmetler Data Lake Store'da verilere erişmek için kullanılır. Birden çok iş yükünün üzerinden erişim gereksinimlerine bağlı olarak, içinde ve dışında kuruluşun güvenliğini sağlamak için bazı noktalar olabilir. Birçok müşteri için tek bir Azure Active Directory Hizmet sorumlusu yeterli olabilir ve Data Lake Store'un kökünde tam izinleri olabilir. Diğer müşteriler, bir küme verileri hem yalnızca okuma erişimi olan başka bir küme tam erişime sahip olduğu farklı hizmet asıl adı ile birden çok küme gerektirebilir. Güvenlik gruplarıyla olduğu gibi bir Data Lake Store hesabı oluşturulduktan sonra her senaryo (okuma, yazma, tam) beklenen için bir hizmet sorumlusu yapmayı düşünebilirsiniz. 

### <a name="enable-the-data-lake-store-firewall-with-azure-service-access"></a>Azure hizmet erişimi olan Data Lake Store güvenlik duvarını etkinleştir 

Data Lake Store bir Güvenlik Duvarı'nı ve Azure Hizmetleri için yalnızca küçük bir saldırı vektörü dış yetkisiz erişimlere için önerilen erişimi sınırlandırma seçeneğini destekler. Güvenlik Duvarı Azure Portal'daki Data Lake Store hesabındaki etkinleştirilebilir **Güvenlik Duvarı** > **etkinleştir Güvenlik Duvarı'nı (açık)** > **Azure hizmetlerine erişime izin ver**  seçenekleri.  

![Güvenlik Duvarı ayarları Data Lake Store'da](./media/data-lake-store-best-practices/data-lake-store-firewall-setting.png "Güvenlik Duvarı ayarları Data Lake Store'da")

Güvenlik Duvarı etkinleştirilmişse, yalnızca Azure Hizmetleri Hdınsight gibi eklendiğinde, veri fabrikası, SQL Data Warehouse, vb. için Data Lake Store erişimi. Azure tarafından kullanılan iç ağ adresi çevirisi nedeniyle Data Lake Store Güvenlik Duvarı'nı belirli hizmetleri tarafından IP kısıtlama desteklemez ve yalnızca Azure dışında uç noktaları gibi şirket içi kısıtlamalarını yöneliktir. 

## <a name="performance-and-scale-considerations"></a>Performansı ve ölçeği dikkat edilecek noktalar

Data Lake Store en güçlü özelliklerini veri akışındaki sabit sınırlara kaldırır biridir. Sınırları kaldırma, müşterilerin kendi veri boyutu büyümesine olanak verir ve performans gereksinimlerini parça için verileri gerek kalmadan eşlik. Data Lake Store performansını iyileştirmek için en önemli konular, en iyi paralellik olduğunda verilen gerçekleştirir biridir.

### <a name="improve-throughput-with-parallelism"></a>Paralellik ile üretilen işi artırmak 

Çekirdek başına 8-12 iş parçacığı için en iyi okuma/yazma işleme vermeye çalışın. Bu tek bir iş parçacığı üzerinde engelleme okuma/yazma nedeniyle ve daha fazla iş parçacığı yüksek eşzamanlılık VM izin verebilirsiniz. Düzeyleri sağlıklı olduğunu ve paralellik artırılabilir gerçekleştirmek için sanal makinenin CPU kullanımı izlemek emin olun.   

### <a name="avoid-small-file-sizes"></a>Küçük dosya boyutunu kaçının

POSIX izinleri ve Data Lake Store'da denetimi ile birlikte gelen çok sayıda küçük dosyalarıyla çalışırken görünür hale bir yükü. En iyi uygulama, binlerce veya küçük dosyalar milyonlarca Data Lake Store'a yazma karşı daha büyük dosyalar halinde verilerinizi toplu gerekir. Küçük dosya boyutunu önleme birden çok avantajları gibi olabilir:

* Kimlik doğrulama denetimleri arasında birden çok dosya indirme
* Açık dosya bağlantılarını azaltılmış
* Daha hızlı kopyalama/çoğaltma
* Veri Gölü deposu POSIX izinleri güncelleştirilirken işlemek için daha az sayıda dosya 

Hangi Hizmetleri ve iş yüklerini veri kullanıyor bağlı olarak, dosya boyutları için dikkate alınması gereken iyi bir aralığı 1 GB, ideal olarak 100 MB altında veya üstünde 2 GB giderek değil 256 MB'tır. Dosya boyutları, Data Lake Store'da giriş olduğunda toplu olamaz, bu dosyalar daha büyük olanları birleştirir ayrı düzenleme iş olabilir. Daha fazla bilgi ve öneri dosya boyutları ve Data Lake Store'da verileri düzenlemek için bkz: [Veri kümenizi yapısı](data-lake-store-performance-tuning-guidance.md#structure-your-data-set). 

### <a name="large-file-sizes-and-potential-performance-impact"></a>Büyük dosya boyutlarına ve olası performans etkisini 

Data Lake Store, Petabayt büyük dosya boyutu, en iyi performans için ve verileri okuma işlemi bağlı olarak desteklese 2 GB ortalama gitmek ideal olmayabilir. Örneğin, kullanırken **Distcp'yi** konumları ve farklı depolama hesapları arasında veri kopyalamak için harita görevleri belirlemek için kullanılan ayrıntı düzeyi en iyi düzeyde dosyalarıdır. Bu nedenle, her biri 1 TB olan 10 dosyaları kopyalıyorsanız en fazla 10 mappers ayrılır. Atanan mappers ile çok sayıda dosya varsa, ayrıca, başlangıçta mappers büyük dosyaların taşınacağı paralel olarak çalışır. Ancak, iş Rüzgar başlatılırken yalnızca birkaç mappers ayrılmış kalır ve büyük bir dosya atanan tek eşleyiciyle kalmış. Microsoft, Hadoop sürümleri gelecekte bu sorunu gidermek için Distcp'yi geliştirmeleri gönderdi.  

Azure Data Lake Analytics'i Data Lake Store ile kullanırken dikkate alınması gereken başka bir örnek verilebilir. Ayıklayıcısı tarafından yapılan işleme bağlı olarak bölünemez bazı dosyalar (örneğin, XML, JSON) 2 GB'den büyük olduğunda performans düşebilir. Burada bir ayıklayıcısı (örneğin, CSV) dosyaları bölünebilir durumlarda, büyük dosyalar tercih edilir.

### <a name="capacity-plan-for-your-workload"></a>İş yükünüzün kapasite planlama 

Azure Data Lake Store Blob Depolama hesaplarında yerleştirilir sınırları sabit GÇ kaldırır. Ancak, ele alınması gereken hala esnek sınırlar vardır. Varsayılan giriş/çıkış azaltma sınırları Çoğu senaryoda ihtiyaçlarını karşılamak. İş yükünüzün artan sınırları olması gerekiyorsa, Microsoft desteği ile çalışır. Böylece GÇ azaltma sınırları üretim sırasında isabet değil de, kavram kanıtı aşaması sırasında sınırları bakın. Bu durumda, onu Microsoft mühendislik ekibinin el ile bir artış bekliyor gerektirebilir. G/ç azaltma meydana gelirse, Azure Data Lake Store 429 hata kodunu döndürür ve ideal bir uygun üstel geri alma İlkesi ile yeniden denenmeli. 

### <a name="optimize-writes-with-the-data-lake-store-driver-buffer"></a>"Yazar" Data Lake Store sürücü arabellekle en iyi duruma getirme 

Performansı iyileştirmek ve Data Lake Store'a Hadoop'tan yazılırken IOPS azaltmak için Data Lake Store sürücü arabellek boyutu olarak mümkün olduğunca yakın yazma işlemleri gerçekleştirin. Arabellek boyutu temizleme önce gibi aşmayacak şekilde deneyin Apache Storm kullanarak akış veya iş yükleri Spark akış. Data Lake Store'a Hdınsight/Hadoop'tan yazarken, Data Lake Store 4 MB arabelleği sürücüsüyle olduğunu bilmek önemlidir. Çok sayıda dosya sistemi sürücülerinin gibi 4 MB boyutuna ulaşmadan önce bu arabellek el ile boşaltılabilir. Aksi halde, sonraki yazma arabellek sınırını aşarsa hemen depolama birimine temizlenir. Mümkünse, eşitleme/temizleme İlkesi sayısı veya zaman penceresi tarafından olduğunda bir taşması veya önemli eksik arabellek kaçınmalısınız.

## <a name="resiliency-considerations"></a>Dayanıklılık konuları 

Data Lake Store veya herhangi bir bulut hizmeti ile bir sistem mimarisi oluşturma, kullanılabilirlik gereksinimlerinizi ve nasıl potansiyel kesintiler hizmet yanıt dikkate almanız gerekir. Bir sorun belirli örneğine yerelleştirilmiş veya bile bölge genelinde, bu nedenle bir planı her ikisi için sahip olmak önemlidir. Bağlı olarak **kurtarma süresi hedefi** ve **kurtarma noktası hedefi** SLA'ları, iş yükü için yüksek kullanılabilirlik ve olağanüstü durum kurtarma için fazla veya az agresif bir stratejisi seçin.

### <a name="high-availability-and-disaster-recovery"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma 

Özellikle veri geldiğinde her biraz farklı bir strateji olsa yüksek kullanılabilirlik (HA) ve olağanüstü durum kurtarma (DR) bazen, birleştirilebilir. Data Lake Store zaten 3 x çoğaltma yerelleştirilmiş donanım arızalarına karşı koruma sağlamak için başlık altında işler. Ancak, bölgeler arasında çoğaltma yerleşik değil olduğundan bunu kendiniz yönetmeniz gerekir. Bir planı için HA oluştururken, bir hizmet kesintisi olması durumunda iş yükü en son verileri mümkün olduğunca hızlı bir şekilde tarafından yerel olarak veya yeni bir bölgede ayrı olarak çoğaltılmış bir örneğine geçişi erişmesi.  

Bir DR stratejisi olası bir bölgenin yıkıcı bir hata olayı için hazırlamak için ayrıca farklı bir bölgeye çoğaltılmış verileri sağlamak önemlidir. Bu veriler başlangıçta çoğaltılan HA verileri aynı olmayabilir. Ancak, burada geri döner için düzenli anlık görüntüleri oluşturmak isteyebilirsiniz veri bozulması gibi kenar durumlarda gereksinimlerinizi dikkate almanız gerekir. Önem ve verilerin boyutuna bağlı olarak, risk toleranslar göre yerel ve/veya ikincil deposu delta anlık görüntüleri 1, 6 ve 24 saatlik dönem çalışırken göz önünde bulundurun. 

Data Lake Store ile veri dayanıklılığı için ideal olarak her saat HA/DR gereksinimlerinizi karşılayan sıklık ayrı bir bölge için verilerinizi coğrafi çoğaltılması için önerilir. Bu çoğaltma sıklığı olan büyük veri hareketleri en aza indirir ana sistem ve daha iyi bir kurtarma noktası hedefi (RPO) ile rakip işleme gerekir. Ayrıca, yolları otomatik olarak Tetikleyicileri izleme aracılığıyla ikincil hesap devredecek Data Lake Store kullanarak uygulama için göz önünde bulundurmanız gerekir veya uzunluğu denemesi başarısız oldu veya Yöneticiler el ile müdahale için en az bir bildirim gönderin. Tekrar çevrimiçi duruma karşı bir hizmet için bekleyen yapabilmesini kolaylığını olduğunu aklınızda bulundurun. Veri çoğaltma işlemi tamamlanmadı, bir yük devretme olası veri kaybı, tutarsızlık veya karmaşık verilerinin birleştirme neden olabilir. 

Aşağıdaki Data Lake Store hesapları ve bunların her birini arasındaki temel farklılıklar arasında çoğaltmayı yönetme için üst üç önerilen seçenek vardır.


|  |Distcp'yi  |Azure Data Factory  |AdlCopy  |
|---------|---------|---------|---------|
|**Ölçek sınırları**     | Alt düğümler tarafından sınırlanmış        | En fazla bulut veri taşıma birimleri sınırlıdır        | Analytics birimlerle bağlı        |
|**Farkları kopyalama destekler**     |   Evet      | Hayır         | Hayır         |
|**Yerleşik düzenleme**     |  Hayır (Oozie hava akışı veya cron işleri kullanın)       | Evet        | Hayır (Azure Otomasyonu veya Windows Görev Zamanlayıcısı'nı kullanın)         |
|**Desteklenen dosya sistemleri**     | ADL, HDFS, WASB, S3, GS, CFS        |Çok sayıda, bkz: [Bağlayıcılar](../data-factory/connector-azure-blob-storage.md).         | ADL, ADL (yalnızca aynı bölge) WASB ADL        |
|**İşletim sistemi desteği**     |Hadoop çalıştıran herhangi bir işletim sistemi         | Yok          | Windows 10         |
   

### <a name="use-distcp-for-data-movement-between-two-locations"></a>İki konum arasında veri taşıma için Distcp'yi kullanın 

Kısaltması dağıtılmış kopya Distcp'yi Hadoop ile gelir ve iki konum arasında Dağıtılmış veri taşıma sağlayan bir Linux komut satırı aracıdır. İki konum Data Lake Store, HDFS, WASB veya S3 olabilir. Bu araç MapReduce işleri tüm düğümlerde genişletilecek Hadoop kümesinde (örneğin, Hdınsight) kullanır. Distcp'yi özel ağ sıkıştırma uygulamaları olmadan büyük veri taşımak için en hızlı yolu olarak kabul edilir. Distcp'yi yalnızca iki konum, tanıtıcıları otomatik yeniden denemeler yanı sıra işlem dinamik ölçeklendirme arasındaki farkları güncelleştirmek için bir seçenek de sağlar. Bu yaklaşım, tek bir dizin pek çok büyük dosyalarla olabilir Hive/Spark tabloları gibi şeyleri çoğaltmak için gelir ve yalnızca değiştirilen verileri kopyalamak istediğiniz zaman son derece verimli olur. Bu nedenlerle, Distcp'yi büyük veri depoları arasında veri kopyalama en önerilen araçtır. 

Kopyası işleri sıklığı veya veri tetikleyicileri yanı sıra, Linux cron işlerini kullanarak Apache Oozie iş akışları tarafından tetiklenebilir. Yoğun çoğaltma işleri için ayarlanmış ve özellikle kopyası işleri için genişletilmiş ayrı bir Hdınsight Hadoop kümesi dönmesi için önerilir. Bu kopyası işleri kritik bir iş ile karışmaması sağlar. Çoğaltma yetecek kadar geniş frekansında çalıştırılıyorsa, küme bile her iş arasında alınabilir. İkincil bölge'ye devretmek, başka bir küme de çalışmaya geri gelmesi sonra birincil Data Lake Store hesabı yeni veri çoğaltmak için ikincil bölge içinde başlar emin emin olun. Distcp'yi kullanım örnekleri için bkz: [kullanım Azure Storage Bloblarında ve Data Lake Store arasında veri kopyalamak için Distcp'yi](data-lake-store-copy-data-wasb-distcp.md).

### <a name="use-azure-data-factory-to-schedule-copy-jobs"></a>Azure Data Factory kopyalama işlerini zamanlamak için kullanın 

Azure Data Factory de kullanılabilir kullanarak kopyalama işlerini zamanlamak için bir **kopyalama etkinliği**ve hatta frekansına ayarlanabilir **Kopyalama Sihirbazı'nı**. Azure Data Factory bulut veri taşıma birimleri (DMUs) bir sınırı vardır ve büyük veri iş yükleri için işleme/işlem sonunda caps aklınızda bulundurun. Ayrıca, Azure Data Factory klasörleri Hive tablolarını gibi çoğaltmak için tam bir kopyasını gerektirecek şekilde Data Lake Store hesapları arasında delta güncelleştirmeleri şu anda sağlamaz. Başvurmak [kopyalama etkinliği ayarlama Kılavuzu](../data-factory/v1/data-factory-copy-activity-performance.md) Data Factory kopyalama hakkında daha fazla bilgi için. 

### <a name="adlcopy"></a>AdlCopy

AdlCopy yalnızca aynı bölge içinde iki Data Lake Store hesapları arasında veri kopyalamanıza olanak sağlayan bir Windows komut satırı aracıdır. AdlCopy aracı, tek başına seçeneği ya da kopyalama işini çalıştırmak için bir Azure Data Lake Analytics hesabı kullanma seçeneği sağlar. İlk olarak güçlü bir çoğaltma aksine isteğe bağlı kopyalar için oluşturuldu ancak aynı bölge içinde Data Lake Store hesapları arasında dağıtılmış kopyalama yapmak için başka bir seçenek sunar. Güvenilirlik için bu hiçbir üretim iş yükü için premium Data Lake Analytics seçeneği kullanmak için önerilir. Tek başına sürüm meşgul yanıtlar döndürebilir ve ölçek ve izleme sınırlıdır. 

Distcp'yi gibi AdlCopy bir Azure Otomasyonu veya Windows Görev Zamanlayıcısı'nı gibi bağımsızlıklar gerekir. Data Factory gibi ile AdlCopy yalnızca güncelleştirilmiş dosyaları kopyalanıyor desteklemez ancak yeniden kopyalar ve mevcut dosyaların üzerine yazabilirsiniz. Daha fazla bilgi ve AdlCopy kullanma örnekleri için bkz: [veri kopyalama Azure Storage Bloblarından Data Lake Store'a](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="monitoring-considerations"></a>İzleme konuları 

Data Lake Store, ayrıntılı tanılama günlüklerini ve denetim sağlar. Data Lake Store Data Lake Store hesabı altında Azure portalı ve Azure İzleyicisi bazı temel ölçümleri sağlar. Data Lake Store kullanılabilirliğini Azure Portalı'nda görüntülenir. Ancak, bu ölçüm yedi dakikada yenilendiğini ve genel olarak kullanıma sunulan bir API aracılığıyla sorgulanamıyor. Bir Data Lake Store hesabı en güncel kullanılabilirliğini almak için kullanılabilirlik doğrulamak için kendi yapay testlerini çalıştırmanız gerekir. Toplam depolama alanı kullanımı, okuma/yazma isteklerini ve giriş/çıkış gibi diğer ölçümleri Yenile 24 saate kadar sürebilir. Bu nedenle, daha fazla güncel ölçümleri el ile Hadoop komut satırı araçları ve günlük bilgilerini toplama hesaplanması gerekir. En son depolama alanı kullanımı almanın en hızlı yoludur, bu HDFS komutu Hadoop küme düğümünden (örneğin, baş düğüm) çalışıyor:   

    hdfs dfs -du -s -h adl://<adls_account_name>.azuredatalakestore.net:443/

### <a name="export-data-lake-store-diagnostics"></a>Dışarı aktarma Data Lake Store tanılama 

Data Lake Deposu'ndan veri aranabilir günlüklerine erişmek için en hızlı yolu, biri günlük dağıtımını etkinleştirmek için **günlük analizi** altında **tanılama** Data Lake Store hesabı dikey penceresinde. Bu, saat ve seçenekleri (e-posta/Web kancası) 15 dakikalık aralıklarla içinde tetiklenen uyarı birlikte içerik filtreleri ile gelen günlükleri anında erişim sağlar. Yönergeler için bkz: [Azure Data Lake Store için tanılama günlüklerine erişme](data-lake-store-diagnostic-logs.md). 

Daha fazla gerçek zamanlı uyarı verme ve günlükleri güden nerede hakkında daha fazla denetim için günlükleri Azure burada içeriği tek tek veya bir zaman penceresi üzerinden gerçek zamanlı bildirimler bir sıraya göndermek için çözümlenebilir EventHub verme göz önünde bulundurun. Ayrı bir uygulama gibi bir [mantıksal uygulama](../connectors/connectors-create-api-azure-event-hubs.md) sonra kullanmasına ve Uyarıları uygun kanala iletişim yanı izleme araçları NewRelic, Datadog veya AppDynamics gibi ölçümleri gönderin. Alternatif olarak, ElasticSearch gibi bir üçüncü taraf aracı kullanıyorsanız, Blob depolama alanına günlükleri verebilir ve kullanmak [Azure Logstash eklentisi](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob) Elasticsearch, Kibana ve Logstash (ELK) yığına verileri kullanmak üzere.

### <a name="turn-on-debug-level-logging-in-hdinsight"></a>Hata ayıklama düzeyi Hdınsight'ta günlük özelliğini açın 

Data Lake Store, günlük aktarma açık değilse Azure Hdınsight da açmak için bir yol sağlayıp sağlamadığını [istemci-tarafı Data Lake Store için günlüğü](data-lake-store-performance-tuning-mapreduce.md) log4j aracılığıyla. Aşağıdaki özelliğini ayarlamalısınız **Ambari** > **YARN** > **Config** > **yarn log4j Gelişmiş yapılandırmaları**: 

    log4j.logger.com.microsoft.azure.datalake.store=DEBUG 

Özellik ayarlanmışsa ve düğümlerin yeniden sonra Data Lake Store tanılama düğümlerde YARN günlüklerini yazılır (/tmp/<user>/yarn.log), veya önemli ayrıntılar gibi hatalar'yi ve azaltma (HTTP 429 hata kodu) ile izlenebilir. Aynı bilgilerin günlük analizi veya yerde günlükleri içeri aktarılır de izlenebilir [tanılama](data-lake-store-diagnostic-logs.md) Data Lake Store hesabı dikey. En az istemci-tarafı günlük kaydı açıksa veya günlük işletimsel görünürlük ve daha kolay hata ayıklama için Data Lake Store seçeneğiyle dağıtımını kullanmak için önerilir.

### <a name="run-synthetic-transactions"></a>Yapay işlem 

Şu anda, Azure portalında, Data Lake Store için hizmet kullanılabilirlik ölçümü 7 dakikalık yenileme penceresi yok. Ayrıca, genel olarak kullanıma sunulan bir API kullanarak sorgulanamıyor. Bu nedenle, en fazla dakika kullanılabilirlik sağlayabilirsiniz Data Lake Store için yapay işlemleri yapar temel bir uygulama oluşturmak için önerilir. Örnek bir Web işi, mantıksal uygulama ya da Azure işlev uygulaması okuma gerçekleştirmek, oluşturmak, Data Lake Store karşı güncelleştirmek ve sonuçları izleme çözümünüz için göndererek oluşturmak. İşlemler, geçici bir klasörde yapılır ve gereksinimlerine bağlı olarak her 30-60 saniye çalışabilir test sonra silinir.

## <a name="directory-layout-considerations"></a>Dizin yerleşim konuları

Veri gölü veri giriş, güvenlik, bölümlendirme ve işleme olabilir verilerin yapısını ön plana önemlidir etkili bir şekilde kullanılan. Azure Data Lake Store, Blob Depolama veya HDFS olup aşağıdaki önerilerin kullanılabilir. Her bir iş yükünün nasıl veri kullanılır, ancak IOT ile çalışırken göz önüne almanız gereken bazı ortak düzenler altındadır ve senaryoları toplu farklı gereksinimleri vardır.

### <a name="iot-structure"></a>IOT yapısı 

IOT iş yükleri, büyük bir bölümünü çok sayıda ürünleri, aygıtlar, kuruluşlar ve müşteriler yayılan veri deposunda landed veri olabilir. Kuruluş, güvenlik ve etkili bir veri işleme için aşağı akış tüketicileri ilişkin dizin düzenini önceden planlamak önemlidir. Dikkate alınması gereken genel bir şablon aşağıdaki düzen olabilir: 

    {Region}/{SubjectMatter(s)}/{yyyy}/{mm}/{dd}/{hh}/ 

Örneğin, uçak motorunun İngiltere içinde telemetri giriş aşağıdaki yapısı gibi görünebilir: 

    UK/Planes/BA1293/Engine1/2017/08/11/12/ 

Klasör yapısı son tarihte koymak için önemli bir neden yoktur. Belirli bölgeler veya konu da kullanıcıları/grupları için kilitleme istiyorsanız, daha sonra kolayca POSIX izinlerle bunu yapabilirsiniz. Belirli bir güvenlik grubu sadece UK veri ya da belirli düzlemleri görüntülemesini sınırlamak için bir gereksinimi varsa, aksi takdirde önde tarih yapısı ile ayrı izni her saat klasörü altındaki çeşitli klasörler için gerekli olacak. Süresi geçti gibi ek olarak, tarih yapısı önde sahip katlanarak klasörlerin sayısını artırır.

### <a name="batch-jobs-structure"></a>Toplu işleri yapısı 

Bir üst düzey, bir sık kullanılan toplu işlem bir "içinde" klasöründe veri güden yaklaşımdır. Ardından, verilerin işlendikten sonra yeni verileri "out" klasörüne kullanmak aşağı akış işlemleri yerleştirin. Bu dizin yapısı, tek tek dosyalar üzerinde işlem gerektiren ve yüksek düzeyde paralel işleme büyük veri kümeleri gerektirmeyebilecek işleri için bazen görülür. Yukarıda önerilen IOT gibi iyi dizin yapısını bölge ve konu önemlidir (örneğin, kuruluş, ürün/üretici) gibi şeyler için üst düzey klasör yapısının. Bu yapı, kuruluşunuz ve iş yüklerinizi verilerin daha iyi yönetim arasında veri güvenliğini yardımcı olur. Ayrıca, tarih ve saat yapısı içinde daha iyi düzenleme, filtrelenmiş aramalar, güvenlik ve Otomasyon işlenmesinde izin vermek için göz önünde bulundurun. Tarih yapısı için ayrıntı düzeyini üzerinde verileri karşıya veya, saatlik gibi günlük veya hatta aylık işlenen aralığa göre belirlenir. 

Bazen dosya işleme veri bozulma veya beklenmeyen biçimleri nedeniyle başarısız. Dizin yapısı bu gibi durumlarda yararlı bir **/hatalı** için denetleme ilerletmek için dosyaları taşımak için klasör. Toplu işlem aynı zamanda raporlama veya bu bildirim işleyebilirsiniz *hatalı* dosyaları el ile müdahale için. Aşağıdaki şablonu yapısını göz önünde bulundurun: 

    {Region}/{SubjectMatter(s)}/In/{yyyy}/{mm}/{dd}/{hh}/ 
    {Region}/{SubjectMatter(s)}/Out/{yyyy}/{mm}/{dd}/{hh}/ 
    {Region}/{SubjectMatter(s)}/Bad/{yyyy}/{mm}/{dd}/{hh}/ 

Örneğin, Kuzey Amerika'da istemcilerinden müşteri güncelleştirmelerin günlük verileri ayıklar pazarlama kesin alır. Önce ve sonra işlenen, aşağıdaki kod parçacığını gibi görünebilir: 

    NA/Extracts/ACMEPaperCo/In/2017/08/14/updates_08142017.csv 
    NA/Extracts/ACMEPaperCo/Out/2017/08/14/processed_updates_08142017.csv 
 
Toplu veri doğrudan Hive gibi veritabanları veya Geleneksel SQL veritabanlarına işlenmekte olan ortak durumda bir gereksinimi yoktur bir **/in** veya **/out** çıkış zaten girmeyeceğini beri klasör bir Hive tablosu veya dış veritabanı için klasör ayırın. Örneğin, müşterilerden gelen günlük ayıklar, ilgili klasörleri ve orchestration Azure Data Factory, Apache Oozie gibi bir şey tarafından güden veya Apache hava akışı işlemek ve Hive tabloya veri yazmak için bir günlük Hive veya Spark işini tetikleyecek.

## <a name="next-steps"></a>Sonraki adımlar     

* [Azure Data Lake Store'a Genel Bakış](data-lake-store-overview.md) 
* [Azure Data Lake Store'da erişim denetimi](data-lake-store-access-control.md) 
* [Azure Data Lake Store'da güvenlik](data-lake-store-security-overview.md)
* [Azure Data Lake Store için performans ayarlama](data-lake-store-performance-tuning-guidance.md)
* [Performans Hdınsight Spark Azure Data Lake Store ile kullanmaya yönelik kılavuz ayarlama](data-lake-store-performance-tuning-spark.md)
* [Performans Azure Data Lake Store ile Hdınsight Hive kullanmaya yönelik kılavuz ayarlama](data-lake-store-performance-tuning-hive.md)
* [Azure Data Lake Store için Azure Data Factory'yi kullanarak Veri Düzenlemesi](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Data Lake Store ile Hdınsight kümeleri oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md) 
