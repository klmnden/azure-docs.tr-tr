---
title: Azure, dış hizmet ücretlerini anlama | Microsoft Docs
description: Daha önce Market bilinen dış hizmetlerin ücretleri Azure faturalama hakkında bilgi edinin.
services: ''
documentationcenter: ''
author: jureid
manager: jureid
editor: ''
tags: billing
ms.assetid: 5e0e2a3c-d111-4054-8508-0c111c1b749b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 90753c6046425b60fda04fa99b2952e706d9c0e5
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67311906"
---
# <a name="understand-your-azure-external-services-charges"></a>Azure dış hizmet ücretlerini anlama
Dış hizmetler, Azure Market'teki üçüncü taraf yazılım satıcıları tarafından yayımlanır. Örneğin, SendGrid, Azure'da satın alabilir, ancak Microsoft tarafından yayımlanmayan bir dış hizmetidir. Bazı Microsoft ürünleri, Azure Marketi aracılığıyla çok satılır.

## <a name="how-external-services-are-billed"></a>Dış Hizmetler üzerinden faturalandırılırsınız nasıl

- Varsa bir [Microsoft Müşteri sözleşmesi](#check-access-to-a-microsoft-customer-agreement), üçüncü taraf hizmetlerinizi Azure hizmetlerinizi geri kalanı ile faturalandırılırsınız.
- Microsoft Müşteri sözleşmesi yoksa, dış hizmetleriniz, Azure hizmetlerinden ayrı olarak faturalandırılır.
- Her bir dış hizmet farklı bir faturalandırma modeli vardır. Bazı hizmetler bir Kullandıkça Öde şekilde faturalandırılırken diğer Aylık Ücretler düzelttik.
- Dış hizmetler için aylık ücretsiz kredi kullanamazsınız. İçeren bir Azure aboneliği kullanıyorsanız [ücretsiz krediler](https://azure.microsoft.com/pricing/spending-limits/), bunlar dış hizmetlerden ücretleri için uygulanamaz. Yeni bir dış hizmete veya kaynağa sağladığınızda, bir uyarı gösterilir:

    ![Market satın alma Uyarısı](./media/billing-understand-your-azure-marketplace-charges/credit-warning.png)

<!-- ## View external service spending and history in the Azure portal
You can view a list of the external services that are on each subscription within the [Azure portal](https://portal.azure.com/): 

1. Sign in to the [Azure portal](https://portal.azure.com/) as the account administrator.
2. In the Hub menu, select **Subscriptions**.
   
    ![Select Subscriptions in the Hub menu](./media/billing-understand-your-azure-marketplace-charges/sub-button.png) 
3. In the **Subscriptions** blade, select the subscription that you want to view, and then select **External services**.
   
    ![Select a subscription in the billing blade](./media/billing-understand-your-azure-marketplace-charges/select-sub-external-services.png)
4. You should see each of your external service orders, the publisher name, service tier you bought, name you gave the resource, and the current order status. To see past bills, select an external service.
   
    ![Select an external service](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)
5. From here, you can view past bill amounts including the tax breakdown.
   
    ![View external services billing history](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png) -->

## <a name="view-and-download-azure-marketplace-invoices"></a>Azure Market faturaları görüntüleyip

Varsa bir [Microsoft Müşteri sözleşmesi](#check-access-to-a-microsoft-customer-agreement), üçüncü taraf Azure ücretlerinizin aynı faturada ücretlerdir. Bilgi nasıl [görüntülemenize ve indirmenize Azure faturanızı](billing-download-azure-invoice.md) üçüncü taraf ücretlerinizi görmek için Azure portalından.

Microsoft Müşteri sözleşmesi yoksa, üçüncü taraf ücretleri için farklı faturalar sahip. Görüntüleyebilir ve aşağıdaki adımları izleyerek Azure portalından Azure Marketi faturalarınızda indirin:

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Arama **maliyet Yönetimi + faturalandırma**.
1. Sol menüde **faturalar**.
1. Tıklayarak **Azure Market ve ayırmaları** sekmesi.  ![Azure Market ve ayırmaları sekmesinin resmi](./media/billing-understand-your-azure-marketplace-charges/invoice-tabs.png)
1. Abonelik açılan menü, faturalar için görmek istediğiniz dış hizmetler içeren aboneliği seçin.

## <a name="view-external-service-spending-for-enterprise-agreement-ea-customers"></a>Kurumsal Anlaşma (EA) müşterileri için harcama dış hizmet görüntüle

EA müşterileri, dış hizmet harcama bakın ve EA portal raporların indirin. Bkz: [EA müşterileri için Azure Marketi](https://ea.azure.com/helpdocs/azureMarketplace) kullanmaya başlamak için.

## <a name="manage-payment-methods-for-external-service-orders"></a>Dış hizmet siparişleri için ödeme yöntemlerini Yönet

Bir dış hizmeti satın alırken, kaynak için bir Azure aboneliği seçin. Seçilen Azure aboneliği ödeme yöntemini dış hizmet için ödeme yöntemini olur. Bir dış hizmet için ödeme yöntemini değiştirmek için şunları yapmanız gerekir [ödeme yöntemini Azure aboneliğinin](billing-how-to-change-credit-card.md) bu dış hizmete bağlı. Dış hizmet siparişinizi aşağıdaki adımları izleyerek bağlıdır hangi abonelik şekil:

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Tıklayarak **tüm kaynakları** sol gezinti menüsünde.
     ![tüm kaynaklar menü öğesinin ekran görüntüsü](./media/billing-understand-your-azure-marketplace-charges/all-resources.png)
1. Dış hizmetiniz için arama yapın.
1. Abonelik adını arayın **abonelik** sütun.
    ![Kaynak için Abonelik adı görüntüsü](./media/billing-understand-your-azure-marketplace-charges/sub-selected.png)
1. Abonelik adına tıklayın ve [etkin ödeme yöntemini güncelleştirmesini](billing-how-to-change-credit-card.md).
 
<!-- Update your payment methods for external service orders from the [Account Center](https://account.windowsazure.com/).

> [!NOTE]
> If you purchased your subscription with a Work or School account, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to make changes to your payment method.

1. Sign in to the [Account Center](https://account.windowsazure.com/) and [navigate to the **marketplace** tab](https://account.windowsazure.com/Store)
   
    ![Select marketplace in the account center](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)
2. Select the external service you want to manage
   
    ![Select the external service you want to manage](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)
3. Click **Change payment method** on the right side of the page. This link brings you to a different portal to manage your payment method.
   
    ![Order summary](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)
4. Click **Edit info** and follow instructions to update your payment information.
   
    ![Select edit info](./media/billing-understand-your-azure-marketplace-charges/edit-info.png) -->

## <a name="cancel-an-external-service-order"></a>Bir dış hizmet siparişi iptal etme
Dış hizmet siparişinizi iptal etmek istiyorsanız, kaynak silme [Azure portalında](https://portal.azure.com).

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Tıklayarak **tüm kaynakları** sol gezinti menüsünde.
    ![Tüm kaynaklar menü öğesinin ekran görüntüsü](./media/billing-understand-your-azure-marketplace-charges/all-resources.png)
1. Dış hizmetiniz için arama yapın.
1. Silmek istediğiniz kaynak yanındaki kutuyu işaretleyin.
1. Seçin **Sil** komut çubuğunda.
    ![Delete düğmesinin Ekran görüntüsü](./media/billing-understand-your-azure-marketplace-charges/delete-button.png)
1. Tür *'Evet'* onay dikey penceresinde.
    ![Kaynak silinemedi](./media/billing-understand-your-azure-marketplace-charges/delete-resource.PNG)
1. Tıklayın **Sil**.



## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

