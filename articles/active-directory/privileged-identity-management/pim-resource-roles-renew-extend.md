---
title: PIM - Azure Active Directory, Azure kaynak rol atamalarını yenileme veya genişletme | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure kaynak rol atamalarını yenileme veya genişletme öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8aef7d4ac37109233af4ffb364b6f3e4c9b291ac
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60287804"
---
# <a name="extend-or-renew-azure-resource-role-assignments-in-pim"></a>PIM Azure kaynak rol atamalarını yenileme veya genişletme

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) Azure kaynakları için erişim ve atama yaşam döngüsünü yönetmek için yeni denetimler sunar. Yöneticiler, üyeliği başlangıç ve bitiş tarihi zamanı özelliklerini kullanarak atayabilirsiniz. Atama sonuna yaklaştığında, PIM etkilenen kullanıcılar veya gruplar için e-posta bildirimleri gönderir. Ayrıca uygun erişim sağlandığından emin olmak için kaynak için Yöneticiler e-posta bildirimleri gönderir. Atamalar yenilenmesi ve erişim genişletilmedi bile görünür 30 güne kadar süresi dolmuş durumda kalır.

## <a name="who-can-extend-and-renew"></a>Kimin genişletin, yenilemek ve?

Yalnızca kaynak yöneticileri, genişletme veya rol atamalarını yenileme. Etkilenen üye süresinin dolmasını ve zaten süresi dolan rolleri yenileme isteği rollerini uzatma isteğinde bulunabilir.

## <a name="when-are-notifications-sent"></a>Bildirimler gönderildiğinde?

PIM, Yöneticiler ve 14 gün ve süre sonundan önce bir gün içinde sona erecek rollerinin etkilenen üyeleri için e-posta bildirimleri gönderir. Atama resmi olarak sona erdiğinde ek e-posta gönderir. 

Süresi dolacak veya süresi dolmuş rolünün bir üyesi uzatın veya yenilemek istediğinde Yöneticiler bildirimleri alın. Belirli bir yöneticinin istek çözdüğünde, diğer yöneticilerin tüm çözüm kararlarını (Onaylandı veya reddedildi) bildirilir. Ardından isteyen üye karar bildirilir. 

## <a name="extend-role-assignments"></a>Rol atamaları genişletme

Aşağıdaki adımlar, isteyen, çözümleme veya bir uzantı veya rol atamasını yenileme yönetme sürecini özetlemektedir. 

### <a name="member-extend"></a>Üye genişletme

Süresi dolan rol atamaları doğrudan rol ataması üyeleri genişletebilir **uygun** veya **etkin** sekmesinde **rollerim** bir kaynağı ve en üst düzey sayfa **Rollerim** PIM portal sayfası. Üyeleri sonraki 14 gün içinde sona uygun ve etkin (atanan) rolleri uzatma isteğinde bulunabilir.

![Rolleri genişletmek](media/azure-pim-resource-rbac/aadpim_rbac_extend_ui.png)

Atama bitiş tarihi / saati olduğunda düğme 14 gün içinde **Genişlet** kullanıcı arabiriminde bir etkin bağlantı haline gelir. Aşağıdaki örnekte, geçerli tarihi 27 Mart olduğunu varsayalım.

![Genişletme düğmesi](media/azure-pim-resource-rbac/aadpim_rbac_extend_within_14.png)

Bu rol ataması isteğinde bulunmanız için seçin **Genişlet** istek formunu açın.

![İstek formunu açın](media/azure-pim-resource-rbac/aadpim_rbac_extend_role_assignment_request.png)

Özgün atama hakkında bilgi görüntülemek için genişletin **atama ayrıntıları**. Uzantı isteği için bir neden girin ve ardından **Genişlet**.

>[!Note]
>Uzantı neden gereklidir ve uzantı ne kadar süreyle verilmelidir (Bu bilgi varsa) için ayrıntılar dahil olmak üzere öneririz.

![Rol atamasını genişletme](media/azure-pim-resource-rbac/aadpim_rbac_extend_form_complete.png)

Birkaç dakika içinde kaynak yöneticileri uzantı isteği gözden isteyen bir e-posta bildirimi alırsınız. Genişletmek için bir istek zaten gönderildi hatayı açıklayan Azure portalının en üstünde bir bildirim görüntülenir.

![Hatayı açıklayan bildirim](media/azure-pim-resource-rbac/aadpim_rbac_extend_failed_existing_request.png)

Git **bekleyen istekler** sol bölmedeki isteğinizin durumunu görüntülemek için veya iptal etmek için sekmesinde.

![Bekleyen istekler](media/azure-pim-resource-rbac/aadpim_rbac_extend_cancel_request.png)

### <a name="admin-approve"></a>Yönetici Onayla

