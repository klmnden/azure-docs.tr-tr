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
ms.author: v-mohabe
ms.openlocfilehash: c23c9a2b9af1947c397b96431ae3c3bcd0e30aaa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65968294"
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
   - **Genel güvenlik puanı**, seçilen aboneliğin ilkelere göre belirlenen puanını gösterir
   - **Kategoriye göre puan güvenli** hangi kaynakların en dikkat gerektiren gösterir
   - **En popüler öneriler göre puan etkisi güvenli** en iyi çözümü kullanırsanız, güvenli puanınız artıracak önerilerden bir listesini sağlar.
 
   ![Güvenli puanı](./media/security-center-secure-score/secure-score-dashboard.png)

3. Aşağıdaki tabloda, her abonelik sayınıza ve genel güvenli puanı her biri için görebilirsiniz.

   > [!NOTE]
   > Aboneliklerinin güvenlik puanının toplamı, genel güvenlik puanına eşit değildir. Güvenlik puanı, iyi durumdaki kaynaklarınızla öneri başına kaynak sayısı arasındaki orana göre hesaplanır ve aboneliklerinizdeki güvenlik puanlarının toplamı değildir. 
   >
4. Tıklayın **görüntülemek önerileri** güvenli puanınız geliştirmek için düzeltebilir Bu abonelik için önerileri görmek için.
4. Öneriler listesi içinde her bir öneri olduğunu gösteren bir sütun görebilirsiniz **puanı etkisi güvenli**. Genel güvenli puanınız önerilerine uyarsanız ne kadar artıracak bu sayı temsil eder. Örneğin, aşağıdaki ekran içinde **kapsayıcı güvenlik yapılandırmalarını güvenlik açıklarını düzelt**, güvenli puanınız 35 noktalarıyla artacaktır.

   ![Güvenli puanı](./media/security-center-secure-score/security-center-secure-score1.png)



## <a name="individual-secure-score"></a>Tek tek güvenli puanı

Ayrıca, tek tek güvenli puanları görmek için tek bir öneri dikey penceresi içinde bulabilirsiniz.  

**Öneri güvenlik puanı**, iyi durumdaki kaynaklarınızla toplam kaynak sayısı arasındaki orana göre hesaplanır. İyi durumdaki kaynakların sayısı toplam kaynak sayısına eşitse en yüksek öneri güvenlik puanı olan 50 puanı alırsınız. Güvenlik puanınızı üst sınıra yaklaştırmaya çalışmak için aşağıdaki önerileri izleyerek iyi durumda olmayan kaynakları düzeltin.

**Öneri etkisi**, önerilen adımları uygulamanız durumunda güvenlik puanınızın ne kadar artacağını gösterir. Örneğin güvenlik puanınız 42 ve **Öneri etkisi** +3 ise öneride belirtilen adımların uygulanması halinde puanınız 45 olacaktır.

Öneri düzeltme adımlarını alınmaz, iş yükünüz Internet'e hangi tehditleri gösterir.

![tek bir öneri güvenli puanı](./media/security-center-secure-score/indiv-recommendation-secure-score.png)







## <a name="next-steps"></a>Sonraki adımlar
Bu makalede güvenlik duruşunu kullanarak iyileştirmek gösterildi **güvenli puanı** Azure Güvenlik Merkezi'nde. Güvenlik Merkezi hakkında daha fazla bilgi için bkz:

* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.


