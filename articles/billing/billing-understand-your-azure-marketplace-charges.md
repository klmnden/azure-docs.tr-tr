---
title: Azure, dış hizmet ücretlerini anlama | Microsoft Docs
description: Daha önce Market bilinen dış hizmetlerin ücretleri Azure faturalama hakkında bilgi edinin.
services: ''
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
tags: billing
ms.assetid: 5e0e2a3c-d111-4054-8508-0c111c1b749b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ae9c2c975bf49725be1858ad02a1c4b90ef58a7f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60370619"
---
# <a name="understand-your-azure-billing-for-external-service-charges"></a>Azure faturalandırmanızı dış hizmet ücretlerini anlama
Dış hizmetler, Azure Market'teki üçüncü taraf yazılım satıcıları tarafından yayımlanır. Örneğin, SendGrid Azure'da satın alabilir, ancak Microsoft tarafından yayımlanmayan bir dış hizmetler olur.

Yeni bir dış hizmete veya kaynağa sağladığınızda, bir uyarı gösterilir:

![Market satın alma Uyarısı](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

> [!NOTE]
> Dış Hizmetler Microsoft olmayan şirketler tarafından yayımlanır, ancak bazen Microsoft ürünleri dış hizmetler da ayrılır.
> 
> 

## <a name="how-external-services-are-billed"></a>Dış Hizmetler üzerinden faturalandırılırsınız nasıl
- Dış hizmetler ayrı olarak faturalandırılır. Bunlar, Azure aboneliğinizde ayrı siparişler olarak kabul edilir. Her hizmet için fatura döneminde, hizmeti satın aldığınızda ayarlanır. Bu alan aboneliğin faturalandırma döneminden karıştırılmamalıdır için. Ayrıca ayrı bir fatura alırsınız ve kredi kartınızdan ayrı olarak ücretlendirilir.
- Her bir dış hizmet farklı bir faturalandırma modeli vardır. Başkalarının aylık tabanlı ödemesi model kullanırken bazı hizmetleri Kullandıkça Öde bir biçimde faturalandırılır. Dış Azure Hizmetleri için bir kredi kartı gerekir, fatura ödeme ile dış hizmetler satın alamazsınız.
- Dış hizmetler için aylık ücretsiz kredi kullanamazsınız. İçeren bir Azure aboneliği kullanıyorsanız [ücretsiz krediler](https://azure.microsoft.com/pricing/spending-limits/), dış hizmet faturaları için uygulanamaz. Dış hizmetler satın almak için bir kredi kartı kullanın.

## <a name="view-external-service-spending-and-history-in-the-azure-portal"></a>Dış hizmet görünümü harcama ve Azure Portalı'nda geçmişi
Her abonelikte bulunan dış hizmetlerin bir listesini görüntüleyebileceğiniz [Azure portalında](https://portal.azure.com/): 

1. Oturum [Azure portalında](https://portal.azure.com/) hesap yöneticisi olarak.
2. Hub menüsünde seçin **abonelikleri**.
   
    ![Hub menüsünde abonelik seçin](./media/billing-understand-your-azure-marketplace-charges/sub-button.png) 
3. İçinde **abonelikleri** dikey penceresinde, görüntülemek istediğiniz aboneliği seçin ve ardından **dış Hizmetler**.
   
    ![Faturalama dikey penceresinde bir abonelik seçin](./media/billing-understand-your-azure-marketplace-charges/select-sub-external-services.png)
4. Her bir dış hizmet siparişlerinizi Yayımcı adı, satın aldığınız hizmet katmanı, kaynak ve geçerli sıra durumu verdiğiniz ad görmeniz gerekir. Faturalar görmek için bir dış hizmet seçin.
   
    ![Bir dış hizmet seçin](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)
5. Burada, fatura miktarları vergi dökümünü dahil olmak üzere görüntüleyebilirsiniz.
   
    ![Faturalama geçmişi görünümü dış hizmetler](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="view-external-service-spending-for-enterprise-agreement-ea-customers"></a>Kurumsal Anlaşma (EA) müşterileri için harcama dış hizmet görüntüle
EA müşterileri, dış hizmet harcama bakın ve EA portal raporların indirin. Bkz: [EA müşterileri için Azure Marketi](https://ea.azure.com/helpdocs/azureMarketplace) kullanmaya başlamak için.

## <a name="manage-payment-methods-for-external-service-orders"></a>Dış hizmet siparişleri için ödeme yöntemlerini Yönet
Ödeme yöntemlerinizi dış hizmet siparişleri için güncelleştirme [hesap Merkezi](https://account.windowsazure.com/).

> [!NOTE]
> Aboneliğinize bir iş veya Okul hesabıyla satın aldıysanız [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ödeme yönteminizi değişiklik yapma.
> 
> 

1. Oturum [hesap Merkezi](https://account.windowsazure.com/) ve [gidin **Market** sekmesi](https://account.windowsazure.com/Store)
   
    ![Market hesap Merkezi'nde seçin](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)
2. Yönetmek istediğiniz dış hizmeti seçin
   
    ![Yönetmek istediğiniz dış hizmeti seçin](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)
3. Tıklayın **ödeme yöntemini değiştirme** sayfanın sağ tarafındaki. Bu bağlantı, ödeme yöntemini yönetmek için farklı bir portal sunar.
   
    ![Sipariş Özeti](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)
4. Tıklayın **bilgileri Düzenle** ve Ödeme bilgilerinizi güncelleştirmek için yönergeleri izleyin.
   
    ![Bilgileri Düzenle](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)

## <a name="cancel-an-external-service-order"></a>Bir dış hizmet siparişi iptal etme
Dış hizmet siparişinizi iptal etmek istiyorsanız, kaynak silme [Azure portalında](https://portal.azure.com).

![Kaynak silinemedi](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

