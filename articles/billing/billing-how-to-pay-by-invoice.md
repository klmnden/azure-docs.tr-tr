---
title: Azure abonelikleri için fatura ile ödeme
description: Azure abonelikleri için fatura ile ödemeyi açıklar.
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
ms.openlocfilehash: 9ca726ef737ce4750018d2461bc4bcd6c7ebb5f5
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491196"
---
# <a name="pay-for-your-azure-subscription-by-invoice"></a>Azure aboneliğiniz için fatura ile ödeme

Fatura ile ödemeyi geçiş yapıyorsanız, bu onay/havale tarafından Fatura tarihinden sonraki 30 gün içinde faturanızın ödeme anlamına gelir. Azure aboneliğinizi faturayla ödeme yapmak için uygun olmak için bir Azure destek isteği gönderin. İsteğiniz onaylandıktan sonra fatura ödeme (onay/havale) içinde geçebilirsiniz [Azure portalında](https://portal.azure.com).

> [!IMPORTANT]
> * Fatura ödeme (onay/havale) yalnızca iş hesapları için kullanılabilir.
> * Fatura ödeme geçmeden önce tüm ödenmemiş ödeme yapmanız gerekir.

## <a name="request-to-pay-by-invoice"></a>Faturayla ödeme talep

1. [Azure Portal](https://portal.azure.com/) oturum açın. Seçin **Yardım + Destek** > **yeni destek isteği**.

    ![Yardım ve destek bağlantısı](./media/billing-how-to-pay-by-invoice/help-and-support.png)

2. Seçin **faturalama** olarak **sorun türü**. *Sorun türü* destek isteği kategorisi. Faturayla ödeme yapmak, bir destek planı seçin ve ardından istediğiniz aboneliği seçin **sonraki**.

3. **Sorun Türü** kutusunda **Fatura ile ödeme**'yi seçin. *Sorun türü* destek isteği kategori olan.

4. Aşağıdaki bilgileri girin **ayrıntıları** kutusuna ve ardından **sonraki**.

         New or existing customer:
         If existing, current payment method:
         Order ID (requesting for invoice option):
         Account Admins Live ID (or Org ID) (should be company domain):
         Commerce Account ID:
         Company Name (as registered under VAT or Government Website):
         Company Address (as registered under VAT or Government Website):
         Company Website:
         Country:
         TAX ID/ VAT ID:
         Company Established on (Year):
         Any prior business with Microsoft:
         Contact Name:
         Contact Phone:
         Contact Email:
         Justification on why you prefer Invoice option over credit card:

        For cores increase, provide the following additional information:

         (Old quota) Existing Cores:
         (New quota) Requested cores:
         Specific region & series of Subscription:

    - **Şirket adı** ve **şirket adresi** Azure hesabı için sağlanan bilgilerin eşleşmesi gerekir. Görüntülemek veya bilgileri güncelleştirmek için bkz: [Azure hesap profili bilgilerinizi değiştirmek](billing-how-to-change-azure-account-profile.md).
    - Kredi sınırınıza onaylanabilir önce Azure portalında faturalama, iletişim bilgilerinizi eklemeniz gerekir. Şirketin Borç hesapları veya Finans departmanı için kişi ayrıntılarını ilişkili olmalıdır. Fatura bilgilerini güncelleştirmek için Git [Azure hesap Merkezi](https://account.azure.com/Profile).

5. İletişim bilgilerinizi ve tercih edilen iletişim yöntemini seçip **Oluştur**'a tıklayın.

Size gereken kredi miktarı nedeniyle bir kredi kontrolü çalıştırmanız gerekiyorsa, kredi application should check göndereceğiz.

## <a name="switch-to-invoice-pay-checkwire-transfer"></a>Ödeme (onay/havale) faturaya geç

Faturayla ödeme onaylandıktan sonra Azure portalında ödeme (onay/havale) fatura geçiş yapabilirsiniz.

Bir Microsoft Online Services programı hesabınız varsa, onay/aktarım kablo için Azure aboneliğinizi geçiş yapabilirsiniz. Microsoft Müşteri sözleşmesi varsa, onay/aktarım kablo için fatura profilinizde geçiş yapabilirsiniz. [Hesap türünüzü kontrol öğrenin](#check-access-to-a-microsoft-customer-agreement).

### <a name="switch-azure-subscription-to-checkwire-transfer"></a>Onay/aktarım kablo için Azure aboneliğine geçiş

Fatura ödeme (onay/havale) için Azure aboneliğinizi geçiş yapmak için aşağıdaki adımları izleyin. **Fatura ödeme (onay/havale) geçtikten sonra kredi kartına geçiş yapamazsınız**.

1. Oturum [Azure portalında](https://portal.azure.com) hesap yöneticisi olarak.
1. Arama **maliyet Yönetimi + faturalandırma**.

    ![Arama gösteren ekran görüntüsü](./media/billing-how-to-pay-by-invoice/search.png)

1. Fatura ödeme geçiş yapmak istediğiniz aboneliği seçin.
1. **Ödeme yöntemleri**'ni seçin.
1. Komut çubuğunda **faturayla ödeme** düğmesi.

    ![Ödeme tarafından Fatura düğmesini gösteren ekran görüntüsü](./media/billing-how-to-pay-by-invoice/pay-by-invoice.png)

### <a name="switch-billing-profile-to-checkwire-transfer"></a>Geçiş için onay/aktarım kablo faturalandırma profili

Onay/kablo aktarımı için bir faturalandırma profili geçmek için aşağıdaki adımları izleyin. Azure için kaydolan kişi varsayılan ödeme yöntemini'fatura profilinin değiştirebileceğinizi unutmayın.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Arama **maliyet Yönetimi + faturalandırma**.
1. Soldaki menüde tıklayarak **faturalandırma profilleri**.

    ![Faturalandırma profili menüde gösteren ekran görüntüsü](./media/billing-how-to-pay-by-invoice/billing-profile.png)

1. Faturalandırma profili seçin.
1. Soldaki menüde **ödeme yöntemlerini**.

   ![Menüde ödeme yöntemlerini gösteren ekran görüntüsü](./media/billing-how-to-pay-by-invoice/billing-profile-payment-methods.png)

1. Onay/havale ile ödeme yapmak uygun olan bildiren mavi başlığa tıklayın.

    ![Onay/kablo geçmek için mavi renkli bir başlık gösteren ekran görüntüsü](./media/billing-how-to-pay-by-invoice/customer-led-switch-to-invoice.png)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- Gerekirse, fatura kişi bilgilerinizi güncelleştirin [Azure hesap Merkezi](https://account.azure.com/Profile).
