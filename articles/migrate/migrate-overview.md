---
title: "Azure Geçişi Hakkında | Microsoft Azure"
description: "Azure Geçişi hizmetine genel bir bakış sağlar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 01/08/2018
ms.author: raynew
ms.openlocfilehash: 0bd3d7a9961e7a095684262ae1031f5a3ac0c3fb
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="about-azure-migrate"></a>Azure Geçişi Hakkında

Azure Geçişi hizmeti, Azure’a geçiş için şirket içi iş yüklerini değerlendirir. Hizmet, geçişe uygunluğu ve performans tabanlı boyutlandırmayı değerlendirir ve şirket içi makinelerinizi Azure’da çalıştırmaya yönelik maliyet tahminleri sağlar. Lift-and-shift ile taşımayı düşünüyorsanız veya geçişin ilk değerlendirme aşamalarındaysanız bu hizmet size uygundur. Değerlendirme sonrasında, makineleri Azure’a geçirmek için Azure Site Recovery ve Azure Veritabanı Geçişi gibi hizmetleri kullanabilirsiniz.

> [!NOTE]
> Azure Geçişi şu anda önizleme aşamasındadır ve üretim iş yüklerini destekler.

## <a name="why-use-azure-migrate"></a>Azure Geçişi hizmetini neden kullanmalısınız?

Azure Geçişi şunları yapmanıza yardımcı olur:

- **Azure için hazır olma durumunu değerlendirme**: Şirket içi makinelerinizin Azure’da çalıştırılmaya uygun olup olmadığını değerlendirme. 
- **Boyut önerileri alma**: Şirket içi VM’lerin performans geçmişine göre Azure VM’leri için boyut önerileri alın. 
- **Aylık maliyetleri tahmin etme**: Azure’da çalışan şirket içi makineler için tahmini maliyetleri alın.  
- **Yüksek güvenle geçirme**: Birlikte değerlendirip geçireceğiniz makine grupları oluşturmak için şirket içi makinelerin bağımlılıklarını görselleştirin. Belirli bir makinenin veya bir gruptaki tüm makinelerin bağımlılıklarını doğru bir şekilde görüntüleyebilirsiniz.

## <a name="current-limitations"></a>Geçerli sınırlamalar

- Şu anda, şirket içi VMware sanal makinelerini (VM) Azure VM’lerine geçiş için değerlendirebilirsiniz.

> [!NOTE]
> Yol haritasında bulunan Hyper-V desteği kısa süre içinde etkinleştirilecektir. Bu arada, Hyper-V iş yüklerinin geçişini planlamak için [Azure Site Recovery Dağıtım Planlayıcısı](http://aka.ms/asr-dp-hyperv-doc)’nı kullanmanız önerilir. 

- Tek keşifte en çok 1000 sanal makine ve tek projede en çok 1500 sanal makine bulabilirsiniz. Ayrıca, tek değerlendirmede en çok 400 sanal makineyi değerlendirebilirsiniz. Daha fazla makineyi bulmanız veya değerlendirmeniz gerekiyorsa, keşiflerin veya değerlendirmelerin sayısını artırabilirsiniz. [Daha fazla bilgi edinin](how-to-scale-assessment.md).
- Değerlendirmek istediğiniz VM, vCenter Server sürüm 5.5, 6.0 veya 6.5 ile yönetilmelidir.
- Azure Geçişi projesini yalnızca Batı Orta ABD bölgesinde oluşturabilirsiniz. Ancak, bu kısıtlama farklı bir hedef Azure konumu için geçiş planlamanızı engellemez. Geçiş projesinin konumu yalnızca şirket içi ortamda bulunan meta verileri depolamak için kullanılır.
- Azure Geçişi yalnızca yönetilen disklerin geçiş değerlendirmesini destekler.

## <a name="what-do-i-need-to-pay-for"></a>Ne için ödeme yapmam gerekiyor?

Azure Geçişi ek ücret ödenmeden kullanılabilir. Ancak genel önizleme sırasında, bağımlılık görselleştirmesi özelliklerinin kullanımı için ek ücret uygulanır. [Bağımlılık görselleştirmesi](concepts-dependency-visualization.md) desteği için Azure Geçişi varsayılan olarak bir Log Analytics çalışma alanı oluşturur. Bağımlılık görselleştirmesi kullanırsanız veya çalışma alanını Azure Geçişi dışında kullanırsanız, çalışma alanı kullanımı için ücretlendirilirsiniz. Ücretler hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/en-us/pricing/details/insight-analytics/). Hizmet genel kullanıma sunulduğunda, bağımlılık görselleştirmesi özelliklerinin kullanımı için herhangi bir ücret ödenmeyecektir.


