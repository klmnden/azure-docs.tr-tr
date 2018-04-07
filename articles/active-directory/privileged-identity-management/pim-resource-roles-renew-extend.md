---
title: Privileged Identity Management Azure kaynakları - genişletmek ve rolleri yenileme | Microsoft Docs
description: Bu belge, Azure kaynak rolleri PIM kaynaklar için yenileme ve genişletmek için açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 2547b3793688eb51a4114f30bfcf61a9402f2cd2
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---extend-or-renew"></a>Privileged Identity Management - kaynak rolleri - genişletmek veya Yenile

PIM Azure kaynakları için Azure kaynakları için erişim ve atama yaşam döngüsü yönetmek için yeni denetimler tanıtır. Yöneticiler, başlangıç ve bitiş tarih-saat özelliklerini kullanarak üyelik atayabilir. Atama sonuna yaklaştığında PIM etkilenen üyeler için e-posta bildirimleri gönderir (Bu bir kullanıcı veya grup olabilir) ve uygun erişim korunduğu emin olmak için kaynak yöneticileri için. Erişim nedeniyle Eylemsizliği genişletilmedi durumunda atamaları yenilenmesi ve 30 güne kadar süresi dolmuş bir durumda görünür kalır.

## <a name="who-can-extend-and-renew"></a>Kim genişletmek yenilemek ve?

Yalnızca Yöneticiler kaynağının genişletin veya rol atamalarını yenileyin. Etkilenen üye dolmak üzere rolleri genişletmek için isteyebilir ve rollerin isteği yenileme süresi zaten dolmuş.

## <a name="when-are-notifications-sent"></a>Bildirimler gönderildiğinde?

PIM Yöneticiler ve etkilenen 14 gün ve süre sonundan önce bir gün içinde süresi doluyorsa rollerinin üyeleri e-posta bildirimleri gönderir. Atama resmi olarak süresi sona erdiğinde ek bir e-posta gönderilir. 

Süresi dolan veya süresi dolmuş rolünün bir üyesi genişletmek veya yenilemek için istediğinde Yöneticiler bildirimleri alır. Belirli bir yöneticinin isteği çözdüğünde diğer tüm yöneticilere (onaylanan veya reddedilen) çözümleme kararı ve bildirilir ve istekte bulunan üye kararı ve bildirilir. 

## <a name="extend-role-assignments"></a>Rol atamalarını genişletme

Aşağıdaki adımlar, adımları ve kullanıcı arabirimi isteyen, çözümleme veya bir uzantı veya bir rol ataması yenilenmesini yönetimiyle ilgili verilmiştir. 

### <a name="member-extend"></a>Üye genişletme

Bir rol ataması üyeleri, süresi dolan rol atamalarını doğrudan "uygun" genişletmek için isteyebileceği veya "Etkin" sekmesinde "Rolleri My" Benim rolleri PIM portal'ın bir kaynağın ve en üst düzey sayfa. Üyeleri sonraki 14 gün içinde sona uygun ve etkin (atanmamış) rollerini genişletmek için isteyebilir.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_ui.png)

Atama son tarih-saat 14 gün içinde olduğunda, "Genişlet" düğmesine kullanıcı arabiriminde etkin bir bağlantı olur. Aşağıdaki örnekte, geçerli tarihe Mart 27 olduğunu varsayalım.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_within_14.png)

Atama uzantı bu rolün istemek için "Genişlet" istek formunu açmak için tıklatın.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_role_assignment_request.png)

"Atama Ayrıntıları" Genişlet özgün atamasını hakkındaki bilgileri görüntülemek için. Uzantı isteği nedenini girin ve "Genişlet"'i tıklatın.

>[!Note]
>Uzantı neden gerekli olduğu ve ne kadar süreyle uzantısı (biliniyorsa) olmalıdır ayrıntıları dahil olmak üzere öneririz.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_form_complete.png)

Birkaç dakika içinde kaynak yöneticileri uzantısı isteği gözden isteyen bir e-posta bildirimi alırsınız. Genişletmek için bir istek zaten gönderildi, bir bildirim hatayı açıklayan Azure portalının en üstünde görünür.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_failed_existing_request.png)

Durumunu görüntülemek veya isteğinizi iptal etmek için sol gezinti menüsünde "bekleyen istekler" sekmesini ziyaret edin.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_cancel_request.png)

### <a name="admin-approve"></a>Yönetici Onayla

