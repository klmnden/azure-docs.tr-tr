---
title: Azure Backup nedir? | Microsoft Belgeleri
description: "Azure Backup ve Kurtarma Hizmetleri&quot;ni kullanarak; Windows Sunucuları’ndan, Windows istemci makinelerinden, System Center DPM sunucularından ve Azure sanal makinelerinden verileri ve uygulamaları yedekleyip geri yükleyebilirsiniz."
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: 
keywords: "yedekleme ve geri yükleme; kurtarma hizmetleri; yedekleme çözümleri"
ms.assetid: 0d2a7f08-8ade-443a-93af-440cbf7c36c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/6/2016
ms.author: jimpark; trinadhk
translationtype: Human Translation
ms.sourcegitcommit: b9737c3da308aecf25d5f18088f96c319edeafd5
ms.openlocfilehash: 76ec51a75240710b24c0e91042d6229e60eeada9


---
# <a name="what-is-azure-backup"></a>Azure Backup nedir?
Azure Backup, verilerinizi Microsoft bulutunda yedeklemek (veya korumak) ve geri yüklemek için kullanabileceğiniz Azure tabanlı bir hizmettir. Azure Backup, var olan şirket içi veya şirket dışı yedekleme çözümünüzün yerine, güvenilir, güvenli ve maliyet açısından rekabetçi bir bulut tabanlı çözüm sunar. Azure Backup, indirdikten sonra uygun bilgisayar, sunucu veya buluta dağıtabileceğiniz birden fazla bileşene sahiptir. Dağıtacağınız bileşen veya aracı, korumak istediğiniz nesnelere göre değişiklik gösterir. Tüm Azure Backup bileşenleri (koruduğunuz veriler şirket içi veya bulut verileri olabilir) verileri Azure’daki bir yedek kasasına yedeklemek için kullanılabilir. Belirli verileri, uygulamaları veya iş yüklerini korumak için kullanılacak bileşenler hakkında bilgi almak için [Azure Backup bileşen tablosuna](backup-introduction-to-azure-backup.md#which-azure-backup-components-should-i-use) (bu makalenin ilerleyen bölümlerinde) bakın.

[Azure Backup'a genel bakış videosunu izleyin](https://azure.microsoft.com/documentation/videos/what-is-azure-backup/)

## <a name="why-use-azure-backup"></a>Azure Backup'ı neden kullanmalısınız?
Geleneksel yedekleme çözümleri, bulutu disk veya bantlara benzer bir uç nokta veya statik depolama hedefi olarak işleyecek şekilde gelişmiştir. Bulut platformunun tüm avantajlarından faydalanamayan bu basit yaklaşım sınırlı, pahalı ve verimsiz bir çözüm sunmaktadır. Yanlış veya ihtiyacınız olmayan depolama bileşenleri satın almanızı gerektiren diğer çözümler yüksek maliyetlidir. Diğer çözümler, ihtiyacınız olan depolama türünü veya miktarını sunmadığından ya da yönetimsel görevler çok uzun zaman aldığından verimli değildir. Buna karşılık Azure Backup şu önemli avantajlara sahiptir:

**Otomatik depolama yönetimi** - Karma ortamlar genelde heterojen depolamaya (bazıları şirket içi, bazıları ise bulutta olan) ihtiyaç duyar. Azure Backup çözümünde, şirket içi depolama cihazlarının kullanımıyla ilişkili maliyetler yoktur. Azure Backup, yedekleme alanını otomatik olarak ayırıp yönetir ve "kullandıkça öde" modelini kullanır. Kullandıkça öde yöntemiyle yalnızca kullandığınız depolama alanı için ödeme yaparsınız. Daha fazla bilgi için bkz. [Azure fiyatlandırma makalesi](https://azure.microsoft.com/pricing/details/backup).

**Sınırsız ölçekleme** - Azure Backup, yüksek kullanılabilirlik sunmak için Azure bulutunun gücünü ve sınırsız ölçekleme özelliğini kullanır. Üstelik bakım ve izleme maliyetleri yoktur. Olaylar hakkında bilgi almak için uyarılar oluşturabilirsiniz ancak buluttaki verilerinizin yüksek kullanılabilirliği konusunda endişelenmeniz gerekmez.

**Birden çok depolama seçeneği** - Yüksek kullanılabilirliğin bir özelliği de depolama çoğaltmadır. Azure Backup iki tür çoğaltma sunar: [Yerel olarak yedekli depolama](../storage/storage-redundancy.md#locally-redundant-storage) ve [coğrafi olarak yedekli depolama](../storage/storage-redundancy.md#geo-redundant-storage). İhtiyacınız olan yedek depolama seçeneğini belirleyin:

* Yerel olarak yedekli depolama (LRS), verilerinizi aynı bölgedeki bir eşlenmiş veri merkezine üç kez kopyalar (verilerinizin üç kopyasını oluşturur). LRS, düşük maliyetli bir seçenektir ve verileri yerel donanım hatalarına karşı koruduğu için harcamalarını alt seviyede tutmak isteyen müşteriler için idealdir.
* Coğrafi çoğaltmalı depolama (GRS), verilerinizi ikincil bir bölgeye (kaynak verilerin birincil konumundan yüzlerce kilometre uzakta) kopyalar. GRS’nin maliyeti LRS’den yüksektir ancak bölgesel kesintiler olsa bile daha yüksek veri dayanıklılık düzeyi sunar.

**Sınırsız veri aktarımı** - Azure Backup, aktardığınız verilerde aşağı ya da yukarı yönlü bir sınırlama yapmaz. Azure Backup ayrıca aktarılan veriler için ücret talep etmez. Ancak, büyük miktarda veriyi içeri aktarmak için Azure İçeri/Dışarı Aktarma hizmetini kullanırsanız gelen verilerden ücret alınır. Bu maliyet hakkında daha fazla bilgi için bkz. [Azure Backup’ta çevrimdışı yedekleme iş akışı](backup-azure-backup-import-export.md). Giden veriler, geri yükleme işlemi sırasında bir yedek kasasından aktarılan verileri tanımlar.

**Veri şifrelemesi** - Veri şifrelemesi, genel bulutta verilerinizin güvenli şekilde iletilmesini ve depolanmasını sağlar. Şifreleme parolası yerel olarak depolanır ve hiçbir zaman Azure'a iletilmez veya orada depolanmaz. Verileri geri yüklemeniz gerekirse, şifreleme parolası veya anahtarı yalnızca sizde olur.

**Uygulamayla tutarlı yedekleme** - Yedeklediğiniz ister bir dosya sunucusu, isterse sanal makine veya SQL veritabanı olsun, kurtarma noktası yedek kopyayı geri yüklemek için gerekli tüm verilere sahiptir. Azure Backup, verilerin geri yüklenmesi için ek düzeltmelere gerek kalmaması için uygulamayla tutarlı yedeklemeler yapılmasını sağlar. Uygulamayla tutarlı verilerin geri yüklenmesi, geri yükleme süresini azaltarak hizmetlerinizin kısa süre içinde çalışır hale gelmesini sağlar.

**Uzun vadeli elde tutma** - Azure yedeklerinizi 99 yıl boyunca tutun. Yedek kopyaları diskten banda alıp bandı da uzun vadeli depolama için uzak bir konuma taşımak yerine Azure’u kullanarak hem kısa vadeli hem de uzun vadeli bekletme seçeneklerinden faydalanabilirsiniz.

## <a name="which-azure-backup-components-should-i-use"></a>Hangi Azure Backup bileşenlerini kullanmalıyım?
İhtiyaçlarınıza uygun Azure Backup bileşenlerinin hangileri olduğundan emin değilseniz, aşağıdaki tabloyu inceleyerek her bir bileşenin koruduğu veriler hakkında bilgi edinebilirsiniz. Azure portalındaki yerleşik sihirbaz, indirmeniz ve dağıtmanız gereken bileşeni seçme konusunda size yardımcı olacaktır. Kurtarma Hizmetleri kasası oluşturma işlemlerinin bir parçası olan sihirbaz, yedekleme hedefinin yanı sıra koruma altına alınacak verileri veya uygulamayı seçmenize yardımcı olur.

| Bileşen | Avantajlar | Sınırlar | Neler korunuyor? | Yedekler nerede saklanıyor? |
| --- | --- | --- | --- | --- |
| Azure Backup (MARS) aracısı |<li>Fiziksel veya sanal Windows işletim sistemi üzerindeki dosya ve klasörleri yedekler (VM’ler şirket içinde veya Azure’da olabilir)<li>Ayrı bir yedekleme sunucusu gerekli değildir. |<li>Günde 3 kez yedekleme <li>Uygulamayı algılamaz; yalnızca dosya, klasör ve birim düzeyinde geri yükleme, <li>  Linux desteği yok. |<li>Dosyalar, <li>Klasörler |Azure Backup kasası |
| System Center DPM |<li>Uygulama kullanan anlık görüntüler (VSS)<li>Yedeklemelerin alınma zamanı için tam esneklik<li>Kurtarma ayrıntı düzeyi (tümü)<li>Azure Backup kasasını kullanabilir<li>Hyper-V ve VMware VM’lerinde Linux desteği <li>DPM 2012 R2 kullanan VMware VM’lerini koruma |Oracle iş yükü yedeklenemiyor.|<li>Dosyalar, <li>Klasörler,<li> Birimler, <li>VM’ler,<li> Uygulamalar,<li> İş yükleri |<li>Azure Backup kasası,<li> Yerel olarak bağlı disk,<li>  Bant (yalnızca şirket içi) |
| Azure Backup Sunucusu |<li>Uygulama kullanan anlık görüntüler (VSS)<li>Yedeklemelerin alınma zamanı için tam esneklik<li>Kurtarma ayrıntı düzeyi (tümü)<li>Azure Backup kasasını kullanabilir<li>Linux desteği (Hyper-V üzerinde barındırılıyorsa)<li>DPM 2012 R2 kullanan VMware VM’lerini koruma<li>System Center lisansı gerektirmez |<li>Oracle iş yükü yedeklenemiyor.<li>Her zaman canlı Azure aboneliği gerektirir<li>Bant yedekleme desteği yoktur |<li>Dosyalar, <li>Klasörler,<li> Birimler, <li>VM’ler,<li> Uygulamalar,<li> İş yükleri |<li>Azure Backup kasası,<li> Yerel olarak bağlı disk |
| Azure IaaS VM Backup |<li>Windows/Linux için yerel yedeklemeler<li>Belirli bir aracı yüklemesi gerekmez<li>Yedekleme altyapısı gerekmeden yapı düzeyinde yedekleme |<li>VM’leri günde bir kez yedekleme <li>VM’leri yalnızca disk düzeyinde geri yükleme<li>Şirket içi yedekleme gerçekleştirilemez |<li>VM’ler, <li>Tüm diskler (PowerShell kullanarak) |<p>Azure Backup kasası</p> |

## <a name="what-are-the-deployment-scenarios-for-each-component"></a>Her bileşen için dağıtım senaryoları nelerdir?
| Bileşen | Azure'da dağıtılabilir mi? | Şirket içinde dağıtılabilir mi? | Hedef depolama desteklenir |
| --- | --- | --- | --- |
| Azure Backup (MARS) aracısı |<p>**Evet**</p> <p>Azure Backup aracısı, Azure'da çalışan herhangi bir Windows Server VM üzerinde dağıtılabilir.</p> |<p>**Evet**</p> <p>Azure Backup aracısı, herhangi bir Windows Server VM veya fiziksel makinesi üzerinde dağıtılabilir.</p> |<p>Azure Backup kasası</p> |
| System Center DPM |<p>**Evet**</p><p>[System Center DPM'yi kullanarak Azure'da iş yüklerini koruma](backup-azure-dpm-introduction.md) hakkında daha fazla bilgi edinin.</p> |<p>**Evet**</p> <p>[Veri merkezinizde iş yüklerini ve VM'leri koruma](https://technet.microsoft.com/en-us/system-center-docs/dpm/data-protection-manager) hakkında daha fazla bilgi edinin.</p> |<p>Yerel olarak bağlı disk,</p> <p>Azure Backup kasası,</p> <p>bant (yalnızca şirket içi)</p> |
| Azure Backup Sunucusu |<p>**Evet**</p><p>[Azure Backup'ı kullanarak Azure'da iş yüklerini koruma](backup-azure-microsoft-azure-backup.md) hakkında daha fazla bilgi edinin.</p> |<p>**Evet**</p> <p>[Azure Backup'ı kullanarak Azure'da iş yüklerini koruma](backup-azure-microsoft-azure-backup.md) hakkında daha fazla bilgi edinin.</p> |<p>Yerel olarak bağlı disk,</p> <p>Azure Backup kasası</p> |
| Azure IaaS VM Backup |<p>**Evet**</p><p>Azure yapısının parçası</p><p>[Hizmet olarak Azure altyapısı (IaaS) sanal makinelerinin yedeklemesine](backup-azure-vms-introduction.md) yöneliktir.</p> |<p>**Hayır**</p> <p>Veri merkezinizdeki sanal makineleri yedeklemek için System Center DPM'yi kullanın.</p> |<p>Azure Backup kasası</p> |

## <a name="which-applications-and-workloads-can-be-backed-up"></a>Hangi uygulamalar ve iş yükleri yedeklenebilir?
Aşağıdaki tabloda Azure Backup kullanarak koruma altına alınabilecek veriler ve iş yükleri verilmiştir. Azure Backup çözümü sütununda ilgili çözüme ait dağıtım belgelerine bağlantı sunulmuştur. Her bir Azure Backup bileşeni Klasik (Service Manager dağıtımı) veya Resource Manager dağıtım modeli ortamında dağıtılabilir.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

| Veri veya İş Yükü | Kaynak ortamı | Azure Backup çözümü |
| --- | --- | --- |
| Dosyalar ve klasörler |Windows Server |<p>[Azure Backup aracısı](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ Azure Backup aracısı),</p> <p>[Azure Backup Sunucusu](backup-azure-microsoft-azure-backup.md) (Azure Backup aracısını içerir)</p> |
| Dosyalar ve klasörler |Windows bilgisayar |<p>[Azure Backup aracısı](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ Azure Backup aracısı),</p> <p>[Azure Backup Sunucusu](backup-azure-microsoft-azure-backup.md) (Azure Backup aracısını içerir)</p> |
| Hyper-V sanal makine (Windows) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup aracısı),</p> <p>[Azure Backup Sunucusu](backup-azure-microsoft-azure-backup.md) (Azure Backup aracısını içerir)</p> |
| Hyper-V sanal makine (Linux) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup aracısı),</p> <p>[Azure Backup Sunucusu](backup-azure-microsoft-azure-backup.md) (Azure Backup aracısını içerir)</p> |
| Microsoft SQL Server |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup aracısı),</p> <p>[Azure Backup Sunucusu](backup-azure-microsoft-azure-backup.md) (Azure Backup aracısını içerir)</p> |
| Microsoft SharePoint |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup aracısı),</p> <p>[Azure Backup Sunucusu](backup-azure-microsoft-azure-backup.md) (Azure Backup aracısını içerir)</p> |
| Microsoft Exchange |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup aracısı),</p> <p>[Azure Backup Sunucusu](backup-azure-microsoft-azure-backup.md) (Azure Backup aracısını içerir)</p> |
| Azure IaaS VM'ler (Windows) |Azure’da çalışan |[Azure Backup (VM uzantısı)](backup-azure-vms-introduction.md) |
| Azure IaaS VM'ler (Linux) |Azure’da çalışan |[Azure Backup (VM uzantısı)](backup-azure-vms-introduction.md) |

