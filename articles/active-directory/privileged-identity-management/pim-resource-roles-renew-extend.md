---
title: Privileged Identity Management'ı kullanarak Azure kaynaklarını rollerinde gözden geçirin ve genişletmek için | Microsoft Docs
description: Bu belge, Azure kaynak rolleri PIM kaynaklar için yenileme ve genişletmek için açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: cd1524bf03256a2ed706d11a8702c7b5eff5dad4
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260514"
---
# <a name="extend-and-review-roles-in-azure-resources-by-using-privileged-identity-management"></a>Privileged Identity Management'ı kullanarak Azure kaynaklarını rollerinde gözden geçirin ve genişletmek için

Azure kaynakları için ayrıcalıklı Kimlik Yönetimi (PIM) Azure kaynakları için erişim ve atama yaşam döngüsü yönetmek için yeni denetimler tanıtır. Yöneticiler, başlangıç ve bitiş tarih-saat özelliklerini kullanarak üyelik atayabilir. Atama sonuna yaklaştığında PIM etkilenen kullanıcılar veya gruplar için e-posta bildirimleri gönderir. Ayrıca uygun erişim korunduğundan emin olmak için kaynağın için Yöneticiler e-posta bildirimleri gönderir. Atamaları yenilenmesi ve erişim genişletilmedi olsa bile görünür 30 güne kadar süresi dolmuş durumda kalır.

## <a name="who-can-extend-and-renew"></a>Kim genişletmek yenilemek ve?

Yalnızca Yöneticiler kaynağının genişletin veya rol atamalarını yenileyin. Etkilenen üye süresinin dolmasını ve zaten süresi rolleri yenileme isteği rollerini genişletmek için isteyebilir.

## <a name="when-are-notifications-sent"></a>Bildirimler gönderildiğinde?

PIM Yöneticiler ve etkilenen 14 gün ve süre sonundan önce bir gün içinde süresi doluyor rollerinin üyeleri e-posta bildirimleri gönderir. Atama resmi olarak sona erdiğinde ek bir e-posta gönderir. 

Süresi dolan veya süresi dolmuş rolünün bir üyesi genişletmek veya yenilemek için istediğinde Yöneticiler bildirimleri alır. Belirli bir yöneticinin isteği çözdüğünde, diğer tüm yöneticilere (onaylanan veya reddedilen) çözümleme kararı ve bildirilir. Ardından isteyen üye kararı ve bildirilir. 

## <a name="extend-role-assignments"></a>Rol atamalarını genişletme

Aşağıdaki adımları isteyen, çözümleme veya bir uzantı veya bir rol ataması yenilenmesini yönetme sürecini özetlemektedir. 

### <a name="member-extend"></a>Üye genişletme

Bir rol ataması üyeleri, süresi dolan rol atamalarını doğrudan genişletebilir **uygun** veya **etkin** sekmesi **My rolleri** bir kaynağın ve en üst düzey sayfa **My rolleri** PIM portal sayfası. Üyeleri sonraki 14 gün içinde sona uygun ve etkin (atanmamış) rollerini genişletmek için isteyebilir.

![Rolleri genişletme](media/azure-pim-resource-rbac/aadpim_rbac_extend_ui.png)

Atama son tarih-saat olduğunda düğme 14 gün içinde **Genişlet** kullanıcı arabiriminde etkin bir bağlantı olur. Aşağıdaki örnekte, geçerli tarihe Mart 27 olduğunu varsayalım.

!["Genişlet" düğmesi](media/azure-pim-resource-rbac/aadpim_rbac_extend_within_14.png)

Bu rol ataması uzantısı istemek için seçin **Genişlet** istek formunu açmak için.

![İstek formunu açın](media/azure-pim-resource-rbac/aadpim_rbac_extend_role_assignment_request.png)

Özgün ataması hakkında bilgi görüntülemek için Genişlet **atama ayrıntıları**. Uzantı isteği nedenini girin ve ardından **Genişlet**.

>[!Note]
>Uzantı neden gerekli olduğu ve (Bu bilgiler varsa) ne kadar süreyle uzantısı verilmelidir ayrıntıları dahil olmak üzere öneririz.

![Rol ataması genişletme](media/azure-pim-resource-rbac/aadpim_rbac_extend_form_complete.png)

Birkaç dakika içinde kaynak yöneticileri uzantısı isteği gözden isteyen bir e-posta bildirimi alırsınız. Genişletmek için bir istek zaten gönderildi hatayı açıklayan Azure portalının en üstünde bir bildirim görüntülenir.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_failed_existing_request.png)

Git **bekleyen istekler** isteğinizin durumunu görüntülemek veya iptal etmek için sol bölmedeki sekmesi.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_cancel_request.png)

### <a name="admin-approve"></a>Yönetici Onayla

