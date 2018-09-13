---
title: Azure abonelikleri için fatura veya kredi uyarıları ayarlama | Microsoft Docs
description: Hem de fatura sürprizlerinden kaçınabilirsiniz şekilde nasıl uyarılar Azure faturanızda ayarlayabileceğiniz açıklanmıştır.
keywords: Kredi uyarı, fatura Uyarısı
services: billing
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: adpick
ms.openlocfilehash: 094bfe041ae04e52b6d998ccfd1d964abf192d6a
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35982903"
---
# <a name="set-up-billing-or-credit-alerts-for-your-microsoft-azure-subscriptions"></a>Microsoft Azure Abonelikleriniz için fatura veya kredi uyarıları ayarlama
Bir Azure aboneliğine yönelik hesap yöneticisi değilseniz, özelleştirilmiş oluşturmak için Azure fatura uyarısı Hizmeti'ni kullanabilirsiniz yardımcı faturalama uyarılarını izleme ve Azure hesaplarınızı için faturalandırma etkinliğini yönetin.

Bu hizmeti Önizleme aşamasında olduğundan Önizleme özellikleri sayfasında önce etkinleştirmeniz gerekir.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="set-the-alert-threshold-and-email-recipients"></a>Uyarı eşiğini ve e-posta alıcıları ayarlayın
1. Ziyaret [Önizleme özellikleri sayfası](https://account.windowsazure.com/PreviewFeatures) ve etkinleştirme **faturalandırma uyarısı hizmeti**.

1. Aboneliğiniz için faturalama hizmeti açık e-posta onayı aldıktan sonra ziyaret [abonelikler sayfasından](https://account.windowsazure.com/Subscriptions) hesap portalı. İzleyin ve ardından istediğiniz aboneliğe tıklayın **uyarılar**.

    ![Azure hesap merkezi abonelik görünümünü ekran görüntüsü ile vurgulanmış uyarıları][Image1]

2. Ardından, **ekleme uyarı** , ilk bir tane oluşturmak için. Abonelik farklı bir eşik ile ve her uyarı için iki e-posta alıcılarını en fazla beş faturalama uyarılarını toplam ayarlayabilirsiniz.

    ![Ekran görüntüsü, uyarı ekleyebileceğiniz uyarılar görünümü][Image2]

3. Bir uyarı eklediğinizde, harcama eşiği seçin benzersiz bir ad verin ve uyarıların gönderileceği e-posta adreslerini seçin. Eşiği ayarlarken ya da tercih edebilirsiniz bir **toplam fatura** veya **para kredisi** gelen **için uyarı** listesi. Bir fatura toplamı, abonelik harcama eşiği aştığında bir uyarı gönderilir. Krediler sınırın altına düşürdüğünüzde, kredi için bir uyarı gönderilir. Krediler, genellikle ücretsiz deneme sürümü ve Visual Studio abonelikleri için geçerlidir.

    ![Alıcılar yapılandırabileceğiniz uyarı toplama görünümünün ekran görüntüsü][Image3]

Azure, herhangi bir e-posta adresi destekler, ancak e-posta adresi çalışır, bu nedenle, yazım hataları denetleyin doğrulamaz.

## <a name="check-on-your-alerts"></a>Üzerinde uyarıları denetleyin
Uyarılar ayarlayın, sonra hesap Merkezi'ne bunları listeler ve daha fazla ayarlayabilirsiniz kaç gösterir. Her uyarı için tarih ve toplam fatura veya kredi için bir uyarı olup olmadığını gönderildiği saat ve ayarladığınız sınırı bakın. Tarih ve saat biçimi 24 saat Evrensel Saat koordine (UTC) ve tarih yyyy-aa-gg biçimindeki. Düzenlemek için listedeki bir uyarı için artı işaretine tıklayın veya silmek için çöp kutusu tıklayın.

## <a name="delete-alerts-or-email-addresses-from-the-azure-billing-alert-service"></a>Azure fatura uyarısı hizmetindeki uyarılar veya e-posta adreslerini Sil
Herhangi bir bilgi hizmetten kaldırmak gerekiyorsa dosya çubuğunda e-posta adresini güncelleştirin veya uyarıyı tamamen silin.

   ![Kişisel bilgiler nerede kaldırabilirsiniz uyarı silme görünümünün ekran görüntüsü][Image4]

## <a name="billing-alerts-for-enterprise-agreement-ea-customers"></a>Kurumsal Sözleşme (EA) müşterileri için fatura uyarıları
EA aboneliklerinde bu hizmet tarafından desteklenmiyor, bunun yerine EA müşterileri uyarılar bir kayıt altında her departman için harcama kotalarını alabilirsiniz. Bkz: [departmanı harcama kotalarını](https://ea.azure.com/helpdocs/departmentSpendingQuotas) kullanmaya başlamak için EA portalında.

## <a name="learn-more-about-azure-cost-management"></a>Azure maliyet yönetimi hakkında daha fazla bilgi edinin
- Tahmini maliyetleri kullanarak [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/), [toplam sahiplik hesaplayıcı maliyeti](https://aka.ms/azure-tco-calculator), ve bir hizmet eklediğinizde.
- [Kullanımı ve maliyetleri Azure portalında düzenli olarak gözden geçirme](billing-getting-started.md#costs).
- Açma [maliyet önerileri Azure Danışmanı](../advisor/advisor-cost-recommendations.md).

Daha fazla bilgi için bkz. [Azure maliyet Yönetimi Kılavuzu](billing-getting-started.md).

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png 
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png 
[Image4]: ./media/azure-billing-set-up-alerts/AlertsDeleteScreen1.PNG
