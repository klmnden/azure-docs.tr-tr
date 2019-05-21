---
title: Azure Güvenlik Merkezi'ni kullanarak, Mevzuat uyumluluğu geliştirin | Microsoft Docs
description: "Öğretici: Azure Güvenlik Merkezi'ni kullanarak, Mevzuat uyumluluğu geliştirin öğrenin."
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 5f50c4dc-ea42-418d-9ea8-158ffeb93706
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/30/2019
ms.author: v-mohabe
ms.openlocfilehash: e1544b0c9bf280c8d097d2fa25f7fc652450b87e
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968565"
---
# <a name="tutorial-improve-your-regulatory-compliance"></a>Öğretici: Mevzuat uyumluluğunuzu artırma
---

Azure Güvenlik Merkezi yasal uyumluluk gereksinimlerini işlemi kolaylaştırmaya yasal uyumluluk Panoyu kullanarak yardımcı olur. Panoda, Güvenlik Merkezi, uyumluluk duruşunuzu Azure ortamınızın devamlı değerlendirmelerle hakkında Öngörüler sağlar. Güvenlik Merkezi tarafından gerçekleştirilen değerlendirmeleri risk faktörleri karma bulut ortamınızda en iyi güvenlik uygulamaları uygun olarak analiz edin. Bu değerlendirmeler uyumluluk denetimleri için desteklenen standartları kümesinden eşlenir. Yasal uyumluluk panosunda, ortamınızda belirli bir standart veya düzenleme bağlamı tüm değerlendirmeler durumunu açık bir görünümünü sahip. Öneriler hareket ve ortamınızda risk faktörleri azaltmak gibi geliştirmek, uyumluluk duruşunu görebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

-   Yasal uyumluluk Panoyu kullanarak, yönetmeliklere uygunluk değerlendirme

