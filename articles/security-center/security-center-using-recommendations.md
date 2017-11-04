---
title: "Güvenliği artırmak için Azure Güvenlik Merkezi'ni kullanın önerileri | Microsoft Docs"
description: " Güvenlik ilkeleri ve öneriler Azure Güvenlik Merkezi'nde güvenlik saldırısı azaltmaya yardımcı olmak için nasıl kullanılacağını öğrenin. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: terrylan
ms.openlocfilehash: 0616f5e501324bfd821c1455ce234602f1fcf1bd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-security-center-recommendations-to-enhance-security"></a>Güvenliği artırmak için Azure Güvenlik Merkezi'ni kullanın önerileri
Bir güvenlik ilkesi yapılandırma ve Azure Güvenlik Merkezi tarafından sağlanan öneriler uygulayarak bir önemli güvenlik olayı olasılığını azaltır. Bu makalede, güvenlik ilkeleri ve öneriler Güvenlik Merkezi'nde güvenlik saldırısı azaltmaya yardımcı olmak için nasıl kullanılacağını gösterir.

> [!NOTE]
> Bu makalede rolleri ve Güvenlik Merkezi tarafından sunulan kavramlar derlemeler [planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md). Devam etmeden önce Planlama Kılavuzu gözden geçirmek iyi bir fikirdir.
>
>

## <a name="managing-security-recommendations"></a>Güvenlik önerilerini yönetme
Bir güvenlik ilkesi belirtilen abonelik veya kaynak grubu içindeki kaynaklar için önerilen denetimleri kümesini tanımlar. Güvenlik Merkezi'nde, şirketinizin güvenlik gereksinimlerine göre ilkeleri tanımlarsınız. Daha fazla bilgi için bkz: [Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md).

Kaynak grupları için güvenlik ilkeleri abonelik düzeyinden devralınır.

![Güvenlik İlkesi Devralma][1]

Belirli kaynak gruplarında özel ilkelere gereksinim duyarsanız, kaynak grubundaki devralmayı devre dışı bırakabilirsiniz. Devre dışı bırakmak için devralma benzersiz için güvenlik ilkesi dikey penceresinde ayarlayın ve Güvenlik Merkezi önerileri için gösterir denetimleri özelleştirebilirsiniz.

