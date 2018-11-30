---
title: Güvenliği artırmak için Azure Güvenlik Merkezi'ni kullanarak önerileri | Microsoft Docs
description: " Güvenlik ilkeleri ve öneriler, Azure Güvenlik Merkezi'nde güvenlik saldırısını önlemeye yardımcı olmak için kullanmayı öğrenin. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: terrylan
ms.openlocfilehash: 3640e4affe42986106791cba50f6cbfd97906806
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52308333"
---
# <a name="use-azure-security-center-recommendations-to-enhance-security"></a>Güvenliği artırmak için Azure Güvenlik Merkezi'ni kullanarak önerileri
Güvenlik İlkesi yapılandırma ve sonra Azure Güvenlik Merkezi tarafından sağlanan öneriler uygulayarak bir önemli güvenlik olayı olasılığını azaltabilirsiniz. Bu makalede güvenlik ilkeleri ve öneriler Güvenlik Merkezi'nde güvenlik saldırısını önlemeye yardımcı olmak için nasıl kullanılacağını gösterir.

> [!NOTE]
> Bu makalede rolleri ve Güvenlik Merkezi tarafından tanıtılan kavramları geliştirir [planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md). Devam etmeden önce Planlama Kılavuzu gözden geçirmek iyi bir fikirdir.
>
>

## <a name="managing-security-recommendations"></a>Güvenlik önerilerini yönetme
Güvenlik İlkesi belirtilen abonelik veya kaynak grubundaki kaynaklar için önerilen denetim kümesini tanımlar. Güvenlik Merkezi'nde şirketinizin güvenlik gereksinimlerine göre ilkeleri tanımlarsınız. Daha fazla bilgi için bkz. [Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-azure-policy.md).

Kaynak grupları için güvenlik ilkeleri abonelik düzeyinden devralındığı.

![Güvenlik İlkesi Devralma][1]

Belirli kaynak gruplarında özel ilkeler gerekiyorsa, kaynak grubundaki devralmayı devre dışı bırakabilirsiniz. Devre dışı bırakmak için devralma için benzersiz güvenlik ilkesi dikey penceresinde ayarlayın ve Güvenlik Merkezi önerileri gösterir denetimlerini özelleştirebilirsiniz.

Örneğin, SQL veritabanı saydam veri şifrelemesi (TDE) ilkesi gerektirmeyen iş yükleriniz varsa ilkeyi abonelik düzeyinde devre dışı bırakın ve yalnızca SQL TDE'nin gerekli olduğu kaynak gruplarında etkinleştirin.

> [!NOTE]
> Abonelik düzeyi ilkesi ile kaynak grubu düzeyi ilkesi arasında bir çakışma olması durumunda, kaynak grubu düzeyi ilkesi önceliklidir.
>
>

Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu analiz eder. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde öneriler güvenlik ilkesi ayarlayın denetimleri temel oluşturur. Öneriler güvenlik gerekli denetimlerin yapılandırılması işlemi boyunca size rehberlik.

