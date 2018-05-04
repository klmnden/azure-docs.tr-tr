---
title: Privileged Identity Management rollerinde Azure kaynak için onay iş akışı | Microsoft Docs
description: Azure kaynakları için onay iş akışı işlemini açıklar.
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
ms.openlocfilehash: 7781c858a5c0e4db8593df0cf77b868b6fd23622
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="approval-workflow-for-azure-resource-roles-in-privileged-identity-management"></a>Privileged Identity Management rollerinde Azure kaynak için onay iş akışı

Onay iş akışı ile ayrıcalıklı Kimlik Yönetimi (PIM), Azure kaynak rolleri için Yöneticiler daha fazla koruyabilir veya kritik kaynaklara erişimi kısıtlama. Diğer bir deyişle, Yöneticiler, rol atamaları etkinleştirmek için onay gerektirebilirsiniz. 

Azure kaynak rolleri için benzersiz bir kaynak hiyerarşinin kavramdır. Bu hiyerarşide aşağı üst kaynak nesneden rol atamalarını üst kapsayıcı içindeki tüm alt kaynaklarına devralma sağlar. 

Örneğin: Bob, bir kaynak yöneticisi, Contoso abonelik sahibi rolünde Alice uygun bir üye olarak atamak için PIM kullanır. Bu atama ile Alice bir uygun kaynak grubu Contoso abonelik alt kapsayıcılara sahibidir. Alice ayrıca bir uygun tüm kaynaklar (örneğin, sanal makineler) abonelik her kaynak grubu sahibi. 

Contoso abonelikte üç kaynak grubu yok varsayalım: Fabrikam Test, Fabrikam Geliştirici ve Fabrikam üretim. Bu kaynak gruplarının her biri tek bir sanal makine içeriyor.

PIM ayarları, her bir kaynak rolü için yapılandırılır. Atamaları aksine bu ayarlar değil devralınan ve kesinlikle Kaynak rolü için geçerlidir. [Uygun atamaları ve kaynak görünürlük hakkında daha fazla bilgiyi](pim-resource-roles-eligible-visibility.md).

Bu örnekle devam edersek: Bob Contoso abonelik isteği onay sahibi roldeki tüm üyelerin etkinleştirilecek gerektirecek şekilde PIM kullanır. Fabrikam üretim kaynak grubundaki kaynaklar korunmasına yardımcı olmak için Bob Ayrıca bu kaynağın sahibine rolünün üyeleri için onay gerektirir. Fabrikam Test ve Fabrikam Geliştirici sahibi rollerinde, etkinleştirme için onay gerektirmez.

Alice'i Contoso abonelik için sahip rolünü etkinleştirme istediğinde onaylayıcı onaylayabilir veya aynen rolünde etkinleştirilmeden önce her isteği reddedecek gerekir. Alice karar verirse [kendi etkinleştirme kapsam](pim-resource-roles-activate-your-roles.md#apply-just-enough-administration-practices) Fabrikam üretim kaynak grubuna onaylayıcı onaylamak veya bu istek çok erişimi engellemeniz gerekir. Ancak Alice birini veya her ikisini Fabrikam Test ya da Fabrikam geliştirici kendi etkinleştirme kapsam karar verirse, onayı gerekli değildir.

Onay iş akışı bir rolün tüm üyeleri için gerekli olmayabilir. Burada, kuruluşunuzun bir Azure aboneliği çalışacak bir uygulamanın geliştirilmesi yardımcı olacak birkaç sözleşme ilişkilendirir anlaşır bir senaryo düşünün. Kaynak yönetici olarak onay gerekli uygun erişim sağlamak için çalışanların istiyor, ancak sözleşme ilişkilendirilmiş onay istemeniz gerekir. Sözleşme ilişkilendirilmiş yalnızca bir için onay iş akışını yapılandırmak için çalışanlara atanan role aynı izinlere sahip bir özel rolü oluşturabilirsiniz. Bu özel rolü etkinleştirmek için onay gerektirebilirsiniz. [Özel rolleri hakkında daha fazla bilgi](pim-resource-roles-custom-role-policy.md).

Onay iş akışını yapılandırmak ve kimlerin onaylamak veya istekleri reddetmesini belirtmek için aşağıdaki yordamları kullanın.

