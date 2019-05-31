---
title: Maliyetleri, Azure sanal makinelerinde SQL Server için etkili bir şekilde yönetme | Microsoft Docs
description: Tercih ettiğiniz fiyatlandırma modeli doğru SQL Server sanal makinesi için en iyi uygulamalar sağlanır.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/09/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: ce07c6c19c19f134cc322309bb338b94ef11ea85
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393849"
---
# <a name="pricing-guidance-for-sql-server-azure-vms"></a>SQL Server Azure Vm'leri için fiyatlandırma Kılavuzu

Bu makale için fiyatlandırma konusunda rehberlik yapmaktadır [SQL Server sanal makineleri](virtual-machines-windows-sql-server-iaas-overview.md) azure'da. Maliyet etkileyen birkaç seçenek vardır ve iş gereksinimleri ile maliyetleri dengeleyen doğru görüntüyü seçmek önemlidir.

> [!TIP]
> Yalnızca belirli bir SQL Server sürümü ve sanal makine boyutu birleşimi için maliyet tahmini öğrenmek gerekiyorsa yönelik fiyatlandırma sayfasına bakın [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows) veya [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux). Platform ve SQL Server sürümünden seçin **işletim sistemi/yazılım** listesi.
>
> ![Sanal makine Fiyatlandırma sayfasında kullanıcı Arabirimi](./media/virtual-machines-windows-sql-server-pricing-guidance/virtual-machines-pricing-ui.png)
>
> Veya [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/#explore-cost) eklemek ve bir sanal makineyi yapılandırmak için. 

## <a name="free-licensed-sql-server-editions"></a>Ücretsiz lisanslı SQL Server sürümleri

Geliştirme, test veya kavram kanıtı oluşturmak istiyorsanız, sonra ücretsiz lisanslı kullanın **SQL Server Developer edition**. Bu sürüm, yapı ve her tür uygulama test etmenize imkan sağlar, SQL Server Enterprise edition'ın tüm özelliklerini sahiptir. Ancak, geliştirici sürümü üretim ortamında çalıştırılamaz. İlişkili SQL Server Lisans maliyetlerini olduğundan SQL Server Developer edition VM, yalnızca VM maliyetini ücreti alınmaz.

Üretim ortamında basit bir iş yükü çalıştırmak istiyorsanız (< 4 çekirdek, < 1 GB bellek, < 10 GB/veritabanı), ücretsiz lisanslı kullanın **SQL Server Express edition**. Bir SQL Server Express edition VM maliyeti, VM ücretleri, yalnızca artmasına neden olur.

Bu iş yükleri eşleşen daha küçük bir VM boyutu seçerek, bu geliştirme/test ve hafif üretim iş yükleri için para kaydedebilirsiniz. DS1v2 bazı senaryolarda iyi bir seçim olabilir.

Bu görüntülerden birini ile bir SQL Server 2017 Azure VM oluşturmak için aşağıdaki bağlantılara bakın:

| Platform | Ücretsiz lisanslı görüntüler |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS) |

## <a name="paid-sql-server-editions"></a>Ücretli SQL Server sürümleri

Basit olmayan üretim iş yükü varsa, aşağıdaki SQL Server sürümlerinden birini kullanın:

| SQL Server sürümü | İş yükü |
|-----|-----|
| Web | Küçük web siteleri |
| Standart | Küçük ve orta ölçekli iş yükleri |
| Enterprise | Büyük veya görev açısından kritik iş yükleri|

Bu sürümlerde SQL Server lisansı için ödeme yapmak için iki seçeneğiniz vardır: *kullanım başına ödeme* veya *kendi lisansınızı getirin (BYOL)* .

## <a name="pay-per-usage"></a>Kullanım ödeme yapın