Örneğin, SQL veritabanında saydam veri şifreleme (TDE'nin) ilkesini gerektirmeyen iş yükleriniz varsa ilkeyi abonelik düzeyinde devre dışı bırakın ve yalnızca SQL TDE'nin gerekli olduğu kaynak gruplarında etkinleştirin.

> [!NOTE]
> Abonelik düzeyi ilkesi ile kaynak grubu düzeyi ilkesi arasında bir çakışma olması durumunda, kaynak grubu düzeyi ilkesi önceliklidir.
>
>

Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu analiz eder. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, güvenlik ilkesi ayarlayın denetimlerinde dayalı olarak öneriler oluşturur. Öneriler gerekli güvenlik denetimlerini yapılandırma işleminde size kılavuzluk.

Güvenlik Merkezi odağı sistem güncelleştirmeleri, işletim sistemi yapılandırması, geçerli ilke önerileri ağ alt ağlar ve sanal makineleri (VM'ler) SQL veritabanı denetimi, SQL veritabanı TDE, güvenlik grubu ve web uygulaması güvenlik duvarları. Güvenlik Merkezi önerilerini en güncel kapsamı için bkz: [Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md).

## <a name="scenario"></a>Senaryo
Bu senaryoda Güvenlik Merkezi Güvenlik Merkezi önerilerini izleme ve eylemde bir önemli güvenlik olayı olasılığını azaltmaya yardımcı olmak için nasıl kullanılacağını gösterir. Senaryo kurgusal şirket, Contoso ve Güvenlik Merkezi tarafından sunulan rollerini kullanır [planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md#security-roles-and-access-controls). Roller, kişiler ve güvenlikle ilgili farklı görevleri gerçekleştirmek için Güvenlik Merkezi'ni kullanabilir takımlar temsil eder. Rolü şunlardır:

![Senaryo rolleri][2]

Contoso son bazı kendi şirket içindeki kaynaklar için Azure geçirildi. Contoso uygulamak ve kendi güvenlik açığı bulut kaynaklarının güvenlik saldırıya azaltmak korumaları sürdürmek istiyor.

## <a name="recommended-solution"></a>Önerilen çözüm
Güvenlik Merkezi önlemek ve güvenlik açıkları algılamak için kullanmak üzere bir çözümdür. Contoso Azure aboneliğini Güvenlik Merkezi'ne erişebilir. [Ücretsiz katmanı](security-center-pricing.md) , Güvenlik Merkezi tüm Azure abonelikleri üzerinde otomatik olarak etkinleştirilir ve veri toplama, Abonelikteki tüm VM'ler üzerinde etkindir.

Contoso'nun BT güvenliği, David yapılandırır bir **Güvenlik İlkesi** Güvenlik Merkezi'ni kullanma. Güvenlik Merkezi Contoso Azure kaynaklarınızın güvenlik durumunu çözümler. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde oluşturur **önerileri** güvenlik ilkesi ayarlayın denetimleri göre.

Jeff, bir bulut iş yükü sahibi, uygulama ve korumaları Contoso'nun güvenlik ilkelerini uygun koruma sorumludur. Jeff korumaları uygulamak için öneriler Güvenlik Merkezi tarafından oluşturulan izleyebilirsiniz. Öneriler Jeff gerekli güvenlik denetimlerini yapılandırma işleminde size kılavuzluk.

Jeff uygulama ve koruma korumak ve güvenlik açıklarını ortadan kaldırmak için kendisinin gerekir:

- Güvenlik Merkezi tarafından sağlanan güvenlik önerileri izleyin
- Güvenlik önerileri değerlendirmek ve kendisine uygulama kapatmak veya, karar verin
- Güvenlik önerileri uygulayın

Şimdi kendisinin Güvenlik Merkezi önerilerini ona güvenlik açıkları ortadan kaldırmak için denetimlerini yapılandırma işleminde size kılavuzluk için nasıl kullandığını görmek için Jeff'ın adımları izleyin.

## <a name="how-to-implement-this-solution"></a>Bu çözümü uygulama
Jeff oturum açtığı [Azure portal](https://azure.microsoft.com/features/azure-portal/) ve Güvenlik Merkezi Konsolu açılır. Günlük izleme faaliyetlerini bir parçası olarak, kendisinin güvenlik önerileri aşağıdaki adımları gerçekleştirerek olup olmadığını denetler:

1. Jeff seçer **önerileri** açmak için kutucuğa **önerileri**.
   ![Öneriler kutucuk seçin][3]
2. Jeff öneriler listesi inceler. Güvenlik Merkezi önerileri en yüksek öncelikli işler düşük öncelikli işler için gelen öncelik sırasına listesi sağlamıştır görür. Yüksek öncelikli öneri listesindeki yönelik olarak verir. Belirliyor **Endpoint Protection Yükle** altında **önerileri**.
3. **Endpoint Protection Yükle** etkin kötü amaçlı yazılımdan koruma VM'lerin listesini görüntüleyen açılır. Jeff VM'lerin listesini gözden geçirir, tüm VM'ler seçer ve ardından seçer **3 Vm'lerinde yükleme**.
   ![Uç nokta korumasını yükleme][4]
4. **Endpoint Protection seçin** Jeff iki kötü amaçlı yazılımdan koruma çözümleri sağlama açar. Jeff seçer **Microsoft Antimalware** çözümü.
5. Kötü amaçlı yazılımdan koruma çözümü hakkında ek bilgi görüntülenir. Jeff seçer **oluşturma**.
   ![Microsoft kötü amaçlı yazılımdan koruma][5]
6. Jeff girer gerekli yapılandırma ayarları altında **yükleme** ve seçer **Tamam**.

[Microsoft Antimalware](../security/azure-security-antimalware.md) artık seçili sanal makinelerin etkindir.

Jeff Orta öncelikli önerileri ve yüksek öncelikli taşımak mantığınız kararları devam eder. Jeff başvuran [güvenlik önerilerini yönetme](security-center-recommendations.md) önerileri ve kendisine uygulanıyorsa, her biri yaptığı anlamak için makale.

Jeff öğrenir [Microsoft Güvenlik Yanıt Merkezi (MSRC)](../security/azure-security-response-center.md) select güvenlik Azure ağ ve altyapı izleme gerçekleştirir ve Üçüncü taraflardan tehdit Intelligence ve kötüye şikayetlerinden alır. Jeff Contoso'nun Azure aboneliği için güvenlik iletişim ayrıntılarını sağlıyorsa, Microsoft kişiler bu Contoso'nun müşteri verilerini MSRC belirlerse, Contoso erişilmeden yasadışı veya yetkisiz bir tarafın. Müşterinizle uygularken Jeff şimdi izleyin **güvenlik iletişim ayrıntılarını sağlamak** öneri (bir öneri önem derecesi Orta Yukarıdaki önerileri listesinde).

1. Jeff seçer **güvenlik iletişim ayrıntılarını sağlamak** altında **önerileri**, açan **güvenlik iletişim ayrıntılarını sağlamak**.
2. Jeff kişi bilgilerini sağlamak için Azure aboneliği seçer. İkinci bir **güvenlik iletişim ayrıntılarını sağlamak** dikey pencere açılır.
   ![Güvenlik kişi ayrıntıları][6]
3. Altında **güvenlik iletişim ayrıntılarını sağlamak**, Jeff girer:

  - (yok kendisinin girebilirsiniz e-posta adresi sayısı için bir sınır) virgülle ayrılmış güvenlik iletişim e-posta adresleri
  - bir güvenlik, telefon numarası ile iletişime geçin

4. Jeff seçeneğini de kapatır **bana Gönder e-postalar uyarılar hakkında** yüksek öneme sahip uyarılar hakkında e-postaları için.
5. Jeff seçer **Tamam** güvenlik bilgilerini Contoso'nun aboneliğine uygulanacak.

Son olarak, düşük öncelikli işler için öneri Jeff incelemeleri **düzeltmek OS güvenlik açıkları** ve bu öneriyi uygulanamaz belirler. Öneri kapatmak istediği. Jeff sağ tarafta görüntülenen üç noktaya seçer ve ardından seçer **atla**.
   ![Öneri yok sayın][7]

## <a name="conclusion"></a>Sonuç
Güvenlik Merkezi'nde öneriler izleme, saldırının gerçekleşmeden önce güvenlik açıkları ortadan kaldırmanıza yardımcı olabilir. Uygulama ve Güvenlik Merkezi'nde güvenlik ilkeleriyle korumaları koruma tarafından bir güvenlik olayı engelleyebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu senaryoda güvenlik ilkeleri ve öneriler Güvenlik Merkezi'nde güvenlik saldırısı azaltmaya yardımcı olmak için nasıl kullanılacağı gösterilmiştir. Bkz: [olay yanıtlama senaryo](security-center-incident-response.md) saldırının gerçekleşmeden önce planınızın bir olay yanıtlama nasıl öğrenin.

Güvenlik Merkezi hakkında daha fazla bilgi için bkz:

* [Güvenlik durumunu izleme](security-center-monitoring.md) — Azure kaynaklarınızı sağlığını izlemek öğrenin.
* [Yönetme ve güvenlik uyarılarını yanıt](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [İzleme ve güvenlik olaylarını işleme](security-center-events-dashboard.md) - nasıl izleyeceğinizi öğrenin ve işlem güvenlik olaylarının zaman içinde toplanır.
* [İş ortağı çözümlerini izleme](security-center-partner-solutions.md) — iş ortağı çözümlerinizin sistem durumunu öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) — en son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-using-recommendations/security-center-policy-inheritance.png
[2]: ./media/security-center-using-recommendations/scenario-roles.png
[3]: ./media/security-center-using-recommendations/select-recommendations-tile.png
[4]: ./media/security-center-using-recommendations/install-endpoint-protection.png
[5]:./media/security-center-using-recommendations/microsoft-antimalware.png
[6]: ./media/security-center-using-recommendations/provide-security-contact-details.png
[7]: ./media/security-center-using-recommendations/dismiss-recommendation.png
