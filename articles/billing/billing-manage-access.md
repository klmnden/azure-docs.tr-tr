---
title: Azure faturalandırma rollerini kullanarak erişimi yönetme | Microsoft Docs
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
ms.author: cwatson
ms.openlocfilehash: 623856f05eed44eca3752d56f047f9bb282bdc8e
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392100"
---
# <a name="manage-access-to-billing-information-for-azure-using-role-based-access-control"></a>Rol tabanlı erişim denetimi kullanarak Azure için fatura bilgilerini erişimi yönetme

Aşağıdaki kullanıcı rollerinden aboneliğinize atayarak takım üyeleriniz için Azure fatura bilgileri için erişim verebilirsiniz: Hesap Yöneticisi, Hizmet Yöneticisi, ortak yönetici, sahibi, katkıda bulunan, okuyucu ve faturalandırma okuyucusu. Fatura bilgilerine erişimleri [Azure portalında](https://portal.azure.com/), ve kullanabilecekleri [faturalandırma API'lerini](billing-usage-rate-card-overview.md) programlı olarak (bir kez kabul-gelen) faturaları ve kullanım ayrıntılarını almak için. Hakkında daha fazla bilgi için kimin rolleri ve verin hangi rollerin neler, bkz: [Azure RBAC rolleri](../role-based-access-control/built-in-roles.md).

## <a name="opt-in"></a> Faturalar erişmek ek kullanıcılara izin verilmesi

Hesap Yöneticisi kullanarak etmeniz gerekir [Azure portalında](https://portal.azure.com/) faturaları API aracılığıyla ve diğer kullanıcılar için erişime.

1. Hesap Yöneticisi olarak aboneliğinizden seçin [abonelikler dikey penceresinden](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure portalında.

1. Seçin **faturalar** ardından **faturalar erişimi**.

    ![Faturalar erişimi devretmek nasıl ekran gösterir](./media/billing-manage-access/AA-optin.png)

1. Kapatma **üzerinde** kullanıcıların abonelikte izin vermek için değişiklikleri kaydederek ve ardından erişim kapsamı karşıdan yükleme faturası için roller.

    ![Ekran görüntüsü üzerinde faturaya erişim devretmek için alma gösterir](./media/billing-manage-access/AA-optinAllow.png)

Seçim Azure portalında PDF fatura indirmek için abonelikte Hizmet Yöneticisi, ortak yönetici, sahibi, katkıda bulunan, okuyucu ve faturalandırma okuyucusu sağlar. Ancak, aralık 2016'dan daha eski faturalar şimdilik yalnızca Hesap Yöneticisi için kullanılabilir.

Hesap Yöneticisi, e-posta ile gönderilen faturalar için de yapılandırabilirsiniz. Daha fazla bilgi için bkz. [faturanızı e-posta ile alın](billing-download-azure-invoice-daily-usage-date.md).

## <a name="adding-users-to-the-billing-reader-role"></a>Faturalandırma okuyucusu rolüne kullanıcı ekleme

Faturalandırma okuyucusu rolü, Azure portalında abonelik fatura bilgilerini yalnızca okuma erişimi ve sanal makineleri ve depolama hesapları gibi hizmetlere erişim yok sahiptir. Abonelik fatura bilgilerini ancak değil Azure hizmetlerini yönetme özelliği erişmesi birine faturalandırma okuyucusu rolü atayın. Bu rolün yalnızca bir Azure aboneliğine mali ve maliyet Yönetimi gerçekleştiren bir kuruluştaki kullanıcı için uygundur.

1. Aboneliğinizden seçin [abonelikler dikey penceresinden](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure portalında.

1. Seçin **erişim denetimi (IAM)** ve ardından **Ekle**.

    ![Ekran görüntüsü IAM abonelik dikey penceresinde gösterir.](./media/billing-manage-access/select-iam.PNG)

1. Seçin **faturalandırma okuyucusu** içinde **bir rol seçin** sayfası.

    ![Ekran görüntüsü, faturalandırma okuyucusu açılan görünümünde gösterir.](./media/billing-manage-access/select-roles.PNG)

1. Davet edin ve ardından istediğiniz kullanıcının e-posta türü **Tamam** davet gönderilecek.

    ![Davet e-posta girmesini gösteren ekran görüntüsü](./media/billing-manage-access/add-user.PNG)

1. Faturalama okuyucusu olarak oturum açmak için davet e-postadaki yönergeleri izleyin.

    ![Azure portalında faturalama okuyucusu görebileceklerini gösteren ekran görüntüsü](./media/billing-manage-access/billing-reader-view.png)

> [!NOTE]
> Faturalandırma okuyucusu özellik Önizleme aşamasındadır ve enterprise (EA) abonelikleri veya genel olmayan bulutlarda henüz desteklemiyor.

## <a name="adding-users-to-other-roles"></a>Diğer rollere kullanıcı ekleme

Sahibi veya katkıda bulunan gibi diğer rolleri yalnızca fatura bilgilerini, ancak Azure hizmetlerine erişebilirsiniz. Bu rolleri yönetmek için bkz: [RBAC ve Azure portalını kullanarak erişimini yönetme](../role-based-access-control/role-assignments-portal.md).

## <a name="who-can-access-the-account-centerhttpsaccountwindowsazurecom"></a>Kimin erişebileceği [hesap Merkezi](https://account.windowsazure.com)?

Yalnızca hesap yöneticisi hesap Merkezi'nde oturum açabilir. Hesap Yöneticisi, aboneliğin yasal sahibi değil. Varsayılan olarak, kaydolduğunuz veya Azure aboneliği satın aldığınız Hesap Yöneticisi sürece kişidir [aboneliğin sahipliğini aktarılan](billing-subscription-transfer.md) başka birine. Hesap Yöneticisi abonelikleri oluşturabilir, aboneliklerinizi iptal etmeniz, bir abonelik için fatura adresini değiştirmek ve abonelik için erişim ilkelerini yönetme.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala başka sorularınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