## <a name="require-approval-to-activate"></a>Etkinleştirmek için onay gerektir

1. PIM için Azure portalında göz atın ve listeden bir kaynak seçin.

   ![Seçilen kaynak "Azure kaynaklarını" bölmesi](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

2. Sol bölmeden seçin **rol ayarlarını**.

3. İçin arama yapın ve bir rol seçin ve ardından **Düzenle** ayarlarını değiştirmek için.

   !["Düzenle" düğmesine işleci rolü](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_view_settings.png)

4. İçinde **etkinleştirme** bölümünde, select **etkinleştirmek için onay iste** onay kutusu.

   !["Etkinleştirme" bölümünü rol ayarları](media/azure-pim-resource-rbac/aadpim_rbac_settings_require_approval_checkbox.png)

## <a name="specify-approvers"></a>Onaylayanlar belirtin

Tıklatın **seçin onaylayanlar** açmak için **bir kullanıcı veya Grup Seç** bölmesi.

>[!NOTE]
>En az bir kullanıcı veya grup ayarı güncelleştirmek için seçmeniz gerekir. Hiçbir varsayılan onaylayanlar vardır.

Kaynak Yöneticileri kullanıcıların ve grupların herhangi bir birleşimini onaylayanlar listesine ekleyebilirsiniz. 

![Seçili bir kullanıcı "bir kullanıcı veya Grup Seç" bölmesiyle](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_select_approvers.png)

## <a name="request-approval-to-activate"></a>Etkinleştirmek için onay iste

Onay isteyen bir üye etkinleştirmek için izlemeniz gereken yordam etkisi yoktur. [Bir rolü etkinleştirmek için adımları gözden](pim-resource-roles-activate-your-roles.md).

Onay gerektiren bir rolü etkinleştirmesi üyesi istenen ve rol artık gerekli değildir, üye PIM isteği iptal edebilirsiniz.

İptal etmek için PIM ve Seç gözatın **isteklerim**. Seçin ve isteği bulun **iptal**.

!["İsteklerim" bölmesi](media/azure-pim-resource-rbac/aadpim_rbac_role_approval_request_pending.png)

## <a name="approve-or-deny-a-request"></a>Onaylamak veya bir isteği reddedecek

Onaylamak veya bir isteği reddetmek için onaylayan listesinin bir üyesi olması gerekir. 

1. PIM içinde seçin **isteklerini onaylama** soldaki menüden sekmesinden ve istek bulun.

   !["İsteklerini onaylama" bölmesi](media/azure-pim-resource-rbac/aadpim_rbac_approve_requests_list.png)

2. İstek'i seçin, bir gerekçe kararı ve seçin **Onayla** veya **reddetme**. İstek sonra çözümlenir.

   ![Seçilen istek ayrıntılı bilgilerle birlikte](media/azure-pim-resource-rbac/aadpim_rbac_approve_request_approved.png)

## <a name="workflow-notifications"></a>İş akışı bildirimleri

İş akışı bildirimleri hakkında bazı unsurlar şunlardır:

- Bir rol için bir istek gözden olduğunda tüm üyeleri onaylayan listesi, e-posta ile bildirilir. E-posta bildirimleri onaylayan burada onaylama veya reddetme isteğine, doğrudan bir bağlantı içerir.
- İstekleri onaylar veya reddeder listenin ilk üye tarafından çözümlenir. 
- Onaylayıcı isteğini yanıtladığında onaylayan listesinin tüm üyeleri eylemini bildirilir. 
- Onaylanan bir üye kendi rolünde etkin olduğunda kaynak yöneticileri bildirilir. 

>[!Note]
>Onaylanan bir üye etkin olmamalıdır düşündüğü bir kaynak yönetici etkin rol ataması PIM içinde kaldırabilirsiniz. Onaylayan listesi üyesi olmadığı sürece kaynak yöneticileri, bekleyen istek bildirilmez olsa da görüntüleyebilir ve bekleyen istekler PIM görüntüleyerek tüm kullanıcıların bekleyen istekleri iptal. 

## <a name="next-steps"></a>Sonraki adımlar

[Benzersiz kullanıcı gruplarını PIM ayarlarını uygula](pim-resource-roles-custom-role-policy.md)
