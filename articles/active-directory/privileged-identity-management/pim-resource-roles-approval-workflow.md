---
title: Privileged Identity Management, Azure kaynak rolleri için onay iş akışı | Microsoft Docs
description: Azure kaynakları için onay iş akışı işlemi açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 42b0a8f94ff09b308a579b962bc99c4796c73c2e
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443044"
---
# <a name="approval-workflow-for-azure-resource-roles-in-privileged-identity-management"></a>Privileged Identity Management, Azure kaynak rolleri için onay iş akışı

Onay iş akışı ile Privileged Identity Management (PIM), Azure kaynak rolleri için Yöneticiler daha fazla koruyabilir veya kritik kaynaklara erişimi kısıtlayın. Diğer bir deyişle, yöneticileri, rol atamalarını etkinleştirmek için onay isteyebilir. 

Azure kaynak rolleri için benzersiz bir kaynak hiyerarşisinin kavramdır. Bu hiyerarşi üst kapsayıcı içindeki tüm alt kaynaklar için rol atamalarını aşağı üst kaynak nesneden devralınmasını sağlar. 

Örneğin: Bob, bir kaynak yöneticisi, uygun bir üye olarak Esra'nın Contoso abonelik sahibi rolüne atamak için PIM kullanır. Bu atama ile Esra'nın Contoso Abonelikteki tüm kaynak grubu kapsayıcıları bir uygun sahibidir. Gamze ayrıca bir uygun tüm kaynakları (sanal makineler gibi) abonelik her bir kaynak grubu içinde sahibidir. 

Contoso abonelikte üç kaynak grubunuz yok varsayalım: Fabrikam Test, Fabrikam geliştirme ve Fabrikam ürün. Bu kaynak gruplarının her biri tek bir sanal makine içeriyor.

PIM ayarları, her bir kaynağın rolünü için yapılandırılır. Atamaları aksine bu ayarları yok devralınır ve kesinlikle Kaynak rolü için geçerlidir. [Uygun atamalar ve Kaynak görünürlüğü hakkında daha fazla bilgiyi](pim-resource-roles-eligible-visibility.md).

Örneğiyle devam etmesini: Bob PIM Contoso abonelik isteği onay sahibi roldeki tüm üyeleri etkinleştirilmesini istemek için kullanır. Fabrikam Prod kaynak grubundaki kaynakları korumak için Bob Ayrıca bu kaynağın sahip rolünün üyeleri için onay gerektirir. Fabrikam Test ve geliştirme Fabrikam sahip roller, etkinleştirme için onay gerekmez.

