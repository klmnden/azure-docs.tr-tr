---
title: Azure faturalama rollerini kullanarak erişimi yönetme | Microsoft Docs
description: ''
services: ''
documentationcenter: ''
author: vikramdesai01
manager: vikdesai
editor: ''
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/22/2017
ms.author: vikdesai
ms.openlocfilehash: 7329b06171bd538cc6e9aa8172380a2d4dd47dae
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="manage-access-to-billing-information-for-azure-using-role-based-access-control"></a>Rol tabanlı erişim denetimini kullanarak Azure için faturalama bilgilerini erişimi yönetme

Aşağıdaki kullanıcı rollerinden birini aboneliğinize atayarak ekibinizin üyeleri için Azure faturalama bilgilerini erişim verebilirsiniz: Hesap Yöneticisi, Hizmet Yöneticisi, ortak yönetici, sahibi, katkıda bulunan, okuyucu ve fatura okuyucu. Faturalama bilgileri erişime sahip [Azure portal](https://portal.azure.com/), ve kullanabileceklerini [fatura API'leri](billing-usage-rate-card-overview.md) programlı olarak (bir kez seçti-gelen) faturaları ve kullanım ayrıntıları almak için. Hakkında daha fazla bilgi için kimin verin rollerini ve hangi rollerin ne, bkz: [Azure RBAC rollerinde](../role-based-access-control/built-in-roles.md).

## <a name="opt-in"></a> Ek kullanıcıların faturaları erişmesine izin verme

Hesap Yöneticisi kullanarak opt gerekir [Azure portal](https://portal.azure.com/) diğer kullanıcılar için ve API aracılığıyla faturalar erişmesine izin vermek.

1. Hesap Yöneticisi olarak aboneliğinizden seçin [abonelikleri dikey](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure portalında.

1. Seçin **faturalar** ve ardından **faturalar erişimi**.

    ![Ekran görüntüsü, faturalar erişimin nasıl gösterir](./media/billing-manage-access/AA-optin.png)

1. Kapatma **üzerinde** kullanıcıların abonelikte izin vermek için değişiklikleri kaydederek ve ardından erişim kapsamına fatura indirmek için rolleri.

    ![Ekran görüntüsü, açık-fatura erişimi devretmek için kapalı gösterir](./media/billing-manage-access/AA-optinAllow.png)

Seçim PDF faturalar Azure portalında indirmek için abonelikte Hizmet Yöneticisi, ortak yönetici, sahibi, katkıda bulunan, okuyucu ve faturalama okuyucu sağlar. Ancak, faturalar aralık 2016'den daha eski şimdilik yalnızca Hesap Yöneticisi için kullanılabilir.

Hesap Yöneticisi, e-posta ile gönderilen faturalar sağlamak için de yapılandırabilirsiniz. Daha fazla bilgi için bkz: [faturanızı e-posta ile almak](billing-download-azure-invoice-daily-usage-date.md).

## <a name="adding-users-to-the-billing-reader-role"></a>Faturalama okuyucu rolüne kullanıcı ekleme

Faturalama okuyucu rolüne sanal makineleri ve depolama hesapları gibi hizmetleri için erişim yok ve abonelik faturalama bilgileri Azure portalında salt okunur erişimi vardır. Abonelik faturalama bilgileri ancak değil Azure Hizmetleri yönetme yeteneği erişmesi birine faturalama okuyucu rolüne atayın. Bu rol yalnızca Azure aboneliklerini finansal ve maliyet Yönetimi gerçekleştiren bir kuruluştaki kullanıcılar için uygun değildir.

1. Aboneliğinizden seçin [abonelikleri dikey](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure portalında.

1. Seçin **erişim denetimi (IAM)** ve ardından **Ekle**.

    ![Ekran görüntüsü IAM abonelik dikey penceresinde gösterir.](./media/billing-manage-access/select-iam.PNG)

1. Seçin **faturalama okuyucu** içinde **bir rol seçin** sayfası.

    ![Ekran görüntüsü, faturalama okuyucu açılan görünümünde gösterir.](./media/billing-manage-access/select-roles.PNG)

1. Davet etmek ve ardından istediğiniz kullanıcı için e-posta türü **Tamam** daveti göndermek için.

    ![Kişileri davet etmek için e-posta girmek için gösteren ekran görüntüsü](./media/billing-manage-access/add-user.PNG)

1. Faturalama okuyucu olarak oturum açmak için davet e-postadaki yönergeleri izleyin.

    ![Azure portalında faturalama okuyucu görebileceklerini gösteren ekran görüntüsü](./media/billing-manage-access/billing-reader-view.png)

> [!NOTE]
> Faturalama okuyucu özellik Önizleme aşamasındadır ve kurumsal (EA) abonelikleri veya genel olmayan bulut henüz desteklemiyor.

## <a name="adding-users-to-other-roles"></a>Diğer roller için kullanıcı ekleme

Sahibi veya katkıda, gibi diğer rolleri yalnızca fatura bilgilerini, ancak Azure hizmetlerine erişebilir. Bu rolleri yönetmek için bkz: [abonelik ya da hizmetleri yönetmek ekleme veya değiştirme Azure yönetici rolleri](billing-add-change-azure-subscription-administrator.md).

## <a name="who-can-access-the-account-centerhttpsaccountwindowsazurecom"></a>Kimin erişebileceği [hesap Merkezi'nde](https://account.windowsazure.com)?

Yalnızca hesap yöneticisi hesap Merkezi'nde oturum açabilir. Hesap Yöneticisi abonelik yasal sahibi. Varsayılan olarak, kaydolup veya Azure aboneliği satın Hesap Yöneticisi sürece kişidir [abonelik sahipliği aktarılan](billing-subscription-transfer.md) başka birine. Hesap Yöneticisi abonelikleri oluşturma, aboneliklerinizi iptal edin, bir abonelik için fatura adresini değiştirmek ve abonelik için erişim ilkelerini yönetme.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala daha fazla, sorularınız varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