Üye bir rol ataması genişletmek için bir istek gönderdiğinde, kaynak yöneticileri özgün atama ve isteğin nedenini ayrıntılarını içeren bir e-posta bildirimi alırsınız. Bildirim isteği onaylamak veya reddetmek yönetici için doğrudan bir bağlantı içerir. 

Bağlantıyı e-posta adresinden aşağıdaki kullanarak ek olarak, yöneticiler onaylayabilir veya PIM Yönetim Portalı ve seçme giderek istekleri reddetmesini **isteklerini onaylama** sol bölmede.

![Hatanın ekran görüntüsü](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Bir yönetici seçtiğinde **Onayla** veya **reddetme**, istek ayrıntılarını gerekçe denetim günlükleri için bir alan birlikte gösterilir.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Rol ataması genişletmek için bir istek onaylama sırasında kaynak yöneticileri yeni başlangıç tarihi, bitiş tarihi ve atama türü seçebilirsiniz. Atama türü değiştirme (örneğin bir gün) belirli bir görevi tamamlamak için sınırlı erişim sağlamak Yönetici isterse gerekli olabilir. Bu örnekte, yönetici bir atama değiştirebilir **uygun** için **etkin**. Başka bir deyişle, bunlar etkinleştirmek için gerek kalmadan istek sahibine erişim sağlayabilir.

### <a name="admin-extend"></a>Yöneticiyi genişlet

Bir rolü üyesi unutması ya da bir rol üyeliğini uzantısı isteği gönderemedi yönetici atama üye adına genişletebilirsiniz. Yönetici rol üyeliğini uzantıları onay gerektirmez, ancak rol genişletilmiş sonra bildirimleri diğer tüm yöneticilere gönderilir.

Bir rol üyeliğini genişletmek için kaynak rol veya üye görünümünü PIM göz atın. Uzantı gerektirir üye bulun. Ardından **Genişlet** eylem sütunundaki.

![Bir rol üyeliğini genişletme](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_extend.png)

## <a name="renew-role-assignments"></a>Rol atamalarını Yenile

Kavramsal olarak benzer uzantı isteyen işlemi sırasında süresi dolan rol ataması yenilemek için farklı işlemidir. Aşağıdaki adımları kullanarak, üyeleri ve Yöneticiler, süresi dolan rollere gerektiğinde erişim yenileyebilirsiniz.

### <a name="member-renew"></a>Üye yenileme

Süresi dolan atama geçmişi 30 güne kadar artık kaynaklara kimin erişebileceğini üyeleri erişebilir. Bunu yapmak için bunlar için Gözat **My rolleri** sol bölmesinde ve seçip **rolleri süresi** Azure kaynak rolleri bölümündeki sekmesinde.

!["Rolleri süresi" sekmesi](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_myroles.png)

Varsayılan olarak gösterilen rollerin listesini **uygun roller**. Etkin roller atama için uygun arasında geçiş yapmak için açılan menüyü kullanın.

Herhangi bir rol atamaları listesinde için yenileme isteği için seçin **yenileme** eylem. Ardından isteği nedeni belirtin. Bir süre onaylamak veya reddetmek karar Kaynak Yöneticisi yardımcı olan her ek bağlam yanı sıra sağlamak yararlıdır.

![Rol ataması yenileme](media/azure-pim-resource-rbac/aadpim_rbac_renew_request_form.png)

İstek gönderildikten sonra kaynak yöneticileri rol ataması yenilemek için bekleyen isteği bildirilir.

### <a name="admin-approves"></a>Yönetici onaylar

Kaynak Yöneticiler e-posta bildirimi veya PIM Azure portalından erişmek ve seçerek bağlantısından yenileme isteği erişebilir **isteklerini onaylama** sol bölmeden.

![İstekleri onayla](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Bir yönetici seçtiğinde **Onayla** veya **reddetme**, istek ayrıntılarını denetim günlükleri için gerekçe alana birlikte gösterilir.

![Rol ataması Onayla](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Rol ataması yenileme isteği onaylama sırasında kaynak yöneticileri yeni başlangıç tarihi, bitiş tarihi ve atama türü girmeniz gerekir. 

### <a name="admin-renew"></a>Yöneticiyi yenile

Kaynak yöneticileri süresi dolan rol atamaları yenileme **üyeleri** sol gezinti menüsünde bir kaynağın sekmesi. Süresi dolan rol atamalarını içinden de yenileyebilirsiniz **süresi doldu** Kaynak rolü rolleri sekmesinde.

Tüm listesini görüntülemek için rol atamalarını, süresi dolan **üyeleri** ekran, select **rolleri süresi**.

![Süresi dolan roller](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_member_blade.png)

## <a name="next-steps"></a>Sonraki adımlar

[Etkinleştirmek için onay iste](pim-resource-roles-approval-workflow.md)

[Bir rolü etkinleştirmesi](pim-resource-roles-use-the-audit-log.md)


