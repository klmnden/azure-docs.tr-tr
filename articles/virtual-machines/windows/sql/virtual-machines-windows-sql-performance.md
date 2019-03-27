---
title: Azure'da SQL Server için performans yönergeleri | Microsoft Docs
description: Microsoft Azure sanal makinelerinde SQL Server performansını iyileştirmek için yönergeler sağlar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a0c85092-2113-4982-b73a-4e80160bac36
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 09/26/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 8d31f04c355b47720a1c9b0334042ba2f6654768
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58448567"
---
# <a name="performance-guidelines-for-sql-server-in-azure-virtual-machines"></a>Azure sanal Makineler'de SQL Server için performans yönergeleri

## <a name="overview"></a>Genel Bakış

Bu makalede, Microsoft Azure sanal Makine'de SQL Server performansını iyileştirmek için yönergeler sağlar. Azure sanal Makineler'de SQL Server çalıştırırken, aynı veritabanı performans için şirket içi sunucu ortamında SQL Server için geçerli olan seçenekleri ayarlama kullanarak devam öneririz. Bununla birlikte genel buluttaki bir ilişkisel veritabanının performansı, sanal makinenin boyutu ve veri disklerinin yapılandırması gibi birçok faktöre bağlıdır.

[SQL Server görüntülerini Azure Portalı'nda sağlanan](quickstart-sql-vm-create-portal.md) genel depolama yapılandırması en iyi uygulamaları izleyin (depolama nasıl yapılandırılacağı ile ilgili daha fazla bilgi için bkz: [SQL Server Vm'leri için depolama yapılandırması](virtual-machines-windows-sql-server-storage-configuration.md)). Sağladıktan sonra bu makalede ele alınan diğer iyileştirmeler uygulamayı düşünün. Seçimlerinizi temel iş yükünüze dayalı ve test sürecinde doğrulayın.

> [!TIP]
> Genellikle maliyetlerini iyileştirme ve performans için en iyi duruma getirme arasında bir ilişki yoktur. Bu makalede odaklanmıştır *en iyi* Azure vm'lerde SQL Server için performans. İş yükünüzü daha az zorlu ise, aşağıda listelenen her iyileştirme gerekmeyebilir. Bu önerileri değerlendirirken, performans gereksinimlerini, maliyetleri ve iş yükü düzenleri göz önünde bulundurun.

## <a name="quick-check-list"></a>Hızlı onay listesi

Azure Virtual Machines'de SQL Server'ın en iyi performans için hızlı onay listesi verilmiştir:

