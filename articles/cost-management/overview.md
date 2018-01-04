---
title: "Cloudyn Tarafından Sağlanan Azure Maliyet Yönetimi’ne Genel Bakış | Microsoft Docs"
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
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2017
---
# <a name="what-is-azure-cost-management"></a>Azure Maliyet Yönetimi nedir?

Cloudyn Tarafından Sağlanan Azure Maliyet Yönetimi, Azure kaynaklarınızın yanı sıra AWS ve Google dahil diğer bulut sağlayıcıları için bulut kullanımınızı ve harcamalarınızı izlemenize imkan tanır. Anlaşılması kolay pano raporları, maliyet ayırma ve ücret hesaplama/yansıtma konusunda yardımcı olur. Maliyet Yönetimi, yönetip ayarlamak isteyeceğiniz az kullanılan kaynakları belirleyerek bulut harcamalarınızı en iyi duruma getirmenize yardımcı olur.

Tanıtım videosunu izlemek için bkz. [Cloudyn tarafından sağlanan Azure Maliyet Yönetimi’ne giriş](https://youtu.be/NWIRny6Wpsk).

## <a name="monitor-usage-and-spending"></a>Kullanımı ve harcamayı izleyin

Kuruluşlar zaman içinde kullandığı kaynaklar için ödeme yaptığından bulut altyapıları için kullanımınızı ve harcamanızı izlemeniz kritik öneme sahiptir. Kullanım sözleşme eşiklerini aşarsa kısa bir sürede beklenmedik fazla kullanım ücretleriyle karşılaşılabilir. Birkaç önemli faktör, geçici izlemeyi zorlaştırabilir. İlk olarak, ortalama kullanıma göre maliyet tahmini yapılırken belirli bir fatura dönemi boyunca tüketiminizin tutarlı kalacağı varsayılır. İkinci olarak, maliyetler bütçenize yaklaştığında veya bütçenizi aştığında harcamanızı proaktif olarak ayarlayabilmeniz için bildirim almanız önemlidir. Ayrıca, bulut hizmeti sağlayıcıları eşikle karşılaştırmalı maliyet tahmini veya dönemler arası karşılaştırma raporları sağlamıyor olabilir.

Raporlar, harcamayı izleyerek bulut kullanımını, maliyetleri ve eğilimleri analiz etmenize ve izlemenize yardımcı olur. Zaman İçinde raporlarını kullanarak normal eğilimlerden farklı olan anormal durumları algılayabilirsiniz. İyileştirme raporlarında bulut dağıtımınızdaki verimsizlikler görülebilir. Ayrıca, maliyet analizi raporlarında verimsizlikler olduğunu fark edebilirsiniz.

![Zaman İçinde Maliyet raporu](media\overview\cost-over-time-rpt.png)


## <a name="manage-costs"></a>Maliyetleri yönetme

Zaman içinde kullanımı ve maliyetleri analiz ederek eğilimleri belirlemeniz durumunda geçmiş veriler maliyetleri yönetmenize yardımcı olabilir. Daha sonra bu eğilimler gelecekte yapılacak harcamaların tahmin edilmesinde kullanılabilir. Maliyet Yönetimi, kullanışlı tahmini maliyet raporları da içerir.

Maliyet ayrımı, etiketleme ilkenize göre maliyetlerinizi analiz ederek maliyetleri yönetir. Maliyet ayırmayı daha ayrıntılı hale getirmek için özel hesaplarınızda, kaynaklarınızda ve varlıklarınızda etiketleri kullanabilirsiniz. Kategori Yöneticisi, etiketlerinizi düzenleyerek ek yönetim sağlanmasına yardımcı olur. Ayrıca, tüketim davranışlarını etkilemek veya kiracı müşterilere ücret uygulamak amacıyla ücret hesaplama/yansıtma için maliyet ayırmayı kullanarak kaynak kullanımını ve bununla ilişkili maliyetleri gösterirsiniz.

Erişim denetimi, kullanıcıların ve takımların yalnızca gereksinim duydukları maliyet yönetimi verilerine erişebilmesini sağlayarak maliyetlerin yönetilmesine yardımcı olur. Erişim atamak için varlık yapısı, kullanıcı yönetimi ve alıcı listeleri ile zamanlanmış raporları kullanabilirsiniz.

Uyarılar, olağan dışı veya fazla harcama gerçekleştiğinde size bildirimde bulunarak maliyetlerin yönetilmesine yardımcı olur. Ayrıca, uyarılar anormal harcama durumları ve fazla harcama riskleri konusunda başka ilgili kişilere de otomatik olarak bildirimde bulunabilir. Bütçe ve maliyet eşikleri konusunda uyarıları destekleyen çeşitli raporlar vardır. Bununla birlikte, CSP iş ortağı hesapları veya abonelikleri için şu an uyarılar desteklenmemektedir.

## <a name="improve-efficiency"></a>Verimliliği artırın

Maliyet Yönetimi ile en uygun VM kullanımını belirleyebilir ve boş VM’leri tespit edebilir ya da boş VM’leri ve bağlı olmayan diskleri kaldırabilirsiniz. Boyutlandırma Optimizasyonu ve Verimsizlik raporlarındaki bilgileri kullanarak VM’lerin boyutunu düşürme veya boş VM’leri kaldırma planı oluşturabilirsiniz. Bununla birlikte, şu an CSP iş ortağı hesapları ve aboneliklerinde iyileştirme raporları desteklenmemektedir.

![boyutlandırma önerileri](.\media\overview\sizing.png)

AWS Ayrılmış Örnekleri sağladıysanız satın alma önerilerini görüntüleme, kullanılmayan ayırmaları değiştirme ve sağlama planlama imkanı sunan Optimizasyon raporlarıyla ayrılmış örnek kullanımınızı geliştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık Maliyet Yönetimi’yle tanıştığınıza göre bir sonraki adım bulut ortamınızı kaydetmek ve verilerinizi keşfetmeye başlamaktır.

- [Tek bir Azure aboneliğini kaydetme](quick-register-azure-sub.md)