## <a name="whats-in-an-assessment"></a>Bir değerlendirme neleri içerir?

Değerlendirme, şirket içi VM’lerin Azure uygunluğunu tanımlamanıza yardımcı olur, doğru boyutlandırma önerilerini ve Azure’da sanal makineleri çalıştırmak için maliyet tahminlerini alın. Değerlendirmeler, aşağıdaki tabloda özetlenen özellikleri temel alır. Bu özellikleri Azure Geçişi portalında değiştirebilirsiniz. 

**Özellik** | **Ayrıntılar**
--- | ---
**Hedef konum** | Geçişi yapmak istediğiniz Azure konumu. Varsayılan hedef konum, Batı ABD 2 olarak ayarlanır. 
**Depolama yedekliliği** | Azure VM’lerinin geçişten sonra kullanacağı depolama türü. Varsayılan seçenek LRS’dir.
**Fiyatlandırma planları** | Değerlendirme, yazılım güvencesine kaydolup kaydolmadığınızı ve [Azure Hibrit Avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/)’nı kullanıp kullanamayacağınızı göz önünde bulundurur. Ayrıca, uygulanması gereken Azure tekliflerini göz önünde bulundurur ve teklifle birlikte alacağınız aboneliğe özel indirimler (%) belirtmenize olanak tanır. 
**Fiyatlandırma katmanı** | Azure VM’lerinin [fiyatlandırma katmanını (temel/standart)](../virtual-machines/windows/sizes-general.md) belirtebilirsiniz. Bunun yapılması, bir üretim ortamında olup olmamanıza bağlı olarak uygun bir Azure VM ailesine geçiş yapmanıza yardımcı olur. Varsayılan olarak [standart](../virtual-machines/windows/sizes-general.md) katmanı kullanılır.
**Performans geçmişi** | Varsayılan olarak, Azure Geçişi %95 yüzdebirlik değer ile bir aylık geçmişi kullanarak şirket içi makinelerin performansını değerlendirir. Bu ayarı değiştirebilirsiniz.
**Konfor katsayısı** | Azure Geçişi, değerlendirme sırasında bir tamponu (konfor katsayısı) göz önünde bulundurur. Bu tampon, VM’lerin makine kullanım verilerinin (CPU, bellek, disk ve ağ) üzerine uygulanır. Konfor katsayısı; sezona özgü kullanım, kısa performans geçmişi ve gelecek kullanımlarda oluşabilecek artışlar gibi konuları hesaba katar.<br/><br/> Örneğin, %20 kullanıma sahip 10 çekirdekli bir VM normalde 2 çekirdekli VM ile sonuçlanır. Ancak, 2.0x konfor katsayısı ile sonuç 4 çekirdekli VM olur. Varsayılan konfor ayarı 1.3x’tir.


## <a name="how-does-azure-migrate-work"></a>Azure Geçişi nasıl çalışır?

1.  Bir Azure Geçişi projesi oluşturursunuz.
2.  Azure Geçişi, toplayıcı gereci adı verilen bir şirket içi VM kullanarak şirket içi makinelerinize ilişkin bilgileri bulur. Aleti oluşturmak için Open Virtualization Appliance (.ova) biçimindeki kurulum dosyasını indirirsiniz ve şirket içi vCenter Server’ınıza VM olarak aktarırsınız.
3.  vCenter Server'da konsol bağlantısı kullanarak VM’ye bağlanın, bağlanırken VM için yeni bir parola belirtin ve sonra keşfi başlatmak için VM’deki toplayıcı uygulamasını çalıştırın.
4.  Toplayıcı, VMware PowerCLI cmdlet’lerini kullanarak VM meta verilerini toplar. Bulma işlemi aracısızdır ve VMware konaklarına ya da VM’lere herhangi bir yükleme yapmaz. Toplanan meta veriler VM bilgilerini (çekirdekler, bellek, diskler, disk boyutları ve ağ bağdaştırıcıları) içerir. Ayrıca, CPU ve bellek kullanımı, disk IOPS, disk aktarım hızı (MB/sn) ve ağ çıktısı (MB/sn) gibi VM’lere ait performans verilerini toplar.
5.  Meta veriler Azure Geçişi projesine gönderilir. Verileri Azure portalında görüntüleyebilirsiniz.
6.  Değerlendirme amacıyla keşfedilen VM’leri gruplar halinde toplayın. Örneğin, aynı uygulamayı çalıştıran VM’leri gruplandırabilirsiniz. Daha kesin gruplandırma için, bir gruptaki belirli bir makine ya da tüm makinelerin bağımlılıklarını görüntülemek ve grubu geliştirmek için bağımlılık görselleştirmeyi kullanabilirsiniz.
7.  Grubunuz oluşturulduğunda, grup için bir değerlendirme oluşturursunuz. 
8.  Değerlendirme tamamlandıktan sonra portalda görüntüleyebilir veya Excel biçiminde indirebilirsiniz.



  ![Azure Planner mimarisi](./media/migration-planner-overview/overview-1.png)

