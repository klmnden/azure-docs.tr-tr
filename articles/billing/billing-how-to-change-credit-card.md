---
title: Azure için kredi kartınızdan değiştirme
description: Bir Azure aboneliği ödemesi için kullanılan kredi kartını değiştirme işlemini açıklamaktadır.
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 7c04e33d6199d3930be28ce84458e9c3a743eb8a
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491300"
---
# <a name="add-update-or-remove-a-credit-card-for-azure"></a>Ekleme, güncelleştirme veya Azure için bir kredi kartını kaldırma

Azure portalında yeni bir kredi kartı eklemeniz, kullanılan mevcut kredi kartını güncelleştirmek veya kullanmadığınız bir kredi kartı Sil. Siz bir [Hesap Yöneticisi](billing-subscription-transfer.md#whoisaa) bu değişiklikleri yapmak için.

Varsa bir [Microsoft Müşteri sözleşmesi](#check-access-to-a-microsoft-customer-agreement), profilleri fatura ile ödeme yöntemlerinizi ilişkilendirilir. Bilgi edinmek için nasıl [varsayılan ödeme yöntemini fatura profili için](#change-payment-method-for-a-billing-profile). Azure için kaydolan kullanıcı, ödeme yöntemini güncelleştirebilirsiniz.

**(Onay/havale) faturayla ödeme geçmek istiyorsunuz?** Bkz: [Azure abonelikleri için fatura ile ödeme](billing-how-to-pay-by-invoice.md).

<a id="addcard"></a>

## <a name="add-a-new-credit-card-to-an-azure-subscription"></a>Yeni bir kredi kartı için Azure aboneliği ekleme

1. Oturum [Azure portalında](https://portal.azure.com) hesap yöneticisi olarak.
1. Arama **maliyet Yönetimi + faturalandırma**.

    ![Arama gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/search.png)

1. Kredi kartı eklemek istediğiniz aboneliği seçin.
1. **Ödeme yöntemleri**'ni seçin.

    ![Yönet ödeme yöntemleri seçeneği belirlenmiş gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Sol üst köşede “+” işaretini seçerek kartı ekleyin. Sağ tarafta bir kredi kartı formu görüntülenir.
1. Kredi kartı bilgileri girin.

    ![Yeni bir kart gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/sub-add-new-x.png)

1. Bunu etkin ödeme yönteminiz kartında, yanındaki kutuyu işaretleyin için **bunu etkin Ödeme Yöntemim yap** formun üstünde. Bu kart, seçilen abonelik olarak aynı kartı kullanan tüm aboneliklerde etkin ödeme aracınız olur.

1. **İleri**’yi seçin.

Kredi kartı ekledikten sonra hata alırsanız bkz [Azure kayıt sırasında kredi kartı reddedildi](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="update-existing-credit-card"></a>Mevcut kredi kartını güncelleştirme

Kredi kartınız yenilenmiş ve sayı aynı kalır, sona erme tarihi gibi mevcut kredi kartı bilgileri güncelleştirin. Kart kaybolduğu için kredi kartı numarası değişikliklerinizi çalındıysa veya süresi dolduğunda, adımları izlerseniz, [bir ödeme yöntemi olarak kredi kartı eklemeniz](#addcard) bölümü. Kart doğrulama kodu güncelleştirmeniz gerekmez.

1. Oturum [Azure portalında](https://portal.azure.com) hesap yöneticisi olarak.
1. Arama **maliyet Yönetimi + faturalandırma**.

    ![Arama gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/search.png)

1. **Ödeme yöntemleri**'ni seçin.

    ![Yönet ödeme yöntemleri seçeneği belirlenmiş gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Düzenlemek istediğiniz kredi kartına tıklayın. Sağ tarafta bir kredi kartı formu görüntülenir.

    ![Kredi kartı seçili gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/edit-card-x.png)

1. Kredi kartı bilgileri güncelleştirin.
1. **Kaydet**’i seçin.

## <a name="use-a-different-credit-card"></a>Farklı bir kredi kartı kullanın

Aynı etkin ödeme yöntemi birden fazla aboneliğiniz varsa, ardından bu aboneliklerden herhangi biri üzerinde etkin ödeme yöntemini değiştirme diğerleri üzerinde etkin ödeme yöntemi de güncelleştirir.

1. Oturum [Azure portalında](https://portal.azure.com) hesap yöneticisi olarak.
1. Arama **maliyet Yönetimi + faturalandırma**.

    ![Arama gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/search.png)

1. Kredi kartı eklemek istediğiniz aboneliği seçin.
1. **Ödeme yöntemleri**'ni seçin.

    ![Yönet ödeme yöntemleri seçeneği belirlenmiş gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Etkin ödeme yöntemi yapmak istediğiniz kartın yanındaki kutuyu seçin.
1. Tıklayın **etkin Ayarla**.
    ![Kredi kartı gösteren ekran görüntüsü, seçilen ve etkin olarak ayarlayın.](./media/billing-how-to-change-credit-card/sub-change-active-x.png)

## <a name="remove-a-credit-card-from-the-account"></a>Kredi kartı hesaptan Kaldır

1. Oturum [Azure portalında](https://portal.azure.com) hesap yöneticisi olarak.
1. Seçin **maliyet Yönetimi + faturalandırma** sayfanın sol tarafındaki.

    ![Arama gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/search.png)

1. Altında **faturalama**seçin **ödeme yöntemlerini**.

    ![Yönet ödeme yöntemleri seçeneği belirlenmiş gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Kaldırmak istediğiniz kartın yanındaki kutuyu seçin.
1. Tıklayın **Sil**.

Kredi kartınızdan herhangi bir Microsoft abonelikler için etkin ödeme yöntemi varsa, Azure hesabınızdan kaldıramazsınız. Bu kredi kartına bağlı olan tüm abonelikler için etkin ödeme yöntemini değiştirme ve yeniden deneyin
<!-- # Add, update, or remove a credit card for Azure

In the Account Center, you can add a new credit card, update an existing credit card, or delete a credit card that you don't use. You must be an [Account Administrator](billing-subscription-transfer.md#whoisaa) to make these changes.

**Want to switch to pay by invoice?** See [Pay for Azure subscriptions by invoice](billing-how-to-pay-by-invoice.md).

<a id="addcard"></a>

## Add a new credit or debit card

1. Sign in to the [Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select a subscription.
1. On the right side of the page, select **Manage payment methods**.

    ![Screenshot that shows Manage payment methods option selected.](./media/billing-how-to-change-credit-card/changesub_new.png)
1. Select “+” to add a card.

    ![Screenshot that shows the edit option next to the payment method.](./media/billing-how-to-change-credit-card/editcard_new.png)
1. Enter credit or debit card details.
1. Select **Save**.

If you get an error after you add the credit card, see [Credit card declined at Azure sign-up](billing-credit-card-fails-during-azure-sign-up.md).

## Update existing credit or debit card

If your credit card gets renewed and the number remains the same, update the existing credit card details like the expiration date. If your credit card number changes because the card is lost, stolen, or expired, follow the steps in the [Add a credit card as a payment method](#addcard) section. You don't need to update the CVV.

1. Sign in to the [Azure Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select the subscription that's linked to the card.
1. Select **Manage payment methods**.
1. Select **Edit** next to the card you want to update.
1. Update the credit or debit card details.
1. Select **Save**.

## Use a different credit card for the Azure subscription

1. Sign in to the [Azure Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select the subscription that's linked to the card.
1. On the right side of the page, select **Manage payment methods**.
1. Click **Use Instead** next to the card that you want to use. This also updates any other subscriptions currently associated with this card.

## Remove a credit or debit card from the account

1. Sign in to the [Azure Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select the subscription that's linked to the card.
3. On the right side of the page, select **Manage payment methods**.
4. Click **Delete** for the credit card that you want to delete.

If your credit card is associated with other active Microsoft subscriptions, you can't remove it from your Azure account. Remove the credit card from all active subscriptions that you have with Microsoft and try again. -->

## <a name="change-payment-method-for-a-billing-profile"></a>Faturalandırma profili için ödeme yöntemini değiştir

Faturalandırma profili için ödeme yöntemini değiştirmek için Azure için kaydolan kişi olması gerekir.

Onay/aktarım kablo, öğrenme için varsayılan ödeme yöntemine geçiş yapmak istiyorsanız, nasıl [onay/kablo aktarımı için bir faturalandırma profili geçiş](billing-how-to-pay-by-invoice.md).

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Arama **maliyet Yönetimi + faturalandırma**.
1. Soldaki menüde tıklayarak **faturalandırma profilleri**.

    ![Faturalandırma profili menüde gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/billing-profile.png)

1. Faturalandırma profili seçin.
1. Soldaki menüde **ödeme yöntemlerini**.

   ![Menüde ödeme yöntemlerini gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/billing-profile-payment-methods.png)

1. Varsayılan ödeme yöntemini tıklatın **değişiklik**.

    ![Değiştir düğmesini gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/customer-led-switch-credit-card.png)

1. Mevcut bir kartı seçin veya yeni bir tane ekleyin.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
Aşağıdaki bölümlerde, kredi kartı veya banka kartı bilgilerinizi değiştirme hakkında sık sorulan soruları yanıtlayın.

### <a name="my-subscription-is-disabled-why-cant-i-remove-my-credit-card-now"></a>Aboneliğim devre dışı bırakıldı. Neden şimdi kredi kartımdan kaldırılamıyor?

Aboneliğiniz devre dışı veya iptal edildikten sonra biz aboneliğinizi kalıcı olarak silinmeden önce 90 gün bekleyin. Aboneliği yeniden etkinleştirmek istediğiniz durumunda bekletme süresi boyunca ödeme yönteminizi dosya çubuğunda saklarız. Bundan sonra abonelik kalıcı olarak silinir.

90 günlük saklama süresi dolmadan önce kredi kartınıza kaldırmanız gerekirse [aboneliğinizi yeniden etkinleştirmek](billing-subscription-become-disable.md). Yeniden etkinleştiremezsiniz, [Azure desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="why-do-i-keep-getting-your-login-session-has-expired-please-click-here-to-log-back-in"></a>Neden "oturumunuzun süresi doldu. almaya devam ediyorum Lütfen tekrar oturum için buraya tıklayın"?

Zaten oturum açmış olsa bile bu hata iletisini almaya devam ederseniz ve geri içinde özel tarama oturumla yeniden deneyin.

### <a name="how-do-i-use-a-different-card-for-each-subscription-i-have"></a>Farklı bir kartı sahibim her abonelik için nasıl kullanabilirim?

Aboneliklerinizi aynı kart kullanıyorsanız, ne yazık ki, bunları farklı kart kullanmak için birbirinden ayırmak mümkün değildir. Ancak, yeni bir abonelik için kaydolduğunuzda, bu abonelik için yeni bir ödeme yöntemi kullanmayı da tercih edebilirsiniz.

### <a name="how-do-i-make-payments"></a>Ödemeleri nasıl yapılsın mı?

Bir kredi kartı ödeme yönteminiz olarak ayarlarsanız, otomatik olarak kartınız sonra her bir fatura dönemi ücret alırız. Herhangi bir şey yapmanız gerekmez.

Size [faturayla ödeme](billing-how-to-pay-by-invoice.md), ödemenin konumuna faturanızı alt kısmında listelenen Gönder.

### <a name="how-do-i-change-the-tax-id"></a>Vergi kimlik numarası nasıl değiştirebilirim?

Profilinizde ekleyin veya vergi kimlik numarası güncelleştirmek için güncelleştirme [Azure hesap Merkezi](https://account.azure.com/Profile), ardından **vergi kayıt**. Vergi numarası, vergi muafiyeti hesaplamaları için kullanılır ve faturanızda görünür.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [Azure ayırmaları](billing-save-compute-costs-reservations.md) , para tasarrufu yapabileceğiniz, görmek için.
