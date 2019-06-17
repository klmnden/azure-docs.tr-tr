---
title: Yedekleme ve Azure Vm'lerde SQL Server için geri yükleme | Microsoft Docs
description: Azure sanal makineler üzerinde çalışan SQL Server veritabanları için yedekleme ve geri yükleme noktaları açıklar.
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
ms.openlocfilehash: ab239d0546508d74874c6b6be03f6afc06b08fa7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60563427"
---
# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Azure Sanal Makineler’de SQL Server için Yedekleme ve Geri Yükleme

Bu makalede azure'da bir Windows sanal makinesinde çalışan yedekleme ve geri yükleme seçenekleri için SQL Server hakkında yönergeler sağlanır. Azure depolama, veri kaybı veya fiziksel veri bozulmasına karşı koruma sağlamak için her Azure VM disk üç kopyasını tutar. Bu nedenle, şirket içi, donanım hatalarını üzerinde odaklanın gerekmez. Ancak, yine de yanlışlıkla veri eklemeler ve silmeleri gibi uygulama veya kullanıcıya hatalara karşı korumak için SQL Server veritabanlarını yedeklemeniz gerekir. Bu durumda, belirli bir noktaya geri yükleme önemlidir.

Bu makalenin ilk bölümünü kullanılabilir yedekleme ve geri yükleme seçenekleri hakkında genel bir bakış sağlar. Bu, her stratejisi hakkında daha fazla bilgi sağlayan bir bölüm tarafından izlenir.

## <a name="backup-and-restore-options"></a>Yedekleme ve geri yükleme seçenekleri

Aşağıdaki tabloda Azure Vm'lerinde çalışan SQL Server için çeşitli yedekleme ve geri yükleme seçenekleri hakkında bilgi sağlar:

