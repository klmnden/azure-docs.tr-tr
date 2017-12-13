---
title: "Azure geçirmek değerlendirme ayarlarını özelleştirme | Microsoft Docs"
description: "Ayarlar ve bir değerlendirme geçirme VMware Vm'leri için Azure geçiş Planlayıcısını kullanarak Azure çalıştırırsınız açıklar"
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: ce47790f6214864afdba33eb5cbe3a9e49b81cd5
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="customize-an-assessment"></a>Bir değerlendirmeyi özelleştirme

[Azure geçirme](migrate-overview.md) varsayılan ayarlarla değerlendirmeleri oluşturur. Bir değerlendirme oluşturduktan sonra bu makaledeki yönergeleri kullanarak bu varsayılan ayarları değiştirebilirsiniz.


## <a name="edit-assessment-values"></a>Değerlendirme değerlerini düzenleme

1. Azure geçirmek projesinde **değerlendirmeleri** sayfasında değerlendirme seçin ve tıklayın **özelliklerini düzenleme**.
2. Aşağıdaki tabloda göre ayarları değiştirin.

    **Ayar** | **Ayrıntılar** | **Varsayılan**
    --- | --- | ---
    **Hedef konum** | Geçişi yapmak istediğiniz Azure konumu. |  Batı ABD 2 varsayılan konumdur.
    **Depolama yedekliliği** | Azure VM’lerinin geçişten sonra kullanacağı depolama türü. | Yalnızca [yerel olarak yedekli depolama (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) çoğaltma şu anda desteklenmiyor.
    **Konfor katsayısı** | Rahatlık, değerlendirme sırasında kullanılan arabellek faktördür. Kullanın Mevsimlik kullanımı gibi şeyler için hesabı, performans geçmişi kısa, büyük olasılıkla gelecekte kullanımını artırır. | Varsayılan ayardır 1.3 x.
    **Perfomance geçmişi** | Performans geçmişi değerlendirmede kullanılan süre. | Bir ay varsayılandır.
    **Yüzdelik kullanımı** | Perfomance geçmişi için dikkate alınması gereken yüzdelik değer. | Varsayılan değer % 95 ' dir.
    **Fiyatlandırma katmanı** | Belirleyebileceğiniz [fiyatlandırma katmanı](https://azure.microsoft.com/blog/basic-tier-virtual-machines-2/) bir VM için.  | Varsayılan olarak [standart](../virtual-machines/windows/sizes-general.md) katmanı kullanılır.
    **Teklif** | [Azure teklifleri](https://azure.microsoft.com/support/legal/offer-details/) geçerli. | [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) varsayılandır.
    **Para birimi** | Fatura para birimi. | ABD Doları varsayılandır.
    **İndirim (%)** | Herhangi bir abonelik özgü iskonto üzerinde herhangi bir teklif alırsınız. | % 0 varsayılan ayardır.
    **Azure karma kullanımı avantajı** | Kayıtlı olup olmadığını gösteren [Azure karma kullanımı avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Evet olarak ayarlanırsa, Windows Azure fiyatlar dikkat edilmelidir, Windows VM'ler için. | Varsayılan değer Evet’tir.

3. Tıklatın **kaydetmek** değerlendirme güncelleştirmek için.


## <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](concepts-assessment-calculation.md) değerlendirmelerinin nasıl hesaplandığını hakkında.
