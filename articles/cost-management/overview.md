---
title: "Azure genel bakış maliyet Yönetimi Cloudyn tarafından | Microsoft Docs"
description: "Cloudyn’in sunduğu Azure Maliyet Yönetimi çözümü, Azure ve diğer bulut kaynaklarını kullanmanıza yardımcı olan çoklu bulut maliyet yönetimi çözümüdür."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: overview
ms.service: cost-management
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 969340080bfe2b04704367c2225895728773119e
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2017
---
# <a name="what-is-azure-cost-management"></a>Azure maliyeti Management nedir?

Azure maliyeti yönetim Cloudyn göre bulut kullanımı ve Azure kaynaklarınızı ve AWS ve Google gibi diğer bulut sağlayıcıları harcamalarını izlemenize olanak tanır. Kolay anlaşılır Pano Yardım maliyet ayırma ve showbacks/ödemelerin de raporlar. Maliyet yönetimi, daha sonra ayarlamak ve yönetebilirsiniz az kaynakları belirleyerek harcama bulut en iyi duruma getirme yardımcı olur.

Giriş bir video izlemek için bkz: [Cloudyn tarafından Azure maliyeti yönetimine giriş](https://youtu.be/NWIRny6Wpsk).

## <a name="monitor-usage-and-spending"></a>Kullanımını izleme ve harcama

Kuruluşlar zamanla kullanmak kaynaklar için ödeme kullanımınızı izleme ve harcama bulut altyapıları için oldukça önemlidir. Kullanım sözleşmesi eşiklerini aşarsa, beklenmeyen maliyet fazlalığı hızla ortaya çıkabilir. Birkaç önemli faktörler izleme geçici zorlaştırabilir. İlk olarak, ortalama kullanım amaçlarına göre maliyetleri yansıtma, tüketim belirli bir fatura döneminde tutarlı kalmasını varsayar. Maliyetleri yakın olan veya bütçenizi aşarsa, ikinci olarak, ileriye dönük olarak, harcama ayarlamak için bildirim almak önemlidir. Ve bulut hizmeti sağlayıcıları eşikleri veya dönem için dönem karşılaştırma raporları ve maliyet projeksiyon sunmayabilir.

Raporlar çözümlemek ve bulut kullanımı, maliyetleri ve eğilimleri izlemek için harcama izlemenize yardımcı olur. Zaman içinde kullanarak raporları, normal eğilimleri farklı anormallikleri algılayabilir. Bulut dağıtımınız verimsiz iyileştirme raporlarında görünür durumda. Ayrıca maliyet analiz raporunda verimsiz fark.

![Zaman içinde maliyeti raporu](media\overview\cost-over-time-rpt.png)


## <a name="manage-costs"></a>Maliyetlerini yönetme

Geçmiş verileri, eğilimleri belirlemek için zaman içinde kullanım ve maliyetleri çözümlediğinizde maliyetlerini yönetmek yardımcı olabilir. Eğilimleri, daha sonra gelecekteki Harcamaları tahmin etmek için kullanılır. Maliyet Yönetimi yararlı tahmini maliyet raporlar da içerir.

Maliyet ayırma etiketleme ilkesini temel alarak maliyetleriniz çözümleyerek maliyetleri yönetir. Maliyet ayırma iyileştirmek için özel hesaplar, kaynakları ve varlıkları etiketleri kullanabilirsiniz. Kategori Yöneticisi ek idare sağlanmasına yardımcı olmak amacıyla etiketlerinizi düzenler. Ve kaynak kullanımı ve tüketim davranışlarını etkilemek veya Kiracı müşteriler ücret ilişkili maliyetler göstermek için maliyet ayırma giderleri/geri ödeme için kullanın.

Erişim denetimi kullanıcılar ve takımlar yalnızca bunlar gerekli maliyet yönetim veri erişim sağlayarak maliyetleri yönetmeye yardımcı olur. Varlık yapısı, kullanıcı yönetimi ve zamanlanan raporlar alıcı listeleriyle erişim atamak için kullanın.

Uyarı olağan dışı harcama veya overspending oluştuğunda, otomatik olarak bildirerek maliyetleri yönetmenize yardımcı olur. Uyarılar ayrıca harcama anormalliklerini ve overspending riskler için otomatik olarak diğer Paydaşlar bildirebilir. Çeşitli raporlar bütçeye bağlı ve eşikleri maliyet uyarıları destekler. Ancak, uyarıları CSP ortağı hesapları ya da abonelik için şu anda desteklenmiyor.

## <a name="improve-efficiency"></a>Verimliliği artırmak

En iyi VM kullanımı belirlemek ve boşta Vm'leri tanımlamak veya boşta VM'ler ve maliyet yönetim eklenmemiş disklerle kaldırın. Boyutlandırma en iyi duruma getirme ve Etkisizliği raporları bilgileri kullanarak, aşağı boyutu veya boşta VM'ler kaldırmak için bir plan oluşturabilirsiniz. Ancak, en iyi duruma getirme raporları CSP ortağı hesapları ya da abonelik için şu anda desteklenmez.

![Boyutlandırma önerileri](.\media\overview\sizing.png)

AWS ayrılmış örnekler sağlanan, ayrılmış örnekler kullanımınızı burada satın alma önerileri görüntüleyebilir, kullanılmayan ayırmalarını değiştirmek ve sağlama planlama iyileştirme raporlarla geliştirebilir.

## <a name="next-steps"></a>Sonraki adımlar

Maliyet yönetimiyle tanıdık, sonraki bulut ortamınızı kaydetmek ve verilerinizi keşfetmeye başlamak için bir adımdır.

- [Tek bir Azure abonelik kaydetme](quick-register-azure-sub.md)
