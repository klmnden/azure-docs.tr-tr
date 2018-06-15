---
title: Azure yığın Virtual Machines'de SQL Server için performans en iyi yöntemler
description: Microsoft Azure yığın Virtual Machines'de SQL Server performansını iyileştirmek için en iyi yöntemler sağlar.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: mabrigg
ms.reviewer: anajod
ms.openlocfilehash: 00c67503f5b9e0027cbb62520e392f56420a75e6
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34701404"
---
# <a name="optimize-sql-server-performance"></a>SQL Server performansını en iyi duruma getirme

Bu makalede, Microsoft Azure yığın sanal makinelerde SQL Server performansını iyileştirmek için yönergeler sağlar. SQL Server Azure yığın sanal makinelerde çalışan, aynı veritabanı performans ayarlama seçenekleri bir şirket içi sunucu ortamında SQL Server için geçerli kullanın. Bir Azure yığın bulutta ilişkisel veritabanı performansını birçok faktöre bağlıdır. Bir sanal makine ailesi boyutunu ve veri diskleri yapılandırmasını Etkenler içerir.

SQL Server görüntülerini oluştururken [sanal makinelerinizi Azure yığın portalında sağlama göz önünde bulundurun](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision). Azure yığın Yönetim Portalı'nda Market yönetimden SQL Iaas uzantısını indirin ve seçiminizi SQL sanal makinenin sanal sabit diskleri (VHD) indirin. Bunlar SQL2014SP2, SQL2016SP1 ve SQL2017 içerir.

> [!NOTE]  
> Genel Azure portalını kullanarak bir SQL Server sanal makine sağlamak makalede olsa da, kılavuzu aşağıdaki farklarla Azure yığını için de geçerlidir.: SSD işletim sistemi diski için kullanılabilir değil, yönetilen diskler kullanılabilir değil ve depolama yapılandırması küçük farklılıklar vardır.

Alma *en iyi* Azure yığın sanal makinelerde Performans SQL Server için olan bu makaleyi odağını. İş yükünüzün daha az kullanılmasına neden olması durumunda her önerilen en iyi duruma getirme gerektirmeyebilecek. Bu öneriler değerlendirirken yükünün desenleri ve performans gereksinimlerini göz önünde bulundurun.

> [!NOTE]  
> Performans yönergeler için SQL Server için Azure sanal makinelerde başvurmak [bu makalede](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance).

## <a name="before-you-begin"></a>Başlamadan önce
Azure yığın sanal makinelerde SQL Server'ın en iyi performans için aşağıdaki denetim listesi verilmiştir:


|Alan|En iyi duruma getirme|
|-----|-----|
|Sanal makine boyutu |[DS3](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) veya SQL Server Enterprise edition için daha yüksek.<br><br>[DS2](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) veya SQL Server Standard edition ve Web edition için daha yüksek.|
|Depolama |Destekleyen bir sanal makine ailesi [Premium depolama](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-acs-differences).|
|Diskler |En az iki veri disklerinin (bir günlük dosyaları için) ve bir veri dosyası ve TempDB kullanın ve kapasite gereksinimlerine göre disk boyutu seçin. Varsayılan veri dosyası konumları SQL Server yüklemesi sırasında bu disklere ayarlayın.<br><br>Veritabanı depolama veya günlük için işletim sistemi veya geçici diskleri kullanmaktan kaçının.<br>Depolama alanları'nı kullanarak daha yüksek g/ç işleme almak için birden çok Azure veri diski şeritler.<br><br>Belgelenen ayırma boyutlarıyla biçimlendirin.|
|G/Ç|Veri dosyaları için anında dosya başlatma etkinleştirin.<br><br>Otomatik büyüme veritabanlarında makul küçük sabit aralıklarla (64 MB - 256 MB) ile sınırlayın.<br><br>Veritabanında daralma devre dışı bırakın.<br><br>Veri diskleri, işletim sistemi diski varsayılan yedekleme ve veritabanı dosyası konumlarının ayarlayın.<br><br>Kilitli sayfalar etkinleştirin.<br><br>SQL Server hizmet paketlerini ve toplu güncelleştirmeler uygulanır.|
|Özelliğe özgü|Doğrudan blob depolamaya (SQL Server sürümü tarafından destekleniyorsa) yedekleyin.|
|||

