---
title: Azure PIM kaynak RBAC genel bakış | Microsoft Docs
description: Terminoloji ve bildirimler gibi PIM'de RBAC özelliğine genel bakış edinin
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: 7cf628495a79fe775528080ae6ec31df8e9a0f37
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447598"
---
# <a name="pim-for-azure-resources"></a>Azure kaynakları için PIM

İle Azure Active Directory Privileged Identity Management (PIM), artık yönetebilir, denetleme ve izleme, kuruluşunuzda Azure kaynaklarına erişim. Bu abonelik, kaynak grupları ve bile sanal makineleri içerir. Herhangi bir kaynağa Azure rol tabanlı erişim denetimi (RBAC) işlevselliği yararlanan Azure portalındaki Azure AD PIM sunduğu tüm harika güvenlik ve yaşam döngüsü yönetim özellikleri yararlanabilir ve getirmeyi planlıyoruz bazı harika yeni özellikler Yakında Azure AD rolleri. 

## <a name="pim-for-azure-resources-helps-resource-administrators"></a>Azure kaynakları için PIM kaynak yöneticileri yardımcı olur.

- Hangi kullanıcıların ve grupların yönettiğiniz Azure kaynakları için rolleri atanmış bakın
- İsteğe bağlı, "yalnızca zamanında" abonelik, kaynak grupları ve diğer kaynakları yönetmek için erişimi etkinleştir
- Yeni zaman sınırı atama ayarları ile otomatik olarak atanan kullanıcılar/gruplar kaynak erişim süre sonu
- Hızlı Görevler veya nöbet zamanlamalar için geçici kaynak erişim atama
- Herhangi bir yerleşik veya özel rolü şirket kaynak erişimi için multi-Factor Authentication yürürlüğe 
- Etkin bir kullanıcının oturumu sırasında kaynak bağıntılı erişim kaynak etkinliği hakkında raporlar alın
- Yeni kullanıcılar veya gruplar kaynak erişimi atandığında ve bunlar uygun atamalar'ı etkinleştirdiğinizde uyarılar alın

Azure AD PIM (ancak bunlarla sınırlı olmamak üzere), özel (RBAC) rolleri yanı sıra yerleşik Azure kaynak rolleri yönetebilirsiniz:

- Sahip
- Kullanıcı Erişimi Yöneticisi
- Katılımcı
- Güvenlik Yöneticisi
- Güvenlik Yöneticisi ve daha fazlası

>[!NOTE]
Kullanıcılar veya sahibi ya da kullanıcı erişimi yöneticisi rolü ve Azure AD'de abonelik yönetimini etkinleştirme genel Yöneticiler atanmış bir grubun üyelerinin kaynağa yöneticilerdir. Bu yöneticileri, Rolleri Ata, rol ayarlarını yapılandırma ve Azure kaynakları için PIM kullanarak erişimi gözden geçirin. Listesini görüntülemek [Azure kaynakları için yerleşik roller](../../role-based-access-control/built-in-roles.md).

## <a name="tasks"></a>Görevler

PIM rollerini etkinleştirmek için bekleyen etkinleştirmeleri/istekler, bekleyen onayları görüntülemek için uygun erişim sağlar (için [Azure AD Dizin rolleri](azure-ad-pim-approval-workflow.md)) ve sol gezinti menüsünde görevleri bölümünden yanıt bekleyen inceler.

Görevleri menü öğelerinden birini genel bakış giriş noktasından erişirken, ekranda hem Azure AD Dizin rolleri hem de Azure kaynak rolleri için sonuçları içerir. 

![](media/azure-pim-resource-rbac/role-settings-details.png)

Rollerim, etkin ve uygun rol atamaları Azure AD Dizin rolleri ve Azure kaynak rolleri için bir listesini içerir.

## <a name="activate-roles"></a>Rollerini etkinleştir

Azure kaynakları için rol etkinleştirme, etkinleştirme için gelecekteki bir tarih/saat zamanlamak ve bir belirli etkinleştirme süresi üst sınırı (yönetici tarafından yapılandırılır) içinde seçmek uygun rolü üyeleri izin veren yeni bir deneyim sağlanıyor. Hakkında bilgi edinin [Azure AD rolleri etkinleştirme](../active-directory-privileged-identity-management-how-to-activate-role.md).

![](media/azure-pim-resource-rbac/contributor.png)

Etkinleştirme menüden istenen başlangıç tarihi ve saati rolü etkinleştirmek için giriş. İsteğe bağlı olarak etkinleştirme süresini azaltmak (sürenin uzunluğunu rol etkin) ve gerekirse; bir gerekçe girin Etkinleştir'e tıklayın.

Başlangıç tarihi ve saati değiştirilmez, rol saniyeler içinde etkinleştirilecektir. Rol etkinleştirme başlık iletisi Sitem rollerini sayfasında üzere kuyruğa alınan görürsünüz. Bu ileti yenile düğmesine tıklayın.

