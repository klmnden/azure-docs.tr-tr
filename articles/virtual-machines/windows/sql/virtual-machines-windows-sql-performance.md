---
title: Azure SQL Server için performans en iyi uygulamaları | Microsoft Docs
description: Microsoft Azure vm'lerinde SQL Server performansını iyileştirmek için en iyi yöntemler sağlar.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a0c85092-2113-4982-b73a-4e80160bac36
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/19/2018
ms.author: jroth
ms.openlocfilehash: 9d3fbbab76f16a8546c431d5acf913bf419edeb4
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31798164"
---
# <a name="performance-best-practices-for-sql-server-in-azure-virtual-machines"></a>Azure Sanal Makinelerde SQL Server için performansa yönelik en iyi yöntemler

## <a name="overview"></a>Genel Bakış

Bu makalede, Microsoft Azure sanal makinesi'nın SQL Server performansını iyileştirmek için yönergeler sağlar. Azure sanal makinelerde SQL Server çalışırken, şirket içi sunucu ortamında SQL Server için geçerli olan seçenekleri ayarlama aynı veritabanı performansını kullanmaya devam öneririz. Ancak, genel bulutta ilişkisel veritabanı performansını bir sanal makine boyutunu ve veri diskleri yapılandırması gibi birçok faktöre bağlıdır.

[Azure Portalı'nda sağlanan SQL Server görüntülerinin](quickstart-sql-vm-create-portal.md) depolama yapılandırma en iyi uygulamaları izleyin. Depolama nasıl yapılandırılacağı ile ilgili daha fazla bilgi için bkz: [SQL Server VM'ler için depolama yapılandırması](virtual-machines-windows-sql-server-storage-configuration.md). Sağladıktan sonra bu makalede ele alınan diğer en iyi duruma getirme uygulamayı düşünün. Seçenekleriniz, iş yüküne temel ve test aracılığıyla doğrulayın.

> [!TIP]
> Bu makalede odaklanmıştır *en iyi* Azure Vm'lerde SQL Server için performans. İş yükünüzün daha az yoğun ise, aşağıda listelenen her iyileştirme gerektirmeyebilecek. Bu öneriler değerlendirirken yükünün desenleri ve performans gereksinimlerini göz önünde bulundurun.

## <a name="quick-check-list"></a>Hızlı onay listesi

Azure Virtual Machines'de SQL Server'ın en iyi performans için bir hızlı onay listesi aşağıdadır:

