---
title: "Yedekleme ve geri yüklemek için SQL Server | Microsoft Docs"
description: "Azure sanal makinelerde çalışan SQL Server veritabanları için yedekleme ve geri yükleme noktaları açıklar."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-resource-management
ms.assetid: 95a89072-0edf-49b5-88ed-584891c0e066
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/15/2016
ms.author: mikeray
ms.openlocfilehash: 65557938673c5442758396a47873be1016e0f71b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Azure Sanal Makineler’de SQL Server için Yedekleme ve Geri Yükleme
## <a name="overview"></a>Genel Bakış
Azure depolama, veri kaybı veya fiziksel veri bozulmasına karşı koruma sağlamak için her Azure VM disk 3 kopyasını tutar. Bu nedenle, şirket içi farklı olarak, bunlar hakkında endişelenmeniz gerekmez. Ancak, yine uygulama veya kullanıcıya hataları (yanlış veri ekleme ya da bir tablo silme örneğin) karşı korumak için SQL Server veritabanlarınızı yedeklemeniz gerekir ve zaman içinde bir noktaya geri yükleyebilirsiniz.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Azure Vm'lerinde çalışan SQL Server için yerel yedekleme kullanın ve teknikleri bağlı disk yedekleme dosyalarının hedefini kullanarak geri yükleyin. Ancak, temel bir Azure sanal makinesine, attach disk sayısı için bir sınır yoktur [sanal makine boyutu](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Dikkate alınması gereken disk Yönetimi ek yükünü yoktur.

