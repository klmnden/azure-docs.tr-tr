---
title: "Azure geçirmek değerlendirme hesaplamalarda | Microsoft Docs"
description: "Değerlendirme hesaplamalar Azure geçirmek hizmetindeki genel bir bakış sağlar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/02/2017
ms.author: raynew
ms.openlocfilehash: b264e2ceac4e76faa37d21972b94cfe323aa3ce5
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="assessment-calculations"></a>Değerlendirme hesaplamaları

[Azure geçirme](migrate-overview.md) geçiş Azure için şirket içi iş yüklerini değerlendirir. Bu makalede değerlendirmelerinin nasıl hesaplandığını hakkında bilgi sağlar.


## <a name="overview"></a>Genel Bakış

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
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Bu işletim sistemlerinin kendi sonuna destek tarih geçtiğinde, makine Azure'da önyükleme, ancak hiçbir işletim sistemi desteği Azure tarafından sağlanır. | Azure için hazır koşullu, Azure'a geçirmeden önce işletim sistemini yükseltmek için önerilir.
Windows İstemcisi 7, 8 ve 10 | Azure yalnızca Visual Studio abonelik desteği sağlar. | Koşullu olarak Azure için hazır
Windows Vista, XP Professional | Bu işletim sistemlerinin kendi sonuna destek tarih geçtiğinde, makine Azure'da önyükleme, ancak hiçbir işletim sistemi desteği Azure tarafından sağlanır. | Azure için hazır koşullu, Azure'a geçirmeden önce işletim sistemini yükseltmek için önerilir.
Linux | Azure Symantec'in bu [Linux işletim sistemleri](../virtual-machines/linux/endorsed-distros.md). Diğer Linux işletim sistemleri Azure içinde önyükleme, ancak Azure'a geçirmeden önce işletim sistemi doğrulanan bir sürüme yükseltmek için önerilir. | Sürüm destekli varsa Azure için hazır.<br/><br/>Sürüm değil destekli koşullu hazır.
Diğer işletim sistemleri<br/><br/> Örneğin, Oracle Solaris Apple Mac OS vb., FreeBSD, vs. | Azure, bu işletim sistemlerini desteklemez. Makine Azure'da önyükleme, ancak hiçbir işletim sistemi desteği Azure tarafından sağlanır. | Azure için hazır koşullu, Azure'a geçirmeden önce desteklenen bir işletim sistemi yüklemek için önerilir.  
İşletim sistemi olarak belirtilen *diğer* vcenter Server'daki | Azure geçirme işletim Sisteminin bu durumda tanımlanamıyor. | Bilinmeyen hazırlık. Azure'da VM içinde çalışan işletim Sisteminin desteklendiğinden emin olun. 
32 bit işletim sistemleri | Makine Azure'da önyükleme, ancak Azure tam destek sağlamaz. | Koşullu olarak Azure için hazır, 64-bit işletim sistemine 32-bit OS Azure'a geçirmeden önce makine işletim sistemini yükseltmeyi göz önünde bulundurun.

## <a name="sizing"></a>Boyutlandırma

Bir makine Azure için hazır olarak işaretlendikten sonra Azure geçirmek VM ve onun diskler için Azure boyutları. Değerlendirme özelliklerinde belirtilen boyutlandırma ölçüt performans tabanlı boyutlandırma yapmak için ise, makine Azure bir VM'nin boyutunu belirlemek için performans geçmişini Azure geçirmek göz önünde bulundurur. Bu yöntem, burada şirket içi VM aşırı ayırmış olmanız ancak kullanımı düşük ve maliyet kaydetmek için Azure sanal makineleri sağ boyutuna istediğiniz senaryolarda faydalıdır.

> [!NOTE]
> Azure geçirme performans geçmişi şirket içi VM'lerin vCenter Server'dan toplar. Doğru şekilde boyutlandırmaya emin olmak için vCenter sunucusu istatistikleri ayarında düzeyi 3 ayarlandığından emin olun ve şirket içi sanal makineleri bulmayı oluşturan önce en az bir gün bekleyin. VCenter sunucusu istatistikleri ayarında 3 düzeyin ise, disk ve ağ için performans verilerini toplanmaz. 

Performans geçmişi göz önünde bulundurun ve VM olarak almak istediğiniz istemiyorsanız-olan Azure için boyutlandırma ölçüt olarak belirtebilirsiniz *şirket içi olarak* ve Azure geçirmek sonra boyutu şirket içi yapılandırmasını temel alarak VM'ler kullanım verileri söz konusu olabilir.

### <a name="performance-based-sizing"></a>Performans tabanlı boyutlandırma

Performans tabanlı boyutlandırma için Azure geçirmek, VM'ye bağlı diskler ile başlar ve ardından ağ bağdaştırıcıları tarafından ve ardından eşlemeleri şirket içi VM işlem gereksinimlerine bir Azure VM bağlı olarak. 
 
