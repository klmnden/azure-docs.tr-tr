---
title: Azure Geçişi'ndeki değerlendirme hesaplamaları | Microsoft Docs
description: Azure geçişi hizmetinde değerlendirme hesaplamaları genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: raynew
ms.openlocfilehash: 012a352b00de2e2d1bf64fd18125ddd10faba5cd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60679128"
---
# <a name="assessment-calculations"></a>Değerlendirme hesaplamaları

[Azure geçişi](migrate-overview.md) Azure'a geçiş için şirket içi iş yüklerini değerlendirir. Bu makalede değerlendirmelerin nasıl hesaplandığı hakkında bilgi sağlar.


## <a name="overview"></a>Genel Bakış

Azure geçişi değerlendirmesi, üç aşamadan oluşur. Boyutlandırma tarafından izlenen bir uygunluğu analiziyle değerlendirme başlar ve son olarak, bir aylık maliyet tahmini. Öncekine geçerse bir makine yalnızca bir sonraki aşamaya taşır. Azure uygunluk denetimi bir makine başarısız olursa, Azure ve boyutlandırma ve maliyet yapılmaz için örneğin, bunu uygun olarak işaretlenir.

## <a name="azure-suitability-analysis"></a>Azure uygunluk analizi

Tüm makineler, bulut kendi kısıtlamaları ve gereksinimleri olduğu gibi bulutta çalıştırmak için uygundur. Azure geçişi, her şirket içi makine için azure'a geçiş uygunluğunu değerlendirir ve makineler aşağıdaki kategorilerden birine kategorilere ayırır:
- **Azure için hazır** -makine olarak geçirilebilir-hiçbir değişiklik yapmadan azure'dur. Tam Azure desteği ile Azure'da önyükleme yapmaz.
- **Azure için koşullu olarak hazır** -makine Azure'da önyüklenebilir ancak Azure tam desteğine sahip olmayabilir. Örneğin, daha eski Windows Server işletim sistemi sürümüne sahip bir makine Azure'da desteklenmiyor. Değerlendirmesi geçiş yapmadan önce hazır olma durumu sorunlarını gidermek için önerilen düzeltme yönergeleri izleyin ve bu makinelerin Azure'a geçiş yapmadan önce dikkat gerekir.
- **Azure için hazır değil** -makine Azure'da önyükleme yapmaz. Örneğin, bir şirket içi makinede bir diski boyutunun 4 TB bağlı birden fazla varsa, Azure üzerinde barındırılamaz. Azure'a geçiş yapmadan önce hazırlık sorunu düzeltmek için değerlendirme önerilen düzeltme yönergeleri gerekir. Doğru boyutlandırma ve maliyet tahmini, Azure için hazır değil olarak işaretlenir makineler için yapılmaz.
- **Hazır olma durumu bilinmiyor** -Azure geçişi vCenter Server'da mevcut yetersiz veri nedeniyle makine hazır olma bulamadı.

Azure geçişi, şirket içi makinenin Azure için hazır olma tanımlamak için konuk işletim sistemi ve makine özelliklerini inceler.

### <a name="machine-properties"></a>Makine özellikleri
Azure geçişi, şirket içi VM, Azure üzerinde bir VM çalıştırıp çalıştıramayacağını tanımlamak için aşağıdaki özellikleri gözden geçirir.

**Özellik** | **Ayrıntılar** | **Azure için hazır olma durumu**
--- | --- | ---
**Önyükleme türü** | Azure, BIOS ve UEFI değil olarak önyükleme türündeki sanal makineleri destekler. | Önyükleme türü UEFI ise koşullu olarak hazır.
**Çekirdek** | Çekirdek makinelerde eşit veya çekirdek (128 çekirdek) bir Azure sanal makinesi için desteklenen en fazla sayısından daha az olmalıdır.<br/><br/> Performans geçmişi kullanılabilir haldeyse, Azure geçişi, karşılaştırma için kullanılan çekirdek olarak değerlendirir. Konfor katsayısı değerlendirme ayarlarında belirtilmişse konfor katsayısı tarafından kullanılan çekirdek sayısı çarpılır.<br/><br/> Performans geçmişi yok ise, Azure geçişi, konfor katsayısı uygulamadan ayrılmış çekirdekler kullanır. | Hazır değilse, küçüktür veya eşittir sınırları.
**Bellek** | Makine bellek boyutu eşit veya en yüksek belleğinden daha az olmalıdır (Azure M serisi Standard_M128m 3892 GB&nbsp;<sup>2</sup>) bir Azure sanal makine için izin verilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).<br/><br/> Performans geçmişi kullanılabilir haldeyse, Azure geçişi, karşılaştırma için kullanılan bellek kabul eder. Konfor katsayısı belirtilirse, kullanılan bellek konfor katsayısı ile çarpılır.<br/><br/> Geçmiş yok ise konfor katsayısı uygulamadan ayrılan bellek kullanılır.<br/><br/> | Varsa sınırları içinde hazır.
**Depolama diski** | Ayrılmış bir diskin boyutu 4 TB (4096 GB) olması gerekir ya da daha az.<br/><br/> Makineye bağlı disk sayısı, 65 veya less, işletim sistemi diskini dahil olmak üzere olması gerekir. | Varsa sınırları içinde hazır.
**Ağ** | Daha az NIC bağlı veya bir makine 32 olmalıdır. | Varsa sınırları içinde hazır.

