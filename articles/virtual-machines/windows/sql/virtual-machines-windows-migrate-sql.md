---
title: "SQL Server VM üzerinde bir SQL Server veritabanı geçirilecek | Microsoft Docs"
description: "Bir şirket içi kullanıcı veritabanını SQL Server'a bir Azure sanal makine geçirme hakkında bilgi edinin."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: craigg
editor: 
tags: azure-service-management
ms.assetid: 00fd08c6-98fa-4d62-a3b8-ca20aa5246b1
ms.service: virtual-machines-sql
ms.workload: iaas-sql-server
ms.tgt_pltfrm: vm-windows-sql-server
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: jroth
ms.openlocfilehash: 64245a968eca94518d2e4238b4bc5c93c952563a
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="migrate-a-sql-server-database-to-sql-server-in-an-azure-vm"></a>Bir SQL Server veritabanını Azure VM’deki SQL Server’a geçirme

Çeşitli Azure VM'deki SQL Server için bir şirket içi SQL Server kullanıcı veritabanını taşımak için yöntemler vardır. Bu makalede, kısa bir süre çeşitli yöntemlerle ele almaktadır ve en iyi yöntem çeşitli senaryolar için önerilir.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="what-are-the-primary-migration-methods"></a>Birincil geçiş yöntemleri nelerdir?
Birincil geçiş yöntemler şunlardır:

* Sıkıştırma kullanarak şirket içi Yedekleme gerçekleştirmek ve yedek dosyayı Azure sanal makinesine el ile kopyalayın
* URL ve Azure sanal makinede geri yükleme URL'den yedekleme yapma
* Ayırın ve ardından Azure blob depolama alanına veri ve günlük dosyaları kopyalayın ve ardından Azure VM'deki SQL Server URL'den ekleyin.
* Hyper-V VHD'ye şirket içi fiziksel makineyi Dönüştür, Azure Blob depolama alanına yükleme ve sonra yeni VM kullanarak VHD karşıya olarak dağıtma
* Sevk sabit sürücü Windows içeri/dışarı aktarma hizmeti kullanma
* Varsa bir AlwaysOn dağıtımını şirket içi, kullanın [Azure çoğaltma Ekleme Sihirbazı'nı](../sqlclassic/virtual-machines-windows-classic-sql-onprem-availability.md) kullanıcılar Azure ve sonra Yük devretme bir çoğaltma oluşturmak için Azure veritabanı örneğine işaret eden
* SQL Server kullanmak [işlemsel çoğaltma](https://msdn.microsoft.com/library/ms151176.aspx) kullanıcılar Azure SQL Server örneği bir abone olarak yapılandırmak ve çoğaltma devre dışı bırakmak için Azure veritabanı örneğine işaret eden

> [!TIP]
> Bu aynı teknikler, Azure SQL Server Vm'lerinin arasında veritabanlarını taşıma için de kullanabilirsiniz. Örneğin, bir SQL Server galeri görüntüsü VM bir sürüm/sürümünden yükseltme için desteklenen yolu yoktur. Bu durumda, yeni bir SQL Server VM ile yeni sürümü/yayını oluşturmak ve ardından geçiş tekniklerden birini bu makalede, veritabanlarını taşıma kullanın gerekir. 

## <a name="choosing-your-migration-method"></a>Geçiş yöntemini seçme
En iyi veri aktarımı performans için veritabanı dosyaları sıkıştırılmış bir yedekleme dosyası kullanarak Azure VM'de oturum geçirin.

Veritabanı geçiş işlemi sırasında kapalı kalma süresini en aza indirmek için AlwaysOn seçeneği veya işlemsel çoğaltma seçeneğini kullanın.

Yukarıdaki yöntemlerden kullanmak mümkün değilse, el ile veritabanınızı geçirin. Bu yöntemi kullanarak, Azure'da Veritabanı yedeğinin bir kopyası tarafından izlenen bir veritabanı yedeği genellikle başlayın ve bir veritabanı geri yükleme gerçekleştirin. Ayrıca Azure'da veritabanı dosyalarını kopyalamak ve ardından bunları ekleyin. Bir veritabanı bir Azure VM geçirme bu el ile işlem gerçekleştirmek çeşitli yöntemler vardır.

> [!NOTE]
> SQL Server 2014 veya SQL Server 2016 için SQL Server'ın önceki sürümlerinden yükseltme yaptığınızda, değişiklikler gerekip gerekmediğini düşünmelisiniz. Geçiş projenizin bir parçası olarak yeni SQL Server sürümü tarafından desteklenmeyen özellikleri tüm bağımlılıkları adres öneririz. Senaryolar ve desteklenen sürümleri hakkında daha fazla bilgi için bkz: [yükseltme için SQL Server](https://msdn.microsoft.com/library/bb677622.aspx).

Aşağıdaki tabloda birincil geçiş yöntemlerinin her birini listeler ve her yöntemin kullanılması en uygun olduğunda açıklanır.

| Yöntem | Kaynak veritabanı sürümü | Hedef veritabanı sürümü | Kaynak veritabanı yedekleme boyutu kısıtlaması | Notlar |
| --- | --- | --- | --- | --- |
| [Sıkıştırma kullanarak şirket içi Yedekleme gerçekleştirmek ve yedek dosyayı Azure sanal makinesine el ile kopyalayın](#backup-and-restore) |SQL Server 2005 veya üzeri |SQL Server 2005 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Makine genelinde veritabanlarını taşıma çok basit ve iyi sınanmış bir yöntem budur. |
| [URL ve Azure sanal makinede geri yükleme URL'den yedekleme yapma](#backup-to-url-and-restore) |SQL Server 2012 SP1 CU2 veya daha büyük |SQL Server 2012 SP1 CU2 veya daha büyük |< 12,8 TB SQL Server 2016, aksi takdirde < 1 TB | Bu yöntem yedekleme dosyası Azure depolama kullanarak VM taşımak için başka bir yoludur. |
| [Detach, Azure blob depolama alanına veri ve günlük dosyalarını kopyalayabilir ve URL'den'da Azure sanal makinede SQL Server'a iliştirin](#detach-and-attach-from-url) |SQL Server 2005 veya üzeri |SQL Server 2014 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Bu yöntemi kullanmak için planlama yaparken [Azure Blob Depolama hizmetinin kullanarak bu dosyaları saklamak](https://msdn.microsoft.com/library/dn385720.aspx) ve bunları Azure VM'deki, özellikle çok büyük veritabanları ile çalışan SQL Server ekleme |
| [Şirket içi makineyi Hyper-V VHD'leri dönüştürme, Azure Blob depolama alanına yükleme ve karşıya yüklenen VHD kullanarak yeni bir sanal makine dağıtma](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) |SQL Server 2005 veya üzeri |SQL Server 2005 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Şu durumlarda kullanın [kendi SQL Server lisansınızı getiren](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md), SQL Server'ın eski bir sürümünde çalışacak bir veritabanı geçirilirken veya sistem ve kullanıcı veritabanlarını veritabanının diğer kullanıcı veritabanlarını ve/veya sistem veritabanları üzerinde bağımlı geçiş işleminin parçası olarak birlikte geçirilirken. |
| [Sevk sabit sürücü Windows içeri/dışarı aktarma hizmeti kullanma](#ship-hard-drive) |SQL Server 2005 veya üzeri |SQL Server 2005 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Kullanım [Windows içeri/dışarı aktarma hizmeti](../../../storage/common/storage-import-export-service.md) el ile kopyalama yöntemi olduğunda çok büyük veritabanları ile çok yavaşsa, gibi |
| [Kullanım Azure Yineleme Sihirbazı Ekle](../sqlclassic/virtual-machines-windows-classic-sql-onprem-availability.md) |SQL Server 2012 veya daha büyük |SQL Server 2012 veya daha büyük |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Her zaman açık şirket içi dağıtıma sahip olduğunuzda kullanın kapalı kalma süresi en aza indirir |
| [SQL Server işlem çoğaltma kullanın](https://msdn.microsoft.com/library/ms151176.aspx) |SQL Server 2005 veya üzeri |SQL Server 2005 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Kapalı kalma süresini en aza indirmek gerektiğinde kullanın ve her zaman açık şirket içi dağıtıma sahip değil |

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme
Veritabanınızı sıkıştırmayla yedekleyin VM yedekleme kopyalayın ve veritabanını geri yükleyin. Yedekleme dosyanızı 1 TB'den büyükse, bir VM disk maksimum boyutu 1 TB olduğundan bu şeritler gerekir. Bu el ile yükleme yöntemini kullanarak bir kullanıcı veritabanı geçirmek için aşağıdaki genel adımları kullanın:

1. Şirket içi bir konuma tam veritabanı yedeklemesi gerçekleştirin.
2. Oluşturun veya bir sanal makine istenen SQL Server sürümünü yükleyin.
3. Bağlantı gereksinimlerinize göre ayarlayın. Bkz: [Azure (Kaynak Yöneticisi) SQL Server sanal makineye bağlanmak](virtual-machines-windows-sql-connect.md).
4. Uzak Masaüstü, Windows Gezgini veya bir komut isteminden copy komutu kullanarak, VM için yedekleme dosyalarınız kopyalayın.

## <a name="backup-to-url-and-restore"></a>URL ve geri yükleme için yedekleme
Yerel bir dosya yedekleme yerine kullanabileceğiniz [URL'ye yedekleme](https://msdn.microsoft.com/library/dn435916.aspx) ve VM URL'den geri yükleyin. SQL Server 2016 ile şeritli yedekleme kümelerini desteklenir, performans için önerilen ve blob başına boyutu sınırları karşıması için gerekli. Çok büyük veritabanları, kullanımı için [Windows içeri/dışarı aktarma hizmeti](../../../storage/common/storage-import-export-service.md) önerilir.

## <a name="detach-and-attach-from-url"></a>Ayırma ve URL'den ekleme
Veritabanı ve günlük dosyalarınızı detach ve bunlara aktarım [Azure Blob Depolama](https://msdn.microsoft.com/library/dn385720.aspx). Ardından, Azure VM'de URL'den veritabanı iliştirilemiyor. Fiziksel veritabanı dosyalarının Blob depolama alanına bulunmasını istiyorsanız, bunu kullanın. Bu, çok büyük veritabanları için yararlı olabilir. Bu el ile yükleme yöntemini kullanarak bir kullanıcı veritabanı geçirmek için aşağıdaki genel adımları kullanın:

1. Şirket içi veritabanı örneğinin veritabanı dosyalarından kullanımdan çıkarın.
2. Azure blob depolama kullanılarak'ye ayrılmış veritabanı dosyalarını kopyalamak [AZCopy komut satırı yardımcı programı](../../../storage/common/storage-use-azcopy.md).
3. Veritabanı dosyaları Azure VM'deki SQL Server örneğini Azure URL'den iliştirin.

## <a name="convert-to-vm-and-upload-to-url-and-deploy-as-new-vm"></a>VM'ye dönüştürüp URL'ye yükleme ve yeni VM olarak dağıtma
Şirket içi SQL Server örneğindeki tüm sistem ve kullanıcı veritabanları için Azure sanal makineyi geçirmek için bu yöntemi kullanın. Tüm SQL Server örneğini el ile bu yöntemi kullanarak geçirmek için aşağıdaki genel adımları kullanın:

1. Fiziksel veya sanal makineleri kullanarak Hyper-V VHD'leri dönüştürme [Microsoft Virtual Machine Converter](https://technet.microsoft.com/library/dn874008(v=ws.11).aspx).
2. Azure depolama birimine kullanarak VHD dosyaları karşıya [Ekle-AzureVHD cmdlet'i](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx).
3. Karşıya yüklenen VHD kullanarak yeni bir sanal makine dağıtın.

> [!NOTE]
> Tüm bir uygulamayı geçirmek üzere kullanmayı [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md)].

## <a name="ship-hard-drive"></a>Sevk sabit sürücü
Kullanım [Windows içeri/dışarı aktarma hizmeti yöntemi](../../../storage/common/storage-import-export-service.md) Azure Blob Depolama ağ üzerinden karşıya olduğu şekilde basımı karşılamayacak kadar pahalı veya değil uygun durumlarda büyük miktarlarda dosya veri aktarmak için. Bu hizmet ile verilerinizi depolama hesabınıza burada karşıya yüklenecek bir Azure veri merkezi için bu verileri içeren bir veya daha fazla sabit sürücüler gönderin.

## <a name="next-steps"></a>Sonraki Adımlar
SQL Server Azure sanal makinelerde çalıştırma hakkında daha fazla bilgi için bkz: [Azure sanal makinelere genel bakış SQL Server'da](virtual-machines-windows-sql-server-iaas-overview.md).

> [!TIP]
> SQL Server sanal makineler hakkında sorularınız varsa bkz [ilgili sık sorulan sorular](virtual-machines-windows-sql-server-iaas-faq.md).

Yakalanan görüntüden bir Azure SQL Server sanal makine oluşturma ile ilgili yönergeler için bkz: [ipuçları ve püf noktaları 'yakalanan görüntülerin Azure SQL sanal makineleri kopyalama'](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) CSS SQL Server mühendisleri blogunda.

