---
title: Azure Güvenlik Merkezi'nde puanı güvenli | Microsoft Docs
description: " Azure Güvenlik Merkezi'nde güvenli puanı kullanarak, güvenlik önerileri öncelik verin. "
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: rkarlin
ms.openlocfilehash: fc521db9ad753c4162b65abfd2f9f23c318fa994
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46131325"
---
# <a name="improve-your-secure-score-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenli puanınız geliştirin


Güvenlik açısından faydalı sunan çok sayıda hizmetleriyle ilk güvenli ve iş yükünüz sağlamlaştırmak için hangi adımlar bilmek zordur. Azure Güvenlik Merkezi güvenli puanı, güvenlik önerileri inceler ve ilk olarak gerçekleştirmek için hangi önerilerin bilmesi bunları sizin için araştırma önceliğini belirleyebilmek en önemli güvenlik açıklarını bulmanıza yardımcı olacak önceliklendirir. Güvenli puanı güvenli bir iş yükü elde etmek için güvenlik sağlamlaştırmak yardımcı olan bir ölçüm aracıdır.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]


![Puan Pano güvenliğini sağlama](./media/security-center-secure-score/secure-score-dashboard.png)

## <a name="secure-score-calculation"></a>Puan hesaplanmasında güvenliğini sağlama

Güvenlik Merkezi, güvenlik önerilerinizi gözden geçirme ve her önerinin nasıl önemli olduğunu belirlemek için gelişmiş algoritmalar uygulayarak güvenlik analisti çalışmasını taklit eder.
Azure Güvenlik Merkezi sürekli etkin öneri incelemeleri ve bunlara bağlı güvenli puanınız hesaplar, iş yükü güvenliğinizi en çok etkileyecek, önem derecesi ve güvenlik en iyi bir öneri puanı türetilir.

**Puanı güvenli** olduğundan, iyi durumdaki kaynaklar ve toplam kaynaklarınız arasındaki oran temel hesaplama. İyi durumdaki kaynaklar sayısını kaynakların toplam sayısı için eşit ise, en fazla 50 güvenli puanı alın. Güvenli puanınız daha yakın en yüksek puana almayı denemek için iyi durumda olmayan kaynaklar için önerileri takip ederek düzeltin.

Güvenlik Merkezi ayrıca genel güvenli puanıyla sağlar. 

**Genel puan güvenli** tüm önerileri birikmesi olduğu. Genel güvenli puanınız, abonelikleri veya yaptığınız seçime bağlı olarak Yönetim grupları arasında görüntüleyebilirsiniz. Puan, seçili abonelikte ve bu Aboneliklerdeki etkin öneri göre değişir.

 

En güvenli puanınız etkisi hangi önerilerdir denetlemek için Güvenlik Merkezi panosunda ilk 3 en etkili önerileri görüntüleyebilir veya öneriler listesi dikey penceresinde kullanarak önerileri sıralayabilirsiniz **güvenli puanı etkisi** sütun.


Genel güvenli puanınız görüntülemek için:

1. Azure panosunda tıklayın **Güvenlik Merkezi** ve ardından **önerileri**.
2. En üstünde, ilkeleri, seçili abonelik başına puanı temsil eden güvenli puanı görebilirsiniz. 
2. Öneriler listeleyen aşağıdaki tabloda, her bir öneri olduğunu gösteren bir sütun görebilirsiniz **puanı etkisi güvenli**. Genel güvenli puanınız önerilerine uyarsanız ne kadar artıracak bu sayı temsil eder. Örneğin, aşağıdaki ekran içinde **kapsayıcı güvenlik yapılandırmalarını güvenlik açıklarını düzelt**, güvenli puanınız 35 noktalarıyla artacaktır.

   ![Güvenli puanı](./media/security-center-secure-score/security-center-secure-score1.png)

## <a name="individual-secure-score"></a>Tek tek güvenli puanı

Ayrıca, tek tek güvenli puanları görmek için tek bir öneri dikey penceresi içinde bulabilirsiniz.  

**Öneri güvenli puanı** olduğundan, iyi durumdaki kaynaklar ve toplam kaynaklarınız arasındaki oran temel hesaplama. İyi durumdaki kaynaklar sayısını kaynakların toplam sayısı için eşit ise, en fazla güvenli puanı öneri alın. Güvenli puanınız daha yakın en yüksek puana almayı denemek için düzeltme adımları izleyerek iyi durumda olmayan kaynaklar düzeltin.

**Öneri etkisi** bildiğiniz ne kadar güvenli puanınız, öneri adımları uygularsanız artıracak sağlar. Örneğin, güvenli bir puan 42 ise ve **öneri etkisi** + 3 arası, öneri de özetlenen adımları gerçekleştirirseniz güvenli puanınız 45 olacak artırır.

Öneri düzeltme adımlarını alınmaz, iş yükünüz Internet'e hangi tehditleri gösterir.

![tek bir öneri güvenli puanı](./media/security-center-secure-score/indiv-recommendation-secure-score.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede güvenlik duruşunu kullanarak iyileştirmek gösterildi **güvenli puanı** Azure Güvenlik Merkezi'nde. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.


