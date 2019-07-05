---
title: Azure faturalama erişimi yönetme | Microsoft Docs
description: Azure, faturalandırma bilgilerinizdeki takım üyeleri için erişim verme konusunda bilgi edinin.
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
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: 6726c876d0895f9488aa2ae5c225a6b2ac19e69f
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491154"
---
# <a name="manage-access-to-billing-information-for-azure"></a>Azure için fatura bilgilerini erişimi yönetme

Azure portalındaki hesabınız için fatura bilgilerine erişim diğerleri sağlayabilirsiniz. Fatura rolleri türünü ve faturalandırma bilgileri erişim sağlamak için yönergeleri fatura hesabınıza türüne göre değişiklik gösterir. Fatura hesabınıza türünü belirlemek için bkz: [fatura hesabınıza türünü kontrol](#check-the-type-of-your-billing-account).

Makale, Microsoft Online Services programı hesaplar kullanan müşteriler için geçerlidir. Bir Kurumsal Anlaşma'ile (EA) bir Azure müşterisi olduğunuz ve kuruluş yöneticisiyseniz, izinleri hesap sahipleri ve departman yöneticilerinin Enterprise Portal'da verebilirsiniz. Daha fazla bilgi için [azure'da yönetim rolleri Azure Kurumsal anlaşmasına anlamak](billing-understand-ea-roles.md). Bir Microsoft Müşteri sözleşme müşterisi iseniz, bkz: [Azure yönetici rollerini anlama Microsoft Müşteri sözleşmesi](billing-understand-mca-roles.md). 

## <a name="account-administrators-for-microsoft-online-service-program-accounts"></a>Microsoft Online Services programı hesaplar için hesap yöneticileri

Bir Hesap Yöneticisi, bir Microsoft çevrimiçi hizmet programı Faturalama hesabı için yalnızca sahibi değil. Rol, Azure'a kaydolduysanız bu durumda bir kişiye atanır. Hesap Yöneticileri, abonelikleri oluşturabilir, faturaları görüntülemek veya değiştirmek için bir abonelik faturalama gibi çeşitli faturalandırma görevlerini gerçekleştirmek için yetkiniz olduğundan.

## <a name="give-others-access-to-view-billing-information"></a>Diğer faturalandırma bilgileri görüntülemek için erişim verin

Hesap Yöneticisi hesabında bir abonelikte aşağıdaki rollerden biri atayarak diğer Azure fatura bilgilerine erişim izni verebilirsiniz.

- Hizmet Yöneticisi
- Ortak yönetici
- Sahip
- Katılımcı
- Okuyucu
- Faturalama okuyucusu

Faturalama bilgileri bu rolleri erişimi [Azure portalında](https://portal.azure.com/). Bu rolleri atanmış olan kişiler de kullanabilir [faturalandırma API'lerini](billing-usage-rate-card-overview.md) program aracılığıyla faturaları ve kullanım ayrıntılarını almak için.

Rolleri atamak için bkz: [RBAC ve Azure portalını kullanarak erişimini yönetme](../role-based-access-control/role-assignments-portal.md).

** Bir EA müşterisinin kullanıyorsanız, bir hesap sahibi ekip diğer kullanıcılara yukarıdaki rol atayabilirsiniz. Ancak bu kullanıcılar fatura bilgilerini görüntülemek kuruluş yöneticisi saniye başına AO görünümü Enterprise Portal'da etkinleştirmeniz gerekir.


### <a name="opt-in"></a> Kullanıcıların faturaları indirmesine izin ver

Diğer kullanıcılara atanmış bir hesap yöneticisi uygun roller sonra Azure portalında faturaları indirmesine izin erişimini etkinleştirmeniz gerekir. Aralık 2016'dan daha eski faturalar yalnızca Hesap Yöneticisi için kullanılabilir.

1. Oturum [Azure portalında](https://portal.azure.com/), olarak bir Hesap Yöneticisi

1. Arama **maliyet Yönetimi + faturalandırma**.

    ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-manage-access/billing-search-cost-management-billing.png)
 
1. Seçin **abonelikleri** sol bölmesinden. Erişiminizi bağlı olarak, fatura bir kapsam seçin ve ardından gerekebilir **abonelikleri**.
 
    ![Abonelik seçmeyi gösteren ekran görüntüsü](./media/billing-manage-access/billing-select-subscriptions.png)

1. Seçin **faturalar** ardından **faturaya erişim**.

    ![Faturalar erişimi devretmek nasıl ekran gösterir](./media/billing-manage-access/AA-optin.png)

1. Seçin **üzerinde** ve kaydedin.

    ![Ekran görüntüsü üzerinde faturaya erişim devretmek için alma gösterir](./media/billing-manage-access/AA-optinAllow.png)

Hesap Yöneticisi ayrıca faturaların e-posta ile gönderilmesini de sağlayabilir. Daha fazla bilgi için bkz. [Faturanızı e-posta ile alma](billing-download-azure-invoice-daily-usage-date.md).

## <a name="give-read-only-access-to-billing"></a>Faturalandırma için yalnızca okuma erişimi verin

Abonelik fatura bilgilerini ancak Azure hizmetleri oluşturmak veya yönetmek için özelliği değil yalnızca okuma erişimi gerektiren bir kişiye faturalandırma okuyucusu rolü atayın. Bu rol, mali ve maliyet yönetimini Azure abonelikleri için sorumlu olan bir kuruluştaki kullanıcılar için uygundur.

Faturalandırma okuyucusu özellik Önizleme aşamasındadır ve henüz genel olmayan bulutlarda desteklemiyor.

1. Oturum [Azure portalında](https://portal.azure.com/), olarak bir Hesap Yöneticisi

1. Arama **maliyet Yönetimi + faturalandırma**.

    ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-manage-access/billing-search-cost-management-billing.png)

1. Seçin **abonelikleri** sol bölmesinden. Erişiminizi bağlı olarak, fatura bir kapsam seçin ve ardından gerekebilir **abonelikleri**.
 
    ![Abonelik seçmeyi gösteren ekran görüntüsü](./media/billing-manage-access/billing-select-subscriptions.png)

1. Seçin **erişim denetimi (IAM)** .
1. Seçin **Ekle** sayfanın üst.

    ![Rol ataması tıklayarak gösteren ekran görüntüsü Ekle](./media/billing-manage-access/billing-click-add-role-assignment.png)

1. İçinde **rol** aşağı açılan listesinde **faturalandırma okuyucusu**.
1. İçinde **seçin** metin eklemek istediğiniz kullanıcı için e-posta veya adı yazın.
1. Kullanıcıyı seçin.
1. **Kaydet**’i seçin.
    ![Rol ataması tıklayarak gösteren ekran görüntüsü Ekle](./media/billing-manage-access/billing-save-role-assignment.png)

1. Birkaç dakika sonra kullanıcı, aboneliğin faturalandırma okuyucusu rolü atanır.

** Bir EA müşterisinin kullanıyorsanız, bir hesap sahibi veya departman Yöneticisi takım üyeleri için faturalandırma okuyucusu rolü atayabilirsiniz. Ancak söz konusu fatura departman veya hesap için fatura bilgilerini Okuyucu için kuruluş yöneticisi etkinleştirmelisiniz **AO ücretleri görüntüle** veya **DA ücretleri görüntüle** Enterprise Portal'da ilkeleri.

## <a name="check-the-type-of-your-billing-account"></a>Fatura hesabınıza türünü kontrol edin
[!INCLUDE [billing-check-account-type](../../includes/billing-check-account-type.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Sahibi veya katkıda bulunan gibi diğer rolleri yalnızca fatura bilgilerini, ancak Azure hizmetlerine erişebilirsiniz. Bu rolleri yönetmek için bkz: [RBAC ve Azure portalını kullanarak erişimini yönetme](../role-based-access-control/role-assignments-portal.md).
- Rolleri hakkında daha fazla bilgi için bkz. [Azure kaynakları için yerleşik roller](../role-based-access-control/built-in-roles.md).

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).
