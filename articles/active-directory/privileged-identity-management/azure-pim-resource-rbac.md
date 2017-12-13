---
title: "Azure PIM kaynak RBAC genel bakış | Microsoft Docs"
description: "RBAC özelliğine genel bakış içinde terminoloji ve bildirimler gibi PIM Al"
services: active-directory
documentationcenter: 
author: barclayn
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2017
ms.author: barclayn
ms.openlocfilehash: 19715f800e7d8d40336d8e9fa3bf8073795dce5b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="pim-for-azure-resources-preview"></a>PIM için Azure kaynaklarını (Önizleme)

İle Azure Active Directory ayrıcalıklı Kimlik Yönetimi (PIM), artık yönetebilir, denetleme ve izleme kuruluşunuz içinde (Önizleme) Azure kaynaklarına erişim. Bu abonelik, kaynak grupları ve hatta sanal makineler içerir. Azure rol tabanlı erişim denetimi (RBAC) işlevselliği yararlanır Azure portalındaki herhangi bir kaynağa sunmak için Azure AD PIM sahip tüm harika güvenlik ve yaşam döngüsü yönetimi özellikleri yararlanabilir ve getirmek planlıyoruz bazı harika yeni özellikler Yakında Azure AD rolleri için. 

## <a name="pim-for-azure-resources-preview-helps-resource-administrators"></a>PIM Azure kaynakları (Önizleme) için kaynak yöneticileri yardımcı olur.

- Hangi kullanıcıların ve grupların yönettiğiniz Azure kaynakları için rolleri atanmış bakın
- İsteğe bağlı, abonelikler, kaynak grupları ve daha fazlasını gibi kaynakları yönetmek için "tam zamanında" erişimi etkinleştir
- Atanmış kullanıcılar/gruplar kaynak erişimini otomatik olarak yeni ayarlarla zaman sınırlı atama süresi dolsun
- Hızlı görevleri ya da çağrısı zamanlamaları geçici kaynak erişimi atayın
- Herhangi bir yerleşik veya özel rol kaynak erişim için çok faktörlü kimlik doğrulamasını zorunlu 
- Bir kullanıcının etkin oturumu sırasında kaynak bağıntılı erişim kaynak etkinliği hakkında raporlar alın
- Kaynak erişim yeni kullanıcılar veya gruplar atandığında ve uygun atamaları etkinleştirdiğinizde uyarıları alma

Azure AD PIM (ancak bunlarla sınırlı değil) dahil olmak üzere özel (RBAC) rolleri yanı sıra yerleşik Azure kaynak rolleri yönetebilir:

- Sahip
- Kullanıcı Erişimi Yöneticisi
- Katılımcı
- Güvenlik Yöneticisi
- Güvenlik Yöneticisi ve daha fazla bilgi

>[!NOTE]
Kullanıcılar veya sahibi ya da kullanıcı erişimi yöneticisi rolü ve Azure AD içinde abonelik yönetimini etkinleştirme genel Yöneticiler atanmış bir grubun üyeleri kaynak yöneticileri olur. Bu yöneticileri, Rolleri Ata, rol ayarlarını yapılandırmak ve Azure kaynakları için PIM kullanarak erişimi gözden geçirin. Listesini görüntülemek [Azure kaynakları için yerleşik roller](../role-based-access-built-in-roles.md).

## <a name="tasks"></a>Görevler

PIM rolleri etkinleştirmek için bekleyen etkinleştirmeleri/istekler, bekleyen onayları görüntülemek için uygun erişim sağlar (için [Azure AD directory rolleri](azure-ad-pim-approval-workflow.md)) ve yanıtınız sol gezinti menüsünde görevleri bölümünden bekleyen inceler.

Görevler menüsü öğeleri genel bakış giriş noktasından erişirken, sonuçta elde edilen görünümü sonuçları Azure AD directory roller ve Azure kaynak rolleri (Önizleme) içerir. 

![](media/azure-pim-resource-rbac/role-settings-details.png)

My rolleri Azure AD directory rolleri ve Azure kaynak rolleri (Önizleme), etkin ve uygun rol atamalarını listesini içerir.

## <a name="activate-roles"></a>Rollerini etkinleştir

(Önizleme) Azure kaynakları için rol etkinleştirme, etkinleştirme için gelecekteki bir tarih/saat zamanlayın ve belirli etkinleştirme süresi (yöneticiler tarafından yapılandırılmış) maksimum içinde seçmek uygun Rol üyeleri izin veren yeni bir deneyim sunar. Hakkında bilgi edinin [burada Azure AD rolleri etkinleştirme](../active-directory-privileged-identity-management-how-to-activate-role.md).

![](media/azure-pim-resource-rbac/contributor.png)

İstenen başlangıç tarihini ve saatini rolünü etkinleştirmek için etkinleştirme menüsünden girin. İsteğe bağlı olarak etkinleştirme süresini azaltmak (süre rolü etkin) ve gerekirse; bir gerekçe girin Etkinleştir'i tıklatın.