- **Diskleri**: bir diski azure'da makineye bağlı her disk eşlemek Azure geçirmek çalışır. 
    
    > [!NOTE]
    > Yalnızca yönetilen disklerde değerlendirmesi için Azure geçirme destekler.
    
    - Etkin disk g/ç / saniye (IOPS) ve üretilen işi (MBps) almak için Azure geçirmek disk IOPS ve üretilen iş rahatlık faktörle çarpar. Disk bir standart veya premium disk Azure eşlenmelidir olmadığını etkili IOPS ve üretilen iş değerlerine bağlı olarak, Azure geçirmek tanımlar.
    - Bir disk gerekli IOPS ve üretilen iş ile Azure geçirmek bulamazsanız, Azure için makine uygun değil olarak işaretler. [Daha fazla bilgi edinin](../azure-subscription-service-limits.md#storage-limits) Azure hakkında disk ve VM sınırlar.
    - Uygun bir disk kümesi bulursa, Azure geçirmek Depolama artıklık yöntemi ve değerlendirme ayarlarında belirtilen konuma desteği olanları seçer.
    - Birden çok uygun diskler varsa, en düşük maliyeti ile bir seçer.
    - İçin performans verilerini de diskler, tüm diskleri Azure standart disklere kullanılamaz eşlenir.

- **Ağ bağdaştırıcıları**: şirket içi makineye bağlı ağ bağdaştırıcıları ve bu ağ bağdaştırıcıları tarafından gerekli performansı sayıda destekleyebilir bir Azure VM bulmak Azure geçirmek çalışır.
    - Şirket içi VM etkili ağ performansını almak için Azure geçirmek, tüm ağ bağdaştırıcıları (ağ) kullanıma makine dışında (MBps) saniye başına aktarılan verileri toplar ve rahatlığı faktörü uygular. Bu sayı, gerekli ağ performansını destekleyebilen bir Azure VM bulmak için kullanılır.
    - Ağ performansı yanı sıra, ayrıca Azure VM gerekli olmadığını destekleyebilir göz önünde bulundurur ağ bağdaştırıcısı sayısı.
    - Ağ performans verileri kullanılabilir ise, yalnızca ağ bağdaştırıcıları sayısı için VM boyutlandırma olarak kabul edilir.

- **VM boyutu**: depolama ve ağ gereksinimleri hesaplandıktan sonra uygun bir VM boyutu Azure'da bulmak için CPU ve bellek gereksinimlerini Azure geçirmek göz önünde bulundurur.
    - Azure geçirme kullanılan çekirdek ve bellek arar ve etkili çekirdek ve bellek almak için rahatlık faktörü uygular. Bu sayıya göre uygun bir VM boyutu Azure'da bulmaya çalışır.
    - Uygun büyüklük bulunursa, makine uygun Azure için işaretlenir.
    - Uygun bir boyut bulunursa, Azure geçirmek depolama ve ağ hesaplamaları geçerlidir. Ardından, konum ve fiyatlandırma katmanı ayarlarını, son VM boyutu öneri de geçerlidir.
    - Birden çok uygun Azure VM boyutlarını varsa, en düşük maliyeti ile bir önerilir.

### <a name="as-on-premises-sizing"></a>Boyutlandırma şirket içi
Boyutlandırma ölçüt ise *gibi şirket içi boyutlandırma*, Azure geçirmek VM'lerin performans geçmişi dikkate almaz ve VM'ler ayırır ve boyutuna göre diskleri şirket içi ayrılmış.
- **Depolama**: her disk için standart bir disk azure'da şirket içi disk boyutu aynı önerilir.
- **Ağ**: her ağ bağdaştırıcısı için bir ağ bağdaştırıcısı Azure önerilir.
- **İşlem**: Azure geçirmek çekirdek sayısı ve şirket içi VM bellek boyutunu arar ve aynı yapılandırmaya sahip bir Azure VM önerir. Birden çok uygun Azure VM boyutlarını varsa, en düşük maliyeti ile bir önerilir.
 
### <a name="confidence-rating"></a>GÜVENİRLİK derecelendirme

Her Azure geçirmek değerlendirmesi 5 yıldız (en düşük olan 1 yıldız ve 5 yıldız en yüksek olan) 1 yıldızdan aralıkları güvenirlik derecelendirme ilişkilendirilir. GÜVENİRLİK derecelendirme değerlendirme işlem için gereken veri noktaları kullanılabilirliğine göre bir değerlendirme atanır. Bir değerlendirme güvenirlik derecesi Azure geçiş tarafından sağlanan boyutu öneriler güvenilirliğini tahmin etmenize yardımcı olur. 

GÜVENİRLİK derecelendirme olduğunda yararlı yapmakta olduğunuz *performans tabanlı boyutlandırma* gibi Azure geçirmek kullanım tabanlı boyutlandırma yapmak için yeterli veri noktası olmayabilir. İçin *gibi şirket içi boyutlandırma*, güvenirlik derecelendirme Azure geçirme gereken VM boyutu için tüm veri noktaları içerdiğinden, her zaman 5 yıldız. 

Performans tabanlı için boyutlandırma VM Azure geçirmek, CPU ve bellek kullanım verileri gerekir. Ayrıca, VM'ye bağlı her disk için okuma/yazma IOPS gerektiği ve üretilen iş. Benzer şekilde VM'ye bağlı her ağ bağdaştırıcısı için Azure geçirmek giriş/çıkış ağ performansı tabanlı boyutlandırma yapmak için gerekir. Yukarıdaki kullanımı sayılar vCenter Server'da mevcut değilse, Azure geçirmek tarafından yapılan boyutu öneri güvenilir olmayabilir. Veri noktaları kullanılabilir yüzdesi bağlı olarak, değerlendirme için güvenirlik derecelendirme sağlanır:

   **Veri noktalarının kullanılabilirliğini** | GÜVENİRLİK derecelendirme
   --- | ---
   0%-20% | 1 yıldız
   21%-40% | 2 yıldız
   41%-60% | 3 yıldız
   61%-80% | 4 yıldız
   81%-100% | 5 Star

Bir değerlendirme aşağıdaki nedenlerden biri dolayısıyla kullanılabilir tüm veri noktaları olmayabilir:
- VCenter Server 3 ve değerlendirme düzey ayarlanmadı istatistikleri ayarında performans tabanlı boyutlandırma boyutlandırma ölçütü olarak sahiptir. VCenter sunucusu istatistikleri ayarında 3 düzeyinden daha düşük ise, disk ve ağ için performans verilerini vCenter Server'dan toplanmaz. Bu durumda, disk ve ağ için Azure geçiş tarafından sağlanan öneri kullanım tabanlı değil. Azure premium disk disk gerekecekse IOPS/üretilen iş diskin düşünmeden Azure geçirmek tanımlayamıyor gibi depolama için Azure geçirmek standart diskler önerir.
- VCenter istatistikleri ayarında sunucu düzeyi 3 daha kısa bir süre için bulmayı oluşturan önce ayarlandı. Örneğin, şimdi 3 Bugün ve yarın (24 saat sonra) Toplayıcı Gereci kullanırken bulma kapalı başlangıç düzeyini ayarlama istatistikleri buradan senaryoyu göz önünde bulundurun. Bir gün için bir değerlendirme oluşturuyorsanız, tüm veri noktaları varsa ve değerlendirme güvenirlik derecesi 5 olacaktır yıldız. Ancak bir ay boyunca performans süresi değerlendirme özelliklerinde değiştiriyorsanız diski olarak güvenirlik derecelendirme arıza ve ağ performans verileri son bir ay boyunca kullanılabilir olmayacaktır. Son bir ay boyunca performans verilerini göz önünde bulundurun istiyorsanız vCenter sunucusu istatistikleri ayarını Düzey 3 bulmayı kazandırın önce bir ay boyunca tutmanız önerilir. 
- Birkaç VM'ler değerlendirme hesaplandığı dönemde kapatılan. Herhangi bir VM için bazı süresi kapalı, vCenter sunucusu belirli bir döneme ait performans verilerini sahip değil. 
- Birkaç VM'ler değerlendirme hesaplandığı dönemi Between oluşturuldu. Örneğin, bir değerlendirme için oluşturuyorsanız, son bir ay ancak birkaç VM'ler performans geçmişi oluşturulmuş ortamda yalnızca bir hafta önce. Böyle durumlarda, yeni VM'ler performans geçmişini tüm süresince var olmaz.

> [!NOTE]
> Tüm değerlendirme güvenirlik derecesi 4 yıldız ise, biz vCenter sunucusu istatistikleri ayarları düzeyi 3'e değiştirmenizi öneririz değerlendirmesi için istediğiniz dikkate alınması gereken süre bekleyin (1 gün/1 hafta/1 ay) ve bulma ve değerlendirme yapın. Yukarıdaki yapılamaz, performans tabanlı boyutlandırma güvenilir olmayabilir ve geçmek için önerilen *gibi şirket içi boyutlandırma* değerlendirme özelliklerini değiştirerek.

## <a name="monthly-cost-estimation"></a>Aylık maliyet tahmini

Boyutlandırma önerileri tamamlandıktan sonra Azure geçirmek geçiş sonrası işlem ve depolama maliyetleri hesaplar.

- **Maliyet işlem**: önerilen Azure VM boyutu kullanarak, Azure geçirmek faturalama VM için aylık maliyeti hesaplamak için API'sini kullanır. Hesaplama, işletim sistemi, Yazılım Güvencesi, konum ve para birimi ayarları dikkate alır. Bu maliyet aylık toplam işlem maliyetini hesaplamak için tüm makineler arasında toplar.
- **Depolama maliyeti**: bir makine tüm diskleri aylık maliyeti toplayarak hesaplanır maliyeti aylık depolama makineye bağlı. Azure geçirme toplam aylık depolama maliyetleri tüm makinelerin depolama maliyetleri toplayarak hesaplar. Şu anda, hesaplama göz önüne değerlendirme ayarlarında belirtilen teklifleri almaz.

Maliyetleri tahakkuk ayarlarında belirtilen para birimi görüntülenir. 


## <a name="next-steps"></a>Sonraki adımlar

[Şirket içi VMware Vm'leri için bir değerlendirme oluşturma](tutorial-assessment-vmware.md)
