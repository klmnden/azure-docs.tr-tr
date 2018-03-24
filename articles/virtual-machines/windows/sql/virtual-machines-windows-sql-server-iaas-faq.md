---
title: SQL Server'da Windows Azure sanal makineler hakkında SSS | Microsoft Docs
description: Bu makale Azure Vm'lerinde SQL Server çalıştıran hakkında sık sorulan soruların yanıtlarını sağlar.
services: virtual-machines-windows
documentationcenter: ''
author: v-shysun
manager: felixwu
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/20/2018
ms.author: v-shysun
ms.openlocfilehash: 42a82a59d0cf786e80b93f124cbe04007b2a4704
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="frequently-asked-questions-for-sql-server-on-windows-azure-virtual-machines"></a>Sık Sorulan Sorular SQL Server için Windows Azure sanal makinelerde

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](../../linux/sql/sql-server-linux-faq.md)

Bu makalede çalıştırma hakkında en yaygın sorulara yanıtlar sağlayan [SQL Server Windows Azure sanal makineler üzerinde](https://azure.microsoft.com/services/virtual-machines/sql-server/).

> [!NOTE]
> Bu makalede Windows Vm'lerde SQL Server için belirli sorunları odaklanır. SQL Server Linux VM'ler üzerinde çalıştırıyorsanız, bkz: [Linux SSS](../../linux/sql/sql-server-linux-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a id="images"></a> Görüntüleri

1. **Hangi SQL Server sanal makineye Galerisi görüntüleri var mı?**

   Azure, Windows ve Linux için tüm sürümleri üzerinde SQL Server'ın önde gelen tüm desteklenen sürümleri için sanal makine görüntülerini korur. Daha fazla ayrıntı için tam listesi için bkz [Windows VM görüntüleri](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo) ve [Linux VM görüntüleri](../../linux/sql/sql-server-linux-virtual-machines-overview.md#create).

1. **Var olan SQL Server sanal makineye Galerisi görüntüleri güncelleştirilmiş mi?**

   Her iki ay sanal makine Galerisi SQL Server görüntülerinde en son Windows ile güncelleştirilir ve Linux güncelleştirir. Windows görüntüleri için bu önemli SQL Server güvenlik güncelleştirmeleri ve hizmet paketleri de dahil olmak üzere Windows Update önemli işaretlenen herhangi bir güncelleştirme içerir. Linux görüntüleri için bu en son sistem güncelleştirmeleri içerir. SQL Server toplu güncelleştirmeler, Linux ve Windows için farklı şekilde ele alınır. Linux için SQL Server toplu güncelleştirmeleri de yenileme dahil edilir. Ancak şu anda SQL Server veya Windows Server birikmeli güncelleştirmeleri içeren Windows VM'ler güncelleştirilmez.

1. **Galeriden bir SQL Server sanal makine görüntülerini kaldırılmaları?**

   Evet. Azure yalnızca bir görüntüsü başına ana sürümü korur. Örneğin, yeni bir SQL Server hizmet paketi yayımlandığında, Azure hizmet paketinin Galerisi'ne yeni bir görüntü ekler. SQL Server görüntüsü önceki hizmet paketi için hemen Azure portalından kaldırıldı. Ancak, sonraki üç ay için Powershell'den sağlamak için kullanılabilir durumda kalır. Üç ay sonra önceki hizmet paketi resmi artık kullanılamıyor. Kendi yaşam döngüsünün sonuna ulaştığında bir SQL Server sürümü desteklenmeyen hale gelirse bu kaldırma İlkesi de geçerli olur.

1. **Bir SQL Server sanal makineden bir VHD görüntüsü oluşturabilir miyim?**

   Evet, ancak orada birkaç noktalardır. Bu VHD azure'da yeni bir VM dağıtırsanız, portal SQL Server yapılandırma bölümünde değil ge yapın. Ardından, SQL Server yapılandırma seçenekleri PowerShell aracılığıyla yönetmeniz gerekir. Ayrıca, görüntünüzü başlangıçta temel SQL VM hızında ücretlendirilir. SQL Server VHD'den dağıtmadan önce kaldırsanız bile bu geçerlidir. 

1. **Sanal makineye Galerisi (örneğin Windows 2008 R2 + SQL Server 2012) içinde gösterilmez yapılandırmaları ayarlamak mümkün mü?**

   Hayır. SQL Server içeren sanal makineye Galerisi görüntüler için sağlanan görüntülerden birini seçmeniz gerekir.

## <a name="creation"></a>Oluşturma

1. **SQL Server ile nasıl bir Azure sanal makinesi oluşturulur?**

   SQL Server içeren bir sanal makine oluşturmak için en kolay çözüm olur. Azure'a kaydolma ve Portalı'ndan bir SQL VM oluşturma bir öğretici için bkz: [Azure portalında bir SQL Server sanal makine sağlama](virtual-machines-windows-portal-sql-server-provision.md). Saniye başına ödeme SQL Server Lisans kullanan bir sanal makine görüntüsünü seçebilir veya kendi SQL Server lisansınızı getirebilir olanak tanıyan bir görüntü kullanabilirsiniz. El ile SQL Server üzerinde bir VM ile ya da serbestçe lisanslı bir sürüm (Geliştirici veya Express) yüklemek veya bir şirket içi lisans yeniden kullanma seçeneğiniz de vardır. Kendi lisansını Getir seçerseniz bulunmalıdır [azure'da Yazılım Güvencesi ile lisans taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility/). Daha fazla bilgi için bkz. [SQL Server Azure VM’leri için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Nasıl şirket içi SQL Server veritabanını buluta geçişini sağlayabilir miyim?**

   İlk Azure sanal makinesi ile bir SQL Server örneği oluşturun. Ardından, şirket içi veritabanlarını bu örneğe geçirin. Veri geçiş stratejileri için bkz: [Azure VM'deki SQL Server için SQL Server veritabanını geçirme](virtual-machines-windows-migrate-sql.md).

## <a name="licensing"></a>Lisanslama

1. **Azure VM temelinde my lisanslı SQL Server kopyası nasıl yükleyebilirim?**

   Bunu yapmanın iki yolu vardır. Aşağıdakilerden birini sağlayabilirsiniz [lisansları destekleyen sanal makine görüntülerini](virtual-machines-windows-sql-server-iaas-overview.md#BYOL). Windows Server VM için SQL Server yükleme medyasını kopyalayın ve ardından SQL Server VM yüklemeniz ise başka bir seçenektir. Nedenlerden lisans için sahip olmanız gerekir [azure'da Yazılım Güvencesi ile lisans taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility/). Daha fazla bilgi için bkz. [SQL Server Azure VM’leri için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Kullandıkça Öde galeri görüntülerden birini oluşturulduysa, kendi SQL Server lisansınızı kullanmak için bir VM değiştirebilir miyim?**

   Hayır. Kendi lisansınızı kullanmak için saniye başına ödeme lisans geçemezsiniz. Yeni Azure birini kullanarak sanal makine oluşturma [KLG görüntüleri](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), ve veritabanlarınızı standart kullanarak yeni sunucusuna geçirmenize [veri geçiş teknikleri](virtual-machines-windows-migrate-sql.md).

1. **Azure VM'de SQL Server Lisans için yalnızca bekleme/yük devretme için kullanılıyorsa ödeme var mı?**

   Yazılım Güvencesi ve lisans taşınabilirliği açıklandığı gibi kullanabilirsiniz, [sanal makine Lisansı hakkında SSS](http://azure.microsoft.com/pricing/licensing-faq/) HA dağıtımında Pasif bir ikincil çoğaltma olarak katılan bir SQL Server Lisans ödeme sahip değil. Aksi takdirde, lisans ödeme gerekir.


## <a name="administration"></a>Yönetim

1. **Aynı VM'de SQL Server ikinci bir örneğini yükleyebilir miyim? Varsayılan örnek yüklü özelliklerini değiştirebilir miyim?**

   Evet. SQL Server yükleme ortamının bir klasörde bulunan **C** sürücü. Çalıştırma **Setup.exe** yeni SQL Server örnekleri eklemek için bu konuma veya bu makinede SQL Server'ın yüklü değişiklik diğer özellikleri. Otomatik yedekleme, otomatik düzeltme eki uygulama ve Azure anahtar kasası tümleştirmeyi gibi bazı özellikler yalnızca varsayılan örnek karşı çalıştığını unutmayın.

1. **SQL Server'ın varsayılan örneğinin kaldırabilir miyim?**

   Evet, ancak var. bazı noktalardır. Önceki yanıtında belirtildiği gibi özellikler, Bel [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md) yalnızca varsayılan örneğinde çalışır. Varsayılan örnek kaldırırsanız, uzantısı aramanız olmaya devam eder ve olay günlüğünü hatalara neden. Bu hatalar aşağıdaki iki kaynaktan kaynaklanır: **Microsoft SQL Server kimlik bilgileri yönetimi** ve **Microsoft SQL Server Iaas Aracısı**. Hatalardan biri, aşağıdakine benzer olabilir:

      SQL Server bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi.

   Varsayılan örnek kaldırmaya karar verirseniz ayrıca kaldırma [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md) de.

1. **Bir SQL VM SQL Server tamamen kaldırabilirim?**

   Evet, ancak açıklandığı gibi SQL VM için uygulanacak sürdürecektir [Kılavuzu SQL Server Azure VM'ler için fiyatlandırma](virtual-machines-windows-sql-server-pricing-guidance.md). SQL Server artık ihtiyacınız varsa, yeni bir sanal makine dağıtın ve veri ve uygulamaları yeni bir sanal makine geçirin. Ardından, SQL Server sanal makine kaldırabilirsiniz.
   
## <a name="updating-and-patching"></a>Güncelleştirme ve düzeltme eki uygulama

1. **Bir Azure VM'de SQL Server'ın yeni bir sürümü/yayını için nasıl yükseltme?**

   Şu anda hiçbir yerinde yükseltme bir Azure VM içinde çalışan SQL Server için yoktur. İstenen SQL Server sürümü/yayını ile yeni bir Azure sanal makine oluşturma ve ardından veritabanlarınızı standart kullanarak yeni sunucuya geçiş [veri geçiş teknikleri](virtual-machines-windows-migrate-sql.md).

1. **Nasıl güncelleştirmeleri ve hizmet paketleri bir SQL Server VM üzerinde uygulanır?**

   Ne zaman ve nasıl güncelleştirmeleri uygulamak gibi konak makinesi üzerinde denetim sanal makineleri sağlar. İşletim sistemi için windows güncelleştirmelerini el ile uygulayabilirsiniz veya adı verilen bir zamanlama hizmeti etkinleştirebilirsiniz [otomatik düzeltme eki uygulama](virtual-machines-windows-sql-automated-patching.md). Otomatik Düzeltme Eki Uygulama, önemli olarak işaretlenmiş tüm güncelleştirmeleri, bu kategorideki SQL Server güncelleştirmeleri de dahil olmak üzere yükler. SQL Server için diğer isteğe bağlı güncelleştirmeler el ile yüklenmelidir.

## <a name="general"></a>Genel

1. **Azure Vm'lerinde SQL Server Yük devretme kümesi örneği (FCI) destekleniyor mu?**

   Evet. Yapabilecekleriniz [Windows Server 2016 Windows Yük devretme kümesi oluşturma](virtual-machines-windows-portal-sql-create-failover-cluster.md) ve küme depolaması için depolama alanları doğrudan (S2D) kullanın. Alternatif olarak, açıklandığı gibi üçüncü taraf kümeleme veya depolama çözümleri kullanabilirsiniz [Azure Virtual Machines'de SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions).

1. **SQL veritabanı hizmetinin SQL VMs arasındaki fark nedir?**

   Kavramsal olarak, SQL Server olmayan bir uzak veri merkezinde SQL Server çalıştıran farklı olan bir Azure sanal makine üzerinde çalışan. Buna karşılık, [SQL veritabanı](../../../sql-database/sql-database-technical-overview.md) hizmet olarak veritabanı sunar. SQL veritabanı ile veritabanlarınızı barındırmak makinelere erişimi sahip değil. Tam bir karşılaştırması için bkz: [bir bulut SQL Server seçeneği seçin: Azure SQL (PaaS) veritabanı veya SQL Server (Iaas) Azure vm'lerinde](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **My Azure VM nasıl SQL Data araçları yükleme?**

    SQL veri Araçları'ndan yükleyip [Microsoft SQL Server veri araçları - Visual Studio 2013 iş zekası](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Kaynaklar

**Windows sanal makineleri**:

* [Bir Windows VM üzerinde SQL Server'ın genel bakış](virtual-machines-windows-sql-server-iaas-overview.md).
* [SQL Server, Windows VM sağlama](virtual-machines-windows-portal-sql-server-provision.md)
* [SQL Server Azure VM'de bir veritabanını geçirme](virtual-machines-windows-migrate-sql.md)
* [Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için Azure sanal makinelerde SQL Server](virtual-machines-windows-sql-high-availability-dr.md)
* [Azure Sanal Makineler’de SQL Server için performansa yönelik en iyi uygulamalar](virtual-machines-windows-sql-performance.md)
* [Azure Sanal Makineler'de SQL Server için Uygulama Desenleri ve Geliştirme Stratejileri](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Linux VM'ler**:

* [Bir Linux VM üzerinde SQL Server'ın genel bakış](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Bir SQL Server Linux VM sağlama](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [SSS (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [SQL Server'da Linux belgeleri](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
