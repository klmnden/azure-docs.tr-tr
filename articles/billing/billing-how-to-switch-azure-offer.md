---
title: Azure abonelik teklifi değiştirme | Microsoft Docs
description: Azure aboneliğinizi değiştirin ve Azure hesap merkezi kullanarak başka bir teklife geç hakkında bilgi edinin
services: ''
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing,top-support-issue
ms.assetid: aae227b3-6d64-4550-a5b6-d359f53f0a59
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: banders
ms.openlocfilehash: bbdcbdc7ef288eeeb279c7e5e59baee492e1f292
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60918776"
---
# <a name="change-your-azure-pay-as-you-go-subscription-to-a-different-offer"></a>Azure Kullandıkça Öde aboneliğinizi farklı bir teklif için değiştirin

Olarak bir [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) müşteri, içinde başka bir teklife Azure aboneliğinizi değiştirebilirsiniz [hesap Merkezi](https://account.windowsazure.com/Subscriptions). Örneğin, yararlanmak için bu özelliği kullanabilirsiniz [Visual Studio aboneleri için aylık kredi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). 

**Yalnızca ücretsiz deneme sürümünden yükseltmek istiyorsunuz?** Bkz: [Kullandıkça Öde aboneliğine yükseltme](billing-upgrade-azure-subscription.md).

## <a name="whats-supported"></a>Ne desteklenir:

| Başlangıç fiyatı | Alıcı |
| --- | --- |
| Kullandıkça Öde |[Kullandıkça Öde geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p/) |
| Kullandıkça Öde |[Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/) |
| Kullandıkça Öde |[Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/) |
| Kullandıkça Öde |[MSDN platformları](https://azure.microsoft.com/offers/ms-azr-0062p/) |
| Kullandıkça Öde |[Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/) |
| Kullandıkça Öde |[Visual Studio Enterprise (Bizspark)](https://azure.microsoft.com/offers/ms-azr-0064p/) |

> [!NOTE]
> Diğer teklif değişiklikleri [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
>
>

## <a name="switch-subscription-offer"></a>Abonelik teklifi değiştirme

> [!VIDEO https://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/Switch-to-a-different-Azure-offer/player]
>
>

1. Oturum açmak [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions).
1. Kullandıkça Öde aboneliğinizi seçin.
1. **Başka bir teklife geç** seçeneğine tıklayın. Bu düğme, yalnızca Kullandıkça Öde aboneliğine sahipseniz ve ilk fatura döneminiz sona erdiyse kullanılabilir.

   ![Sayfanın sağ tarafında anahtar teklif düğmesinde dikkat edin.](./media/billing-how-to-switch-azure-offer/switchbutton.png)
1. **İstediğiniz teklif seçin** tekliflerin listeden aboneliğinizi olarak değiştirilebilir. Bu liste, hesabınızın ilişkilendirildiği üyeliklere göre değişir. Hiçbir şey olup olmadığını denetleyin [kullanılabilir teklifleri için geçiş listesi](#whats-supported) ve üyeliklerini doğru olduğundan emin olun. 

   ![Geçiş yapmak istediğiniz bir teklif seçin](./media/billing-how-to-switch-azure-offer/selectoffer.png)
1. Geçiş yapıyorsanız, teklif bağlı olarak, geçiş etkisi hakkında bir not görebilirsiniz. Bu listede dikkatle gidin ve devam etmeden önce yönergeleri izleyin.

   ![Notlarını gözden geçirin](./media/billing-how-to-switch-azure-offer/thingstonote.png)
1. Aboneliğinizi yeniden adlandırabilirsiniz. Varsayılan olarak, bu yeni teklif adına ayarladık. Tıklayın **anahtar teklif** tıklayarak işlemi tamamlar.

   ![Yeşil düğmeyi tıklatın](./media/billing-how-to-switch-azure-offer/confirmpage.png)
1. Başarılı! Aboneliğiniz, artık yeni teklife geçmesini anahtarlanır.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="what-is-an-azure-offer"></a>Bir Azure teklifine nedir?

Bir Azure teklif *türü* , sahip olduğunuz Azure abonelik. Örneğin, [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/), [Open ile Azure](https://azure.microsoft.com/offers/ms-azr-0111p/), ve [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/) tüm Azure teklifler şunlardır. Her bir teklifin farklı olan [koşulları](https://azure.microsoft.com/support/legal/offer-details/) ve bazı özel avantajlara sahip olursunuz. Aboneliğinizin teklif hesap Merkezi'nde abonelik sayfasında bulunabilir. Daha fazla bilgi almak için teklif adına tıklayın.

   ![Daha fazla bilgi edinmek için hesap Merkezi'nde teklif bağlantısına tıklayın](./media/billing-how-to-switch-azure-offer/offerlink.png)

### <a name="why-dont-i-see-the-button"></a>Düğme neden göremiyorum?

Görmeyebilirsiniz **başka bir teklife geç** düğmesi:

* Açık değil [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/). Şu an için yalnızca Kullandıkça Öde abonelikleri diğer tekliflere dönüştürülebilir.
  * Üzerinde kullanıyorsanız [ücretsiz deneme](https://azure.microsoft.com/free/), bilgi nasıl [Kullandıkça Öde aboneliğine yükseltme](billing-upgrade-azure-subscription.md).
  * Farklı bir abonelikten teklif geçmek [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
* Yine de ilk fatura döneminiz üzerinde olduğunuz; Teklifler, değiştirmeden önce sonlandırmak ilk fatura döneminiz için beklemeniz gerekir.

### <a name="why-do-i-see-there-are-no-offers-available-in-your-region-or-country-at-this-time"></a>"Kullanılabilir teklif, ülke veya bölgenizde şu anda" neden görüyorum?

* Herhangi bir teklif anahtarlar için uygun olmayabilir. Denetleme [kullanılabilir teklifleri için geçiş listesi](#whats-supported) ve Visual Studio veya Bizspark doğru avantaj etkinleştirildikten sonra emin olun.
* Bazı teklifler tüm ülkelerde kullanılamayabilir.

### <a name="what-does-switching-azure-offers-do-to-my-service-and-billing"></a>Azure tekliflerini hizmetime değiştirme ve faturalama yapar?

Değiştirdiğinizde, Azure hesap Merkezi'nde sunar ne ilişkin ayrıntılar aşağıdadır.

#### <a name="no-service-downtime"></a>Hizmet kapalı kalma süresi olmadan

Abonelikle ilişkili herhangi bir kullanıcı için hiçbir hizmet kapalı kalma süresi yoktur. Ancak, geçiş teklif kısıtlamaları olabilir. Örneğin, üretim kaynakları başka bir aboneliğe taşıma gerek üretim sırasında kullanım, bazı teklifler engelliyor.

#### <a name="quota-increases-are-reset"></a>Kota artırımlarına sıfırlanır

Değiştirdiğinizde teklifleri, tüm [sınırı ve kota artırır varsayılan sınırı](../azure-supportability/resource-manager-core-quotas-request.md) sıfırlanır. Varsayılan sınırından daha fazla kaynağa sahip olsanız bile hizmet kapalı kalma süresi yoktur. Örneğin, aboneliğinizde 200 çekirdek kullandığınız ve teklifler geçiş çekirdek kotanız varsayılanına 20 çekirdek sıfırlar. 200 çekirdek kullanan VM'ler bunlardan etkilenmez ve çalışmaya devam eder. Ancak, başka bir kota artırma isteği yapmazsanız, daha fazla çekirdek sağlayamazsınız.

#### <a name="billing"></a>Faturalandırma

Gün, geçiş, fatura için ödenmemiş tüm oluşturulur. Sonra aboneliğinizi yeni teklife ilişkin fiyatlandırma koşulları faturalandırılır. Abonelik faturalama dönümünü teklifler değiştirdiğiniz tarih olarak değiştirir. Kullanım ve indirim değişiklik korunmaz önce öneririz geçmeden önce bir kopyasını indirmek için faturalama.

### <a name="can-i-migrate-from-pay-as-you-go-to-cloud-solution-providerhttpspartnermicrosoftcomsolutionscloud-reseller-overview-csp-or-enterprise-agreementhttpsazuremicrosoftcompricingenterprise-agreement-ea"></a>Gelen Kullandıkça Öde aboneliğine geçiş yapabilirsiniz [bulut çözümü sağlayıcısı](https://partner.microsoft.com/Solutions/cloud-reseller-overview) (CSP) veya [Kurumsal Anlaşma](https://azure.microsoft.com/pricing/enterprise-agreement/) (EA)?

* CSP'ye geçirmek için bkz [Azure Kullandıkça Öde aboneliğine geçiş CSP'ye](https://docs.microsoft.com/azure/cloud-solution-provider/migration/migration-from-payg-to-csp).
* Geçirmek için EA kayıt yöneticiniz hesabınızı EA ekleyin gerekir. Aboneliklerinizi EA kaydı altında taşınmış olması davet e-postadaki yönergeleri izleyin. Daha fazla bilgi için bkz. [mevcut bir hesabı ilişkilendirin](https://ea.azure.com/helpdocs/associateExistingAccount) EA portalında.

### <a name="can-i-migrate-data-and-services-to-a-new-subscription"></a>Yeni bir abonelik için veri ve hizmetlerinizi geçirebilirim?

* Kaynakların doğrudan yeni bir aboneliğe geçirme, bkz: [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/resource-group-move-resources.md).
* Bir Azure aboneliği ve içindeki her şeyi sahipliğini başka bir kişiye aktarmak için bkz: [Azure aboneliğinin sahipliğini devretme](billing-subscription-transfer.md)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).
