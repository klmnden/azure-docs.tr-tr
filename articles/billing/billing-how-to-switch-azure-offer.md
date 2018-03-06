---
title: "Değişiklik Azure abonelik teklif | Microsoft Docs"
description: "Azure aboneliğinizi değiştirme ve farklı bir teklif Azure hesap Merkezi'ni kullanarak geçiş hakkında bilgi edinin"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing,top-support-issue
ms.assetid: aae227b3-6d64-4550-a5b6-d359f53f0a59
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 08/30/2017
ms.author: genli
ms.openlocfilehash: 381994079b7bcaeff08802b06573b977bf631e9d
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="change-your-azure-pay-as-you-go-subscription-to-a-different-offer"></a>Farklı bir teklif Azure Kullandıkça Öde aboneliğinizi değiştirme

Farklı bir [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) müşteri, başka bir teklif için Azure aboneliğinizin geçirebilirsiniz [hesap Merkezi'nde](https://account.windowsazure.com/Subscriptions). Örneğin, yararlanmak için bu özelliği kullanabilirsiniz [Visual Studio aboneleri için aylık kredilerle](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). 

**Yalnızca ücretsiz deneme sürümünden yükseltmek istediğiniz?** Bkz: [Kullandıkça Öde yükseltme](billing-upgrade-azure-subscription.md).

## <a name="whats-supported"></a>Ne desteklenir:

| Başlangıç | Alıcı |
| --- | --- |
| Kullandıkça Öde |[Kullandıkça Öde geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p/) |
| Kullandıkça Öde |[Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/) |
| Kullandıkça Öde |[Visual Studio Test uzmanı](https://azure.microsoft.com/offers/ms-azr-0060p/) |
| Kullandıkça Öde |[MSDN platformları](https://azure.microsoft.com/offers/ms-azr-0062p/) |
| Kullandıkça Öde |[Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/) |
| Kullandıkça Öde |[Visual Studio Enterprise (Bizspark)](https://azure.microsoft.com/offers/ms-azr-0064p/) |

> [!NOTE]
> Diğer teklif değişiklikleri [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
>
>

## <a name="switch-subscription-offer"></a>Anahtar abonelik teklifi

> [!VIDEO https://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/Switch-to-a-different-Azure-offer/player]
>
>

1. Adresinde oturum açın [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions).
1. Kullandıkça Öde aboneliğinizi seçin.
1. **Başka bir teklife geç** seçeneğine tıklayın. Bu düğme, yalnızca Kullandıkça Öde aboneliğine sahipseniz ve ilk fatura döneminiz sona erdiyse kullanılabilir.

   ![Sayfanın sağ tarafında anahtar teklif düğmesi dikkat edin](./media/billing-how-to-switch-azure-offer/switchbutton.png)
1. **İstediğiniz teklif seçin** teklifleri listesinden aboneliğinizi olarak değiştirilebilir. Bu liste hesabınız ile ilişkili üyeliklerini göre değişir. Hiçbir şey olup olmadığını denetleyin [için geçiş kullanılabilir teklifleri listesi](#whats-supported) ve doğru üyeliklerini olduğundan emin olun. 

   ![Geçiş yapmak istediğiniz bir teklif seçin](./media/billing-how-to-switch-azure-offer/selectoffer.png)
1. Geçiş yapıyorsanız teklif bağlı olarak, geçiş etkisi hakkında bir not görebilirsiniz. Bu liste dikkatle gidin ve devam etmeden önce yönergeleri izleyin.

   ![Notlarını gözden geçirin](./media/billing-how-to-switch-azure-offer/thingstonote.png)
1. Aboneliğinizi yeniden adlandırabilirsiniz. Varsayılan olarak, biz bunu yeni teklif adına ayarlayın. Tıklatın **anahtar teklif** işlemi tamamlamak için.

   ![Yeşil düğmesini tıklatın](./media/billing-how-to-switch-azure-offer/confirmpage.png)
1. Başarılı! Aboneliğiniz için yeni teklif artık anahtarlanır.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="what-is-an-azure-offer"></a>Bir Azure teklifi nedir?

Bir Azure teklifidir *türü* sahip olduğunuz Azure aboneliğinin. Örneğin, [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/), [Open ile Azure](https://azure.microsoft.com/offers/ms-azr-0111p/), ve [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/) olan tüm Azure sunar. Her teklif farklı olan [koşulları](https://azure.microsoft.com/support/legal/offer-details/) ve bazı özel avantajlara sahip olursunuz. Aboneliğinizi teklif hesap Merkezi'nde abonelik sayfasında bulunabilir. Daha fazla bilgi almak için teklif adına tıklayın.

   ![Daha fazla bilgi almak için hesap Merkezi'nde teklif bağlantısına tıklayın](./media/billing-how-to-switch-azure-offer/offerlink.png)

### <a name="why-dont-i-see-the-button"></a>Düğme neden göremiyorum?

Görmemiş olabilirsiniz **başka bir teklife geçiş** varsa düğmesi:

* Açık değil [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/). Şu anda başka bir teklife yalnızca Kullandıkça Öde abonelikleri dönüştürülebilir.
  * Üzerinde iseniz [ücretsiz deneme](https://azure.microsoft.com/free/), bilgi nasıl [Kullandıkça Öde yükseltme](billing-upgrade-azure-subscription.md).
  * Farklı bir abonelik tekliften geçiş yapmak için [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
* Yine, ilk fatura dönemi olduğunuz; teklifleri geçiş yapmadan önce sona erdirmek, ilk fatura dönemi için beklemeniz gerekir.

### <a name="why-do-i-see-there-are-no-offers-available-in-your-region-or-country-at-this-time"></a>"Teklif yok kullanılabilir, ülke veya bölgenizde şu anda" neden görüyorum?

* Bir teklif anahtar için uygun olmayabilir. Denetleme [için geçiş kullanılabilir teklifleri listesi](#whats-supported) ve Visual Studio veya Bizspark sağ faydalarıyla etkinleştirdiğinizden emin olun.
* Bazı teklifleri tüm ülkelerde kullanılamayabilir.

### <a name="what-does-switching-azure-offers-do-to-my-service-and-billing"></a>Azure teklifleri my hizmetine geçiş yapma ve faturalama yaptığı?

Burada, değiştirdiğinizde Azure hesap Merkezi'nde sunar olanlar Ayrıntılar verilmiştir.

#### <a name="no-service-downtime"></a>Hizmeti kapalı kalma süresi yok

Abonelikle ilişkili tüm kullanıcılar için bir hizmet kesintisi yok. Bununla birlikte, geçiş teklif sınırlamaları olabilir. Örneğin, başka bir abonelik için üretim kaynakları taşımak gereken şekilde bazı teklifleri üretim kullanılmasını engeller.

#### <a name="quota-increases-are-reset"></a>Kota artar Sıfırla

Geçirdiğinizde teklifleri, her [sınırı ve kota artırır varsayılan sınır](../azure-supportability/resource-manager-core-quotas-request.md) sıfırlanır. Varsayılan sınırından daha fazla kaynak olsa bile herhangi bir hizmet kesintisi yok. Örneğin, 200 çekirdek, aboneliğinizde kullanıyorsanız, ardından teklifleri geçiş çekirdek kota varsayılanına 20 olarak belirlenen çekirdek sıfırlar. 200 çekirdek kullanan sanal makineleri etkilenmez ve çalışmaya devam eder. Ancak, başka bir kota isteği artırmak yapmazsanız, tüm daha fazla sayıda çekirdek sağlayamazsınız.

#### <a name="billing"></a>Faturalandırma

Geçiş günde bir fatura için tüm bekleyen ücretleri oluşturulur. Ardından, aboneliğinizin yeni teklif 's fiyatlandırma koşulları faturalandırılır. Abonelik faturalama Yıldönümü teklifleri değiştirdiğiniz tarih değiştirir. Kullanım ve teklif değişiklik korunmaz önce öneririz değiştirmeden önce bir kopyasını indirmek için veri faturalama.

### <a name="can-i-migrate-from-pay-as-you-go-to-cloud-solution-providerhttpspartnermicrosoftcomsolutionscloud-reseller-overview-csp-or-enterprise-agreementhttpsazuremicrosoftcompricingenterprise-agreement-ea"></a>Kullandıkça Öde aboneliğine gelen geçirebilirsiniz [bulut çözümü sağlayıcısı](https://partner.microsoft.com/Solutions/cloud-reseller-overview) (CSP) veya [Kurumsal Anlaşma](https://azure.microsoft.com/pricing/enterprise-agreement/) (EA)?

* CSP'ye geçirmek için bkz [Azure Pas olarak-size-Git abonelik geçişi için CSP](https://docs.microsoft.com/azure/cloud-solution-provider/migration/migration-from-payg-to-csp).
* İçin EA geçirmek için kayıt hesabınızı EA ekleme yöneticiniz olması. Aboneliklerinizi EA kayıt altında taşınmış olması için davet e-postadaki yönergeleri izleyin. Daha fazla bilgi için bkz: [var olan bir hesabı ilişkilendirin](https://ea.azure.com/helpdocs/associateExistingAccount) EA Portalı'nda.

### <a name="can-i-migrate-data-and-services-to-a-new-subscription"></a>Yeni bir abonelik için veri ve hizmetlerini geçirebilirsiniz?

* Kaynakların doğrudan yeni aboneliği taşımak, bkz: [yeni kaynak grubu veya abonelik kaynaklarını taşıma](../azure-resource-manager/resource-group-move-resources.md).
* Bir Azure aboneliği ve içindeki her şeyi sahipliğini başka birine aktarmak için bkz: [bir Azure aboneliği sahipliğini aktarma](billing-subscription-transfer.md)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala daha fazla, sorularınız varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