## <a name="what-are-the-port-requirements"></a>Bağlantı noktası gereksinimleri nelerdir?

Tabloda Azure Geçişi iletişimleri için gereken bağlantı noktaları özetlenmektedir.

|Bileşen          |Şununla iletişim kurmak için     |Gereken bağlantı noktası  |Neden   |
|-------------------|------------------------|---------------|---------|
|Toplayıcı          |Azure Geçişi hizmeti   |TCP 443        |Toplayıcı SSL bağlantı noktası 443 üzerinden hizmete bağlanır|
|Toplayıcı          |vCenter Server          |Varsayılan 9443   | Varsayılan olarak, toplayıcı bağlantı noktası 9443 üzerinden vCenter sunucusuna bağlanır. Sunucu farklı bir bağlantı noktasında dinliyorsa, toplayıcı VM üzerinde giden bağlantı noktası olarak yapılandırılması gerekir. |
|Şirket içi VM     | Operations Management Suite (OMS) Çalışma Alanı          |[TCP 443](../log-analytics/log-analytics-windows-agent.md) |MMA aracısı Log Analytics’e bağlanmak için TCP 443’ü kullanır. Bu bağlantı noktası yalnızca bağımlılık görselleştirmesi özelliğini kullanıyorsanız ve Microsoft Monitoring Agent’ı (MMA) yüklüyorsanız gereklidir. |


  
## <a name="what-happens-after-assessment"></a>Değerlendirmeden sonra ne olur?

Azure Geçişi hizmeti ile geçiş için şirket içi makineleri değerlendirdikten sonra, geçiş işlemini gerçekleştirmek üzere birkaç araç kullanabilirsiniz:

- **Azure Site Recovery**: Azure’a geçiş için Azure Site Recovery’yi aşağıdaki gibi kullanabilirsiniz:
  - Bir Azure aboneliği, Azure sanal ağı ve depolama hesabı dahil olmak üzere Azure kaynaklarını hazırlayın.
  - Şirket içi VMware sunucularınızı geçiş için hazırlayın. Site Recovery için VMware desteği gereksinimlerini doğrulayın, VMware sunucularını bulma işlemi için hazırlayın, geçirmek istediğiniz VM’lere Site Recovery Mobility hizmetini yüklemeye hazırlanın. 
  - Geçişi ayarlayın. Bir Kurtarma Hizmetleri kasası ayarlayın, kaynak ve hedef geçiş ayarlarını yapılandırın, bir çoğaltma ilkesi ayarlayın ve çoğaltmayı etkinleştirin. Bir VM’yi Azure’a geçirme işleminin doğru şekilde çalışıp çalışmadığını denetlemek için olağanüstü durum kurtarma tatbikatı gerçekleştirebilirsiniz.
  - Şirket içi makineleri Azure’a geçirmek için yük devretme çalıştırın. 
  - Site Recovery geçiş öğreticisinde [daha fazla bilgi edinebilirsiniz](../site-recovery/tutorial-migrate-on-premises-to-azure.md).

- **Azure Veritabanı Geçişi**: Şirket içi makineleriniz SQL Server, MySQL veya Oracle gibi bir veritabanı çalıştırıyorsa, bunları Azure’a geçirmek için Azure Veritabanı Geçişi hizmetini kullanabilirsiniz. [Daha fazla bilgi edinin](https://azure.microsoft.com/campaigns/database-migration/).



## <a name="next-steps"></a>Sonraki adımlar 
Şirket içi VMware VM değerlendirmesi oluşturmak için [bir öğreticiyi izleyin](tutorial-assessment-vmware.md).