Daha fazla bilgi için *nasıl* ve *neden* bu iyileştirmeler olmak için lütfen ayrıntıları ve aşağıdaki bölümlerde verilen yönergeleri gözden geçirin.

## <a name="virtual-machine-size-guidance"></a>Sanal makine boyutu Kılavuzu

Aşağıdaki performans duyarlı uygulamalar için [sanal makine boyutlarını](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) önerilir:

- **SQL Server Enterprise edition:** DS3 veya üzeri

- **SQL Server Standard edition ve Web edition:** DS2 veya üzeri

Azure yığınla DS ve DS_v2 sanal makine ailesi serisi arasında herhangi bir performans farkı yoktur.

## <a name="storage-guidance"></a>Depolama Kılavuzu

(Birlikte, DSv2 serisi) DS serisi sanal makineler Azure yığınında en yüksek işletim sistemi diski ve veri diski üretilen işi (IOPS) sağlar. Bir sanal makineyi DS veya DSv2 serisi 2,300 IOPS türü veya seçilen disk boyutunu olsun veri disk başına en fazla ve işletim sistemi diski için en çok 1.000 IOPS sağlar.

Veri diski verimlilik sanal makine ailesi serisi benzersiz olarak göre belirlenir. Yapabilecekleriniz [bu makalesine başvurun](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) her sanal makine ailesi seri veri disk performansını belirlemek için.

> [!NOTE]  
> Üretim iş yükleri için bir DS serisi veya DSv2 serisi maksimum olası IOPS işletim sistemi diski ve veri diskleri sağlamak için sanal makine seçin.

Bu özellik Azure yığın içinde kullanılabilir olmadığından bir depolama hesabı Azure yığınında oluştururken, coğrafi çoğaltma seçeneği etkisi yoktur.

## <a name="disks-guidance"></a>Diskleri Kılavuzu

Bir Azure yığın sanal makinede üç ana disk türleri şunlardır:

- **İşletim sistemi diski:** Azure yığın sanal makine oluşturduğunuzda, en az bir disk platform ekler (olarak etiketli **C** sürücü) sanal makine işletim sistemi diski. Bu disk, depolama birimindeki bir sayfa blob'u olarak kaydedilen bir vhd'dir.

- **Geçici disk:** Azure yığın sanal makineleri içeren geçici disk adlı başka bir diske (olarak etiketli **D** sürücü). Bir disk için boş alanın kullanılabilir düğüm üzerinde budur.

- **Veri diskleri:** ek diskleri sanal makineniz veri diskleri olarak ekleyebilir ve bu diskleri sayfa blobları depolama alanında depolanır.

Aşağıdaki bölümlerde bu farklı diskler kullanarak önerileri açıklanmaktadır.

### <a name="operating-system-disk"></a>İşletim sistemi diski

Bir işletim sistemi diski, önyükleme ve bağlama çalışan bir işletim sistemi sürümü olarak bir VHD olduğunu ve olarak etiketli **C** sürücü.

### <a name="temporary-disk"></a>Geçici disk

Olarak etiketli geçici depolama sürücüsü **D** sürücü, kalıcı değildir. Kullanıcı veritabanı dosyaları veya kullanıcı işlem günlüğü dosyalarını gibi açık kaybetmek istemiyor herhangi bir veriyi depolamaz **D** sürücü.

Her veri diski maksimum veri disk başına en fazla 2,300 IOPS sağladığı gibi TempDB üzerinde bir veri diski depolamanızı öneririz.

### <a name="data-disks"></a>Veri diskleri

- **Veri diskleri için veri ve günlük dosyalarını kullanın.** Disk şeritleme kullanmıyorsanız, iki veri diskleri Premium storage destekleyen bir sanal makineden bir disk günlük dosyaları, içerdiği kullanın ve diğer veri ve TempDB dosyalarını içerir. Bölümünde açıklandığı gibi bir dizi IOPS ve bant genişliği (MB/sn) sanal makine ailesinin bağlı olarak her veri diski sağlar [Azure yığınında desteklenen sanal makine boyutlarını](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes). Depolama alanları gibi bir disk şeritleme teknik kullanıyorsanız, tüm veri ve günlük dosyalarını (TempDB dahil) aynı sürücüsüne yerleştirin. Bu yapılandırma için SQL Server'ı kullanmak, hangi dosya gereksinimlerini olsun bunları belirli bir zamanda kullanılabilir IOPS sayısını sağlar.

