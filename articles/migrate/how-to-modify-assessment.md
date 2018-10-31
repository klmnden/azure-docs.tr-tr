---
title: Azure geçişi değerlendirmesi ayarları özelleştirme | Microsoft Docs
description: Ayarlama ve Azure Migration Planner'ı kullanarak Azure'a geçirme VMware Vm'leri için değerlendirme çalıştırma açıklar
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 10/30/2018
ms.author: raynew
ms.openlocfilehash: d0cfab51b686b5b6eb9617d4424ac3f834de8d6f
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50241081"
---
# <a name="customize-an-assessment"></a>Bir değerlendirmeyi özelleştirme

[Azure geçişi](migrate-overview.md) değerlendirmeleri varsayılan özelliklerle oluşturur. Değerlendirme oluşturduktan sonra bu makaledeki yönergeleri kullanarak varsayılan özelliklerini değiştirebilirsiniz.


## <a name="edit-assessment-properties"></a>Değerlendirme özelliklerini Düzenle

1. Geçiş projesinin **Değerlendirmeler** sayfasında değerlendirmeyi seçin ve **Özellikleri düzenle**’ye tıklayın.
2. Aşağıdaki ayrıntılara bağlı olarak değerlendirme özelliklerini Özelleştir:

    **Ayar** | **Ayrıntılar** | **Varsayılan**
    --- | --- | ---
    **Hedef konum** | Geçişi yapmak istediğiniz Azure konumu.<br/><br/> Şu anda Azure Geçişi tarafından desteklenen 30 bölge şunlardır: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Orta Hindistan, Orta ABD, Çin Doğu, Çin Kuzey, Doğu Asya, Doğu ABD, Almanya Orta, Almanya Kuzeydoğu, Doğu ABD 2, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Orta Kuzey ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Güney Hindistan, UK Güney, UK Batı, US Gov Arizona, US Gov Teksas, US Gov Virginia, Orta Batı ABD, Batı Avrupa, Batı Hindistan, Batı ABD ve Batı ABD 2. |  Batı ABD 2, varsayılan konumdur.
    **Fiyatlandırma katmanı** | Hedef Azure sanal makineleri için [fiyatlandırma katmanını (Temel/Standart)](../virtual-machines/windows/sizes-general.md) belirtebilirsiniz. Örneğin, bir üretim ortamına geçiş yapmayı planlıyorsanız standart katmanını göz önünde istiyor. Öte yandan, bir Geliştirme ve Test ortamınız varsa, daha yüksek gecikme süresi ve daha düşük maliyetle sanal makineler içeren Temel katmanını göz önünde bulundurmak isteyebilirsiniz. | Varsayılan olarak [Standart](../virtual-machines/windows/sizes-general.md) katmanı kullanılır.
    **Depolama türü** | Azure'da atamak istediğiniz diskleri türünü belirtmek için bu özelliği kullanabilirsiniz. Boyutlandırma-şirket içi için hedef disk türünü Premium yönetilen diskler veya standart yönetilen diskler olarak belirtebilirsiniz. Performansa dayalı boyutlandırma için hedef disk türünü otomatik, Premium yönetilen diskler veya standart yönetilen diskler olarak belirtebilirsiniz. Otomatik olarak depolama türü belirttiğinizde, disk önerisi disklerin (IOPS ve aktarım hızı) performans verileri göre yapılır. Örneğin, elde etmek istiyorsanız bir [Tek Örnekli sanal makine SLA'sı % 99,9 düzeyinde](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/), tüm değerlendirmesi diskleri Premium yönetilen diskler olarak önerilen sağlayacak Premium yönetilen diskler olarak depolama türünü belirtmek isteyebilirsiniz. Azure Geçişi’nin yönetilen diskleri yalnızca geçiş değerlendirmesi için desteklediğini unutmayın. | Varsayılan değer: Premium yönetilen diskler (boyutlandırma ölçütü ile *şirket içi olarak boyutlandırma*).
    **Ayrılmış Örnekler** |  Azure’da [ayrılmış örneklere](https://azure.microsoft.com/pricing/reserved-vm-instances/) sahip olup olmadığınızı belirttiğinizde Azure Geçişi, maliyeti buna göre tahmin eder. Ayrılmış örnekler bağımsız bölgeler (Azure kamu, Almanya ve Çin) ilgili değildir ve bunlar yalnızca Kullandıkça Öde teklifi Azure Geçişi'ndeki geçerlidir. | 3 yıl ayrılmış örnekleri bu özellik için varsayılan değerdir.
    **Boyutlandırma ölçütü** | Azure için sanal makineleri doğru şekilde boyutlandırmak üzere Azure Geçişi tarafından kullanılacak ölçüt. Performans geçmişini dikkate almadan, *performans tabanlı* boyutlandırma yapabilir veya sanal makineleri *şirket içi olarak* boyutlandırabilirsiniz. | Varsayılan seçenek, performans tabanlı boyutlandırmadır.
    **Performans geçmişi** | Sanal makinelerin performansını değerlendirmek için dikkate alınacak süre. Bu özellik yalnızca boyutlandırma ölçütü *performans tabanlı boyutlandırma* olduğunda geçerlidir. | Varsayılan bir gündür.
    **Yüzdebirlik kullanımı** | Doğru boyutlandırma için dikkate alınacak performans örnek kümesinin yüzdebirlik değeri. Bu özellik yalnızca boyutlandırma ölçütü *performans tabanlı boyutlandırma* olduğunda geçerlidir.  | Varsayılan, 95. yüzdebirliktir.
    **VM serisi** | Uygun boyutlandırma için değerlendirmek istediğiniz bir VM serisini belirtebilirsiniz. Örneğin, Azure’da A serisi VM’lere geçirmeyi planlamadığınız bir üretim ortamınız varsa, A serisini liste veya serilerin dışında bırakabilirsiniz, böylece uygun boyutlandırma yalnızca seçili serilerde yapılır. | Varsayılan olarak tüm VM serisi seçilir.
    **Konfor katsayısı** | Azure Geçişi, değerlendirme sırasında bir tamponu (konfor katsayısı) göz önünde bulundurur. Bu tampon, VM’lerin makine kullanım verilerinin (CPU, bellek, disk ve ağ) üzerine uygulanır. Konfor katsayısı; sezona özgü kullanım, kısa performans geçmişi ve gelecek kullanımlarda oluşabilecek artışlar gibi konuları hesaba katar.<br/><br/> Örneğin, %20 kullanıma sahip 10 çekirdekli bir VM normalde 2 çekirdekli VM ile sonuçlanır. Ancak, 2.0x konfor katsayısı ile sonuç 4 çekirdekli VM olur. | Varsayılan ayar, 1.3x değeridir.
    **Teklif** | Kaydolduğunuz [Azure Teklifi](https://azure.microsoft.com/support/legal/offer-details/). | Varsayılan, [Kullandıkça öde](https://azure.microsoft.com/offers/ms-azr-0003p/)’dir.
    **Para Birimi** | Fatura para birimi. | Varsayılan, ABD Doları’dır.
    **İndirim (%)** | Azure teklifinin yanı sıra aldığınız, aboneliğe özgü indirim. | Varsayılan ayar, %0’dır.
    **VM çalışma süresi** | Sanal makinelerinizi Azure'da 7/24 çalıştırılması kullanmayacaksanız, bunlar çalıştırıyordur için süresi (gün / ay sayısı) ve her gün saat sayısı belirtebilirsiniz ve maliyet tahminleri uygun şekilde gerçekleştirilir. | 31 gün / ay ve günde 24 saat varsayılan değerdir.
    **Azure Hibrit Avantajı** | Yazılım güvencesine sahip olup olmadığınızı ve [Azure Hibrit Avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/) için uygun olup olmadığınızı belirtin. Evet olarak ayarlanırsa, Windows sanal makineleri için Windows olmayan Azure fiyatları dikkate alınır. | Varsayılan değer Evet’tir.

3. Tıklayın **Kaydet** güncelleştirme değerlendirmesi için.


## <a name="next-steps"></a>Sonraki adımlar

Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
