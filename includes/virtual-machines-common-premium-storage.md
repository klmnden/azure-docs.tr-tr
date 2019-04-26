---
title: include dosyası
description: include dosyası
services: storage
author: ramankumarlive
ms.service: storage
ms.topic: include
ms.date: 09/24/2018
ms.author: ramankum
ms.custom: include file
ms.openlocfilehash: 40ff2339ad34a72079109317bf0a89dfbc6458e8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60232754"
---
# <a name="high-performance-premium-storage-and-managed-disks-for-vms"></a>Yüksek performanslı Premium depolama ve VM'ler için yönetilen diskler

Azure Premium depolama, giriş/çıkış (g/ç) ile sanal makineleri (VM'ler) için yüksek performanslı, düşük gecikme süreli disk desteği sunar-yoğun iş yükleri. Premium depolama kullanan sanal makine diskleri verileri katı hal sürücülerine (SSD) depolar. Hızını avantajlarından ve premium depolama disklerini performansını yararlanmak için var olan VM diskleri Premium depolamaya geçiş yapabilirsiniz.

Azure'da bir VM için birkaç premium depolama diskleri ekleyebilirsiniz. Sağlar, uygulamalarınızın birden çok disk kullanarak 256 TB uygulamanız, sanal makine başına yaklaşık 2 PiB kadar olabilir önizleme boyutları kullanıyorsanız, VM başına depolama alanı. Premium depolama sayesinde, uygulamalarınızı (IOPS) VM başına saniyede 80.000 g/ç işlemleri ve en çok 2.000 megabayttır (MB/sn) VM başına saniyede, bir disk aktarım hızı elde edebilirsiniz. Okuma işlemleri, çok düşük gecikme süreleri sağlar.

Premium depolama sayesinde, Azure, gerçek anlamda lift-and-shift ile taşıma zorlu kurumsal uygulamalar Dynamics AX, Dynamics CRM, Exchange Server, SAP Business Suite ve SharePoint grupları gibi buluta olanağı sunar. SQL Server, Oracle, MongoDB, MySQL ve Redis, tutarlı, yüksek performanslı ve düşük gecikme süresi gerektiren gibi uygulamalarda performans bakımından yoğun veritabanı iş yüklerini çalıştırabilirsiniz.

> [!NOTE]
> Uygulamanız için en iyi performans için Premium depolama yüksek IOPS gerektiren herhangi bir VM disk geçirme öneririz. Yüksek IOPS, disk gerektirmiyorsa, standart Azure depolama alanında tutarak sınırı maliyetleri yardımcı olabilir. Standart depolama alanında, sanal makine disk verilerini sabit disk sürücülerinin (HDD'ler) yerine SSD üzerinde depolanır.

Azure Vm'leri için premium depolama disklerini oluşturmak için iki yol sunar:

* **Yönetilmeyen diskler**

    Özgün yöntemi yönetilmeyen diskleri kullanmaya yöneliktir. Yönetilmeyen bir disk, sanal makine disklerinizi karşılık gelen sanal sabit disk (VHD) dosyaları depolamak için kullandığınız depolama hesapları yönetin. VHD dosyaları, Azure depolama hesaplarındaki sayfa blobları olarak depolanır. 

* **Yönetilen diskler**

    Seçeneğini belirlediğinizde [Azure yönetilen diskler](../articles/virtual-machines/windows/managed-disks-overview.md), Azure tarafından yönetilen depolama hesapları, VM diskleri için kullanın. Disk türünü (Premium veya standart) ve ihtiyacınız olan diskin boyutunu belirtin. Azure, oluşturur ve disk sizin yerinize yönetir. Diskleri depolama hesaplarınız için ölçeklenebilirlik sınırları içinde kalmasını sağlamak için birden çok depolama hesabında yerleştirerek hakkında endişelenmeniz gerekmez. Azure, sizin yerinize çözer.

Kendi çok sayıda özellikten yararlanmak için yönetilen diskler, seçmenizi öneririz.

Premium depolama ile kullanmaya başlamak için [ücretsiz Azure hesabınızı oluşturun](https://azure.microsoft.com/pricing/free-trial/). 

Varolan sanal makinelerinize Premium depolamaya geçiş hakkında daha fazla bilgi için bkz: [Windows VM'yi yönetilmeyen disklerden yönetilen disklere dönüştürme](../articles/virtual-machines/windows/convert-unmanaged-to-managed-disks.md) veya [bir Linux VM'yi yönetilmeyen disklerden yönetilen disklere dönüştürme](../articles/virtual-machines/linux/convert-unmanaged-to-managed-disks.md).

> [!NOTE]
> Premium depolama bölgelerinin çoğunda kullanılabilir. Satır için kullanılabilir bölgelerin listesi için bkz. **Disk Depolama** içinde [Azure bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/#services).

## <a name="features"></a>Özellikler

Premium depolama özelliklerinden bazıları şunlardır:

* **Premium depolama diskleri**

    Premium depolama için belirli boyut serisi VM'ler eklenebilecek VM disklerini destekler. Premium depolama, çok çeşitli Azure Vm'leri destekler. Sekiz GA disk boyutları vardır:  P4 (32 GiB) P6 (64 GiB) P10 (128 Gib'a) P15 (256 GiB), P20 (512 Gib'a) P30 (1024 GiB), P40 (2.048 GiB) P50 (4.095 GiB). Yanı sıra, üç Önizleme disk boyutları: P60 olmak üzere 8.192 GiB (8 tib'a kadar), P70 16,348 GiB (16 tib'a kadar), P80 32.767 GiB (32 tib'a kadar). P4, P6, P15, P60, P70 ve P80 disk boyutları şu anda yalnızca yönetilen diskler için desteklenir. Her disk boyutu, kendi performans özellikleri vardır. Uygulama gereksinimlerinize bağlı olarak, sanal makinenizde bir veya daha fazla disk ekleyebilirsiniz. Özellikleri daha ayrıntılı olarak açıklanmaktadır [Premium depolama ölçeklenebilirlik ve performans hedefleri](#scalability-and-performance-targets).

* **Premium sayfa blobları**

    Premium depolama, sayfa bloblarını destekler. Sayfa blobları, kalıcı, yönetilmeyen diskler için Premium depolama Vm'lerini depolamak için kullanın. Standart Azure depolama Premium depolama değil blok bloblarını destekler, blobları, dosyalar, tablolar veya kuyruklar Ekle. Premium sayfa bloblarını desteklemediğinden altı boyutları P10 P50 ve P60 (8191GiB). P60 Premium sayfa blobu VM disklerinin eklenmesi desteklenmiyor. 

    Bir premium depolama hesabında bulunabilecek herhangi bir nesne bir sayfa blobu olacaktır. Sayfa blob'u desteklenen sağlanan boyutları birine tutturur. Premium depolama hesabı küçük blobları depolamak için kullanılması amaçlanmamıştır nedeni budur.

* **Premium depolama hesabı**

    Premium Storage'ı kullanmaya başlamak için yönetilmeyen diskler için premium depolama hesabı oluşturun. İçinde [Azure portalında](https://portal.azure.com), premium depolama hesabı oluşturma, seçin **Premium** performans katmanı. Seçin **yerel olarak yedekli depolama (LRS)** çoğaltma seçeneği. Performans katmanı ayarlayarak bir premium depolama hesabı oluşturabilirsiniz **Premium_LRS**. Performans katmanını değiştirmek için aşağıdaki yaklaşımlardan birini kullanın:
     
  - [Azure depolama için PowerShell](../articles/storage/common/storage-powershell-guide-full.md#manage-the-storage-account)
  - [Azure depolama için Azure CLI](../articles/storage/common/storage-azure-cli.md#manage-storage-accounts)
  - [Azure depolama kaynak sağlayıcısı REST API'si](https://docs.microsoft.com/rest/api/storagerp) (Azure Resource Manager dağıtımları için) veya bir Azure depolama kaynak sağlayıcısı istemci kitaplığı

    Premium depolama hesabı sınırları hakkında bilgi edinmek için [ölçeklenebilirlik ve performans hedefleri](#scalability-and-performance-targets).

* **Premium yerel olarak yedekli depolama**

    Premium depolama hesabı yalnızca yerel olarak yedekli depolama çoğaltma seçeneği olarak destekler. Yerel olarak yedekli depolama, tek bir bölgede verilerin üç kopyasını tutar. Bölgesel bir olağanüstü durum kurtarma için farklı bir bölgede sanal makine disklerinizi kullanarak yedeklemelisiniz [Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md). Ayrıca, yedekleme kasası olarak coğrafi olarak yedekli depolama (GRS) hesabı kullanmanız gerekir. 

    Azure depolama hesabınızın yönetilmeyen diskleriniz için bir kapsayıcı kullanır. Yönetilmeyen diskler ile Premium Storage destekleyen Azure VM oluşturma ve bir premium depolama hesabı seçin, işletim sistemi ve veri diskleri bu depolama hesabında depolanır.

## <a name="supported-vms"></a>Desteklenen VM'ler

Premium depolama, çok çeşitli Azure Vm'leri üzerinde desteklenir. Bu VM türleri ile standart ve premium depolama diskleri kullanabilirsiniz. Bu işlem, depolama ile uyumlu Premium olmayan VM serisi ile premium depolama diskleri kullanamazsınız.


Windows için Azure’da VM türleri ve boyutları hakkında bilgi edinmek için bkz. [Windows VM boyutları](../articles/virtual-machines/windows/sizes.md). Linux için Azure’da VM türleri ve boyutları hakkında bilgi edinmek için bkz. [Linux VM boyutları](../articles/virtual-machines/linux/sizes.md).

Bazı bunlar depolama özellikli VM'ler üzerinde premium desteklenen özellikler:

* **Kullanılabilirlik kümesi**

    DS serisi VM örneği kullanarak, yalnızca DS serisi VM'ler içeren bir bulut hizmeti için bir DS serisi VM ekleyebilirsiniz. DS serisi VM'ler, DS serisi sanal makineler dışında herhangi bir türü olan bir bulut hizmetiniz için eklemeyin. Var olan Vhd'lerinizi yalnızca DS serisi Vm'lerde çalışan yeni bir bulut hizmetine geçirebilirsiniz. Aynı sanal IP adresini kullanın, DS serisi sanal makineler barındıran yeni bir bulut hizmeti için kullanmak istiyorsanız [ayrılmış IP adresleri](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).

* **İşletim sistemi diski**

    Premium depolama VM'nizi, premium veya standart işletim sistemi diski kullanacak şekilde ayarlayabilirsiniz. En iyi deneyim için bir Premium depolama tabanlı işletim sistemi diski kullanmanızı öneririz.

* **Veri diskleri**

    Aynı Premium depolama VM, premium ve standart diskleri kullanabilirsiniz. Premium depolama sayesinde, bir VM sağlama ve birden çok kalıcı veri diskleri VM'e ekleyin. Gerekirse birimin performans ve kapasite artırmak için disklerde stripe.

    > [!NOTE]
    > Premium depolama diskleri kullanarak stripe varsa [depolama alanları](https://technet.microsoft.com/library/hh831739.aspx), depolama alanları ' için kullandığınız her disk 1 sütun kümesi. Aksi takdirde, genel performansını şeritli birim nedeniyle trafik düzensiz şekilde dağıtılmasının disklerde beklenenden daha düşük olabilir. Varsayılan olarak, Sunucu Yöneticisi'nde, 8 adede kadar disk sütunları ayarlayabilirsiniz. 8'den fazla disk varsa, birim oluşturmak için PowerShell kullanın. Sütun sayısını el ile belirtin. Daha fazla disk olsa bile Aksi takdirde, Sunucu Yöneticisi kullanıcı Arabirimi 8 sütunları kullanmaya devam eder. Örneğin, tek kümesi 32 diskiniz varsa, 32 sütunları belirtin. Sanal diski kullanan sütun sayısını belirtmek için [New-VirtualDisk](https://technet.microsoft.com/library/hh848643.aspx) PowerShell cmdlet'ini kullanın *NumberOfColumns* parametresi. Daha fazla bilgi için [depolama alanlarına genel bakış](https://technet.microsoft.com/library/hh831739.aspx) ve [depolama alanları sık sorulan sorular](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).

* **Önbellek**

    Premium depolamayı destekler, sanal makineler (VM) aktarım hızı ve gecikme süresinin daha yüksek düzeyde benzersiz bir önbelleğe alma özelliği vardır. Kendi önbelleğe alma özelliği, temel alınan premium depolama diski performansı aşıyor. Tüm VM'ler önbelleğe alma desteği ancak, bu nedenle Lütfen gözden geçirme daha fazla bilgi için ilginizi VM boyutları için VM belirtimleri.  Önbelleğe alma desteği Vm'leri bu kendi spec "Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı" ölçüm ile gösterir.  Bunlar ayrıca doğrudan VM title altında belirtilir.
    
    Önbelleğe alma, önbelleğe alma ilkesi için premium depolama diskler üzerinde disk ayarlayabilirsiniz **salt okunur**, **ReadWrite**, veya **hiçbiri**. Önbelleğe alma ilkesi varsayılan disk **salt okunur** tüm premium veri diskleri için ve **ReadWrite** işletim sistemi diskleri için. En iyi performans için uygulama Lütfen için doğru önbellek ayarını kullandığınızdan emin olun. 
    
    Önbelleğe alma ilkesi için disk örneği SQL Server veri dosyaları gibi okuma yoğunluklu mu salt okunur veri diskleri için ayarladığınız gibi **salt okunur**. Yazma yoğunluklu veya salt yazılır veri diskleri için SQL Server günlük dosyaları gibi ayarlamak için ilkeyi önbelleğe alan disk **hiçbiri**. 
    
    Tasarımınızı Premium depolama ile en iyi duruma getirme hakkında daha fazla bilgi için bkz: [tasarım performans için Premium depolamayla](../articles/virtual-machines/windows/premium-storage-performance.md).

* **Analizler**

    Premium depolama disklerini kullanarak sanal makine performansını çözümlemek için VM tanılamayı açın [Azure portalında](https://portal.azure.com). Daha fazla bilgi için [Azure tanılama uzantısı ile Azure VM izleme](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/). 

    Disk performansını görmek için gibi işletim sistemi tabanlı Araçları [Windows Performans İzleyicisi'ni](https://technet.microsoft.com/library/cc749249.aspx) Windows Vm'leri için ve [iostat](http://linux.die.net/man/1/iostat) Linux VM'ler için komutu.

* **Sanal makine ölçek sınırlarını ve performans**

    Her bir Premium Depolama tarafından desteklenen VM boyutu, Ölçek sınırlarını ve IOPS, bant genişliği ve VM başına eklenebilecek disk sayısını performans özellikleri vardır. İle Vm'leri premium depolama disklerini kullanmak, IOPS ve bant genişliği sürücü diski trafiği için vm'nizdeki yeterli olduğundan emin olun.

    Örneğin, 32 MB/sn premium depolama disk trafiği için ayrılmış bant genişliği STANDARD_DS1 VM vardır. P10 premium depolama diski 100 MB/sn bant genişliği sağlar. P10 premium depolama diski bu sanal Makineye bağlıysa, yalnızca en fazla 32 MB/sn gidebilirsiniz. En fazla 100 MB/P10 disk sağlayabilen sn kullanamazsınız.

    Şu anda, DS serisi, büyük VM Standard_DS15_v2 ' dir. Standard_DS15_v2 tüm disklerde en fazla 960 MB/sn sağlar. En büyük GS serisi VM Standard_GS5 ' dir. Standard_GS5 tüm disklerde en fazla 2.000 MB/sn sağlar.

    Yalnızca disk trafiği için bu limitlerdir. Bu sınırlar, İsabetli Önbellek okuma sayısı ve ağ trafiği dahil değildir. Ayrı bir bant genişliği, VM ağ trafiği için kullanılabilir. Ağ trafiği için bant genişliği, premium depolama diskleri tarafından kullanılan ayrılmış bant genişliği farklıdır.

    En güncel hakkında bilgi için maksimum IOPS ve aktarım hızı (bant) Premium depolama-VM'ler için bkz [Windows VM boyutları](../articles/virtual-machines/windows/sizes.md) veya [Linux VM boyutları](../articles/virtual-machines/linux/sizes.md).

    Premium depolama disklerini ve bunların IOPS ve aktarım hızlı sınırları hakkında daha fazla bilgi için sonraki bölümdeki tabloya bakın.

## <a name="scalability-and-performance-targets"></a>Ölçeklenebilirlik ve performans hedefleri
Bu bölümde, biz Premium depolama kullanırken dikkate alınması gereken ölçeklenebilirlik ve performans hedefleri açıklanmaktadır.

Premium depolama hesapları aşağıdaki ölçeklenebilirlik hedefleri vardır:

| Toplam hesabı kapasitesi | Yerel olarak yedekli depolama hesabı için toplam bant genişliği |
| --- | --- | 
| Disk kapasitesi: 35 TB <br>Anlık görüntü kapasitesi: 10 TB | 50 Gigabit / saniye için yukarı gelen<sup>1</sup> + giden<sup>2</sup> |

<sup>1</sup> bir depolama hesabına gönderilen tüm verileri (istekler)

<sup>2</sup> bir depolama hesabından alınan tüm verileri (yanıtlar)

Daha fazla bilgi için [Azure depolama ölçeklenebilirlik ve performans hedefleri](../articles/storage/common/storage-scalability-targets.md).

Yönetilmeyen diskler için premium depolama hesapları kullandığınız ve uygulamanızı bir tek bir depolama hesabı ölçeklenebilirlik hedefleri aşarsa, yönetilen disklere geçirmek isteyebilirsiniz. Yönetilen disklere geçirmek istemiyorsanız, birden çok depolama hesaplarını kullanmak için uygulamanızı oluşturun. Ardından, bu depolama hesabı arasında veri bölümleme. Örneğin, birden çok VM arasında 51 TB disk eklemek istiyorsanız, bunları iki depolama hesabı arasında yayılabilir. Tek bir premium depolama hesabı için belirlenen sınırı 35 TB'dir. Tek bir premium depolama hesabı hiçbir zaman sağlanan diskleri 35 TB'den fazla olduğundan emin olun.

### <a name="premium-storage-disk-limits"></a>Premium depolama disk limitleri
Premium depolama disk sağlarken, diskin maksimum IOPS ve aktarım hızı (bant) belirler. Azure premium depolama disklerini sekiz GA türlerini sunar: P4 (yönetilen diskler yalnızca), P6 (yönetilen diskler yalnızca), P10, P15 (yönetilen diskler yalnızca), P20, P30, P40 ve P50. Yanı sıra, üç Önizleme disk boyutları: P60 P70 ve P80. Her premium depolama disk türüne, IOPS ve aktarım hızı için belirli sınırları vardır. Disk türleri için sınırlar aşağıdaki tabloda açıklanmıştır:

Bir yıldız işaretiyle gösterilen boyutları şu anda Önizleme aşamasındadır.

| Premium disk türü  | P4    | P6    | P10    | P15    | P20    | P30              | P40             | P50             | P60 *            | P70 *               | P80 *               |
|---------------------|-------|-------|--------|--------|--------|------------------|-----------------|-----------------|-----------------|--------------------|--------------------|
| Disk boyutu           | 32 GiB| 64 GiB| 128 GiB| 256 GiB| 512 GiB| 1024 (1 TiB) giB | 2048 giB (2 tib'a kadar)| 4095 giB (4 tib'a kadar)| 8192 giB (8 tib'a kadar)| 16,384 giB (16 tib'a kadar)| 32.767 giB (32 tib'a kadar)|
| Disk başına IOPS       | 120   | 240   | 500    | 1100   | 2300   | 5000             | 7500            | 7500            | 12,500          | 15.000             | 20.000             |
| Disk başına aktarım hızı | Saniye başına 25 MB | Saniye başına 50 MB | Saniye başına 100 MB | Saniye başına 125 MB | 150 MB / saniye | Saniye başına 200 MB | Saniye başına 250 MB | Saniye başına 250 MB | 480 MB / saniye | Saniye başına 750 MB | Saniye başına 750 MB |

> [!NOTE]
> Açıklanan şekilde yeterli bant genişliği sürücü diski trafiği, sanal makinenizde kullanılabilir olduğundan emin olun [desteklenen VM'ler](#supported-vms). Aksi takdirde, disk aktarım hızı ve IOPS je omezeno değerleri daha düşük. En fazla aktarım hızı ve IOPS sınırları VM, yukarıdaki tabloda açıklanan disk limitleri değil temel alır.  
> Azure Premium depolama platformu yüksek düzeyde paralel olacak şekilde tasarlanmıştır. Uygulamanızın çok iş parçacıklı olacak şekilde tasarlama büyük disk boyutları sunulan yüksek performanslı hedef elde etmek için yardımcı olur.

Premium depolama ölçeklenebilirlik ve performans hedefleri hakkında bilmeniz gereken bazı önemli noktalar şunlardır:

* **Sağlanan kapasite ve performans**

    Standart depolama aksine, bir premium depolama disk sağlarken, kapasite, IOPS ve aktarım hızı bu diskin garanti edilir. Örneğin, P50 disk oluşturursanız, Azure 4.095 GB depolama kapasitesi, 7.500 IOPS ve 250-MB/sn aktarım hızı bu disk için hazırlar. Uygulamanız, kapasite ve performans bölümünü veya tümünü kullanabilirsiniz. Premium SSD diskleri, hedef performans % 99,9 oranında sağlamak üzere tasarlanmıştır.

* **Disk boyutu**

    Azure, en yakın premium depolama disk seçeneğine, önceki bölümdeki tabloda belirtilen disk boyutu (yuvarlanır) eşler. Örneğin, bir disk boyutu 100 GB bir P10 seçeneği olarak sınıflandırılır. Bu en fazla 500 IOPS ile en fazla 100-MB/sn Aktarım gerçekleştirebilirsiniz. Benzer şekilde, bir disk boyutu 400 GB arası P20 sınıflandırılır. 150-MB/sn aktarım hızı ile 2,300 IOPS kadar gerçekleştirebilirsiniz.

    > [!NOTE]
    > Var olan disklerin boyutunu kullanarak bir kolayca artırabilirsiniz. Örneğin, 30 GB disk 128 GB ve hatta 1 TB boyutunu artırmak isteyebilirsiniz. Ya da daha fazla kapasitesi veya daha fazla IOPS ve aktarım hızı gerektiğinden P20 diskinizi P30 diske dönüştürmek isteyebilirsiniz. 

* **G/ç boyutu**

    Bir g/ç boyutu 256 KB'dir. Aktarılan veriler 256 KB'den küçük ise, 1 g/ç birim olarak kabul edilir. Büyük g/ç boyutları sayılır olarak birden çok g/ç boyutu 256 KB. Örneğin, 1.100 KB g/ç 5 g/ç birimi sayılır.

* **Aktarım hızı**

    Aktarım hızı sınırı diske yazma işlemlerini içerir ve önbellekten sunulan olmayan disk okuma işlemleri içerir. Örneğin, bir P10 disk, disk başına 100-MB/sn aktarım hızı sahiptir. Geçerli işleme bir P10 disk için bazı örnekler aşağıdaki tabloda gösterilmiştir:

    | P10 disk başına en fazla aktarım hızı | Diskten olmayan önbellek okuma | Disk önbellek olmayan yazma işlemleri |
    | --- | --- | --- |
    | 100 MB/s | 100 MB/s | 0 |
    | 100 MB/s | 0 | 100 MB/s |
    | 100 MB/s | 60 MB/sn | 40 MB/sn |

* **İsabetli Önbellek okuma sayısı**

    Ayrılmış IOPS veya disk aktarım hızı tarafından İsabetli Önbellek okuma sayısı sınırlı değildir. Örneğin, bir veri diski ile kullandığınızda bir **salt okunur** Premium depolama, önbellekten sunulan okuma tarafından desteklenen bir VM'de önbellek ayarı olmayan IOPS ve aktarım hızı tabi diskin büyük. Genellikle, bir diskin iş yükü ise, okuma, çok yüksek performans alabilirsiniz. Önbellek ayrı IOPS ve aktarım hızı sınırlarına VM, VM boyutunu temel alan düzeyi, tabidir. DS serisi VM'ler, yaklaşık 4.000 IOPS ve önbellek ve yerel SSD g/ç için çekirdek başına 33-MB/sn aktarım hızı sahiptir. GS serisi VM'ler 5.000 IOPS ve önbellek ve yerel SSD g/ç için çekirdek başına 50-MB/sn aktarım hızı bir sınırı vardır.

## <a name="throttling"></a>Azaltma

Uygulama IOPS veya üretilen iş ayrılmış limitlerini premium depolama diski için azaltma, meydana gelebilir. Disk bant genişliği sınırını kullanılabilir VM için VM üzerindeki tüm disklerde toplam disk trafiğiniz aşarsa, azaltma de oluşabilir. Azaltmayı önlemek için bekleyen disk g/ç isteklerinin sayısını sınırla öneririz. Ölçeklenebilirlik ve performans hedefleri, sağlanan disk ve disk bant genişliği VM kullanılabilir dayalı bir sınır kullanın.  

Azaltmayı önlemek için tasarlarken, uygulamanızın en düşük gecikme süresi elde edebilirsiniz. Bekleyen g/ç istekleri disk sayısı çok küçükse, ancak uygulamanızı maksimum IOPS ve disk için kullanılabilir olan aktarım hızı düzeylerine yararlanamaz.

Aşağıdaki örnekler, azaltma düzeyleri hesaplamak nasıl ekleyebileceğiniz gösterilmektedir. Tüm hesaplamalar 256 KB üzerinde bir g/ç birim boyutunu temel alır.

### <a name="example-1"></a>Örnek 1

Uygulamanızı 495 g/ç birimleri 16 KB'lık boyutunun P10 disk üzerindeki bir saniye içinde işledi. G/ç birimleri 495 IOPS sayılır. 2 MB g/ç aynı ikinci çalışırsanız, g/ç birimlerinin toplam 495 + 8 IOPS için eşittir. Bunun nedeni, 2 MB g/ç g/ç birim boyutu 256 KB olduğunda 2.048 KB / 256 KB = 8 g/ç birimler =. 495 + 8 toplam disk için 500 IOPS sınırı aştığından, azaltma gerçekleşir.

### <a name="example-2"></a>Örnek 2

Uygulamanızı bir P10 disk üzerindeki 256 KB'lık boyut 400 g/ç birimleri işledi. Tüketilen toplam bant genişliği (400 &#215; 256) / 1024 KB = 100 MB/sn. Bir P10 disk 100 MB/sn aktarım hızı sınırı vardır. Uygulamanız bu ikinci daha fazla g/ç işlemleri gerçekleştirmeye çalışırsa, ayrılmış sınırı aştığından kısıtlanır.

### <a name="example-3"></a>Örnek 3

İki P30 disk bağlı DS4 VM var. Her P30 disk 200-MB/sn aktarım hızı yeteneğine sahiptir. Ancak, DS4 VM toplam disk 256 MB/sn bant genişliği kapasitesi vardır. Her iki bağlı diskleri için en fazla aktarım hızı bu DS4 VM üzerinde aynı anda sürücüsü olamaz. Bu sorunu çözmek için bir disk üzerindeki 200 MB/sn ve 56 MB/sn diğer disk üzerinde trafiği karşılayabilir. Toplam disk trafiğinizin 256 MB/sn aşması durumunda, disk trafiği azaltılır.

> [!NOTE]
> Disk trafiğiniz çoğunlukla küçük g/ç boyutlarını oluşuyorsa, büyük olasılıkla, uygulamanızın aktarım hızı sınırı önce IOPS sınırı ulaşırsınız. Disk trafiğinin genellikle büyük g/ç boyutlarını oluşuyorsa, ancak büyük olasılıkla, uygulamanızın aktarım hızı sınırı ilk olarak, IOPS sınırı yerine ulaşırsınız. Uygulamanızın IOPS ve üretilen iş kapasitesi en iyi g/ç boyutları kullanarak en üst düzeye çıkarabilirsiniz. Ayrıca, bekleyen bir disk g/ç isteklerinin sayısını sınırlayabilir.

Premium Depolama'yı kullanarak yüksek performans için tasarlama hakkında daha fazla bilgi için bkz: [tasarım performans için Premium depolamayla](../articles/virtual-machines/windows/premium-storage-performance.md).

## <a name="snapshots-and-copy-blob"></a>Anlık görüntü ve kopya blob'u

Depolama hizmeti için VHD dosyasının bir sayfa blobudur. Sayfa blobları anlık ve bunları başka bir konuma gibi farklı bir depolama hesabına kopyalayın.

### <a name="unmanaged-disks"></a>Yönetilmeyen diskler

Oluşturma [artımlı anlık](../articles/virtual-machines/linux/incremental-snapshots.md) ile standart depolama anlık görüntüleri kullanmak aynı şekilde yönetilmeyen premium diskler için. Premium depolama yalnızca yerel olarak yedekli depolama çoğaltma seçeneği olarak destekler. Anlık görüntüler oluşturun ve ardından bir coğrafi olarak yedekli standart depolama hesabı için anlık görüntüleri kopyalayın öneririz. Daha fazla bilgi için [Azure depolama yedekliliği seçenekleri](../articles/storage/common/storage-redundancy.md).

Bir VM'ye ekli bir diskin, disk üzerinde bazı API işlemleri izin verilmez. Örneğin, gerçekleştiremezsiniz bir [kopya blob'u](/rest/api/storageservices/Copy-Blob) disk sanal Makineye bağlıysa, blob üzerinde işlem. Bunun yerine, önce bu blob anlık görüntüsünü kullanarak oluşturmanız [anlık görüntü Blob](/rest/api/storageservices/Snapshot-Blob) REST API. Daha sonra gerçekleştirmek [kopya blob'u](/rest/api/storageservices/Copy-Blob) bağlı disk kopyalamak için anlık görüntü. Alternatif olarak, disk ayırma ve gerekli işlemleri gerçekleştirin.

Premium depolama blob anlık görüntüleri için aşağıdaki sınırlar geçerlidir:

| Premium depolama sınırı | Değer |
| --- | --- |
| Blob başına anlık görüntü sayısı | 100 |
| Anlık görüntüler için depolama hesabı kapasitesi<br>(Yalnızca anlık görüntü verileri içerir. "Veri temel bir blobun içermez.) | 10 TB |
| Art arda anlık görüntü arasındaki minimum süre | 10 dakika |

Coğrafi olarak yedekli, anlık görüntü kopyalarını bulundurmak için anlık görüntüler bir premium depolama hesabından bir coğrafi olarak yedekli standart depolama hesabı için AzCopy veya kopya blob'u kullanarak kopyalayabilirsiniz. Daha fazla bilgi için [AzCopy komut satırı yardımcı programı ile veri aktarma](../articles/storage/common/storage-use-azcopy.md) ve [kopya blob'u](/rest/api/storageservices/Copy-Blob).

Bir premium depolama hesabında sayfa BLOB'ları karşı REST işlemlerini gerçekleştirme hakkında ayrıntılı bilgi için bkz: [Blob hizmeti işlemleri Azure Premium depolama ile](https://go.microsoft.com/fwlink/?LinkId=521969).

### <a name="managed-disks"></a>Yönetilen diskler

Yönetilen disk için anlık görüntü, yönetilen diskin salt okunur bir kopyasıdır. Anlık görüntü, standart yönetilen disk olarak depolanır. Şu anda [artımlı anlık](../articles/virtual-machines/windows/incremental-snapshots.md) yönetilen diskler için desteklenmez. Yönetilen disk için anlık öğrenmek için bkz. [Azure yönetilen olarak depolanmış VHD kopyası oluşturma yönetilen anlık görüntüleri kullanarak Windows disk](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md) veya [Azure yönetilen olarak depolanmış VHD kopyası oluşturma kullanarak yönetilen disk Linux'ta anlık görüntüleri](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md).

Yönetilen disk sanal Makineye bağlıysa, bazı API işlemleri disk üzerinde izin verilmez. Örneğin, bir paylaşılan erişim imzası (SAS) disk sanal Makineye bağlı durumdayken, bir kopyalama işlemini gerçekleştirmek için oluşturulamıyor. Bunun yerine, ilk diskin anlık görüntüsünü oluşturun ve ardından anlık görüntü kopyası gerçekleştirin. Alternatif olarak, disk ayırma ve ardından kopyalama işlemini gerçekleştirmek için bir SAS oluşturun.


## <a name="premium-storage-for-linux-vms"></a>Linux Vm'leri için Premium depolama
Premium depolama alanında Linux Vm'lerinizi ayarlamanıza yardımcı olması için aşağıdaki bilgileri kullanabilirsiniz:

Önbellek ile tüm premium depolama disklerini ayarlayın Premium depolama ölçeklenebilirlik hedefleri başarmanın **salt okunur** veya **hiçbiri**, dosya sistemi bağladığınızda "engelleri" devre dışı bırakmanız gerekir. Premium depolama disklerini yazma işlemleri için bu önbellek ayarlarını dayanıklı olduğundan bu senaryoda engelleri gerekmez. Yazma isteği başarıyla tamamlandığında, kalıcı depoya veri yazıldı. "Engelleri" devre dışı bırakmak için aşağıdaki yöntemlerden birini kullanın. Dosya sistemi için bir tane seçin:
  
* İçin **reiserFS**engelleri devre dışı bırakmak, kullanmak için `barrier=none` bağlama seçeneği. (Engellerini etkinleştirmek için `barrier=flush`.)
* İçin **ext3/ext4**engelleri devre dışı bırakmak, kullanmak için `barrier=0` bağlama seçeneği. (Engellerini etkinleştirmek için `barrier=1`.)
* İçin **XFS**engelleri devre dışı bırakmak, kullanmak için `nobarrier` bağlama seçeneği. (Engellerini etkinleştirmek için `barrier`.)
* Önbellek ile diskler için Premium depolama kümesine **ReadWrite**, engelleri yazma dayanıklılık için etkinleştirin.
* Birim etiketleri VM yeniden başlatıldıktan sonra kalıcı olması diskleri evrensel benzersiz tanımlayıcı (UUID) başvurular /etc/fstab güncelleştirmeniz gerekir. Daha fazla bilgi için [bir Linux VM'ye yönetilen disk ekleme](../articles/virtual-machines/linux/add-disk.md).

Aşağıdaki Linux dağıtımları için Azure Premium depolama doğrulandı. Daha iyi performans ve kararlılık Premium depolama ile sanal makineleriniz için en azından bu sürümlerinden birini (veya sonraki bir sürümü) yükseltmenizi öneririz. Bazı sürümleri, Azure için en son Linux Integration Services (LIS), v4.0, gerektirir. İndirmek ve bir dağıtım yüklemek için aşağıdaki tabloda listelenen bağlantı izleyin. Biz doğrulama tamamlandı olarak listeye görüntüleri ekleriz. Performans için her görüntü değişir bizim doğrulamaları Göster unutmayın. Performans, iş yükü özelliklerine ve görüntü ayarlarınızı bağlıdır. Farklı görüntüleri farklı türde iş yükleri için ayarlanmıştır.

| Dağıtım | Version | Desteklenen bir çekirdek | Ayrıntılar |
| --- | --- | --- | --- |
| Ubuntu | 12.04 | 3.2.0-75.110+ | Ubuntu-12_04_5-LTS-amd64-server-20150119-en-us-30GB |
| Ubuntu | 14.04 | 3.13.0-44.73+ | Ubuntu-14_04_1-LTS-amd64-server-20150123-en-us-30GB |
| Debian | 7.x, 8.x | 3.16.7-ckt4-1+ | &nbsp; |
| SUSE | SLES 12| 3.12.36-38.1+| SuSE-sles-12-öncelik-v20150213 <br> sles 12 v20150213 SuSE |
| SUSE | SLES 11 SP4 | 3.0.101-0.63.1+ | &nbsp; |
| CoreOS | 584.0.0+| 3.18.4+ | CoreOS 584.0.0 |
| CentOS | 6.5, 6.6, 6.7, 7.0 | &nbsp; | [Gerekli LIS4](https://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Sonraki bölümde nota bakın* |
| CentOS | 7.1+ | 3.10.0-229.1.2.el7+ | [Önerilen LIS4](https://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Sonraki bölümde nota bakın* |
| Red Hat Enterprise Linux (RHEL) | 6.8+, 7.2+ | &nbsp; | &nbsp; |
| Oracle | 6.0+, 7.2+ | &nbsp; | UEK4 veya RHCK |
| Oracle | 7.0-7.1 | &nbsp; | UEK4 veya çakışabilmektedir RHCK[LIS 4.1 +](https://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |
| Oracle | 6.4-6.7 | &nbsp; | UEK4 veya çakışabilmektedir RHCK[LIS 4.1 +](https://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |


### <a name="lis-drivers-for-openlogic-centos"></a>OpenLogic CentOS LIS sürücüleri

OpenLogic CentOS sanal makineleri çalıştırıyorsanız, en son sürücüleri yüklemek için aşağıdaki komutu çalıştırın:

```
sudo rpm -e hypervkvpd  ## (Might return an error if not installed. That's OK.)
sudo yum install microsoft-hyper-v
```

Yeni sürücülerin etkinleştirmek için VM'yi yeniden başlatın.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Premium depolama kullandığınızda, aşağıdaki fatura değerlendirmeleri geçerlidir:

* **Premium depolama diski ve blob boyutu**

    Bir premium depolama disk veya blob faturalandırması sağlanan blobu ve disk boyutuna bağlıdır. Azure en yakın premium depolama disk seçeneğine (yuvarlanır) sağlanan boyut eşler. Ayrıntılar için bölümündeki tabloya bakın [ölçeklenebilirlik ve performans hedefleri](#scalability-and-performance-targets). Her disk desteklenen sağlanan disk boyutuna eşlenir ve buna göre faturalandırılır. Sağlanmış bir diski için faturalama, saatlere Premium depolama teklif için aylık fiyat kullanılarak eşit olarak dağıtılır. Örneğin, bir P10 disk sağlanır ve 20 saat sonra silindi, P10 teklif 20 saat eşit olarak için fatura edilir. Disk veya IOPS ve aktarım hızı kullanılan yazılan gerçek veri miktarından bağımsız olarak budur.

* **Premium yönetilmeyen diskler, anlık görüntüler**

    Premium yönetilmeyen diskler üzerindeki anlık görüntüler, anlık görüntüleri tarafından kullanılan ek kapasite için faturalandırılır. Anlık görüntüler hakkında daha fazla bilgi için bkz. [bir blobun anlık görüntüsünü oluşturma](/rest/api/storageservices/Snapshot-Blob).

* **Premium yönetilen disk anlık görüntüleri**

    Bir yönetilen diskin anlık görüntüsünü, diskin salt okunur bir kopyasıdır. Disk, standart yönetilen disk olarak depolanır. Standart yönetilen disk olarak anlık görüntüsünü aynı maliyeti. Örneğin, 128 GB premium yönetilen disk anlık görüntüsünü alın, 128 GB standart yönetilen disk için anlık görüntü maliyetini eşdeğerdir.  

* **Giden veri aktarımları**

    [Giden veri aktarımları](https://azure.microsoft.com/pricing/details/data-transfers/) (Azure veri merkezlerinden giden veriler) bant genişliği kullanımı için fatura doğurur.

Premium depolama Vm'leri Premium Depolama tarafından desteklenen ve yönetilen diskler için fiyatlandırma hakkında ayrıntılı bilgi için şu makalelere bakın:

* [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/)
* [Sanal makineleri fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Yönetilen diskler fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks/)

## <a name="azure-backup-support"></a>Azure yedekleme desteği

Bölgesel bir olağanüstü durum kurtarma için farklı bir bölgede sanal makine disklerinizi kullanarak yedeklemelisiniz [Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md) ve GRS depolama hesabı olarak bir yedekleme kasası.

Zaman tabanlı Yedekleme ile bir yedekleme işi oluşturmak için kolay VM geri yükleme ve yedek bekletme ilkeleri, Azure yedekleme kullanın. Her iki yedekleme ile yönetilmeyen ve yönetilen diskleri kullanabilirsiniz. Daha fazla bilgi için [yönetilmeyen disklere sahip VM'ler için Azure Backup](../articles/backup/backup-azure-vms-first-look-arm.md) ve [yönetilen disklere sahip VM'ler için Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup). 

## <a name="next-steps"></a>Sonraki adımlar

Premium depolama hakkında daha fazla bilgi için aşağıdaki makalelere bakın.
