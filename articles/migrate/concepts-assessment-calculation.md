---
title: Azure geçirmek değerlendirme hesaplamalarda | Microsoft Docs
description: Değerlendirme hesaplamalar Azure geçirmek hizmetindeki genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: raynew
ms.openlocfilehash: be4fb15d96f5598d4b1ddbbaa4befe7f6530152c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="assessment-calculations"></a>Değerlendirme hesaplamaları

[Azure geçirme](migrate-overview.md) geçiş Azure için şirket içi iş yüklerini değerlendirir. Bu makalede değerlendirmelerinin nasıl hesaplandığını hakkında bilgi sağlar.


## <a name="overview"></a>Genel bakış

Bir Azure geçirmek değerlendirme üç aşamadan oluşur. Boyutlandırma tarafından izlenen uygunluğu çözümleme, değerlendirme başlar ve son olarak, aylık tahmini maliyet. Öncekinin geçerse bir makine yalnızca bir sonraki aşamaya taşır. Bir makine Azure uygunluğu denetimi başarısız olursa, Azure ve boyutlandırma ve maliyetlendirme yapılmaz için örneğin, uygun değil olarak işaretlendiğinden.


## <a name="azure-suitability-analysis"></a>Azure uygunluğu çözümleme

Tüm makineler, bulut kendi kısıtlamalar ve gereksinimler taşıdığından bulutta çalıştırmak için uygundur. Azure geçirme her bir şirket içi makine Azure geçiş uygunluğu için değerlendirir ve makineler aşağıdaki kategoriden kategorilere ayırır:
- **Azure için hazır** -makine olarak geçirilebilir-Azure için hiçbir değişiklik yapmadan değil. Tam Azure desteği ile Azure'da önyükleme yapmaz.
- **Koşullu olarak Azure için hazır** -makine Azure'da önyükleme, ancak tam Azure desteğine sahip değil. Örneğin, Windows Server işletim Sisteminin daha eski bir sürümüne sahip bir makine Azure'da desteklenmiyor. Azure için bu makineleri geçirmeden önce dikkatli olun ve değerlendirmesi, geçirmeden önce hazır olma durumu sorunlarını gidermek için önerilen düzeltme yönergeleri izleyin gerekir.
- **Azure için hazır değil** -makine Azure'da önyükleme yapmaz. Örneğin, bir şirket içi makinede bir diski boyutunun 4 TB bağlı olandan daha fazla varsa, Azure üzerinde barındırılamaz. Değerlendirme için Azure geçirmeden önce hazırlık sorunu düzeltmek için önerilen düzeltme yönergeleri izlemeniz gerekir. Azure için hazır değil olarak işaretlenmiş makineler için boyutlandırmaya ve maliyet tahmini yapılmamaktadır.
- **Hazırlık bilinmeyen** -Azure geçirmek yeterli veri vCenter Server'da kullanılabilir nedeniyle makine hazırlık bulamadı.

Azure geçirme makine özelliklerini ve konuk işletim sistemi şirket içi makineyi Azure hazır olup olmadığını belirlemek için inceler.

### <a name="machine-properties"></a>Makine özellikleri
Azure geçirme Azure üzerinde bir VM çalıştırıp çalıştıramayacağını tanımlamak için şirket içi VM aşağıdaki özelliklerini inceler.

**Özellik** | **Ayrıntılar** | **Azure hazır olma durumu**
--- | --- | ---
**Önyükleme türü** | Azure VM'ler BIOS ve UEFI değil olarak önyükleme türüyle destekler. | Koşullu UEFI önyükleme türü ise, Azure için hazır.
**Çekirdek** | Makinelerdeki çekirdek sayısına eşit veya bir Azure VM için desteklenen en yüksek çekirdek sayısı (32) değerinden olması gerekir.<br/><br/> Performans geçmişi kullanılabilir durumdaysa, karşılaştırma için kullanılan çekirdek Azure geçirmek göz önünde bulundurur. Rahatlık faktörü değerlendirme ayarlarında belirtilirse, kullanılan çekirdek sayısı rahatlık faktörüyle çarpılır.<br/><br/> Performans geçmişi yok ise, Azure geçirmek rahatlık faktörü uygulamadan ayrılmış çekirdek kullanır. | Çekirdek sayısı 32'den büyükse, hazır değil.
**Bellek** | Makine bellek boyutu eşit veya bir Azure VM için izin verilen en fazla bellek (448 GB) küçük olmalıdır. <br/><br/> Performans geçmişi kullanılabilir durumdaysa, karşılaştırma için kullanılan bellek Azure geçirmek göz önünde bulundurur. Rahatlık faktörü belirtilirse, kullanılan bellek rahatlık faktörüyle çarpılır.<br/><br/> Geçmiş ise rahatlık faktörü uygulamadan ayrılmış bellek kullanılır.<br/><br/> | Bellek boyutu 448 GB'den büyükse, hazır değil.
**Depolama diskini** | Ayrılmış bir disk boyutunu 4 TB (4096 GB) olması gerekir veya daha az.<br/><br/> Makineye bağlı disk sayısı 65 veya daha az, da dahil olmak üzere işletim sistemi diski olması gerekir. | Herhangi bir disk boyutu 4 TB veya makineye bağlı birden fazla 65 diskler olup olmadığını büyük varsa hazır değil.
**Ağ** | Daha az NIC'ler bağlı veya bir makine 32 olmalıdır. | Makine 32'den fazla NIC'ler varsa hazır değil

