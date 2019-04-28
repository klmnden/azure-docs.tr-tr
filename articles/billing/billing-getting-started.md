---
title: Beklenmeyen maliyetleri engelleme, azure'da faturalama yönetin | Microsoft Docs
description: Azure faturanızda beklenmeyen ücretlerden kaçınmak öğrenin. Maliyet-izleme ve yönetim özelliklerine, bir Microsoft Azure aboneliği için kullanın.
services: ''
documentationcenter: ''
author: bandersmsft
manager: alherz
editor: ''
tags: billing
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2018
ms.author: banders
ms.openlocfilehash: d215e2ba5d650b532b9d7554ccca9ad5537cf4de
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60919445"
---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Azure'da faturalandırma ve maliyet yönetimi ile beklenmeyen ücretlerden

Azure için kaydolduğunuzda, harcamalarınızı daha iyi bir fikir almak için yapabileceğiniz birkaç şey vardır. [Fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) bir Azure kaynak oluşturmadan önce maliyet tahmini sağlar. [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) aboneliğiniz için geçerli Maliyet dağılımı ve tahmini sağlar. Grup ve maliyetleri farklı projeler ya da ekipler için anlamak istiyorsanız, bakmak [kaynak etiketleme](../azure-resource-manager/resource-group-using-tags.md). Kuruluşunuzda kullanmak için tercih ettiğiniz bir raporlama sistemi varsa kullanıma [faturalama API'leri](billing-usage-rate-card-overview.md).

- Aboneliğinizi bir Kurumsal Anlaşma (EA) ise, Azure portalında maliyetlerinizi görmek için genel önizlemede kullanılabilir. Aboneliğiniz, bulut çözümü sağlayıcısı (CSP) veya Azure Sponsorluğu ise, ardından aşağıdaki özelliklerden bazıları için geçerli olmayabilir. Bkz: [EA, CSP ve sponsorluk için ek kaynaklar](#other-offers) daha fazla bilgi için.

- Ücretsiz bir deneme aboneliğiniz varsa [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), açık (AIO) veya BizSpark Azure tüm kredilerinizi kullanıldığında, aboneliğinizi otomatik olarak devre dışı bırakıldı. Hakkında bilgi edinin [harcama limitleri](#spending-limit) beklenmedik bir şekilde devre dışı aboneliğiniz olmamasına özen gösterin.

- İçin açtıysanız [ücretsiz Azure hesabı](https://azure.microsoft.com/free/), [bazı en popüler Azure Hizmetleri 12 ay boyunca ücretsiz kullanabileceğiniz](billing-create-free-services-included-free-account.md). Aşağıda listelenen öneriler birlikte bkz [ücretsiz hesap ücret önlemek](billing-avoid-charges-free-account.md).

## <a name="get-estimated-costs-before-adding-azure-services"></a>Azure Hizmetleri eklemeden önce tahmini maliyetleri alın

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Çevrimiçi fiyatlandırma hesaplayıcısını kullanarak maliyet tahmini

Kullanıma [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) bir tahmini aylık maliyet, ilgilendiğiniz hizmeti alınamıyor. Tahmini maliyet almak için tüm birinci taraf Azure kaynak ekleyebilirsiniz.

![Fiyatlandırma hesaplayıcı menüsünün ekran görüntüsü](./media/billing-getting-started/pricing-calc.png)

Örneğin, A1 Windows sanal makinesi (VM), tam zamanlı çalışan bırakırsanız 66.96 ABD Doları/ay işlem saatleri maliyet tahmini:

![Fiyatlandırma hesaplayıcı bir A1 Windows VM 66.96 ABD Doları aylık maliyet tahmini gösteren ekran görüntüsü](./media/billing-getting-started/pricing-calcVM.png)

Fiyatlandırma hakkında daha fazla bilgi için bkz. Bu [SSS](https://azure.microsoft.com/pricing/faq/). Veya bir Azure satış temsilcisine konuşmak isterseniz, 1-800-867-1389 başvurun.

### <a name="review-the-estimated-cost-in-the-azure-portal"></a>Azure portalında tahmini maliyeti gözden geçirin

Genellikle Azure Portalı'nda hizmet eklediğinizde, benzer bir tahmini maliyeti aylık gösteren bir görünüm yok. Örneğin, Windows sanal makinenizin boyutunu seçtiğinizde, işlem saatleri için tahmini aylık maliyet bakın:

![Örnek: bir A1 Windows VM 66.96 ABD Doları aylık maliyet tahmini](./media/billing-getting-started/vm-size-cost.PNG)

### <a name="spending-limit"></a> Harcama limiti varsa kontrol edin

Krediler kullanan bir aboneliğiniz varsa, ardından harcama limiti, varsayılan olarak etkinleştirilir. Bu şekilde tüm kredilerinizi, harcama, kredi kartınıza ücretlendirilirsiniz değil. Bkz: [Azure tekliflerini ve harcama limiti kullanılabilirliğini tam listesi](https://azure.microsoft.com/support/legal/offer-details/).

Ancak, harcama sınırına ulaşırsanız, hizmetleriniz devre dışı. Sanal makinelerinizin serbest anlamına gelir. Hizmet kapalı kalma süresini önlemek için harcama limitinizi etkinleştirmeniz gerekir. Kapasite aşımları üzerine, kayıtlı kredi kartından ücret. 

Üzerinde harcama limiti var olmadığını görmek için Git [hesap Merkezi'nde abonelikleri görmek](https://account.windowsazure.com/Subscriptions). Harcama limitiniz açıksa, bir başlık görünür:

![Harcama sınırı üzerinde hesap Merkezi'nde olan hakkında bir uyarı gösteren ekran görüntüsü](./media/billing-getting-started/spending-limit-banner.PNG)

Başlığa tıklayın ve harcama limitini kaldırmak için yönergeleri izleyin. Kaydolurken kredi kartı bilgilerini girmediniz, harcama limitini kaldırmak için girmeniz gerekir. Daha fazla bilgi için [Azure harcama limiti – nasıl çalışır ve etkinleştirme veya kaldırmayı nasıl](https://azure.microsoft.com/pricing/spending-limits/).

Kullanabileceğiniz [Cloudyn](https://www.cloudyn.com/) hizmetinin otomatik olarak anomalileri harcama ve fazla harcama riskleri paydaşlara bildirmek uyarılar oluşturun. Uyarıları destekleyen bütçe ve maliyet eşiklerine raporları kullanarak uyarılar oluşturabilirsiniz. Cloudyn kullanma hakkında daha fazla bilgi için bkz. [Öğreticisi: Kullanımı ve maliyetleri gözden geçirme](../cost-management/tutorial-review-usage.md).

Bu örnekte **zaman içinde gerçek maliyet** harcamalarınızı bir Azure sanal makinesinde toplam bütçenize yaklaştığında bildirim göndermek için rapor. Bu senaryoda, 20.000 $ toplam bütçeye varsa ve maliyetleri 10.000 ABD Doları ulaştığında maliyetler bütçenize $ 9. 000 ve ek bir uyarının yarısını yaklaştığı olduğunda bir bildirim almak istiyorsanız.

1. Cloudyn portal üst kısmındaki menüden **maliyetleri** > **maliyet analizi** > **zaman içinde gerçek maliyet**. 
2. **Gruplar** seçeneğini **Hizmet** olarak, **Hizmet filtreleme** seçeneğini **Azure/VM** olarak ayarlayın. 
3. Üst raporun sağ seçin **eylemleri** seçip **rapor zamanla**.
4. Zamanlanan aralıklarla kendinize raporu e-posta göndermek için seçin **zamanlama** sekmesinde **kaydedin veya bu zamanlamayı** rapor iletişim. **E-posta ile gönder**’i seçtiğinizden emin olun. Tüm etiketler, gruplandırma ve filtreleme, kullanım, e-postayla rapora dahil edilir. 
5. Seçin **eşiği** sekmesini seçip **gerçek maliyet vs. Eşik** öğesini seçin. 
   1. İçinde **kırmızı uyarıya** 10000 eşiği kutusuna girin. 
   2. İçinde **sarı uyarı** 9000 eşiği kutusuna girin. 
   3. İçinde **ardışık olarak verilecek uyarı sayısını** kutusunda, almak için ardışık olarak verilecek uyarı sayısını girin. Belirttiğiniz toplam uyarı sayısını aldığınızda, başka bir uyarı gönderilmez. 
6. **Kaydet**’i seçin.

    ![Kırmızı ve sarı uyarı eşikleri harcamayı göre gösteren örnek](./media/billing-getting-started/schedule-alert01.png)

Ayrıca seçebilirsiniz **maliyet yüzdesi ve. Bütçe** eşiği ölçümünü kullanarak uyarı oluşturmayı. Bu, bütçenizi para birimi değerleri yerine yüzde olarak eşikleri belirlemenizi sağlar.

## <a name="ways-to-monitor-your-costs-when-using-azure-services"></a>Azure hizmetlerini kullanırken maliyetlerinizi izlenecek yolları

### <a name="tags"></a> Faturalama verilerinize gruplandırmak için kaynaklarınıza etiketler ekleme

Desteklenen hizmetler için fatura verileri gruplandırmak için etiketleri kullanabilirsiniz. Farklı ekipler için birkaç VM çalıştırırsanız, örneğin, ardından etiketleri maliyetleri maliyet merkezi (ik, pazarlama, Finans) veya ortama (üretim, üretim öncesi test) göre kategorilere ayırmak için kullanabilirsiniz. 

![Portalda etiketlerini ayarlama gösteren ekran görüntüsü](./media/billing-getting-started/tags.PNG)

Etiketleri farklı maliyet görünümleri raporlama gösterilir. Örneğin, görünür, [maliyet analizi görüntüleme](#costs) hemen sağ ve ilk fatura döneminiz sonra kullanım .csv dosyasındaki ayrıntı.

Daha fazla bilgi için [etiketleri kullanarak Azure kaynaklarınızı düzenleme](../azure-resource-manager/resource-group-using-tags.md).

### <a name="costs"></a> Düzenli olarak Maliyet dağılımı için portalı denetleyin ve yazma hızı

Hizmetlerinizi çalışır aldıktan sonra ne kadar bunlar, maliyet düzenli aralıklarla denetleyin. Geçerli spend görmek ve Azure Portalı'nda yazma oranı.

1. Ziyaret [Azure portalında abonelikleri](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ve bir abonelik seçin.

2. Aboneliğiniz için destekleniyorsa, maliyet dökümünü görmek ve yazma oranı.

    ![Yazma hızı ve Azure Portalı'nda dökümünü ekran görüntüsü](./media/billing-getting-started/burn-rate.PNG)

3. Tıklayın **maliyet analizi** kaynağı ile Maliyet dağılımı görmek için sol listesinde. Bir hizmet için doldurmak veri ekledikten sonra 24 saat bekleyin.

    ![Azure portalında maliyet analizi görünümünün ekran görüntüsü](./media/billing-getting-started/cost-analysis.PNG)

4. Gibi farklı özelliklere göre filtreleyebilirsiniz [etiketleri](#tags), kaynak türü, kaynak grubu ve zaman aralığı. Tıklayın **Uygula** filtreleri doğrulamak ve **indirin** görünümü bir virgül ile ayrılmış değerler (.csv) dosyasına dışarı aktarmak istiyorsanız.

5. Ayrıca, geçmiş ve kaynak her gün maliyetinin ne günlük görmek için bir kaynağa tıklayabilirsiniz.

    ![Azure portalında spend geçmişi görünümünün ekran görüntüsü](./media/billing-getting-started/costhistory.PNG)

Hizmetleri seçildiğinde gördüğünüz olan tahminleri gördüğünüz maliyetleri kontrol öneririz. Maliyet tahminleri, çift onay kaynaklarınız için seçtiğiniz fiyatlandırma planı çok farklı varsa.

### <a name="consider-enabling-cost-cutting-features-like-auto-shutdown-for-vms"></a>Otomatik kapatma gibi maliyet kesme özellikler sanal makineler için etkinleştirmeyi düşünün

Senaryonuza bağlı olarak, Azure portalında sanal makineleriniz için otomatik kapatma yapılandırabilirsiniz. Daha fazla bilgi için [Azure Resource Manager kullanarak sanal makineler için otomatik kapatma](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Otomatik kapatma seçeneği Portalı'ndaki ekran görüntüsü](./media/billing-getting-started/auto-shutdown.PNG)

Otomatik kapatma zaman, güç seçenekleri ile VM içinde kapatma olarak aynı değildir. Otomatik kapatma durdurur ve ek kullanım ücretleri durdurmak için sanal makinelerinizin kaldırır. Fiyatlandırma SSS için daha fazla bilgi için bkz. [Linux Vm'leri](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows Vm'leri](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) VM durumlarıyla ilgili.

Geliştirme ve test ortamlarınızı daha fazla maliyet kesme özellikleri için kullanıma [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

### <a name="turn-on-and-check-out-azure-advisor-recommendations"></a>Açma ve Azure Danışmanı önerileri denetleyin

[Azure Danışmanı](../advisor/advisor-overview.md) olan yardımcı olan bir özellik, düşük kullanım ile kaynakları belirleyerek maliyetleri azaltın. Advisor, Azure portalında ziyaret edin:

![Azure portalında Azure Danışmanı ekran düğmesi](./media/billing-getting-started/advisor-button.PNG)

Ardından, eyleme dönüştürülebilir öneriler alabileceğiniz **maliyet** Danışman Panosu sekmesinde:

![Ekran görüntüsü, Advisor maliyet önerisi örneği](./media/billing-getting-started/advisor-action.PNG)

Daha fazla bilgi için [Advisor maliyet önerileri](../advisor/advisor-cost-recommendations.md).

## <a name="reviewing-costs-at-the-end-of-your-billing-cycle"></a>Bir faturalama döneminin sonunda maliyetleri gözden geçirme

Faturalandırma döngünüzün sonunda faturanız düzenlenir. Ayrıca [faturalar indirin ve ayrıntılı kullanım dosyalarını](billing-download-azure-invoice-daily-usage-date.md) herhangi bir ücret doğru emin olmak için. Günlük kullanımınızla faturanızı ile karşılaştırması hakkında daha fazla bilgi için bkz. [Microsoft Azure için faturanızı anlayın bölümü](billing-understand-your-bill.md).

### <a name="billing-api"></a>Faturalama API’si

Program aracılığıyla kullanım verilerini almak için fatura API'mizi kullanın. RateCard API'si ve kullanım API'si faturalandırılan kullanım almak için birlikte kullanın. Daha fazla bilgi için [Microsoft Azure kaynak tüketiminize öngörü](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a> Ek kaynaklar ve özel durumlar

### <a name="ea-csp-and-sponsorship-customers"></a>Müşterilerin EA, CSP ve sponsorluk
Başlamak için Hesap Yöneticisi veya Azure iş ortağı konuşun.

| Sunduğu | Kaynaklar |
|-------------------------------|-----------------------------------------------------------------------------------|
| Kurumsal Anlaşma (EA) | [EA portal](https://ea.azure.com/), [docs Yardım](https://ea.azure.com/helpdocs), ve [Power BI raporu](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Bulut Çözümü Sağlayıcısı (CSP) | Sağlayıcınıza konuşun |
| Azure Sponsorluğu | [Sponsorluk portalı](https://www.microsoftazuresponsorships.com/) |

Siz yönetiyorsanız BT büyük bir kuruluş için okuma öneririz [Azure Kurumsal iskelesi](/azure/architecture/cloud-adoption-guide/subscription-governance) ve [kurumsal BT teknik incelemesi](https://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (.pdf indirme, yalnızca İngilizce).

#### <a name="EA"></a> Önizleme Kurumsal Anlaşma Azure portalındaki görünümleri maliyeti 

Kurumsal Maliyet görünümleri şu anda genel Önizleme aşamasındadır. Alınacaklar:

- Abonelik maliyetleri kullanımına göre ve önceden ödenmiş tutarlar, fazla kullanımlar, dahil edilen miktarlar, ayarlamalar ve vergiler dahil değildir. Gerçek ücretleri kayıt düzeyinde hesaplanır.
- Azure portalında gösterilen tutarlar Enterprise Portal'da nedir daha farklı olabilir. Enterprise Portal'da güncelleştirmeleri değişiklikleri Azure portalında gösterilmeden önce birkaç dakika sürebilir.
- Maliyetleri görmüyorsanız, aşağıdaki nedenlerden biri olabilir:
    - Abonelik düzeyinde izinlere sahip değilsiniz. Kurumsal Maliyet görünümleri görmek için bir faturalandırma okuyucusu, okuyucu, katılımcı veya abonelik düzeyinde sahibi olmalıdır.
    - Bir hesap sahibi olduğunuz ve kayıt yöneticiniz "ayarını AO ücretleri görüntüle" devre dışı bıraktı.  Kayıt maliyetleri erişim elde etmek için yöneticinize başvurun. 
    - Bir departman Yöneticisi olduğunuzu ve kayıt yöneticiniz "ayarını DA ücretleri görüntüle" devre dışı bıraktı.  Kayıt erişim almak için yöneticinize başvurun.
    - Bir kanal iş ortağı aracılığıyla satın aldığınız Azure ve iş ortağı sürüm fiyatlandırma bilgileri alamadık.  
- Enterprise Portal'da erişim maliyet ilgili ayarlar güncelleştirirseniz, değişiklikleri, Azure portalında gösterilmeden önce birkaç dakika gecikme yoktur.
- Harcama limiti ve fatura Kılavuzu EA abonelikleri için geçerli değildir.

### <a name="check-your-subscription-and-access"></a>Abonelik ve erişim denetimi

Maliyetleri görüntülemek için olmalıdır [fatura bilgileri için abonelik düzeyinde erişim](billing-manage-access.md). Yalnızca Hesap Yöneticisi erişip [hesap Merkezi](https://account.azure.com/Subscriptions), faturalandırma bilgileri değiştirin ve aboneliklerini yönetin. Hesap Yöneticisi kayıt sürecinden geçmeden kişidir. Daha fazla bilgi için [aboneliği veya hizmetleri yöneten Azure yöneticisi rollerini ekleme veya değiştirme](billing-add-change-azure-subscription-administrator.md).

Hesap Yöneticisi olup olmadığınızı görmek için Git [Azure portalında abonelikleri](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Erişiminiz Aboneliklerin listesini inceleyin. Altına bakın **rolüm**. Bu derse *Hesap Yöneticisi*, sonra Tamam'ı. Gibi başka bir şey diyorsa *sahibi*, sonra da tam ayrıcalığa sahip değilsiniz.

![Azure portalında abonelikleri görünümünde rolü ekran görüntüsü](./media/billing-getting-started/sub-blade-view.PNG)

Hesap Yöneticisi değilseniz sonra birisi büyük olasılıkla kısmi erişim kullanarak verdiğiniz [Azure Active Directory rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md) (RBAC). Abonelik ve faturalandırma bilgileri, değişiklik yönetmek için [Hesap Yöneticisi Bul](billing-subscription-transfer.md#whoisaa). Hesap Yöneticisi görevleri gerçekleştirmek için isteyin veya [abonelik için aktarım](billing-subscription-transfer.md).

Hesap yöneticiniz, kuruluşunuz artık ise ve faturalandırma, yönetmeniz gereken [bizimle](https://go.microsoft.com/fwlink/?linkid=2083458).


### <a name="how-to-request-a-service-level-agreement-credit-for-a-service-incident"></a>Nasıl bir hizmet olayı için bir hizmet düzeyi sözleşmesi kredi isteği

Servis Düzeyi Sözleşmeleri (SLA), Microsoft’un çalışma süresi ve bağlantı hakkındaki taahhütlerini açıklar. Azure Hizmetleri, söz konusu etkileri çalışır durumda kalma süresi veya bağlantı, genellikle "kesinti." adlandırılan bir sorunla karşılaştığınızda, bir hizmet olayı bildirilir Ardından biz değil elde etmek ve hizmet düzeyleri her hizmet için SLA açıklandığı gibi korumak, bir bölümü, aylık Hizmet ücretlerini kredilerimi kullanmaya uygun olabilirsiniz.

Bir kredi istemek için:

1. [Azure Portal](https://portal.azure.com/) oturum açın. Birden çok hesabı varsa, Azure kapalı kalma süresi tarafından etkilenen birini kullandığınızdan emin olun. Bu, gerekli arka plan bilgileri otomatik olarak toplamak ve durum daha hızlı çözün desteği yardımcı olur.
2. Yeni bir destek isteği oluşturun.
3. Altında **sorun türü**seçin **faturalama**.
4. Altında **sorun türü**seçin **iade isteği**.
5. Bir SLA kredisine isteyen belirtin, etkilenen hizmetler (VM'ler, Web siteleri, vb.) yanı sıra tarih/saat/saat dilimi bahsetmek için Ayrıntılar ekleyin
6. İletişim bilgilerinizi doğrulayın ve seçin **Oluştur** düğmesi isteğinizi gönderebilirsiniz.

SLA eşik hizmeti tarafından farklılık gösterir. Örneğin, SQL Web Katmanı % 99,9 SLA'ya sahiptir, % 99,95 oranında SLA'sı sanal makinelerinizin ve % 99,99 SLA'sı SQL standart katmanı vardır.

Bazı hizmetler için SLA'sı uygulamak önkoşulları vardır. Örneğin, sanal makineler aynı kullanılabilirlik kümesi'nde dağıtılan bir veya daha fazla örneğe sahip olmalıdır.

Daha fazla bilgi için [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/en-us/support/legal/sla/) belgeleri ve [Azure Hizmetleri için SLA özeti](https://azure.microsoft.com/en-us/support/legal/sla/summary/) belgeleri.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).
