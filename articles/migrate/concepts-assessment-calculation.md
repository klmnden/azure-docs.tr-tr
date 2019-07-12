---
title: Azure Geçişi'ndeki değerlendirme hesaplamaları | Microsoft Docs
description: Azure geçişi hizmetinde değerlendirme hesaplamaları genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: raynew
ms.openlocfilehash: a328549307772cbdf470160cc1ad713fe1ee5e05
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67805978"
---
# <a name="assessment-calculations"></a>Değerlendirme hesaplamaları

Azure geçişi Server değerlendirmesi, Azure'a geçiş için şirket içi iş yüklerini değerlendirir. Bu makalede değerlendirmelerin nasıl hesaplandığı hakkında bilgi sağlar.


[Azure geçişi](migrate-services-overview.md) bulma, değerlendirme ve şirket içi uygulamalarınızı ve iş yükleri ve private/public bulut örneklerini azure'a geçişini izlemek için merkezi bir nokta sağlar. Hub araçları Azure geçişi, değerlendirme ve geçiş, ek olarak üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.

## <a name="overview"></a>Genel Bakış

Azure geçişi Server değerlendirmesi değerlendirme üç aşamadan oluşur. Boyutlandırma tarafından izlenen bir uygunluğu analiziyle değerlendirme başlar ve son olarak, bir aylık maliyet tahmini. Öncekine geçerse bir makine yalnızca bir sonraki aşamaya taşır. Azure uygunluk denetimi bir makine başarısız olursa, Azure ve boyutlandırma ve maliyet yapılmaz için örneğin, bunu uygun olarak işaretlenir.


## <a name="whats-in-an-assessment"></a>Bir değerlendirme neleri içerir?