| Alan | En iyi duruma getirme |
| --- | --- |
| [VM boyutu](#vm-size-guidance) |[DS3](../sizes-general.md) veya SQL Enterprise edition için daha yüksek.<br/><br/>[DS2](../sizes-general.md) veya SQL Standard ve Web sürümleri için daha yüksek. |
| [Depolama](#storage-guidance) |Kullanım [Premium depolama](../premium-storage.md). Standart depolama yalnızca geliştirme ve test için önerilir.<br/><br/>Tutmak [depolama hesabı](../../../storage/common/storage-create-storage-account.md) ve SQL Server VM ile aynı bölgede.<br/><br/>Azure devre dışı [coğrafi olarak yedekli depolama](../../../storage/common/storage-redundancy.md) (coğrafi çoğaltma) depolama hesabı üzerinde. |
| [Diskleri](#disks-guidance) |En az 2 kullanmak [P30 diskleri](../premium-storage.md#scalability-and-performance-targets) (günlük dosyaları ve veri dosyalarını ve TempDB; veya stripe iki 1 veya daha fazla disk ve tüm dosyaların tek bir birimde depolama için 1).<br/><br/>Veritabanı depolama veya günlük için işletim sistemi veya geçici diskleri kullanmaktan kaçının.<br/><br/>Veri dosyaları ve TempDB veri dosyalarını barındıran diskler üzerinde okuma önbelleğe almayı etkinleştir.<br/><br/>Günlük dosyası barındırma diskler üzerinde önbelleğe alma etkinleştirmeyin.<br/><br/>Önemli: bir Azure VM disk önbellek ayarlarını değiştirirken SQL Server hizmetini durdurun.<br/><br/>Daha yüksek g/ç işleme almak için birden çok Azure veri diski şeritler.<br/><br/>Belgelenen ayırma boyutlarıyla biçimlendirin. |
| [G/Ç](#io-guidance) |Veritabanı Sayfa sıkıştırmayı etkinleştirin.<br/><br/>Veri dosyaları için anında dosya başlatma etkinleştirin.<br/><br/>Veritabanında autogrowing sınırlayın.<br/><br/>Veritabanında daralma devre dışı bırakın.<br/><br/>Sistem veritabanları dahil olmak üzere veri diskleri için tüm veritabanlarını taşıyın.<br/><br/>SQL Server hata günlüğü ve izleme dosya dizinleri veri diskleri için taşıyın.<br/><br/>Varsayılan yedekleme ve veritabanı dosyası konumlarını ayarlayın.<br/><br/>Kilitli sayfalar etkinleştirin.<br/><br/>SQL Server performans düzeltmeleri uygulayın. |
| [Özelliğe özgü](#feature-specific-guidance) |Doğrudan blob depolama alanına yedekleyebilir. |

Daha fazla bilgi için *nasıl* ve *neden* bu iyileştirmeler olmak için lütfen ayrıntıları ve aşağıdaki bölümlerde verilen yönergeleri gözden geçirin.

## <a name="vm-size-guidance"></a>VM boyutu Kılavuzu

Performans hassas uygulamalar için aşağıdaki kullanmanız önerilir [sanal makine boyutları](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json):

* **SQL Server Enterprise Edition**: DS3 veya üzeri
* **SQL Server Standard ve Web sürümleri**: DS2 veya üzeri

## <a name="storage-guidance"></a>Depolama Kılavuzu

DS serisi (birlikte, DSv2 serisi ve GS serisi) VM'ler Destek [Premium depolama](../premium-storage.md). Premium depolama tüm üretim iş yükleri için önerilir.

> [!WARNING]
> Standart depolama değişen gecikmeleri ve bant genişliği vardır ve yalnızca geliştirme ve test iş yükleri için önerilir. Premium Storage üretim iş yükleri kullanmanız gerekir.

Ayrıca, aktarım gecikmelerini azaltmak için SQL Server sanal makineleri aynı veri merkezinde Azure depolama hesabınızın oluşturmanızı öneririz. Birden çok disk arasında tutarlı yazma sipariş yıkıcıları gibi bir depolama hesabı oluştururken, coğrafi çoğaltma devre dışı bırakın. Bunun yerine, bir SQL Server olağanüstü durum kurtarma teknoloji iki Azure veri merkezleri arasında yapılandırmayı göz önünde bulundurun. Daha fazla bilgi için bkz: [yüksek kullanılabilirlik ve olağanüstü durum kurtarma Azure Virtual Machines'de SQL Server için](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Diskleri Kılavuzu

Azure VM temelinde üç ana disk türleri şunlardır:

* **İşletim sistemi diski**: bir Azure sanal makine oluşturduğunuzda, platform en az bir disk ekleme (olarak etiketli **C** sürücü), işletim sistemi diski VM'ye. Bu disk, depolama birimindeki bir sayfa blob'u olarak kaydedilen bir vhd'dir.
* **Geçici disk**: Azure sanal makineleri içeren geçici disk adlı başka bir diske (olarak etiketli **D**: sürücü). Bir disk için boş alanın kullanılabilir düğüm üzerinde budur.
* **Veri diskleri**: da ek diskleri, sanal makine veri diski ekleyebilirsiniz ve bunlar sayfa BLOB olarak depolama alanında depolanır.

Aşağıdaki bölümlerde bu farklı diskler kullanarak önerileri açıklanmaktadır.

### <a name="operating-system-disk"></a>İşletim sistemi diski

Bir işletim sistemi diski, önyükleme ve bağlama çalışan bir işletim sistemi sürümü olarak bir VHD olduğunu ve olarak etiketli **C** sürücü.

Varsayılan işletim sistemi diski ilkesindeki önbelleğe alma **okuma/yazma**. Performans hassas uygulamalar için işletim sistemi diski yerine veri diskleri kullanmanızı öneririz. Aşağıdaki veri diskler üzerinde bölümüne bakın.

### <a name="temporary-disk"></a>Geçici disk

Olarak etiketli geçici depolama sürücüsü **D**: sürücü, Azure blob depolama alanına kalıcı yapılmaz. Kullanıcı veritabanı dosyaları veya kullanıcı işlem günlüğü dosyalarını depolamayın **D**: sürücü.

D-serisi, Dv2-serisi ve G-serisi VM'ler için geçici bu vm'lerde SSD tabanlı sürücüdür. İş yükünüzün TempDB yoğun olarak kullanılır (örneğin, geçici nesneler veya karmaşık birleştirmeler) yaparsa TempDB depolama **D** sürücü neden yüksek TempDB performansı ve TempDB gecikme süresini azaltın.

Premium depolama (DS serisi, DSv2 serisi ve GS serisi) desteği VM'ler için etkinleştirilmiş okuma önbelleği ile Premium Storage destekleyen bir diskte TempDB depolamanızı öneririz. Bu öneri için bir istisna vardır; TempDB kullanımınızı yazma yoğunluklu ise, yerel TempDB depolayarak daha yüksek performans elde edebilirsiniz **D** de bu makine boyutlarına SSD tabanlı sürücü.

### <a name="data-disks"></a>Veri diskleri

* **Veri diskleri veri ve günlük dosyaları için kullanan**: disk şeritleme kullanmıyorsanız, iki Premium depolama kullanan [P30 diskleri](../premium-storage.md#scalability-and-performance-targets) burada bir disk günlük dosyalarını ve diğer TempDB dosyaları ve verileri içerir. Her Premium depolama diskini Iops'yi ve bant genişliği (MB/sn) boyutuna bağlı olarak birkaç makalesinde açıklandığı gibi sağlar [diskler için Premium depolama alanını kullanarak](../premium-storage.md). Depolama alanları gibi bir disk şeritleme teknik kullanıyorsanız, tüm verileri yerleştirin ve günlük dosyalarını aynı sürücüde öneririz.

   > [!NOTE]
   > Bir SQL Server VM portalında sağladığınızda, depolama yapılandırmanızı düzenleme seçeneğiniz vardır. Yapılandırmanıza bağlı olarak, bir veya daha fazla disk Azure yapılandırır. Birden çok disk şeritleme ile tek bir depolama havuzu birleştirilir. Veri ve günlük dosyaları, bu yapılandırmada birlikte bulunur. Daha fazla bilgi için bkz: [SQL Server VM'ler için depolama yapılandırması](virtual-machines-windows-sql-server-storage-configuration.md).

* **Disk şeritleme**: daha fazla verimlilik için ek veri disklerinin ekleyebilir ve Disk şeritleme kullanabilirsiniz. Veri diskleri sayısını belirlemek için IOP ve günlük dosyalarınızı ve verilerinizi ve TempDB dosyalar için gereken bant sayısını çözümlemek gerekir. Farklı VM boyutları IOPS ve desteklenen bant genişliği sayısına farklı sınırlar olduğunu fark, tablo başına IOPS bakın [VM boyutu](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Aşağıdaki yönergeleri kullanın:

  * Windows 8/Windows Server 2012 veya daha sonra kullanmak [depolama alanları](https://technet.microsoft.com/library/hh831739.aspx) aşağıdaki yönergeleri ile:

      1. OLTP iş yükleri için 64 KB (65536 bayt) ve veri ambarı iş yükleri için 256 KB (262144 bayt) performans etkisi bölüm uyuşmazlığın nedeniyle önlemek için ayırma (stripe boyutu) ayarlayın. PowerShell ile ayarlamanız gerekir.
      1. Ayarlama sütun sayısı = fiziksel disk sayısı. Birden fazla 8 disk (Sunucu Yöneticisi kullanıcı Arabirimi olmayan) yapılandırırken PowerShell kullanın. 

    Örneğin, aşağıdaki PowerShell ayırma boyutu 64 KB ile 2 için sütun sayısı ile yeni bir depolama havuzu oluşturur:

    ```powershell
    $PoolCount = Get-PhysicalDisk -CanPool $True
    $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

    New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false 
    ```

  * Windows 2008 R2 veya önceki sürümleri, Şerit boyutunun her zaman 64 KB'tır ve dinamik diskler (işletim sistemi şeritli birimler) kullanabilirsiniz. Bu seçenek Windows 8/Windows sürümünden itibaren Server 2012 kullanım dışıdır unutmayın. İçin destek ifadesine bilgi [Sanal Disk Hizmeti Windows Depolama Yönetimi API için geçiş](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

  * Kullanıyorsanız [depolama alanları doğrudan (S2D)](/windows-server/storage/storage-spaces/storage-spaces-direct-in-vm) gibi bir senaryoyla [SQL Server Yük devretme kümesi örnekleri](virtual-machines-windows-portal-sql-create-failover-cluster.md), tek bir havuzu yapılandırmanız gerekir. Farklı birimler üzerinde tek havuzun oluşturulabilir olsa da, bunlar tüm aynı önbellek İlkesi gibi aynı özellikleri paylaşır olduğunu unutmayın.

  * Depolama havuzunuz yük beklentilerinizi ilişkili disk sayısını belirler. Farklı VM boyutları eklenen veri disklerini farklı sayıda izin göz önünde bulundurun. Daha fazla bilgi için bkz: [sanal makineler için Boyutlar](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

  * Premium depolama (dev/test senaryoları) kullanmıyorsanız, veri diski tarafından desteklenen maksimum sayısı eklemek için önerilir, [VM boyutu](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve Disk şeritleme kullanın.

* **İlke önbelleği**: İlke depolama yapılandırmanıza bağlı olarak önbelleğe alma için aşağıdaki önerileri unutmayın.

  * Veri ve günlük dosyaları için ayrı diskler kullanıyorsanız, okuma, veri dosyalarınızı ve TempDB veri dosyalarını barındıran veri disklerde önbelleğe almayı etkinleştir. Bu, bir önemli performans avantajı neden olabilir. Bu küçük bir performans düşüklüğü neden olarak günlük dosyası bulunduran disk önbelleğe alma etkinleştirmeyin.

  * Disk şeritleme kullanıyorsanız, çoğu iş yükleri okuma önbelleği yararlı olacaktır. Günlük dosyası aynı sürücüde olsa bile disk şeritleme ile performans kazancı nedeniyle bu öneri geçerlidir. Belirli ağır yazma iş yüklerini önbelleğe alma ile daha iyi performans elde. Bu, test aracılığıyla yalnızca belirlenebilir.

  * Önceki önerileri Premium depolama diskleri için geçerlidir. Premium depolama kullanmıyorsanız, tüm veri disklerde önbelleğe alma etkinleştirmeyin.

  * Disk önbelleği yapılandırma ile ilgili yönergeler için aşağıdaki makalelere bakın. Klasik (ASM) dağıtım modeli için bkz: [kümesi AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) ve [kümesi AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx). Azure Resource Manager dağıtım modeli için bkz: [kümesi AzureRMOSDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk?view=azurermps-4.4.1) ve [kümesi AzureRMVMDataDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmdatadisk?view=azurermps-4.4.1).

     > [!WARNING]
     > Veritabanında bozulma olasılığını önlemek için Azure VM Disk önbellek ayarı değiştirirken SQL Server hizmetini durdurun.

* **NTFS ayırma birimi boyutu**: veri diski biçimlendirme sırasında TempDB yanı sıra veri ve günlük dosyaları için 64 KB ayırma birimi boyutu kullanmanız önerilir.

* **Disk Yönetimi en iyi uygulamaları**: ne zaman bir veri diski kaldırma veya değiştirme, önbellek türü SQL Server hizmetini durdurun sırasında değişiklik. Önbelleğe alma ayarlarını işletim sistemi disk üzerinde değiştirildiğinde Azure VM durdurur, önbellek türü değiştirir ve VM'yi yeniden başlatır. Bir veri diski önbellek ayarlarını değiştirildiğinde VM durdurulmadı, ancak veri diski sanal makineden değişiklik sırasında ayrılmış ve ardından yeniden.

  > [!WARNING]
  > Bu işlemleri sırasında SQL Server hizmetini durdurmak için hata veritabanında bozulmaya neden olabilir.

## <a name="io-guidance"></a>G/ç Kılavuzu

* Uygulama ve istekleri paralel hale, Premium depolama en iyi sonuçları elde edilir. Premium depolama (depolama yoğun olan olsa bile) tek iş parçacıklı seri istekler için çok az kayıpla veya hiç performans artışı göreceğiniz şekilde GÇ sıra derinliği 1'den büyük olduğu senaryolar için tasarlanmıştır. Örneğin, bu performans çözümleme araçları SQLIO gibi tek iş parçacıklı test sonuçlarını etkileyebilir.

* Kullanmayı [veritabanı sayfa sıkıştırma](https://msdn.microsoft.com/library/cc280449.aspx) olarak g/ç yoğun iş yüklerinin performansını artırmaya yardımcı olabilir. Ancak, veri sıkıştırma veritabanı sunucusundaki CPU tüketimi artırabilir.

* İlk dosya ayırma için gereken süreyi azaltmak anında dosya başlatma etkinleştirmeyi düşünün. Anında dosya başlatma yararlanmak için SQL Server (MSSQLSERVER) hizmeti hesabıyla SE_MANAGE_VOLUME_NAME vermek ve ekleyin **Birim bakım görevlerini gerçekleştirme** güvenlik ilkesi. Azure için bir SQL Server platform görüntüsü kullanıyorsanız, varsayılan hizmet hesabı (NT Service\MSSQLSERVER) için eklenmez **Birim bakım görevlerini gerçekleştirme** güvenlik ilkesi. Diğer bir deyişle, anında dosya başlatma bir SQL Server Azure platform görüntüsünde etkin değil. SQL Server hizmet hesabına ekledikten sonra **Birim bakım görevlerini gerçekleştirme** güvenlik ilkesi, SQL Server hizmetini yeniden başlatın. Bu özelliği kullanmak için güvenlik konuları olabilir. Daha fazla bilgi için bkz: [veritabanı dosyası başlatma](https://msdn.microsoft.com/library/ms175935.aspx).

* **otomatik büyüme** beklenmeyen büyüme için yalnızca bir yedek olarak değerlendirilir. Veri ve günlük büyüme otomatik büyüme ile günlük aralıklarla yönetmez. Otomatik büyüme kullanılırsa, dosyanın boyutu anahtarı kullanarak önceden artar.

* Emin olun **daralma** performansını olumsuz yönde etkileyebilir gereksiz yükünü önlemek için devre dışı bırakılır.

* Sistem veritabanları dahil olmak üzere veri diskleri için tüm veritabanlarını taşıyın. Daha fazla bilgi için bkz: [sistem veritabanlarını taşıma](https://msdn.microsoft.com/library/ms345408.aspx).

* SQL Server hata günlüğü ve izleme dosya dizinleri veri diskleri için taşıyın. Bu SQL Server Configuration Manager'ın SQL Server örneğinizi sağ tıklatıp Özellikler'i seçerek yapılabilir. Hata günlüğü ve izleme dosyası ayarları değiştirilebilir **başlangıç parametreleri** sekmesi. Döküm dizini belirtilen **Gelişmiş** sekmesi. Aşağıdaki ekran görüntüsünde hata günlüğü başlangıç parametresi için nereye gösterir.

    ![SQL hata günlüğüne ekran görüntüsü](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

* Varsayılan yedekleme ve veritabanı dosyası konumlarını ayarlayın. Bu makalede önerileri kullanın ve sunucu özellikleri penceresinde değişiklikleri yapın. Yönergeler için bkz: [görüntülemek veya veri ve günlük dosyaları (SQL Server Management Studio) için varsayılan konumları Değiştir](https://msdn.microsoft.com/library/dd206993.aspx). Aşağıdaki ekran görüntüsünde, bu değişiklikleri yapmak nereye gösterir.

    ![SQL veri günlüğü ve yedekleme dosyaları](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)
* G/ç ve disk belleği etkinlikler azaltmak kilitli sayfalar etkinleştirin. Daha fazla bilgi için bkz: [bellek seçeneği (Windows) kilit sayfalarında etkinleştirmek](https://msdn.microsoft.com/library/ms190730.aspx).

* SQL Server 2012 çalıştırıyorsanız, Service Pack 1 Cumulative Update 10 yükleyin. SQL Server 2012'de geçici table deyimi içinde seçin yürüttüğünüzde Bu güncelleştirme performansın g/ç üzerinde için düzeltmeyi içerir. Bu bilgi için [Bilgi Bankası makalesi](http://support.microsoft.com/kb/2958012).

* Tüm veri dosyalarını Azure içeri/dışarı aktarılırken sıkıştırmayı göz önünde bulundurun.

## <a name="feature-specific-guidance"></a>Özellik özgü yönergeler

Bazı dağıtımlarda daha gelişmiş yapılandırma teknikleri kullanarak ek performans avantajı elde edebilirsiniz. Aşağıdaki listede, daha iyi performans elde etmek için yardımcı olabilecek bazı SQL Server özelliklerini vurgular:

* **Azure depolama yedekleme**: Azure sanal makinelerinde çalışan SQL Server için yedeklemeleri gerçekleştirirken kullanabileceğiniz [SQL Server Yedekleme URL'ye](https://msdn.microsoft.com/library/dn435916.aspx). Bu özellik SQL Server 2012 SP1 CU2 ile başlayan kullanılabilir ve eklenen veri disklerini yedekleme için önerilir. Ne zaman, yedekleme/geri yükleme/Azure depolama biriminden izleyin, sağlanan öneriler [SQL Server Yedekleme URL en iyi yöntemler ve sorun giderme ve geri yükleme için yedeklemeler saklanmasını Azure storage'da](https://msdn.microsoft.com/library/jj919149.aspx). Ayrıca bu yedeklemeler kullanarak otomatikleştirebilirsiniz [Azure Virtual Machines'de SQL Server için otomatik yedekleme](virtual-machines-windows-sql-automated-backup.md).

    SQL Server 2012'den önce kullandığınız [SQL Server Yedekleme Azure aracı](https://www.microsoft.com/download/details.aspx?id=40740). Bu araç, birden çok yedek stripe hedef kullanarak yedekleme verimliliğini artırmak için yardımcı olabilir.

* **SQL Server veri dosyaları azure'da**: Bu yeni özellik [azure'da SQL Server veri dosyaları](https://msdn.microsoft.com/library/dn385720.aspx), SQL Server 2014 ile başlayarak kullanılabilir. Azure veri dosyaları ile SQL Server çalıştıran Azure veri diskleri kullanarak olarak karşılaştırılabilir performans özellikleri gösterir.

## <a name="next-steps"></a>Sonraki Adımlar

En iyi yöntemler için bkz: [Azure Virtual Machines'de SQL Server için güvenlik konuları](virtual-machines-windows-sql-security.md).

Diğer SQL Server sanal makinesine makalelerini gözden [Azure sanal makineleri genel bakış SQL Server'da](virtual-machines-windows-sql-server-iaas-overview.md). SQL Server sanal makineleri hakkında sorularınız olursa [Sık Sorulan Sorular](virtual-machines-windows-sql-server-iaas-faq.md) bölümüne bakın.