Başlangıç tarihi ve saati değiştirilmeyen, rol saniye içinde etkinleştirilir. Sitem rollerini sayfasında etkinleştirme başlık iletisi için bir rol sıraya görürsünüz. Bu iletiyi temizlemek için Yenile düğmesini tıklatın.

![](media/azure-pim-resource-rbac/my-roles.png)

Etkinleştirme bir gelecek tarih zaman için planlandıysa bekleyen istek sol gezinti menüsünde bekleyen istekler sekmesinde görünür. Rol etkinleştirme artık gerekli olması durumunda, kullanıcı isteği sayfasının sağ tarafında iptal düğmesine tıklayarak iptal edebilirsiniz.

![](media/azure-pim-resource-rbac/pending-requests.png)

## <a name="discover-and-manage-azure-resources"></a>Keşfetmek ve Azure kaynaklarını yönetmek

Bul ve rolleri için bir Azure kaynağı yönetmek için sol gezinti menüsünde Yönet sekmesi altında Azure kaynakları (Önizleme) seçin. Bir kaynak bulmak için sayfanın en üstünde filtreleri veya arama çubuğu'ı kullanın.

![](media/azure-pim-resource-rbac/azure-resources.png)

## <a name="resource-dashboards"></a>Kaynak panoları

Yönetici görünümünü Pano dört birincil bileşenleri içerir. Kaynak rol etkinleştirme grafik gösterimi son yedi gün içinde. Bu veriler seçilen kaynağa kapsamlı ve en yaygın rolleri (sahibi, katkıda bulunan, kullanıcı erişimi Yöneticisi) ve birleşik tüm roller için etkinleştirme görüntüler.

Grafiğin sağ tarafında etkinleştirmeleri için kullanıcılar ve gruplar için atama türü tarafından rol atamalarını dağıtımını görüntülemek iki grafik var. Grafiğin bir dilim seçme değerini yüzde (veya tersi) değiştirir.

![](media/azure-pim-resource-rbac/admin-view.png)

Grafikler, yeni rol atamaları olan kullanıcılar ve gruplar sayısı (sol) son 30 gün ve rolleri (Azalan) toplam atamaları göre sıralanan listesini üzerinden bakın.

![](media/azure-pim-resource-rbac/role-settings.png)

## <a name="manage-role-assignments"></a>Rol atamalarını yönetme

Yöneticiler, sol gezinti bölmesinden rol ya da üyeleri seçerek rol atamalarını yönetebilir. Rolleri seçme üyeleri gösterirken, kaynak için tüm kullanıcı ve Grup rol atamalarını yönetim görevlerini belirli bir rol için kapsamı yöneticilerinin olanak tanır.

![](media/azure-pim-resource-rbac/roles.png)

![](media/azure-pim-resource-rbac/members.png)

>[!NOTE]
Bir rol etkinleştirme bekleyen varsa, sayfanın en üstünde üyelik görüntülerken bir bildirim başlığı görüntülenir.

## <a name="assign-roles"></a>Rolleri Ata

Bir kullanıcı veya grup için bir rol atamak için (rolleri görüntülerken) rolünü seçin veya Ekle Eylem çubuğu'ndan (üyeleri görünümü varsa) tıklayın.

![](media/azure-pim-resource-rbac/members2.png)

>[!NOTE]
Bir kullanıcı veya grup üyeleri sekmesinden ekleme, kullanıcı veya grup seçebilmeniz için önce Rol Ekle menüsünden seçmeniz gerekir.

![](media/azure-pim-resource-rbac/select-role.png)

Bir kullanıcı veya grup dizinden seçin.

![](media/azure-pim-resource-rbac/choose.png)

Uygun atama açılır menüsünden seçin. 

**Yalnızca zaman ataması içinde:** kullanıcı veya grup üyeleri rolüne uygun ancak kalıcı değil erişimi olan belirli bir dönem için zaman veya süresiz olarak sağladığı (Rol ayarlarında yapılandırılmışsa). 

**Doğrudan atama:** (sürekli erişim olarak da bilinir) rol ataması etkinleştirmek kullanıcının veya grubun üyeleri gerektirmez. Microsoft görev tamamlandığında, burada erişim gerekli olmayacak çağrısı kaydırmalar veya zaman hassas etkinlikleri gibi kısa süreli kullanım için doğrudan atanmasına kullanılmasını önerir.

![](media/azure-pim-resource-rbac/membership-settings.png)

Atama türü açılır altında bir onay kutusu atama kalıcı olup olmayacağını belirtin (saat atama/kalıcı olarak etkin, sadece doğrudan ataması için etkinleştirmek kalıcı olarak uygun) sağlar. Belirli atama süresini belirtmek için onay kutusunun seçimini kaldırın ve başlangıç değiştirmek ve/veya tarih ve saat alanları bitmelidir.

>[!NOTE]
Onay kutusu başka bir yöneticinin her atama türü yüksek atama süresince rol ayarlarında belirtilen, değiştirilemeyen olabilir.

![](media/azure-pim-resource-rbac/calendar.png)

