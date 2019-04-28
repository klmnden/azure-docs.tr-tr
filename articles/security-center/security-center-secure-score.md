---
title: Azure Güvenlik Merkezi'nde puanı güvenli | Microsoft Docs
description: " Azure Güvenlik Merkezi'nde güvenli puanı kullanarak, güvenlik önerileri öncelik verin. "
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/15/2019
ms.author: monhaber
ms.openlocfilehash: 22791fc43ff17d56e1f51e7f7737a10109f47c59
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60906240"
---
# <a name="improve-your-secure-score-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenli puanınız geliştirin


Güvenlik açısından faydalı sunan çok sayıda hizmetleriyle ilk güvenli ve iş yükünüz sağlamlaştırmak için hangi adımlar bilmek zordur. Azure güvenli puanı, güvenlik önerileri inceler ve ilk kez gerçekleştirmek için hangi önerilerin bilmesi bunları sizin için önceliklendirir. Bu araştırma önceliğini belirleyebilmek en önemli güvenlik açıklarını bulmanıza yardımcı olur. Güvenli puanı, iş yükü güvenlik duruşunuzu değerlendirmenize yardımcı olur. bir araçtır.

## <a name="secure-score-calculation"></a>Puan hesaplanmasında güvenliğini sağlama

Güvenlik Merkezi, her önerinin nasıl önemli olduğunu belirlemek için güvenlik önerilerini gözden geçirmek ve gelişmiş algoritmalar uygulayarak bir güvenlik analisti, çalışmasını taklit eder.
Azure Güvenlik Merkezi sürekli etkin öneri incelemeleri ve bunlara bağlı güvenli puanınız hesaplar, bir öneri puanı, İş yükünüzün güvenliğine en çok etkileyecek en iyi güvenlik uygulamaları ve önem derecesi türetilir.

Güvenlik Merkezi ayrıca sağlar, bir **genel puan güvenli**. 

**Genel puan güvenli** tüm öneri puanları birikmesi olduğu. Genel güvenli puanınız, abonelikleri veya yaptığınız seçime bağlı olarak Yönetim grupları arasında görüntüleyebilirsiniz. Puan, seçili abonelikte ve bu Aboneliklerdeki etkin öneri göre değişir.

 
Hangi önerileri en güvenli puanınız etkisi denetlemek için Güvenlik Merkezi panosunda ilk üç en etkili önerileri görüntüleyebilir veya öneriler listesi dikey penceresinde kullanarak önerileri sıralayabilirsiniz **güvenli puanı etkisi** sütun.


Genel güvenli puanınız görüntülemek için:

1. Azure panosunda tıklayın **Güvenlik Merkezi** ve ardından **güvenli puanı**.
2. Üst kısmında, önemli puan güvenli görebilirsiniz:
   - **Genel puan güvenli** ilkeleri, seçili abonelik başına puanı temsil eder
   - **Kategoriye göre puan güvenli** hangi kaynakların en dikkat gerektiren gösterir
   - **En popüler öneriler göre puan etkisi güvenli** en iyi çözümü kullanırsanız, güvenli puanınız artıracak önerilerden bir listesini sağlar.
 
   ![Güvenli puanı](./media/security-center-secure-score/secure-score-dashboard.png)

3. Aşağıdaki tabloda, her abonelik sayınıza ve genel güvenli puanı her biri için görebilirsiniz.

   > [!NOTE]
   > Genel güvenli puanı her abonelik güvenli puanı toplamına eşit değil. Güvenli puanı sağlıklı kaynaklarınızı ve öneri, değil, abonelikler arasında güvenli puanlarının toplam başına toplam kaynaklarınız arasındaki oran dayalı bir hesaplamadır. 
   >
4. Tıklayın **görüntülemek önerileri** güvenli puanınız geliştirmek için düzeltebilir Bu abonelik için önerileri görmek için.
4. Öneriler listesi içinde her bir öneri olduğunu gösteren bir sütun görebilirsiniz **puanı etkisi güvenli**. Genel güvenli puanınız önerilerine uyarsanız ne kadar artıracak bu sayı temsil eder. Örneğin, aşağıdaki ekran içinde **kapsayıcı güvenlik yapılandırmalarını güvenlik açıklarını düzelt**, güvenli puanınız 35 noktalarıyla artacaktır.

   ![Güvenli puanı](./media/security-center-secure-score/security-center-secure-score1.png)



## <a name="individual-secure-score"></a>Tek tek güvenli puanı

Ayrıca, tek tek güvenli puanları görmek için tek bir öneri dikey penceresi içinde bulabilirsiniz.  

**Öneri güvenli puanı** olduğundan, iyi durumdaki kaynaklar ve toplam kaynaklarınız arasındaki oran temel hesaplama. İyi durumdaki kaynaklar sayısını kaynakların toplam sayısı için eşit ise, en fazla güvenli puanı 50 öneri alın. Güvenli puanınız daha yakın en yüksek puana almayı denemek için iyi durumda olmayan kaynaklar için önerileri takip ederek düzeltin.

**Öneri etkisi** bildiğiniz ne kadar güvenli puanınız artırır sağlar öneri adımları uygularsanız. Örneğin, güvenli puanınız 42 ise ve **öneri etkisi** + 3 arası, olan geliştirmek 45 olacak puanınız öneri de özetlenen adımları gerçekleştirmeden.

Öneri düzeltme adımlarını alınmaz, iş yükünüz Internet'e hangi tehditleri gösterir.

![tek bir öneri güvenli puanı](./media/security-center-secure-score/indiv-recommendation-secure-score.png)







## <a name="next-steps"></a>Sonraki adımlar
Bu makalede güvenlik duruşunu kullanarak iyileştirmek gösterildi **güvenli puanı** Azure Güvenlik Merkezi'nde. Güvenlik Merkezi hakkında daha fazla bilgi için bkz:

* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.


