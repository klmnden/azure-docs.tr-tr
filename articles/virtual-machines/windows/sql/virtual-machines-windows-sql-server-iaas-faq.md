---
title: SQL Server Windows sanal makinelerinde Azure SSS | Microsoft Docs
description: Bu makalede, Azure Vm'lerinde SQL Server çalıştırma hakkında sık sorulan soruların yanıtlarını sağlar.
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
ms.date: 07/12/2018
ms.author: v-shysun
ms.openlocfilehash: f308b814da06598b95337708f7a8c84d506eed78
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57781808"
---
# <a name="frequently-asked-questions-for-sql-server-running-on-windows-virtual-machines-in-azure"></a>Azure'da Windows sanal makineler üzerinde çalışan SQL Server için sık sorulan sorular

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](../../linux/sql/sql-server-linux-faq.md)

Bu makalede çalıştırma hakkında en yaygın soruların yanıtları sağlanır [azure'da Windows sanal makinelerinde SQL Server'a](https://azure.microsoft.com/services/virtual-machines/sql-server/).

> [!NOTE]
> Bu makalede Windows sanal makinelerinde SQL Server'a özgü sorunları ele alınmaktadır. Linux VM üzerinde SQL Server çalıştırıyorsanız bkz [Linux SSS](../../linux/sql/sql-server-linux-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a id="images"></a> Görüntüleri

1. **Hangi SQL Server sanal makine galeri görüntüleri kullanılabilir mi?**

   Azure, hem Windows hem de Linux için sanal makine görüntüleri için tüm sürümleri üzerinde SQL Server'ın tüm desteklenen ana sürümler tutar. Daha fazla bilgi için tam listesi görmek [Windows VM görüntüleri](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo) ve [Linux VM görüntüleri](../../linux/sql/sql-server-linux-virtual-machines-overview.md#create).

1. **Mevcut SQL Server sanal makine galeri görüntüleri güncelleştirilir mi?**

   Her iki ayda, SQL Server görüntüleri sanal makine galerisindeki en son Windows ile güncelleştirilir ve Linux güncelleştirir. Windows görüntüleri için bu Windows güncelleştirme önemli SQL Server güvenlik güncelleştirmeleri ve hizmet paketleri dahil olmak üzere, önemli olarak işaretlenmiş tüm güncelleştirmeleri içerir. Linux görüntüleri için bu sistem güncelleştirmeleri içerir. SQL Server toplu güncelleştirmeler, Linux ve Windows için farklı şekilde ele alınır. Linux için SQL Server toplu güncelleştirmeleri ayrıca yenileme dahil edilir. Ancak şu anda SQL Server veya Windows Server toplu güncelleştirmeleri ile Windows Vm'leri güncelleştirilmez.

1. **Galeriden bir SQL Server sanal makine görüntüleri kaldırılmaları?**

   Evet. Azure, yalnızca tek bir görüntü ana sürüm ve sürüm bazında tutar. Örneğin, yeni bir SQL Server hizmet paketi yayımlandığında, Azure galeri için hizmet paketi için yeni bir görüntü ekler. SQL Server görüntüsü önceki hizmet paketi için hemen Azure portalından kaldırılır. Ancak, sonraki üç ay için Powershell'den sağlamak için kullanılabilir durumda kalır. Üç ay sonra önceki hizmet paketi görüntü artık kullanılamıyor. Yaşam döngüsü sonuna ulaştığında bir SQL Server sürümü desteklenmeyen hale gelirse, bu kaldırma İlkesi de geçerli.


1. **Bu, Azure portalında görünür olmayan bir SQL Server'ın daha eski bir görüntüsünü dağıtmak mümkün mü?**

   Evet, PowerShell kullanarak. PowerShell kullanarak SQL Server Vm'leri dağıtma hakkında daha fazla bilgi için bkz. [Azure PowerShell ile SQL Server sanal makineler sağlamak nasıl](virtual-machines-windows-ps-sql-create.md).

1. **Bir SQL Server sanal makinesinden bir VHD görüntüsü oluşturabilir miyim?**

   Evet, ancak burada birkaç faktörlerdir. Bu VHD azure'da yeni bir VM dağıtırsanız, portal SQL Server yapılandırma bölümünde değil ge yapın. Ardından, PowerShell aracılığıyla SQL Server yapılandırma seçenekleri de yönetmeniz gerekir. Ayrıca, görüntünüzü ilk temel SQL VM oranı üzerinden ücretlendirilirsiniz. Dağıtmadan önce SQL Server VHD'den kaldırılsa bile bu geçerlidir. 

1. **(Örneğin Windows 2008 R2 + SQL Server 2012) için sanal makine galerisindeki gösterilmeyen yapılandırmaları ayarlamak mümkündür?**

   Hayır. SQL Server içeren sanal makine galeri görüntüleri için Azure portalı üzerinden ya da yoluyla sağlanan görüntülerden birini seçmelisiniz [PowerShell](virtual-machines-windows-ps-sql-create.md). 


## <a name="creation"></a>Oluşturma

1. **Azure sanal makine'de SQL Server ile nasıl oluşturabilirim?**

   SQL Server içeren bir sanal makine oluşturmak için en kolay çözümdür bakın. Azure'a kaydolma ve Portalı'ndan bir SQL VM oluşturmaya ilişkin öğretici için bkz: [Azure Portal'da bir SQL Server sanal makinesi sağlama](virtual-machines-windows-portal-sql-server-provision.md). Saniye başına ödeme SQL Server Lisans kullanan bir sanal makine görüntüsünü seçebilir veya kendi SQL Server lisansınızı getirebilirsiniz olanak tanıyan bir görüntü kullanabilirsiniz. Ayrıca, el ile SQL Server üzerinde bir VM ile ücretsiz lisanslı bir sürüme (Geliştirici veya Express) yükleme ya da şirket içi lisans yeniden kullanma seçeneğiniz de vardır. Kendi lisansınızı getirirseniz, olmalıdır [azure'de Yazılım Güvencesiyle lisans taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility/). Daha fazla bilgi için bkz. [SQL Server Azure VM’leri için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Şirket içi SQL Server veritabanımı buluta nasıl geçirebilirim?**

   İlk Azure sanal makine'de SQL Server örneği ile oluşturun. Ardından, şirket içi veritabanlarını bu örneğe geçirin. Veri geçiş stratejileri için bkz. [bir SQL Server veritabanını Azure VM'deki SQL Server'a geçirme](virtual-machines-windows-migrate-sql.md).

## <a name="licensing"></a>Lisanslama

1. **Bir Azure sanal makinesinde SQL Server lisanslı kopyamı nasıl yükleyebilirim?**

   Bunu yapmanın iki yolu vardır. Birini sağlayabileceğiniz [lisansları destekleyen bir sanal makine görüntüleri](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), getirin-kendi lisansını (KLG) da bilinir. Başka bir seçenek, Windows Server VM için SQL Server yükleme medyası kopyalayın ve ardından sanal makinede SQL Server'ı yükleyin oluşturmaktır. Ancak, SQL Server'ı el ile yükleyin, portal bir tümleştirme yoktur ve SQL Server Iaas Aracısı uzantısı desteklenmiyor, bu nedenle otomatik yedekleme ve otomatik düzeltme eki uygulama gibi özellikleri bu senaryoda çalışmaz. Bu nedenle, KLG galeri görüntülerden birini kullanmanızı öneririz. KLG veya kendi SQL Server medya, bir Azure sanal makinesinde kullanmak için olmalıdır [azure'de Yazılım Güvencesiyle lisans taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility/). Daha fazla bilgi için bkz. [SQL Server Azure VM’leri için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md).


1. **Yalnızca bekleme/yük devretme için kullanılıyorsa, Azure VM'deki SQL Server lisansı için ödeme gerekir mi?**

   Yazılım Güvencesi ve lisans taşınabilirliği açıklandığı kullanın [sanal makine lisanslama SSS](https://azure.microsoft.com/pricing/licensing-faq/), HA dağıtımında pasif ikincil bir çoğaltma olarak katılan tek bir SQL Server Lisansı ödeme yapmam gerekir mi sonra. Aksi takdirde, lisans için ücret ödemem gerekir.

1. **Kullandıkça Öde galeri görüntülerden birini oluşturulmuşsa, kendi SQL Server Lisansımı kullanmak için bir VM değiştirebilirim?**

   Evet. Kolayca taşıyabilirsiniz ilk olarak bir Kullandıkça Öde galeri görüntüsü ile başladıysanız iki lisanslama modelleri arasında taşıyın. Ancak, başlangıçta bir KLG görüntüsü ile başladıysanız PAYG için kendi lisansınızı geçmek mümkün olmayacaktır. Daha fazla bilgi için [SQL Server sanal makinesi için lisanslama modelini değiştirme](virtual-machines-windows-sql-ahb.md).

   > [!Note]
   > Şu anda bu özellik yalnızca genel bulut müşterileri için kullanılabilir.

1. **Yeni SQL VM oluşturmak için KLG görüntüleri ya da SQL VM RP kullanmalı mıyım?**

   -Kendi-lisansını getir (KLG) görüntüleri, yalnızca EA müşterileri tarafından kullanılabilir. Diğer Yazılım Güvencesine sahip müşteriler SQL VM kaynak sağlayıcısı ile bir SQL VM oluşturmak için kullanması gereken [Azure hibrit Avantajı'nı (AHB)](https://azure.microsoft.com/pricing/licensing-faq/). 

1. **Geçiş lisanslama modelleri, SQL Server için kapalı kalma süresi gerekiyor mu?**

   Hayır. [Lisanslama modelini değiştirme](virtual-machines-windows-sql-ahb.md) kapalı kalma süresi için SQL Server gerektirmez, bu değişiklik hemen etkili olur ve VM yeniden başlatma gerektirmez. Ancak, SQL Server VM'nize SQL VM kaynak sağlayıcısı ile kaydolmak için [SQL Iaas uzantısı](virtual-machines-windows-sql-server-agent-extension.md) önkoşuldur ve SQL Iaas uzantısı yükleme SQL Server hizmetini yeniden başlatır. SQL Iaas uzantısı yüklü olması gerekir, bu nedenle, ardından da bir bakım penceresi sırasında yapılması gerekir. 

1. **CSP aboneliklerinde Azure hibrit avantajı etkinleştirebilir miyim?**

   Evet, Azure hibrit avantajı için CSP aboneliklerinde kullanılabilir. CSP müşterileri bir Kullandıkça Öde görüntüsü önce dağıtmanız ve ardından [lisanslama modelini değiştirme](virtual-machines-windows-sql-ahb.md) getirin-kendi lisansını için.  

1. **Sanal Makinem ile yeni SQL VM kaynak sağlayıcısı kaydetme ek maliyet getirir?**

   Hayır. SQL VM kaynak sağlayıcısı yalnızca ek yönetilebilirlik Azure vm'lerde SQL Server için herhangi bir ek ücret ile sağlar. 

1. **SQL VM kaynak sağlayıcısı, tüm müşteriler için kullanılabilir mi?**
 
   Evet. Tüm müşteriler yeni SQL VM kaynak sağlayıcısı ile kaydolmak olanağına sahip olursunuz. Ancak, yalnızca Müşteriler Yazılım Güvencesi avantajı ile etkinleştirebilir [Azure hibrit Avantajı'nı (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) (veya KLG) SQL Server VM üzerinde. 

1. **_ İçin ne*VM kaynağı taşıdıysanız veya bırakılan Microsoft.SqlVirtualMachine_* kaynak?** 

   Ne zaman Microsoft.Compute/VirtualMachine kaynak bırakılmış veya ilişkili Microsoft.SqlVirtualMachine kaynak işlemi zaman uyumsuz olarak çoğaltmak için bildirim sonra taşındı.

1. **VM, ne _* Microsoft.SqlVirtualMachine_* kaynak bırakıldı?**

   Microsoft.SqlVirtualMachine kaynak bırakıldığında Microsoft.Compute/VirtualMachine kaynak etkilenmez. Ancak, lisans değişikliklerinin özgün görüntü kaynağı için varsayılan olarak kullanılır. 

1. **SQL VM kaynak sağlayıcısı ile şirket içinde dağıtılmış SQL Server Vm'leri kaydetmek mümkündür?**

   Evet. SQL Server'ı kendi medyadan dağıtılan ve SQL Iaas uzantısı yüklü değilse, SQL Server VM'nize SQL Iaas uzantısı tarafından sağlanan yönetilebilirlik avantajlarından yararlanabilmek için kaynak sağlayıcısı ile kaydedebilirsiniz. Ancak, şirket içinde dağıtılan bir SQL VM Kullandıkça Öde aboneliğine dönüştürülemiyor.

## <a name="administration"></a>Yönetim

1. **Aynı VM'de SQL Server'ın ikinci bir örneğini yükleyebilir miyim? Varsayılan örneği yüklü özelliklerini değiştirebilir miyim?**

   Evet. SQL Server yükleme medyası üzerinde bir klasörde bulunan **C** sürücü. Çalıştırma **Setup.exe** yeni SQL Server örneklerini eklemek için bu konuma veya makine üzerinde SQL Server'ın yüklü değişiklik diğer özellikleri. Otomatik yedekleme, otomatik düzeltme eki uygulama ve Azure anahtar kasası tümleştirmeyi, gibi bazı özellikler, yalnızca varsayılan örneğine karşı çalışan veya adlandırılmış bir örnek Not düzgün şekilde yapılandırıldı (soru 3 bakın). 

1. **SQL Server varsayılan örneğini kaldırabilirim?**

   Evet, ancak burada noktalardır. Önceki yanıt belirtildiği gibi kullanan özellikler vardır [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).  Varsayılan örnek Iaas uzantısı da kaldırmadan kaldırırsanız, uzantı için görünür olmaya devam eder ve olay günlüğünü hatalara neden. Bu hatalar, aşağıdaki iki kaynaktan kaynaklanır: **Microsoft SQL Server kimlik bilgileri yönetimi** ve **Microsoft SQL Server Iaas Aracısı**. Hatalardan biri, aşağıdakine benzer olabilir:

      Bir SQL Server bağlantısı kurulurken ağla ilgili veya örneğe özel bir hata oluştu. Sunucu bulunamadı veya erişilebilir durumda değildi.

   Varsayılan örnek kaldırmaya karar verirseniz, ayrıca kaldırma [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md) de.

1. **Iaas uzantısı ile SQL Server'ın adlandırılmış bir örnek kullanabilirsiniz**?
   
   Evet, adlandırılmış örnek yalnızca SQL Server örneğiyse ve özgün varsayılan örnek düzgün bir şekilde kaldırıldı. Adlandırılmış bir örnek kullanmak için aşağıdakileri yapın:
    1. Market'ten bir SQL Server VM dağıtın. 
    1. Iaas uzantıyı kaldırın.
    1. SQL Server'ı tamamen kaldırın.
    1. SQL Server adlandırılmış bir örnekle yükleyin. 
    1. Iaas uzantıyı yükleyin. 

1. **Bir SQL VM üzerindeki SQL Server tamamen kaldırabilirim?**

   Evet, ancak bölümünde anlatıldığı gibi SQL sanal Makineniz için ödeme yapmaya devam edecek [SQL Server Azure Vm'leri için fiyatlandırma Kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md). SQL Server artık ihtiyacınız yoksa, yeni bir sanal makine dağıtma ve veri ve uygulamaları yeni sanal makineye geçirin. Ardından, SQL Server sanal makineyi kaldırabilirsiniz.
   
## <a name="updating-and-patching"></a>Güncelleştirme ve düzeltme eki uygulama

1. **Azure VM'de SQL Server'ın yeni bir sürümü/yayını için nasıl değiştirebilirim?**

   Yazılım Güvencesine sahip müşteriler yerinde yükseltme toplu lisans portalında yükleme medyasını kullanarak bir Azure sanal makinesinde çalışan kendi SQL Server olanağına sahip olursunuz. Ancak, şu anda, SQL Server örneği sürümünü değiştirmek için hiçbir yolu yoktur. İstenen SQL Server sürümü ile yeni bir Azure sanal makine oluşturma ve veritabanlarınızı yeni sunucuya standardını kullanarak geçirmenize [veri taşıma tekniklerini](virtual-machines-windows-migrate-sql.md).

1. **Güncelleştirmelerin ve hizmet paketlerinin nasıl bir SQL Server sanal makinesinde uygulanır?**

   Sanal makineler güncelleştirmeleri ne zaman ve nasıl uygulayacağınız dahil olmak üzere ana makine üzerinde denetim sağlar. İşletim sistemi için windows güncelleştirmelerini el ile uygulayabilirsiniz veya adlı bir zamanlama hizmetini etkinleştirebilirsiniz [otomatik düzeltme eki uygulama](virtual-machines-windows-sql-automated-patching.md). Otomatik Düzeltme Eki Uygulama, önemli olarak işaretlenmiş tüm güncelleştirmeleri, bu kategorideki SQL Server güncelleştirmeleri de dahil olmak üzere yükler. SQL Server için diğer isteğe bağlı güncelleştirmeler el ile yüklenmelidir.

## <a name="general"></a>Genel

1. **Azure Vm'lerinde SQL Server Yük devretme kümesi örneği (FCI) destekleniyor?**

   Evet. Yapabilecekleriniz [Windows Server 2016'da bir Windows Yük devretme kümesi oluşturma](virtual-machines-windows-portal-sql-create-failover-cluster.md) ve depolama alanları doğrudan (S2D) kümesi depolaması için kullanabilirsiniz. Alternatif olarak, açıklandığı gibi üçüncü taraf kümeleme veya depolama çözümleri kullanabilirsiniz [Azure sanal Makineler'de SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions).

   > [!IMPORTANT]
   > Şu anda [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md) azure'da SQL Server FCI için desteklenmiyor. Uzantı FCI katılan vm'lerden kaldırmanızı öneririz. Bu uzantıyı otomatik yedekleme ve düzeltme eki uygulama ve SQL için portal bazı özellikleri gibi özellikleri destekler. Aracı kaldırıldıktan sonra bu özellikler, SQL VM'ler için çalışmaz.

1. **SQL veritabanı hizmeti SQL VM'ler arasındaki fark nedir?**

   Kavramsal olarak, SQL Server değil, SQL Server'ı uzak bir veri merkezinde çalışan farklı bir Azure sanal makinesi üzerinde çalışıyor. Buna karşılık, [SQL veritabanı](../../../sql-database/sql-database-technical-overview.md) hizmet olarak veritabanı sağlar. SQL veritabanı ile veritabanlarınızı barındıran makinelerin erişiminiz yok. Tam bir karşılaştırması için bkz. [bir bulut SQL Server seçeneği seçin: Azure SQL (PaaS) veritabanı ya da (Iaas) Azure vm'lerinde SQL Server](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **SQL veri araçları Azure VM'deki uygulamalarımdan birine nasıl yüklerim?**

    SQL veri Araçları'ndan yükleyip [Microsoft SQL Server veri araçları - Visual Studio 2013 için iş zekası](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Kaynaklar

**Windows Vm'leri**:

* [Bir Windows VM üzerinde SQL Server'a genel bakış](virtual-machines-windows-sql-server-iaas-overview.md).
* [Bir SQL Server Windows VM sağlama](virtual-machines-windows-portal-sql-server-provision.md)
* [Bir veritabanını Azure VM'deki SQL Server'a geçirme](virtual-machines-windows-migrate-sql.md)
* [Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için Azure sanal makineler'de SQL Server](virtual-machines-windows-sql-high-availability-dr.md)
* [Azure Sanal Makineler’de SQL Server için performansa yönelik en iyi uygulamalar](virtual-machines-windows-sql-performance.md)
* [Azure Sanal Makineler'de SQL Server için Uygulama Desenleri ve Geliştirme Stratejileri](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Linux Vm'leri**:

* [Bir Linux sanal makinesinde SQL Server'a genel bakış](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [SQL Server Linux VM'si sağlama](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [SSS (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Linux üzerinde SQL Server belgeleri](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
