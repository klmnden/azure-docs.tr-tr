---
title: Yedekleme ve geri yükleme için Azure VM'ler, SQL Server | Microsoft Docs
description: Azure sanal makinelerde çalışan SQL Server veritabanları için yedekleme ve geri yükleme noktaları açıklar.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-resource-management
ms.assetid: 95a89072-0edf-49b5-88ed-584891c0e066
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/04/2018
ms.author: mikeray
ms.openlocfilehash: 4b90d1b9b2ee64722d3c92bcbd8fa205c9b59ebd
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809616"
---
# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Azure Sanal Makineler’de SQL Server için Yedekleme ve Geri Yükleme

Bu makalede Windows Azure sanal makinelerde çalışan yedekleme ve geri yükleme seçenekleri kullanılabilir SQL Server için yönergeler sağlar. Azure depolama, veri kaybı veya fiziksel veri bozulmasına karşı koruma sağlamak için her Azure sanal disk üç kopyasını tutar. Bu nedenle, şirket içi, donanım hataları üzerinde odaklanmak gerekmez. Ancak, yanlışlıkla veri eklemeleri ya da silme işlemleri gibi uygulama veya kullanıcıya hatalara karşı korumak için SQL Server veritabanlarınızı hala yedeklemeniz gerekir. Bu durumda, zaman içinde belirli bir noktaya geri olması önemlidir.

Bu makalenin ilk bölümü kullanılabilir yedekleme ve geri yükleme seçeneklerini genel bakış sağlar. Bu, her stratejisi hakkında daha fazla bilgi sağlayan bölümleri tarafından izlenir.

## <a name="backup-and-restore-options"></a>Yedekleme ve geri yükleme seçenekleri

Aşağıdaki tabloda Azure Vm'lerinde çalıştırılan SQL Server için çeşitli yedekleme ve geri yükleme seçenekleri hakkında bilgi sağlar:

| Stratejisi | SQL sürümleri | Açıklama |
|---|---|---|---|
| [Otomatik Yedekleme](#automated) | 2014<br/> 2016<br/> 2017 | Otomatik yedekleme SQL Server VM üzerinde tüm veritabanları için düzenli yedeklemeler zamanlamanıza olanak sağlar. Yedeklemeler, 30 güne kadar Azure depolama alanında depolanır. SQL Server 2016 ile başlayarak, otomatik yedekleme v2 el ile planlama ve tam sıklığını ve günlük yedekleri yapılandırma gibi ek seçenekler sunar. |
| [SQL VM'ler için Azure yedekleme](#azbackup) | 2012<br/> 2014<br/> 2016<br/> 2017 | Azure Backup, Azure Vm'lerinde çalışan SQL Server için bir kurumsal sınıf yedekleme özelliği sağlar. Bu hizmet ile birden çok sunucu ve binlerce veritabanının yedeklerini merkezi olarak yönetebilir. Veritabanları, Portalı'nda zaman içinde belirli bir noktaya geri yüklenebilir. Yedeklemeleri yıldır koruyabilirsiniz özelleştirilebilir bekletme ilkesi sunar. Bu özellik şu anda genel önizlemede değil. |
| [El ile yedekleme](#manual) | Tümü | SQL Server sürümüne bağlı olarak, el ile yedekleme ve bir Azure VM üzerinde çalışan SQL Server geri yüklemek için çeşitli teknikler vardır. Bu senaryoda, veritabanlarınızı nasıl yedeklenir ve depolama konumu ve bu yedeklemeler yönetimi için sorumludur. |

Aşağıdaki bölümlerde her seçenek daha ayrıntılı açıklanmıştır. Son bölümü, bu makalenin özelliği matrisi biçiminde bir özetini sağlar.

## <a id="autoamted"></a> Otomatik yedekleme

Otomatik yedekleme bir otomatik yedekleme hizmeti SQL Server Standard ve Enterprise sürümleri için bir Windows Azure VM'de çalıştıran sağlar. Bu hizmet tarafından sağlanan [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md), otomatik olarak yüklendiği SQL Server, Windows sanal makine görüntüleri Azure portalında.

Tüm veritabanları, yapılandırdığınız bir Azure depolama hesabı yedeklenir. Yedeklemeleri şifrelenir ve 30 gün boyunca tutulur.

SQL Server 2016 ve daha yüksek VM'ler otomatik yedekleme v2 ile daha fazla özelleştirme seçenekleri sunar. Bu geliştirmeler içerir:

- Sistem Veritabanı Yedeklemeleri
- El ile yedekleme zamanlaması ve zaman penceresi
- Yedekleme sıklığı tam ve günlük dosyası

Bir veritabanını geri yüklemek için depolama hesabında gerekli yedekleme dosyaları bulun ve SQL Server Management Studio (SSMS) veya Transact-SQL komutlarını kullanarak, SQL VM bir geri yükleme gerçekleştirin.

Otomatik yedekleme SQL VM'ler için yapılandırma hakkında daha fazla bilgi için aşağıdaki makaleler birine bakın:

- **SQL Server 2016/2017**: [Azure sanal makineler için yedekleme v2 otomatik ](virtual-machines-windows-sql-automated-backup-v2.md)
- **SQL Server 2014**: [SQL Server 2014 sanal makineler için otomatik yedekleme](virtual-machines-windows-sql-automated-backup.md)

## <a id="azbackup"></a> Azure yedekleme için SQL VM'ler (genel Önizleme)

[Azure yedekleme](/azure/backup/) Azure Vm'lerinde çalışan SQL Server için bir kurumsal sınıf yedekleme özelliği sağlar. Tüm yedeklemeler saklanır ve bir kurtarma Hizmetleri kasasına yönetilir. Bu çözüm, özellikle kuruluşlar için sağlayan çeşitli avantajları vardır:

- **Sıfır altyapı yedekleme**: yedekleme sunucusu veya depolama konumları yönetmek zorunda değilsiniz.
- **Ölçek**: birçok SQL VM ve veritabanları binlerce koruyun.
- **Kullandıkça Öde**: Bu özellik Azure yedekleme tarafından sağlanan ayrı bir hizmet olmakla birlikte, tüm Azure hizmetlerinde gibi yalnızca, kullanım için ödeme yaparsınız.
- **Merkezi Yönetim ve izleme**: tüm azure'da tek bir panosundan Azure Backup destekleyen diğer iş yükleri de dahil olmak üzere Yedeklemelerinizin merkezi olarak yönetin.
- **Yedekleme ve bekletme güdümlü İlkesi**: düzenli yedeklemeler için standart yedekleme ilkeleri oluşturun. Yedeklemeleri yıldır korumak için bekletme ilkeleri oluşturun.
- **SQL her zaman açık desteği**: algılar ve bir SQL Server Always On yapılandırmayı korumak ve yedekleme kullanılabilirlik grubu yedekleme tercihi kabul.
- **15 dakikalık kurtarma noktası hedefi (RPO)**: yapılandırma SQL işlem günlüğü yedeklemeleri 15 dakikada bir kadar.
- **Geri yükleme noktası**: el ile birden fazla tam, fark, geri yüklemek zorunda kalmadan zamanında veritabanları belirli bir noktaya kurtarmak üzere portalı kullanın ve günlük yedekleri.
- **E-posta uyarıları hataları için birleştirilmiş**: yapılandırma hatalar için e-posta bildirimleri birleştirilir.
- **Rol tabanlı erişim denetimi**: kimin yedekleme yönetmek ve geri yükleme işlemleri Portalı aracılığıyla belirler.

Bir tanıtım birlikte nasıl çalıştığına ilişkin hızlı bir genel bakış için aşağıdaki videoyu izleyin:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2dNbw]

Bu Azure Backup çözümü SQL VM'ler için şu anda genel önizlemede değil. Daha fazla bilgi için bkz: [Azure SQL Server veritabanına yedekleme](../../../backup/backup-azure-sql-database.md).

## <a id="manual"></a> El ile yedekleme

El ile yedekleme yönetmek ve geri yükleme işlemleri, SQL vm'lerde istiyorsanız, kullanmakta olduğunuz SQL Server sürümüne bağlı olarak birkaç seçeneğiniz vardır. Yedekleme ve geri yüklemeye genel bakış için SQL Server sürümüne bağlı aşağıdaki makalelere bakın:

- [Yedekleme ve geri yükleme ve sonraki sürümler için SQL Server 2016](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases)
- [Yedekleme ve geri yükleme için SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms187048%28v=sql.120%29.aspx)
- [Yedekleme ve geri yükleme için SQL Server 2012](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx)
- [Yedekleme ve geri yüklemek için SQL Server SQL Server 2008 R2](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx)
- [Yedekleme ve geri yükleme için SQL Server 2008](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx)

Aşağıdaki bölümlerde, çeşitli el ile yedekleme açıklar ve geri yükleme seçenekleri daha ayrıntılı.

### <a name="backup-to-attached-disks"></a>Ekli diske yedekleme

Azure Vm'lerinde çalışan SQL Server için yerel yedekleme kullanın ve teknikleri bağlı disklerde VM için yedekleme dosyalarının hedef kullanarak geri yükleyin. Ancak, temel bir Azure sanal makinesine, attach disk sayısı için bir sınır yoktur [sanal makine boyutu](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Dikkate alınması gereken disk Yönetimi ek yükünü yoktur.

El ile SQL Server Management Studio (SSMS) veya Transact-SQL kullanarak tam bir veritabanı yedeği oluşturma örneği için bkz: [tam bir veritabanı yedekleme oluşturma](https://docs.microsoft.com/sql/relational-databases/backup-restore/create-a-full-database-backup-sql-server).

### <a name="backup-to-url"></a>URL'ye yedekleme

SQL Server 2012 SP1 CU2 ile başlayarak, yedekleme ve URL'ye yedek olarak da bilinen doğrudan Microsoft Azure Blob depolama alanına, geri yükleme. SQL Server 2016 Ayrıca bu özellik için aşağıdaki geliştirmeleri sunulmuştur:

| 2016 geliştirme | Ayrıntılar |
| --- | --- |
| **Şeritleme** |Microsoft Azure blob depolama alanına yedeklerken SQL Server 2016 12,8 TB'lık en büyük veritabanlarını yedeklemek etkinleştirmek için birden çok BLOB yedekleme destekler. |
| **Anlık görüntü yedekleme** |Azure anlık görüntüleri kullanımı ile neredeyse anlık yedeklemeler ve hızlı geri yüklemeler Azure Blob Depolama hizmetinin kullanarak depolanan veritabanı dosyaları için SQL Server dosya anlık görüntüsü yedekleme sağlar. Bu özellik, yedekleme basitleştirmek ve ilkelerini geri olanak sağlar. Dosya anlık görüntüsü yedekleme, geri yükleme noktası da destekler. Daha fazla bilgi için bkz: [azure'da veritabanı dosyaları için anlık görüntü yedeklerini](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure). |

Daha fazla bilgi için biri olan SQL Server sürümüne bağlı aşağıdaki makalelere bakın:

- **SQL Server 2016/2017**: [URL SQL Server Yedekleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service)
- **SQL Server 2014**: [URL SQL Server 2014 yedekleme](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx)
- **SQL Server 2012**: [URL SQL Server 2012 yedekleme](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx)

### <a name="managed-backup"></a>Yönetilen yedekleme

SQL Server 2014 ile başlayarak, yönetilen yedekleme yedeklemeleri Azure depolamaya oluşturulmasını otomatik hale getirir. Arka planda URL özelliği, bu makalenin önceki bölümde açıklanan yedekleme kullanımı yönetilen yedekleme yapar. Yönetilen yedekleme de SQL Server VM otomatik yedekleme hizmetini destekleyen temel özelliğidir.

SQL Server 2016'den itibaren yönetilen yedekleme zamanlaması, yedekleme ve tam sistem veritabanı ve günlük yedekleme sıklığı için ek seçenekler aldı.

Daha fazla bilgi için bir SQL Server sürümüne bağlı aşağıdaki makalelere bakın:

- [SQL Server 2016 için Microsoft Azure yedekleme ve daha sonra yönetilen](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure)
- [Yönetilen yedekleme SQL Server 2014 için Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx)

## <a name="decision-matrix"></a>Karar Matrisi

Aşağıdaki tabloda her azure'da SQL Server sanal makineler için yedekleme ve geri yükleme seçeneği yeteneklerini özetler.

|| **Otomatik Yedekleme** | **SQL Azure yedekleme** | **El ile yedekleme** |
|---|---|---|---|
| Ek Azure hizmeti gerektirir |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Azure portalında yedekleme ilkesini yapılandırma | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Azure portalında veritabanlarını geri yükleyin |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Tek bir Panoda birden çok sunucuyu yönetmek |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Belirli bir noktaya geri yükleme | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| 15 dakikalık kurtarma noktası hedefi (RPO) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| Kısa vadeli yedekleme bekletme ilkesi (gün) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Uzun vadeli yedekleme bekletme ilkesi (ay, yıl) |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| SQL Server Always On için yerleşik destek |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Azure depolama hesapları için yedekleme | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png)(otomatik) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png)(otomatik) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png)(müşteri yönetilen) |
| Depolama ve yedekleme dosyalarının yönetimi | | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |  |
| VM ekli disklerde yedekleme |   |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| Merkezi özelleştirilebilir yedekleme raporlar |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Hataları görmek için birleştirilmiş bir e-posta uyarıları |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| OMS üzerinde temel izleme özelleştirme |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| SSMS veya Transact-SQL komut dosyaları ile yedekleme işlerini izleme | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| SSMS veya Transact-SQL komut dosyaları ile veritabanlarını geri yükleyin | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |

## <a name="next-steps"></a>Sonraki adımlar

Dağıtımınızı Azure VM'deki SQL Server'ın planlıyorsanız aşağıdaki kılavuzda sağlama yönergeler bulabilirsiniz: [Azure portalında Windows SQL Server sanal makine sağlamak nasıl](virtual-machines-windows-portal-sql-server-provision.md).

Yedekleme ve geri yükleme, verileri geçirmek için kullanılabilir olmasa da, Azure VM'de SQL Server potansiyel olarak daha kolay veri geçiş yolu vardır. Geçiş seçenekleri ve öneriler tam bir tartışma için bkz: [SQL Server'a Azure VM'de bir veritabanını geçirme](virtual-machines-windows-migrate-sql.md).