### <a name="guest-operating-system"></a>Konuk işletim sistemi
VM özellikleriyle birlikte Azure geçişi ayrıca VM, Azure üzerinde çalışıp çalışamayacağını tanımlamak için şirket içi sanal makinenin konuk işletim sistemi inceler.

> [!NOTE]
> Azure geçişi, vCenter Server'da şu çözümlemenin yapmak için belirtilen işletim sistemi olarak değerlendirir. Azure geçişi tarafından yapılan keşif alet tabanlı olduğundan, sanal makine içinde çalışan işletim Sisteminin bir belirtilen vCenter sunucusu aynı olup olmadığını doğrulamak için bir yol yoktur.

Aşağıdaki mantık, Azure geçişi tarafından işletim sistemi temelinde VM'nin Azure için hazır olma tanımlamak için kullanılır.

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

Bir makine, Azure için hazır olarak işaretlendikten sonra Azure Geçişi sanal makine ve disklerine için Azure boyutları. Değerlendirme özelliklerinde belirtilen boyutlandırma ölçütü performansa dayalı boyutlandırma gerçekleştirmek amacıyla ise, Azure geçişi, makinenin Azure VM boyutu ve disk türünü tanımlamak için performans geçmişini dikkate alır. Bu yöntem, burada şirket içi VM fazladan ayrıldı ancak kullanımı düşüktür ve maliyetinden tasarruf etmek Azure sanal makineleri doğru boyuta istediğiniz senaryolarda yararlıdır.

VM boyutu için performans geçmişini göz önünde bulundurun ve VM olarak almak istiyorsanız istemiyorsanız-olan, Azure'a boyutlandırma ölçütü olarak belirtebilirsiniz *şirket içi olarak* ve Azure geçişi ardından boyut şirket içi sanal makinelerinizi kullanım verileri dikkate almadan yapılandırması. Disk boyutlandırma, bu durumda, (standart disk veya Premium disk) değerlendirme özelliklerinde belirttiğiniz depolama türüne göre yapılır

### <a name="performance-based-sizing"></a>Performansa dayalı boyutlandırma

Performansa dayalı boyutlandırma için Azure geçişi başlatır VM'ye bağlı diskleri olan ağ bağdaştırıcıları tarafından izlenen ve şirket içi VM hesaplama gereksinimlerine göre'da bir Azure VM eşlemeleri temel.