![](media/azure-pim-resource-rbac/my-roles.png)

Etkinleştirme için gelecekteki bir tarih saat zamanlanırsa, bekleyen istek sol gezinti menüsünde bekleyen istekler sekmede görünür. Rol etkinleştirme artık gerekli olması durumunda kullanıcı sayfanın sağ tarafında İptal düğmesini tıklatarak istek iptal edebilirsiniz.

![](media/azure-pim-resource-rbac/pending-requests.png)

## <a name="discover-and-manage-azure-resources"></a>Bulma ve Azure kaynaklarını yönetme

Bulmak ve rolleri yönetmek için bir Azure kaynağı için sol gezinti menüsünde Yönet sekmesinin altındaki Azure kaynaklarını seçin. Filtre veya arama çubuğuna bir kaynağı bulmak için sayfanın en üstündeki kullanın.

![](media/azure-pim-resource-rbac/azure-resources.png)

## <a name="resource-dashboards"></a>Kaynak panolar

Yönetici görünümü Pano dört birincil bileşenleri içerir. Son yedi gün içindeki kaynak rol etkinleştirmeleri grafiksel gösterimi. Bu veriler, seçili kaynak için kapsamlı ve en sık kullanılan rolleri (sahibi, katkıda bulunan, kullanıcı erişimi Yöneticisi) ve birleşik tüm roller için etkinleştirme görüntüler.

Etkinleştirme graf sağında, hem kullanıcılar hem de grupları atama türü tarafından rol atamalarını dağılımını gösteren iki grafik var. Bir dilimi grafiğin seçilmesi, yüzde (veya tersi) değeri değiştirir.

![](media/azure-pim-resource-rbac/admin-view.png)

Grafikleri (solda) son 30 gün ve roller (Azalan) toplam atamaları göre sıralanmış listesini üzerinde olan yeni rol atamaları kullanıcılar ve gruplar sayısını görebilirsiniz.

![](media/azure-pim-resource-rbac/role-settings.png)

## <a name="manage-role-assignments"></a>Rol atamalarını yönetme

Yöneticiler, rol ya da üyeleri sol gezinti bölmesinden seçerek rol atamalarını yönetebilir. Rolleri seçme, yöneticilerin üyeleri kaynak için tüm kullanıcı ve Grup rol atamaları görüntülerken yönetim görevlerinin belirli bir role kapsam olanak sağlar.

![](media/azure-pim-resource-rbac/roles.png)

![](media/azure-pim-resource-rbac/members.png)

>[!NOTE]
Rol etkinleştirme bekleyen varsa, sayfanın en üstündeki üyelik görüntülerken bir bildirim başlığı görüntülenir.

## <a name="assign-roles"></a>Roller atama

Bir kullanıcı veya grup için bir rol atamak için (rolleri görüntülerken) rolü seçin ve Ekle Eylem çubuğu'ndan (üyeleri görünümü varsa) tıklayın.

![](media/azure-pim-resource-rbac/members2.png)

>[!NOTE]
Bir kullanıcı veya grup üyeleri sekmesinden ekliyorsanız bir kullanıcı veya grup seçebilmeniz için önce Rol Ekle Menüsü'nden seçmeniz gerekir.

![](media/azure-pim-resource-rbac/select-role.png)

Bir kullanıcı veya grup, dizinden seçin.

![](media/azure-pim-resource-rbac/choose.png)

Açılan menüden uygun atama türü seçin. 

**Yalnızca zaman atama içinde:** kullanıcı veya grup üyeleri rolüne uygun ancak değil kalıcı erişimi olan belirli bir dönem için zaman veya süresiz olarak sağladığı (Rol ayarlarında yapılandırılmışsa). 

**Doğrudan atamayı:** (kalıcı erişim bilinir) rol ataması etkinleştirmek kullanıcı veya grup üyeleri gerektirmez. Microsoft, görev tamamlandığında burada erişim gerekli olmayacak nöbet kaydırmalar ya da zaman hassas etkinlikleri gibi kısa süreli kullanım için doğrudan atama kullanılmasını önerir.

![](media/azure-pim-resource-rbac/membership-settings.png)

Atama türü açılır altında bir onay kutusu, atama kalıcı olup olmayacağını belirtin (zaman atama/kalıcı olarak etkin anlık için doğrudan atamayı etkinleştirmek kalıcı olarak uygun) sağlar. Belirli atama süresi belirtmek için onay kutusunun seçimini kaldırın ve başlangıç değiştirmek ve/veya tarih ve saat alanları bitmelidir.

>[!NOTE]
Onay kutusunu başka bir yöneticinin her bir atama türü için en fazla atama süresi rol ayarlarında belirtilen değiştirilemeyen olabilir.

![](media/azure-pim-resource-rbac/calendar.png)

## <a name="view-activation-and-azure-resource-activity"></a>Etkinleştirme ve Azure kaynak etkinliği görüntüle

