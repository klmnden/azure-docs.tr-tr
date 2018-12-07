---
title: Azure haritalar için fiyatlandırma katmanı sağ seçin | Microsoft Docs
description: Azure haritalar tarafından sağlanan katmanları fiyatlandırması hakkında bilgi edinin
author: walsehgal
ms.author: v-musehg
ms.date: 12/05/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: ee277867f449afddeb89c3fd73b5b577a68a4497
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52998375"
---
# <a name="choosing-the-right-pricing-tier-in-azure-maps"></a>Azure haritalar fiyatlandırma katmanı sağ seçme

Azure haritalar, iki fiyatlandırma katmanı sunar. Bu makalenin amacı, fiyatlandırma katmanını gereksinimlerinize sağ seçmenize yardımcı olmaktır. Fiyatlandırma katmanı sağ seçmenize yardımcı olması için iki soruları kendinize sorun:

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>Kullanmak hangi Jeo-uzamsal özellikler planlıyor musunuz?
Düşünüyorsanız, hizmet gereksinimlerinizi çekirdek Jeo uzamsal API'leri tarafından karşılandığından, sonra fiyatlandırma katmanı S0 size uygun olduğunu. Uygulamanızın areal + karma resimler gibi daha gelişmiş özellikler istiyorsanız, rota aralığı alma vb. coğrafi kodlama toplu, edilmesiyle için S1 fiyatlandırma katmanını göz önünde bulundurun. Aşağıdaki tabloda ile **fiyatlandırma katmanı özellikleri** uygulamanızın ihtiyaçlarına daha iyi bir fikir sağlar ve ayrıca seçtiğiniz bir fiyatlandırma katmanı uygulamanız için en uygun yardım eder.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>Desteklemek kaç tane eş zamanlı kullanıcı planlıyor musunuz? 
S0 ve S1 fiyatlandırma katmanları farklı miktarda veri aktarım hızını işleyebilir. Fiyatlandırma Katmanı'nın bir Azure haritalar'ı seçmeden önce kendiniz nasıl çok sayıda eşzamanlı kullanıcıyı desteklemek istediğiniz gibi sorular sorabilirsiniz? Fiyatlandırma katmanı kadar işleyebilir S0 **Saniyedeki sorgu sayısı 50** ve S1 fiyatlandırma katmanını işleyebilir **Saniyedeki sorgu sayısı 50'den fazla**.

### <a name="pricing-tier-capabilities"></a>Fiyatlandırma katmanı özellikleri

| Özellik                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Arama                                  |        ✓           |     ✓    |
| Yönlendirme                                 |        ✓           |     ✓    |
| İşleme                                  |        ✓           |     ✓    |
| Trafik                                 |        ✓           |     ✓    |
| Saat dilimleri                              |        ✓           |     ✓    |
| * Tanımayı + karma resimler (Önizleme)     |        ✓           |     ✓    |
| * Rota aralığı (Önizleme)                  |        ✓           |     ✓    |
| * IP 2 konumu (Önizleme)                |        ✓           |     ✓    |
| * Çokgenler arama (Önizleme)         |        ✓           |     ✓    |
| * Toplu coğrafi kodlama (Önizleme)              |        ✓           |     ✓    |
| * Batch (Önizleme) yönlendirme                |        ✓           |     ✓    |
| * Matris yönlendirme (Önizleme)               |        ✓           |     ✓    |

> [!Note]
> Fiyatlandırma katmanı S0 bu özelliklerden erişimi 4 Şubat 2019 sonra kullanımdan kaldırılacaktır.

Bazı ek veri noktaları dikkate değer, ne tür bir kuruluş, belirtilmiş veya uygulamanın ne kadar kritik oluşturulmakta?

İle tabloya bakın **fiyatlandırma katmanı müşterileri hedeflenen** S0 ve S1 fiyatlandırma katmanlarının daha iyi bir fikir edinebilirsiniz. Azure haritalar fiyatlandırma hakkında daha fazla bilgi için [Azure haritalar fiyatlandırma](https://azure.microsoft.com/pricing/details/azure-maps/). 

### <a name="pricing-tier-targeted-customers"></a>Müşteriler fiyatlandırma katmanı hedeflenen

| Fiyatlandırma katmanı  |        Hedeflenen müşteriler                                                                |
|---------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>S0 fiyatlandırma katmanı küçük veya orta ölçekli işletmelerin isteyen müşterilere yöneliktir. Bu, yüksek hacimli eşzamanlı kullanıcı beklemiyoruz ve hizmet gereksinimlerinizi çekirdek Jeo-uzamsal aşağıdaki tabloda gösterildiği gibi API'leri tarafından karşılandığından, fiyatlandırma katmanını hakkı olur. Bu katman genel kullanıma sunuldu ve tüm geliştirme kavram kanıtı üretimden aşamalarını ve erken aşama uygulama üretim ve dağıtım için test uygulamaları için geçerlidir.<p>|
| S1            |    <p>S1 fiyatlandırma katmanını büyük ölçekli kurumsal iş açısından kritik uygulamalar, yüksek hacimli eşzamanlı kullanıcı için destek geçirilmesi gereken müşterilere yöneliktir ve gelişmiş bir Jeo-uzamsal hizmetler gerektirir.</p>|


## <a name="next-steps"></a>Sonraki adımlar

Görüntüleme ve fiyatlandırma katmanını değiştirme hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Fiyatlandırma Katmanı'nı yönetme](how-to-manage-pricing-tier.md)