> [!NOTE]  
> Portal'da bir SQL Server sanal makine sağlarken, depolama yapılandırmanızı düzenleme seçeneğiniz vardır. Yapılandırmanıza bağlı olarak, bir veya daha fazla disk Azure yığın yapılandırır. Birden çok diske, tek bir depolama havuzu birleştirilir. Veri ve günlük dosyaları, bu yapılandırmada birlikte bulunur.

- **Disk şeritleme:** daha fazla verimlilik için ek veri disklerinin ekleyebilir ve disk şeritleme kullanabilirsiniz. Gereksinim duyduğunuz veri diski sayısı belirlemek için IOP ve verilerinizi ve TempDB dosyalarını ve günlük dosyaları için gereken bant sayısını analiz edin. Sanal makine serisi ailesini temel alan ve sanal makine boyutuna göre değil veri disk başına IOPS sınırları olduğuna dikkat edin. Ağ bant genişliği sınırları, ancak, sanal makine boyutuna dayanır. Bkz: tabloları [sanal makine boyutları Azure yığınında](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) daha fazla ayrıntı için. Aşağıdaki yönergeleri kullanın:

    - Windows Server 2012 veya daha sonra kullanmak [depolama alanları](https://technet.microsoft.com/library/hh831739.aspx) aşağıdaki yönergeleri ile:

        1.  Ayırma (stripe boyutu) çevrimiçi işlem işleme (OLTP) iş yükleri ve 256 bölüm uyuşmazlığın nedeniyle performans etkisini önlemek için KB (262.144 bayt veri ambarı iş yükleri için) 64 KB (65,536 bayt) olarak ayarlayın. PowerShell ile ayarlamanız gerekir.

        2.  Ayarlama sütun sayısı = fiziksel disk sayısı. PowerShell sekiz diskleri (Sunucu Yöneticisi kullanıcı Arabirimi olmayan) yapılandırırken kullanın.

            Örneğin, aşağıdaki PowerShell yeni bir depolama havuzu ayırma boyutu 64 KB ile 2 için sütun sayısı için ayarını oluşturur:

          ```PowerShell  
          $PoolCount = Get-PhysicalDisk -CanPool $True
          $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

          New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false
          ```

- Depolama havuzunuz yük beklentilerinizi ilişkili disk sayısını belirler. Başka bir sanal makine boyutları eklenen veri disklerini farklı sayıda izin göz önünde bulundurun. Daha fazla bilgi için bkz: [Azure yığınında desteklenen sanal makine boyutlarını](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes).
- Veri diskleri için olası en büyük IOPS alabilmek için veri diski tarafından desteklenen maksimum sayısı eklemek için önerilir, [sanal makine boyutu](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) ve disk şeritleme kullanın.
- **NTFS ayırma birimi boyutu:** veri diski biçimlendirme sırasında TempDB yanı sıra veri ve günlük dosyaları için 64 KB ayırma birimi boyutu kullanmanız önerilir.
- **Disk Yönetimi uygulamalarını:** bir veri diski kaldırırken, değişiklik sırasında SQL Server hizmetini durdurun. Ayrıca, tüm performans iyileştirmeleri sağlamaz gibi disklerde önbellek ayarları değişmez.

> [!WARNING]  
> Bu işlemleri sırasında SQL hizmetini durdurmak için hata veritabanında bozulmaya neden olabilir.

## <a name="io-guidance"></a>G/ç Kılavuzu

- İlk dosya ayırma için gereken süreyi azaltmak anında dosya başlatma etkinleştirmeyi düşünün. Anında dosya başlatma yararlanmak için SQL Server (MSSQLSERVER) hizmeti hesabıyla vermek **SE_MANAGE_VOLUME_NAME** ve ekleyin **Birim bakım görevlerini gerçekleştirme** güvenlik ilke. Azure için bir SQL Server platform görüntüsü kullanıyorsanız, varsayılan hizmet hesabı (**NT Service\MSSQLSERVER**) için eklenmez **Birim bakım görevlerini gerçekleştirme** güvenlik ilkesi. Diğer bir deyişle, anında dosya başlatma bir SQL Server Azure platform görüntüsünde etkin değil. SQL Server hizmet hesabına ekledikten sonra **Birim bakım görevlerini gerçekleştirme** güvenlik ilkesi, SQL Server hizmetini yeniden başlatın. Bu özelliği kullanmak için güvenlik konuları olabilir. Daha fazla bilgi için bkz: [veritabanı dosyası başlatma](https://msdn.microsoft.com/library/ms175935.aspx).
- **Otomatik büyüme** beklenmeyen büyüme için bir yedek değil. Veri ve günlük büyüme otomatik büyüme ile günlük aralıklarla yönetmez. Otomatik büyüme kullanılırsa, kullanarak dosya önceden büyümesine **boyutu** geçin.
- Emin olun **daralma** performansını olumsuz yönde etkileyebilir gereksiz yükünü önlemek için devre dışı bırakılır.
- Varsayılan yedekleme ve veritabanı dosyası konumlarını ayarlayın. Bu makalede önerileri kullanın ve sunucu özellikleri penceresinde değişiklikleri yapın. Yönergeler için bkz: [görüntülemek veya veri ve günlük dosyaları (SQL Server Management Studio) için varsayılan konumları Değiştir](https://msdn.microsoft.com/library/dd206993.aspx). Aşağıdaki ekran görüntüsünde, bu değişiklikleri yapmak nereye gösterir:

    > ![Görüntülemek veya varsayılan konumları değiştirme](./media/sql-server-vm-considerations/image1.png)

- G/ç ve disk belleği etkinlikler azaltmak kilitli sayfalar etkinleştirin. Daha fazla bilgi için bkz: [bellek seçeneği (Windows) kilit sayfalarında etkinleştirmek](https://msdn.microsoft.com/library/ms190730.aspx).

- Tüm veri dosyalarını Azure yedeklemeler dahil yığınını içeri/dışarı aktarılırken sıkıştırmayı göz önünde bulundurun.

## <a name="feature-specific-guidance"></a>Özellik özgü yönergeler

Bazı dağıtımlarda daha gelişmiş yapılandırma teknikleri kullanarak ek performans avantajı elde edebilirsiniz. Aşağıdaki listede daha iyi performans elde yardımcı olabilecek bazı SQL Server özelliklerini vurgular:

- **Azure yedekleme** **depolama.** SQL Azure yığın sanal makinelerde çalışan sunucu için yedeklemeleri gerçekleştirirken, SQL Server Yedekleme URL'ye kullanabilirsiniz. Bu özellik SQL Server 2012 SP1 CU2 ile başlayan kullanılabilir ve eklenen veri disklerini yedekleme için önerilir.

    Ne zaman Yedekleme veya geri yükleme Azure depolama kullanarak izleyin sağlanan öneriler [URL en iyi yöntemler ve sorun giderme için SQL Server Yedekleme](https://msdn.microsoft.com/library/jj919149.aspx) ve [geri gelen yedeklemeler depolanan Microsoft azure'da](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure?view=sql-server-2017). Ayrıca bu yedeklemeler kullanarak otomatikleştirebilirsiniz [Azure Virtual Machines'de SQL Server için otomatik yedekleme](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup).

-   **Azure yığın depolama alanına yedekleyebilir.** Azure yığın depolama benzer bir şekilde yedekleme gibi Azure Storage ile yedekleyebilirsiniz. Bir yedekleme SQL Server Management Studio (SSMS) içinde oluşturduğunuzda, yapılandırma bilgilerini el ile girmeniz gerekir. Depolama kapsayıcısı veya paylaşılan erişim imzası oluşturmak için SSMS kullanamazsınız. SSMS yalnızca Azure Abonelikleri, değil Azure yığın abonelikleri bağlanır. Bunun yerine, Azure yığın portalında veya PowerShell ile depolama hesabı, kapsayıcı ve paylaşılan erişim imzası oluşturmanız gerekir.

    SQL Server Yedekleme iletişim bilgilerini yerleştirdiğinizde:

    ![SQL Server Yedekleme](./media/sql-server-vm-considerations/image3.png)

    > [!NOTE]  
    > Paylaşılan erişim imzası Azure yığın portalından başında olmadan SAS belirteci olan '?' Bağlantı dizesindeki. Kopyalama işlevi portalından kullanırsanız başında silmeniz gerekir '?' SQL Server'ın içinde çalışmak için belirteci.

    Bir kez yedekleme ayarlanmış hedef sahip ve SQL Server'da yapılandırılmış, ardından Azure yığın blob depolama alanına yedekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Hizmetlerini kullanarak ya da uygulamaları için Azure yığın oluşturma](azure-stack-considerations.md)