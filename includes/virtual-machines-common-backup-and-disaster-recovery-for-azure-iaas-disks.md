
# <a name="backup-and-disaster-recovery-for-azure-iaas-disks"></a>Azure Iaas diskler için yedekleme ve olağanüstü durum kurtarma

Bu makalede, yedekleme ve olağanüstü durum kurtarma (DR) Iaas sanal makineleri (VM'ler) ve Azure diskleri planlama açıklanmaktadır. Bu belge, yönetilen ve yönetilmeyen diskleri kapsar.

İlk olarak, yerel arızalarına karşı korunmasına yardımcı olur Azure platformu yerleşik hata toleransı özellikleri kapsar. Biz sonra tam olarak yerleşik özellikleri tarafından kapsanan olağanüstü durum senaryolarını ele alır. Bu belge tarafından gönderilen ana konu budur. Ayrıca bazı örnekleri farklı yedekleme ve kurtarma hakkında dikkat edilecek noktalar burada uygulayabilirsiniz iş yükü senaryoları gösteriyoruz. Biz sonra DR, Iaas diskler için olası çözümleri gözden geçirin. 

## <a name="introduction"></a>Giriş

Azure platformu yedeklik ve hataya dayanıklılık için çeşitli yöntemler yerelleştirilmiş donanım hatalarından müşterilerin korunmasına yardımcı olmak için kullanır. Yerel hatalar bu sunucu üzerinde bir sanal disk için verilerin bir kısmını ya da bir SSD veya HDD hatalarının depolayan bir Azure Storage server makinesindeki sorunlar dahil edebilirsiniz. Bu tür yalıtılmış donanım bileşeni arızalarına normal işlemler sırasında gerçekleşebilir. 

Azure platformu bu hatalarına dayanıklı olacak şekilde tasarlanmıştır. Ana afetler hataları veya çok sayıda depolama sunucuları veya hatta tüm veri merkezi inaccessibility neden olabilir. VM'ler ve diskleri yerelleştirilmiş hatalarından normalde korunan rağmen bölge çapında geri dönülemez hataları, VM ve diskleri etkileyebilecek büyük bir felaket gibi iş yükünü korumak ek adımlar gereklidir.

Platform hataları olasılığını ek olarak, müşteri uygulaması veya veri sorunlar oluşabilir. Örneğin, uygulamanızın yeni bir sürümünü bölüneceği neden verileri yanlışlıkla bir değişiklik yapabilir. Bu durumda, uygulama ve veri bilinen son iyi duruma içeren bir önceki sürüme geri isteyebilirsiniz. Bu, düzenli yedeklemeler koruma gerektirir.

Bölgesel olağanüstü durum kurtarma için farklı bir bölgeye Iaas VM disklerinizi yedeklemeniz gerekir. 

Şimdi biz yedekleme ve kurtarma seçenekleri bakın önce yerelleştirilmiş hata işleme için birkaç yöntem kullanılabilir olduðunu.

### <a name="azure-iaas-resiliency"></a>Azure Iaas dayanıklılık

*Dayanıklılık* donanım bileşenleri ortaya normal hata toleransı başvuruyor. Dayanıklılık, arızalardan kurtarmak ve çalışmaya devam yeteneğidir. Hataları önleme, ancak hataları kapalı kalma süresi veya veri kaybı önler şekilde yanıt verme hakkında değil. Dayanıklılığın hedefi, bir hatanın ardından uygulamayı tam çalışır duruma geri döndürmektir. Azure sanal makineleri ve diskleri için genel donanım hataları dayanıklı olacak şekilde tasarlanmıştır. Azure Iaas platform bu dayanıklılık nasıl sağladığını konumundaki bakalım.

Bir sanal makine çoğunlukla iki bölümden oluşur: bir işlem sunucusu ve kalıcı diskler. Her ikisi de bir sanal makinenin hata toleransı etkiler.

VM barındırıldığı Azure işlem ana bilgisayar sunucusu nadir, bir donanım hatası oluşursa Azure otomatik olarak VM başka bir sunucuya geri yüklemek için tasarlanmıştır. Bu, bilgisayarı yeniden başlatma gerçekleşir ve VM geri bir süre sonra gelmesi durumunda. Azure otomatik olarak bu tür donanım hataları algılar ve VM müşteri olabildiğince çabuk kullanılabilir sağlamaya yardımcı olmak için kurtarma yürütür.

Iaas disklerini veri dayanıklılığı kalıcı depolama platform için kritik öneme sahiptir. Azure müşterilerin önemli iş uygulamaları Iaas'da çalışan sahip ve veri kalıcılığı üzerinde bağlıdır. Bu Iaas disklerle yerel olarak depolanan verilerin üç yedek kopyaları için Azure tasarımları koruma. Bu kopya yerel hatalarına karşı yüksek dayanıklılık sağlar. Diskinizin tutan donanım bileşenleri biri başarısız olursa, disk isteklerini desteklemek için iki ek kopyalarını olduğundan, VM etkilenmez. Bir disk destekleyen iki farklı donanım bileşenleri (olan çok nadir) aynı anda başarısız olsa bile düzgün, çalışır. 

Üç çoğaltmaların her zaman korumak, üç birini kopyalar, Azure Storage veri arka planda yeni bir kopyasını otomatik olarak çoğaltılır emin olmak için kullanılamaz duruma gelir. Bu nedenle, bu hataya dayanıklılık için Azure disklerle RAID kullanmak için gerekli olmamalıdır. Basit bir RAID 0 yapılandırma gerekirse, daha büyük birimleri oluşturmak diskleri bölmek için yeterli olmalıdır.

Bu mimari nedeniyle Azure Kurumsal düzeyde tutarlı bir şekilde teslim Iaas için dayanıklılık diskler, endüstri lideri ile sıfır yüzde [değer yıllık hata oranı](https://en.wikipedia.org/wiki/Annualized_failure_rate).

Yerelleştirilmiş donanım hataları işlem üzerinde ana bilgisayar veya depolama platform tarafından kapsanan VM geçici kullanılamama sonuçlanır bazen değiştirebileceğiniz [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) VM kullanılabilirlik. Azure, Azure Premium Storage diskler kullanan tek VM örnekleri için endüstri lideri SLA de sağlar.

Müşteriler geçici olarak kullanım dışı kalması bir disk veya VM nedeniyle kapalı kalma süresini uygulama iş yüklerini korumak için kullanabileceğiniz [kullanılabilirlik kümeleri](../articles/virtual-machines/windows/manage-availability.md). Bir kullanılabilirlik kümesinde iki veya daha fazla sanal makineler uygulama için artıklık sağlar. Azure daha sonra bu sanal makineleri ve diskleri ayrı hata etki alanlarında farklı güç, ağ ve sunucu bileşenleri ile oluşturur. 

Bu ayrı hata etki alanları nedeniyle yerelleştirilmiş donanım hataları genellikle birden çok VM kümesindeki aynı anda etkilemez. Ayrı hata etki alanlarını sahip uygulamanız için yüksek kullanılabilirlik sağlar. Yüksek kullanılabilirlik gerekli olduğunda kullanılabilirlik kümeleri kullanmak iyi bir uygulama dikkate almıştır. Sonraki bölümde olağanüstü durum kurtarma yönlerini kapsar.

### <a name="backup-and-disaster-recovery"></a>Yedekleme ve olağanüstü durum kurtarma

Olağanüstü durum kurtarma kaynaklı nadir, ancak ana, olayların kurtarma olanağı ' dir. Bu, tüm bölgeyi etkiler hizmet kesintisi gibi geçici olmayan, geniş ölçekli hataları içerir. Olağanüstü durum kurtarma veri yedekleme ve arşivleme içerir ve bir veritabanını bir yedekten geri yükleme gibi el ile müdahale içerebilir.

Büyük ölçekli kesintileri büyük bir felaket neden olursa Azure platformun yerelleştirilmiş hatalarına karşı yerleşik koruma VM'ler/diskleri tam olarak korumaz. Bu gibi geri dönülemez olayları içeren bir veri merkezinde ulaşılırsa hortum, deprem, yangın tarafından ya da büyük ölçekli donanım birim hataları ise. Ayrıca, uygulama veya veri sorunları nedeniyle hataları karşılaşabilirsiniz.

Iaas iş yüklerinizi kesintilere karşı korunmasına yardımcı olmak için artıklık için planlama ve kurtarmayı etkinleştirmek için yedeklemeler olması gerekir. Olağanüstü durum kurtarma için birincil sitesinden farklı bir coğrafi konumda yedeklemelisiniz. Bu, yedekleme ilk olarak VM veya disklerin etkilenen aynı olayı tarafından etkilenmez sağlamaya yardımcı olur. Daha fazla bilgi için bkz: [Azure uygulamaları için olağanüstü durum kurtarma](/azure/architecture/resiliency/disaster-recovery-azure-applications).

DR konularınızdan aşağıdaki açıları şunlar olabilir:

- Yüksek Kullanılabilirlik: önemli kapalı kalma süresi olmadan sağlıklı bir durumda çalışmaya devam etmesini uygulama özelliği. Tarafından *sağlıklı duruma*, esnek bir uygulamasıdır ve kullanıcıların uygulamaya bağlanmak ve onlarla etkileşime demek isteriz. Belirli görev açısından kritik uygulamalar ve veritabanları bile platform hatası olduğunda her zaman kullanılabilir durumda gerekli olabilir. Bu iş yükleri için artıklık verilerin yanı sıra, uygulama için planlama gerekebilir.

- Veri dayanıklılığı: Bazı durumlarda, ana göz önünde bulundurarak bir olağanüstü durumda verilerin korunduğundan emin olmaktır. Bu nedenle, bir yedekleme verilerinizin farklı bir sitede gerekebilir. Bu tür iş yükleri için uygulama, ancak yalnızca normal yedekleme diskler için tam artıklık gerekmeyebilir.

## <a name="backup-and-dr-scenarios"></a>Yedekleme ve kurtarma senaryoları

Uygulama iş yükü senaryoları, bazı tipik örnekler için ve olağanüstü durum kurtarma için planlama konuları bakalım.

### <a name="scenario-1-major-database-solutions"></a>Senaryo 1: Ana veritabanı çözümleri

Bir üretim veritabanı sunucusu, SQL Server veya yüksek kullanılabilirliği destekleyen Oracle gibi göz önünde bulundurun. Kritik üretim uygulamaların ve kullanıcıların bu veritabanında bağlıdır. Bu sistem için olağanüstü durum kurtarma planı aşağıdaki gereksinimleri desteklemesi gerekebilir:

- Veriler korumalı ve kurtarılabilir olmalıdır.
- Sunucusunun kullanılabilir olması gerekir.

Olağanüstü durum kurtarma planı yedek olarak farklı bir bölgede veritabanının bir kopyasını koruyarak gerektirebilir. Sunucu kullanılabilirliği ve veri kurtarma gereksinimlerine bağlı olarak, verilerin düzenli çevrimdışı yedeklemeler için bir etkin-etkin veya etkin-pasif kopyayı sitesinden çözümü farklılık gösterebilir. SQL Server ve Oracle, gibi ilişkisel veritabanları çoğaltma için çeşitli seçenekler sağlar. SQL Server için kullanmak [SQL Server AlwaysOn Kullanılabilirlik grupları](https://msdn.microsoft.com/library/hh510230.aspx) yüksek kullanılabilirlik için.

MongoDB gibi NoSQL veritabanlarını da destek [çoğaltmaları](https://docs.mongodb.com/manual/replication/) artıklık için. Yüksek kullanılabilirlik çoğaltmaları kullanılır.

### <a name="scenario-2-a-cluster-of-redundant-vms"></a>Senaryo 2: Küme yedek VM'ler

Artıklık sağlamak ve Yük Dengeleme sanal makineleri bir küme tarafından işlenen bir iş yükü göz önünde bulundurun. Bir bölgede dağıtılmış Cassandra küme bir örnektir. Bu tür bir mimari zaten bu bölgedeki artıklık yüksek düzeyde sağlar. Ancak, iş yükü bölgesel düzeyinde arızasına karşı korumak için küme iki bölgeler arasında yayılmak veya başka bir bölgeye düzenli yedeklemeler yapma düşünmelisiniz.

### <a name="scenario-3-iaas-application-workload"></a>Senaryo 3: Iaas uygulama iş yükü

Iaas uygulama iş yükü bakalım. Örneğin, bir Azure VM üzerinde çalışan bir tipik bir üretim iş yükünü kaynaklanıyor olabilir. Bir web sunucusu veya dosya sunucusu içeriği ve diğer kaynakları bir sitenin bulunduran olabilir. Veri, kaynaklar ve uygulama durumunu VM disklerinde depolanan bir VM üzerinde çalışan bir özel olarak geliştirilmiş iş uygulaması de olabilir. Bu durumda, düzenli aralıklarla yedekleri alması emin olmak önemlidir. Yedekleme sıklığı VM iş yükü doğasına bağlı olmalıdır. Örneğin, uygulama her gün çalışır ve veri değiştirir, yedekleme saatte gerçekleştirilmelidir.

Başka kaynaklardan veri çeker ve toplanan raporlar üretir raporlama sunucusu başka bir örnektir. Bu VM veya diskleri kaybı raporları kaybına neden. Ancak, raporlama işlemi yeniden çalıştırın ve çıktıyı yeniden oluşturmak mümkün olabilir. Bu durumda, Raporlama sunucusu ile bir olağanüstü durum isabet olsa bile, veri kaybı gerçekten yok. Sonuç olarak, Raporlama sunucusunda veri parçası kaybetme dayanıklılık daha yüksek düzeyde olabilir. Bu durumda, daha az sık gerçekleştirilen yedeklemeler maliyetlerini azaltmak için bir seçenektir.

### <a name="scenario-4-iaas-application-data-issues"></a>Senaryo 4: Iaas uygulama verileri sorunları

Diğer bir olasılık Iaas uygulama veri sorunlardır. Hesaplar, korur ve fiyatlandırma bilgileri gibi kritik ticari verileri hizmet veren bir uygulamayı göz önünde bulundurun. Uygulamanızı yeni bir sürümü yanlış fiyatlandırma hesaplanır ve platform tarafından sunulan varolan ticaret verileri bozuk bir yazılım hata vardı. Burada, uygulama ve verilerin önceki sürümüne geri dönmek için iyi davranış biçimini verilmiştir. Bu ayarı etkinleştirmek için sisteminizi düzenli yedeklemelerini alın.

## <a name="disaster-recovery-solution-azure-backup"></a>Olağanüstü durum kurtarma çözümü: Azure yedekleme 

[Azure yedekleme](https://azure.microsoft.com/services/backup/) yedeklemeler ve DR için kullanılır ve birlikte çalıştığı [yönetilen diskleri](../articles/virtual-machines/windows/managed-disks-overview.md) yanı [yönetilmeyen diskleri](../articles/virtual-machines/windows/about-disks-and-vhds.md#unmanaged-disks). Bir yedekleme işi zaman tabanlı yedeklemeler, kolay VM geri yükleme ve yedekleme bekletme ilkeleri oluşturabilirsiniz. 

Kullanırsanız [Premium depolama diskleri](../articles/virtual-machines/windows/premium-storage.md), [yönetilen diskleri](../articles/virtual-machines/windows/managed-disks-overview.md), veya diğer disk türlerinde [yerel olarak yedekli depolama](../articles/storage/common/storage-redundancy.md#locally-redundant-storage) seçeneği, onu önemlidir düzenli DR yapmak özellikle yedeklemeler. Azure yedekleme, Kurtarma Hizmetleri kasası uzun vadeli bekletme için verileri depolar. Seçin [coğrafi olarak yedekli depolama](../articles/storage/common/storage-redundancy.md#geo-redundant-storage) seçeneği yedekleme kurtarma Hizmetleri kasası. Bu seçenek, yedeklemeler bölgesel olağanüstü durumlarını koruma için farklı bir Azure bölgesine çoğaltılır sağlar.

İçin [yönetilmeyen diskleri](../articles/virtual-machines/windows/about-disks-and-vhds.md#unmanaged-disks), Iaas diskler için yerel olarak yedekli depolama türünü kullanın, ancak Azure Backup ile kurtarma Hizmetleri kasası için coğrafi olarak yedekli depolama seçeneği etkin olduğundan emin olun.

> [!NOTE]
> Kullanırsanız [coğrafi olarak yedekli depolama](../articles/storage/common/storage-redundancy.md#geo-redundant-storage) veya [okuma erişimli coğrafi olarak yedekli depolama](../articles/storage/common/storage-redundancy.md#read-access-geo-redundant-storage) seçeneği yönetilmeyen diskleriniz için yine gereksinim tutarlı anlık görüntüler için yedekleme ve kurtarma. Kullanın ya da [Azure Backup](https://azure.microsoft.com/services/backup/) veya [tutarlı anlık görüntüleri](#alternative-solution-consistent-snapshots).

 Aşağıdaki tabloda, çözümleri DR için kullanılabilir bir özetidir.

| Senaryo | Otomatik çoğaltma | DR çözümü |
| --- | --- | --- |
| Premium depolama diskleri | Yerel ([yerel olarak yedekli depolama](../articles/storage/common/storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Yönetilen diskler | Yerel ([yerel olarak yedekli depolama](../articles/storage/common/storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Yönetilmeyen yerel olarak yedekli depolama diskleri | Yerel ([yerel olarak yedekli depolama](../articles/storage/common/storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Yönetilmeyen coğrafi olarak yedekli depolama diskleri | Çapraz bölge ([coğrafi olarak yedekli depolama](../articles/storage/common/storage-redundancy.md#geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Tutarlı anlık görüntüler](#alternative-solution-consistent-snapshots) |
| Yönetilmeyen okuma erişimli coğrafi olarak yedekli depolama diskleri | Çapraz bölge ([okuma erişimli coğrafi olarak yedekli depolama](../articles/storage/common/storage-redundancy.md#read-access-geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Tutarlı anlık görüntüler](#alternative-solution-consistent-snapshots) |

Yüksek kullanılabilirlik, en iyi kullanılabilirlik yanı sıra Azure yedekleme kümesi yönetilen diskleri kullanarak karşılanır. Yönetilmeyen disk kullanırsanız, Azure Backup DR için kullanabilirsiniz. Azure Yedekleme'yi yapamıyorsanız, ardından alma [tutarlı anlık görüntüleri](#alternative-solution-consistent-snapshots), bir sonraki bölümde açıklandığı gibi yedekleme için alternatif bir çözüm ve DR.

Yüksek kullanılabilirlik, yedekleme ve uygulama veya altyapı düzeylerinde DR ilgili seçimlerinizi gibi gösterilebilir:

| Düzey |   Yüksek kullanılabilirlik   | Yedekleme veya kurtarma |
| --- | --- | --- |
| Uygulama | SQL Server AlwaysOn | Azure Backup |
| Altyapı    | Kullanılabilirlik kümesi  | Coğrafi olarak yedekli depolama ile tutarlı anlık görüntüler |

### <a name="using-azure-backup"></a>Azure Yedekleme kullanılarak 

[Azure yedekleme](../articles/backup/backup-azure-vms-introduction.md) Vm'leriniz Windows çalıştıran yedekleyebilir veya Linux Azure kurtarma Hizmetleri kasası. Yedekleme ve geri yükleme iş açısından kritik verilerin veri üretmek uygulamaları çalıştırırken, iş açısından kritik verilerin yedeklenmesi gereken olgusu karmaşık. 

Bu sorunu gidermek için Microsoft iş yükleri için uygulamayla tutarlı yedeklemeler Azure yedekleme sağlar. Veri depolama için doğru yazıldığından emin olmak için birim gölge hizmeti kullanır. Linux işlevselliği için birim gölge hizmeti eşdeğer olmadığından Linux VM'ler için yalnızca dosya tutarlı yedeklemeler olasılığı vardır.

![Azure yedekleme akışı][1]

Azure Backup, zamanlanan saatte bir yedekleme işi başlattığında, zaman içinde nokta anlık almak için VM'de yüklü yedekleme uzantısını tetikler. Bir anlık görüntü kapatmak zorunda kalmadan sanal makinenin disklerinin tutarlı bir anlık görüntü almak için birim gölge Hizmeti'nin birlikte alınır. VM yedekleme uzantısına tüm yazma işlemlerini tutarlı bir anlık görüntüsü tüm diskler önce aktarır. Anlık görüntü alındıktan sonra verileri Azure yedekleme tarafından yedekleme Kasası'na aktarılır. Yedekleme işlemini daha verimli hale getirmek için hizmet tanımlar ve yalnızca son yedeklemeden sonra değiştirilen veri blokları aktarır.

Geri yüklemek için Azure Backup aracılığıyla kullanılabilir yedeklemeleri görüntüleyebilir ve geri yükleme başlatmak. Oluşturma ve Azure yedeklemeleri aracılığıyla geri [Azure portal](https://portal.azure.com/), göre [PowerShell kullanarak](../articles/backup/backup-azure-vms-automation.md), veya kullanarak [Azure CLI](/cli/azure/). 

### <a name="steps-to-enable-a-backup"></a>Bir yedekleme etkinleştirme adımları

Vm'leriniz yedeklerini kullanarak etkinleştirmek için aşağıdaki adımları kullanın [Azure portal](https://portal.azure.com/). Tam senaryonuza bağlı olarak bazı değişim yoktur. Başvurmak [Azure Backup](../articles/backup/backup-azure-vms-introduction.md) tam Ayrıntılar için belgelerine bakın. Azure ayrıca yedekleme [VM'ler ile yönetilen disklerini destekleyen](https://azure.microsoft.com/blog/azure-managed-disk-backup/).

1.  Bir kurtarma Hizmetleri kasası için bir VM oluşturun:

    a. İçinde [Azure portal](https://portal.azure.com/), Gözat **tüm kaynakları** ve Bul **kurtarma Hizmetleri kasaları**.

    b. Üzerinde **kurtarma Hizmetleri kasaları** menüsünde tıklatın **Ekle** ve VM ile aynı bölgede yeni bir kasa oluşturmak için aşağıdaki adımları izleyin. Örneğin, VM Batı ABD bölgesinde ise, Batı ABD için kasa seçin.

2.  Yeni oluşturulan kasa için depolama çoğaltma doğrulayın. Kasasında erişim **kurtarma Hizmetleri kasaları** ve Git **ayarları** > **yedekleme yapılandırması**. Olun **coğrafi olarak yedekli depolama** seçeneği, varsayılan olarak seçilidir. Bu, kasanız için ikincil veri merkezine otomatik olarak çoğaltılır sağlar. Örneğin, Batı ABD, kasaya Doğu ABD için otomatik olarak çoğaltılır.

3.  Yedekleme ilkesi yapılandırın ve aynı kullanıcı Arabiriminden VM seçin.

4.  Backup Aracısı VM üzerinde yüklü olduğundan emin olun. Bir Azure galerisinde görüntüsünü kullanarak VM oluşturduysanız, Yedekleme aracısı zaten yüklüdür. (Diğer bir deyişle, özel bir görüntü kullanırsanız) Aksi durumda, yönergeleri kullanın [VM Aracısı sanal makineye yükleme](../articles/backup/backup-azure-arm-vms-prepare.md#install-the-vm-agent-on-the-virtual-machine).

5.  VM yedekleme hizmetinin çalışması için ağ bağlantısı verdiğinden emin olun. Yönergeleri izleyin [ağ bağlantınızı](../articles/backup/backup-azure-arm-vms-prepare.md#establish-network-connectivity).

6.  Önceki adımları tamamladıktan sonra yedekleme İlkesi belirtildiği gibi düzenli aralıklarla yedekleme çalıştırır. Gerekirse ilk yedekleme Azure portalındaki kasa Panosu el ile tetikleyebilirsiniz.

Azure Backup komut dosyalarını kullanarak otomatikleştirmek için bkz [VM yedekleme için PowerShell cmdlet'leri](../articles/backup/backup-azure-vms-automation.md).

### <a name="steps-for-recovery"></a>Kurtarma için adımları

Onarmak veya VM yeniden ihtiyacınız varsa, yedekleme kurtarma noktalarının kasadaki birinden VM geri yükleyebilirsiniz. Birkaç kurtarmayı gerçekleştirmek için farklı seçenekler vardır:

-   Yeni bir VM, yedeklenen VM olarak zaman içinde nokta gösterimini oluşturabilirsiniz.

-   Diskleri geri yükleyin ve özelleştirin ve geri yüklenen VM yeniden oluşturmak için VM şablonu kullanın. 

Daha fazla bilgi için yönergeler için bkz: [sanal makineleri geri yüklemek için Azure portal'ı kullanmanızı](../articles/backup/backup-azure-arm-restore-vms.md). Bu belgede, ayrıca birincil veri merkezindeki bir olağanüstü durum varsa, coğrafi olarak yedekli yedekleme kasası kullanılarak yedeklenen VM'ler eşleştirilmiş bir veri merkezine geri yüklemek için belirli adımlar açıklanmaktadır. Bu durumda, Azure Backup geri yüklenen sanal makine oluşturmak için ikincil bölgesinden işlem hizmeti kullanır.

PowerShell için de kullanabilirsiniz [VM geri](../articles/backup/backup-azure-arm-restore-vms.md#restore-a-vm-during-an-azure-datacenter-disaster) veya [yeni bir sanal makineden oluşturma geri diskleri](../articles/backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

## <a name="alternative-solution-consistent-snapshots"></a>Alternatif çözüm: tutarlı anlık görüntüler

Azure Yedekleme'yi yapamıyorsanız, anlık görüntülerini kullanarak kendi yedekleme mekanizması uygulayabilirsiniz. Bir VM tarafından kullanılan tüm diskler için tutarlı anlık görüntüler oluşturma ve ardından bu anlık görüntülerin başka bir bölgeye çoğaltmaya karmaşık. Bu nedenle, özel bir çözüm oluşturmak daha iyi bir seçenek olarak Yedekleme hizmetini kullanarak Azure dikkate alır. 

Okuma erişimli coğrafi olarak yedekli depolama/coğrafi olarak yedekli depolama için diskleri kullanırsanız, anlık görüntüler bir ikincil veri merkezine otomatik olarak çoğaltılır. Yerel olarak yedekli depolama için diskleri kullanırsanız, verilerin kendiniz çoğaltmanız gerekir. Daha fazla bilgi için bkz: [artımlı anlık görüntüleri ile Azure yönetilmeyen VM diskleri yedekleme](../articles/virtual-machines/windows/incremental-snapshots.md).

Bir nesne belirli bir noktada bir gösterimini zaman içinde bir anlık görüntüdür. Anlık isteğe bağlı olarak veri için artımlı boyutu ayrı tutma fatura doğurur. Daha fazla bilgi için bkz: [blob anlık görüntü](../articles/storage/blobs/storage-blob-snapshots.md).

### <a name="create-snapshots-while-the-vm-is-running"></a>VM çalışırken anlık görüntülerini oluşturma

Herhangi bir zamanda bir anlık görüntü alabilirse VM çalışıyorsa, hala disklere akıtılan veri yoktur ve anlık görüntüleri uçuş modunda olan kısmi işlemleri içerebilir. Ayrıca, söz konusu birden çok disk varsa, farklı disklerin anlık görüntüleri farklı zamanlarda oluşmuş olabilir. Başka bir deyişle, bu anlık görüntüleri uyumlu olmayabilir. Bu değişikliği yedekleme sırasında yapılmışsa, dosya bozuk olabilir şeritli birimler için özellikle sorunlu oluşturur.

Bu durumu önlemek için yedekleme işlemi aşağıdaki adımları uygulamanız gerekir:

1.  Tüm disklerin dondurma.

2.  Tüm bekleyen yazma temizlenir.

3.  [Blob anlık görüntü](../articles/storage/blobs/storage-blob-snapshots.md) tüm diskler için.

SQL Server gibi bazı Windows uygulamaları uygulama tutarlı yedeklemeler oluşturmak için birim gölge hizmeti aracılığıyla koordineli bir yedekleme mekanizması sağlar. Linux üzerinde diskleri düzenlemekten fsfreeze gibi bir araç kullanabilirsiniz. Bu araç, dosya tutarlı yedeklemeler, ancak değil uygulamayla tutarlı anlık görüntüleri sağlar. Kullanmayı düşünmeniz gerekir böylece bu işlemi karmaşıktır [Azure yedekleme](../articles/backup/backup-azure-vms-introduction.md) veya zaten bu yordamı uygulayan bir üçüncü taraf yedekleme çözümünün.

Önceki işlem VM belirli bir zaman içinde nokta görünümünü temsil eden tüm VM diskleri için Eşgüdümlü anlık görüntü koleksiyonu sonuçlanır. VM için bir yedekleme geri yükleme noktası budur. Zamanlanan aralıklarla düzenli yedeklemeler oluşturmak için bu işlemi yineleyebilirsiniz. Bkz: [yedeklemeler başka bir bölgeye kopyalama](#copy-the-snapshots-to-another-region) anlık görüntüleri DR için başka bir bölgeye kopyalamak adımlar için.

### <a name="create-snapshots-while-the-vm-is-offline"></a>VM çevrimdışı durumdayken anlık görüntülerini oluşturma

VM kapatma ve her disk blob anlık görüntülerini almak için tutarlı yedeklemeler oluşturmak için başka bir seçenek değil. BLOB anlık görüntüleri alma çalışan bir VM anlık görüntülerini Eşgüdümleme daha kolaydır, ancak birkaç dakika kalma süresi gerekir.

1. VM kapatma.

2. Bir anlık görüntü yalnızca birkaç saniye sürer her sanal sabit sürücü BLOB oluşturun.

    Bir anlık görüntü oluşturmak için kullanabileceğiniz [PowerShell](../articles/storage/common/storage-powershell-guide-full.md), [Azure Storage REST API'sini](https://msdn.microsoft.com/library/azure/ee691971.aspx), [Azure CLI](/cli/azure/), ya da Azure Storage istemci kitaplıklarından birini gibi [ .NET için depolama istemci Kitaplığı](https://msdn.microsoft.com/library/azure/hh488361.aspx).

3. Kapalı kalma süresi sona VM başlatın. Genellikle, tüm işlem birkaç dakika içinde tamamlanır.

Bu işlem, VM için bir yedekleme geri yükleme noktası sağlama tüm diskler için tutarlı anlık görüntüler koleksiyonu verir.

### <a name="copy-the-snapshots-to-another-region"></a>Başka bir bölgeye anlık görüntüleri kopyalama

Tek başına anlık görüntü oluşturmaya DR için yeterli olmayabilir. Ayrıca, başka bir bölgeye anlık görüntüsü yedekleri çoğaltılması gerekir.

Ardından, disklerin coğrafi olarak yedekli depolama veya coğrafi olarak yedekli depolamaya okuma erişimi kullanıyorsanız, anlık görüntüleri ikincil bölge'ye otomatik olarak çoğaltılır. Birkaç dakika önce çoğaltma gecikmesi olabilir. Anlık görüntü çoğaltma bitirmeden birincil veri merkezindeki kullanılamaz hale gelirse ikincil veri merkezinden anlık görüntüleri erişemiyor. Bu olasılığını küçüktür.

> [!NOTE] 
> Diskin bir coğrafi olarak yedekli depolama okuma erişimli coğrafi olarak yedekli sahip yalnızca depolama hesabı olağanüstü durumlarını VM koruma sağlamaz. Ayrıca, Azure Yedekleme'yi Eşgüdümlü anlık görüntülerini oluşturma veya gerekir. Bu, bir VM tutarlı bir duruma kurtarmak için gereklidir.

Yerel olarak yedekli depolama kullanırsanız, anlık görüntü hemen oluşturduktan sonra farklı bir depolama hesabı için anlık görüntüleri kopyalamanız gerekir. Kopyalama hedefi Kopyala olan uzak bir bölgede içinde kaynaklanan bir yerel olarak yedekli depolama hesabı farklı bir bölgede olabilir. Ayrıca, anlık görüntü okuma erişimli coğrafi olarak yedekli depolama hesabının aynı bölgede kopyalayabilirsiniz. Bu durumda, anlık görüntü gevşek uzak ikincil bölge'ye çoğaltılır. Yedekleme birincil site olağanüstü gelen kopyaladıktan sonra korumalı ve çoğaltma tamamlanır.

Artımlı anlık DR için verimli bir şekilde kopyalamak için yönergeleri gözden [artımlı anlık görüntüleri ile Azure yönetilmeyen VM diskleri yedekleme](../articles/virtual-machines/windows/incremental-snapshots.md).

![Artımlı anlık görüntüleri ile Azure yönetilmeyen VM diskleri yedekleyin][2]

### <a name="recovery-from-snapshots"></a>Anlık görüntüler kurtarma

Bir anlık görüntü almak için yeni blob yapmak üzere kopyalayın. Anlık görüntü birincil hesabından kopyalıyorsanız, anlık görüntü üzerinde anlık görüntü temel blob kopyalayabilirsiniz. Bu işlem, anlık görüntü diske geri döner. Bu işlem, anlık görüntü yükseltme olarak bilinir. Bir okuma erişimli coğrafi olarak yedekli depolama hesabı söz konusu olduğunda ikincil bir hesaptan anlık görüntü yedekleme kopyalıyorsanız birincil hesabına kopyalamanız gerekir. Anlık görüntü tarafından kopyalayabilirsiniz [PowerShell kullanarak](../articles/storage/common/storage-powershell-guide-full.md) veya AzCopy yardımcı programını kullanarak. Daha fazla bilgi için bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy).

Birden çok diske sahip VM'ler için aynı Eşgüdümlü geri yükleme noktası parçası olan tüm anlık görüntüleri kopyalamanız gerekir. Anlık görüntüler için yazılabilir VHD bloblarının kopyaladıktan sonra VM için VM şablonu kullanarak yeniden oluşturmak için BLOB'ları kullanabilirsiniz.

## <a name="other-options"></a>Diğer seçenekler

### <a name="sql-server"></a>SQL Server

Bir VM içinde çalışan SQL Server, SQL Server veritabanınızın Azure Blob storage veya dosyaya yedekleme paylaşmak için kendi yerleşik özellikler vardır. Depolama hesabı coğrafi olarak yedekli depolama veya coğrafi olarak yedekli depolamaya okuma erişimi varsa, daha önce açıklandığı gibi kısıtlamalarıyla bir olağanüstü durumda bu yedekleri depolama hesabının ikincil veri merkezinde erişebilir. Daha fazla bilgi için bkz: [yedekleme ve geri yükleme, SQL Server için Azure sanal makinelerinde](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md). Yedekleme ve geri yükleme, ayrıca [SQL Server AlwaysOn Kullanılabilirlik grupları](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md) veritabanlarını ikincil çoğaltmaların koruyabilirsiniz. Bu, olağanüstü durum kurtarma süresini önemli ölçüde azaltır.

## <a name="other-considerations"></a>Diğer konular

Bu makalede nasıl yedeklemek veya Vm'lerinizi ve bunların diskleri olağanüstü durum kurtarma desteği için anlık görüntülerini almak ve bu verilerinizi kurtarmak için nasıl kullanılacağını ele. Azure Resource Manager modeli ile birçok kişi Azure'da Vm'leri ve diğer altyapısına oluşturmak için şablon kullanın. Her zaman aynı yapılandırmaya sahip bir VM oluşturmak için bir şablon kullanın. Vm'leriniz oluşturmak için özel resimler kullanırsanız, ayrıca görüntülerinizi depolamaya okuma erişimli coğrafi olarak yedekli depolama hesabı kullanarak korunduğundan emin olmalısınız.

Sonuç olarak, yedekleme işlemi iki şey bir bileşimi olabilir:

- Verileri (diskler) yedekleyin.
- Yapılandırmayı (şablonları ve özel resimler) yedekleyin.

Seçtiğiniz yedekleme seçeneği bağlı olarak hem verileri hem de yapılandırma yedeğini işlemeye sahip olabilir veya yedekleme hizmeti tümünü sizin için işleyebilirsiniz.

## <a name="appendix-understanding-the-impact-of-data-redundancy"></a>Ek: veri artıklığı etkisini anlama

Azure depolama hesapları için olağanüstü durum kurtarma ile ilgili göz önünde bulundurmalısınız veri artıklığı üç tür vardır: yerel olarak yedekli, coğrafi olarak yedekli veya coğrafi olarak yedekli okuma erişimi. 

Yerel olarak yedekli depolama, veri aynı veri merkezinde üç kopyasını tutar. VM veri yazdığında aynı bilmesi başarı çağırana döndürülmeden önce tüm üç kopyaları güncelleştirilir. Aynı anda tüm üç kopyaları etkilenen son derece düşüktür olduğundan diskinizin yerel hatalarından korunur. Yerel olarak yedekli depolama durumunda olmadığından hiçbir coğrafi yedeklilik, disk datacenter ya da depolama biriminin tamamını etkileyebilir yıkıcı hatalarından korumalı değil.

Coğrafi olarak yedekli depolama ve okuma erişimli coğrafi olarak yedekli depolama verilerinizin üç kopyasını tarafından seçilir birincil bölge içinde korunur. Verilerinizin üç ek kopya Azure tarafından ayarlanır karşılık gelen bir ikincil bölge korunur. Örneğin, Batı ABD veri depolarsanız, verileri Doğu ABD çoğaltılır. Birincil ve ikincil siteler için güncelleştirmeleri arasında kısa bir gecikme yoktur ve kopyalama bekletme zaman uyumsuz olarak yapılır. İkincil site disklerde çoğaltmalarının (gecikmeyle) bir disk başına temelinde tutarlı, ancak birden fazla etkin diskleri çoğaltmalarının birbirleri ile eşit olmayabilir. Birden çok diskte tutarlı çoğaltmaları için tutarlı anlık görüntüleri gereklidir.

Coğrafi olarak yedekli depolama okuma erişimli coğrafi olarak yedekli depolama arasındaki temel fark, okuma erişimli coğrafi olarak yedekli depolama ile ikincil kopya herhangi bir zamanda okuyabildiğini ' dir. Birincil bölge verileri erişilemeyen işleyen bir sorun varsa, Azure ekibi erişimi geri yüklemek için her türlü çabayı göstermektedir. Birincil çalışmadığında etkin coğrafi olarak yedekli depolamaya okuma erişimi varsa, ikincil veri merkezinde verilere erişebilir. Birincil erişilemediği durumda çoğaltmasından okuma planlıyorsanız, bu nedenle, ardından okuma erişimli coğrafi olarak yedekli depolama dikkate alınmalıdır.

Önemli bir kesinti olmasını öyle, Azure ekibi bir coğrafi yük devretme tetiklemek ve ikincil depolama birimine işaret edecek şekilde birincil DNS girişlerini değiştirin. Bu noktada, coğrafi olarak yedekli depolama veya etkin coğrafi olarak yedekli depolamaya okuma erişimi varsa, verileri ikincil kullanılan bölgede erişebilir. Diğer bir deyişle, coğrafi olarak yedekli depolama, depolama hesabınız ise ve bir sorun olduğundan, yalnızca bir yük devretme coğrafi ise ikincil depolama erişebilir.

Daha fazla bilgi için bkz: [bir Azure Storage kesinti oluşursa yapmanız gerekenler](../articles/storage/common/storage-disaster-recovery-guidance.md). 

>[!NOTE] 
>Microsoft, bir yük devretme olup olmadığını denetler. Tek tek müşteriler tarafından karar değil şekilde yük devretme depolama hesabı denetlenmez. Belirli bir depolama hesapları veya sanal makine disklerini için olağanüstü durum kurtarma uygulamak için bu makalede daha önce açıklanan teknikleri kullanmanız gerekir.



[1]: ./media/virtual-machines-common-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-1.png
[2]: ./media/virtual-machines-common-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-2.png