| Alan | En iyi duruma getirme |
| --- | --- |
| [VM boyutu](#vm-size-guidance) | - [DS3_v2](../sizes-general.md) veya SQL Enterprise edition için daha yüksek.<br/><br/> - [DS2_v2](../sizes-general.md) veya üzeri SQL Standard ve Web sürümleri. |
| [Depolama](#storage-guidance) | -Kullanma [premium SSD](../disks-types.md). Standart depolama, yalnızca geliştirme/test için önerilir.<br/><br/> -Tutmak [depolama hesabı](../../../storage/common/storage-create-storage-account.md) ve SQL Server VM ile aynı bölgede.<br/><br/> * Azure devre dışı [coğrafi olarak yedekli depolama](../../../storage/common/storage-redundancy.md) (coğrafi çoğaltma) depolama hesabındaki. |
| [Diskler](#disks-guidance) | -En az 2 kullanma [P30 disk](../disks-types.md#premium-ssd) (günlük dosyaları ve tempdb'yi dahil olmak üzere veri dosyaları için 1 için 1). Yaklaşık 50.000 IOPS gerektiren iş yükleri için Ultra yüksek bir SSD kullanabilirsiniz. <br/><br/> -Sorunlarını giderme veritabanı depolama veya günlük kaydı için işletim sistemi veya geçici diskler kullanmaktan kaçının.<br/><br/> -TempDB veri dosyaları ve veri dosyalarını barındıran diskler üzerinde önbelleğe alma enable okuyun.<br/><br/> -Günlük dosyası barındırma diskler üzerinde önbelleğe alma özelliğini etkinleştirmeyin.  **Önemli**: Bir Azure VM disk için önbellek ayarları değiştirildiğinde SQL Server hizmetini durdurun.<br/><br/> -Birden çok Azure veri diski daha yüksek g/ç aktarım hızı alma için stripe.<br/><br/> -Belgelenmiş ayırma boyutları ile biçimlendirin. <br/><br/> -Bir yerde TempDB (doğru VM boyutunu seçme sonra) görev açısından kritik SQL Server iş yükleri için yerel SSD üzerinde. |
| [G/Ç](#io-guidance) |-Veritabanı Sayfa sıkıştırmayı etkinleştirin.<br/><br/> -Veri dosyaları için anında dosya başlatma etkinleştirin.<br/><br/> -Veritabanı otomatik büyüme sınırlayın.<br/><br/> -Otomatik olarak küçültme veritabanının devre dışı bırakın.<br/><br/> -Sistem veritabanları dahil olmak üzere veri diskleri için tüm veritabanlarını taşıyın.<br/><br/> -Veri diskleri için SQL Server hata günlüğü ve izleme dosya dizinleri taşıyın.<br/><br/> -Varsayılan yedekleme ve veritabanı dosyası konumlarını ayarlayın.<br/><br/> -Kilitli sayfalar sağlar.<br/><br/> -SQL Server performans düzeltmeleri uygulayın. |
| [Özelliğe özgü](#feature-specific-guidance) | -Doğrudan blob depolama alanına yedekleyin. |

Daha fazla bilgi için *nasıl* ve *neden* bu iyileştirmeler yapmak için lütfen ayrıntıları ve aşağıdaki bölümlerde verilen yönergeleri gözden geçirin.

## <a name="vm-size-guidance"></a>VM boyutu Kılavuzu

Performans duyarlı uygulamalar için aşağıdaki kullanmanız önerilir [sanal makine boyutları](../sizes.md):

* **SQL Server Enterprise Edition**: DS3_v2 veya üzeri
* **SQL Server Standard ve Web sürümleri**: DS2_v2 veya üzeri

[DSv2 serisi](../sizes-general.md#dsv2-series) en iyi performans için önerilen premium depolama Vm'leri destekler. Taban çizgileri İşte, ancak seçtiğiniz gerçek makine boyutu, iş yükü gereksinimlerine bağlıdır boyutları önerilir. DSv2 serisi VM'ler diğer makine boyutları, belirli iş yükü türleri için optimize edilmiş ise, çeşitli iş yükleri için uygundur, genel amaçlı vm'leridir. Örneğin, [M serisi](../sizes-memory.md#m-series) en yüksek vCPU sayısını ve en büyük SQL Server iş yükleri için bellek sunar. [GS serisi](../sizes-memory.md#gs-series) ve [DSv2 serisi 11-15](../sizes-memory.md#dsv2-series-11-15) büyük bellek gereksinimlerini en iyi duruma getirilir. Bu dizinin her ikisi de mevcuttur [kısıtlı çekirdek boyutları](../../windows/constrained-vcpu.md), talepleri düşük iş yükleriyle işlem için para kaydeder. [Ls serisi](../sizes-storage.md) makineler, yüksek disk aktarım hızı ve g/ç için iyileştirilmiştir. SQL Server yükünüzü düşünün ve bu VM serisi ve boyutu seçiminiz için geçerli önemlidir.

## <a name="storage-guidance"></a>Depolama yönergeleri

(Birlikte, DSv2 serisi ve GS serisi) DS serisi VM'lerin desteklediği [premium SSD](../disks-types.md). Premium SSD tüm üretim iş yükleri için önerilir.

> [!WARNING]
> Standart HDD'ler ve SSD'ler değişen gecikme süresi ve bant genişliği olması ve yalnızca geliştirme/test iş yükleri için önerilir. Üretim iş yükleri, premium SSD kullanmanız gerekir.

Ayrıca, aktarım gecikmelerini azaltmak için SQL Server sanal makineleri aynı veri merkezinde Azure depolama hesabınızın oluşturmanızı öneririz. Birden çok disk arasında tutarlı yazma sırasını garanti edilmez gibi bir depolama hesabı oluştururken, coğrafi çoğaltma devre dışı bırakın. Bunun yerine, bir SQL Server olağanüstü durum kurtarma teknoloji iki Azure veri merkezleri arasında yapılandırmayı göz önünde bulundurun. Daha fazla bilgi için [yüksek kullanılabilirlik ve olağanüstü durum kurtarma için Azure sanal Makineler'de SQL Server](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Diskleri Kılavuzu

Bir Azure sanal makinesinde üç ana disk türü vardır:

* **İşletim sistemi diski**: Bir Azure sanal makine oluşturduğunuzda, platform en az bir disk tanıdığına (olarak etiketlenmiş **C** sürücü), işletim sistemi diski VM'ye. Bu disk depodaki bir sayfa blobu olarak depolanan bir vhd'dir.
* **Geçici disk**: Azure sanal makineleri içeren başka bir disk geçici diski de denen (olarak etiketlenmiş **D**: sürücü). Bu çalışma alanı için kullanılabilir düğüm üzerinde bir disktir.
* **Veri diskleri**: Ayrıca ek diskler, sanal makineye veri diski ekleyebilirsiniz ve depolama alanında bunlar sayfa blobları depolanır.

Aşağıdaki bölümlerde, bu farklı diskler kullanarak önerileri açıklanmaktadır.

### <a name="operating-system-disk"></a>İşletim sistemi diski

Önyükleme ve bağlama çalışan bir işletim sistemi sürümü bir VHD bir işletim sistemi diskidir ve olarak etiketlenmiş **C** sürücü.

Varsayılan önbelleğe alma İlkesi, işletim sistemi disk üzerinde **okuma/yazma**. Performans duyarlı uygulamalar için işletim sistemi diski yerine veri diskleri kullanmanızı öneririz. Veri disklerini aşağıdaki bölüme bakın.

### <a name="temporary-disk"></a>Geçici disk

Geçici depolama sürücü olarak etiketlenmiş **D**: sürücü, Azure blob depolama alanına kalıcı değil. Kullanıcı veritabanı dosyaları veya kullanıcı işlem günlüğü dosyaları üzerinde depolamayın **D**: sürücü.

D serisi, Dv2 serisi ve G serisi VM'ler için SSD tabanlı geçici sürücüyü bu VM'ler üzerinde. İş yükünüz TempDB yoğun olarak kullanılır (örneğin, geçici nesneler veya karmaşık birleştirmeler) yaparsa TempDB depolama **D** sürücü neden TempDB daha yüksek verim ve daha düşük TempDB gecikme süresi. Örnek bir senaryo için şu blog gönderisinde TempDB tartışmalara bakın: [Azure vm'lerde SQL Server için depolama yapılandırma yönergeleri](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/09/25/storage-configuration-guidelines-for-sql-server-on-azure-vm).

Premium SSD (DS serisi, DSv2 serisi ve GS serisi) destekleyen VM'ler için etkinleştirilmiş okuma önbelleğini ile premium SSD destekleyen bir diskte TempDB depolamanızı öneririz.

Bu öneri için bir istisna vardır: _yazma yoğunluklu TempDB kullanımınızı ise yerel TempDB depolayarak daha iyi performans elde **D** SSD tabanlı bu makine boyutları olan sürücü._

### <a name="data-disks"></a>Veri diskleri

* **Veri ve günlük dosyaları için veri diskleri kullanma**: Disk bölümleme türüyle kullanmıyorsanız, burada bir disk günlük dosyaları ve diğer veri ve TempDB dosyaları içerir iki premium SSD P30 disk kullanın. Her premium SSD IOPS ve bant genişliği (MB/sn) boyutuna bağlı olarak birkaç makalede gösterildiği gibi sağlar [bir disk türü seçin](../disks-types.md). Depolama alanları gibi bir disk bölümleme türüyle teknik kullanıyorsanız, günlük dosyaları ve diğer veri dosyaları için iki havuz sağlayarak en iyi bir performansa ulaşın. Ancak, SQL Server Yük devretme kümesi örneği (FCI) kullanmayı planlıyorsanız, bir havuzu yapılandırmanız gerekir.

   > [!TIP]
   > - İçin test sonuçlarını çeşitli disk ve iş yükü yapılandırmaları için şu blog yayınına bakın: [Azure vm'lerde SQL Server için depolama yapılandırma yönergeleri](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/09/25/storage-configuration-guidelines-for-sql-server-on-azure-vm/).
   > - Görev açısından kritik performans için gereken SQL sunucuları için yaklaşık 50.000 IOPS değiştirmeyi göz önünde bulundurun Ultra yüksek bir SSD ile 10 - P30 disk. Daha fazla bilgi için için şu blog yayınına bakın: [Görev açısından kritik performans Ultra SSD ile](https://azure.microsoft.com/blog/mission-critical-performance-with-ultra-ssd-for-sql-server-on-azure-vm/).

   > [!NOTE]
   > Bir SQL Server VM'si portalında sağladığınızda, depolama yapılandırmanızı düzenleme seçeneğiniz vardır. Yapılandırmanıza bağlı olarak, Azure, bir veya daha fazla disk yapılandırır. Birden çok disk bölümleme türüyle birlikte bir tek bir depolama havuzu halinde birleştirilir. Veri ve günlük dosyaları, bu yapılandırmada birlikte yer alır. Daha fazla bilgi için [SQL Server Vm'leri için depolama yapılandırması](virtual-machines-windows-sql-server-storage-configuration.md).

* **Disk bölümleme türüyle**: Daha fazla aktarım hızı için ek veri diskleri ekleyip Disk bölümleme türüyle kullanabilirsiniz. Veri disk sayısını belirlemek için IOPS ve bant genişliği için günlük dosyalarınızı ve verilerinizi ve TempDB dosyaları için gerekli sayıda analiz etmeniz. Farklı VM boyutları farklı sınırlar IOPS ve bant genişliği desteklenen sayısına sahip olduğunu fark, şirket başına IOPS tablolara bakın [VM boyutu](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Aşağıdaki yönergeleri kullanın:

  * Windows 8/Windows için Server 2012 veya üzeri kullanın [depolama alanları](https://technet.microsoft.com/library/hh831739.aspx) aşağıdaki yönergeleri ile:

      1. OLTP iş yükleri için 64 KB (65536 bayt) ve 256 KB (262144 bayt) veri ambarı iş yükleri için bölüm yanlış hizalanmış nedeniyle performansı etkilemelerini önlemek için ayırma (stripe boyutu) ayarlayın. PowerShell ile ayarlamanız gerekir.
      2. Ayarlama sütun sayısı = fiziksel disk numarası. PowerShell, 8'den fazla disk (Sunucu Yöneticisi kullanıcı Arabirimi değil) yapılandırırken kullanın. 

    Örneğin, aşağıdaki PowerShell ayırma boyutu 64 KB ile 2 sütun sayısı ile yeni bir depolama havuzu oluşturur:

    ```powershell
    $PoolCount = Get-PhysicalDisk -CanPool $True
    $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

    New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false 
    ```

  * Windows 2008 R2 veya önceki sürümlerinde, dinamik diskleri (işletim sistemi şeritli birimler) kullanabilirsiniz ve Şerit boyutunun her zaman 64 KB'tır. Bu seçenek Windows 8/Windows itibarıyla Server 2012 kullanım dışı olduğunu unutmayın. Bilgi için destek bildirimine bakın [Sanal Disk Hizmeti için Windows Depolama Yönetimi API'si geçiş](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

  * Kullanıyorsanız [depolama alanları doğrudan (S2D)](/windows-server/storage/storage-spaces/storage-spaces-direct-in-vm) ile [SQL Server Yük devretme kümesi örneklerinin](virtual-machines-windows-portal-sql-create-failover-cluster.md), tek bir havuz yapılandırmanız gerekir. Tek havuzda farklı birimleri oluşturulabilir olsa da, bunlar tüm aynı önbellek İlkesi gibi aynı özellikleri paylaşır olduğunu unutmayın.

  * Depolama havuzunuz yük beklentilerinizi ilişkili disk sayısını belirler. Farklı VM boyutları, bağlı veri diskleri farklı sayıda izin verdiğini unutmayın. Daha fazla bilgi için [sanal makine boyutları](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

  * Premium SSD (geliştirme/test senaryoları) kullanmıyorsanız, tarafından desteklenen veri diskleri en fazla sayıda eklemek için kullanılması önerilir, [VM boyutu](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve Disk bölümleme türüyle kullanın.

* **Önbelleğe alma İlkesi**: Önbelleğe alma İlkesi, depolama yapılandırmasına bağlı olarak aşağıdaki önerileri unutmayın.

  * Veri ve günlük dosyalarını ayrı diskler kullanıyorsanız, veri dosyaları ve TempDB veri dosyalarını barındıran veri disklerinde okuma önbelleğini etkinleştirin. Bu önemli bir performans avantaj olarak sonuçlanabilir. Günlük dosyası küçük bir performans düşüklüğü açacağından bulunduran disk üzerinde önbelleğe alma etkinleştirmeyin.

  * Tek bir depolama havuzunda disk bölümleme türüyle kullanıyorsanız, çoğu iş yükü okuma önbelleğini yararlı olacaktır. Günlük ve veri dosyaları için ayrı depolama havuzları varsa, yalnızca veri dosyaları için depolama Havuzu'ndaki okuma önbelleğini etkinleştirin. Bazı ağır yazma iş yüklerinin önbelleğe alma ile daha iyi performans elde. Bu, test sürecinde yalnızca belirlenebilir.

  * Önceki öneriler, premium SSD için geçerlidir. Premium SSD kullanmıyorsanız, tüm veri disklerinde önbelleğe alma etkinleştirmeyin.

  * Disk önbelleğe almayı yapılandırma ile ilgili yönergeler için aşağıdaki makalelere bakın. Dağıtım modeli Klasik (ASM) için bkz: [Set-AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) ve [kümesi AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx). Azure Resource Manager dağıtım modeli görmek için: [Set-AzOSDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk) ve [kümesi AzVMDataDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdatadisk).

     > [!WARNING]
     > Azure VM Disk önbellek ayarı veritabanında bozulma olasılığını önlemek için değiştirilirken SQL Server hizmetini durdurun.

* **NTFS ayırma birimi boyutu**: Veri diski biçimlendirme sırasında TempDB yanı sıra veri ve günlük dosyaları için 64 KB ayırma birimi boyutu kullanmanız önerilir.

* **Disk Yönetimi en iyi**: Veri diski kaldırma veya değiştirme önbelleğinde yazdığınızda, değişiklik sırasında SQL Server hizmetini durdurun. İşletim sistemi diskinde önbelleğe alma ayarlarını değiştiğinde, Azure VM'yi durdurur, önbellek türü değiştirir ve VM'yi yeniden başlatır. Bir veri diskinin önbellek ayarları değiştirildiğinde VM durdurulmadı, ancak veri diski sanal makineden değişiklik sırasında kullanımdan çıkarıldı ve ardından eklenemeyeceği.

  > [!WARNING]
  > Bu işlemleri sırasında SQL Server hizmetini durdurmak için hata veritabanında bozulmaya neden olabilir.


## <a name="io-guidance"></a>G/ç Kılavuzu

* Uygulamanızı ve istekleri paralel hale getirmek, premium SSD ile en iyi sonuçlar elde edilir. Premium SSD (depolama yoğunluklu olsa bile) tek iş parçacıklı seri istekleri için çok az kayıpla veya hiç performans artışı göreceğiniz şekilde GÇ kuyruğu derinlik 1'den büyük olduğu senaryolar için tasarlanmıştır. Örneğin, bu tek iş parçacıklı test sonuçlarını SQLIO gibi performans analiz araçlarını etkileyebilir.

* Kullanmayı [veritabanı sayfa sıkıştırmasını](https://msdn.microsoft.com/library/cc280449.aspx) gibi g/ç yoğunluklu iş yüklerinin performansını artırmaya yardımcı olabilir. Ancak, veri sıkıştırma veritabanı sunucusundaki CPU tüketimini artırabilir.

* İlk dosya ayırma için gereken süreyi azaltmak üzere anında dosya başlatma etkinleştirmeyi düşünün. Anında dosya başlatma yararlanmak için SQL Server (MSSQLSERVER) hizmet hesabına SE_MANAGE_VOLUME_NAME ile verin ve ekleyin **Birim bakım görevleri gerçekleştirme** güvenlik ilkesi. Azure için bir SQL Server platform görüntüsü kullanıyorsanız, varsayılan hizmet hesabı'nı (NT Service\MSSQLSERVER) için eklenmez **Birim bakım görevleri gerçekleştirme** güvenlik ilkesi. Diğer bir deyişle, bir SQL Server Azure platform görüntüsüne anında dosya başlatma etkin değil. SQL Server hizmet hesabına ekledikten sonra **Birim bakım görevleri gerçekleştirme** güvenlik ilkesi, SQL Server hizmetini yeniden başlatın. Bu özelliği kullanmak için güvenlik konuları olabilir. Daha fazla bilgi için [veritabanı dosya başlatma](https://msdn.microsoft.com/library/ms175935.aspx).

* **otomatik büyüme** yalnızca beklenmeyen büyüme için bir yedek olarak kabul edilir. Veri ve günlük büyümesini otomatik büyüme ile günlük olarak yönetmez. Otomatik büyüme kullanılması durumunda dosyanın boyutu anahtarı kullanarak önceden büyütün.

* Emin **otomatik olarak küçültme** performansı olumsuz etkileyebilir gereksiz ek yükten kaçınmak için devre dışı bırakıldı.

* Tüm veritabanları, sistem veritabanları dahil olmak üzere veri diskleri için taşıyın. Daha fazla bilgi için [sistem veritabanlarını taşıma](https://msdn.microsoft.com/library/ms345408.aspx).

* Veri diskleri için SQL Server hata günlüğü ve izleme dosya dizinleri taşıyın. Bu SQL Server Yapılandırma Yöneticisi'nde SQL Server Örneğinize sağ tıklatıp Özellikler'i seçerek gerçekleştirebilirsiniz. Hata günlüğü ve izleme dosyası ayarları değiştirilebilir **başlangıç parametreleri** sekmesi. Döküm dizini belirtilen **Gelişmiş** sekmesi. Aşağıdaki ekran görüntüsünde, hata günlüğü başlangıç parametresi aranacağı gösterir.

    ![SQL hata günlüğüne ekran görüntüsü](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

* Varsayılan yedekleme ve veritabanı dosyası konumlarını ayarlayın. Bu makalede önerileri kullanın ve sunucu Özellikler penceresinde değişiklikleri yapın. Yönergeler için [görüntülemek veya veri ve günlük dosyaları (SQL Server Management Studio) için varsayılan konumları değiştirme](https://msdn.microsoft.com/library/dd206993.aspx). Aşağıdaki ekran görüntüsünde, bu değişiklikleri yapmak nereye gösterir.

    ![SQL veri günlüğü ve yedekleme dosyaları](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)
* GÇ ve herhangi bir disk belleği etkinlik azaltmak kilitli sayfalar sağlar. Daha fazla bilgi için [bellek seçeneği (Windows) kilit sayfalarında etkinleştirme](https://msdn.microsoft.com/library/ms190730.aspx).

* SQL Server 2012 çalıştırıyorsanız, hizmet paketi 1 Cumulative Update 10 yükleyin. Bu güncelleştirme, SQL Server 2012'de geçici bir tablo ifadesi içine seçin yürüttüğünüzde g/ç kötü performans düzeltmesini içerir. Bu bilgi için [Bilgi Bankası makalesi](https://support.microsoft.com/kb/2958012).

* Azure içeri/dışarı aktarma, tüm veri dosyaları sıkıştırma göz önünde bulundurun.

## <a name="feature-specific-guidance"></a>Özelliğe özgü yönergeleri

Bazı dağıtımlar, daha gelişmiş yapılandırma teknikleri kullanarak ek performans avantajlarını elde edebilirsiniz. Aşağıdaki listede, daha iyi performans elde etmenize yardımcı olabilecek bazı SQL Server özelliklerini vurgular:

* **Azure depolama için Yedekleme**: Azure sanal makineler'de çalışan SQL Server için yedeklemeleri gerçekleştirirken kullanabileceğiniz [URL'ye SQL Server Yedekleme](https://msdn.microsoft.com/library/dn435916.aspx). Bu özellik, SQL Server 2012 SP1 CU2'ile başlayan kullanılabilir ve bağlı veri diskleri yedekleme için önerilen. Ne zaman verilen önerileri uygulayın, yedekleme/geri yükleme/Azure Depolama'dan [SQL Server Yedekleme'den URL en iyi yöntemler ve sorun giderme ve geri yükleme için yedeklemeler depolanan Azure storage'da](https://msdn.microsoft.com/library/jj919149.aspx). Ayrıca bu yedeklemeler kullanarak otomatikleştirebilirsiniz [Azure sanal Makineler'de SQL Server için otomatik yedekleme](virtual-machines-windows-sql-automated-backup.md).

    SQL Server 2012'den önce kullandığınız [Azure aracı SQL Server Yedekleme](https://www.microsoft.com/download/details.aspx?id=40740). Bu aracı kullanarak birden çok yedekleme stripe hedef yedekleme verimliliğini artırmak için yardımcı olabilir.

* **Azure'da SQL Server veri dosyaları**: Bu yeni özellik [azure'da SQL Server veri dosyaları](https://msdn.microsoft.com/library/dn385720.aspx), ile SQL Server 2014'ten itibaren kullanıma sunuluyor. SQL Server veri dosyaları azure'da çalışan, benzer performans özelliklerini kullanarak Azure veri diski olarak gösterir.

## <a name="next-steps"></a>Sonraki Adımlar

Depolama ve performans hakkında daha fazla bilgi için bkz. [Azure vm'lerde SQL Server için depolama yapılandırma yönergeleri](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/09/25/storage-configuration-guidelines-for-sql-server-on-azure-vm/)

En iyi güvenlik için bkz: [Azure sanal Makineler'de SQL Server için güvenlik konuları](virtual-machines-windows-sql-security.md).

Diğer SQL Server sanal makinesi makalelerini gözden [SQL Server Azure sanal makinelerine genel bakış](virtual-machines-windows-sql-server-iaas-overview.md). SQL Server sanal makineleri hakkında sorularınız olursa [Sık Sorulan Sorular](virtual-machines-windows-sql-server-iaas-faq.md) bölümüne bakın.