### <a name="guest-operating-system"></a>Konuk işletim sistemi
VM Özellikleri'yanı sıra Azure geçirmek de VM Azure üzerinde çalışıp çalışamayacağını tanımlamak için şirket içi VM konuk işletim sistemi bakar.

> [!NOTE]
> Azure geçirme vCenter Server'da aşağıdaki analizi yapmak için belirtilen işletim sistemi göz önünde bulundurur. Azure geçirmek tarafından yapılan bulma Gereci tabanlı olduğundan, VM içinde çalışan işletim sistemi bir belirtilen vCenter sunucusu aynı olup olmadığını doğrulamak için bir yol yok.

Aşağıdaki mantık Azure geçirmek tarafından işletim sistemi temelinde VM Azure hazır olup olmadığını belirlemek için kullanılır.

**İşletim Sistemi** | **Ayrıntılar** | **Azure hazır olma durumu**
--- | --- | ---
Windows Server 2016 & tüm Sp'ler | Azure tam destek sağlar. | Azure için hazır
Windows Server 2012 R2 & tüm Sp'ler | Azure tam destek sağlar. | Azure için hazır
Windows Server 2012 & tüm Sp'ler | Azure tam destek sağlar. | Azure için hazır
Windows Server 2008 R2 ile tüm Sp'ler | Azure tam destek sağlar.| Azure için hazır
Windows Server 2003-2008 R2 | Bu işletim sistemleri destek tarihi ve gerek kendi son başarılı bir [özel destek sözleşmesi (CSA)](https://aka.ms/WSosstatement) Azure desteği. | Koşullu olarak Azure için hazır, Azure'a geçirmeden önce işletim sistemi yükseltme yapmayı düşünün.
Windows 2000, 98, 95 ' NT 3.1, MS-DOS | Bu işletim sistemlerinin kendi sonuna destek tarih geçtiğinde, makine Azure'da önyükleme, ancak hiçbir işletim sistemi desteği Azure tarafından sağlanır. | Azure için hazır koşullu, Azure'a geçirmeden önce işletim sistemini yükseltmek için önerilir.
Windows İstemcisi 7, 8 ve 10 | Azure yalnızca Visual Studio abonelik desteği sağlar. | Azure için koşullu olarak hazır
Windows Vista, XP Professional | Bu işletim sistemlerinin kendi sonuna destek tarih geçtiğinde, makine Azure'da önyükleme, ancak hiçbir işletim sistemi desteği Azure tarafından sağlanır. | Azure için hazır koşullu, Azure'a geçirmeden önce işletim sistemini yükseltmek için önerilir.
Linux | Azure Symantec'in bu [Linux işletim sistemleri](../virtual-machines/linux/endorsed-distros.md). Diğer Linux işletim sistemleri Azure içinde önyükleme, ancak Azure'a geçirmeden önce işletim sistemi doğrulanan bir sürüme yükseltmek için önerilir. | Sürüm destekli varsa Azure için hazır.<br/><br/>Sürüm değil destekli koşullu hazır.
Diğer işletim sistemleri<br/><br/> Örneğin, Oracle Solaris Apple Mac OS vb., FreeBSD, vs. | Azure, bu işletim sistemlerini desteklemez. Makine Azure'da önyükleme, ancak hiçbir işletim sistemi desteği Azure tarafından sağlanır. | Azure için hazır koşullu, Azure'a geçirmeden önce desteklenen bir işletim sistemi yüklemek için önerilir.  
İşletim sistemi olarak belirtilen *diğer* vcenter Server'daki | Azure geçirme işletim Sisteminin bu durumda tanımlanamıyor. | Bilinmeyen hazırlık. Azure'da VM içinde çalışan işletim Sisteminin desteklendiğinden emin olun.
32 bit işletim sistemleri | Makine Azure'da önyükleme, ancak Azure tam destek sağlamaz. | Koşullu olarak Azure için hazır, 64-bit işletim sistemine 32-bit OS Azure'a geçirmeden önce makine işletim sistemini yükseltmeyi göz önünde bulundurun.

## <a name="sizing"></a>Boyutlandırma

Bir makine Azure için hazır olarak işaretlendikten sonra Azure geçirmek VM ve onun diskler için Azure boyutları. Değerlendirme özelliklerinde belirtilen boyutlandırma ölçüt performans tabanlı boyutlandırma yapmak için ise, makine Azure bir VM'nin boyutunu belirlemek için performans geçmişini Azure geçirmek göz önünde bulundurur. Bu yöntem, burada şirket içi VM aşırı ayırmış olmanız ancak kullanımı düşük ve maliyet kaydetmek için Azure sanal makineleri sağ boyutuna istediğiniz senaryolarda faydalıdır.

> [!NOTE]
> Azure geçirme performans geçmişi şirket içi VM'lerin vCenter Server'dan toplar. Doğru şekilde boyutlandırmaya emin olmak için vCenter sunucusu istatistikleri ayarında düzeyi 3 ayarlandığından emin olun ve şirket içi sanal makineleri bulmayı oluşturan önce en az bir gün bekleyin. VCenter sunucusu istatistikleri ayarında 3 düzeyin ise, disk ve ağ için performans verilerini toplanmaz.

VM-sizing için performans geçmişi göz önünde bulundurun ve VM olarak almak istediğiniz istemiyorsanız-olan Azure için boyutlandırma ölçüt olarak belirtebilirsiniz *şirket içi olarak* ve Azure geçirmek sonra boyutu şirket içi tabanlı VM'ler kullanım verileri dikkate olmadan yapılandırma. Disk boyutlandırma, bu durumda, hala performans verilerini temel alır.

### <a name="performance-based-sizing"></a>Performans tabanlı boyutlandırma

Performans tabanlı boyutlandırma için Azure geçirmek, VM'ye bağlı diskler ile başlar ve ardından ağ bağdaştırıcıları tarafından ve ardından eşlemeleri şirket içi VM işlem gereksinimlerine bir Azure VM bağlı olarak.

- **Depolama**: bir diski azure'da makineye bağlı her disk eşlemek Azure geçirmek çalışır.

    > [!NOTE]
    > Yalnızca yönetilen disklerde değerlendirmesi için Azure geçirme destekler.

    - Etkin disk g/ç / saniye (IOPS) ve üretilen işi (MBps) almak için Azure geçirmek disk IOPS ve üretilen iş rahatlık faktörle çarpar. Disk bir standart veya premium disk Azure eşlenmelidir olmadığını etkili IOPS ve üretilen iş değerlerine bağlı olarak, Azure geçirmek tanımlar.
    - Bir disk gerekli IOPS ve üretilen iş ile Azure geçirmek bulamazsanız, Azure için makine uygun değil olarak işaretler. [Daha fazla bilgi edinin](../azure-subscription-service-limits.md#storage-limits) Azure hakkında disk ve VM sınırlar.
    - Uygun bir disk kümesi bulursa, Azure geçirmek Depolama artıklık yöntemi ve değerlendirme ayarlarında belirtilen konuma desteği olanları seçer.
    - Birden çok uygun diskler varsa, en düşük maliyeti ile bir seçer.
    - İçin performans verilerini de diskler, tüm diskleri Azure standart disklere kullanılamaz eşlenir.

- **Ağ**: şirket içi makineye bağlı ağ bağdaştırıcıları ve bu ağ bağdaştırıcıları tarafından gerekli performansı sayıda destekleyebilir bir Azure VM bulmak Azure geçirmek çalışır.
    - Şirket içi VM etkili ağ performansını almak için Azure geçirmek, tüm ağ bağdaştırıcıları (ağ) kullanıma makine dışında (MBps) saniye başına aktarılan verileri toplar ve rahatlığı faktörü uygular. Bu sayı, gerekli ağ performansını destekleyebilen bir Azure VM bulmak için kullanılır.
    - Ağ performansı yanı sıra, ayrıca Azure VM gerekli olmadığını destekleyebilir göz önünde bulundurur ağ bağdaştırıcısı sayısı.
    - Ağ performans verileri kullanılabilir ise, yalnızca ağ bağdaştırıcıları sayısı için VM boyutlandırma olarak kabul edilir.

- **İşlem**: depolama ve ağ gereksinimleri hesaplandıktan sonra uygun bir VM boyutu Azure'da bulmak için CPU ve bellek gereksinimlerini Azure geçirmek göz önünde bulundurur.
    - Azure geçirme kullanılan çekirdek ve bellek arar ve etkili çekirdek ve bellek almak için rahatlık faktörü uygular. Bu sayıya göre uygun bir VM boyutu Azure'da bulmaya çalışır.
    - Uygun büyüklük bulunursa, makine uygun Azure için işaretlenir.
    - Uygun bir boyut bulunursa, Azure geçirmek depolama ve ağ hesaplamaları geçerlidir. Ardından, konum ve fiyatlandırma katmanı ayarlarını, son VM boyutu öneri de geçerlidir.
    - Birden fazla uygun Azure sanal makinesi boyutu varsa, en düşük maliyetli olan önerilir.

### <a name="as-on-premises-sizing"></a>Boyutlandırma şirket içi
Boyutlandırma ölçüt ise *gibi şirket içi boyutlandırma*, Azure geçirmek VM'lerin performans geçmişi dikkate almaz ve şirket içi ayrılan boyutuna göre VM'ler ayırır. Ancak, disk boyutlandırma için standart veya Premium diskleri önermek için disk performans geçmişi göz önünde bulundurun.  
- **Depolama**: Azure geçiş diski azure'da makineye bağlı her disk eşler.

    > [!NOTE]
    > Yalnızca yönetilen disklerde değerlendirmesi için Azure geçirme destekler.

    - Etkin disk g/ç / saniye (IOPS) ve üretilen işi (MBps) almak için Azure geçirmek disk IOPS ve üretilen iş rahatlık faktörle çarpar. Disk bir standart veya premium disk Azure eşlenmelidir olmadığını etkili IOPS ve üretilen iş değerlerine bağlı olarak, Azure geçirmek tanımlar.
    - Bir disk gerekli IOPS ve üretilen iş ile Azure geçirmek bulamazsanız, Azure için makine uygun değil olarak işaretler. [Daha fazla bilgi edinin](../azure-subscription-service-limits.md#storage-limits) Azure hakkında disk ve VM sınırlar.
    - Uygun bir disk kümesi bulursa, Azure geçirmek Depolama artıklık yöntemi ve değerlendirme ayarlarında belirtilen konuma desteği olanları seçer.
    - Birden çok uygun diskler varsa, en düşük maliyeti ile bir seçer.
    - İçin performans verilerini de diskler, tüm diskleri Azure standart disklere kullanılamaz eşlenir.
- **Ağ**: her ağ bağdaştırıcısı için bir ağ bağdaştırıcısı Azure önerilir.
- **İşlem**: Azure geçirmek çekirdek sayısı ve şirket içi VM bellek boyutunu arar ve aynı yapılandırmaya sahip bir Azure VM önerir. Birden fazla uygun Azure sanal makinesi boyutu varsa, en düşük maliyetli olan önerilir. CPU ve bellek kullanımı verileri için şirket içi boyutlandırma olarak dikkate alınmaz.

### <a name="confidence-rating"></a>Güvenilirlik derecesi

Azure Geçişi’ndeki her değerlendirme 1 yıldız ile 5 yıldız (1 yıldız en düşük, 5 yıldız en yüksektir) arasında değişen bir güvenilirlik derecesiyle ilişkilendirilir. Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliği temelinde bir değerlendirmeye atanır. Bir değerlendirmenin güvenilirlik derecesi, Azure Geçişi tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur.

VM’nin performans tabanlı boyutlandırması için Azure Geçişi, CPU ve bellek için kullanım verileri gerektirir. Ayrıca, VM'ye bağlı her disk boyutlandırma için okuma/yazma IOPS gerektiği ve üretilen iş. Sanal makineye eklenmiş her bir ağ bağdaştırıcısı için benzer şekilde Azure Geçişi’nin performansa dayalı boyutlandırma gerçekleştirmek amacıyla ağ giriş/çıkışına ihtiyacı vardır. Yukarıdaki kullanım rakamlarından herhangi biri vCenter Server’da mevcut değilse Azure Geçişi’nin yaptığı boyut önerisi güvenilir olmayabilir. Değerlendirme için güvenirlik derecelendirme kullanılabilir veri noktası yüzdesini bağlı olarak, aşağıdaki gibi sağlanır:

   **Veri noktalarının kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
   --- | ---
   %0-%20 | 1 Yıldız
   %21-%40 | 2 Yıldız
   %41-%60 | 3 Yıldız
   %61-%80 | 4 Yıldız
   %81-%100 | 5 Yıldız

Aşağıdaki nedenlerle bir değerlendirme için tüm veri noktaları kullanılabilir olmayabilir:
- VCenter istatistikleri ayarında sunucu düzeyi 3 ayarlı değil. vCenter Server’daki istatistik ayarı 3 düzeyinden küçükse vCenter Server’dan disk ve ağ için performans verileri toplanmaz. Bu durumda, Azure Geçişi tarafından disk ve ağ için sağlanan öneri, kullanım tabanlı olmaz. Diskin IOPS/üretilen iş düşünüldüğünde, olmadan Azure premium disk disk gerekecekse, Azure geçirmek tanımlayamıyor, bu nedenle, bu durumda, Azure geçirmek tüm diskler için standart diskler önerir.
- vCenter Server’daki istatistik ayarı, keşif başlatılmadan önce daha kısa süre bir için 3. düzey olarak ayarlanmıştır. Örneğin, bugün istatistik ayarı düzeyini 3 olarak değiştirdiğiniz, yarın ise (24 saat sonra) toplayıcı gerecini kullanarak keşfi başlattığınız bir senaryoyu ele alalım. Bir gün için değerlendirme oluşturuyorsanız tüm veri noktalarına sahip olursunuz ve değerlendirmenin güvenilirlik derecesi 5 yıldız olur. Ancak değerlendirme özelliklerinde performans süresini bir ay olarak değiştiriyorsanız, son bir ay için disk ve ağ performansı verileri kullanılabilir halde olmayacağından güvenilirlik derecesi düşer. Son bir ayın performans verilerini dikkate almak istiyorsanız, keşif başlatmadan önce bir ay boyunca vCenter Server istatistik ayarını 3 düzeyinde tutmanız önerilir.
- Değerlendirmenin hesaplandığı dönem boyunca birkaç sanal makine kapatılmıştır. Herhangi bir sanal makine belirli bir süre boyunca kapatıldıysa vCenter Server o süreye ait performans verilerine sahip olmaz.
- Değerlendirmenin hesaplandığı dönem boyunca birkaç sanal makine oluşturulmuştur. Örneğin, son bir ayın performans geçmişi için değerlendirme oluşturuyorsanız, ancak yalnızca bir hafta önce ortamda birkaç sanal makine oluşturulduysa. Bu tür durumlarda yeni sanal makinelerin performans geçmişi, sürenin tamamı boyunca mevcut olmaz.

> [!NOTE]
> Herhangi bir değerlendirmenin güvenilirlik derecesi 4 Yıldız’ın altında ise vCenter Server istatistik ayarları düzeyini 3 olarak değiştirmeniz, değerlendirme için göz önünde bulundurmak istediğiniz süre (1 gün/1 hafta/1 ay) boyunca bekleyip daha sonra keşif ve değerlendirme gerçekleştirmeniz önerilir. Bu yapılamazsa, performansa dayalı boyutlandırma güvenilir olmayabilir ve değerlendirme özellikleri değiştirilerek *şirket içi olarak boyutlandırmaya* geçiş yapılması önerilir.

## <a name="monthly-cost-estimation"></a>Aylık maliyet tahmini

Boyutlandırma önerileri tamamlandıktan sonra Azure geçirmek geçiş sonrası işlem ve depolama maliyetleri hesaplar.

- **Maliyet işlem**: önerilen Azure VM boyutu kullanarak, Azure geçirmek faturalama VM için aylık maliyeti hesaplamak için API'sini kullanır. Hesaplama, işletim sistemi, Yazılım Güvencesi, konum ve para birimi ayarları dikkate alır. Bu maliyet aylık toplam işlem maliyetini hesaplamak için tüm makineler arasında toplar.
- **Depolama maliyeti**: bir makine tüm diskleri aylık maliyeti toplayarak hesaplanır maliyeti aylık depolama makineye bağlı. Azure geçirme toplam aylık depolama maliyetleri tüm makinelerin depolama maliyetleri toplayarak hesaplar. Şu anda, hesaplama göz önüne değerlendirme ayarlarında belirtilen teklifleri almaz.

Maliyetleri tahakkuk ayarlarında belirtilen para birimi görüntülenir.


## <a name="next-steps"></a>Sonraki adımlar

[Şirket içi VMware Vm'leri için bir değerlendirme oluşturma](tutorial-assessment-vmware.md)