SQL Server 2014 ile başlayarak, yedekleme ve Microsoft Azure Blob depolama alanına geri yükleme. SQL Server 2016 de bu seçenek için geliştirmeler içerir. Ayrıca, Microsoft Azure Blob depolamada depolanan veritabanı dosyaları için SQL Server 2016 bir seçenek neredeyse anlık yedeklemeler için ve Azure anlık görüntülerini kullanarak hızlı geri yüklemeler için sağlar. Bu makalede bu seçeneklerine genel bakış sağlar ve ek bilgiler bulunabilir [SQL Server Yedekleme ve geri yükleme ile Microsoft Azure Blob Depolama hizmetinin](https://msdn.microsoft.com/library/jj919148.aspx).

> [!NOTE]
> Çok büyük veritabanlarını yedekleme seçeneklerini tartışma için bkz [çoklu terabayt SQL Server veritabanı yedekleme stratejilerini Azure Virtual Machines için](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).
> 
> 

Aşağıdaki bölümlerde farklı bir Azure sanal makinesi desteklenen bir SQL Server sürümleri özgü bilgileri içerir.

## <a name="sql-server-virtual-machines"></a>SQL Server sanal makineler
Bir Azure sanal makinesinde SQL Server örneğinizi çalıştırırken, veritabanı dosyalarınızı veri disklerde Azure'da zaten bulunur. Bu diskleri Azure Blob depolama alanına dinamik. Bu nedenle, veritabanını ve yaklaşımlar, yedekleme nedenleri değişiklik biraz alın. Aşağıdakileri göz önünde bulundurun. 

* Artık Microsoft Azure Microsoft Azure hizmetin bir parçası olarak bu koruma sağladığından donanım ve medya hatalarına karşı koruma sağlamak üzere veritabanı yedeklemeleri gerçekleştirmek için gerekir.
* Hala kullanıcı hataları karşı ya da arşivleme amacıyla, yasal nedenlerden veya yönetimsel amaçlar için koruma sağlamak üzere veritabanı yedeklemeleri gerçekleştirmek için gerekir.
* Yedekleme dosyasını doğrudan Azure'de depolayabilirsiniz. Daha fazla bilgi için SQL Server'ın farklı sürümleri için kılavuzluk aşağıdaki bölümlere bakın.

## <a name="sql-server-2016"></a>SQL Server 2016
Microsoft SQL Server 2016 destekleyen [yedekleme ve geri yükleme ile Azure BLOB'ları](https://msdn.microsoft.com/library/jj919148.aspx) özellikleri SQL Server 2014'te bulundu. Ancak aynı zamanda aşağıdaki geliştirmeleri içerir:

| 2016 geliştirme | Ayrıntılar |
| --- | --- |
| **Şeritleme** |Microsoft Azure blob depolama alanına yedeklerken SQL Server 2016 12,8 TB'lık en büyük veritabanlarını yedeklemek etkinleştirmek için birden çok BLOB yedekleme destekler. |
| **Anlık görüntü yedekleme** |Azure anlık görüntüleri kullanımı ile neredeyse anlık yedeklemeler ve hızlı geri yüklemeler Azure Blob Depolama hizmetinin kullanarak depolanan veritabanı dosyaları için SQL Server dosya anlık görüntüsü yedekleme sağlar. Bu özellik, yedekleme basitleştirmek ve ilkelerini geri olanak sağlar. Dosya anlık görüntüsü yedekleme, geri yükleme noktası da destekler. Daha fazla bilgi için bkz: [azure'da veritabanı dosyaları için anlık görüntü yedeklerini](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx). |
| **Yedekleme zamanlaması yönetilen** |SQL Server yönetilen yedekleme Azure artık özel zamanlamaları destekler. Daha fazla bilgi için bkz: [Microsoft Azure SQL Server yönetilen yedekleme](https://msdn.microsoft.com/library/dn449496.aspx). |

Azure Blob Depolama kullanırken SQL Server 2016 özelliklerini öğretici için bkz [Öğreticisi: SQL Server 2016 veritabanları ile Microsoft Azure Blob Depolama hizmetinin kullanarak](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQL Server 2014
SQL Server 2014 aşağıdaki geliştirmeleri içerir:

1. **Yedekleme ve geri yükleme için Azure**:
   
   * *SQL Server Yedekleme URL'ye* desteği SQL Server Management Studio'da artık sahiptir. Seçenek Azure'a yedeklemek için yedekleme veya geri yükleme görev ya da bakım planı oluşturma sihirbazını SQL Server Management Studio'da kullanırken kullanıma sunulmuştur. Daha fazla bilgi için bkz: [SQL Server Yedekleme URL'ye](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
   * *SQL Server yönetilen yedekleme Azure* otomatik yedekleme yönetim sağlayan yeni işlevselliğe sahiptir. Bir Azure makinede çalışan SQL Server 2014 örnekleri için Yedekleme Yönetimi otomatikleştirmek için özellikle yararlıdır. Daha fazla bilgi için bkz: [Microsoft Azure SQL Server yönetilen yedekleme](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
   * *Otomatik yedekleme* otomatik olarak etkinleştirmek için ek Otomasyon sağlar *SQL Server yönetilen yedekleme Azure* tüm mevcut ve yeni veritabanları için azure'da bir SQL Server VM üzerinde. Daha fazla bilgi için bkz. [Azure Virtual Machines’de SQL Server için Otomatik Yedekleme](virtual-machines-windows-sql-automated-backup.md).
   * Azure SQL Server 2014 yedekleme için tüm seçenekleri genel bakış için bkz: [SQL Server Yedekleme ve geri yükleme ile Microsoft Azure Blob Depolama hizmetinin](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
2. **Şifreleme**: SQL Server 2014'ü destekleyen bir yedek oluştururken veri şifreleme. Birçok şifreleme algoritmalarını ve kullanımını destekleyen osf bir sertifika veya asimetrik anahtar. Daha fazla bilgi için bkz: [yedekleme şifreleme](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>SQL Server 2012
SQL Server Yedekleme ve geri yükleme SQL Server 2012'de hakkında ayrıntılı bilgi için bkz: [yedekleme ve geri yükleme, SQL Server veritabanları (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

SQL Server 2012 SP1 toplu güncelleştirme 2'de başlayarak, yedekleme ve Azure Blob Storage hizmetinden geri yükleyin. Bu geliştirme, bir Azure sanal makine veya bir şirket içi örneği üzerinde çalışan bir SQL Server üzerinde SQL Server veritabanlarını yedeklemek için kullanılabilir. Daha fazla bilgi için bkz: [SQL Server Yedekleme ve geri yükleme ile Azure Blob Depolama hizmetinin](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Azure Blob Depolama hizmetinin kullanmanın avantajları bazıları için 16 disk sınırı bağlı diskler, yönetim, SQL Server örneği bir Azure sanal makine üzerinde çalışan başka bir örneği için yedek dosyasını doğrudan kullanılabilirliğini kolaylığı atlama olanağı içerir , veya bir şirket içi örneğiyle geçiş veya olağanüstü durum kurtarma amacıyla. SQL Server Yedekleme için bir Azure blob depolama hizmeti kullanmanın avantajları tam bir listesi için bkz: *avantajları* bölümüne [SQL Server Yedekleme ve geri yükleme ile Azure Blob Depolama hizmetinin](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

En iyi uygulama önerileri ve sorun giderme bilgileri için bkz: [yedekleme ve geri yükleme en iyi yöntemler (Azure Blob Depolama hizmetinin)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQL Server 2008
SQL Server Yedekleme ve geri yükleme SQL Server 2008 R2 için bkz: [yedekleme ve geri yükleme veritabanlarını SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

SQL Server Yedekleme ve geri yükleme SQL Server 2008 için bkz: [yedekleme ve geri yükleme veritabanlarını SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Sonraki adımlar
Dağıtımınızı Azure VM'deki SQL Server'ın planlıyorsanız aşağıdaki öğreticide sağlama yönergeler bulabilirsiniz: [Azure Resource Manager ile Azure SQL Server sanal makineye sağlama](virtual-machines-windows-portal-sql-server-provision.md).

Yedekleme ve geri yükleme, verileri geçirmek için kullanılabilir olmasa da, Azure VM'de SQL Server potansiyel olarak daha kolay veri geçiş yolu vardır. Geçiş seçenekleri ve öneriler tam bir tartışma için bkz: [SQL Server'a Azure VM'de bir veritabanını geçirme](virtual-machines-windows-migrate-sql.md).

Diğer gözden [SQL Server Azure sanal makinelerde çalışan kaynakları](virtual-machines-windows-sql-server-iaas-overview.md).