Üye bir rol atamasını genişletme isteği gönderdiğinde, kaynak yöneticileri özgün atama ve isteğin nedenini ayrıntılarını içeren bir e-posta bildirimi alırsınız. Bildirim isteği onaylamak veya reddetmek yönetici için doğrudan bir bağlantı içerir. 

Bağlantıyı e-postadan takip kullanmanın yanı sıra yöneticileri onaylayabilir veya seçerek ve portal PIM yönetimini giderek istekleri reddetme **istekleri onaylama** sol bölmesinde.

![Hatanın ekran görüntüsü](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Yönetici seçtiğinde **Onayla** veya **Reddet**, isteğin ayrıntılarını bir alan için denetim günlüklerini gerekçesi sağlamak için birlikte gösterilir.

![Rol ataması isteği Onayla](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Rol atamasını genişletme isteği onaylama sırasında kaynak yöneticileri yeni başlangıç tarihi, bitiş tarihi ve atama türü seçebilirsiniz. Atama türü değiştirme (örneğin bir gün) belirli bir görevi tamamlamak için sınırlı erişim sağlamak yönetici istiyorsa, gerekli olabilir. Bu örnekte, yönetici atamadan değiştirebilir **uygun** için **etkin**. Başka bir deyişle, bunlar etkinleştirmeye gerek kalmadan istek sahibine erişim sağlayabilir.

### <a name="admin-extend"></a>Yöneticiyi genişlet

Bir rol üyesi unutması veya bir rolü üyeliği uzantı isteği gönderemedi, yönetici atama üye adına genişletebilirsiniz. Rol üyeliğini yönetim uzantıları onay gerektirmez, ancak rol genişletilmiş sonra diğer yöneticiler için bildirimler gönderilir.

Rol üyeliği genişletmek için kaynak rolü veya üye görünümünü PIM göz atın. Bir uzantının gerektirdiği üye bulun. Ardından **Genişlet** Eylem sütununda.

![Rol üyeliği genişletme](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_extend.png)

## <a name="renew-role-assignments"></a>Rol atamalarını yenileme

Kavramsal olarak benzer bir uzantı isteme işlemi sırasında süresi dolmuş bir rol atamasını yenileme işlemi farklıdır. Aşağıdaki adımları kullanarak, üyeleri ve Yöneticiler, süresi dolan roller gerektiğinde erişim yenileyebilirsiniz.

### <a name="member-renew"></a>Üye yenileme

En çok 30 gün süresi dolmuş atama geçmişi kaynaklarına artık erişemez üyeleri erişebilir. Bunu yapmak için bunlar için Gözat **My rolleri** sol bölmesi ve ardından **rolleri süresi** Azure kaynak rolleri bölümünün sekmesinde.

![Süresi dolan rolleri sekmesi](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_myroles.png)

Varsayılan olarak gösterilen rollerin listesini **uygun roller**. Etkin roller atama için uygun arasında geçiş yapmak için açılan menüyü kullanın.

Herhangi birinin listesinde rol atamalarını yenileme isteği için seçin **yenileme** eylem. Daha sonra istek için bir neden belirtin. Bir süre onaylamak veya reddetmek karar kaynak yönetici yardımcı olan herhangi bir ek bağlam yanı sıra sağlamak yararlıdır.

![Rol atamasını yenileme](media/azure-pim-resource-rbac/aadpim_rbac_renew_request_form.png)

İstek gönderildikten sonra kaynak yöneticileri rol atamasını yenileme bekleyen isteği bildirilir.

### <a name="admin-approves"></a>Yönetici onaylar

Kaynak yöneticileri yenileme isteği e-posta bildirimi veya Azure portalından PIM erişme ve seçerek bağlantıdan erişebileceğiniz **istekleri onaylama** sol bölmeden.

![İstekleri onaylama](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Yönetici seçtiğinde **Onayla** veya **Reddet**, isteğinin ayrıntılarını denetim günlükleri için gerekçe göstermesi bir alanı ile birlikte gösterilir.

![Rol atamasını Onayla](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Rol atamasını yenileme isteği onaylama sırasında kaynak yöneticileri yeni başlangıç tarihi, bitiş tarihi ve atama türü girmeniz gerekir. 

### <a name="admin-renew"></a>Yöneticiyi yenile

Kaynak yöneticileri süresi dolmuş rol atamaları yenilemek **üyeleri** kaynağın sol gezinti menüsünde sekmesi. Süresi dolan rol atamaları içinden de yenileyebilirsiniz **süresi dolan** kaynak rolünün rol sekmesi.

Tüm listesini görüntülemek için rol atamalarını tarihinde isteğin süresi doldu **üyeleri** ekranındayken **rolleri süresi**.

![Süresi dolan roller](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_member_blade.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Onaylayın veya reddedin PIM Azure kaynak rolleri için istekleri](pim-resource-roles-approval-workflow.md)
- [PIM'de Azure kaynak rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