**Özellik** | **Ayrıntılar**
--- | ---
**Hedef konum** | Geçişi yapmak istediğiniz Azure konumu.<br/><br/> Azure geçişi şu anda bu hedef bölgeler destekler: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Orta Hindistan, Orta ABD, Çin Doğu, Kuzey Çin, Doğu Asya, Doğu ABD, Doğu ABD 2, Almanya Orta, Almanya Kuzeydoğu, Doğu, Japonya Batı, Kore Orta, Kore Japonya Güney, Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, Hindistan Güney, UK Güney, UK Batı, ABD Devleti Arizona, ABD Devleti Texas, ABD Devleti Virginia, Batı Orta ABD, Batı Avrupa, Batı Hindistan, Batı ABD ve Batı ABD 2.<br/> Varsayılan hedef bölge, Batı ABD 2 olarak ayarlanır.
**Depolama türü** | Standart HHD diskler/standart SSD disk/Premium.<br/><br/> Disk önerisi, değerlendirme içinde otomatik olarak depolama türü belirttiğinizde, disklerin (IOPS ve aktarım hızı) performans verileri üzerinde temel alır.<br/><br/> Premium veya standart depolama türü belirtirseniz, SKU'nun disk depolama türü içinde bir değerlendirme önerir seçili.<br/><br/> Tek bir örneği sanal makine SLA'sı % 99,9 elde etmek istiyorsanız, depolama türü Premium yönetilen diskler ayarlayabilirsiniz. Ardından tüm değerlendirmesi diskler, Premium yönetilen diskler önerilecektir. <br/> Azure Geçişi yalnızca yönetilen disklerin geçiş değerlendirmesini destekler.<br/> 
**Ayrılmış örnekleri (RI)** | Ayrılmış örnekleriniz varsa bu özelliği belirtin. Maliyet tahminleri değerlendirmede RI indirimleri hesaba katması. Ayrılmış örnekleri şu anda yalnızca Azure geçişi Kullandıkça Öde teklifleri için desteklenir.
**Boyutlandırma ölçütü** | Doğru boyuta VM'ler için kullanılır. Performans tabanlı boyutlandırma olabilir veya as-performans geçmişini dikkate almadan şirket içinde olduğu.
**Performans geçmişi** | VM performansı değerlendirmek için dikkate alınacak süre. Bu özellik yalnızca boyutlandırma performans tabanlı olduğunda geçerlidir.
**Yüzdebirlik kullanımı** | Doğru boyutlandırma VM'ler için kullanılan performans örnek yüzdelik dilim değeri. Bu özellik yalnızca boyutlandırma performans tabanlı olduğunda geçerlidir.
**VM serisi** | VM serisi, boyut tahmini için kullanılır. Örneğin, Azure’da A serisi VM’lere geçirmeyi planlamadığınız bir üretim ortamınız varsa, A serisini liste veya serilerin dışında bırakabilirsiniz. Boyutlandırma yalnızca seçili serilerde yapılır.
**Konfor katsayısı** | Azure geçişi Server değerlendirmesi değerlendirme sırasında bir Tamponu (konfor katsayısı) göz önünde bulundurur. Bu tampon, VM’lerin makine kullanım verilerinin (CPU, bellek, disk ve ağ) üzerine uygulanır. Konfor katsayısı; sezona özgü kullanım, kısa performans geçmişi ve gelecek kullanımlarda oluşabilecek artışlar gibi konuları hesaba katar.<br/><br/> Örneğin, %20 kullanıma sahip 10 çekirdekli bir VM normalde 2 çekirdekli VM ile sonuçlanır. Ancak, 2.0x konfor katsayısı ile sonuç 4 çekirdekli VM olur.
**Teklif** | Kaydolduğunuz [Azure teklifi](https://azure.microsoft.com/support/legal/offer-details/). Azure Geçişi, buna göre bir maliyet tahmini oluşturur.
**Para Birimi** | Fatura para birimi. 
**İndirim (%)** | Azure teklifinin yanı sıra aldığınız, aboneliğe özgü indirim.<br/> Varsayılan ayar, %0’dır.
**VM çalışma süresi** | Sanal makinelerinizi Azure'da 7/24 çalıştırılması kullanmayacaksanız, bunlar çalıştırıyordur için süresi (gün / ay sayısı) ve her gün saat sayısı belirtebilirsiniz ve maliyet tahminleri buna göre uygulanır.<br/> 31 gün / ay ve günde 24 saat varsayılan değerdir.
**Azure Hibrit Avantajı** | Yazılım Güvencesine sahip ve için uygun olup olmadığını belirtir [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Evet olarak ayarlanırsa, Windows sanal makineleri için Windows olmayan Azure fiyatları dikkate alınır. 



## <a name="azure-suitability-analysis"></a>Azure uygunluk analizi

Tüm makineler, Azure'da çalıştırmak için uygundur. Azure geçişi Server değerlendirmesi, her geçiş için şirket içi makinede değerlendirir ve makinelerin uygunluğunu kategoriden birine kategorilere ayırır:
- **Azure için hazır** -makine olarak geçirilebilir-hiçbir değişiklik yapmadan azure'dur. Tam Azure desteği ile Azure'da önyükleme yapmaz.
- **Azure için koşullu olarak hazır** -makine Azure'da önyüklenebilir ancak Azure tam desteğine sahip olmayabilir. Örneğin, daha eski Windows Server işletim sistemi sürümüne sahip bir makine Azure'da desteklenmiyor. Değerlendirmesi geçiş yapmadan önce hazır olma durumu sorunlarını gidermek için önerilen düzeltme yönergeleri izleyin ve bu makinelerin Azure'a geçiş yapmadan önce dikkat gerekir.
- **Azure için hazır değil** -makine Azure'da önyükleme yapmaz. Örneğin, bir şirket içi makinede bir diski boyutunun 4 TB bağlı birden fazla varsa, Azure üzerinde barındırılamaz. Azure'a geçiş yapmadan önce hazırlık sorunu düzeltmek için değerlendirme önerilen düzeltme yönergeleri gerekir. Doğru boyutlandırma ve maliyet tahmini, Azure için hazır değil olarak işaretlenir makineler için yapılmaz.
- **Hazır olma durumu bilinmiyor** -Azure geçişi vCenter Server'da mevcut yetersiz veri nedeniyle makine hazır olma bulamadı.



### <a name="machine-properties"></a>Makine özellikleri

Azure geçişi şirket içi VM'nin Azure'da çalıştırabilirsiniz olup olmadığını belirlemek için aşağıdaki özellikleri gözden geçirir.

**Özellik** | **Ayrıntılar** | **Azure için hazır olma durumu**
--- | --- | ---
**Önyükleme türü** | Azure, BIOS ve UEFI değil olarak önyükleme türündeki sanal makineleri destekler. | Önyükleme türü UEFI ise koşullu olarak hazır.
**Çekirdek** | Çekirdek makinelerde eşit veya çekirdek (128 çekirdek) bir Azure sanal makinesi için desteklenen en fazla sayısından daha az olmalıdır.<br/><br/> Performans geçmişi kullanılabilir haldeyse, Azure geçişi, karşılaştırma için kullanılan çekirdek olarak değerlendirir. Konfor katsayısı değerlendirme ayarlarında belirtilmişse konfor katsayısı tarafından kullanılan çekirdek sayısı çarpılır.<br/><br/> Performans geçmişi yok ise, Azure geçişi, konfor katsayısı uygulamadan ayrılmış çekirdekler kullanır. | Hazır değilse, küçüktür veya eşittir sınırları.
**Bellek** | Makine bellek boyutu eşit veya en yüksek belleğinden daha az olmalıdır (Azure M serisi Standard_M128m 3892 GB&nbsp;<sup>2</sup>) bir Azure sanal makine için izin verilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).<br/><br/> Performans geçmişi kullanılabilir haldeyse, Azure geçişi, karşılaştırma için kullanılan bellek kabul eder. Konfor katsayısı belirtilirse, kullanılan bellek konfor katsayısı ile çarpılır.<br/><br/> Geçmiş yok ise konfor katsayısı uygulamadan ayrılan bellek kullanılır.<br/><br/> | Varsa sınırları içinde hazır.
**Depolama diski** | Ayrılmış bir diskin boyutu 4 TB (4096 GB) olması gerekir ya da daha az.<br/><br/> Makineye bağlı disk sayısı, 65 veya less, işletim sistemi diskini dahil olmak üzere olması gerekir. | Varsa sınırları içinde hazır.
**Ağ** | Daha az NIC bağlı veya bir makine 32 olmalıdır. | Varsa sınırları içinde hazır.

### <a name="guest-operating-system"></a>Konuk işletim sistemi
VM özellikleriyle birlikte, konuk işletim sistemi Azure'da çalıştırabilirsiniz olup olmadığını belirlemek için makineleri Azure geçişi Server değerlendirmesi bakar.

> [!NOTE]
> Azure geçişi Server değerlendirmesi için vcenter sunucusu analizi için VM belirtilen işletim sistemi kullanır. Gereç tabanlı VM bulma için Azure geçişi Server değerlendirmesi ve sanal makinede çalışan işletim sisteminin vCenter Server'da belirtilenle aynı olup olmadığını doğrulayamıyor.

Aşağıdaki mantık, Azure geçişi Server değerlendirmesi tarafından işletim sistemini temel alan Azure için hazır olma tanımlamak için kullanılır.

**İşletim Sistemi** | **Ayrıntılar** | **Azure için hazır olma durumu**
--- | --- | ---
Windows Server 2016 ve tüm Sp'ler | Azure, tam destek sağlar. | Azure için hazır
Windows Server 2012 R2 & tüm Sp'ler | Azure, tam destek sağlar. | Azure için hazır
Windows Server 2012 & tüm Sp'ler | Azure, tam destek sağlar. | Azure için hazır
Windows Server 2008 R2 ile tüm Sp'ler | Azure, tam destek sağlar.| Azure için hazır
Windows Server 2008 (32-bit ve 64-bit) | Azure, tam destek sağlar. | Azure için hazır
Windows Server 2003, 2003 R2 | Bu işletim sistemlerinin kendi destek tarihi ve gereksinim sonuna başarılı bir [özel destek anlaşması (CSA)](https://aka.ms/WSosstatement) Azure desteği için. | Koşullu olarak Azure için hazır, Azure'a geçiş yapmadan önce işletim sistemini yükseltmeyi düşünün.
Windows 2000, 98, 95 ' NT 3.1, MS-DOS | Bu işletim sistemlerinin kendi destek sonu tarihi geçtiğinde, makine Azure'da önyüklenebilir ancak Azure tarafından işletim sistemi desteği sağlanmaz. | Azure için koşullu olarak hazır, Azure'a geçiş yapmadan önce işletim sistemi yükseltmesi önerilir.
Windows istemci 7, 8 ve 10 | Azure desteği ile sağlar [yalnızca Visual Studio aboneliği.](https://docs.microsoft.com/azure/virtual-machines/windows/client-images) | Azure için koşullu olarak hazır
Windows 10 Pro Masaüstü | Azure desteği ile sağlar [çok Kiracılı barındırma hakları.](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment) | Azure için koşullu olarak hazır
Windows Vista, XP Professional | Bu işletim sistemlerinin kendi destek sonu tarihi geçtiğinde, makine Azure'da önyüklenebilir ancak Azure tarafından işletim sistemi desteği sağlanmaz. | Azure için koşullu olarak hazır, Azure'a geçiş yapmadan önce işletim sistemi yükseltmesi önerilir.
Linux | Azure bu onayladığı [Linux işletim sistemleri](../virtual-machines/linux/endorsed-distros.md). Diğer Linux işletim sistemleri, Azure'da önyüklenebilir ancak Azure'a geçiş yapmadan önce işletim Sisteminin desteklenen bir sürüme yükseltmek için tavsiye edilir. | Sürüm desteklenen, Azure için hazır.<br/><br/>Sürüm değil onaylı koşullu olarak hazır.
Diğer işletim sistemleri<br/><br/> Örneğin, Oracle Solaris Apple Mac işletim sistemi vb., FreeBSD, vs. | Azure, bu işletim sistemlerini desteklemez. Makine Azure'da önyüklenebilir ancak Azure tarafından işletim sistemi desteği sağlanır. | Koşullu olarak Azure için hazır, Azure'a geçiş yapmadan önce desteklenen bir işletim sistemini yüklemeniz önerilir.  
İşletim sistemi olarak belirtilen **diğer** vcenter sunucusu | Azure geçişi, bu durumda işletim sistemi tanımlanamıyor. | Hazırlık bilinmiyor. Azure'da sanal makine içinde çalışan işletim Sisteminin desteklendiğinden emin olmak.
32 bit işletim sistemleri | Makine Azure'da önyüklenebilir ancak Azure tam destek sağlamaz. | Koşullu olarak Azure için hazır, Azure'a geçiş yapmadan önce makinenin işletim sistemini 32-bit işletim sistemi 64-bit işletim sistemine yükseltmeniz önerilir.

## <a name="sizing"></a>Boyutlandırma

Bir makine, Azure için hazır olarak işaretlendikten sonra Azure Geçişi sanal makine ve disklerine için Azure boyutları.

- Değerlendirme performansa dayalı boyutlandırma kullanıyorsa, Azure geçişi, makinenin Azure VM boyutu ve disk türünü tanımlamak için performans geçmişi dikkate alır. Bu yöntem, şirket içi VM, fazladan ayrıldı ancak kullanımı düşüktür ve maliyet tasarrufu için azure'da VM için doğru boyutu istiyorsanız özellikle yardımcı olur.
- Size bir olarak kullanarak şirket içi değerlendirmesi, Azure geçişi Server değerlendirmesi kullanımı verilerini göz önüne almadan şirket ayarlarını temel alan VM boyutu. Disk boyutlandırma, bu durumda, depolama türü (standart disk veya Premium disk) değerlendirme özelliklerinde belirttiğiniz temel alır.

### <a name="performance-based-sizing"></a>Performansa dayalı boyutlandırma

Performansa dayalı boyutlandırma için Azure geçişi ağ bağdaştırıcıları tarafından izlenen VM, bağlı diskler ile başlar ve ardından eşlemeleri bir Azure sanal makinesi şirket içi VM hesaplama gereksinimlerine göre temel.

- **Depolama**: Azure geçişi, azure'da bir diski makineye bağlı her disk eşlemek çalışır.
    - Etkin disk g/ç (IOPS) ve üretilen iş (MBps) başına almak için Azure geçişi disk IOPS ve aktarım hızı konfor katsayısı ile çarpar. Disk, azure'daki standart veya premium diske eşlenmelidir etkili IOPS ve aktarım hızı değerleri bağlı olarak, Azure geçişi belirtir.
    - Azure geçişi gereken IOPS ve aktarım hızı sahip bir diski bulamazsanız, Azure için makine uyumsuz olarak işaretler. [Daha fazla bilgi edinin](../azure-subscription-service-limits.md#storage-limits) Azure hakkında disk ve VM sınırlar.
    - Azure geçişi, uygun bir diskler kümesi bulursa, depolama yedekliliği yöntemi ve değerlendirme ayarlarında belirtilen konuma destekleyen olanları seçer.
    - Birden fazla uygun disk varsa, düşük maliyetli bir seçer.
    - Diskler için performans verilerini kullanılamıyorsa, tüm disklerin standart diskler azure'da eşlenir.

- **Ağ**: Azure geçişi, şirket içi makineye bağlı ağ bağdaştırıcıları ve bu ağ bağdaştırıcıları tarafından gerekli performansı sayısını destekleyen bir Azure VM bulmayı dener.
    - Şirket içi sanal makinenin en etkili ağ performansı elde etmek Azure geçişi, tüm ağ bağdaştırıcıları (ağ) kullanıma makine dışında (MBps) saniye başına aktarılan verileri toplar ve konfor katsayısı uygular. Bu sayı, gerekli ağ performansı destekleyen bir Azure VM bulmak için kullanılır.
    - Ağ performansı ile birlikte, ayrıca Azure VM gerekli destekleyip desteklemediğini dikkate ağ bağdaştırıcılarının sayısı.
    - Ağ performansı verileri kullanılabilir ise, yalnızca ağ bağdaştırıcıları sayısı için VM boyutu olarak kabul edilir.

- **İşlem**: Depolama ve ağ gereksinimleri hesaplandıktan sonra Azure geçişi, Azure'da uygun bir VM boyutu bulmak için CPU ve bellek gereksinimleri dikkate alır.
    - Azure geçişi, kullanılan çekirdek ve bellek arar ve konfor katsayısı etkili çekirdek ve bellek almak için geçerlidir. Bu sayıya göre Azure'da uygun bir VM boyutu bulmak çalışır.
    - Uygun boyut bulunursa, makine uygunsuz Azure için işaretlenir.
    - Azure geçişi, uygun bir boyut bulunursa, depolama ve ağ hesaplamalar geçerlidir. Ardından, konum ve fiyatlandırma katmanı ayarları, son sanal makine boyutu önerisi için de geçerlidir.
    - Birden fazla uygun Azure sanal makinesi boyutu varsa, en düşük maliyetli olan önerilir.

### <a name="as-on-premises-sizing"></a>Boyutlandırma şirket

Şirket içi boyutlandırma olarak kullanırsanız, bir VM SKU boyutu şirket içi tabanlı azure'da Server değerlendirmesi ayırır. Benzer şekilde, disk boyutlandırma için bunu depolama türü (standart/Premium) değerlendirme özelliklerinde belirtilen bakar ve disk türü uygun şekilde önerir. Varsayılan depolama Premium diskler türüdür.

## <a name="confidence-ratings"></a>Güvenle derecelendirmeleri
Azure Geçişi'ndeki her bir performans tabanlı değerlendirme beş başlatır (yüksek) diğerine (en düşük) değişen bir güvenilirlik derecesiyle ilişkilendirilir.
- Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliği temelinde bir değerlendirmeye atanır.
- Bir değerlendirmenin güvenilirlik derecesi, Azure Geçişi tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur.
- Güvenilirlik derecelendirmesi şirket değerlendirmeleri gibi için geçerli değildir.
- Performansa dayalı boyutlandırma için Azure geçişi Server değerlendirmesi gerekir:
    - CPU kullanımı verilerini ve VM bellek.
    - Bunlara ek olarak, VM'ye eklenen her disk için disk IOPS ve aktarım hızı değerleri de gerekir.
    - Benzer şekilde bir VM'ye bağlı her ağ bağdaştırıcısı için performansa dayalı boyutlandırma gerçekleştirmek amacıyla ağ g/ç güvenilirlik derecelendirmesi gerekir.
    - Yukarıdaki kullanım rakamlarından herhangi birini vCenter Server'da mevcut değilse, boyut önerisi güvenilir olmayabilir. 

Kullanılabilir veri noktalarının yüzdesine bağlı olarak değerlendirme için aşağıdaki gibi güvenilirlik derecelendirmesi sağlanır:

   **Veri noktalarının kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
   --- | ---
   %0-%20 | 1 Yıldız
   %21-%40 | 2 Yıldız
   %41-%60 | 3 Yıldız
   %61-%80 | 4 Yıldız
   %81-%100 | 5 Yıldız

### <a name="low-confidence-ratings"></a>Düşük güven derecelendirmeleri

Birkaç nedenleri neden, düşük güvenilirlik derecelendirmesi değerlendirme alabilir:

- Değerlendirme oluşturma süresi boyunca ortamınızın profilini kaydetmedi. Örneğin, performans süresi 1 gün olarak ayarlayın, değerlendirmesi oluşturursanız, için en az toplanan almak tüm veri noktaları için bulma başlattıktan sonra bir gün beklemeniz gerekir.
- Değerlendirmenin hesaplandığı dönem boyunca bazı VM'ler kapatılmıştır. Herhangi bir VM için bazı süresi kapalı, Azure geçişi Server değerlendirmesi, bu dönem için performans verilerini toplayamazsınız.
- Değerlendirmenin hesaplandığı dönem arasında bazı VM'ler oluşturulur. Örneğin, son bir ayın performans geçmişi için değerlendirme oluşturun, ancak bazı VM'ler yalnızca bir hafta önce ortamda oluşturulmuş olan yeni sanal makinelerin performans geçmişi yok sürenin tamamı boyunca olmayacaktır.

  > [!NOTE]
  > Herhangi bir değerlendirmenin güvenilirlik derecesi beş yıldızdan düşükse, en az gereç ortamı profil bir gün bekleyin ve ardından değerlendirmeyi yeniden hesaplamak öneririz. Aksi takdirde, performansa dayalı boyutlandırma güvenilir olmayabilir ve biz ve şirket içi boyutlandırma olarak kullanılmak üzere değerlendirme geçiş önerilir.
  
## <a name="monthly-cost-estimation"></a>Aylık maliyet tahmini

Boyutlandırma önerileri tamamlandıktan sonra Azure geçişi geçişten sonra işlem ve depolama maliyetleri için hesaplar.

- **İşlem maliyetine**: Önerilen Azure VM boyutu kullanarak, Azure geçişi faturalandırma API'si bir VM için aylık maliyeti hesaplamak için kullanır.
    - Hesaplama, işletim sistemi, Yazılım Güvencesi, ayrılmış örnekler, VM çalışma süresi, konum ve para birimi ayarlarını dikkate alır.
    - Toplam aylık işlem maliyeti hesaplamak için tüm makineler arasında maliyeti araya getirir.
- **Depolama maliyeti**: Aylık depolama maliyeti bir makine için aylık maliyet makineye bağlı tüm disklerin toplayarak hesaplanır
    - Azure geçişi Server değerlendirmesi, aylık toplam depolama maliyetlerini tüm makinelerin depolama maliyetlerini toplayarak hesaplar.
    - Şu anda, hesaplama değerlendirme ayarlarını göz önüne belirtilen teklifler almaz.

Maliyetleri değerlendirme ayarlarında belirtilen para birimi görüntülenir.


## <a name="next-steps"></a>Sonraki adımlar

Değerlendirme için oluşturmak [VMware Vm'lerini](tutorial-assess-vmware.md) veya [Hyper-V Vm'lerini](tutorial-assess-hyper-v.md).
