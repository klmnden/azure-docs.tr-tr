---
title: Azure, dış hizmet ücretlerini anlama | Microsoft Docs
description: Önceden Market bilinen dış hizmetler, Azure ücretlere faturalama hakkında bilgi edinin.
services: ''
documentationcenter: ''
author: adpick
manager: tonguyen
editor: ''
tags: billing
ms.assetid: 5e0e2a3c-d111-4054-8508-0c111c1b749b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/9/2017
ms.author: adpick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6f7d8b89806e1f6d59e1a64e589558cd972f4fdc
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="understand-your-azure-billing-for-external-service-charges"></a>Azure fatura dış servis ücretleri anlamak
Dış hizmetler Azure markette üçüncü taraf yazılım satıcıları tarafından yayımlanır. Örneğin, ClearDB ve SendGrid Azure'da satın alabilir, ancak Microsoft tarafından yayımlanmayan dış hizmetleridir.

Yeni bir dış hizmet veya kaynak sağladığınızda, bir uyarı gösterilir:

![Market satın uyarı](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

> [!NOTE]
> Dış Hizmetler Microsoft olmayan şirketler tarafından yayımlanır, ancak bazen Microsoft ürünleri dış hizmetler de ayrılır.
> 
> 

## <a name="how-external-services-are-billed"></a>Dış hizmetler faturalandırılır nasıl
- Dış hizmetler ayrı ayrı faturalandırılır. Bunlar, Azure aboneliğinizin içinde tek tek siparişleri olarak kabul edilir. Hizmeti satın aldığınız her hizmet için fatura döneminde ayarlanır. Altında aldığınız abonelik faturalama dönemi ile karıştırılmamalıdır için. Ayrıca ayrı faturaları aldığınız ve kredi kartınızdan ayrı olarak ücretlendirilir.
- Dış her hizmetin farklı bir fatura modeli vardır. Diğer bir aylık tabanlı ödeme modeli kullanırken bazı hizmetler Kullandıkça Öde biçimde faturalandırılır. Azure dış hizmetler için bir kredi kartı gerekir, fatura ödeme ile dış hizmetler satın alın.
- Dış Hizmetleri için aylık ücretsiz krediler kullanamazsınız. İçeren bir Azure aboneliği kullanıyorsanız [ücretsiz krediler](https://azure.microsoft.com/pricing/spending-limits/), dış hizmet faturaları uygulanamaz. Dış hizmetler satın almak için bir kredi kartı kullanın.

## <a name="view-external-service-spending-and-history-in-the-azure-portal"></a>Görünüm dış hizmet harcama ve Azure portalında geçmişi
Her abonelik içinde bulunan dış hizmetlerin bir listesini görüntüleyebileceğiniz [Azure portal](https://portal.azure.com/): 

1. Oturum [Azure portal](https://portal.azure.com/) hesabı yönetici olarak.
2. Hub menüsünde seçin **abonelikleri**.
   
    ![Hub menüsünde aboneliklerini seçin](./media/billing-understand-your-azure-marketplace-charges/sub-button.png) 
3. İçinde **abonelikleri** dikey penceresinde, görüntülemek istediğiniz aboneliği seçin ve ardından **dış Hizmetler**.
   
    ![Faturalama dikey penceresinde bir abonelik seçin](./media/billing-understand-your-azure-marketplace-charges/select-sub-external-services.png)
4. Her bir dış hizmet siparişler, yayımcı adını, satın aldığınız hizmet katmanı, kaynak ve geçerli sıra durumu verdiğiniz adı görmeniz gerekir. Faturaları görmek için bir dış hizmet seçin.
   
    ![Bir dış hizmet seçin](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)
5. Buradan, vergi dökümünü dahil olmak üzere fatura miktarda görüntüleyebilirsiniz.
   
    ![Dış Hizmetleri fatura geçmişini görüntüle](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="view-external-service-spending-for-enterprise-agreement-ea-customers"></a>Kurumsal Anlaşma (Kurumsal Sözleşme) müşteriler için harcama dış Service'i Görüntüle
EA müşteriler dış hizmet harcama bakın ve EA portalında raporları indirin. Bkz: [EA müşteriler için Azure Marketi](https://ea.azure.com/helpdocs/azureMarketplace) başlamak için.

## <a name="manage-payment-methods-for-external-service-orders"></a>Dış servis siparişleri için ödeme yöntemleri yönetme
Dış servis siparişleri için Ödeme yöntemleriniz güncelleştirme [hesap Merkezi'nde](https://account.windowsazure.com/).

> [!NOTE]
> Bir iş veya Okul hesabı aboneliğinizle satın aldıysanız [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ödeme yönteminizi değişiklik yapma.
> 
> 

1. Oturum [hesap Merkezi'nde](https://account.windowsazure.com/) ve [gidin **Market** sekmesi](https://account.windowsazure.com/Store)
   
    ![Market hesap Merkezi'nde seçin](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)
2. Yönetmek istediğiniz dış hizmeti seçin
   
    ![Yönetmek istediğiniz dış hizmeti seçin](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)
3. Tıklatın **ödeme yöntemini değiştirme** sayfasının sağ tarafında. Bu bağlantıyı ödeme yönteminizi yönetmek için farklı bir portal getirir.
   
    ![Sipariş Özeti](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)
4. Tıklatın **Düzenle bilgisi** ve Ödeme bilgilerinizi güncellemek için yönergeleri izleyin.
   
    ![Bilgi Düzenle](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)

## <a name="cancel-an-external-service-order"></a>Bir dış hizmet siparişi iptal etme
Dış hizmet siparişinizi iptal etmek istiyorsanız, kaynak silin [Azure portal](https://portal.azure.com).

![Kaynağı silme](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.
Hala sorularınız varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.