## <a name="linux-support"></a>Linux desteği
Aşağıdaki tabloda Linux destekli Azure Backup bileşenleri gösterilmektedir.  

| Bileşen | Linux (Azure destekli) Desteği |
| --- | --- |
| Azure Backup (MARS) aracısı |Hayır (Yalnızca Windows tabanlı aracı) |
| System Center DPM |Yalnızca Hyper-V üzerinde dosya tutarlı yedekleme<br/> (Azure VM için kullanılamaz) |
| Azure Backup Sunucusu |Yalnızca Hyper-V üzerinde dosya tutarlı yedekleme<br/> (Azure VM için kullanılamaz) |
| Azure IaaS VM Backup |Evet |

## <a name="using-premium-storage-vms-with-azure-backup"></a>Azure Backup ile Premium Depolama VM’leri
Azure Backup, Premium Depolama VM'leri koruma altına alır. Azure Premium Depolama, G/Ç açısından yoğun iş yüklerini desteklemek için tasarlanan katı hal sürücüsü (SSD) tabanlı bir depolama hizmetidir. Premium Depolama, sanal makine (VM) iş yükleri için idealdir. Premium Depolama hakkında daha fazla bilgi almak için bkz. [Premium Depolama: Azure Sanal Makine İş Yükleri için Yüksek Performanslı Depolama](../storage/storage-premium-storage.md)

