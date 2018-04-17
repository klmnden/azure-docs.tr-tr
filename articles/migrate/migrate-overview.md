---
title: Azure Geçişi Hakkında | Microsoft Azure
description: Azure Geçişi hizmetine genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 03/27/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 8f6c2c54dd20b663eb43c716a2bd82310cf0256a
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="about-azure-migrate"></a>Azure Geçişi Hakkında

Azure Geçişi hizmeti, Azure’a geçiş için şirket içi iş yüklerini değerlendirir. Hizmet, şirket içi makinelerin Azure’a geçiş uygunluğunu ve performans tabanlı boyutlandırmayı değerlendirip şirket içi makinelerinizi Azure’da çalıştırmaya yönelik maliyet tahminleri sağlar. Lift-and-shift ile taşımayı düşünüyorsanız veya geçişin ilk değerlendirme aşamalarındaysanız bu hizmet size uygundur. Değerlendirme sonrasında, makineleri Azure’a geçirmek için [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) ve [Azure Veritabanı Geçiş Hizmeti](https://docs.microsoft.com/azure/dms/dms-overview) gibi hizmetleri kullanabilirsiniz.

## <a name="why-use-azure-migrate"></a>Azure Geçişi hizmetini neden kullanmalısınız?

Azure Geçişi şunları yapmanıza yardımcı olur:

- **Azure için hazır olma durumunu değerlendirme**: Şirket içi makinelerinizin Azure’da çalıştırılmaya uygun olup olmadığını değerlendirme. 
- **Boyut önerileri alma**: Şirket içi VM’lerin performans geçmişine göre Azure VM’leri için boyut önerileri alın. 
- **Aylık maliyetleri tahmin etme**: Azure’da çalışan şirket içi makineler için tahmini maliyetleri alın.  
- **Yüksek güvenle geçirme**: Birlikte değerlendirip geçireceğiniz makine grupları oluşturmak için şirket içi makinelerin bağımlılıklarını görselleştirin. 

## <a name="current-limitations"></a>Geçerli sınırlamalar

- Şu anda Azure sanal makinelerine geçiş için yalnızca şirket içi VMware sanal makinelerini (VM) değerlendirebilirsiniz. VMware sanal makineleri, vCenter Server (sürüm 5.5, 6.0 veya 6.5) tarafından yönetilmelidir.
- Yol haritamızda Hyper-V desteği bulunur. Bu arada, Hyper-V iş yüklerinin geçişini planlamak için [Azure Site Recovery Dağıtım Planlayıcısı](http://aka.ms/asr-dp-hyperv-doc)’nı kullanmanız önerilir. 
- Tek keşifte en fazla 1500 sanal makine ve tek projede en fazla 1500 sanal makine bulabilirsiniz. Ayrıca tek değerlendirmede en fazla 1500 sanal makineyi değerlendirebilirsiniz.
- Azure Geçişi projesini yalnızca Batı Orta ABD veya Doğu ABD bölgesinde oluşturabilirsiniz. Ancak, bu kısıtlama farklı bir hedef Azure konumu için geçiş planlamanızı engellemez. Geçiş projesinin konumu yalnızca şirket içi ortamda bulunan meta verileri depolamak için kullanılır.
- Azure Geçişi yalnızca yönetilen disklerin geçiş değerlendirmesini destekler.


## <a name="what-do-i-need-to-pay-for"></a>Ne için ödeme yapmam gerekiyor?

Azure Geçişi fiyatlandırması hakkında daha fazla bilgiyi [burada](https://azure.microsoft.com/en-in/pricing/details/azure-migrate/) bulabilirsiniz.


## <a name="whats-in-an-assessment"></a>Bir değerlendirme neleri içerir?

Değerlendirme, şirket içi VM’lerin Azure uygunluğunu tanımlamanıza yardımcı olur, doğru boyutlandırma önerilerini ve Azure’da sanal makineleri çalıştırmak için maliyet tahminlerini alın. Değerlendirmelerin özellikleri değiştirilerek değerlendirmeler ihtiyaçlarınıza göre özelleştirilebilir. Aşağıda, bir değerlendirme oluşturulurken değerlendirilen özellikler verilmiştir. 

**Özellik** | **Ayrıntılar**
--- | ---
**Hedef konum** | Geçişi yapmak istediğiniz Azure konumu.<br/><br/>Şu anda Azure Geçişi tarafından desteklenen 30 bölge şunlardır: ABD Batı, ABD Batı 2, ABD Doğu, ABD Doğu 2, ABD Orta, ABD Orta Batı, ABD Orta Güney, ABD Orta Kuzey, Almanya Kuzeydoğu, Almanya Orta, Avustralya Doğu, Avustralya Güneydoğu, Batı Avrupa, Batı Hindistan, Brezilya Güney, Çin Doğu, Çin Kuzey, Doğu Asya, Güney Hindistan, Güneydoğu Asya, Hindistan Orta, Japonya Batı, Japonya Doğu, Kanada Doğu, Kanada Orta, Kore Güney, Kore Orta, Kuzey Avrupa, UK Batı ve UK Güney. Varsayılan hedef konum, Batı ABD 2 olarak ayarlanır. 
**Depolama yedekliliği** | Azure sanal makinelerinin geçişten sonra kullanacağı [depolama yedekliliği](https://docs.microsoft.com/azure/storage/common/storage-redundancy) türü. Yerel Olarak Yedekli Depolama (LRS) varsayılandır. Azure Geçişi’nin yalnızca yönetilen diskleri temel alan değerlendirmeleri desteklediğini ve yönetilen disklerin yalnızca LRS’yi desteklediğini; bu nedenle de özelliğin şu anda yalnızca LRS seçeneğini içerdiğini unutmayın. 
**Boyutlandırma Ölçütü** | Azure için sanal makineleri doğru şekilde boyutlandırmak üzere Azure Geçişi tarafından kullanılacak ölçüt. Şirket içi sanal makinelerin *performans geçmişini* temel alarak boyutlandırma yapabilir veya performans geçmişini dikkate almadan Azure için *şirket içi olarak* sanal makineleri boyutlandırabilirsiniz. Varsayılan değer, performans tabanlı boyutlandırmadır.
**Fiyatlandırma planları** | Maliyet hesaplamaları için bir değerlendirme, yazılım güvencesine sahip olup olmadığınızı ve [Azure Hibrit Avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/) için uygun olup olmadığınızı göz önünde bulundurur. Ayrıca, kaydolabileceğiniz [Azure Tekliflerini](https://azure.microsoft.com/support/legal/offer-details/) göz önünde bulundurur ve teklifle birlikte alabileceğiniz aboneliğe özel indirimleri (%) belirtmenize olanak sağlar. 
**Fiyatlandırma katmanı** | Hedef Azure sanal makineleri için [fiyatlandırma katmanını (Temel/Standart)](../virtual-machines/windows/sizes-general.md) belirtebilirsiniz. Örneğin, bir üretim ortamına geçiş yapmayı planlıyorsanız, daha düşük gecikme süresi ile sanal makineler sağlayan, ancak daha fazla maliyetli olabilecek Standart katmanını göz önünde bulundurmak istersiniz. Öte yandan, bir Geliştirme ve Test ortamınız varsa, daha yüksek gecikme süresi ve daha düşük maliyetle sanal makineler içeren Temel katmanını göz önünde bulundurmak isteyebilirsiniz. Varsayılan olarak [Standart](../virtual-machines/windows/sizes-general.md) katmanı kullanılır.
**Performans geçmişi** | Yalnızca boyutlandırma ölçütü performans tabanlı olduğunda uygulanabilir. Varsayılan olarak Azure Geçişi, %95 yüzdebirlik değer ile son bir günün performans geçmişini kullanarak şirket içi makinelerin performansını değerlendirir. Değerlendirme özelliklerinde bu değerleri değiştirebilirsiniz. 
**Konfor katsayısı** | Azure Geçişi, değerlendirme sırasında bir tamponu (konfor katsayısı) göz önünde bulundurur. Bu tampon, VM’lerin makine kullanım verilerinin (CPU, bellek, disk ve ağ) üzerine uygulanır. Konfor katsayısı; sezona özgü kullanım, kısa performans geçmişi ve gelecek kullanımlarda oluşabilecek artışlar gibi konuları hesaba katar.<br/><br/> Örneğin, %20 kullanıma sahip 10 çekirdekli bir VM normalde 2 çekirdekli VM ile sonuçlanır. Ancak, 2.0x konfor katsayısı ile sonuç 4 çekirdekli VM olur. Varsayılan konfor ayarı 1,3x’tir.


## <a name="how-does-azure-migrate-work"></a>Azure Geçişi nasıl çalışır?

1.  Bir Azure Geçişi projesi oluşturursunuz.
2.  Azure Geçişi, toplayıcı aleti adı verilen bir şirket içi VM kullanarak şirket içi makinelerinize ilişkin bilgileri bulur. Aleti oluşturmak için Open Virtualization Appliance (.ova) biçimindeki kurulum dosyasını indirirsiniz ve şirket içi vCenter Server’ınıza VM olarak aktarırsınız.
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
|Toplayıcı          |vCenter Server          |Varsayılan 9443   | Varsayılan olarak toplayıcı, 9443 numaralı bağlantı noktası üzerinden vCenter Server’a bağlanır. Sunucu farklı bir bağlantı noktasında dinliyorsa, toplayıcı VM üzerinde giden bağlantı noktası olarak yapılandırılması gerekir. |
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