**SQL Server Lisansı kullanım başına ödeme** saniye başına maliyeti Azure VM'de çalışan SQL Server Lisans maliyetini içerir'anlamına gelir. Azure fiyatlandırma sayfası için sanal makine içindeki farklı SQL Server sürümleri için (Web, Standard, Enterprise) fiyatlandırmayı görmek [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows) veya [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux).

Maliyet, SQL Server (2012 SP3 için 2017) tüm sürümleri için aynıdır. Saniye başına lisans maliyeti, VM Vcpu sayısına bağlıdır.

SQL Server ödeme lisans kullanım için tavsiye edilir:

- **Geçici veya dönemsel iş yükleri**. Örneğin, bir uygulama, bir olay birkaç ay için her yıl ya da İş analizi Pazartesi günleri desteklemesi gerekir.

- **Bilinmeyen ömrü veya ölçek iş yükleri**. Örneğin, birkaç ay içinde gerekli olabilir değil ya da daha fazla veya daha az işlem gücü, isteğe bağlı olarak hangi gerektirebilir uygulama.

Bu kullanım başına ödeme görüntülerden birini ile bir SQL Server 2017 Azure VM oluşturmak için aşağıdaki bağlantılara bakın:

| Platform | Lisanslı görüntü |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonWindowsServer2016)<br/>[SQL Server 2017 standart Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonWindowsServer2016)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 standart Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2)<br/>[SQL Server 2017 standart Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS)<br/>[SQL Server 2017 standart Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS) |

