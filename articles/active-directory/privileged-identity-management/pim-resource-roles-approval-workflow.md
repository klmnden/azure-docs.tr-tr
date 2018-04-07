---
title: Privileged Identity Management - Azure kaynak rolleri için onay iş akışı | Microsoft Docs
description: Azure kaynakları için onay iş akışı işleminin nasıl açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: c02d595d75b2d63558896054c185102ebb23cc9e
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---approve"></a>Privileged Identity Management - kaynak rolleri - Onayla

Onay iş akışı ile PIM Azure kaynak rolleri için Yöneticiler daha fazla koruyabilir veya rol atamalarını etkinleştirmek için onay gerektirerek kritik kaynaklara erişimi kısıtlama. Azure kaynak rolleri için benzersiz bir kaynak hiyerarşinin kavramdır. Bu hiyerarşide aşağı üst kaynak nesneden rol atamalarını üst kapsayıcı içindeki tüm alt alt kaynaklarına devralma sağlar. 

Örneğin Bob, Contoso abonelik sahibi rolünde Alice uygun bir üye olarak atamak için PIM bir kaynak yöneticisi kullanır. Bu atama ile Alice de Contoso abonelik içindeki tüm kaynak grubu kapsayıcıları abonelik her kaynak grubu, içerdiği tüm kaynaklar (örneğin, sanal makineler) ve uygun bir sahibi değil. Contoso abonelikte üç kaynak grubu yok varsayalım; Fabrikam Test, Fabrikam geliştirme ve Fabrikam üretim. Bu kaynak gruplarının her biri tek bir sanal makine içeriyor.

PIM ayarlar her bir kaynak rolü için yapılandırılır ve atamaları aksine, bu ayarlar değil devralınan ve kesinlikle Kaynak rolü için geçerlidir. [Uygun atamaları ve kaynak görünürlük hakkında daha fazlasını okuyun.](pim-resource-roles-eligible-visibility.md)

Yukarıdaki örneği kullanarak Bob PIM Contoso abonelik isteği onayı sahip rolünü etkinleştirmek tüm üyeleri gerektirecek şekilde kullanır. Fabrikam üretim kaynak grubunda yer alan kaynakları korumak için Bob onay bu kaynağın sahibine rolünün üyeleri için de gerektirir. Fabrikam Test ve Fabrikam Geliştirici sahibi rollerinde etkinleştirmek için onay gerektirmez.

