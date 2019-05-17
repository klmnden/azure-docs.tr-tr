---
title: Azure'da Cloudyn’e genel bakış | Microsoft Docs
description: Cloudyn, Azure ve diğer bulut kaynaklarını kullanmanıza yardımcı olan çoklu bulut maliyet yönetimi çözümüdür.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/14/2019
ms.topic: overview
ms.service: cost-management
manager: benshy
ms.custom: seodec18
ms.openlocfilehash: 8b989e5cf5b66d21c19d58f2f64fbba1927f5d69
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65792793"
---
# <a name="what-is-the-cloudyn-service"></a>Cloudyn hizmeti nedir?

Microsoft’un bir bağlı şirketi olan Cloudyn, Azure kaynaklarınızın yanı sıra AWS ve Google dahil diğer bulut sağlayıcıları için bulut kullanımınızı ve harcamalarınızı izlemenize imkan tanır. Anlaşılması kolay pano raporları, maliyet ayırma ve ücret hesaplama/yansıtma konusunda yardımcı olur. Cloudyn, yönetip ayarlamak isteyeceğiniz az kullanılan kaynakları belirleyerek bulut harcamalarınızı en iyi duruma getirmenize yardımcı olur.

Tanıtım videosunu izlemek için bkz. [Azure Cloudyn’e Giriş](https://azure.microsoft.com/resources/videos/azure-cost-management-overview-and-demo).

Azure Maliyet Yönetimi, Cloudyn'e benzer işlevler sunar. Azure Maliyet Yönetimi, yerel Azure maliyet yönetimi çözümüdür. Maliyet analizi yapmanıza, bütçe oluşturup yönetmenize, verileri dışarı aktarmanıza ve tasarruf önerilerini gözden geçirip gerekli eylemleri gerçekleştirmenize yardımcı olur. Daha fazla bilgi için bkz. [Azure Maliyet Yönetimi](overview-cost-mgt.md).

İzleme [Azure maliyet yönetimi ve Cloudyn video](https://www.youtube.com/watch?v=PmwFWwSluh8) iş gereksinimlerinize göre Azure maliyet Yönetimi veya Cloudyn kullanmalısınız önerileri görmek için.

>[!VIDEO https://www.youtube.com/embed/PmwFWwSluh8]

## <a name="cloudyn-features-moving-to-azure-cost-management"></a>Azure maliyet Yönetimi'ne taşıma Cloudyn özellikleri

Microsoft Cloudyn alınan ve maliyet yönetimi özellikleri Cloudyn portalından Azure'a yerel olarak geçirme. Yeni özellikleri kullanmak için oturum açma için Azure portalını ve gidin [maliyet yönetim ve Faturama](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) Azure hizmetleri listesinde. Cloudyn'e karşılaştırıldığında, Gelişmiş performans ve daha düşük veri gecikme süresi yaklaşık sekiz saat yerel deneyimi sunar.

Azure maliyet yönetimi, Kurumsal Anlaşma, Kullandıkça Öde ve MSDN Teklif kategorileri için temel bir özelliği geçiş tamamlanır. Üzerinden Azure maliyet Yönetimi'ne geçiş sürecinde CSP abonelikleri içindir.

Varsa bir teklif kategori henüz geçirilmiş, Cloudyn portalını kullanmaya devam etmeniz gerekir. Diğer herkes, Azure maliyet yönetimi kullanabilirsiniz.

| Microsoft Azure teklifleri ve özellikleri | Önerilen maliyet Yönetimi Hizmeti |
| --- | --- |
| Azure Kurumsal Anlaşma | [Azure Maliyet Yönetimi](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) |
| Azure Web Direct (PAYG/MSDN) | [Azure Maliyet Yönetimi](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) |
| Azure Kamu | [Azure Maliyet Yönetimi](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) |
| Azure CSP | [Cloudyn](https://azure.cloudyn.com) |
| Bulutlar arası maliyet analizi AWS desteği (önizlemede) | [Azure Maliyet Yönetimi](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) |
| AWS önerileri | [Cloudyn](https://azure.cloudyn.com) |

Aşağıdaki özelliklerden bazıları Cloudyn'de kullanılabilir, ancak bunların tümünü Azure maliyet Yönetimi'nde kullanıma sunuldu.

- API'ler
- Azure işlem önerileri
- Azure ayırma önerileri
- Bütçeler
- Maliyet analizi
- Azure depolama hesabınız için verileri dışarı aktarma
- Düşük gecikme süresi
- Power BI İçerik Paketi ve bağlayıcı
- Kaynak etiketi desteği

## <a name="monitor-usage-and-spending"></a>Kullanımı ve harcamayı izleyin

Kuruluşlar zaman içinde kullandığı kaynaklar için ödeme yaptığından bulut altyapıları için kullanımınızı ve harcamanızı izlemeniz kritik öneme sahiptir. Kullanım sözleşme eşiklerini aşarsa kısa bir sürede beklenmedik fazla kullanım ücretleriyle karşılaşılabilir. Birkaç önemli faktör geçici izlemeyi zorlaştırabilir. İlk olarak, ortalama kullanıma göre maliyet tahmini yapılırken belirli bir fatura dönemi boyunca tüketiminizin tutarlı kalacağı varsayılır. İkinci olarak, maliyetler bütçenize yaklaştığında veya bütçenizi aştığında harcamanızı proaktif olarak ayarlayabilmeniz için bildirim almanız önemlidir. Ayrıca, bulut hizmeti sağlayıcıları eşikle karşılaştırmalı maliyet tahmini veya dönemler arası karşılaştırma raporları sağlamıyor olabilir.

Raporlar, harcamayı izleyerek bulut kullanımını, maliyetleri ve eğilimleri analiz etmenize ve izlemenize yardımcı olur. Zaman İçinde raporlarını kullanarak normal eğilimlerden farklı olan anormal durumları algılayabilirsiniz. İyileştirme raporlarında bulut dağıtımınızdaki verimsizlikler görülebilir. Ayrıca, maliyet analizi raporlarında verimsizlikler olduğunu fark edebilirsiniz.

## <a name="manage-costs"></a>Maliyetleri yönetme

Zaman içinde kullanımı ve maliyetleri analiz ederek eğilimleri belirlemeniz durumunda geçmiş veriler maliyetleri yönetmenize yardımcı olabilir. Daha sonra bu eğilimler gelecekte yapılacak harcamaların tahmin edilmesinde kullanılabilir. Cloudyn, kullanışlı tahmini maliyet raporları da içerir.

Maliyet ayrımı, etiketleme ilkenize göre maliyetlerinizi analiz ederek maliyetleri yönetir. Maliyet ayırmayı daha ayrıntılı hale getirmek için özel hesaplarınızda, kaynaklarınızda ve varlıklarınızda etiketleri kullanabilirsiniz. Kategori Yöneticisi, etiketlerinizi düzenleyerek ek yönetim sağlanmasına yardımcı olur. Ayrıca, tüketim davranışlarını etkilemek veya kiracı müşterilere ücret uygulamak amacıyla ücret hesaplama/yansıtma için maliyet ayırmayı kullanarak kaynak kullanımını ve bununla ilişkili maliyetleri gösterirsiniz.

Erişim denetimi, kullanıcıların ve takımların yalnızca gereksinim duydukları maliyet yönetimi verilerine erişebilmesini sağlayarak maliyetlerin yönetilmesine yardımcı olur. Erişim atamak için varlık yapısı, kullanıcı yönetimi ve alıcı listeleri ile zamanlanmış raporları kullanabilirsiniz.

Uyarılar, olağan dışı veya fazla harcama gerçekleştiğinde size bildirimde bulunarak maliyetlerin yönetilmesine yardımcı olur. Ayrıca, uyarılar anormal harcama durumları ve fazla harcama riskleri konusunda başka ilgili kişilere de otomatik olarak bildirimde bulunabilir. Bütçe ve maliyet eşikleri konusunda uyarıları destekleyen çeşitli raporlar vardır. Bununla birlikte, CSP iş ortağı hesapları veya abonelikleri için şu an uyarılar desteklenmemektedir.

## <a name="improve-efficiency"></a>Verimliliği artırın

Cloudyn ile en uygun VM kullanımını belirleyebilir ve boş VM’leri tespit edebilir ya da boş VM’leri ve bağlı olmayan diskleri kaldırabilirsiniz. Boyutlandırma Optimizasyonu ve Verimsizlik raporlarındaki bilgileri kullanarak VM’lerin boyutunu düşürme veya boş VM’leri kaldırma planı oluşturabilirsiniz. Bununla birlikte, şu an CSP iş ortağı hesapları ve aboneliklerinde iyileştirme raporları desteklenmemektedir.

AWS Ayrılmış Örnekleri sağladıysanız satın alma önerilerini görüntüleme, kullanılmayan ayırmaları değiştirme ve sağlama planlama imkanı sunan Optimizasyon raporlarıyla ayrılmış örnek kullanımınızı geliştirebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Artık Cloudyn ile tanıştığınıza göre bir sonraki adım bulut ortamınızı kaydetmek ve verilerinizi keşfetmeye başlamaktır.

- [Tek bir Azure aboneliğini kaydetme](quick-register-azure-sub.md)
