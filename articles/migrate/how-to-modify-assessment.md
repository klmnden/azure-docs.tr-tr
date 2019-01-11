---
title: Azure geçişi değerlendirmesi ayarları özelleştirme | Microsoft Docs
description: Ayarlama ve Azure Migration Planner'ı kullanarak Azure'a geçirme VMware Vm'leri için değerlendirme çalıştırma açıklar
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 01/10/2019
ms.author: raynew
ms.openlocfilehash: 8419d7e7a91e4cbfd0eebfe00d35bf498cf5998c
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54200318"
---
# <a name="customize-an-assessment"></a>Bir değerlendirmeyi özelleştirme

[Azure geçişi](migrate-overview.md) değerlendirmeleri varsayılan özelliklerle oluşturur. Değerlendirme oluşturduktan sonra bu makaledeki yönergeleri kullanarak varsayılan özelliklerini değiştirebilirsiniz.


## <a name="edit-assessment-properties"></a>Değerlendirme özelliklerini Düzenle

1. Geçiş projesinin **Değerlendirmeler** sayfasında değerlendirmeyi seçin ve **Özellikleri düzenle**’ye tıklayın.
2. Aşağıdaki ayrıntılara bağlı olarak değerlendirme özelliklerini Özelleştir:

    **Ayar** | **Ayrıntılar** | **Varsayılan**
    --- | --- | ---
    **Hedef konum** | Geçişi yapmak istediğiniz Azure konumu.<br/><br/> Şu anda Azure Geçişi tarafından desteklenen 30 bölge şunlardır: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Orta Hindistan, Orta ABD, Çin Doğu, Çin Kuzey, Doğu Asya, Doğu ABD, Almanya Orta, Almanya Kuzeydoğu, Doğu ABD 2, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Orta Kuzey ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Güney Hindistan, UK Güney, UK Batı, US Gov Arizona, US Gov Teksas, US Gov Virginia, Orta Batı ABD, Batı Avrupa, Batı Hindistan, Batı ABD ve Batı ABD 2. |  Batı ABD 2, varsayılan konumdur.
    **Depolama türü** | Azure'da taşımak istediğiniz diskleri türünü belirtmek için bu özelliği kullanabilirsiniz. Boyutlandırma-şirket içi için hedef disk türünü Premium yönetilen diskler veya standart yönetilen diskler olarak belirtebilirsiniz. Performansa dayalı boyutlandırma için hedef disk türünü otomatik, Premium yönetilen diskler veya standart yönetilen diskler olarak belirtebilirsiniz. Otomatik olarak depolama türü belirttiğinizde, disk önerisi disklerin (IOPS ve aktarım hızı) performans verileri göre yapılır. Örneğin, elde etmek istiyorsanız bir [Tek Örnekli sanal makine SLA'sı % 99,9 düzeyinde](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/), Premium yönetilen diskler depolama türünü belirtmek isteyebilirsiniz. Bu, tüm diskler değerlendirmede Premium yönetilen diskler önerilen sağlar. Azure Geçişi’nin yönetilen diskleri yalnızca geçiş değerlendirmesi için desteklediğini unutmayın. | Varsayılan değer: Premium yönetilen diskler (boyutlandırma ölçütü ile *şirket içi olarak boyutlandırma*).
    **Ayrılmış Örnekler** |  Azure’da [ayrılmış örneklere](https://azure.microsoft.com/pricing/reserved-vm-instances/) sahip olup olmadığınızı belirttiğinizde Azure Geçişi, maliyeti buna göre tahmin eder. Ayrılmış örnekleri şu anda yalnızca Kullandıkça Öde teklifine Azure Geçişi'ndeki desteklenir. | 3 yıl ayrılmış örnekleri bu özellik için varsayılan değerdir.
    **Boyutlandırma ölçütü** | Azure için sanal makineleri doğru şekilde boyutlandırmak üzere Azure Geçişi tarafından kullanılacak ölçüt. Performans geçmişini dikkate almadan, *performans tabanlı* boyutlandırma yapabilir veya sanal makineleri *şirket içi olarak* boyutlandırabilirsiniz. | Varsayılan seçenek, performans tabanlı boyutlandırmadır.
    **Performans geçmişi** | Sanal makinelerin performansını değerlendirmek için dikkate alınacak süre. Bu özellik yalnızca boyutlandırma ölçütü *performans tabanlı boyutlandırma* olduğunda geçerlidir. | Varsayılan bir gündür.
    **Yüzdebirlik kullanımı** | Doğru boyutlandırma için dikkate alınacak performans örnek kümesinin yüzdebirlik değeri. Bu özellik yalnızca boyutlandırma ölçütü *performans tabanlı boyutlandırma* olduğunda geçerlidir.  | Varsayılan, 95. yüzdebirliktir.
    **VM serisi** | Uygun boyutlandırma için değerlendirmek istediğiniz bir VM serisini belirtebilirsiniz. Örneğin, A serisi Vm'lerde bulunan Azure geçirmeyi planlamadığınız bir üretim ortamınız varsa, A serisi listeden hariç tutabilirsiniz veya serisi ve boyutlandırmaya yalnızca seçili dizide gerçekleştirilir. | Varsayılan olarak tüm VM serisi seçilir.
    **Konfor katsayısı** | Azure Geçişi, değerlendirme sırasında bir tamponu (konfor katsayısı) göz önünde bulundurur. Bu tampon, VM’lerin makine kullanım verilerinin (CPU, bellek, disk ve ağ) üzerine uygulanır. Konfor katsayısı; sezona özgü kullanım, kısa performans geçmişi ve gelecek kullanımlarda oluşabilecek artışlar gibi konuları hesaba katar.<br/><br/> Örneğin, %20 kullanıma sahip 10 çekirdekli bir VM normalde 2 çekirdekli VM ile sonuçlanır. Ancak, 2.0x konfor katsayısı ile sonuç 4 çekirdekli VM olur. | Varsayılan ayar, 1.3x değeridir.
    **Teklif** | Kaydolduğunuz [Azure Teklifi](https://azure.microsoft.com/support/legal/offer-details/). | Varsayılan, [Kullandıkça öde](https://azure.microsoft.com/offers/ms-azr-0003p/)’dir.
    **Para Birimi** | Fatura para birimi. | Varsayılan, ABD Doları’dır.
    **İndirim (%)** | Azure teklifinin yanı sıra aldığınız, aboneliğe özgü indirim. | Varsayılan ayar, %0’dır.
    **VM çalışma süresi** | Sanal makinelerinizi Azure'da 7/24 çalıştırılması kullanmayacaksanız, bunlar çalıştırıyordur için süresi (gün / ay sayısı) ve her gün saat sayısı belirtebilirsiniz ve maliyet tahminleri buna göre uygulanır. | 31 gün / ay ve günde 24 saat varsayılan değerdir.
    **Azure Hibrit Avantajı** | Yazılım güvencesine sahip olup olmadığınızı ve [Azure Hibrit Avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/) için uygun olup olmadığınızı belirtin. Evet olarak ayarlanırsa, Windows sanal makineleri için Windows olmayan Azure fiyatları dikkate alınır. | Varsayılan değer Evet’tir.

3. Tıklayın **Kaydet** güncelleştirme değerlendirmesi için.

## <a name="faqs-on-assessment-properties"></a>Değerlendirme özellikleri hakkında SSS

### <a name="what-is-the-difference-between-as-on-premises-sizing-and-performance-based-sizing"></a>Olarak şirket içi boyutlandırma ve performans tabanlı boyutlandırma arasındaki fark nedir?

Olarak şirket içi olarak boyutlandırma ölçütü belirttiğinizde boyutlandırma, Azure Geçişi sanal makinelerin performans verilerini dikkate almaz ve şirket içi yapılandırmasını temel alan VM boyutları. Boyutlandırma ölçütü performans tabanlı olduğunda, boyutlandırma kullanım verilerine göre yapılır. Örneğin, 4 çekirdek içeren bir şirket içi sanal makine ve 8 GB bellek ile 50 CPU kullanımı ve % 50 bellek kullanımı ise. 4 çekirdek içeren bir Azure VM SKU'su boyutlandırma şirket olarak boyutlandırma ölçütü ise ve 8 GB bellek önerilir, ancak boyutlandırma ölçütü performansa dayalı sanal makine SKU'su 2 Çekirdek ve 4 GB önerilen olarak kullanım yüzdesi kabul ederken boyutu önerme.

Benzer şekilde, diskler için disk boyutlandırma ölçütü ve depolama türü boyutlandırma iki değerlendirme özelliklerine - bağlıdır. Boyutlandırma ölçütü performansa dayalı ve depolama türünün otomatik olduğundan, diskin IOPS ve aktarım hızı değerleri hedef disk türünü (standart veya Premium) tanımlamak için olarak kabul edilir. Boyutlandırma ölçütü performansa dayalı ve premium depolama türü ise, bir premium disk önerilir, premium disk SKU azure'da Seçili şirket içi disk boyutuna göre. Aynı mantığı boyutlandırma, şirket içi boyutlandırma olarak boyutlandırma ölçütü olduğunda ve depolama türü standart veya premium disk için kullanılır.

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>Boyut önerileri üzerinde performans geçmişi ve yüzdebirlik kullanımı nasıl bir etkisi var mı?

Bu özellikler, yalnızca performans tabanlı boyutlandırma için geçerlidir. Azure geçişi, şirket içi makinelerin performans geçmişi toplar ve Azure VM boyutu ve disk türü önermek için kullanır.

- Toplayıcı gerecini her 20 saniyede gerçek zamanlı kullanım verilerini toplamak için şirket içi ortamı sürekli olarak profiller.
- Gereç 20 saniye örneklerini yapar ve her 15 dakikada bir tek veri noktası oluşturur. Tek bir veri noktası oluşturmak için Gereci tüm 20 saniye örnekleri en yüksek değer seçer ve Azure'a gönderir.
- Değerlendirme performansı süresi ve performans geçmişi yüzdelik dilim değeri, göre azure'da oluşturduğunuzda, Azure geçişi etkili kullanımı değeri hesaplar ve boyutlandırma için kullanır.

Örneğin, performans süresi 1 gün ve 95 yüzdelik dilim değeri olarak ayarlarsanız, Azure geçişi toplayıcısı tarafından gönderilen son bir gün için 15 dakikalık örnek noktaları kullanıyorsa, artan düzende sıralar ve 95. yüzdebirlik etkin olarak seçer kullanımı. 95. yüzdebirlik 99. yüzdebirlik dilimde seçerseniz, gelebilir herhangi bir aykırı değer, yoksayıyorsunuz sağlar. En yüksek kullanımı olarak döneme ait seçmek istediğiniz ve herhangi bir aykırı değer kaçırmayın istemiyorsanız, 99. yüzdebirlik dilimde seçmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
