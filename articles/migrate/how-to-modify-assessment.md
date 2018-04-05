---
title: Azure geçirmek değerlendirme ayarlarını özelleştirme | Microsoft Docs
description: Ayarlar ve bir değerlendirme geçirme VMware Vm'leri için Azure geçiş Planlayıcısını kullanarak Azure çalıştırırsınız açıklar
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 02/26/2018
ms.author: raynew
ms.openlocfilehash: 459a29012ec879d4d4989e51b5688b9042adc1a1
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="customize-an-assessment"></a>Bir değerlendirmeyi özelleştirme

[Azure geçirme](migrate-overview.md) varsayılan özelliklere sahip değerlendirmeleri oluşturur. Bir değerlendirme oluşturduktan sonra bu makaledeki yönergeleri kullanarak varsayılan özelliklerini değiştirebilirsiniz.


## <a name="edit-assessment-properties"></a>Değerlendirme özelliklerini Düzenle

1. İçinde **değerlendirmeleri** sayfasında geçiş proje, değerlendirme seçin ve tıklatın **özelliklerini düzenleme**.
2. Özellikler aşağıdaki tabloda uygun olarak değiştirin:

    **Ayar** | **Ayrıntılar** | **Varsayılan**
    --- | --- | ---
    **Hedef konum** | Geçişi yapmak istediğiniz Azure konumu.<br/><br/> Şu anda Azure Geçişi tarafından desteklenen 30 bölge şunlardır: ABD Batı, ABD Batı 2, ABD Doğu, ABD Doğu 2, ABD Orta, ABD Orta Batı, ABD Orta Güney, ABD Orta Kuzey, Almanya Kuzeydoğu, Almanya Orta, Avustralya Doğu, Avustralya Güneydoğu, Batı Avrupa, Batı Hindistan, Brezilya Güney, Çin Doğu, Çin Kuzey, Doğu Asya, Güney Hindistan, Güneydoğu Asya, Hindistan Orta, Japonya Batı, Japonya Doğu, Kanada Doğu, Kanada Orta, Kore Güney, Kore Orta, Kuzey Avrupa, UK Batı ve UK Güney. |  Batı ABD 2 varsayılan konumdur.
    **Depolama yedekliliği** | Azure sanal makinelerini geçişten sonra kullanacağı depolama artıklığı türü. | [Yerel olarak yedekli depolama (LRS)](../storage/common/storage-redundancy-lrs.md) varsayılan değerdir. Disk tabanlı değerlendirmeleri Azure geçirme yalnızca destekler yönetilen ve yönetilen diskler yalnızca LRS destekler, bu nedenle özelliği şu anda yalnızca LRS seçeneği vardır. 
    **Boyutlandırma ölçütü** | Azure için sanal makineleri doğru şekilde boyutlandırmak üzere Azure Geçişi tarafından kullanılacak ölçüt. Her iki yapabilirsiniz *performans tabanlı* boyutlandırma veya VM'ler boyut *şirket içi olarak*, performans geçmişi dikkate olmadan. | Performans tabanlı boyutlandırma varsayılan seçenektir.
    **Performans geçmişi** | VM'lerin performansını değerlendirmek için dikkate alınması gereken süre. Bu özellik yalnızca boyutlandırma ölçütü olduğunda geçerlidir *performans tabanlı boyutlandırma*. | Varsayılan bir gündür.
    **Yüzdelik kullanımı** | Yüzdelik değer performans örnek boyutlandırmaya için kabul edilmesi için ayarlayın. Bu özellik yalnızca boyutlandırma ölçütü olduğunda geçerlidir *performans tabanlı boyutlandırma*.  | 95 varsayılandır.
    **Fiyatlandırma katmanı** | Hedef Azure sanal makineleri için [fiyatlandırma katmanını (Temel/Standart)](../virtual-machines/windows/sizes-general.md) belirtebilirsiniz. Örneğin, bir üretim ortamına geçiş yapmayı planlıyorsanız, daha düşük gecikme süresi ile sanal makineler sağlayan, ancak daha fazla maliyetli olabilecek Standart katmanını göz önünde bulundurmak istersiniz. Öte yandan, bir Geliştirme ve Test ortamınız varsa, daha yüksek gecikme süresi ve daha düşük maliyetle sanal makineler içeren Temel katmanını göz önünde bulundurmak isteyebilirsiniz. | Varsayılan olarak [Standart](../virtual-machines/windows/sizes-general.md) katmanı kullanılır.
    **Konfor katsayısı** | Azure Geçişi, değerlendirme sırasında bir tamponu (konfor katsayısı) göz önünde bulundurur. Bu tampon, VM’lerin makine kullanım verilerinin (CPU, bellek, disk ve ağ) üzerine uygulanır. Konfor katsayısı; sezona özgü kullanım, kısa performans geçmişi ve gelecek kullanımlarda oluşabilecek artışlar gibi konuları hesaba katar.<br/><br/> Örneğin, %20 kullanıma sahip 10 çekirdekli bir VM normalde 2 çekirdekli VM ile sonuçlanır. Ancak, 2.0x konfor katsayısı ile sonuç 4 çekirdekli VM olur. | Varsayılan ayardır 1.3 x.
    **Teklif** | [Azure teklifi](https://azure.microsoft.com/support/legal/offer-details/) için kaydedilen. | [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) varsayılandır.
    **Para birimi** | Fatura para birimi. | ABD Doları varsayılandır.
    **İndirim (%)** | Herhangi bir abonelik özgü iskonto üstünde Azure teklifi alırsınız. | % 0 varsayılan ayardır.
    **Azure karma avantajı** | Yazılım Güvencesi sahip ve için uygun belirtin [Azure karma avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Evet olarak ayarlanırsa, Windows Azure fiyatlar Windows VM'ler için kabul edilen durumunda. | Varsayılan değer Evet’tir.

3. Tıklatın **kaydetmek** değerlendirme güncelleştirmek için.


## <a name="next-steps"></a>Sonraki adımlar

Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
