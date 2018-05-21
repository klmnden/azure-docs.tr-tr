---
title: Azure abonelikleri için faturalama veya kredi uyarıları ayarlama | Microsoft Docs
description: Fatura beklenmeyen durumları kaçınmak için nasıl uyarılarını Azure faturanızda ayarlayabileceğiniz açıklar.
keywords: Kredi uyarı, fatura Uyarısı
services: ''
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
tags: billing
ms.assetid: 9b7b3eeb-cd9d-4690-86a3-51b1e2a8974f
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/9/2017
ms.author: adpick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fa7d1853226647917925e8c75e01a1c83d84daeb
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="set-up-billing-or-credit-alerts-for-your-microsoft-azure-subscriptions"></a>Microsoft Azure Abonelikleriniz için faturalama veya kredi uyarıları ayarlama
Bir Azure aboneliğine yönelik hesap yöneticisi değilseniz, özelleştirilmiş oluşturmak için Azure faturalama uyarı hizmetini kullanabilirsiniz yardımcı faturalama uyarılarını izleme ve Azure hesaplarınız için fatura etkinlik yönetme.

Bu hizmet önizlemede olduğundan, Önizleme özellikleri sayfasında ilk etkinleştirmeniz gerekir.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="set-the-alert-threshold-and-email-recipients"></a>Uyarı eşiği ve e-posta alıcılarının ayarlama
1. Ziyaret [Önizleme özellikleri sayfasında](https://account.windowsazure.com/PreviewFeatures) ve etkinleştirme **faturalama uyarı hizmeti**.

1. Aboneliğiniz için faturalama hizmeti açık e-posta onayı aldıktan sonra ziyaret [abonelik sayfasında](https://account.windowsazure.com/Subscriptions) hesap portalındaki. İzleme ve ardından istediğiniz aboneliğe tıklayın **uyarıları**.

    ![Azure hesap merkezi abonelikleri görünümünü ekran vurgulanmış uyarılar][Image1]

2. Bundan sonra öğesini **eklemek uyarı** , ilk bir tane oluşturmak için. Abonelik, her uyarı için iki e-posta alıcıları kadar farklı bir eşik ile başına beş faturalama uyarılarını toplam ayarlayabilirsiniz.

    ![Uyarı, ekleyebileceğiniz uyarıları görünümünün ekran görüntüsü][Image2]

3. Bir uyarı eklediğinizde, harcama eşik seçin benzersiz bir ad verin ve Uyarıları gönderildiği e-posta adresi seçin. Eşik ayarlarken ya da seçebilirsiniz bir **faturalama toplamı** veya **kredi** gelen **için uyarı** listesi. Abonelik harcama eşiği aştığında bir faturalama toplamı bir uyarı gönderilir. Parasal kredileriniz sınırın altına bıraktığınızda kredi için bir uyarı gönderilir. Parasal kredileriniz genellikle ücretsiz deneme sürümü ve Visual Studio abonelikler için geçerlidir.

    ![Alıcılar yapılandırabileceğiniz uyarı toplama görünümünün ekran görüntüsü][Image3]

Azure herhangi bir e-posta adresi destekler, ancak e-posta adresi çalışır, böylece olduğunu yazım hataları denetleyin doğrulayın değil.

## <a name="check-on-your-alerts"></a>Uyarılarınızı üzerinde denetleyin
Uyarıları ayarladıktan sonra hesap merkezi bunları listeler ve daha ayarlayabilirsiniz kaç gösterir. Her uyarı için tarih ve Saat Faturalama toplamı veya kredi için bir uyarı olup olmadığını gönderildiği ile ayarladığınız sınırı bakın. Tarih ve saat biçimi 24 saat Evrensel Saat koordinatı'nı (UTC) ve tarih yyyy-aa-gg biçiminde. Düzenlemek için listedeki bir uyarı için artı işaretini tıklatın veya silmek için çöp can'ı tıklatın.

## <a name="billing-alerts-for-enterprise-agreement-ea-customers"></a>Kurumsal Anlaşma (Kurumsal Sözleşme) müşteriler için fatura uyarıları
EA abonelikleri bu hizmet tarafından desteklenmiyor, bunun yerine EA müşteriler uyarılar için bir kayıt altında her bölüm kotaları harcama ayarlayarak elde edebilirsiniz. Bkz: [departmanı harcama kotaları](https://ea.azure.com/helpdocs/departmentSpendingQuotas) başlamak için EA Portalı'nda.

## <a name="learn-more-about-azure-cost-management"></a>Azure maliyeti yönetimi hakkında daha fazla bilgi edinin
- Tahmini maliyetleri kullanarak [fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/), [toplam sahip olma hesaplayıcı maliyeti](https://aka.ms/azure-tco-calculator), ve bir hizmet eklediğinizde.
- [Kullanım ve maliyetler Azure portalında düzenli olarak gözden](billing-getting-started.md#costs).
- Aç [Azure Advisor önerileri maliyet](../advisor/advisor-cost-recommendations.md).

Daha fazla bilgi için bkz: [Azure maliyeti Yönetim Kılavuzu](billing-getting-started.md).

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png 
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png 