## <a name="view-activation-and-azure-resource-activity"></a>Etkinleştirme ve Azure kaynak etkinliği görüntüleme

Belirli bir kullanıcı çeşitli kaynaklara sürdü hangi eylemleri görmek gerekmesi durumunda belirtilen etkinleştirme süresi (uygun kullanıcılar için) ile ilişkili Azure kaynak etkinliği gözden geçirebilirsiniz. Bir kullanıcı üyeleri görünümünden veya belirli bir roldeki üyelerin listesi seçerek başlatın. Sonuç tarihe göre Azure kaynaklarına kullanıcının eylemleri ve aynı bu süre boyunca yeni rol etkinleştirmeleri grafik bir görünümünü görüntüler.

![](media/azure-pim-resource-rbac/user-details.png)

Belirli bir rol etkinleştirme seçerek rol etkinleştirme ayrıntıları ve bu kullanıcının etkinken oluşan karşılık gelen Azure kaynak etkinliği gösterir.

![](media/azure-pim-resource-rbac/audits.png)

## <a name="modify-existing-assignments"></a>Varolan atamalarını değiştirin

Kullanıcı/Grup ayrıntı görünümünden varolan atamalarını değiştirmek için sayfanın üst kısmındaki eylem çubuğunda değişiklik ayarlarını seçin. Atama türü yalnızca zaman atama içinde veya doğrudan atama olarak değiştirin.

![](media/azure-pim-resource-rbac/change-settings.png)

## <a name="review-who-has-access-in-a-subscription"></a>Bir abonelikte erişebilen gözden geçirme

Rol atamalarını aboneliğinizde gözden geçirmek için sol gezinti bölmesinden üyeler sekmesini seçin veya rolleri seçin ve üyeleri gözden geçirmek için belirli bir rol seçin. 

Gözden geçirme mevcut erişim incelemeleri görüntülemek ve yeni bir gözden geçirme oluşturmak için Ekle'yi seçin eylem çubuğundan seçin.

![](media/azure-pim-resource-rbac/owner.png)

[Erişim gözden geçirme hakkında daha fazla bilgi edinin](../active-directory-privileged-identity-management-how-to-perform-security-review.md)

>[!NOTE]
İncelemeleri şu anda yalnızca abonelik kaynak türleri için desteklenir.

## <a name="configure-role-settings"></a>Rol ayarlarını yapılandırma

Rol ayarlarını yapılandırmaya atamaları PIM ortamında uygulanan varsayılan değerleri tanımlayın. Bu, kaynak için tanımlamak için sol gezinti ya da geçerli seçeneklerini görüntülemek için herhangi bir rolü eylem çubuğunda rol ayarlar düğmesinden rol ayarları sekmesini seçin.

Sayfanın üstünde eylem çubuğunda Düzen'i tıklatarak, her ayar değiştirmenize olanak sağlar.

![](media/azure-pim-resource-rbac/owner.png)

![](media/azure-pim-resource-rbac/owner02.png)

Son güncelleştirilme tarihi zamanı ve ayarlarında değişiklik yönetici de dahil olmak üzere rol Ayarları sayfasında ayarlarında yapılan değişiklikler kaydedilir.

![](media/azure-pim-resource-rbac/role-settings-02.png)

## <a name="resource-audit"></a>Kaynak Denetim

Kaynak Denetim tüm rol etkinlik kaynak için bir görünümünü sağlar. Önceden tanımlanmış tarih veya özel aralığı kullanarak bilgi filtreleyebilirsiniz.
![](media/azure-pim-resource-rbac/last-day.png)Kaynak Denetim ayrıca kullanıcının Etkinlik ayrıntıları görüntülemek için hızlı erişim sağlar. Görünümü'nde "rolünü etkinleştirmek" tüm eylemler belirli istek sahibinin kaynak etkinliği bağlantılardır.
![](media/azure-pim-resource-rbac/resource-audit.png)

## <a name="just-enough-administration"></a>Tam yetecek kadar yönetim

Kaynak rol atamalarınızı ile tam olarak yeterli yönetim (JEA) en iyi yöntemler kullanarak Azure kaynakları için PIM basittir. Kullanıcı ve Grup üyeleri ile Azure abonelikleri veya kaynak grupları atamalarını kendi mevcut rol ataması azaltılmış bir kapsamda etkinleştirebilirsiniz. 

Arama sayfasından yönetmeniz gereken bağımlı kaynak bulunamıyor.

![](media/azure-pim-resource-rbac/azure-resources-02.png)

My rolleri sol gezinti menüsünde ve etkinleştirmek için uygun rolü seçin. Rolü kaynak grubu yerine abonelik aşağıda gösterildiği gibi atandı beri bildirimi atama türü devralınır.

![](media/azure-pim-resource-rbac/my-roles-02.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için yerleşik roller](../role-based-access-built-in-roles.md)
- Hakkında bilgi edinin [burada Azure AD rolleri etkinleştirme](../active-directory-privileged-identity-management-how-to-activate-role.md)
- [PIM onay iş akışları](azure-ad-pim-approval-workflow.md)
