---
title: Azure haritalar için fiyatlandırma katmanı sağ seçin | Microsoft Docs
description: Azure haritalar tarafından sağlanan katmanları fiyatlandırması hakkında bilgi edinin
author: walsehgal
ms.author: v-musehg
ms.date: 01/02/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 1bf60014b20f4cec29d661db4deb029fea4ab1d5
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59547352"
---
# <a name="choose-the-right-pricing-tier-in-azure-maps"></a>Azure haritalar fiyatlandırma katmanı sağ seçin

Azure haritalar, iki fiyatlandırma katmanı sunar. Bu makalenin amacı, fiyatlandırma katmanını gereksinimlerinize sağ seçmenize yardımcı olmaktır. Fiyatlandırma katmanı sağ seçmenize yardımcı olması için aşağıdaki iki soruları kendinize sorun.

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>Kullanmak hangi Jeo-uzamsal özellikler planlıyor musunuz?
Çekirdek Jeo-uzamsal API hizmeti gereksinimlerinizi karşılıyorsa S0 fiyatlandırma katmanını size uygun olduğunu. Uygulamanız için daha gelişmiş özellikleri isterseniz, kabul edilirse için S1 fiyatlandırma katmanını göz önünde bulundurun. Örnek özellikleri alma rota aralığı ve batch coğrafi kodlama karma resimler areal. **Fiyatlandırma katmanı özellikleri** aşağıdaki tabloda, uygulamanızın ihtiyaçlarına daha iyi bir fikir verir. Ayrıca, uygulamanız için en uygun bir fiyatlandırma katmanı seçmenize yardımcı olur.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>Desteklemek kaç tane eş zamanlı kullanıcı planlıyor musunuz? 
S0 ve S1 fiyatlandırma katmanları farklı miktarda veri aktarım hızı işleyin. Fiyatlandırma Katmanı'nın bir Azure haritalar'ı seçmeden önce kendinize bazı sorular sorun. "Kaç tane eş zamanlı kullanıcı miyim desteklemek istiyor musunuz?" örneğidir Fiyatlandırma katmanı S0 işleme kadar **Saniyedeki sorgu sayısı 50**. S1 fiyatlandırma katmanını tutamaçları **Saniyedeki sorgu sayısı 50'den fazla**.

### <a name="pricing-tier-capabilities"></a>Fiyatlandırma katmanı özellikleri

| Özellik                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Arama (İleri Sar/ters coğrafi kodlama, ilgilenilen noktaları)  |        ✓           |     ✓    |
| Batch coğrafi kodlama (Önizleme)              |                   |     ✓    |
| Aramaya ilişkin çokgenler          |                   |     ✓    |
| Yönlendirme                                 |        ✓           |     ✓    |
| Rota aralığı                    |                   |     ✓    |
| Toplu iş (Önizleme) yönlendirme                |                   |     ✓    |
| Matris (Önizleme) yönlendirme               |                   |     ✓    |
| İşleme                                  |        ✓           |     ✓    |
| Gözünüzde yanı sıra karma resimler    |            |     ✓    |
| Trafik                                 |        ✓           |     ✓    |
| Saat dilimleri                              |        ✓           |     ✓    |
| Coğrafi konum (Önizleme)                |        ✓           |     ✓    |
| Veri (Önizleme)               |                   |     ✓    |
| Uzamsal (Önizleme)               |                   |     ✓    |
| Bölge sınırlaması (Önizleme)               |                   |     ✓    |



Bu ek veri noktaları, dikkate değer vardır:
* Ne tür bir kurumsal var mı?
* Ne kadar kritik olduğunu oluşturulmakta olan uygulama?

Bkz: **hedeflenen müşteriler fiyatlandırma katmanı** tablo S0 ve S1 fiyatlandırma katmanlarının daha iyi bir fikir edinebilirsiniz. Daha fazla bilgi için [Azure haritalar fiyatlandırma](https://azure.microsoft.com/pricing/details/azure-maps/). 

### <a name="pricing-tier-targeted-customers"></a>Müşteriler fiyatlandırma katmanı hedeflenen

| Fiyatlandırma katmanı  |     Hedeflenen müşteriler                                                                |
|---------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>S0 fiyatlandırma katmanı küçük veya orta ölçekli işletmelerin isteyen müşterilere yöneliktir. Bu, yüksek hacimli eşzamanlı kullanıcı düşünmüyorsanız, fiyatlandırma katmanı sağ olur. Yukarıdaki tabloda gösterilen API'leri çekirdek Jeo uzamsal hizmet gereksinimlerinizi karşılıyorsa da sağ. Bu genel kullanıma sunulmuştur. Uygulamalar için tüm üretim aşamada çalışır: kavram kanıtı geliştirme ve test uygulama üretim ve dağıtım için erken aşama.<p>|
| S1            |    <p>S1 fiyatlandırma katmanını, büyük ölçekli Kurumsal, görev açısından kritik uygulamaları veya yüksek hacimli eşzamanlı kullanıcı için destek geçirilmesi gereken müşterilere yöneliktir. Ayrıca Gelişmiş Jeo-uzamsal hizmetler ihtiyaç duyan müşteriler için bir hizmettir.</p>|

## <a name="next-steps"></a>Sonraki adımlar

Görüntüle ve Değiştir fiyatlandırma katmanları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"] 
> [Bir fiyatlandırma Katmanı'nı yönetme](how-to-manage-pricing-tier.md)