- **Depolama**: Azure geçişi, azure'da bir diski makineye bağlı her disk eşlemek çalışır.

    > [!NOTE]
    > Yalnızca yönetilen diskleri değerlendirmesi için Azure geçişi destekler.

    - Etkin disk g/ç (IOPS) ve üretilen iş (MBps) başına almak için Azure geçişi disk IOPS ve aktarım hızı konfor katsayısı ile çarpar. Disk, azure'daki standart veya premium diske eşlenmelidir etkili IOPS ve aktarım hızı değerleri bağlı olarak, Azure geçişi belirtir.
    - Azure geçişi gereken IOPS ve aktarım hızı sahip bir diski bulamazsanız, Azure için makine uyumsuz olarak işaretler. [Daha fazla bilgi edinin](../azure-subscription-service-limits.md#storage-limits) Azure hakkında disk ve VM sınırlar.
    - Azure geçişi, uygun bir diskler kümesi bulursa, depolama yedekliliği yöntemi ve değerlendirme ayarlarında belirtilen konuma destekleyen olanları seçer.
    - Birden fazla uygun disk varsa, düşük maliyetli bir seçer.
    - Performans verileri için diskler, tüm diskleri azure'da standart diskler için kullanılamaz, eşlenir.

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
Boyutlandırma ölçütü ise *şirket içi olarak boyutlandırma*, Azure geçişi, VM'ler ve diskleri performans geçmişini dikkate almaz ve şirket içi ayrılan boyutuna göre azure'da bir sanal makine SKU'su ayırır. Benzer şekilde, disk boyutlandırma için bunu depolama türü (standart/Premium) değerlendirme özelliklerinde belirtilen bakar ve disk türü uygun şekilde önerir. Premium diskler varsayılan depolama türüdür.

### <a name="confidence-rating"></a>Güvenilirlik derecelendirmesi
Azure Geçişi’ndeki her performans tabanlı değerlendirme 1 yıldız ile 5 yıldız (1 yıldız en düşük, 5 yıldız en yüksektir) arasında değişen bir güvenilirlik derecesiyle ilişkilendirilir. Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliği temelinde bir değerlendirmeye atanır. Bir değerlendirmenin güvenilirlik derecesi, Azure Geçişi tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur. Güvenilirlik derecelendirmesi, şirket içi değerlendirmeler için geçerli değildir.

Performansa dayalı boyutlandırma için Azure Geçişi, CPU ile VM'nin belleği için kullanım verilerine ihtiyaç duyar. Bunlara ek olarak, VM'ye eklenen her disk için disk IOPS ve aktarım hızı değerleri de gerekir. Sanal makineye eklenmiş her bir ağ bağdaştırıcısı için benzer şekilde Azure Geçişi’nin performansa dayalı boyutlandırma gerçekleştirmek amacıyla ağ giriş/çıkışına ihtiyacı vardır. Yukarıdaki kullanım rakamlarından herhangi biri vCenter Server’da mevcut değilse Azure Geçişi’nin yaptığı boyut önerisi güvenilir olmayabilir. Kullanılabilir veri noktalarının yüzdesine bağlı olarak değerlendirme için aşağıdaki gibi güvenilirlik derecelendirmesi sağlanır:

   **Veri noktalarının kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
   --- | ---
   %0-%20 | 1 Yıldız
   %21-%40 | 2 Yıldız
   %41-%60 | 3 Yıldız
   %61-%80 | 4 Yıldız
   %81-%100 | 5 Yıldız

   Değerlendirme, düşük güvenilirlik derecelendirmesi alabilir neden aşağıdaki nedenlerle ilgili:

- Değerlendirmeyi oluşturduğunuz süre boyunca ortamınızın profilini oluşturmadınız. Örneğin, değerlendirmeyi, 1 gün olarak ayarlanmış performans süresiyle oluşturuyorsanız, toplanacak tüm veri noktalarının keşfini başlattıktan sonra en az bir gün beklemeniz gerekir.

- Değerlendirmenin hesaplandığı dönem boyunca birkaç sanal makine kapatılmıştır. Herhangi bir VM, belirli bir süre boyunca kapatıldıysa o süreye ait performans verilerine sahip olamayız.

- Değerlendirmenin hesaplandığı dönem boyunca birkaç sanal makine oluşturulmuştur. Örneğin, son bir ayın performans geçmişi için değerlendirme oluşturuyorsanız, ancak yalnızca bir hafta önce ortamda birkaç sanal makine oluşturulduysa. Bu tür durumlarda yeni sanal makinelerin performans geçmişi, sürenin tamamı boyunca mevcut olmaz.

  > [!NOTE]
  > Herhangi bir değerlendirmenin güvenilirlik derecesi 5 yıldızdan düşükse, gereç ortamı profilini oluşturmak için en az bir gün beklemeniz önerilir ve ardından *yeniden hesapla* değerlendirme. Bu yapılamazsa, performansa dayalı boyutlandırma güvenilir olmayabilir ve değerlendirme özellikleri değiştirilerek *şirket içi olarak boyutlandırmaya* geçiş yapılması önerilir.

## <a name="monthly-cost-estimation"></a>Aylık maliyet tahmini

Boyutlandırma önerileri tamamlandıktan sonra Azure geçişi, geçiş sonrası işlem ve depolama maliyetlerini hesaplar.

- **İşlem maliyetine**: Önerilen Azure VM boyutu kullanarak, Azure geçişi faturalandırma API'si bir VM için aylık maliyeti hesaplamak için kullanır. Hesaplama, işletim sistemi, Yazılım Güvencesi, ayrılmış örnekler, VM çalışma süresi, konum ve para birimi ayarlarını dikkate alır. Toplam aylık işlem maliyeti hesaplamak için tüm makineler arasında maliyeti araya getirir.
- **Depolama maliyeti**: Aylık depolama maliyeti bir makine için aylık maliyet makineye bağlı tüm disklerin toplayarak hesaplanır. Azure geçişi, aylık toplam depolama maliyetlerini tüm makinelerin depolama maliyetlerini toplayarak hesaplar. Şu anda, hesaplama değerlendirme ayarlarını göz önüne belirtilen teklifler almaz.

Maliyetleri değerlendirme ayarlarında belirtilen para birimi görüntülenir.


## <a name="next-steps"></a>Sonraki adımlar

[Şirket içi VMware Vm'leri için bir değerlendirme oluşturun](tutorial-assessment-vmware.md)