> [!IMPORTANT]
> Portalda, bir SQL Server sanal makinesi oluşturduğunuzda **bir boyut seçin** penceresi bir tahmini maliyetini gösterir. Bu tahmin yalnızca lisans maliyetlerini (Windows veya üçüncü taraf Linux işletim sistemleri) herhangi bir işletim sistemi ile birlikte VM çalıştırmaya yönelik işlem maliyetleri olduğuna dikkat edin önemlidir.
>
> ![VM boyutu dikey](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-choose-size-pricing-estimate.png)
>
>Ek SQL Server Lisans maliyetlerini Web, Standard ve Enterprise sürümleri içermez. En doğru fiyatlandırma kestirmek için işletim sistemi ve SQL Server sürümü için Fiyatlandırma sayfasında seçin [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) veya [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

> [!NOTE]
> Şimdi Ödeme-kendi lisansınızı getirin (BYOL) ve kullanım başına bir lisanslama modeli değiştirmek mümkündür. Daha fazla bilgi için [bir SQL VM için lisanslama modelini değiştirme](virtual-machines-windows-sql-ahb.md). 

## <a id="byol"></a> Kendi lisansınızı getirin (BYOL)

**Lisans taşınabilirliği kendi SQL Server lisansınızı getirmek**, de denilen **KLG**, Azure VM'deki Yazılım Güvencesi içeren mevcut bir SQL Server toplu lisans kullanarak anlamına gelir. SQL Server zaten lisanslarınızı ve Yazılım Güvencesi Toplu Lisanslama programı aracılığıyla satın aldığınız koşuluyla, tek ücretleri KLG VM çalıştırmanın maliyeti, SQL Server Lisans değil, kullanarak bir VM.

> [!IMPORTANT]
> KLG görüntüleri, Yazılım Güvencesi ile Kurumsal Sözleşme gerektirir. Şu anda Azure Cloud Solution Partner (CSP) bir parçası olarak kullanılamaz. Bir Kullandıkça Öde görüntüsü dağıtma ve ardından etkinleştirme, CSP müşterileri kendi lisansını Getir [Azure hibrit avantajı](virtual-machines-windows-sql-ahb.md).

> [!NOTE]
> KLG görüntüleri şu anda yalnızca Windows sanal makineler için kullanılabilir. Ancak, yalnızca Linux VM üzerinde SQL Server el ile yükleyebilirsiniz. İçindeki yönergelere bakın [Linux SQL VM SSS](../../linux/sql/sql-server-linux-faq.md).

Kendi SQL getirme ile lisans taşınabilirliği lisanslama için önerilir:

- **Sürekli iş yükleri**. Örneğin, bir uygulama, işle ilgili işlemlerin 24 x 7 desteklemesi gerekir.

- **Bilinen bir yaşam süresi ve ölçeklendirme ile iş yüklerini**. Örneğin, yılın tamamına denk gelen ve hangi Talep tahmini için gerekli bir uygulama.

KLG SQL Server VM ile kullanmak için SQL Server Standard veya Enterprise lisansınız olmalıdır ve [Yazılım Güvencesi](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx#tab=1), bazı toplu lisanslama programları ve diğerleri ile isteğe bağlı bir satın alma yoluyla gerekli bir seçenek olduğu. Toplu Lisanslama programları üzerinden sunulan fiyatlandırma düzeyi, anlaşma ve miktarı veya ve SQL Server için taahhüt türüne göre değişir. Ancak, bir kural karşısında, sürekli üretim iş yükleri için kendi lisansınızı getirmek aşağıdaki faydaları vardır:

| KLG avantajı | Açıklama |
|-----|-----|
| **Maliyet tasarrufu** | Kendi SQL Server lisansınızı getirmek daha düşük bir iş yükü SQL Server Standard veya Enterprise için sürekli olarak çalışıyorsa, kullanım başına ödeme yapma daha maliyetli *10 aydan daha*. |
| **Uzun vadeli tasarruflar** | Ortalama olarak olan *% 30'luk yılda ucuz* satın almanız veya ilk 3 yıl için bir SQL Server lisansınızı yenileyin. Ayrıca, 3 yıl sonra gerekmez lisans artık yenilemek için yalnızca Yazılım Güvencesi için ödeme yaparsınız. Bu noktada, olan *%200 ucuz*. |
| **Ücretsiz pasif ikincil çoğaltma** | Kendi lisansınızı getirmek başka bir faydası [ücretsiz Pasif bir ikincil çoğaltma için lisanslama](https://azure.microsoft.com/pricing/licensing-faq/) başına SQL Server yüksek kullanılabilirlik sağlamak için. Bu yüksek oranda kullanılabilir bir SQL Server dağıtımı (örneğin, Always On kullanılabilirlik grupları'nı kullanarak) yarı lisans maliyeti keser. Pasif ikincil çalıştırmak üzere hakları sağlanan yük devretme sunucuları Yazılım Güvencesi avantajı aracılığıyla. |

Bu duruma-kendi lisansını görüntülerden birini ile bir SQL Server 2017 Azure VM oluşturmak için "{KLG}" ile önek Vm'leri bakın:

- [SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017EnterpriseWindowsServer2016)
- [SQL Server 2017 standart Azure VM](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017StandardonWindowsServer2016)

> [!IMPORTANT]
> Bize 10 gün içinde kaç SQL Server Lisansı Azure'da kullandığınız bildirin. Önceki görüntülerinin bağlantılarını bunun nasıl yapılacağı hakkında yönergeler sahip.

> [!NOTE]
> Şimdi Ödeme-kendi lisansınızı getirin (BYOL) ve kullanım başına bir lisanslama modeli değiştirmek mümkündür. Daha fazla bilgi için [bir SQL VM için lisanslama modelini değiştirme](virtual-machines-windows-sql-ahb.md). 



## <a name="reduce-costs"></a>Maliyetleri azaltın

Gereksiz maliyetleri önlemek için en uygun sanal makine boyutu seçin ve sürekli olmayan iş yükleri için aralıklı kapatmalar göz önünde bulundurun.

### <a id="machinesize"></a> Sanal makinenizin doğru boyut

SQL Server Lisans maliyetini Vcpu sayısı doğrudan ilgilidir. CPU, bellek, depolama ve g/ç bant genişliği için beklenen gereksinimlerinize uyan bir VM boyutu seçin. Makine boyutu seçeneklerini tam bir listesi için bkz. [Windows VM boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) ve [Linux VM boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

SQL Server iş yüklerini de belirli türleriyle çalışma yeni makine boyutları vardır. Bu makine boyutları, bellek, depolama ve g/ç bant genişliği yüksek düzeyde korumak ancak daha düşük bir sanallaştırılmış çekirdek sayısı sahiptir. Örneğin, aşağıdaki örneği göz önünde bulundurun:

| VM Boyutu | Vcpu | Bellek | En büyük diskler | En fazla g/ç aktarım hızı | SQL lisanslama maliyetleri | Toplam Maliyet (işlem + lisanslama) |
|---|---|---|---|---|---|---|
| **Standard_DS14v2** | 16 | 112 GB | 32 | 51.200 IOPS veya 768 MB/sn | | |
| **Standard_DS14-4v2** | 4 | 112 GB | 32 | 51.200 IOPS veya 768 MB/sn | %75 daha düşük | % 57'daha düşük |

> [!IMPORTANT]
> Bu zaman içinde nokta örnektir. En son özellikleri için makine boyutları makalelerini ve fiyatlandırma sayfası için Azure bakın [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) ve [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

Önceki örnekte gördüğünüz gibi belirtimleri **Standard_DS14v2** ve **Standard_DS14 4v2** Vcpu dışında aynıdır. Sonek **-4v2** sonunda **Standard_DS14 4v2** makine boyutu etkin Vcpu sayısını belirtir. SQL Server Lisans maliyetlerini Vcpu sayısı bağlı oldukları için bu burada ek Vcpu gerekmeyen senaryolarında VM maliyetini önemli ölçüde azaltır. Bu bir örnektir ve bu sonek desen ile tanıtılan kısıtlanmış Vcpu'lar ile birçok makine boyutları vardır. Daha fazla bilgi için bkz. blog gönderisine [daha uygun maliyetli veritabanı iş için yeni bir Azure VM boyutları Duyurusu](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/).

### <a name="shut-down-your-vm-when-possible"></a>Mümkün olduğunda, VM kapatma

Sürekli olarak çalıştırmayın herhangi bir iş yükünü kullanıyorsanız, etkin olmayan dönemlerde sanal makinesi kapatılıyor göz önünde bulundurun. Sadece kullandığınız kadar ödersiniz.

Örneğin, yalnızca SQL Server Azure sanal makinesinde kullanıma çalışıyorsanız, yanlışlıkla hafta boyunca çalışır durumda bırakarak ücretleri uygulanmaya istemezsiniz. Bir çözüm ise kullanılacak [otomatik kapatma özelliği](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![SQL VM autoshutdown](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-auto-shutdown.png)

Otomatik kapatma benzer özellikleri tarafından sağlanan daha büyük bir parçası olan [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab).

Diğer iş akışları için otomatik olarak kapatılmasını ve gibi bir komut dosyası çözüm ile Azure Vm'leri yeniden başlatmayı düşünün [Azure Otomasyonu](https://azure.microsoft.com/services/automation/).

> [!IMPORTANT]
> Kapatma ve, VM serbest bırakılıyor ücretlerden kaçınmak için tek yoludur. Yine de sadece durdurma veya sanal makineyi güç seçeneklerini kullanarak kullanım ücreti alınmaz.

## <a name="next-steps"></a>Sonraki adımlar

Genel Azure için fiyatlandırma kılavuzu için bkz [Azure'da faturalandırma ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](../../../billing/billing-getting-started.md). En son sanal makineler için fiyatlandırma, SQL Server dahil olmak üzere Azure VM fiyatlandırma sayfası için Azure bakın [Windows Vm'leri](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) ve [Linux Vm'leri](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

Azure sanal makineler üzerinde çalışan SQL Server'a genel bakış için aşağıdaki makalelere bakın:

- [Windows vm'lerinde SQL Server'a genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)
- [Linux vm'lerinde SQL Server'a genel bakış](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
