---
title: Maliyetleri SQL Server için Azure sanal makinelerde etkili şekilde yönetebilmek. | Microsoft Docs
description: Doğru SQL Server sanal makine fiyatlandırma modeli seçmek için en iyi yöntemler sağlar.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2018
ms.author: jroth
ms.openlocfilehash: 71c86af9d4dcdf1026b4f539574b9932ef1cfc89
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32767809"
---
# <a name="pricing-guidance-for-sql-server-azure-vms"></a>SQL Server Azure VM'ler için fiyatlandırma Kılavuzu

Bu makalede yönelik fiyatlandırma yönergeler sağlanmaktadır [SQL Server sanal makineleri](virtual-machines-windows-sql-server-iaas-overview.md) azure'da. Maliyet etkileyen birkaç seçeneğiniz vardır ve iş gereksinimleri maliyetleriyle dengeleyen doğru görüntüyü seçmek önemlidir.

> [!TIP]
> Yalnızca SQL Server sürümü ve sanal makine boyutu belirli bir bileşimini yönelik bir maliyet tahmini öğrenmek gerekirse için fiyatlandırma sayfasını bkz [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows) veya [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux). SQL Server sürümünden ve platform seçin **işletim sistemi/yazılım** listesi.
>
> ![VM Fiyatlandırma sayfasında kullanıcı Arabirimi](./media/virtual-machines-windows-sql-server-pricing-guidance/virtual-machines-pricing-ui.png)
>
> Veya [fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/#explore-cost) eklemek ve bir sanal makine yapılandırmak için. 

## <a name="free-licensed-sql-server-editions"></a>Serbest lisanslı SQL Server sürümleri

Geliştirme, test ya da bir kavram kanıtı oluşturmak istiyorsanız, ardından serbestçe lisanslı kullanmak **SQL Server Geliştirici sürümü**. Bu sürüm SQL Server Enterprise Edition, yapı ve uygulama herhangi bir türde test olanak tanıyan tüm özelliklere sahiptir. Ancak, geliştirici sürümü üretimde çalışamaz. İlişkili SQL Server Lisans maliyetleri olduğundan SQL Server Geliştirici sürümü VM yalnızca VM maliyeti için ücret doğurur.

Üretimde basit bir iş yükü çalıştırmak isteyip istemediğinizi (< 4 çekirdek, < 1 GB bellek, < 10 GB/veritabanı), ücretsiz olarak lisanslı kullanmak **SQL Server Express edition**. Bir SQL Server Express edition VM ayrıca yalnızca VM maliyeti için ücret doğurur.

Bu geliştirme ve test ve basit üretim iş yükleri için bu iş yükleri eşleşen daha küçük bir VM boyutu seçerek para da kaydedebilirsiniz. DS1v2 bazı senaryolarda iyi bir seçenek olabilir.

Bu görüntülerden birini bir SQL Server 2017 Azure VM oluşturmak için aşağıdaki bağlantılara bakın:

| Platform | Ücretsiz olarak lisanslı görüntüleri |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Geliştirici Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Geliştirici Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Geliştirici Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Geliştirici Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS) |

## <a name="paid-sql-server-editions"></a>Ücretli SQL Server sürümleri

Basit olmayan üretim iş yükü varsa, aşağıdaki SQL Server sürümlerinden birini kullanın:

| SQL Server sürümü | İş yükü |
|-----|-----|
| Web | Küçük web siteleri |
| Standart | Küçük ve orta iş yükleri |
| Enterprise | Büyük veya kritik iş yükleri|

Bu sürümleri için SQL Server Lisans için ödeme yapmak için iki seçeneğiniz vardır: *kullanım başına ödeme* veya *kendi lisansını (KLG)*.

## <a name="pay-per-usage"></a>Kullanım Öde

**SQL Server Lisansı kullanım başına ödeme** saniyede maliyeti Azure VM çalışan SQL Server Lisans maliyetini içerdiği anlamına gelir. Fiyatlandırma sayfası için Azure VM'deki farklı SQL Server sürümleri için (Web, Standard, Enterprise) fiyatlandırma görebilirsiniz [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows) veya [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux).

Maliyet (2012 SP3 için 2017) SQL Server'ın tüm sürümleri için aynıdır. Saniye başına lisanslama ücreti tüm SQL Server Lisans için bir standart olan VM çekirdek sayısına bağlıdır.

SQL Server ödeme kullanım lisansı için önerilir:

- **Geçici veya düzenli aralıklarla iş yükleri**. Örneğin, bir uygulama, bir olay için ayda birkaç her yıl veya İş analizi Pazartesi günleri desteklemesi gerekir.

- **Bilinmeyen yaşam süresi veya ölçeğe sahip iş yüklerini**. Örneğin, birkaç ay içinde gerekli olabilecek değil ya da daha fazla veya daha az işlem gücü, isteğe bağlı olarak gerektirebilecek uygulama.

Bu kullanım başına ödeme görüntülerden birini bir SQL Server 2017 Azure VM oluşturmak için aşağıdaki bağlantılara bakın:

| Platform | Lisanslı görüntüleri |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonWindowsServer2016)<br/>[SQL Server 2017 standart Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonWindowsServer2016)<br/>[SQL Server 2017 kuruluş Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 standart Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 kuruluş Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2)<br/>[SQL Server 2017 standart Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2)<br/>[SQL Server 2017 kuruluş Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS)<br/>[SQL Server 2017 standart Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS)<br/>[SQL Server 2017 kuruluş Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS) |

> [!IMPORTANT]
> Portalda, bir SQL Server sanal makine oluşturduğunuzda **bir boyutu seçin** penceresi tahmini maliyet gösterir. Bu tahmin lisanslama maliyetleri (Windows veya üçüncü taraf Linux işletim sistemleri) herhangi bir işletim sistemi ile birlikte VM çalıştırmak için yalnızca işlem maliyetleri olduğunu dikkate almak önemlidir.
>
> ![VM boyutu dikey seçin](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-choose-size-pricing-estimate.png)
>
>Ek SQL Server Lisans maliyetleri Web, Standard ve Enterprise sürümleri için içermez. En doğru fiyatlandırma kestirmek için işletim sistemi ve SQL Server sürümü için Fiyatlandırma sayfasında seçin [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) veya [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="bring-your-own-license-byol"></a>Kendi lisansınızı getirin (BYOL)

**SQL Server Lisansı ile lisans taşınabilirliği getiren**, de denilen **KLG**, Azure VM'deki Yazılım Güvencesi ile varolan bir SQL Server toplu lisans kullanarak anlamına gelir. SQL Server zaten lisansları ve Yazılım Güvencesi Toplu Lisanslama programı aracılığıyla satın aldığınız koşuluyla, KLG yalnızca ücretleri VM çalıştıran maliyeti için SQL Server Lisans değil, kullanarak bir VM.

> [!NOTE]
> KLG görüntüleri şu anda yalnızca Windows sanal makineler için kullanılabilir. Ancak, yalnızca Linux VM üzerinde SQL Server el ile yükleyebilirsiniz. İçindeki yönergelere bakın [Linux SQL VM SSS](../../linux/sql/sql-server-linux-faq.md).

Kendi SQL getirme ile lisans taşınabilirliği lisans için önerilir:

- **Sürekli iş yükleri**. Örneğin, bir uygulama, iş işlemleri 7 x 24 desteklemesi gerekir.

- **Bilinen yaşam süresi ve ölçek ile iş yüklerini**. Örneğin, bir uygulama, tüm yıl ve hangi talebin tahmini için gereklidir.

KLG bir SQL Server VM ile kullanmak için SQL Server Standard veya Enterprise lisansınız olmalıdır ve [Yazılım Güvencesi](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx#tab=1), bazı toplu lisanslama programları ve başkalarıyla isteğe bağlı bir satın alma yoluyla gerekli bir seçenek değil. Toplu Lisanslama programları üzerinden sağlanan fiyatlandırma düzeyi, anlaşma ve miktarı ve veya SQL Server taahhüt türüne göre değişir. Ancak bir altın kural olarak, sürekli üretim iş yükleri için kendi lisansını getiren aşağıdaki faydaları vardır:

| KLG avantajı | Açıklama |
|-----|-----|
| **Maliyet tasarrufları** | Kendi SQL Server lisansınızı getiren daha düşük bir iş yükü SQL Server Standard veya Enterprise için sürekli olarak çalışıyorsa, kullanım ödeme daha maliyetli *10 aydan daha uzun*. |
| **Uzun vadeli tasarrufları** | Ortalama olarak olan *% 30 yıllık ucuz* satın almanız veya bir SQL Server Lisansı ilk 3 yıldır yenileyin. Ayrıca, 3 yıl sonra kullanmadığınız lisans artık yenilemek için yalnızca Yazılım Güvencesi için ödeme yaparsınız. Bu noktada, olan *% 200 ucuz*. |
| **Ücretsiz pasif ikincil çoğaltma** | Kendi lisansını getiren başka bir yararı [Pasif bir ikincil çoğaltma için lisans serbest](https://azure.microsoft.com/pricing/licensing-faq/) başına SQL Server yüksek kullanılabilirlik sağlamak için. Bu yüksek oranda kullanılabilir bir SQL Server dağıtımı (örneğin, Always On kullanılabilirlik grupları kullanarak) yarım lisanslama ücreti keser. Pasif ikincil çalıştırmak üzere hakları sağlanan yük devretme sunucuları Yazılım Güvencesi avantajı aracılığıyla. |

Bu Getir bilgisayarınızı-kendi-lisans görüntülerden birini bir SQL Server 2017 Azure VM oluşturmak için "{KLG}" ile önek VM'ler bakın:

- [SQL Server 2017 kuruluş Azure VM](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017EnterpriseWindowsServer2016)
- [SQL Server 2017 standart Azure VM](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017StandardonWindowsServer2016)

> [!IMPORTANT]
> Bize 10 gün içinde Azure'da kullandığınız kaç SQL Server lisansları bildirin. Önceki görüntüleri bağlantılara bunun nasıl yapılacağı hakkında yönergeler vardır.

> [!NOTE]
> Bir ödeme saniyede kendi lisansınızı kullanmak için SQL Server VM lisans modelini değiştirmek mümkün değil. Bu durumda yeni bir KLG VM oluşturmanız ve veritabanlarınızı yeni VM'ye geçirmeniz gerekir.

## <a name="reduce-costs"></a>Maliyetlerini azaltma

Gereksiz maliyetleri önlemek için bir en iyi sanal makine boyutu seçin ve aralıklı kapatmalar sürekli olmayan iş yükleri için göz önünde bulundurun.

### <a id="machinesize"></a> Doğru VM boyutu

SQL Server Lisans maliyetini çekirdek sayısı doğrudan ilişkilidir. Beklenen gereksiniminize CPU, bellek, depolama ve g/ç bant genişliği için bir VM boyutu seçin. Makine boyutu seçeneklerini tam bir listesi için bkz: [Windows VM boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) ve [Linux VM boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

SQL Server iş yüklerini de belirli türleriyle çalışma yeni makine boyutlarını vardır. Bu makineler boyutları yüksek düzeyde bellek, depolama ve g/ç bant genişliği korumak, ancak daha düşük bir sanallaştırılmış çekirdek sayısı sahiptirler. Örneğin, aşağıdaki örnekte göz önünde bulundurun:

| VM Boyutu | vCPU sayısı | Bellek | Max diskleri | En fazla g/ç işleme | SQL lisanslama maliyetleri | Toplam fiyatlar (işlem + lisans) |
|---|---|---|---|---|---|---|
| **Standard_DS14v2** | 16 | 112 GB | 32 | 51.200 IOPS veya 768 MB/sn | | |
| **Standard_DS14-4v2** | 4 | 112 GB | 32 | 51.200 IOPS veya 768 MB/sn | % 75 daha düşük | %57 daha düşük |

> [!IMPORTANT]
> Bu zaman içinde nokta örnektir. En son özelliklerini, makine boyutlarını makaleleri ve fiyatlandırma sayfası için Azure bakın [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) ve [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

Önceki örnekte, gördüğünüz özelliklerini **Standard_DS14v2** ve **Standard_DS14 4v2** Vcpu'lar dışında aynıdır. Sonek **-4v2** sonunda **Standard_DS14 4v2** makine boyutu etkin Vcpu'lar sayısını belirtir. SQL Server Lisans maliyetleri çekirdek sayısı bağlı oldukları için bu VM burada fazladan Vcpu'lar gerekmeyen senaryolarda maliyetini önemli ölçüde azaltır. Bu bir örnektir ve bu soneki deseni ile tanımlanır kısıtlanmış Vcpu'lar ile çok sayıda makine boyutlarını vardır. Daha fazla bilgi için blog gönderisine bakın [yeni Azure VM boyutları için daha fazla uygun maliyetli veritabanı iş Duyurusu](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/).

### <a name="shut-down-your-vm-when-possible"></a>Mümkün olduğunda, VM kapatma

Sürekli olarak çalıştırmayın herhangi bir iş yükünü kullanıyorsanız, etkin olmayan dönemlerde sanal makinesi kapatılıyor göz önünde bulundurun. Sadece kullandığınız kadar ödersiniz.

Yalnızca Azure VM'de SQL Server çıkışı çalışıyorsanız, örneğin, yanlışlıkla hafta boyunca çalışır durumda bırakarak ücretlendirme istediğiniz değil. Bir çözüm kullanmaktır [otomatik kapatma özelliği](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![SQL VM autoshutdown](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-auto-shutdown.png)

Otomatik kapatma benzer özellikleri tarafından sağlanan daha büyük bir parçası olan [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab).

Diğer iş akışları için otomatik olarak kapatma ve gibi bir komut dosyası çözümüyle Azure sanal makineleri yeniden başlatmayı düşünün [Azure Otomasyonu](https://azure.microsoft.com/services/automation/).

> [!IMPORTANT]
> Kapatılıyor ve VM serbest bırakma ücret yazılmaması için tek yoludur. Hala basitçe durdurma veya VM kapatma için Güç Seçenekleri'ni kullanarak kullanım ücretleri doğurur.

## <a name="next-steps"></a>Sonraki adımlar

Kılavuzlar, fiyatlandırma için genel Azure bkz [Azure faturalama ve maliyet yönetimi ile beklenmeyen maliyetleri önlemek](../../../billing/billing-getting-started.md). Son sanal makineler için fiyatlandırma, SQL Server dahil olmak üzere Azure VM fiyatlandırma sayfası için Azure bkz [Windows Vm'lerini](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) ve [Linux VM'ler](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

Azure sanal makinelerde çalışan SQL Server genel bakış için aşağıdaki makalelere bakın:

- [Windows vm'lerde SQL Server'ın genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)
- [Linux sanal makineleri üzerinde SQL Server'ın genel bakış](../../linux/sql/sql-server-linux-virtual-machines-overview.md)