Alice Contoso abonelik için sahip rolünü etkinleştirme isteğinde bulunduğunda, bir onaylayan onaylayabilir veya o roldeki etkinleştirilmeden önce her bir isteği reddetmek gerekir. Alice karar verirse [her etkinleştirme kapsam](pim-resource-roles-activate-your-roles.md#apply-just-enough-administration-practices) Fabrikam Prod kaynak grubunu, bir onaylayan gerekir onaylayın veya çok bu isteği reddedin. Ancak, Esra'nın Fabrikam Test veya geliştirme Fabrikam biri veya her ikisi için her etkinleştirme kapsam karar verirse, onayı gerekli değildir.

Onay iş akışı, bir rolün tüm üyeleri için gerekli olmayabilir. Burada, kuruluşunuzun bir Azure aboneliğinde çalıştırılan uygulamanın geliştirilmesine yardımcı olmak için birkaç sözleşme ilişkilendirir hires bir senaryo düşünün. Bir kaynak yöneticisi, gerekli onay uygun erişim sağlamak için çalışanların istediğiniz, ancak sözleşme ilişkilendirir onay istemelisiniz. Onay iş akışı için yalnızca sözleşme ilişkilendirir yapılandırmak için atanmış çalışanlara rolle aynı izinlere sahip bir özel rol oluşturabilirsiniz. Bu özel rolü etkinleştirmek için onay gerektirebilir. [Özel roller hakkında daha fazla bilgi](pim-resource-roles-custom-role-policy.md).

Onay iş akışını yapılandırın ve kimler onaylayabilir veya istekleri reddetme belirtmek için aşağıdaki yordamları kullanın.

## <a name="require-approval-to-activate"></a>Etkinleştirmek için onay gerektir

1. PIM için Azure portalında göz atın ve listeden bir kaynak seçin.

   ![Seçilen kaynak "Azure kaynaklarını" bölmesi](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

2. Sol bölmeden **rol ayarları**.

3. İçin arama yapın ve bir rol seçin ve ardından **Düzenle** ayarlarını değiştirmek için.

   !["Düzenle" düğmesine operatörü rolü için](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_view_settings.png)

4. İçinde **etkinleştirme** bölümünden **etkinleştirmek için onay gerektir** onay kutusu.

   ![Rol ayarları "Etkinleştirme" bölümü](media/azure-pim-resource-rbac/aadpim_rbac_settings_require_approval_checkbox.png)

## <a name="specify-approvers"></a>Onaylayanlar belirtin

Tıklayın **onaylayanları seçin** açmak için **bir kullanıcı veya Grup Seç** bölmesi.

>[!NOTE]
>En az bir kullanıcı veya grup ayarını güncelleştirmek için seçmeniz gerekir. Hiçbir varsayılan onaylayanlar vardır.

Kaynak yöneticileri, kullanıcıları ve grupları herhangi bir birleşimini onaylayanlar listesine ekleyebilirsiniz. 

![Kullanıcı tarafından seçilen bir "bir kullanıcı veya Grup Seç" bölmesi](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_select_approvers.png)

## <a name="request-approval-to-activate"></a>Etkinleştirmek için onay isteyin

Onay isteme üyesi etkinleştirmek için izlemeniz gereken yordam üzerinde hiçbir etkisi olmaz. [Bir rolü etkinleştirmek için adımları gözden](pim-resource-roles-activate-your-roles.md).

Onay gerektiren bir rolü etkinleştirmesi üyesi istenen ve rol artık gerekli değildir, üye PIM isteğini iptal edebilirsiniz.

İptal etmek için PIM ve seçin için Gözat **isteklerim**. Seçin ve istek bulun **iptal**.

!["İsteklerim" bölmesi](media/azure-pim-resource-rbac/aadpim_rbac_role_approval_request_pending.png)

## <a name="approve-or-deny-a-request"></a>Onaylayın veya reddedin isteği

Onaylayın veya bir isteği reddetmek için onaylayan listesinin bir üyesi olmalıdır. 

1. PIM içinde seçin **istekleri onaylama** sol menüde sekmesinden ve istek bulun.

   !["İstekleri onaylama" bölmesi](media/azure-pim-resource-rbac/aadpim_rbac_approve_requests_list.png)

2. İstek, bir kullanıcının bir gerekçe kararı seçip **Onayla** veya **Reddet**. İstek ardından çözülür.

   ![Seçilen istek ayrıntılı bilgileri](media/azure-pim-resource-rbac/aadpim_rbac_approve_request_approved.png)

## <a name="workflow-notifications"></a>İş akışı bildirimlerini

İş akışı bildirimlerini hakkında bazı bilgiler şunlardır:

- Bir rol için bir isteği, gözden geçirme olduğunda tüm üyelerinin onaylayan listesini e-posta ile bildirilir. E-posta bildirimleri onaylayan burada onaylama veya reddetme isteğini doğrudan bir bağlantı içerir.
- İstekleri onaylar veya reddeder listenin ilk üyesi tarafından çözümlenir. 
- Bir onaylayan isteği yanıtladığında onaylayan listenin tüm üyelerini eylemi bildirilir. 
- Onaylanan bir üyenin içindeki rollerine etkin olduğunda, kaynak yöneticileri bildirilir. 

>[!Note]
>Onaylanan bir üyenin etkin olmamalıdır düşündüğü bir kaynak yöneticisi PIM'de etkin bir rol atamasını kaldırabilir. Onaylayan listesini üyesi olduğu sürece kaynak yöneticileri, bekleyen istek bildirilmez olsa da, görüntüleyebilir ve bekleyen istekler PIM görüntüleyerek bekleyen tüm kullanıcıların istekleri iptal et. 

## <a name="next-steps"></a>Sonraki adımlar

[PIM ayarları benzersiz kullanıcı gruplarına uygulanır.](pim-resource-roles-custom-role-policy.md)