### <a name="back-up-premium-storage-vms"></a>Premium Storage VM'lerini yedekleme
Premium Storage VM'lerini yedeklerken, Backup hizmeti, Premium Storage hesabında geçici bir hazırlama konumu oluşturur. "AzureBackup-" adlı hazırlama konumu, VM'ye bağlı premium disklerin toplam veri boyutuna eşittir. Depolama hesabında geçici hazırlama konumu için yeterli boş alan olup olmadığını denetleyin. Ek bilgi için [premium depolama sınırlamaları](../storage/storage-premium-storage.md#premium-storage-scalability-and-performance-targets) makalesine bakın.

> [!NOTE]
> Hazırlama konumunu değiştirmeyin veya düzenlemeyin.
>
>

Yedekleme işi tamamlandıktan sonra, hazırlama konumu silinir. Hazırlama konumu için kullanılan depolama alanının fiyatı tüm [Premium depolama fiyatlandırması](../storage/storage-premium-storage.md#pricing-and-billing) ile tutarlıdır.

### <a name="restore-premium-storage-vms"></a>Premium Storage VM'lerini geri yükleme
Premium Depolama VM’leri Premium Depolama veya normal depolama birimine geri yüklenebilir. Premium Storage VM'si kurtarma noktasının Premium Storage'a geri yüklenmesi genel bir geri yükleme işlemidir. Ancak Premium Storage VM'si kurtarma noktasının standart depolamaya geri yüklenmesi uygun maliyetli olabilir. Bu tür bir geri yükleme işlemi, VM'den bir dosya alt kümesine ihtiyaç duymanız halinde kullanılabilir.

## <a name="what-are-the-features-of-each-backup-component"></a>Her bir Azure Backup bileşeninin özellikleri nelerdir?
Aşağıdaki bölümlerde her bir Azure Backup bileşeninin kullanılabilirliğini veya desteklediği özellikleri özetleyen tablolar yer almaktadır. Ek destek bilgileri veya ayrıntılar için her tablonun altındaki bilgileri inceleyin.

### <a name="storage"></a>Depolama
| Özellik | Azure Backup aracısı | System Center DPM | Azure Backup Sunucusu | Azure IaaS VM Backup |
| --- | --- | --- | --- | --- |
| Azure Backup kasası |![Evet][green] |![Evet][green] |![Evet][green] |![Evet][green] |
| Disk depolama | |![Evet][green] |![Evet][green] | |
| Bant depolama | |![Evet][green] | | |
| Sıkıştırma <br/>(Backup kasasında) |![Evet][green] |![Evet][green] |![Evet][green] | |
| Artımlı yedekleme |![Evet][green] |![Evet][green] |![Evet][green] |![Evet][green] |
| Disk için yinelenenleri kaldırma | |![Kısmi][yellow] |![Kısmi][yellow] | |

![tablo anahtarı](./media/backup-introduction-to-azure-backup/table-key.png)

Backup kasası, tüm bileşenler genelinde tercih edilen depolama hedefidir. System Center DPM ve Azure Backup Sunucusu da yerel disk kopyası bulundurma seçeneğini sağlar. Ancak yalnızca System Center DPM, bir bant depolama cihazına veri yazma seçeneği sağlar.

#### <a name="compression"></a>Sıkıştırma
Yedeklemeler, gerekli depolama alanını azaltmak için sıkıştırılır. Sıkıştırma kullanmayan tek bileşen, VM uzantısıdır. VM uzantısı, depolama hesabınızdaki tüm yedekleme verilerini aynı bölgedeki Backup kasasına kopyalar. Veri aktarımı sırasında sıkıştırma yapılmaz. Veriler, sıkıştırma kullanılmadan aktarıldığı için kullanılan depolama alanı biraz fazla olur. Ancak verilerin sıkıştırma olmadan depolanması, ilgili kurtarma noktasına ihtiyaç duymanız halinde geri yüklemenin daha hızlı gerçekleştirilmesini sağlar.

#### <a name="incremental-backup"></a>Artımlı yedekleme
Her bileşen, hedef depolamadan (disk, bant, yedekleme kasası) bağımsız olarak artımlı yedeklemeyi destekler. Artımlı yedekleme, yalnızca son yedeklemeden sonra yapılan değişiklikleri aktararak yedeklemelerin depolama ve zaman açısından verimli olmasını sağlar.

#### <a name="disk-deduplication"></a>Disk İçin Yinelenenleri Kaldırma
[Hyper-V sanal makinesinde](http://blogs.technet.com/b/dpm/archive/2015/01/06/deduplication-of-dpm-storage-reduce-dpm-storage-consumption.aspx) System Center DPM veya Azure Backup Sunucusu dağıttığınızda disk için yinelenenleri kaldırma özelliğinden faydalanabilirsiniz. Windows Server, yedekleme alanı olarak sanal makineye bağlanmış olan sanal makine sabit diskleri (VHD'ler) üzerinde yinelenenleri kaldırma özelliğini kullanır.

> [!NOTE]
> Yinelenenleri kaldırma, Azure Backup bileşenlerinden herhangi biri için kullanılamaz. System Center DPM ve Backup Sunucusu Azure'da dağıtıldığında, VM'ye bağlı depolama diskleri için yinelenenler kaldırılamaz.
>
>

### <a name="security"></a>Güvenlik
| Özellik | Azure Backup aracısı | System Center DPM | Azure Backup Sunucusu | Azure IaaS VM Backup |
| --- | --- | --- | --- | --- |
| Ağ güvenliği<br/> (Azure’a) |![Evet][green] |![Evet][green] |![Evet][green] |![Kısmi][yellow] |
| Veri güvenliği<br/> (Azure’da) |![Evet][green] |![Evet][green] |![Evet][green] |![Kısmi][yellow] |

![tablo anahtarı](./media/backup-introduction-to-azure-backup/table-key.png)

#### <a name="network-security"></a>Ağ güvenliği
Sunucularınızdan Backup kasasına giden tüm yedekleme trafiği, Gelişmiş Şifreleme Standardı 256 kullanılarak şifrelenir. Yedekleme verileri güvenli bir HTTPS bağlantısı üzerinden gönderilir. Ayrıca yedekleme verileri, şifreli biçimde Backup kasasında depolanır. Yalnızca Azure müşterisi olarak siz bu verilerin kilidini açmak için kullanılan parolaya sahip olursunuz. Microsoft herhangi bir noktada yedekleme verilerinin şifresini çözemez.

> [!WARNING]
> Backup kasasını oluşturduktan sonra şifreleme anahtarına yalnızca sizin erişiminiz olur. Microsoft, şifreleme anahtarınızın bir kopyasını saklamaz ve bu anahtara asla erişimi yoktur. Anahtarın kaybedilmesi durumunda Microsoft yedekleme verilerini kurtaramaz.
>
>

#### <a name="data-security"></a>Veri güvenliği
Azure VM'lerin yedeklenmesi için şifrelemenin sanal makine *içinde* ayarlanması gerekir. Windows sanal makinelerde BitLocker'ı ve Linux sanal makinelerde **dm-crypt** özelliğini kullanın. Azure Backup, bu yol üzerinden gelen yedekleme verilerini otomatik olarak şifrelemez.

### <a name="network"></a>Ağ
| Özellik | Azure Backup aracısı | System Center DPM | Azure Backup Sunucusu | Azure IaaS VM Backup |
| --- | --- | --- | --- | --- |
| Ağ sıkıştırma <br/>(**backup sunucusuna**) | |![Evet][green] |![Evet][green] | |
| Ağ sıkıştırma <br/>(**backup kasasına**) |![Evet][green] |![Evet][green] |![Evet][green] | |
| Ağ protokolü <br/>(**backup sunucusuna**) | |TCP |TCP | |
| Ağ protokolü <br/>(**backup kasasına**) |HTTPS |HTTPS |HTTPS |HTTPS |

![tablo anahtarı](./media/backup-introduction-to-azure-backup/table-key-2.png)

VM uzantısı (IaaS VM üzerindeki) verileri doğrudan depolama ağı aracılığıyla Azure depolama hesabından okuduğu için bu trafiğin sıkıştırılması gerekmez.

Verilerinizi System Center DPM veya Azure Backup Sunucusu’na yedekliyorsanız, ana sunucudan yedekleme sunucusuna giden verileri sıkıştırın. Verileri DPM veya Azure Backup Sunucusu’na yedeklemeden önce sıkıştırdığınızda daha az bant genişliği kullanmış olursunuz.

#### <a name="network-throttling"></a>Ağ Kapasitesi Azaltma
Azure Backup aracı, veri aktarımı sırasında ağ bant genişliğinin nasıl kullanıldığını denetlemenizi sağlayan ağ kapasitesi azaltma özelliğine sahiptir. Azaltma özelliği, çalışma saatlerinde veri yedeklemesi yapmanız gerektiğinde ancak yedekleme işleminin diğer İnternet trafiğine engel olmasını istemediğinizde faydalı olabilir. Veri aktarımına yönelik azaltma, yedekleme ve geri yükleme etkinlikleri için geçerlidir.

### <a name="backup-and-retention"></a>Yedekleme ve bekletme

Azure Backup’ta, Backup kasası başına 9999 kurtarma noktası (yedekleme kopyası veya anlık görüntü olarak da bilinir) sınırı vardır. Aşağıdaki tablo, her bileşen için en fazla yedekleme (kasaya) sıklığını gösterir. Kurtarma noktalarını hangi hızla tükettiğiniz, yedekleme ilkesi yapılandırmanız tarafından belirlenir. Örneğin, her gün bir kurtarma noktası oluşturursanız; kurtarma noktalarınızı 27 yıl boyunca bitmeden tutabilirsiniz. Aylık olarak kurtarma noktası alırsanız, kurtarma noktalarınızı 833 yıl boyunca bitmeden tutabilirsiniz. Backup hizmeti, kurtarma noktası üzerinde bir sona erme süresi ayarlamaz.

|  | Azure Backup aracısı | System Center DPM | Azure Backup Sunucusu | Azure IaaS VM Backup |
| --- | --- | --- | --- | --- |
| Yedekleme sıklığı<br/> (Backup kasasına) |Günde üç yedekleme |Günde iki yedekleme |Günde iki yedekleme |Günde bir yedekleme |
| Yedekleme sıklığı<br/> (diske) |Uygulanamaz |<li>SQL Server için 15 dakikada bir <li>Diğer iş yükleri için saatte bir |<li>SQL Server için 15 dakikada bir <li>Diğer iş yükleri için saatte bir</p> |Uygulanamaz |
| Bekletme seçenekleri |Günlük, haftalık, aylık, yıllık |Günlük, haftalık, aylık, yıllık |Günlük, haftalık, aylık, yıllık |Günlük, haftalık, aylık, yıllık |
| Sunucu başına en fazla kurtarma noktası |9999|9999|9999|9999|
| En uzun bekletme süresi |Yedekleme sıklığına bağlıdır |Yedekleme sıklığına bağlıdır |Yedekleme sıklığına bağlıdır |Yedekleme sıklığına bağlıdır |
| Yerel diskteki kurtarma noktaları |Uygulanamaz |<li>Dosya Sunucuları için 64<li>Uygulama Sunucuları için 448 |<li>Dosya Sunucuları için 64<li>Uygulama Sunucuları için 448 |Uygulanamaz |
| Banttaki kurtarma noktaları |Uygulanamaz |Sınırsız |Uygulanamaz |Uygulanamaz |

## <a name="what-is-the-vault-credential-file"></a>Kasa kimlik bilgileri dosyası nedir?
Kasa kimlik bilgileri dosyası, her bir Backup kasası için portal tarafından oluşturulan bir sertifikadır. Portal daha sonra ortak anahtarı Access Control Service'e (ACS) yükler. Özel anahtar, kimlik bilgilerini indirdiğinizde sizinle paylaşılır. Koruma altına alacağınız bilgisayarları kaydetmek için bu anahtarı kullanın. Özel anahtar, sunucuların veya bilgisayarların belirli bir Backup kasasına veri göndermesi için kimlik doğrulamasından geçmesini sağlar.

Kasa kimlik bilgilerini yalnızca sunucuları veya bilgisayarları kaydetmek için kullanırsınız. Ancak, kasa kimlik bilgilerini iyi saklamanız gerekir. Bu anahtarın kaybolması veya başkaları tarafından ele geçirilmesi halinde, kasa kimlik bilgileri aynı kasaya başka makinelerin kaydedilmesi için kullanılabilir. Yedeklenen veriler yalnızca sizin erişebildiğiniz bir parola ile korunduğundan, var olan yedek bilgilerine izinsiz erişim sağlanamaz. Kasa kimlik bilgilerinin süresi 48 sonra dolar. Backup kasasının kimlik bilgilerini dilediğiniz zaman indirebilirsiniz ancak kayıt işlemi için yalnızca en güncel kimlik bilgileri kullanılabilir.

## <a name="how-does-azure-backup-differ-from-azure-site-recovery"></a>Azure Backup'ın Azure Site Recovery'den farkı nedir?
Azure Backup ve Azure Site Recovery, veri yedekleme ve geri yükleme özelliklerine sahip olmaları açısından aynı olsa da temelde sundukları değerler farklıdır.

Azure Backup, şirket içinde ve bulutta yer alan verileri korur. Azure Site Recovery, sanal makine ve fiziksel sunucu çoğaltma, yük devretme ve yeniden çalışma işlemlerini düzenler. Olağanüstü durum kurtarma çözümünüzün verilerinizin güvenli ve kurtarılabilir kalmasını sağlaması (Backup) *ve* kesintiler meydana geldiğinde, iş yüklerinizin kullanılabilirliğini koruması (Site Recovery) gerektiği için her iki hizmet de önemlidir.

Aşağıdaki kavramlar, yedekleme ve olağanüstü durum kurtarma ile ilgili önemli kararları almanıza yardımcı olabilir.

| Kavram | Ayrıntılar | Backup | Olağanüstü durum kurtarma (DR) |
| --- | --- | --- | --- |
| Kurtarma noktası hedefi (RPO) |Kurtarma işleminin gerekli olduğu durumlarda kabul edilebilir veri kaybı miktarı. |Backup çözümleri, kabul edilebilir RPO değerlerinde büyük bir değişkenliğe sahiptir. Sanal makine yedeklemeleri genellikle bir günlük bir RPO'ya sahipken, veritabanı yedeklemelerinin RPO değerleri 15 dakika kadar düşük olabilir. |Olağanüstü durum kurtarma çözümleri düşük RPO'lara sahiptir. DR kopyalama, birkaç saniye veya birkaç dakika geride olabilir. |
| Kurtarma süresi hedefi (RTO) |Bir geri yükleme veya kurtarma işlemini tamamlamak için geçen süre. |Daha büyük RPO nedeniyle, bir yedekleme çözümünün işlemesi gereken veri miktarı genellikle çok daha yüksektir; bu da daha uzun RTO'lara yol açar. Örneğin, bandın şirket dışı bir konumdan taşınması için harcanan süreye bağlı olarak, bantlardan veri geri yükleme işlemi birkaç gün sürebilir. |Olağanüstü durum kurtarma çözümleri, kaynakla aralarındaki eşitlemenin daha yüksek olması nedeniyle daha küçük RTO'lara sahiptir. Daha az değişikliğin işlenmesi gerekir. |
| Bekletme |Verilerin depolanması gereken süre |İşletimsel kurtarma gerektiren senaryolar (veri bozulması, yanlışlıkla dosya silme, işletim sistemi arızası) için, yedekleme verileri genellikle 30 gün veya daha kısa süreyle elde tutulur.<br>Uyumluluk açısından verilerin aylarca ve hatta yıllarca depolanması gerekebilir. Bu gibi durumlarda arşivleme için yedekleme verileri ideal niteliktedir. |Olağanüstü durum kurtarma için yalnızca işletimsel kurtarma verilerine gerek duyulur; bu, genellikle birkaç saat veya en fazla bir gün sürer. DR çözümlerinde kullanılan ayrıntılı veri yakalama işlemi nedeniyle, uzun vadeli bekletme için DR verilerinin kullanılması önerilmez. |

## <a name="next-steps"></a>Sonraki adımlar
Aşağıda yer alan Windows Server üzerindeki verileri veya Azure’daki bir sanal makinede (VM) bulunan verileri koruma altına almayı ayrıntılı ve adım adım gösteren öğreticileri inceleyebilirsiniz:

* [Dosya ve Klasörleri yedekleme](backup-try-azure-backup-in-10-mins.md)
* [Azure Sanal Makinelerini yedekleme](backup-azure-vms-first-look-arm.md)

Diğer iş yüklerini koruma hakkında ayrıntılı bilgi için şu makaleleri inceleyin:

* [Windows Server verilerinizi yedekleme](backup-configure-vault.md)
* [Uygulama iş yüklerini yedekleme](backup-azure-microsoft-azure-backup.md)
* [Azure IaaS VM'lerini yedekleme](backup-azure-vms-prepare.md)

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png



<!--HONumber=Dec16_HO1-->


