---
title: Azure için kredi kartınızdan Değiştir | Microsoft Docs
description: Açıklayan nasıl bir Azure aboneliği ödemesi için kullanılan kredi kartını değiştirme
services: ''
documentationcenter: ''
author: genlin
manager: jlian
editor: ''
tags: billing
ms.assetid: 15252ced-1841-4a66-ae79-2e58af1d3370
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: cwatson
ms.openlocfilehash: 68f987daff5bc0ef81c248f6f5e75aaf1318b025
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47395500"
---
# <a name="add-update-or-remove-a-credit-or-debit-card-for-azure"></a>Ekleme, güncelleştirme veya Azure için bir kredi kartı veya banka kartı kaldırma

Hesap Merkezi'nde yeni bir kredi kartı eklemeniz, kullanılan mevcut kredi kartını güncelleştirmek veya kullanmadığınız bir kredi kartı Sil. Siz [Hesap Yöneticisi](billing-subscription-transfer.md#whoisaa) bu değişiklikleri yapmak için.

**Fatura ile ödeme geçmek istiyorsunuz?** Bkz: [Azure abonelikleri için fatura ile ödeme](billing-how-to-pay-by-invoice.md).
 
<a id="addcard"></a>

## <a name="add-a-new-credit-or-debit-card"></a>Yeni bir kredi kartı veya banka kartı Ekle

1. Oturum [hesap Merkezi](https://account.windowsazure.com/Subscriptions) hesap yöneticisi olarak.
1. Bir abonelik seçin.
1. Sayfanın sağ tarafında **Ödeme yöntemlerini yönet**’i seçin.

    ![Yönet ödeme yöntemleri seçeneği belirlenmiş gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/changesub_new.png)
1. Seçin "+" bir kart ekleyin.

    ![Ödeme yöntemini yanındaki Düzenle seçeneğini gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/editcard_new.png)
1. Kredi girin veya banka kartı ayrıntıları.
1. **Kaydet**’i seçin. 

Kredi kartı ekledikten sonra hata alırsanız bkz [Azure kayıt sırasında kredi kartı reddedildi](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="update-existing-credit-or-debit-card"></a>Mevcut kredi veya banka kartı güncelleştir

Kredi kartınız yenilenmiş ve sayı aynı kalır, sona erme tarihi gibi mevcut kredi kartı bilgileri güncelleştirin. Kart kaybolduğu için kredi kartı numarası değişikliklerinizi çalındıysa veya süresi dolduğunda, adımları izlerseniz, [bir ödeme yöntemi olarak kredi kartı eklemeniz](#addcard) bölümü. Kart doğrulama kodu güncelleştirmeniz gerekmez.

1. Oturum [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions) hesap yöneticisi olarak.
1. Kartına bağlı aboneliği seçin.
1. Seçin **ödeme yöntemlerini Yönet**.
1. Seçin **Düzenle** güncelleştirmek istediğiniz kartın yanında.
1. Kredi kartı veya banka kartı ayrıntıları güncelleştirin.
1. **Kaydet**’i seçin.

## <a name="use-a-different-credit-card-for-the-azure-subscription"></a>Azure aboneliği için farklı bir kredi kartı kullanın

1. Oturum [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions) hesap yöneticisi olarak.
1. Kartına bağlı aboneliği seçin.
1. Sayfanın sağ tarafında **Ödeme yöntemlerini yönet**’i seçin.
1. Tıklayın **kullanmak yerine** kullanmak istediğiniz kartın yanında. Bu şu anda bu kart ile ilişkili diğer abonelikleri de güncelleştirir. 

## <a name="remove-a-credit-or-debit-card-from-the-account"></a>Bir kredi kartı veya banka kartı hesaptan Kaldır

1. Oturum [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions) hesap yöneticisi olarak.
1. Kartına bağlı aboneliği seçin.
3. Sayfanın sağ tarafında **Ödeme yöntemlerini yönet**’i seçin.
4. Tıklayın **Sil** silmek istediğiniz kredi kartı için.

Kredi kartınız etkin diğer Microsoft aboneliği ile ilişkili ise, Azure hesabınızdan kaldırılamaz. Kredi kartı ile Microsoft ve yeniden deneyin tüm etkin aboneliklerden Kaldır.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="my-subscription-is-disabled-why-cant-i-remove-my-credit-card-now"></a>Aboneliğim devre dışı bırakıldı. Neden şimdi kredi kartımdan kaldırılamıyor?

Aboneliğiniz devre dışı veya iptal edildikten sonra biz aboneliğinizi kalıcı olarak silinmeden önce 90 gün bekleyin. Aboneliği yeniden etkinleştirmek istediğiniz durumunda bekletme süresi boyunca ödeme yönteminizi dosya çubuğunda saklarız. Bundan sonra abonelik tamamen silinir.

90 günlük saklama süresi dolmadan önce kredi kartı veya banka kartınız kaldırmanız gerekirse [aboneliğinizi yeniden etkinleştirmek](billing-subscription-become-disable.md). Yeniden etkinleştiremezsiniz, [Azure desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="why-do-i-keep-getting-your-login-session-has-expired-please-click-here-to-log-back-in"></a>Neden "oturumunuzun süresi doldu. almaya devam ediyorum Lütfen tekrar oturum için buraya tıklayın"?

Zaten oturum kapalı olsa bile bu hata iletisini almaya devam ederseniz ve geri içinde özel tarama oturumla yeniden deneyin.

### <a name="how-do-i-use-a-different-card-for-each-subscription-i-have"></a>Farklı bir kartı sahibim her abonelik için nasıl kullanabilirim?

Aboneliklerinizi aynı kart kullanıyorsanız, ne yazık ki, bunları farklı kart kullanmak için birbirinden ayırmak mümkün değildir. Ancak, yeni bir abonelik için kaydolduğunuzda, bu abonelik için yeni bir ödeme yöntemi kullanmayı da tercih edebilirsiniz.

### <a name="how-do-i-make-payments"></a>Ödemeleri nasıl yapılsın mı?

Bir kredi kartı veya banka kartı ödeme yönteminiz olarak ayarlarsanız, otomatik olarak kartınız sonra her bir fatura dönemi ücret alırız. Herhangi bir şey yapmanız gerekmez.

Size [faturayla ödeme](billing-how-to-pay-by-invoice.md), ödemenin konumuna faturanızı alt kısmında listelenen Gönder.

### <a name="how-do-i-make-a-one-time-payment"></a>Bir kerelik ödeme nasıl yapılsın mı?

Ne yazık ki Azure kredi ve banka kartları için şu anda tek seferlik ödemeleri desteklememektedir. 

### <a name="how-do-i-change-the-tax-id"></a>Vergi kimlik numarası nasıl değiştirebilirim?

Ekleme veya güncelleştirme vergi kimliği için şurayı ziyaret edin [ **profili** Azure hesap Merkezi'nde](https://account.azure.com/Profile), ardından **vergi kayıt**. Vergi numarası, vergi muafiyeti hesaplamaları için kullanılır ve faturanızda görünür.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
