---
title: Azure faturalama erişimi yönetme | Microsoft Docs
description: ''
services: ''
documentationcenter: ''
author: vikramdesai01
manager: amberb
editor: ''
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: cwatson
ms.openlocfilehash: 7a4e19ae5bf770949623f4cee7fa0d3033ccfa29
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582624"
---
# <a name="manage-access-to-billing-information-for-azure"></a>Azure için fatura bilgilerini erişimi yönetme

Çoğu abonelikler için takımınız üyeleri için fatura bilgilerine erişim verebilirsiniz **abonelikleri** Azure portalında. Bir Kurumsal Anlaşma (EA müşterisinin) ile bir Azure müşterisi olduğunuz ve kuruluş yöneticisiyseniz, izinler departman yöneticilerinin ve hesap sahipleri Enterprise Portal'da verebilirsiniz.

## <a name="give-access-to-billing"></a>Faturalandırma için erişim verin

Takım üyeleri için aşağıdaki kullanıcı rollerinden atayarak EA müşterileri hariç tüm Azure faturalandırma bilgileri erişimi verebilir:

- Hesap Yöneticisi
- Hizmet Yöneticisi
- Ortak yönetici
- Sahip
- Katılımcı
- Okuyucu
- Faturalama Okuyucusu

Rolleri atamak için bkz: [RBAC ve Azure portalını kullanarak erişimini yönetme](../role-based-access-control/role-assignments-portal.md).

Faturalama bilgileri bu rolleri erişimi [Azure portalında](https://portal.azure.com/). Bu rolleri atanmış olan kişiler de kullanabilir [faturalandırma API'lerini](billing-usage-rate-card-overview.md) program aracılığıyla faturaları ve kullanım ayrıntılarını almak için. Daha fazla bilgi için [Azure RBAC rolleri](../role-based-access-control/built-in-roles.md).

### <a name="opt-in"></a> Kullanıcıların faturaları indirmesine izin ver

Sonra Ata Hesap Yöneticisi, ekip üyelerinin uygun rollere etkinleştirmeniz gerekir değerlendirmek Azure portalında faturaları indirebilir. Aralık 2016'dan daha eski faturalar yalnızca Hesap Yöneticisi için kullanılabilir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Hesap Yöneticisi olarak aboneliğinizden seçin [abonelikler dikey penceresinden](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure portalında.

1. Seçin **faturalar** ardından **faturalar erişimi**.

    ![Faturalar erişimi devretmek nasıl ekran gösterir](./media/billing-manage-access/AA-optin.png)

1. Seçin **üzerinde** ve kaydedin.

    ![Ekran görüntüsü üzerinde faturaya erişim devretmek için alma gösterir](./media/billing-manage-access/AA-optinAllow.png)

Hesap Yöneticisi ayrıca faturaların e-posta ile gönderilmesini de sağlayabilir. Daha fazla bilgi için bkz. [Faturanızı e-posta ile alma](billing-download-azure-invoice-daily-usage-date.md).

## <a name="give-read-only-access-to-billing"></a>Faturalandırma için yalnızca okuma erişimi verin

Abonelik fatura bilgilerini ancak Azure hizmetleri oluşturmak veya yönetmek için özelliği değil yalnızca okuma erişimi gerektiren bir kişiye faturalandırma okuyucusu rolü atayın. Bu rol, mali ve maliyet yönetimini Azure abonelikleri için sorumlu olan bir kuruluştaki kullanıcılar için uygundur.

Bir EA müşterisiyseniz, bir hesap sahibi veya departman Yöneticisi takım üyeleri için faturalandırma okuyucusu rolü atayabilirsiniz. Ancak söz konusu fatura departman veya hesap için fatura bilgilerini Okuyucu için kuruluş yöneticisi etkinleştirmelisiniz **AO ücretleri görüntüle** veya **DA ücretleri görüntüle** Enterprise Portal'da ilkeleri.

Faturalandırma okuyucusu özellik Önizleme aşamasındadır ve henüz genel olmayan bulutlarda desteklemiyor.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Azure portalda [Abonelikler dikey penceresinden](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) aboneliğinizi seçin.

1. Seçin **erişim denetimi (IAM)**.
1. Seçin **rol atamaları** Bu abonelik için tüm rol atamalarını görüntülemek için.
1. Seçin **Ekle** > **rol ataması Ekle**.
1. İçinde **rol** aşağı açılan listesinde **faturalandırma okuyucusu**.
1. İçinde **seçin** metin eklemek istediğiniz kullanıcı için e-posta veya adı yazın.
1. **Kaydet**’i seçin.
1. Birkaç dakika sonra kullanıcıya abonelik kapsamında faturalandırma okuyucusu rolü atanır.
1. Faturalandırma okuyucusu, oturum açmak için bir bağlantı içeren bir e-posta alır.

    ![Azure portalında faturalama okuyucusu görebileceklerini gösteren ekran görüntüsü](./media/billing-manage-access/billing-reader-view.png)

## <a name="allow-department-administrator-or-account-owner-billing-access"></a>Bölüm Yöneticisi veya hesap sahibi fatura erişime izin ver

Kuruluş Yöneticisi, hesap sahipleri ve departman yöneticilerinin kullanım ayrıntıları ve Departmanlar ve yönettikleri hesaplarına ilişkili maliyetleri görüntülemek üzere izin verebilirsiniz.

1. Kuruluş yöneticisi olarak oturum açın [EA portal](https://ea.azure.com/).
1. Seçin **yönetme**.
1. Altında **kayıt**, değiştirme **DA ücretleri görüntüle** için **etkin** kullanımı ve maliyetleri görüntülemek üzere Bölüm Yöneticisi için.
1. Değişiklik **AO ücretleri görüntüle** için **etkin** kullanımı ve maliyetleri görüntülemek için hesap sahibi.


Daha fazla bilgi için [azure'da yönetim rolleri Azure Kurumsal anlaşmasına anlamak](billing-understand-ea-roles.md).

## <a name="only-account-admins-can-access-account-center"></a>Hesap Yöneticileri hesap Merkezi'ne erişebilirsiniz.

Hesap Yöneticisi, aboneliğin yasal sahibi değil. Varsayılan olarak, kaydolduğunuz veya Azure aboneliği satın aldığınız Hesap Yöneticisi sürece kişidir [aboneliğin sahipliğini aktarılan](billing-subscription-transfer.md) başka birine. Hesap Yöneticisi abonelikleri oluşturabilir, aboneliklerinizi iptal etmeniz, bir abonelik için fatura adresini değiştirin ve abonelikten için erişim ilkelerini yönetme [hesap Merkezi](https://account.azure.com/Subscriptions).

## <a name="next-steps"></a>Sonraki adımlar

- Sahibi veya katkıda bulunan gibi diğer rolleri yalnızca fatura bilgilerini, ancak Azure hizmetlerine erişebilirsiniz. Bu rolleri yönetmek için bkz: [RBAC ve Azure portalını kullanarak erişimini yönetme](../role-based-access-control/role-assignments-portal.md).
- Rolleri hakkında daha fazla bilgi için bkz. [Azure kaynakları için yerleşik roller](../role-based-access-control/built-in-roles.md).

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