Üye bir rol ataması genişletmek için bir istek gönderdiğinde, kaynak yöneticileri özgün atamasını ve istek sahibi tarafından sağlanan nedeni ayrıntılarını içeren bir e-posta bildirimi alırsınız. Bildirim isteği onaylamak veya reddetmek yönetici için doğrudan bir bağlantı içerir. 

Yöneticiler, bağlantıyı e-posta adresinden ek olarak aşağıdakileri onaylayabilir veya PIM yönetim portalına gezinme ve "Onayla istekleri" sol gezinti menüsünde seçerek istekleri reddetmesini.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Yönetici onaylama veya reddetme seçtiğinde isteğinin ayrıntılarını yanı sıra bir alan için denetim günlüklerini gerekçe gösterilmektedir.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Kaynak yöneticileri rol ataması genişletmek için bir istek onaylama sırasında yeni bir başlangıç ve bitiş tarih-saat ve atama türü seçebilirsiniz. Atama türü değiştirme (örneğin bir gün) belirli bir görevi tamamlamak için sınırlı erişim sağlamak Yönetici isterse gerekli olabilir. Bu örnekte, yönetici atama uygun etkin istek sahibi erişimi etkinleştirmek için gerek kalmadan sağlama değiştirebilirsiniz.

### <a name="admin-extend"></a>Yönetici genişletme

Bir rolü üyesi unutması veya rol üyeliğini uzantısı isteği gönderemedi durumunda yönetici atama üye adına genişletebilir. Rol üyeliğini yönetim uzantıları onay gerektirmez, ancak rol genişletme tamamlandıktan sonra tüm diğer yöneticilere bildirimler gönderilir.

Üyelik bir rolü genişletmek için PIM kaynak rol veya üye görünümüne gidin. Uzantı gerektiren üye bulun ve Eylem sütununda "Genişlet"'i tıklatın.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_extend.png)

## <a name="renew-role-assignments"></a>Rol atamalarını Yenile

Kavramsal olarak benzer olsa da, süresi dolan rol ataması yenilemek için üyeleri ve Yöneticiler için uzantı isteyen daha farklı işlemidir. Üyeleri ve yöneticilerin aşağıdaki adımları kullanarak süresi dolmuş rollere gerektiğinde erişim yenileyebilirsiniz.

### <a name="member-renew"></a>Üye yenileme

Artık kaynaklara kimin erişebileceğini üyeleri My rollere PIM sol gezinti bölmesinde gezinme ve Azure kaynak roller bölümünde "rolleri süresi" sekmesini seçerek 30 güne kadar zaman aşımına uğramış atama geçmişi erişebilir.

![](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_myroles.png)

Uygun atamaları varsayılanlara gösterilen rollerin listesi. Etkin roller atama için uygun arasında geçiş yapmak için açılan listeyi kullanın.

Herhangi bir rol için yenileme isteği için atamaları listesinde "Yenile" eylemini seçin ve isteği nedeni sağlayın. Bir süre onaylamak veya reddetmek karar Kaynak Yöneticisi yardımcı olacak herhangi bir ek bağlam yanı sıra sağlamak yararlıdır.

![](media/azure-pim-resource-rbac/aadpim_rbac_renew_request_form.png)

İsteğin bir gönderim kaynak yöneticileri rol ataması yenilemek için bekleyen isteği bildirilir.

### <a name="admin-approves"></a>Yönetici onaylar

Kaynak yöneticileri yenileme isteği e-posta bildirimi veya Azure Portalı'ndan PIM erişme ve "Onayla istekleri" sol gezinti menüsünde seçerek bağlantıyı erişebilirsiniz.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Yönetici onaylama veya reddetme seçtiğinde isteğinin ayrıntılarını yanı sıra bir alan için denetim günlüklerini gerekçe gösterilmektedir.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Kaynak yöneticileri rol ataması yenileme isteği onaylama sırasında yeni bir başlangıç ve bitiş tarih-saat ve atama türü girmeniz gerekir. 

### <a name="admin-renew"></a>Yönetici yenileme

Kaynak yöneticileri süresi dolan rol atamalarını bir kaynağın sol gezinti menüsünde üyeleri sekmesinden veya içinden yenileme süresi doldu roller sekmesini kaynak rolünün.

Üyeleri ekranından süresi doldu rolleri tüm süresi dolan rol atamalarını listesini görüntülemek için seçin.

![](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_member_blade.png)

## <a name="next-steps"></a>Sonraki adımlar

[Etkinleştirmek için onay iste](pim-resource-roles-approval-workflow.md)

[Bir rolü etkinleştirmesi](pim-resource-roles-use-the-audit-log.md)


