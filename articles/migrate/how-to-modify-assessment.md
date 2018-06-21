---
title: Azure geçirmek değerlendirme ayarlarını özelleştirme | Microsoft Docs
description: Ayarlar ve bir değerlendirme geçirme VMware Vm'leri için Azure geçiş Planlayıcısını kullanarak Azure çalıştırırsınız açıklar
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: 9ddd6c32388b2e05fd97138414958b67c009f9ee
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284922"
---
# <a name="customize-an-assessment"></a>Bir değerlendirmeyi özelleştirme

[Azure geçirme](migrate-overview.md) varsayılan özelliklere sahip değerlendirmeleri oluşturur. Bir değerlendirme oluşturduktan sonra bu makaledeki yönergeleri kullanarak varsayılan özelliklerini değiştirebilirsiniz.


## <a name="edit-assessment-properties"></a>Değerlendirme özelliklerini Düzenle

1. Geçiş projesinin **Değerlendirmeler** sayfasında değerlendirmeyi seçin ve **Özellikleri düzenle**’ye tıklayın.
2. Özellikleri aşağıdaki tabloya uygun şekilde değiştirin:

    **Ayar** | **Ayrıntılar** | **Varsayılan**
    --- | --- | ---
    **Hedef konum** | Geçişi yapmak istediğiniz Azure konumu.<br/><br/> Şu anda Azure Geçişi tarafından desteklenen 30 bölge şunlardır: ABD Batı, ABD Batı 2, ABD Doğu, ABD Doğu 2, ABD Orta, ABD Orta Batı, ABD Orta Güney, ABD Orta Kuzey, Almanya Kuzeydoğu, Almanya Orta, Avustralya Doğu, Avustralya Güneydoğu, Batı Avrupa, Batı Hindistan, Brezilya Güney, Çin Doğu, Çin Kuzey, Doğu Asya, Güney Hindistan, Güneydoğu Asya, Hindistan Orta, Japonya Batı, Japonya Doğu, Kanada Doğu, Kanada Orta, Kore Güney, Kore Orta, Kuzey Avrupa, UK Batı, UK Güney, US Gov Arizona, US Gov Teksas ve US Gov Virginia. |  Batı ABD 2 varsayılan konumdur.
    **Fiyatlandırma katmanı** | Hedef Azure sanal makineleri için [fiyatlandırma katmanını (Temel/Standart)](../virtual-machines/windows/sizes-general.md) belirtebilirsiniz. Örneğin, bir üretim ortamına geçiş yapmayı planlıyorsanız, daha düşük gecikme süresi ile sanal makineler sağlayan, ancak daha fazla maliyetli olabilecek Standart katmanını göz önünde bulundurmak istersiniz. Öte yandan, bir Geliştirme ve Test ortamınız varsa, daha yüksek gecikme süresi ve daha düşük maliyetle sanal makineler içeren Temel katmanını göz önünde bulundurmak isteyebilirsiniz. | Varsayılan olarak [Standart](../virtual-machines/windows/sizes-general.md) katmanı kullanılır.
    **Depolama türü** | Azure'da tahsis etmek istediğiniz diskler türünü belirtebilirsiniz. Bu özellik, boyutlandırma ölçütü içi boyutlandırma gibi olduğunda geçerlidir. Hedef disk türünün Premium yönetilen olarak da belirtebilirsiniz diskleri veya standart yönetilen disk. Performans tabanlı boyutlandırma için disk önerisini otomatik olarak VM'ler performans verilerine göre yapılır. Azure geçirmek yalnızca yönetilen diskleri geçiş değerlendirmesi için desteklediğini unutmayın. | Yönetilen Premium diskler varsayılan değerdir (boyutlandırma ölçüt olarak ile *gibi şirket içi boyutlandırma*).
    **Ayrılmış örnekler** |  Olup olmadığını belirtebilirsiniz [ayrılmış örnekleri](https://azure.microsoft.com/pricing/reserved-vm-instances/) Azure ve Azure geçirmek tahmini maliyet buna göre. Ayrılmış örnekler sovereign bölgeler (Azure kamu, Almanya ve Çin) geçerli değil ve yalnızca Azure geçirmek Kullandıkça Öde teklif uygulanabilir. | Bu özellik için varsayılan değer 3 yıl ayrılmış örnekler geçerlidir.
    **Boyutlandırma ölçütü** | Azure için sanal makineleri doğru şekilde boyutlandırmak üzere Azure Geçişi tarafından kullanılacak ölçüt. Performans geçmişini dikkate almadan, *performans tabanlı* boyutlandırma yapabilir veya sanal makineleri *şirket içi olarak* boyutlandırabilirsiniz. | Varsayılan seçenek, performans tabanlı boyutlandırmadır.
    **Performans geçmişi** | Sanal makinelerin performansını değerlendirmek için dikkate alınacak süre. Bu özellik yalnızca boyutlandırma ölçütü *performans tabanlı boyutlandırma* olduğunda geçerlidir. | Varsayılan bir gündür.
    **Yüzdebirlik kullanımı** | Doğru boyutlandırma için dikkate alınacak performans örnek kümesinin yüzdebirlik değeri. Bu özellik yalnızca boyutlandırma ölçütü *performans tabanlı boyutlandırma* olduğunda geçerlidir.  | Varsayılan, 95. yüzdebirliktir.
    **VM serisi** | Uygun boyutlandırma için değerlendirmek istediğiniz bir VM serisini belirtebilirsiniz. Örneğin, Azure’da A serisi VM’lere geçirmeyi planlamadığınız bir üretim ortamınız varsa, A serisini liste veya serilerin dışında bırakabilirsiniz, böylece uygun boyutlandırma yalnızca seçili serilerde yapılır. | Varsayılan olarak, tüm VM dizisi seçilir.
    **Konfor katsayısı** | Azure Geçişi, değerlendirme sırasında bir tamponu (konfor katsayısı) göz önünde bulundurur. Bu tampon, VM’lerin makine kullanım verilerinin (CPU, bellek, disk ve ağ) üzerine uygulanır. Konfor katsayısı; sezona özgü kullanım, kısa performans geçmişi ve gelecek kullanımlarda oluşabilecek artışlar gibi konuları hesaba katar.<br/><br/> Örneğin, %20 kullanıma sahip 10 çekirdekli bir VM normalde 2 çekirdekli VM ile sonuçlanır. Ancak, 2.0x konfor katsayısı ile sonuç 4 çekirdekli VM olur. | Varsayılan ayar, 1.3x değeridir.
    **Teklif** | Kaydolduğunuz [Azure Teklifi](https://azure.microsoft.com/support/legal/offer-details/). | Varsayılan, [Kullandıkça öde](https://azure.microsoft.com/offers/ms-azr-0003p/)’dir.
    **Para Birimi** | Fatura para birimi. | Varsayılan, ABD Doları’dır.
    **İndirim (%)** | Azure teklifinin yanı sıra aldığınız, aboneliğe özgü indirim. | Varsayılan ayar, %0’dır.
    **VM çalışır durumda kalma süresi** | Vm'leriniz 7 x 24 Azure'da çalıştırıyor değil kullanacaksanız, hangi bunlar çalışır durumda için süresi (ay / gün sayısı) ve her gün saat sayısını belirtebilirsiniz ve maliyet tahminler uygun şekilde gerçekleştirilir. | Varsayılan değer, her ay ve günde 24 saat 31 günde birdir.
    **Azure Hibrit Avantajı** | Yazılım güvencesine sahip olup olmadığınızı ve [Azure Hibrit Avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/) için uygun olup olmadığınızı belirtin. Evet olarak ayarlanırsa, Windows sanal makineleri için Windows olmayan Azure fiyatları dikkate alınır. | Varsayılan değer Evet’tir.

3. Tıklatın **kaydetmek** değerlendirme güncelleştirmek için.


## <a name="next-steps"></a>Sonraki adımlar

Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
