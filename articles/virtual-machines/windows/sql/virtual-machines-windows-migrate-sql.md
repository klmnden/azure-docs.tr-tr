---
title: SQL Server VM üzerinde bir SQL Server veritabanını geçirme | Microsoft Docs
description: Bir şirket içi kullanıcı veritabanını bir Azure sanal makineler'de SQL Server'a geçirme hakkında bilgi edinin.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 00fd08c6-98fa-4d62-a3b8-ca20aa5246b1
ms.service: virtual-machines-sql
ms.workload: iaas-sql-server
ms.tgt_pltfrm: vm-windows-sql-server
ms.devlang: na
ms.topic: article
ms.date: 08/18/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 8e5a7bfc243fc8c797ffc66b2130756567ddc0fb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65795789"
---
# <a name="migrate-a-sql-server-database-to-sql-server-in-an-azure-vm"></a>Bir SQL Server veritabanını Azure VM’deki SQL Server’a geçirme

Bir şirket içi SQL Server kullanıcı veritabanını Azure VM'deki SQL Server'a geçirmek için yöntemler vardır. Bu makalede, kısa bir süre çeşitli yöntemleri ele ve en iyi yöntem çeşitli senaryolar için önerilir.


[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

  > [!NOTE]
  > SQL Server 2008 ve SQL Server 2008 R2 yaklaşan [destek yaşam döngüsü sonuna](https://www.microsoft.com/sql-server/sql-server-2008) şirket içi örnekleri için. Desteğini genişletmek için Azure VM'deki SQL Server Örneğinize geçirme veya şirket içinde kalmasını sağlamak için güvenlik güncelleştirmeleri Genişletilmiş satın alın. Daha fazla bilgi için [desteği SQL Server 2008 ve 2008 R2 Azure ile genişletme](virtual-machines-windows-sql-server-2008-eos-extend-support.md)

## <a name="what-are-the-primary-migration-methods"></a>Birincil geçiş yöntemlerinin nelerdir?
Birincil geçiş yöntemler şunlardır:

* Şirket içi yedekleme sıkıştırma kullanılarak ve yedekleme dosyasını Azure sanal makinesine el ile kopyalama
* URL'den yedekleme URL'si ve Azure sanal makine geri yükleme
* Ayırma ve ardından veri ve günlük dosyalarını Azure blob depolama alanına kopyalayın ve ardından Azure VM'de SQL Server için URL ekleyin.
* Şirket içi fiziksel makine için Hyper-V VHD dönüştürme, Azure Blob depolama alanına yükleyin ve ardından VHD'yi karşıya kullanarak yeni sanal makine olarak dağıtın
* Sabit sürücü Windows içeri/dışarı aktarma hizmetini kullanarak gönderin
* Varsa bir AlwaysOn Kullanılabilirlik grubu hem şirket içinde kullanın [Azure çoğaltması Ekleme Sihirbazı'nı](../sqlclassic/virtual-machines-windows-classic-sql-onprem-availability.md) kullanıcılar Azure ve sonra Yük devretme bir çoğaltma oluşturmak için Azure veritabanı örneğine işaret eden
* SQL Server'ı [işlemsel çoğaltma](https://msdn.microsoft.com/library/ms151176.aspx) kullanıcıların abone olarak Azure SQL Server örneğini yapılandırmak ve çoğaltma devre dışı bırakmak için Azure veritabanı örneğine işaret eden

> [!TIP]
> Bu aynı teknikleri, azure'da SQL Server Vm'leri arasında veritabanlarını taşımak için de kullanabilirsiniz. Örneğin, bir SQL Server galeri görüntüsü VM bir sürümüne/sürümünden yükseltmek için desteklenen hiçbir yolu yoktur. Bu durumda, yeni sürümü ile yeni bir SQL Server VM oluşturun ve ardından geçiş tekniklerden birini bu makaledeki veritabanlarınızı taşımak için kullanın gerekir. 

## <a name="choosing-your-migration-method"></a>Geçiş yönteminizi seçme
En iyi veri aktarımı performans için veritabanı dosyalarını sıkıştırılmış bir yedekleme dosyasını kullanarak Azure VM'de oturum geçirin.

Veritabanı geçiş işlemi sırasında kapalı kalma süresini en aza indirmek için AlwaysOn seçeneği veya işlemsel çoğaltma seçeneği'ni kullanın.

Yukarıdaki yöntemleri kullanmak mümkün değilse, veritabanınızı el ile geçirme. Bu yöntemi kullanarak, genellikle Azure'a Veritabanı yedeğinin bir kopyası tarafından izlenen bir veritabanı yedeği başlayın ve ardından bir veritabanı geri yükleme gerçekleştirin. Ayrıca veritabanı dosyalarını Azure'a kopyalayın ve ardından bunları ekleyin. Bir veritabanını Azure VM'deki geçirme el ile bu işlemi gerçekleştirmek çeşitli yöntemler vardır.

> [!NOTE]
> SQL Server 2014 veya SQL Server 2016 için SQL Server'ın önceki sürümlerinden yükselttiğinizde, değişikliklerin gerekli olup olmadığını göz önünde bulundurmalısınız. Geçiş projenizi bir parçası olarak yeni SQL Server sürümü tarafından desteklenmeyen özellikler tüm bağımlılıkları adres öneririz. Senaryolar ve desteklenen sürümleri hakkında daha fazla bilgi için bkz. [yükseltme için SQL Server](https://msdn.microsoft.com/library/bb677622.aspx).

Aşağıdaki tabloda birincil geçiş yöntemlerinin her birini listeler ve her yöntemin kullanılması en uygun olduğunda açıklanır.

| Yöntem | Kaynak veritabanı sürümü | Hedef veritabanı sürümü | Kaynak veritabanı yedekleme boyutu kısıtlaması | Notlar |
| --- | --- | --- | --- | --- |
| [Şirket içi yedekleme sıkıştırma kullanılarak ve yedekleme dosyasını Azure sanal makinesine el ile kopyalama](#backup-and-restore) |SQL Server 2005 veya üzeri |SQL Server 2005 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Makineler arasında veritabanlarını taşıma için basit ve iyi sınanmış bir yöntem budur. |
| [URL'den yedekleme URL'si ve Azure sanal makine geri yükleme](#backup-to-url-and-restore) |SQL Server 2012 SP1 CU2 veya üzeri |SQL Server 2012 SP1 CU2 veya üzeri |< 12,8 TB SQL Server 2016, aksi takdirde < 1 TB | Bu yöntem yedekleme dosyası Azure depolama kullanarak VM'ye taşımak için başka bir yoludur. |
| [Ayırma, Azure blob depolama alanına veri ve günlük dosyalarını kopyalayabilir ve URL'den'da Azure sanal makineler'de SQL Server'a iliştirin](#detach-and-attach-from-url) |SQL Server 2005 veya üzeri |SQL Server 2014 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |İçin planlama yaparken bu yöntemi kullanmak [Azure Blob Depolama hizmetini kullanarak bu dosyaları depolamak](https://msdn.microsoft.com/library/dn385720.aspx) ve özellikle büyük veritabanları ile bir Azure VM'de çalışan SQL Server ekleme |
| [Şirket içi makine için Hyper-V VHD dönüştürme, Azure Blob depolama alanına yükleyin ve ardından karşıya yüklenen VHD kullanarak yeni bir sanal makine dağıtma](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) |SQL Server 2005 veya üzeri |SQL Server 2005 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Şu durumlarda kullanın [kendi SQL Server lisansınızı getirmek](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md), SQL Server'ın daha eski bir sürümü üzerinde çalışacak bir veritabanı geçirilirken veya sistem ve kullanıcı veritabanlarını birlikte veritabanı diğerine bağımlı geçişi kapsamında geçişi sırasında kullanıcı veritabanları ve/veya sistem veritabanları. |
| [Sabit sürücü Windows içeri/dışarı aktarma hizmetini kullanarak gönderin](#ship-hard-drive) |SQL Server 2005 veya üzeri |SQL Server 2005 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Kullanım [Windows içeri/dışarı aktarma hizmeti](../../../storage/common/storage-import-export-service.md) el ile kopyalama yöntemi olduğunda çok büyük veritabanları ile çok yavaş olduğu gibi |
| [Kullanım Azure çoğaltma Sihirbazı Ekle](../sqlclassic/virtual-machines-windows-classic-sql-onprem-availability.md) |SQL Server 2012 veya daha büyük |SQL Server 2012 veya daha büyük |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Always On şirket içi dağıtımına sahip olduğunuzda kullanın kapalı kalma süresi en aza indirir |
| [SQL Server işlem çoğaltma kullanma](https://msdn.microsoft.com/library/ms151176.aspx) |SQL Server 2005 veya üzeri |SQL Server 2005 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Kapalı kalma süresini en aza indirmek, ihtiyacınız olduğunda kullanın ve Always On şirket içi dağıtımına sahip değilsiniz |

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme
Veritabanınız ile sıkıştırma, yedekleme VM yedekleme kopyalayın ve sonra veritabanını geri yükleyin. Yedekleme dosyanız 1 TB'den büyükse, sanal makine diskini en büyük boyutu 1 TB olduğundan, stripe gerekir. El ile bu yöntemi kullanarak bir kullanıcı veritabanına geçirmek için aşağıdaki genel adımları kullanın:

1. Bir şirket içi konumuna bir veritabanının tam yedeklemesini gerçekleştirin.
2. Oluşturun veya SQL Server'ın gerekli sürümü ile bir sanal makinenizi karşıya yükleyebilirsiniz.
3. Gereksinimlerinize göre bağlantı kurun. Bkz: [(Resource Manager) azure'da bir SQL Server sanal makinesine bağlanma](virtual-machines-windows-sql-connect.md).
4. Sanal makinenize Uzak Masaüstü, Windows Gezgini veya bir komut isteminden kopyalama komutu kullanarak, yedekleme dosyaları kopyalayın.

## <a name="backup-to-url-and-restore"></a>Yedekleme için URL ve geri yükleme
Yerel bir dosyaya yedekleme yerine kullanabileceğiniz [URL'ye yedekleme](https://msdn.microsoft.com/library/dn435916.aspx) ve URL'den VM'ye geri yükleyin. SQL Server 2016 ile şeritli yedekleme kümeleri desteklenir, performans için önerilen ve blob başına boyutu sınırları aşmanız gerekli. Çok büyük veritabanları, kullanımı için [Windows içeri/dışarı aktarma hizmeti](../../../storage/common/storage-import-export-service.md) önerilir.

## <a name="detach-and-attach-from-url"></a>URL'den ekleme ve ayırma
Veritabanı ve günlük dosyalarınızı ayırma ve bunlara aktarım [Azure Blob Depolama](https://msdn.microsoft.com/library/dn385720.aspx). Ardından Azure vm'nizdeki URL'den veritabanı ekleyin. Fiziksel veritabanı dosyaları Blob Depolama alanında bulunan isterseniz bunu kullanın. Bu, çok büyük veritabanları için yararlı olabilir. El ile bu yöntemi kullanarak bir kullanıcı veritabanına geçirmek için aşağıdaki genel adımları kullanın:

1. Şirket içi veritabanı örneği veritabanı dosyalarından çıkarın.
2. Azure blob depolama kullanarak ayrılmış veritabanı dosyalarını kopyalamak [AZCopy komut satırı yardımcı programını](../../../storage/common/storage-use-azcopy.md).
3. Veritabanı dosyaları, Azure VM'deki SQL Server örneğine Azure URL'den ekleyin.

## <a name="convert-to-vm-and-upload-to-url-and-deploy-as-new-vm"></a>VM'ye dönüştürüp URL'ye yükleme ve yeni VM olarak dağıtma
Azure sanal makinesi için bir şirket içi SQL Server örneğindeki tüm sistem ve kullanıcı veritabanlarını geçirmek için bu yöntemi kullanın. El ile bu yöntemi kullanarak bir SQL Server örneğinin geçirmek için aşağıdaki genel adımları kullanın:

1. Fiziksel veya sanal makine için Hyper-V VHD Dönüştür.
2. Kullanarak VHD dosyalarını Azure Depolama'ya yükleme [Add-AzureVHD cmdlet'i](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx).
3. Karşıya yüklenen VHD kullanarak yeni bir sanal makine dağıtın.

> [!NOTE]
> Tüm uygulama geçirmek için kullanmayı [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md)].

## <a name="ship-hard-drive"></a>Sabit sürücü gönderin
Kullanım [Windows içeri/dışarı aktarma hizmeti yöntemi](../../../storage/common/storage-import-export-service.md) büyük miktarlardaki dosya verilerini Azure Blob Depolama ağ üzerinden karşıya olduğu fazla vakit pahalı veya uygun olmayan durumda aktarmak için. Bu hizmet ile verilerinizi depolama hesabınıza burada karşıya yüklenecek bir Azure veri merkezi bu verileri içeren bir veya daha fazla sabit sürücüler gönderin.

## <a name="next-steps"></a>Sonraki adımlar
Azure sanal Makineler'de SQL Server çalıştırma hakkında daha fazla bilgi için bkz. [SQL Server Azure sanal makinelerine genel bakış](virtual-machines-windows-sql-server-iaas-overview.md).

> [!TIP]
> SQL Server sanal makineleri hakkında sorularınız olursa [Sık Sorulan Sorular](virtual-machines-windows-sql-server-iaas-faq.md) bölümüne bakın.

Yakalanan görüntüden bir Azure SQL Server sanal makinesi oluşturma ile ilgili yönergeler için bkz: [ipuçları ve püf noktaları 'yakalanan görüntülerin Azure SQL sanal makineleri kopyalama'](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) CSS SQL Server mühendisleri blogundaki.