Belirli bir kullanıcı çeşitli kaynaklardaki sürdü hangi eylemleri görmek için ihtiyacınız olayda bir belirtilen etkinleştirme süresi (için uygun olan kullanıcılar) ile ilişkili Azure kaynak etkinliği gözden geçirebilirsiniz. Bir kullanıcı üye görünümünden veya belirli bir roldeki üyelerin listesi seçerek başlatın. Sonuç, tarihe göre Azure kaynaklarına kullanıcının eylemleri ve bu aynı süre boyunca yeni rol etkinleştirmeleri grafik bir görünümünü görüntüler.

![](media/azure-pim-resource-rbac/user-details.png)

Özel rol etkinleştirme seçerek rol etkinleştirme ayrıntıları ve bu kullanıcı etkin olduğu sırada gerçekleşen karşılık gelen Azure kaynak etkinliği gösterir.

![](media/azure-pim-resource-rbac/audits.png)

## <a name="modify-existing-assignments"></a>Mevcut atamalarını değiştirin

Kullanıcı/Grup ayrıntı görünümü mevcut atamaları değiştirmek için sayfanın üstünde Eylem çubuğu ayarları değiştir seçin. Atama türü, yalnızca zaman atama içinde veya doğrudan atama değiştirin.

![](media/azure-pim-resource-rbac/change-settings.png)

## <a name="review-who-has-access-in-a-subscription"></a>Bir abonelikte erişimi olan gözden geçirme

Aboneliğinizdeki rol atamalarını gözden geçirmesini üyeler sekmesini seçin sol gezinti bölmesinden veya rolleri seçin ve üyeleri gözden geçirmek için belirli bir rol seçin. 

Gözden geçirme mevcut erişim gözden geçirmeleri görüntülemek ve yeni bir gözden geçirme oluşturmak için Ekle'yi seçin için Eylem çubuğu seçin.

![](media/azure-pim-resource-rbac/owner.png)

[Erişim gözden geçirmeleri hakkında daha fazla bilgi edinin](../active-directory-privileged-identity-management-how-to-perform-security-review.md)

>[!NOTE]
İncelemeler, şu anda yalnızca abonelik kaynak türleri için desteklenir.

## <a name="configure-role-settings"></a>Rol ayarlarını yapılandırma

Rol ayarları yapılandırmayı atamalar PIM ortamında uygulanan varsayılan değerleri tanımlayın. Bunlar kaynağınızın tanımlamak için sol gezinti ya da geçerli seçeneklerini görüntülemek için herhangi bir rol eylem çubuğunda rol ayarlar düğmesinden rol ayarları sekmesini seçin.

Sayfanın üstünde eylem çubuğunda Düzen'i tıklatarak, her ayar değiştirmenize olanak sağlar.

![](media/azure-pim-resource-rbac/owner.png)

![](media/azure-pim-resource-rbac/owner02.png)

Son güncelleştirme tarihi zamanı ve ayarlarında değişiklik yapıldığı yönetici dahil olmak üzere rol Ayarları sayfasında ayarlarında yapılan değişiklikler kaydedilir.

![](media/azure-pim-resource-rbac/role-settings-02.png)

## <a name="resource-audit"></a>Kaynak denetimi

Kaynak Denetim tüm rol etkinliklerin kaynağın görünüm sağlar. Önceden tanımlanmış tarih veya özel aralığı kullanarak bilgileri filtreleyebilirsiniz.
![](media/azure-pim-resource-rbac/last-day.png) Kaynak Denetim, ayrıca bir kullanıcının etkinlik ayrıntılarını görüntülemek için hızlı erişim sağlar. Görünümü'nde "rolü etkinleştir" tüm eylemleri belirli sahibinin kaynak etkinliği bağlantılardır.
![](media/azure-pim-resource-rbac/resource-audit.png)

## <a name="just-enough-administration"></a>Yeterli yönetim

Yeterli yönetim (JEA) en iyi uygulamalar, kaynak rol atamaları kullanarak Azure kaynakları için PIM ile basit bir işlemdir. Kullanıcı ve Grup üyeleri atamaları Azure abonelik veya kaynak grupları ile birlikte, mevcut rol ataması azaltılmış bir kapsamda etkinleştirebilirsiniz. 

Arama sayfasından yönetmeniz gereken alt kaynak bulun.

![](media/azure-pim-resource-rbac/azure-resources-02.png)

Rollerim sol gezinti menüsünden ve etkinleştirmek için uygun rolü seçin. Rol kaynak grubu yerine abonelik aşağıda gösterildiği gibi atanan bu yana, atama türü bildirimi devralınır.

![](media/azure-pim-resource-rbac/my-roles-02.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için yerleşik roller](../../role-based-access-control/built-in-roles.md)
- Hakkında bilgi edinin [Azure AD rolleri etkinleştirme](../active-directory-privileged-identity-management-how-to-activate-role.md)
- [PIM onayı iş akışı](azure-ad-pim-approval-workflow.md)