Ne zaman Alice istekleri etkinleştirme Contoso abonelik onaylayıcı onun sahibi rolünün onaylamak veya bu aynen rolünde etkinleştirilmeden önce her isteği reddedecek gerekir. Ayrıca Alice karar verirse [kendi etkinleştirme kapsam](pim-resource-roles-activate-your-roles.md#just-enough-administration) Fabrikam üretim kaynak grubuna onaylayıcı onaylamak veya bu istek çok erişimi engellemeniz gerekir. Ancak, Alice, kendi etkinleştirme birini veya her ikisini Fabrikam Test ya da Fabrikam Geliştirici kapsam karar verirse, onayı gerekli olmaz.

Onay iş akışı bir rolün tüm üyeleri için gerekli olmayabilir. Burada, kuruluşunuzun bir Azure aboneliği çalışacak bir uygulamanın geliştirilmesi yardımcı olmak için birkaç sözleşme ilişkilendirir anlaşır bir senaryo düşünün. Bir kaynak yöneticisi olarak onay gerekli uygun erişim sağlamak için çalışanların ister misiniz ancak sözleşme ilişkilendirilmiş onay istemeniz gerekir. Yalnızca bu sözleşmenin için onay iş akışını yapılandırmak için ilişkilendirilmiş bir rol aynı izinlere sahip özel bir rol oluşturmak çalışanlara atanır ve bu özel rolü etkinleştirmek için onay gerektirir. [Özel rolleri hakkında daha fazla bilgi](pim-resource-roles-custom-role-policy.md).

Onay iş akışını yapılandırmak ve kimlerin onaylamak veya istekleri reddetmesini belirtmek için aşağıdaki adımları izleyin.

## <a name="require-approval-to-activate"></a>Etkinleştirmek için onay gerektir

PIM için Azure portalında gidin ve listeden bir kaynak seçin.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

Sol gezinti menüsünden seçin **rol ayarlarını**.

İçin arama yapın ve bir rol seçin ve tıklatın **Düzenle** ayarlarını değiştirmek için.

![](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_view_settings.png)

Etkinleştirme bölümünde onay kutusunu **etkinleştirmek için onay iste**.

![](media/azure-pim-resource-rbac/aadpim_rbac_settings_require_approval_checkbox.png)

## <a name="specify-approvers"></a>Onaylayanlar belirtin

Tıklatın **seçin onaylayanlar** seçimi ekranını açmak için.

>[!NOTE]
>En az bir kullanıcı veya grup ayarı güncelleştirmek için seçmeniz gerekir. Hiçbir varsayılan onaylayanlar vardır.

Kaynak Yöneticileri kullanıcıların ve grupların herhangi bir birleşimini onaylayanlar listesine ekleyebilirsiniz. 

![](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_select_approvers.png)

## <a name="request-approval-to-activate"></a>Etkinleştirmek için onay iste

Onay yordamı üzerinde hiçbir etkisi olmaz isteyen bir üye etkinleştirmek için izlemeniz gerekir. [Bir rolü etkinleştirmek için adımları gözden](pim-resource-roles-activate-your-roles.md).

Onay gerektiren bir rolü etkinleştirmesi üyesi istenen ve rol artık gerekli değildir, üye PIM isteği iptal edebilirsiniz.

İptal etmek için PIM gidin ve "İsteklerim" seçin. İstek bulun ve "İptal" i tıklatın.

![](media/azure-pim-resource-rbac/aadpim_rbac_role_approval_request_pending.png)

## <a name="approve-or-deny-a-request"></a>Onaylamak veya bir isteği reddedecek

Onaylamak veya bir isteği reddetmek için onaylayan listesinin bir üyesi olması gerekir. PIM, sol gezinti menüsünde sekmesinden "isteklerini onaylama"'ı seçin ve istek bulun.

![](media/azure-pim-resource-rbac/aadpim_rbac_approve_requests_list.png)

İstek'i seçin, bir gerekçe kararı ve seçin onaylamak veya reddetmek, istek çözümleniyor.

![](media/azure-pim-resource-rbac/aadpim_rbac_approve_request_approved.png)

## <a name="workflow-notifications"></a>İş akışı bildirimleri

- Bir rol için bir istek gözden olduğunda tüm üyeleri onaylayan listesi, e-posta ile bildirilir. E-posta bildirimleri onaylayan burada onaylama veya reddetme isteği doğrudan bir bağlantı içerir.
- İstekleri onaylar veya reddeder listedeki ilk üye tarafından çözümlenir. 
- Onaylayıcı isteğini yanıtladığında onaylayan listesinin tüm üyeleri eylemini bildirilir. 
- Onaylanan bir üye kendi rolünde etkin olduğunda kaynak yöneticileri bildirilir. 

>[!Note]
>Burada bir kaynak yöneticisi onaylanan üye etkin olmamalıdır düşündüğü bir durumda, bunlar PIM etkin rol atamasını kaldırabilir. Onaylayan listesi üyesi olmadığı sürece kaynak yöneticileri, bekleyen istek bildirilmez olsa da, bunlar görüntülemek ve bekleyen istekler PIM görüntüleyerek tüm kullanıcıların bekleyen istekleri iptal. 

## <a name="next-steps"></a>Sonraki adımlar

[Benzersiz kullanıcı gruplarını PIM ayarlarını uygula](pim-resource-roles-custom-role-policy.md)