Sistem güncelleştirmeleri, işletim sistemi yapılandırması, Güvenlik Merkezi Odak geçerli İlkesi önerileri ağ güvenlik grupları alt ağlar ve sanal makineler (VM'ler) SQL veritabanı denetimi, SQL veritabanı TDE ve web uygulaması güvenlik duvarları. Güvenlik Merkezi önerilerini en güncel kapsamı için bkz: [Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md).

## <a name="scenario"></a>Senaryo
Bu senaryo izleme Güvenlik Merkezi önerileri ve eylemi gerçekleştirmeden önemli güvenlik olayı olasılığını azaltmaya yardımcı olmak için Güvenlik Merkezi'ni kullanmayı gösterir. Senaryo adlı kurgusal şirketin, Contoso ve Güvenlik Merkezi tarafından sunulan rollerini kullanır [planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md#security-roles-and-access-controls). Rolleri, kişiler ve Güvenlik Merkezi, güvenlikle ilgili farklı görevleri gerçekleştirmek için kullanabileceği takımlar temsil eder. Rolü şunlardır:

![Senaryo rolleri][2]

Contoso kısa süre önce şirket içi kaynaklarından bazılarını Azure'a taşımıştır. Contoso, uygulamak ve bulut kaynaklarından bazılarını güvenlik saldırısı kendi güvenlik açığını azaltmak korumaları sürdürmek istiyor.

## <a name="recommended-solution"></a>Önerilen çözüm
Güvenlik Merkezi önleme ve güvenlik açıklarını algılamak için kullanmak için bir çözümdür. Contoso Azure aboneliğini Güvenlik Merkezi'ne erişebilir. [Ücretsiz katmanı](security-center-pricing.md) , Güvenlik Merkezi, tüm Azure abonelikleri üzerinde otomatik olarak etkinleştirilir ve aboneliklerinde tüm vm'lerde veri toplama etkinleştirilir.

David, Contoso'nun BT güvenlik yapılandırır bir **Güvenlik İlkesi** Güvenlik Merkezi'ni kullanma. Güvenlik Merkezi, Contoso'nun Azure kaynaklarınızın güvenlik durumunu analiz eder. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, oluşturduğu **önerileri** güvenlik ilkesi ayarlayın denetimleri göre.

Jeff, bulut iş yükü sahibi uygulama ve koruma Contoso'nun güvenlik ilkelerine uygun olarak koruma sorumludur. Jeff, koruma uygulamak için Güvenlik Merkezi tarafından oluşturulan öneriler izleyebilirsiniz. Öneriler Jeff gerekli güvenlik denetimlerini yapılandırma işleminde size rehberlik.

Jeff, uygulamak ve korumalarını sağlamak ve güvenlik açığını ortadan kaldırmak, kendisi için gerekir:

- Güvenlik Merkezi tarafından sağlanan güvenlik önerilerini izleyin
- Güvenlik önerileri değerlendirin ve o uygulama kapatmak veya seçemeyeceğini
- Güvenlik önerilerini uygulama

Şimdi Güvenlik Merkezi önerilerini ona güvenlik açığını ortadan kaldırmak için denetimlerin yapılandırılması işlemi boyunca rehberlik edecek kullandığı nasıl görmek için Jeff'in adımları izleyin.

## <a name="how-to-implement-this-solution"></a>Bu çözümü uygulama
Jeff oturum açtığı [Azure portalında](https://azure.microsoft.com/features/azure-portal/) ve Güvenlik Merkezi Konsolu açılır. Kendi günlük izleme etkinliklerinin bir parçası olarak, kendisine güvenlik önerileri aşağıdaki adımları gerçekleştirerek olup olmadığını denetler:

1. Jeff seçer **önerileri** açmak için kutucuğa **önerileri**.
   ![Öneriler kutucuğu seçin][3]
2. Jeff, öneriler listesini inceler. Güvenlik Merkezi, en düşük önceliğin en yüksek önceliğe öncelik sırası öneriler listesi ayarının görür. Yüksek öncelikli öneri listesindeki yönelik olarak belirler. Belirliyor **Endpoint Protection Yükle** altında **önerileri**.
3. **Endpoint Protection Yükle** etkin kötü amaçlı yazılımdan koruma VM'lerin listesini görüntüleyen açılır. Jeff VM'lerin listesini gözden geçirmeleri, tüm Vm'leri seçer ve ardından seçer **3 VM üzerinde yükleme**.
   ![Uç nokta korumasını yükleme][4]
4. **Endpoint Protection'ı seçin** Jeff iki kötü amaçlı yazılımdan koruma çözümleri sağlayan açılır. Jeff seçer **Microsoft Antimalware** çözüm.
5. Kötü amaçlı yazılımdan koruma çözümü hakkında ek bilgiler görüntülenir. Jeff seçer **Oluştur**.
   ![Microsoft kötü amaçlı yazılımdan koruma][5]
6. Jeff, gerekli yapılandırma ayarlarında girer **yükleme** seçer **Tamam**.

[Microsoft Antimalware](../security/azure-security-antimalware.md) artık seçili Vm'lerde etkindir.

Jeff yüksek öncelikli ve orta öncelikli öneriler aracılığıyla taşımak mantığınız kararların devam eder. Jeff başvuran [güvenlik önerilerini yönetme](security-center-recommendations.md) öneriler ve he uygulanıyorsa, her biri yaptığı anlamak için makaleyi.

Jeff öğrenir [Microsoft Güvenlik Yanıt Merkezi (MSRC)](../security/azure-security-response-center.md) select güvenlik Azure ağ ve altyapı izleme gerçekleştirir ve tehdit zekasını ve kötüye şikayetlerinin Üçüncü taraflardan alır. Jeff, Contoso'nun Azure aboneliği için güvenlik kişi ayrıntılarını sağlıyorsa, Microsoft kişiler, Contoso'nun müşteri verilerini MSRC belirlerse, Contoso erişilmeden yasadışı veya yetkisiz bir tarafın tarafından. Jeff kendisinin geçerlidir şimdi izleyin **güvenlik kişi ayrıntılarını sağlama** öneri (önem derecesi orta bölümünde önerinin Yukarıdaki öneriler listesi).

1. Jeff seçer **güvenlik kişi ayrıntılarını sağlama** altında **önerileri**, açan **güvenlik kişi ayrıntılarını sağlama**.
2. Jeff, şirket iletişim bilgilerini sağlamak için Azure aboneliği seçer. İkinci **güvenlik kişi ayrıntılarını sağlama** dikey penceresi açılır.
   ![Güvenlik ilgili kişisinin bilgilerini][6]
3. Altında **güvenlik kişi ayrıntılarını sağlama**, Jeff girer:

  - Güvenlik ilgili kişi e-posta adresleri (değil o girebilirsiniz e-posta adresleri sayısına bir sınır) virgülle ayrılmış
  - bir güvenlik ilgili kişi telefon numarası

4. Jeff, ayrıca seçeneğini kapatır **Gönder e-posta uyarıları hakkında** yüksek öneme sahip uyarılar hakkında e-posta almak için.
5. Jeff seçer **Tamam** Contoso'nun abonelik için güvenlik bilgilerini uygulamak için.

Son olarak, Jeff düşük öncelikli öneri incelemeleri **düzeltme işletim sistemi güvenlik açıklarını** ve bu öneriyi uygulanamaz belirler. Öneri sonlandırmak istiyor. Jeff, sağ tarafta görüntülenen üç noktaya seçer ve ardından seçer **atla**.
   ![Öneri Kapat][7]

## <a name="conclusion"></a>Sonuç
Güvenlik Merkezi'nde öneriler izleme, bir saldırı gerçekleşmeden önce güvenlik açıklarını ortadan kaldırmanıza yardımcı olabilir. Uygulama ve koruma ile Güvenlik Merkezi'nde güvenlik ilkelerini koruma, bir güvenlik olayı engelleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu senaryoda güvenlik ilkeleri ve öneriler Güvenlik Merkezi'nde güvenlik saldırısını önlemeye yardımcı olmak için nasıl kullanılacağını gösterdi. Bkz: [olay yanıtı senaryosu](security-center-incident-response.md) olay yanıtı bir saldırı gerçekleşmeden önce planınızın olması hakkında bilgi edinmek için.

Güvenlik Merkezi hakkında daha fazla bilgi için bkz:

* [Güvenlik durumunu izleme](security-center-monitoring.md) — Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Yönetme ve güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [İzleme ve güvenlik olaylarını işleme](security-center-events-dashboard.md) - izleme hakkında bilgi edinin ve işlem güvenlik olaylarının zaman içinde toplanır.
* [İş ortağı çözümlerini izleme](security-center-partner-solutions.md) — iş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) — en son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-using-recommendations/security-center-policy-inheritance.png
[2]: ./media/security-center-using-recommendations/scenario-roles.png
[3]: ./media/security-center-using-recommendations/select-recommendations-tile.png
[4]: ./media/security-center-using-recommendations/install-endpoint-protection.png
[5]:./media/security-center-using-recommendations/microsoft-antimalware.png
[6]: ./media/security-center-using-recommendations/provide-security-contact-details.png
[7]: ./media/security-center-using-recommendations/dismiss-recommendation.png
