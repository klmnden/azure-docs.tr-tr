# <a name="high-performance-premium-storage-and-managed-disks-for-vms"></a>Yüksek performanslı Premium depolama ve VM'ler için yönetilen diskleri
Azure Premium Storage giriş/çıkış (g/ç) ile sanal makineleri (VM'ler) için yüksek performanslı, düşük gecikmeli disk desteği sunar-yoğun iş yükleri. Premium depolama kullanan VM diskleri verileri katı hal sürücüleri (SSD) depolar. Hız ve premium depolama diskleri performansını yararlanacak Premium depolama alanına var olan VM diskleri geçirebilirsiniz.

Azure üzerinde bir VM için birkaç premium depolama diskleri ekleyebilirsiniz. Birden çok disk kullanarak uygulamalarınızı verir VM başına en fazla 256 TB. Premium depolama ile uygulamalarınızı 80.000 g/ç işlemi (IOPS) saniyede VM ve 2.000 megabayt (MB/sn) saniyede VM başına disk verimini elde edebilirsiniz. Okuma işlemleri çok düşük gecikme sağlar.

Premium Storage ile Azure gerçekten yükseltme-ve-shift zorlu kurumsal uygulamalar Dynamics AX, Dynamics CRM, Exchange Server, SAP Business Suite ve SharePoint grupları gibi buluta olanağı sunar. SQL Server, Oracle, MongoDB, MySQL ve Redis, tutarlı yüksek performans ve düşük gecikme gerektiren gibi uygulamalarda, veritabanı performansı yoğun iş yükleri çalıştırabilirsiniz.

> [!NOTE]
> Uygulamanız için en iyi performans için Premium depolama alanına yüksek IOPS gerektiren herhangi bir VM disk geçirmek öneririz. Diskinizin yüksek IOPS gerektirmiyorsa, standart Azure depolama alanında tutarak sınırı maliyetleri yardımcı olabilir. Standart depolama biriminde, sabit disk sürücülerinin (HDD'ler) yerine ssd'lerde VM disk veriler depolanır.
> 

Azure VM'ler için premium depolama diskleri oluşturmak için iki yol sunar:

* **Yönetilmeyen diskler**

    Özgün yöntemi yönetilmeyen diskleri kullanmaktır. Yönetilmeyen bir disk VM disklerinizi karşılık gelen sanal sabit disk (VHD) dosyaları depolamak için kullandığınız depolama hesaplarını yönetin. VHD dosyaları, Azure storage hesapları sayfa bloblarını olarak depolanır. 

* **Yönetilen diskleri**

    Seçtiğinizde [Azure yönetilen diskleri](../articles/virtual-machines/windows/managed-disks-overview.md), Azure VM diskleriniz için kullandığınız depolama hesapları yönetir. Disk türünü (Premium veya standart) ve ihtiyacınız diskin boyutunu belirtin. Azure oluşturur ve disk tarafından yönetilir. Diskleri depolama hesapları için ölçeklenebilirlik sınırları içinde kalmasını sağlamak için birden çok depolama hesabı yerleştirerek hakkında endişelenmeniz gerekmez. Azure, sizin için işler.

Bunların çoğu özelliklerden yararlanmak için yönetilen diskleri seçmenizi öneririz.

Premium depolama ile çalışmaya başlamak için [ücretsiz Azure hesabınızı oluşturmak](https://azure.microsoft.com/pricing/free-trial/). 

Premium depolama alanına, var olan sanal makineleri geçirme hakkında daha fazla bilgi için bkz [Windows VM yönetilmeyen disklerden yönetilen Diske Dönüştür](../articles/virtual-machines/windows/convert-unmanaged-to-managed-disks.md) veya [bir Linux VM yönetilmeyen disklerden yönetilen Diske Dönüştür](../articles/virtual-machines/linux/convert-unmanaged-to-managed-disks.md).

> [!NOTE]
> Premium depolama çoğu bölgede kullanılabilir. Satır için kullanılabilir bölgelerin listesi için bkz **Disk Depolama** içinde [Azure ürünleri bölgeye göre](https://azure.microsoft.com/regions/#services).
> 

## <a name="features"></a>Özellikler

Premium depolama özelliklerden bazıları şunlardır:

* **Premium depolama diskleri**

    Premium depolama belirli boyutu-serisi VM'ler eklenebilecek VM diskleri destekler. Premium depolama destekleyen DS serisi, DSv2 serisi, GS serisi, Ls-serisi ve Fs-serisi VM'ler. Yedi disk boyutları seçmesini: P4 (32GB), P6 (64GB), P10 (128GB), P20 (512GB), P30 (1024GB), P40 (2048GB), P50 (4095GB). P4 ve P6 disk boyutları henüz yalnızca yönetilen diskler için desteklenir. Her disk boyutu, kendi performans özellikleri vardır. Uygulama gereksinimlerinize bağlı olarak, VM için bir veya daha fazla disk ekleyebilirsiniz. Biz belirtimleri daha ayrıntılı olarak açıklayan [Premium Storage ölçeklenebilirlik ve performans hedefleri](#scalability-and-performance-targets).

* **Premium sayfa BLOB'ları**

    Premium depolama sayfa bloblarını destekler. Premium depolama VM'ler için kalıcı, yönetilmeyen disklerini depolamak için sayfa BLOB'ları kullanın. Standart Azure depolama, Premium depolama yok blok bloblarını destekler, BLOB'lar, dosyalar, tabloların veya kuyrukların ekleyin. Premium sayfa bloblarını destekler altı boyutlarına P10 P50 ve P60 (8191GiB). P60 Premium sayfa blobu VM disk eklenecek desteklenmiyor. 

    Premium depolama hesabı yerleştirilen herhangi bir nesne bir sayfa blob'u olacaktır. Sayfa blobu desteklenen sağlanan boyutlarından birini tutturur. Premium depolama hesabı küçük blobları depolamak için kullanılması amaçlanmamıştır nedeni budur.

* **Premium depolama hesabı**

    Premium Storage'ı kullanmaya başlamak için yönetilmeyen diskler bir premium depolama hesabı oluşturun. İçinde [Azure portal](https://portal.azure.com), premium depolama hesabı oluşturma, seçin **Premium** performans katmanı. Seçin **yerel olarak yedekli depolama (LRS)** çoğaltma seçeneği. Premium depolama hesabı türü ayarlayarak oluşturabilirsiniz **Premium_LRS** aşağıdaki konumlardan birinde:
    * [Storage REST API'sini](https://docs.microsoft.com/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference) (2014-02-14 sürümünü veya sonraki bir sürümünü)
    * [Hizmet Yönetimi REST API'si](http://msdn.microsoft.com/library/azure/ee460799.aspx) (sürüm 2014-10-01 veya sonraki bir sürümünü; Azure Klasik dağıtımlar için)
    * [Azure depolama kaynak sağlayıcısı REST API](https://docs.microsoft.com/rest/api/storagerp) (için Azure Resource Manager dağıtımları)
    * [Azure PowerShell](/powershell/azureps-cmdlets-docs.md) (sürüm 0.8.10 veya sonraki bir sürümünü)

    Premium depolama hesabı sınırları hakkında bilgi edinmek için [Premium Storage ölçeklenebilirlik ve performans hedefleri](#premium-storage-scalability-and-performance-targets).

* **Premium yerel olarak yedekli depolama**

    Premium depolama hesabı yalnızca yerel olarak yedekli depolama çoğaltma seçeneği olarak destekler. Yerel olarak yedekli depolama tek bir bölge içinde verileri üç kopyasını tutar. Bölgesel olağanüstü durum kurtarma için farklı bir bölgede, VM diskleri kullanarak yedeklemelisiniz [Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md). Ayrıca Yedekleme kasası olarak coğrafi olarak yedekli depolama (GRS) hesabı kullanmanız gerekir. 

    Azure depolama hesabınızın yönetilmeyen diskleriniz için bir kapsayıcı olarak kullanır. Bir Azure DS serisi, DSv2 serisi, GS serisi, oluşturduğunuzda veya Fs-serisi VM yönetilmeyen disklerle ve premium depolama hesabı, işletim sisteminizi seçin ve veri diskleri bu depolama hesabında depolanır.

## <a name="supported-vms"></a>Desteklenen VM'ler
Premium depolama destekleyen DS serisi, DSv2 serisi, GS serisi, Ls serisi, Fs-serisi ve B-serisi VM'ler. Bu VM türleriyle standart ve premium depolama diskleri kullanabilirsiniz. Premium depolama diskleri depolama uyumlu Premium olmayan VM dizisi ile kullanamazsınız.

VM türler ve Windows için Azure boyutları hakkında daha fazla bilgi için bkz: [Windows VM boyutları](../articles/virtual-machines/windows/sizes.md). Linux VM türler ve Azure boyutları hakkında bilgi için bkz: [Linux VM boyutları](../articles/virtual-machines/linux/sizes.md).

DS serisi, DSv2 serisi, GS serisi özelliklerden bazıları bunlar Ls-serisi ve Fs-serisi VM'ler:

* **Bulut hizmeti**

    DS serisi VM'ler yalnızca DS serisi VM'ler sahip bir bulut hizmeti ekleyebilirsiniz. DS serisi VM'ler DS serisi VM'ler dışında türünde olan bir bulut hizmetini eklemeyin. Yalnızca DS serisi VM'ler çalıştıran yeni bir bulut hizmeti için mevcut Vhd'lerinizi geçirebilirsiniz. Aynı sanal IP adresi kullanım sizin DS serisi VM'ler barındıran bir yeni bulut hizmeti kullanmak istiyorsanız, [ayrılmış IP adresi](../articles/virtual-network/virtual-networks-instance-level-public-ip.md). GS serisi VM'ler yalnızca GS serisi VM'ler sahip mevcut bir bulut hizmeti eklenebilir.

* **İşletim sistemi diski**

    Premium depolama VM'nizi bir premium veya standart işletim sistemi diski kullanacak şekilde ayarlayabilirsiniz. En iyi deneyim için Premium depolama-tabanlı işletim sistemi diski kullanmanızı öneririz.

* **Veri diskleri**

    Premium ve standart diskler aynı Premium Storage VM'si kullanabilirsiniz. Premium depolama ile bir VM sağlayabilir ve birkaç kalıcı veri diskini VM'e ekleyin. Gerekirse kapasitesini ve birim performansını artırmak için disklerde şeritler.

    > [!NOTE]
    > Premium depolama veri diskleri kullanarak şeritler varsa [depolama alanları](http://technet.microsoft.com/library/hh831739.aspx), kullandığınız her disk için 1 sütun ile depolama alanları ayarlayın. Aksi takdirde, genel performansını şeritli birim trafiği düzensiz dağıtım nedeniyle disklerde beklenenden daha düşük olabilir. Varsayılan olarak, Sunucu Yöneticisi ' nde sütunları 8 adete kadar diskler için ayarlayabilirsiniz. 8'den çok disk varsa, birim oluşturmak için PowerShell kullanın. Sütun sayısı el ile belirtin. Daha fazla disk olsa bile Aksi halde, Sunucu Yöneticisi kullanıcı Arabirimi 8 sütunları kullanmaya devam eder. Örneğin, tek kümesi 32 diskiniz varsa, 32 sütunları belirtin. Sanal diski kullanan sütun sayısını belirtmek için [New-VirtualDisk](http://technet.microsoft.com/library/hh848643.aspx) PowerShell cmdlet'ini, kullanım *NumberOfColumns* parametresi. Daha fazla bilgi için bkz: [depolama alanlarına genel bakış](http://technet.microsoft.com/library/hh831739.aspx) ve [depolama alanları sık sorulan sorular](http://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).
    >
    > 

* **Önbellek**

    Premium Storage destekleyen boyutu serideki VM'ler yüksek düzeyde üretilen iş ve gecikmeyi için benzersiz bir önbelleğe alma özelliğine sahip. Önbelleğe alma özelliği, temel alınan premium depolama disk performansı aşıyor. Premium depolama diskleri için ilke önbelleğe alma disk ayarlayabilirsiniz **ReadOnly**, **ReadWrite**, veya **hiçbiri**. İlke önbelleği varsayılan disk **ReadOnly** tüm premium veri diskleri için ve **ReadWrite** işletim sistemi diskler için. Uygulamanız için en iyi performans için doğru önbellek ayarını kullanın. Örneğin, okuma ağır veya salt okunur veri diskleri için SQL Server veri dosyaları gibi ayarlamak için ilkeyi önbelleğe alma disk **salt okunur**. Ağır yazma ya da salt yazılır veri diskleri için SQL Server günlük dosyaları gibi ayarlamak için ilkeyi önbelleğe alma disk **hiçbiri**. Tasarımınızın Premium depolama alanına sahip en iyi duruma getirme hakkında daha fazla bilgi için bkz: [tasarım performans için Premium depolama ile](../articles/virtual-machines/windows/premium-storage-performance.md).

* **Analizler**

    Premium depolama disklerini kullanarak VM performansını çözümlemek için VM tanılamada Aç [Azure portal](https://portal.azure.com). Daha fazla bilgi için bkz: [Azure VM ile Azure tanılama uzantısını izleme](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/). 

    Disk performansını görmek için işletim sistemi tabanlı araçları kullanın ister [Windows Performans İzleyicisi'ni](https://technet.microsoft.com/library/cc749249.aspx) Windows VM'ler için ve [iostat](http://linux.die.net/man/1/iostat) Linux VM'ler için komutu.

* **VM ölçek sınırlarını ve performans**

    Ölçek sınırlarını ve performans özelliklerini IOPS, bant genişliği ve VM başına bağlı disk sayısı için her Premium depolama desteklenen VM boyutu vardır. Premium depolama diskleri VM ile birlikte kullandığınızda, yeterli IOPS ve sürücü disk trafiği için VM üzerindeki bant genişliği olduğundan emin olun.

    Örneğin, bir STANDARD_DS1 VM 32 MB/sn premium depolama disk trafiği için olan ayrılmış bir bant genişliğine sahiptir. P10 premium depolama diskini bir bant genişliği 100 MB/sn sağlayabilir. Bu VM P10 premium depolama diskini bağlıysa, yalnızca en fazla 32 MB/sn gidebilirsiniz. En fazla 100 MB/P10 disk sağlayabilen s kullanamazsınız.

    Şu anda, DS serisi en büyük VM'yi Standard_DS15_v2 değil. Standard_DS15_v2 kadar 960 MB/sn tüm disklerde sağlayabilir. GS serisi en büyük VM'yi Standard_GS5 ' dir. Standard_GS5 en fazla 2000 MB/sn tüm disklerde sağlayabilir.

    Bu sınırlar yalnızca disk trafiği olduğunu unutmayın. Bu sınırlar İsabetli Önbellek okuma sayısı ve ağ trafiğini eklemeyin. VM ağ trafiği için ayrı bir bant genişliği kullanılabilir. Ağ trafiği için bant genişliği, premium depolama diskleri tarafından kullanılan ayrılmış bant genişliği farklıdır.

    En güncel hakkında bilgi için maksimum IOPS ve üretilen işi (bant) Premium depolama desteklenen VM'ler için bkz: [Windows VM boyutları](../articles/virtual-machines/windows/sizes.md) veya [Linux VM boyutları](../articles/virtual-machines/linux/sizes.md).

    Premium depolama disklerini ve bunların IOPS ve üretilen iş sınırları hakkında daha fazla bilgi için sonraki bölümündeki tabloya bakın.

## <a name="scalability-and-performance-targets"></a>Ölçeklenebilirlik ve performans hedefleri
Bu bölümde, Premium depolama kullanırken dikkate alınması gereken ölçeklenebilirlik ve performans hedefleri açıklanmaktadır.

Premium depolama hesapları aşağıdaki ölçeklenebilirlik hedefleri vardır:

| Toplam hesabı kapasitesi | Yerel olarak yedekli depolama hesabı için toplam bant genişliği |
| --- | --- | 
| Disk kapasitesi: 35 TB <br>Kapasite anlık görüntü: 10 TB | Yedeklemek için saniye başına 50 Gigabit gelen<sup>1</sup> + giden<sup>2</sup> |

<sup>1</sup> bir depolama hesabına gönderilen tüm veriler (istek sayısı)

<sup>2</sup> bir depolama hesabından alınan tüm verileri (yanıtlar)

Daha fazla bilgi için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](../articles/storage/common/storage-scalability-targets.md).

Yönetilmeyen diskler için premium depolama hesapları kullanıyorsanız ve uygulamanızın tek bir depolama hesabı ölçeklenebilirlik hedeflerini aşarsa, yönetilen disklere geçirmek isteyebilirsiniz. Yönetilen disklere geçirmek istemiyorsanız, birden çok depolama hesabı kullanmak için uygulamanızı oluşturun. Ardından, bu depolama hesaplarında verilerinizi bölüm. Örneğin, 51-TB diskler arasında birden çok VM eklemek isterseniz, bunları iki depolama hesapları arasında yayılır. 35 TB tek premium depolama hesabı için sınırı vardır. Tek premium depolama hesabı hiçbir zaman birden fazla 35 TB sağlanan diskleri sahip olduğundan emin olun.

### <a name="premium-storage-disk-limits"></a>Premium depolama disk sınırları
Premium depolama diskini sağladığınızda, diskin boyutunu en yüksek IOPS ve üretilen işi (bant) belirler. Azure premium depolama disklerinin yedi türleri sunar: P4 (yönetilen diskler yalnızca), P6 (yönetilen diskler yalnızca), P10, P20, P30, P40 ve P50. Her premium depolama disk türü IOPS ve üretilen iş için belirli sınırları vardır. Disk türleri için sınırları aşağıdaki tabloda açıklanmıştır:

| Premium diskler türü  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Disk boyutu           | 32 GB| 64 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| Disk başına IOPS       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Disk başına aktarım hızı | Saniye başına 25 MB  | Saniye başına 50 MB  | Saniyede 100 MB | 150 MB / saniye | 200 MB / saniye | Saniye başına 250 MB | Saniye başına 250 MB | 

> [!NOTE]
> Bölümünde açıklandığı gibi yeterli bant genişliği VM'nize sürücü disk trafiği üzerinde kullanılabilir olduğundan emin olun [Premium depolama desteklenen VM'ler](#premium-storage-supported-vms). Aksi takdirde, disk verimlilik ve IOPS kısıtlanması değerleri daha düşük. En yüksek verimlilik ve IOPS VM sınırları değil yukarıdaki tabloda açıklanan disk sınırlarını dayanır.  
> 
> 

Premium Storage ölçeklenebilirlik ve performans hedefleri hakkında bilmeniz gereken bazı önemli şey şunlardır:

* **Sağlanan kapasite ve performans**

    Standart depolama, farklı bir premium depolama diskini sağladığınızda kapasite, IOPS ve bu disk verimini sağlanır. Örneğin, bir P50 disk oluşturursanız, Azure 4.095 GB depolama kapasitesi, 7.500 IOPS ve bu disk için 250 MB/sn üretilen iş sağlar. Uygulamanız, kapasite ve performans bölümünü veya tümünü kullanabilir.

* **Disk boyutu**

    Azure (yuvarlanan) disk boyutu en yakın premium depolama disk seçeneğini tablonun önceki bölümde belirtildiği gibi eşler. Örneğin, 100 GB disk boyutunu P10 seçeneği olarak sınıflandırılır. En fazla 100 MB/sn üretilen iş ile en fazla 500 IOPS gerçekleştirebilirsiniz. Benzer şekilde, bir disk boyutu 400 GB P20 sınıflandırılır. 150 MB/sn üretilen iş ile en fazla 2,300 IOPS gerçekleştirebilirsiniz.
    
    > [!NOTE]
    > Kolayca var olan disklerin boyutunu artırabilirsiniz. Örneğin, 128 GB ve hatta 1 TB 30 GB disk boyutunu artırmak isteyebilirsiniz. Ya da daha fazla kapasite ya da daha fazla IOPS ve üretilen iş gerektiğinden P20 diskinizin P30 Diske Dönüştür isteyebilirsiniz. 
    > 
 
* **G/ç boyutu**

    Bir g/ç boyutu 256 KB'tır. Aktarılan veri 256 KB'den az ise, 1 g/ç birim olarak kabul edilir. Daha büyük g/ç boyutları olarak birden çok g/ç boyutu sayılır 256 KB. Örneğin, 1.100 KB g/ç 5 g/ç birimleri kabul edilir.

* **Aktarım hızı**

    Üretilen iş sınırı disk yazma işlemlerini içerir ve önbellekten hizmet olmayan disk okuma işlemleri içerir. Örneğin, 100 MB/sn'ye disk başına P10 disk vardır. Geçerli işleme bir P10 disk için bazı örnekleri aşağıdaki tabloda gösterilmiştir:

    | P10 disk başına en fazla üretilen işi | Diskten olmayan önbelleğini okur | Diske olmayan önbellek yazma |
    | --- | --- | --- |
    | 100 MB/s | 100 MB/s | 0 |
    | 100 MB/s | 0 | 100 MB/s |
    | 100 MB/s | 60 MB/sn | 40 MB/sn |

* **İsabetli Önbellek okuma sayısı**

    Ayrılmış IOPS veya disk verimini İsabetli Önbellek okuma sayısı sınırlı değildir. Örneğin, bir veri diski ile kullandığınızda bir **ReadOnly** Premium depolama, önbellekten sunulan okuma tarafından desteklenen bir VM'de önbellek ayarı olmayan IOPS ve üretilen iş tabi diskin caps. Bir disk iş yükünü daha ise okur, çok yüksek performans alabilirsiniz. Önbellek ayrı IOPS ve üretilen iş sınırları VM adresindeki VM boyutuna göre düzeyi, tabidir. DS serisi VM'ler yaklaşık 4.000 IOPS ve çekirdek önbelleği ve yerel SSD g/ç için başına 33 MB/sn üretilen iş vardır. GS serisi VM'ler 5.000 IOPS'yi ve çekirdek önbelleği ve yerel SSD g/ç için başına 50 MB/sn üretilen iş sınırı vardır. 

## <a name="throttling"></a>Azaltma
Uygulamanızı IOPS veya verimlilik premium depolama diski için ayrılmış sınırlarını aşıyor azaltma, meydana gelebilir. Toplam disk trafiğinizi VM üzerindeki tüm diskleri arasında disk için bant genişliği sınırı kullanılabilir VM aşarsa azaltma de oluşabilir. Azaltma önlemek için bekleyen disk g/ç istek sayısı sınırlamanızı öneririz. Sağlanan bir disk için ölçeklenebilirlik ve performans hedefleri ve disk kullanılabilen bant genişliği VM dayalı bir sınır kullanın.  

Azaltma önlemek üzere tasarlanmış olduğunda, uygulamanızın en düşük gecikme elde edebilirsiniz. Bekleyen disk g/ç istek sayısı kadar küçükse, ancak, uygulamanızın diske kullanılabilir işleme düzeyleri ve en fazla IOPS yararlanamaz.

Aşağıdaki örnekler, azaltma düzeylerini hesaplama göstermektedir. Tüm hesaplamalar 256 KB üzerinde bir g/ç birim boyutunu temel alır.

### <a name="example-1"></a>Örnek 1
Uygulamanızı bir P10 diskte bir saniye içinde 16 KB boyutunu 495 g/ç birimleri işledi. G/ç birimleri 495 IOPS sayılır. 2 MB g/ç aynı ikinci çalışırsanız, g/ç birimleri toplam 495 + 8 IOPS eşittir. Bunun nedeni, 2 MB g/ç 2.048 KB / 256 KB = 8 g/ç birimleri = g/ç birim boyutu 256 KB'dir olduğunda. 495 + 8 toplamını disk için 500 IOPS sınırı aştığından, azaltma oluşur.

### <a name="example-2"></a>Örnek 2
Uygulamanızı bir P10 diskte 256 KB boyutunu 400 g/ç birimleri işledi. Kullanılan toplam bant genişliği (400 &#215; 256) / 1.024 KB = 100 MB/s. P10 disk, 100 MB/sn üretilen iş sınırı vardır. Uygulamanız bu saniye içinde daha fazla g/ç işlemleri gerçekleştirmeye çalışırsa, ayrılmış sınırını aştığı için kısıtlanır.

### <a name="example-3"></a>Örnek 3
Bağlı iki P30 disklerle DS4 VM var. Her P30 disk 200 MB/sn verimini yeteneğine sahiptir. Ancak, bir DS4 VM 256 MB/sn toplam disk bant genişliği kapasitesine sahiptir. Eklenen her iki diskin en yüksek verimlilik bu DS4 VM üzerinde aynı anda sürücüsü olamaz. Bu sorunu çözmek için bir disk üzerindeki 200 MB/sn ve diğer diskteki 56 MB/sn, trafiği karşılayabilir. Toplam disk trafiğinizin 256 MB/sn kalırsa, disk trafiği azaltılır.

> [!NOTE]
> Disk trafiğinizi çoğunlukla küçük g/ç boyutlarını oluşuyorsa, büyük olasılıkla, uygulamanızın üretilen iş sınırı önce IOPS sınırı karşılaşır. Disk trafiği çoğunlukla büyük g/ç boyutlarını oluşuyorsa, ancak, büyük olasılıkla, uygulamanızın üretilen iş sınırı ilk olarak, IOPS sınırı yerine karşılaşır. Uygulamanızın IOPS ve işleme kapasitesi en iyi g/ç boyutları kullanarak en üst düzeye çıkarabilirsiniz. Ayrıca, bekleyen bir disk g/ç istek sayısını sınırlayabilirsiniz.
> 

Premium depolama kullanarak yüksek performans için tasarlama hakkında daha fazla bilgi için bkz: [tasarım performans için Premium depolama ile](../articles/virtual-machines/windows/premium-storage-performance.md).

## <a name="snapshots-and-copy-blob"></a>Anlık görüntüler ve kopyalama Blob

Depolama hizmeti için bir sayfa blob'u VHD dosyasıdır. Sayfa bloblarını anlık görüntülerini almak ve bunları başka bir konuma gibi farklı bir depolama hesabı için kopyalayın.

### <a name="unmanaged-disks"></a>Yönetilmeyen diskler

Oluşturma [artımlı anlık görüntüleri](../articles/virtual-machines/linux/incremental-snapshots.md) ile standart depolama anlık görüntüleri kullanmak aynı şekilde yönetilmeyen premium diskler için. Premium depolama yalnızca yerel olarak yedekli depolama çoğaltma seçeneği olarak destekler. Anlık görüntüler oluşturmak ve ardından anlık görüntüler bir standart coğrafi olarak yedekli depolama hesabına kopyalamak öneririz. Daha fazla bilgi için bkz: [Azure depolama artıklığı seçeneği](../articles/storage/common/storage-redundancy.md).

Bir VM'ye bir disk bağlıysa, disk üzerinde bazı API işlemleri izin verilmez. Örneğin, gerçekleştiremezsiniz bir [kopyalama Blob](/rest/api/storageservices/Copy-Blob) disk için bir VM bağlıysa, blob işlemi. Bunun yerine, önce bir anlık görüntüsünü o blob kullanarak oluşturmak [anlık görüntü Blob](/rest/api/storageservices/Snapshot-Blob) REST API. Ardından, gerçekleştirin [kopyalama Blob](/rest/api/storageservices/Copy-Blob) bağlı diske kopyalamak için anlık görüntü. Alternatif olarak, disk ayırma ve gerekli işlemleri gerçekleştirin.

Premium depolama blob anlık görüntüler için aşağıdaki sınırları Uygula:

| Premium depolama sınırı | Değer |
| --- | --- |
| Maksimum sayıda anlık görüntü blob başına | 100 |
| Anlık görüntüler için depolama hesabı kapasitesi<br>(Verileri yalnızca anlık görüntülerini içerir. Veri temel blob dahil değildir.) | 10 TB |
| Art arda gelen anlık görüntüleri arasındaki en kısa süre | 10 dakika |

Coğrafi olarak yedekli, anlık görüntü kopyalarını bulundurmak için anlık görüntüler bir premium depolama hesabından bir standart coğrafi olarak yedekli depolama hesabına AzCopy veya kopya Blob kullanarak kopyalayabilirsiniz. Daha fazla bilgi için bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](../articles/storage/common/storage-use-azcopy.md) ve [kopyalama Blob](/rest/api/storageservices/Copy-Blob).

Premium depolama hesabı sayfa bloblarını karşı REST işlemlerini gerçekleştirme hakkında ayrıntılı bilgi için bkz: [Blob Azure Premium Storage ile hizmet işlemleri](http://go.microsoft.com/fwlink/?LinkId=521969).

### <a name="managed-disks"></a>Yönetilen diskler

Yönetilen bir disk için bir anlık görüntü, yönetilen disk, salt okunur bir kopyasıdır. Anlık görüntü, standart yönetilen disk olarak depolanır. Şu anda [artımlı anlık görüntüleri](../articles/virtual-machines/windows/incremental-snapshots.md) yönetilen disklerde desteklenmez. Yönetilen bir disk için bir anlık görüntüsünü öğrenmek için bkz: [Azure yönetilen olarak depolanan VHD bir kopyasını oluşturmak Windows'da yönetilen anlık görüntülerini kullanarak disk](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md) veya [Azure yönetilen olarak depolanan VHD bir kopyasını oluşturmak Linux içinde yönetilen anlık görüntülerini kullanarak disk](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md).

Yönetilen bir disk için bir VM bağlıysa, disk üzerinde bazı API işlemleri izin verilmez. Örneğin, bir paylaşılan erişim imzası (SAS) disk için bir VM eklenirken bir kopyalama işlemi gerçekleştirmek için oluşturulamıyor. Bunun yerine, önce diskin anlık görüntüsünü oluşturabilir ve sonra anlık görüntü kopyası gerçekleştirin. Alternatif olarak, disk ayırma ve ardından kopyalama işlemi gerçekleştirmek için bir SAS oluşturun.


## <a name="premium-storage-for-linux-vms"></a>Premium depolama Linux VM'ler
Premium depolama Linux Vm'lerinizi ayarlamanıza yardımcı olması için aşağıdaki bilgileri kullanın:

Premium depolama önbellek alt disklerle kümesine için Premium depolama ölçeklenebilirlik hedeflerini elde etmek için **ReadOnly** veya **hiçbiri**, dosya sistemi bağladığınızda "engelleri" devre dışı bırakmanız gerekir. Premium depolama diskleri yazma işlemleri için bu önbellek ayarlarını dayanıklı olduğundan bu senaryoda engelleri gerekmez. Yazma isteği başarıyla tamamlandığında, kalıcı depoya veri yazıldı. "Engelleri" devre dışı bırakmak için aşağıdaki yöntemlerden birini kullanın. Bir dosya sistemi için seçin:
  
* İçin **reiserFS**engelleri devre dışı bırakmak, kullanmak için `barrier=none` bağlama seçeneği. (Engelleri etkinleştirmek için `barrier=flush`.)
* İçin **ext3/ext4**engelleri devre dışı bırakmak, kullanmak için `barrier=0` bağlama seçeneği. (Engelleri etkinleştirmek için `barrier=1`.)
* İçin **XFS**engelleri devre dışı bırakmak, kullanmak için `nobarrier` bağlama seçeneği. (Engelleri etkinleştirmek için `barrier`.)
* Premium storage için önbellek disklerle kümesine **ReadWrite**, engelleri yazma dayanıklılık için etkinleştirin.
* Birim etiketleri VM yeniden başlattıktan sonra kalıcı olması diskleri evrensel benzersiz tanımlayıcı (UUID) başvuruları /etc/fstab güncelleştirmeniz gerekir. Daha fazla bilgi için bkz: [bir Linux VM yönetilen bir disk eklemek](../articles/virtual-machines/linux/add-disk.md).

Aşağıdaki Linux dağıtımları için Azure Premium Storage doğrulandı. Daha iyi performans ve kararlılık Premium depolama alanına sahip en az bu sürümlerinden birine (veya sonraki bir sürümünü) Vm'leriniz yükseltmenizi öneririz. Bazı sürümler son Linux Tümleştirme hizmetleri (LIS), v4.0, Azure için gerektirir. Karşıdan yüklemek ve bir dağıtım yüklemek için aşağıdaki tabloda listelenen bağlantıyı izleyin. Biz doğrulama tamamlarken biz görüntüleri listesine ekleyin. Bizim doğrulamaları performans için her görüntü değişir Göster unutmayın. Performans, iş yükü özellikleri ve görüntü ayarlarınızı bağlıdır. Farklı görüntüleri farklı türde iş yükleri için ayarlanmıştır.

| Dağıtım | Sürüm | Desteklenen çekirdek | Ayrıntılar |
| --- | --- | --- | --- |
| Ubuntu | 12.04 | 3.2.0-75.110+ | Ubuntu-12_04_5-LTS-amd64-Server-20150119-en-us-30GB |
| Ubuntu | 14.04 | 3.13.0-44.73+ | Ubuntu-14_04_1-LTS-amd64-Server-20150123-en-us-30GB |
| Debian | 7.x, 8.x | 3.16.7-ckt4-1+ | &nbsp; |
| SUSE | SLES 12| 3.12.36-38.1+| SuSE-sles-12-öncelik-v20150213 <br> sles 12 v20150213 SuSE |
| SUSE | SLES 11 SP4 | 3.0.101-0.63.1+ | &nbsp; |
| CoreOS | 584.0.0+| 3.18.4+ | CoreOS 584.0.0 |
| CentOS | 6.5, 6.6, 6.7, 7.0 | &nbsp; | [Gerekli LIS4](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Sonraki bölümde nota bakın* |
| CentOS | 7.1+ | 3.10.0-229.1.2.el7+ | [Önerilen LIS4](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Sonraki bölümde nota bakın* |
| Red Hat Enterprise Linux (RHEL) | 6.8+, 7.2+ | &nbsp; | &nbsp; |
| Oracle | 6.0+, 7.2+ | &nbsp; | UEK4 veya RHCK |
| Oracle | 7.0-7.1 | &nbsp; | UEK4 veya RHCK içeren[LIS 4.1 +](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |
| Oracle | 6.4-6.7 | &nbsp; | UEK4 veya RHCK içeren[LIS 4.1 +](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |


### <a name="lis-drivers-for-openlogic-centos"></a>OpenLogic CentOS için LIS sürücüleri

OpenLogic CentOS VM'ler çalıştırıyorsanız, en son sürücüleri yüklemek için aşağıdaki komutu çalıştırın:

```
sudo rpm -e hypervkvpd  ## (Might return an error if not installed. That's OK.)
sudo yum install microsoft-hyper-v
```

Yeni sürücülerin etkinleştirmek için bilgisayarı yeniden başlatın.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Premium depolama kullandığınızda, aşağıdaki fatura değerlendirmeleri geçerlidir:

* **Premium depolama diski ve blob boyutu**

    Premium depolama diskini veya blob için fatura disk veya blob sağlanan boyutuna bağlıdır. Azure (yuvarlanan) sağlanan boyutuna yakın premium depolama diski seçeneği eşler. Ayrıntılar için bölümündeki tabloya bakın [Premium Storage ölçeklenebilirlik ve performans hedefleri](#premium-storage-scalability-and-performance-targets). Her disk için desteklenen sağlanan disk boyutu eşler ve buna göre faturalandırılır. Sağlanan herhangi bir disk için fatura, saatlik için Premium depolama teklifi aylık fiyat kullanarak eşit olarak bölünür. Örneğin, P10 disk sağlanan ve 20 saat sonra silinir, P10 teklifi 20 saate eşit olarak için fatura edilir. Disk veya IOPS ve üretilen iş kullanılan için yazılan gerçek veri miktarı bakılmaksızın budur.

* **Premium yönetilmeyen disklerin anlık görüntüleri**

    Yönetilmeyen premium disklerde anlık görüntüler, anlık görüntü tarafından kullanılan ek kapasite için faturalandırılır. Anlık görüntüler hakkında daha fazla bilgi için bkz: [blob görüntüsünü](/rest/api/storageservices/Snapshot-Blob).

* **Yönetilen premium disklerin anlık görüntüleri**

    Yönetilen bir disk görüntüsünü diskin salt okunur bir kopyasıdır. Disk standart yönetilen disk olarak depolanır. Standart yönetilen disk olarak bir anlık görüntü aynı maliyetleri. Örneğin, 128 GB premium yönetilen diskin bir anlık görüntüsünü alırsanız, 128 GB standart yönetilen disk için anlık görüntü maliyetini eşdeğerdir.  

* **Giden veri aktarımları**

    [Giden veri aktarımları](https://azure.microsoft.com/pricing/details/data-transfers/) (Azure veri merkezleri dışında giderek veri) bant genişliği kullanımı için fatura doğurur.

Premium depolama, Premium depolama desteklenen VM'ler ve yönetilen diskleri için fiyatlandırma hakkında ayrıntılı bilgi için bu makalelere bakın:

* [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/)
* [Sanal makineler fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Fiyatlandırma yönetilen diskleri](https://azure.microsoft.com/pricing/details/managed-disks/)

## <a name="azure-backup-support"></a>Azure yedekleme desteği 

Bölgesel olağanüstü durum kurtarma için farklı bir bölgede, VM diskleri kullanarak yedeklemelisiniz [Azure yedekleme](../articles/backup/backup-introduction-to-azure-backup.md) ve bir yedekleme kasası olarak GRS depolama hesabı.

Zaman tabanlı yedeklemeler sayesinde bir yedekleme işi oluşturmak için kolay VM geri yükleme ve yedekleme bekletme ilkeleri, Azure yedekleme kullanın. Yönetilen ve yönetilmeyen disklerle yedekleme hem de kullanabilirsiniz. Daha fazla bilgi için bkz: [yönetilmeyen diskleri olan VM'ler için Azure Backup](../articles/backup/backup-azure-vms-first-look-arm.md) ve [yönetilen diskleri olan VM'ler için Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup). 

## <a name="next-steps"></a>Sonraki adımlar
Premium depolama hakkında daha fazla bilgi için aşağıdaki makalelere bakın.

### <a name="design-and-implement-with-premium-storage"></a>Premium depolama ile tasarlayıp
* [Premium depolama alanına sahip bir performans için tasarlama](../articles/virtual-machines/windows/premium-storage-performance.md)
* [Premium depolama alanına sahip BLOB Depolama işlemleri](http://go.microsoft.com/fwlink/?LinkId=521969)

### <a name="operational-guidance"></a>İşlemsel kılavuz
* [Azure Premium depolama alanına geçiş](../articles/storage/common/storage-migration-to-premium-storage.md)

### <a name="blog-posts"></a>Blog yazıları
* [Azure Premium Storage genel olarak kullanılabilir](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/)
* [GS serisi tanışın: en büyük sanal makineleri genel bulut Premium depolama ekleme desteği](https://azure.microsoft.com/blog/azure-has-the-most-powerful-vms-in-the-public-cloud/)