-   Öneriler eylemi gerçekleştirerek, uyumluluk duruşunu

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide ele alınan özelliklerin üzerinden adımlamak için Güvenlik Merkezi'nin standart fiyatlandırma katmanında olmalıdır. Ücretsiz olarak Güvenlik Merkezi standart deneyebilirsiniz.
Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/). [Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme](https://docs.microsoft.com/azure/security-center/security-center-get-started) başlıklı hızlı başlangıçta Standart katmanına nasıl yükseltebileceğiniz adım adım açıklanmıştır.

##  <a name="assess-your-regulatory-compliance"></a>Yasal, uyumluluk değerlendirme

Güvenlik Merkezi, güvenlik sorunlarını ve güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın yapılandırmasını sürekli olarak değerlendirir. Bu değerlendirme, güvenlik sağlığı geliştirmeye odaklanma konusunda öneriler olarak gösterilir. Yasal uyumluluk Panoda desteklenen gereksinimleri için geçerli güvenlik değerlendirmeleri burada eşlenir kendi gereksinimler ile uyumluluk standartlarını kümesini görüntüleyebilirsiniz. Bu sayede, uyumluluk duruşunu standart göre görüntülemek Bu değerlendirme durumu temelinde.

Yasal uyumluluk Pano görünümü, dikkatinizi standart veya sizin için önemli olan düzenleme uyduğunuzu boşlukları üzerinde odaklanmanıza yardımcı olabilir. Bu odaklanmış bir görünümünü sürekli olarak zaman içinde dinamik Bulut ve karma ortamlar içinde uyumluluk puanınız izlemenizi sağlar.

>[!NOTE]
> Şu anda desteklenen Mevzuat standartlarına şunlardır: Azure CIS, PCI DSS 3.2, ISO 27001 ve SOC TSP. Onu geliştiren gibi ek standartları Panoda yansıtılır.
1.  Ana menü, Güvenlik Merkezi **İLKE ve Uyumluluk** seçin **yasal Uyumluluk**. <br>
Ekranın üst kısmında, uyumluluk durumunuzu genel bir bakış ile desteklenen uyumluluk düzenlemelerini kümesini içeren bir Pano görürsünüz. Genel uyumluluk puanınız ve her bir standart ile ilişkili konularla ilgili değerlendirmelerini başarısız karşılaştırması geçirme sayısını görebilirsiniz.

    ![bilgisayar açıklama yüksek güvenilirlik](./media/security-center-compliance-dashboard/compliance-dashboard.png)


2.  Size uygun bir uyumluluk standardını bir sekmesini seçin. Bu standart için tüm denetim listesini görürsünüz. Uygun denetimler için geçirme ve değerlendirmeler, denetimle ilişkili başarısız ayrıntılarını görüntüleyebilirsiniz. Bazı denetimler gri görünür. Bu denetimler ilişkili herhangi bir Güvenlik Merkezi değerlendirme yok. Bunlar gereksinimlerini çözümlemek ve bunları kendi ortamınızdaki değerlendirmek gerekir. Bunlardan bazıları teknik değil ve işlem ile ilgili olabilir.

    ![Uyumluluk sekmesi](./media/security-center-compliance-dashboard/compliance-pci.png)

3. Seçin **tüm** ilgili tüm Güvenlik Merkezi önerilerini ve bunların ilişkili standartlarını görmek için sekmesinde. Bu görünümde, belirli bir öneriden etkilenen farklı standartları tanımlamak için yararlı olabilir. <br> Bu görünüm olabilecek öneriler çözmeniz gereken önceliğini belirlemek için de kullanabilirsiniz. Örneğin gördüğünüz, öneri **, aboneliğinizin sahibine izinleri olan hesaplar için MFA etkinleştirin** birden çok kaynak üzerinde başarısız oluyor ve ardından bu öneriyi çözümlemek birden çok standartları ile ilişkili Genel uyumluluk puanınız yüksek etkili olacaktır.

    ![Uyumluluk puanı etkisi](./media/security-center-compliance-dashboard/compliance-all-tabs.png)

1. Oluşturmak ve belirli bir standart için geçerli uyumluluk durumunu özetleyen bir PDF raporu indirmek için tıklayın **raporu indir**.

    Rapor, Güvenlik Merkezi Değerlendirme verileri temel alan seçili standardı için üst düzey, Uyumluluk durumunun özetini sağlar ve bu belirli standart denetimler göre düzenlenir. Rapor ilgili hissedarlarla paylaşılabilir ve iç ve dış denetçiler için kanıt sağlamak için hizmet verebilir.

    ![indir](./media/security-center-compliance-dashboard/download-report.png)

## <a name="improve-your-compliance-posture"></a>Uyumluluk duruşunu

Verilen bilgilerle Mevzuat uyumluluğu panosunda, doğrudan panonun içinden önerileri çözerek, uyumluluk duruşunu artırabilir.

1.  Bu öneri ayrıntılarını görüntülemek için Panoda görünen başarısız değerlendirmeleri birini tıklayın. Her bir öneri kümesi bu sorunu gidermek için izlenmesi gereken düzeltme adımlarını içerir.

2.  Daha fazla ayrıntı görüntülemek ve bu kaynak için öneriyi çözümlemek için belirli bir kaynak seçebilirsiniz. <br>Örneğin, **Azure CIS standart** sekmesinde öneriye tıklayabilirsiniz **depolama hesabına güvenli aktarım gerektir**.

    ![Uyumluluk önerisi](./media/security-center-compliance-dashboard/compliance-recommendation.png)

3. Öneri bilgilerin tıklama yoluyla ve iyi durumda olmayan bir kaynak seçin, etkinleştirme doğrudan deneyimi sunduğuna **güvenli aktarım depolama** Azure portalındaki.<br>Önerileri uygulama hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md).

    ![Uyumluluk önerisi](./media/security-center-compliance-dashboard/compliance-remediate-recommendation.png)

4.  Önerileri çözümleme için eyleme geçtikten sonra uyumluluk puanı artırdığı uyumluluk Panosu rapor etkileri görürsünüz.

    > [!NOTE]
    > Yalnızca değerlendirme çalıştırdıktan sonra uyumluluk verileriniz üzerinde etkisi görürsünüz. Bu nedenle değerlendirmeleri yaklaşık olarak her 12 saatte çalıştırılır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Güvenlik Merkezi'nin yasal uyumluluk panoya kullanma hakkında bilgi edindiniz:

-   Görüntüleyebilir ve sizin için önemli olan düzenlemeleri ve standartları göre uyumluluk duruşunu izleyin.

-   Uyumluluk durumunuzu ilgili önerileri çözümleme ve puan geliştirmek uyumluluğunu izlemeye geliştirin.

Yasal uyumluluk Panosu, önemli ölçüde ve uyumluluk sürecini basitleştirmek ve Azure ve karma ortamınızın uyumluluk kanıtı toplamak için gereken zamanı önemli ölçüde Kes.

Güvenlik Merkezi hakkında daha fazla bilgi için bkz:

-   [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.

-   [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md)--Azure kaynaklarınızı korumaya yardımcı olmak için Azure Güvenlik Merkezi'nde öneriler kullanmayı öğrenin.

-   [Azure Güvenlik Merkezi'nde güvenli puanınız geliştirmek](security-center-secure-score.md)--güvenlik açıklarını ve en güvenliğinizi artırmak için güvenlik önerilerini öncelik öğrenin.

-   [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
