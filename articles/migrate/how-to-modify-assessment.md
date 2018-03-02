---
title: "Azure geçirmek değerlendirme ayarlarını özelleştirme | Microsoft Docs"
description: "Ayarlar ve bir değerlendirme geçirme VMware Vm'leri için Azure geçiş Planlayıcısını kullanarak Azure çalıştırırsınız açıklar"
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 02/26/2018
ms.author: raynew
ms.openlocfilehash: efb4ad59d25a0c1209e4f0f6cd406c2f0d48159c
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="customize-an-assessment"></a>Bir değerlendirmeyi özelleştirme

[Azure geçirme](migrate-overview.md) varsayılan özelliklere sahip değerlendirmeleri oluşturur. Bir değerlendirme oluşturduktan sonra bu makaledeki yönergeleri kullanarak varsayılan özelliklerini değiştirebilirsiniz.


## <a name="edit-assessment-properties"></a>Değerlendirme özelliklerini Düzenle

1. İçinde **değerlendirmeleri** sayfasında geçiş proje, değerlendirme seçin ve tıklatın **özelliklerini düzenleme**.
2. Özellikler aşağıdaki tabloda uygun olarak değiştirin:

    **Ayar** | **Ayrıntılar** | **Varsayılan**
    --- | --- | ---
    **Hedef konum** | Geçişi yapmak istediğiniz Azure konumu.<br/><br/> Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Merkezi, Doğu Kanada, Orta Hindistan, Orta ABD, Çin, Doğu, Kuzey Çin, Doğu Asya, Doğu ABD, Almanya Merkezi, Almanya Kuzeydoğu, Doğu ABD 2, Japonya dahil olmak üzere 30 bölgeler şu anda Azure geçirme destekler Doğu, Japonya Batı, Orta, Kore Güney Kore, Kuzey Orta ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Güney Hindistan, Birleşik Krallık Güney, Birleşik Krallık Batı, Batı Orta ABD, Batı Avrupa, Batı Hindistan, Batı ABD ve Batı US2. |  Batı ABD 2 varsayılan konumdur.
    **Depolama yedekliliği** | Azure sanal makinelerini geçişten sonra kullanacağı depolama artıklığı türü. | [Yerel olarak yedekli depolama (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) varsayılan değerdir. Disk tabanlı değerlendirmeleri Azure geçirme yalnızca destekler yönetilen ve yönetilen diskler yalnızca LRS destekler, bu nedenle özelliği şu anda yalnızca LRS seçeneği vardır. 
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