| Stratejisi | SQL sürümleri | Açıklama |
|---|---|---|
| [Otomatik Yedekleme](#automated) | 2014<br/> 2016<br/> 2017 | Otomatik yedekleme, SQL Server VM üzerindeki tüm veritabanları için normal yedeklemelerinin zamanlamasını olanak tanır. Yedeklemeler, 30 gün boyunca Azure depolama alanında depolanır. Otomatik yedekleme v2, SQL Server 2016 ile başlayarak, el ile zamanlama ve tam sıklığını ve günlük yedekleri yapılandırma gibi ek seçenekleri sunar. |
| [SQL VM'leri için Azure Backup](#azbackup) | 2012<br/> 2014<br/> 2016<br/> 2017 | Azure Backup, Azure Vm'lerinde çalışan SQL Server için kurumsal sınıf yedekleme özelliği sağlar. Bu hizmet ile birden çok sunucu ve binlerce veritabanının yedeklerini merkezi olarak yönetebilir. Veritabanları, portalında zaman içinde belirli bir noktaya geri yüklenebilir. Yedeklemeleri yıllarca saklayabilirsiniz bir özelleştirilebilir bekletme ilkesi sunar. Bu özellik şu anda genel Önizleme aşamasındadır. |
| [El ile yedekleme](#manual) | Tümü | SQL Server sürümüne bağlı olarak, el ile yedekleme ve bir Azure sanal makinesinde çalışan SQL Server'a geri yüklemek için çeşitli teknikler vardır. Bu senaryoda, veritabanlarınızı nasıl yedeklenir ve depolama konumu ve bu yedeklemeler yönetimini sorumludur. |

Aşağıdaki bölümlerde her seçeneği daha ayrıntılı açıklanmaktadır. Bu makalenin son bölümü, bir özellik matrisi biçiminde bir özetini sağlar.

## <a id="automated"></a> Otomatik yedekleme

Otomatik yedekleme, azure'daki bir Windows VM'de çalışan SQL Server Standard ve Enterprise sürümleri için otomatik bir yedekleme hizmeti sağlar. Bu hizmet tarafından sağlanan [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md), otomatik olarak yüklendiği SQL Server Windows sanal makine görüntüleri Azure portalında üzerinde.

Tüm veritabanları, yapılandırdığınız Azure depolama hesabı için desteklenir. Yedeklemeleri şifrelenir ve 30 gün boyunca tutulur.

SQL Server 2016 ve üzeri Vm'leri otomatik yedekleme v2 ile daha fazla özelleştirme seçenekleri sunar. Bu geliştirmeler şunları içerir:

- Sistem Veritabanı Yedeklemeleri
- El ile yedekleme zamanlaması ve zaman penceresi
- Tam ve günlük dosyasını yedekleme sıklığı

Bir veritabanını geri yüklemek için gerekli yedekleme dosyalarının depolama hesabında bulun ve SQL SQL Server Management Studio (SSMS) ya da Transact-SQL komutlarını kullanarak sanal makinenizde bir geri yükleme gerçekleştirin.

SQL VM'ler için otomatik yedekleme yapılandırma hakkında daha fazla bilgi için aşağıdaki makalelerden birine bakın:

- **SQL Server 2016/2017**: [Azure sanal makineleri için otomatik yedekleme v2](virtual-machines-windows-sql-automated-backup-v2.md)
- **SQL Server 2014**: [SQL Server 2014 sanal makineleri için otomatik yedekleme](virtual-machines-windows-sql-automated-backup.md)

## <a id="azbackup"></a> SQL Vm'leri için Azure yedekleme

[Azure yedekleme](/azure/backup/) Azure Vm'lerinde çalışan SQL Server için kurumsal sınıf yedekleme özelliği sağlar. Tüm yedeklemeler depolanır ve bir kurtarma Hizmetleri Kasası'nda yönetilen. Bu çözüm, özellikle kuruluşlar için sağladığı çeşitli avantajları vardır:

- **Sıfır altyapı yedeklemesine**: Yedekleme sunucusu veya depolama konumlarına yönetmek zorunda değildir.
- **Ölçek**: Çok sayıda SQL VM ve binlerce koruyun.
- **Kullandıkça Öde**: Bu özellik, Azure Backup tarafından sağlanan ayrı bir hizmet olmakla birlikte olarak tüm Azure Hizmetleri ile yalnızca kullandığınız kadarı için ödeme yaparsınız.
- **Merkezi Yönetim ve izleme**: Tüm destekleyen Azure Backup, azure'daki tek bir panodan diğer iş yükleri de dahil olmak üzere Yedeklemelerinizin merkezi olarak yönetin.
- **Yedekleme ve bekletme temelli İlkesi**: Standart yedekleme ilkelerine düzenli yedeklemeler oluşturun. Yıllık yedeklemeler için bekletme ilkeleri oluşturun.
- **Desteklemek için SQL her zaman şirket**: Algılayın ve bir SQL Server Always On yapılandırmayı korumak ve yedekleme kullanılabilirlik grubu yedekleme tercihini uymanız.
- **15 dakikalık kurtarma noktası hedefi (RPO)** : SQL işlem günlüğü yedeklemeleri 15 dakikada bir kadar yapılandırın.
- **Noktaya geri yükleme noktası**: Günlük yedeklemelerine ve el ile birden fazla tam, değişiklik geri yüklemeye gerek kalmadan veritabanları belirli bir noktaya geri için portal'ı kullanın.
- **Hatalar için e-posta uyarıları birleştirilmiş**: Hatalar için birleştirilmiş e-posta bildirimleri yapılandırın.
- **Rol tabanlı erişim denetimi**: Kimin yedeklemeyi yönetme ve geri yükleme işlemleri portal üzerinden belirler.

Bir tanıtım ile birlikte nasıl çalıştığını ilişkin hızlı genel bakış için aşağıdaki videoyu izleyin:

> [!VIDEO https://www.youtube.com/embed/wmbANpHos_E]

Bu SQL VM'ler için Azure Backup çözümü, genel kullanıma sunulmuştur. Daha fazla bilgi için [SQL Server veritabanını azure'a yedekleme](../../../backup/backup-azure-sql-database.md).

## <a id="manual"></a> El ile yedekleme

El ile yedeklemeyi yönetme ve SQL Vm'lerinizi işlemleri geri yüklemek istiyorsanız, kullanmakta olduğunuz SQL Server sürümüne bağlı olarak birkaç seçenek vardır. Yedekleme ve geri yüklemeye genel bakış için SQL Server sürümünü temel alan aşağıdaki makalelerden birine bakın:

- [Yedekleme ve geri yükleme için SQL Server 2016 ve üzeri](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases)
- [Yedekleme ve geri yükleme için SQL Server 2014](https://msdn.microsoft.com/library/ms187048%28v=sql.120%29.aspx)
- [Yedekleme ve geri yükleme için SQL Server 2012](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx)
- [Yedekleme ve geri yükleme için SQL Server SQL Server 2008 R2](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx)
- [Yedekleme ve geri yükleme için SQL Server 2008](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx)

Aşağıdaki bölümlerde, çeşitli el ile yedekleme açıklar ve geri yükleme seçenekleri ayrıntılı.

### <a name="backup-to-attached-disks"></a>Bağlı diskleri yedekleme

Azure Vm'lerinde çalışan SQL Server için yerel yedekleme kullanın ve teknikleri kullanıma açılan diskler, sanal makinede yedekleme dosyalarının hedefini kullanarak geri yükleyebilirsiniz. Ancak, disk sayısına ilişkin temel bir Azure sanal makinesine, eklemek için bir sınır yoktur [sanal makinenin boyutunu](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Dikkate alınması gereken disk Yönetimi ek yükü de mevcuttur.

El ile SQL Server Management Studio (SSMS) ya da Transact-SQL kullanarak tam bir veritabanı yedeği oluşturma örneği için bkz: [tam bir veritabanı yedekleme oluşturma](https://docs.microsoft.com/sql/relational-databases/backup-restore/create-a-full-database-backup-sql-server).

### <a name="backup-to-url"></a>URL'ye yedekleme

SQL Server 2012 SP1 CU2 ile başlayarak, yedekleme ve URL'sine yedek olarak da bilinen doğrudan Microsoft Azure Blob depolama alanına geri yükleyebilirsiniz. SQL Server 2016, bu özellik için aşağıdaki geliştirmeler de kullanıma sunulmuştur:

| 2016 geliştirme | Ayrıntılar |
| --- | --- |
| **Şeritleme** |Microsoft Azure blob depolama alanına yedeklenmesi, SQL Server 2016'yı etkinleştirmek 12,8 TB en büyük veritabanlarını yedeklemek için birden çok BLOB'ları yedeklemenin destekler. |
| **Anlık görüntü yedekleme** |Azure anlık görüntüleri kullanarak SQL Server dosya anlık görüntüsü Backup neredeyse anında yedekleme ile depolanan Azure Blob Depolama hizmetini kullanarak veritabanı dosyaları için hızlı geri yüklemeler sağlar. Bu özellik, yedekleme basitleştirin ve ilkeleri geri yüklemenize olanak sağlar. Dosya anlık görüntüsü backup, zaman geri yükleme noktası da destekler. Daha fazla bilgi için [azure'daki veritabanı dosyaları için anlık görüntüsü yedekleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure). |

Daha fazla bilgi için bir SQL Server sürümünü temel alan aşağıdaki makalelere bakın:

- **SQL Server 2016/2017**: [URL'ye SQL Server Yedekleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service)
- **SQL Server 2014**: [URL'ye SQL Server 2014 yedekleme](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx)
- **SQL Server 2012**: [URL'ye yedekleme SQL Server 2012](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx)

### <a name="managed-backup"></a>Yönetilen yedekleme

SQL Server 2014 ile başlayarak, yönetilen yedekleme Azure depolama için yedeklemeleri oluşturulmasını otomatik hale getirir. Arka planda URL özelliği bu makalenin önceki bölümde açıklanan yedekleme kullanımı yönetilen yedekleme yapar. Yönetilen yedekleme de SQL Server VM otomatik yedekleme hizmetinin desteklediği temel özelliğidir.

SQL Server 2016'dan başlayarak, yönetilen yedekleme zamanlaması, yedekleme ve tam sistem veritabanı ve günlük yedekleme sıklığı için ek seçenekler alındı.

Daha fazla bilgi için SQL Server sürümünü temel alan aşağıdaki makalelerden birine bakın:

- [Yönetilen SQL Server 2016 için Microsoft Azure yedekleme ve üzeri](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure)
- [SQL Server 2014 için Microsoft Azure tarafından yönetilen yedekleme](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx)

## <a name="decision-matrix"></a>Karar Matrisi

Aşağıdaki tabloda, her Azure sanal makineler'de SQL Server için yedekleme ve geri yükleme seçeneği yeteneklerini özetler.

|| **Otomatik Yedekleme** | **SQL Azure yedekleme** | **El ile yedekleme** |
|---|---|---|---|
| Ek Azure hizmeti gerektirir |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Azure portalında yedekleme ilkesi yapılandırma | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Azure Portalı'nda veritabanlarını geri yükleme |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Tek bir Panoda birden çok sunucuyu yönetme |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Belirli bir noktaya geri yükleme | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| 15 dakikalık kurtarma noktası hedefi (RPO) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| Kısa vadeli yedekleme bekletme ilkesi (gün) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Uzun süreli yedek saklama ilkesini (ay, yıl) |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| SQL Server Always On için yerleşik destek |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Azure depolama hesapları için yedekleme | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png)(otomatik) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png)(otomatik) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png)(müşteri tarafından yönetilen) |
| Depolama ve yedekleme dosyaların Yönetimi | | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |  |
| VM üzerinde kullanıma açılan diskler için yedekleme |   |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| Merkezi özelleştirilebilir yedekleme raporları |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Hatalar için birleştirilmiş e-posta uyarıları |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Azure İzleyici günlüklerine göre izlemeyi özelleştirme |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| SSMS veya Transact-SQL betikleri ile yedekleme işleri İzle | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| SSMS veya Transact-SQL betikleri ile veritabanlarını geri yükleme | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |

## <a name="next-steps"></a>Sonraki adımlar

Azure VM'de SQL Server dağıtımını planlıyorsanız, aşağıdaki kılavuzda sağlama kılavuzu bulabilirsiniz: [Azure portalında Windows SQL Server bir sanal makine sağlamak nasıl](virtual-machines-windows-portal-sql-server-provision.md).

Yedekleme ve geri yükleme, verileri geçirmek için kullanılabilir olsa da, Azure VM'deki SQL Server'a potansiyel olarak daha kolay veri geçiş yolu vardır. Geçiş seçenekleri ve öneriler tam bir irdelemesi için bkz. [bir veritabanını Azure VM'deki SQL Server'a geçirme](virtual-machines-windows-migrate-sql.md).
