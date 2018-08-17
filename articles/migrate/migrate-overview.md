---
title: Azure Geçişi Hakkında | Microsoft Azure
description: Azure Geçişi hizmetine genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 08/08/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 8371a160d129586f63b2f14946ed34a8d0637f6c
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39714249"
---
# <a name="about-azure-migrate"></a>Azure Geçişi Hakkında

Azure Geçişi hizmeti, Azure’a geçiş için şirket içi iş yüklerini değerlendirir. Hizmet, şirket içi makinelerin Azure’a geçiş uygunluğunu değerlendirir, performans tabanlı boyutlandırma gerçekleştirir ve şirket içi makineleri Azure’da çalıştırmaya yönelik maliyet tahminleri sağlar. Lift-and-shift ile taşımayı düşünüyorsanız veya geçişin ilk değerlendirme aşamalarındaysanız bu hizmet size uygundur. Değerlendirme sonrasında, makineleri Azure’a geçirmek için [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) ve [Azure Veritabanı Geçiş Hizmeti](https://docs.microsoft.com/azure/dms/dms-overview) gibi hizmetleri kullanabilirsiniz.

## <a name="why-use-azure-migrate"></a>Azure Geçişi hizmetini neden kullanmalısınız?

Azure Geçişi şunları yapmanıza yardımcı olur:

- **Azure için hazır olma durumunu değerlendirme**: Şirket içi makinelerinizin Azure’da çalıştırılmaya uygun olup olmadığını değerlendirme.
- **Boyut önerileri alma**: Şirket içi VM’lerin performans geçmişine göre Azure VM’leri için boyut önerileri alın.
- **Aylık maliyetleri tahmin etme**: Azure’da çalışan şirket içi makineler için tahmini maliyetleri alın.  
- **Yüksek güvenle geçirme**: Birlikte değerlendirip geçireceğiniz makine grupları oluşturmak için şirket içi makinelerin bağımlılıklarını görselleştirin.

## <a name="current-limitations"></a>Geçerli sınırlamalar

- Şu anda Azure sanal makinelerine geçiş için yalnızca şirket içi VMware sanal makinelerini (VM) değerlendirebilirsiniz. VMware sanal makineleri, vCenter Server (sürüm 5.5, 6.0 veya 6.5) tarafından yönetilmelidir.
- Hyper-V sanal makinelerini ve fiziksel sunucuları değerlendirmek istiyorsanız Hyper-V için [Azure Site Recovery Dağıtım Planlayıcısı](http://aka.ms/asr-dp-hyperv-doc)'nı, fiziksel makineler için de [iş ortaklarımız tarafından sunulan araçları](https://azure.microsoft.com/migration/partners/) kullanabilirsiniz.
- Tek keşifte en fazla 1500 sanal makine ve tek projede en fazla 1500 sanal makine bulabilirsiniz. Ayrıca tek değerlendirmede en fazla 1500 sanal makineyi değerlendirebilirsiniz.
- Daha büyük bir ortam keşfetmek istiyorsanız keşfi bölüp birden fazla proje oluşturabilirsiniz. [Daha fazla bilgi edinin](how-to-scale-assessment.md). Azure Geçişi, abonelik başına 20’ye kadar projeyi destekler.
- Azure Geçişi projesini yalnızca Batı Orta ABD veya Doğu ABD bölgesinde oluşturabilirsiniz. Bu durum, herhangi bir Azure hedef konumu için yapacağınız geçiş planını etkilemez. Geçiş projesinin konumu yalnızca şirket içi ortamda bulunan meta verileri depolamak için kullanılır.
- Azure Geçişi yalnızca yönetilen disklerin geçiş değerlendirmesini destekler.


## <a name="what-do-i-need-to-pay-for"></a>Ne için ödeme yapmam gerekiyor?

Azure Geçişi fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/azure-migrate/).


## <a name="whats-in-an-assessment"></a>Bir değerlendirme neleri içerir?

Değerlendirme ayarları ihtiyaçlarınıza göre özelleştirilebilir. Değerlendirme özellikleri aşağıdaki tabloda özetlenmiştir.

**Özellik** | **Ayrıntılar**
--- | ---
**Hedef konum** | Geçişi yapmak istediğiniz Azure konumu.<br/><br/>Azure Geçişi şu anda 30 bölgeyi desteklemektedir. [Hangi bölgeler olduğuna bakın](https://azure.microsoft.com/global-infrastructure/services/). Varsayılan hedef bölge, Batı ABD 2 olarak ayarlanır.
**Depolama türü** | Azure'da ayırmak istediğiniz disklerin türüdür. Bu özellik, boyutlandırma ölçütü **şirket içi olarak** olduğunda geçerlidir. Hedef disk türünü premium (varsayılan) veya standart yönetilen diskler olarak belirtebilirsiniz. Performans tabanlı boyutlandırma için, disk boyutlandırma önerisi VM'lerin performans verilerine göre otomatik olarak yapılır. 
**Boyutlandırma ölçütü** | Boyutlandırmayı şirket içi sanal makinelerin **performans geçmişini** temel alarak yapabilir veya performans geçmişini dikkate almadan **şirket içi olarak** (varsayılan) yapabilirsiniz. 
**Azure teklifi** | Kaydolduğunuz [Azure teklifi](https://azure.microsoft.com/support/legal/offer-details/). Azure Geçişi, buna göre bir maliyet tahmini oluşturur.
**Azure Hibrit Avantajı** | İndirimli fiyatlardan yararlanmak için yazılım güvencesine sahip olup olmadığınız ve [Azure Hibrit Avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/) için uygun olup olmadığınız bilgisi.
**Ayrılmış Örnekler** |  Azure'da [ayrılmış örneklere](https://azure.microsoft.com/pricing/reserved-vm-instances/) sahip olup olmama durumunuz. Azure Geçişi, buna göre bir maliyet tahmini oluşturur.
**VM çalışma süresi** | VM'lerin Azure'da çalıştırılacağı süre. Maliyet tahminleri bu bilgiye göre gerçekleştirilir.
**Fiyatlandırma katmanı** | Hedef Azure VM'leri için [fiyatlandırma katmanı (temel/standart)](../virtual-machines/windows/sizes-general.md). Örneğin, bir üretim ortamına geçiş yapmayı planlıyorsanız, daha düşük gecikme süresi ile sanal makineler sağlayan, ancak daha fazla maliyetli olabilecek standart katmanını göz önünde bulundurmak istersiniz. Diğer taraftan bir test ortamında yüksek gecikme süresine ve daha düşük maliyete sahip temel katmanı kullanabilirsiniz. Varsayılan olarak [standart](../virtual-machines/windows/sizes-general.md) katmanı kullanılır.
**Performans geçmişi** | Varsayılan olarak Azure Geçişi, %95 yüzdebirlik değer ile son günün performans geçmişini kullanarak şirket içi makinelerin performansını değerlendirir. 
**VM serisi** | VM serisi, boyut tahmini için kullanılır. Örneğin, Azure’da A serisi VM’lere geçirmeyi planlamadığınız bir üretim ortamınız varsa, A serisini liste veya serilerin dışında bırakabilirsiniz. Boyutlandırma yalnızca seçili serilerde yapılır.   
**Konfor katsayısı** | Azure Geçişi, değerlendirme sırasında bir tamponu (konfor katsayısı) göz önünde bulundurur. Bu tampon, VM’lerin makine kullanım verilerinin (CPU, bellek, disk ve ağ) üzerine uygulanır. Konfor katsayısı; sezona özgü kullanım, kısa performans geçmişi ve gelecek kullanımlarda oluşabilecek artışlar gibi konuları hesaba katar.<br/><br/> Örneğin, %20 kullanıma sahip 10 çekirdekli bir VM normalde 2 çekirdekli VM ile sonuçlanır. Ancak, 2.0x konfor katsayısı ile sonuç 4 çekirdekli VM olur. Varsayılan konfor ayarı 1,3x’tir.


## <a name="how-does-azure-migrate-work"></a>Azure Geçişi nasıl çalışır?

1.  Bir Azure Geçişi projesi oluşturursunuz.
2.  Azure Geçişi, toplayıcı aleti adı verilen bir şirket içi VM kullanarak şirket içi makinelerinize ilişkin bilgileri bulur. Aleti oluşturmak için Open Virtualization Appliance (.ova) biçimindeki kurulum dosyasını indirirsiniz ve şirket içi vCenter Server’ınıza VM olarak aktarırsınız.
3. VM'ye vCenter Server'dan bağlanır ve bağlantı sırasında yeni bir parola belirtirsiniz.
4. Bulmayı başlatmak için VM'de toplayıcıyı çalıştırırsınız.
5. Toplayıcı, VMware PowerCLI cmdlet’lerini kullanarak VM meta verilerini toplar. Bulma işlemi aracısızdır ve VMware konaklarına ya da VM’lere herhangi bir yükleme yapmaz. Toplanan meta veriler VM bilgilerini (çekirdekler, bellek, diskler, disk boyutları ve ağ bağdaştırıcıları) içerir. Ayrıca, CPU ve bellek kullanımı, disk IOPS, disk aktarım hızı (MB/sn) ve ağ çıktısı (MB/sn) gibi VM’lere ait performans verilerini toplar.
5.  Meta veriler Azure Geçişi projesine gönderilir. Verileri Azure portalında görüntüleyebilirsiniz.
6.  Değerlendirme amacıyla keşfedilen VM’leri gruplar halinde toplayın. Örneğin, aynı uygulamayı çalıştıran VM’leri gruplandırabilirsiniz. Daha kesin gruplandırma için, bir gruptaki belirli bir makine ya da tüm makinelerin bağımlılıklarını görüntülemek ve grubu geliştirmek için bağımlılık görselleştirmeyi kullanabilirsiniz.
7.  Grup tanımlandıktan sonra bir değerlendirme oluşturursunuz.
8.  Değerlendirme tamamlandıktan sonra portalda görüntüleyebilir veya Excel biçiminde indirebilirsiniz.

  ![Azure Geçişi mimarisi](./media/migration-planner-overview/overview-1.png)

## <a name="what-are-the-port-requirements"></a>Bağlantı noktası gereksinimleri nelerdir?

Tabloda Azure Geçişi iletişimleri için gereken bağlantı noktaları özetlenmektedir.

Bileşen | İletişim kurar |  Ayrıntılar
--- | --- |--- 
Toplayıcı  | Azure Geçişi hizmeti | Toplayıcı SSL bağlantı noktası 443 üzerinden hizmete bağlanır.
Toplayıcı | vCenter Server | Varsayılan olarak toplayıcı, 443 numaralı bağlantı noktası üzerinden vCenter Server’a bağlanır. Sunucu farklı bir bağlantı noktasında dinliyorsa, VM üzerinde giden bağlantı noktası olarak yapılandırın. 
Şirket içi VM | Log Analytics Çalışma Alanı | [TCP 443] | [Microsoft Monitoring Agent (MMA)](../log-analytics/log-analytics-windows-agent.md), Log Analytics'e bağlanmak için 443 numaralı TCP bağlantı noktasını kullanır. Bu bağlantı noktası yalnızca MMA aracısına ihtiyaç duyan bağımlılık görselleştirmesi özelliğini kullanıyorsanız gereklidir. 


## <a name="what-happens-after-assessment"></a>Değerlendirmeden sonra ne olur?

Şirket içi makineleri değerlendirdikten sonra, geçiş işlemini gerçekleştirmek üzere birkaç araç kullanabilirsiniz:

- **Azure Site Recovery**: Azure’a geçiş için Azure Site Recovery’yi kullanabilirsiniz. Bunu yapmak için depolama hesabı ve sanal ağ olmak üzere ihtiyacınız olan [Azure bileşenlerini hazırlarsınız](../site-recovery/tutorial-prepare-azure.md). Şirket içinde [VMware ortamınızı hazırlarsınız](../site-recovery/vmware-azure-tutorial-prepare-on-premises.md). Her şey hazır olduğunda Azure'a çoğaltmayı kurup etkinleştirir ve VM'leri geçirirsiniz. [Daha fazla bilgi edinin](../site-recovery/vmware-azure-tutorial.md).
- **Azure Veritabanı Geçişi**: Şirket içi makineler SQL Server, MySQL veya Oracle gibi bir veritabanı çalıştırıyorsa, bunları Azure’a geçirmek için [Azure Veritabanı Geçiş Hizmeti](../dms/dms-overview.md)'ni kullanabilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

- Şirket içi VMware VM değerlendirmesi oluşturmak için [öğreticiyi izleyin](tutorial-assessment-vmware.md).
- Azure Geçişi hakkında [sık sorulan soruları gözden geçirin](resources-faq.md).
