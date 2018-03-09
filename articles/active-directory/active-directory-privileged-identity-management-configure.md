---
title: "Azure AD Privileged Identity Management yapılandırma | Microsoft Docs"
description: "Azure AD Privileged Identity Management nedir ve PIM bulut güvenliğinizi artırmak için nasıl kullanılacağını açıklayan bir konu."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: c548ed2e-06e3-4eaf-a63d-0f02ee72da25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 827e3521be8918f4de00113fd9eaf4e01679cac5
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management nedir?

Azure Active Directory (AD) Privileged Identity Management sayesinde kuruluşunuz içindeki erişimi yönetebilir, denetleyebilir ve izleyebilirsiniz. Bu kaynaklara erişimi Azure AD, Azure kaynakları (Önizleme) içerir ve diğer Microsoft Online Services ister Office 365 veya Microsoft Intune.

> [!NOTE]
> Privileged Identity Management'ı kiracınız için etkinleştirdiğinizde, geçerli Azure AD Premium P2 veya Enterprise Mobility + güvenlik E5 ücretli veya deneme lisansı ile etkileşime giren veya hizmetinden bir avantajı alan her kullanıcı için gereklidir. Örnekler kullanıcıları/kullanıcılar > olan bir grup:
>
>- Ayrıcalıklı rol yöneticisi rolüne atanan 
>- PIM yönetilebilir diğer dizin rollere uygun atanmış 
>- PIM istekleri Onayla/Reddet yapılamıyor 
>- Bir kez veya doğrudan (zaman tabanlı) atamaları yalnızca Azure kaynak rolüne atanan  
>- Atanmış bir erişim gözden geçirme
>
>Daha fazla bilgi için bkz. [Azure Active Directory sürümleri](active-directory-editions.md).

Kuruluşlar, çünkü bu erişim alma kötü niyetli bir kullanıcı ya da yanlışlıkla hassas kaynak etkileyen yetkili bir kullanıcı olasılığını azaltır bilgi veya kaynaklarını güvenli hale getirmek için erişime sahip kişilerin sayısını en aza indirmek istediğiniz.  Ancak, kullanıcılar hala Azure AD'de ayrıcalıklı işlemleri gerçekleştirmek gereken Azure, Office 365 ya da SaaS uygulamaları. Kuruluşlar, abonelikleri ve Azure AD gibi Azure kaynakları için kullanıcılar ayrıcalıklı erişim verebilirsiniz. Yönetici ayrıcalıklarını bu kullanıcıların ne yaptıklarını gözetim gerek yoktur. Azure AD Privileged Identity Management erişim hakları kötüye kullanımını veya aşırı, gereksiz riskini azaltmaya yardımcı olur.

Azure AD Privileged Identity Management kuruluşunuz yardımcı olur:

- Hangi kullanıcıların hangi kullanıcıların yönetici rolleri Azure AD'de atanmış yanı sıra (Önizleme), Azure kaynaklarınızı yönetmek için ayrıcalıklı rolleri atanmış bakın
- İsteğe bağlı, "tam zamanında" yönetimsel erişimi Office 365 ve Intune gibi Microsoft Çevrimiçi Hizmetler ve abonelikler, kaynak grupları ve kaynakların sanal makineler gibi Azure kaynaklarını (Önizleme) etkinleştir 
-   Hangi değişiklikleri Yöneticiler Azure kaynaklarını (Önizleme) yapılan dahil olmak üzere yönetici etkinleştirme Geçmişi'ne bakın
- Yönetici atamaları değişiklikler hakkında uyarı alın
- Ayrıcalıklı Azure AD yönetim rolleri (Önizleme) etkinleştirmek için onay iste 
- Yönetici rolleri üyeliğini gözden geçirin ve kullanıcıların bir gerekçe sürekli üyelik iste

Azure AD içinde Azure AD Privileged Identity Management yerleşik olarak atanan kullanıcılar yönetebilmeniz için genel yönetici gibi Azure AD kuruluş rolleri. Azure'da, kullanıcıları ve grupları sahibi veya katkıda dahil olmak üzere Azure RBAC rolleri atanan Azure AD Privileged Identity Management yönetebilirsiniz.

## <a name="just-in-time-administrator-access"></a>Yalnızca süresi yönetici erişimi

Tarihsel olarak, bir kullanıcı bir yönetici rolü Azure portalı, diğer Microsoft Online Services portalı veya Windows PowerShell'de Azure AD cmdlet'leri aracılığıyla atayabilir. Sonuç olarak, bu kullanıcının hale bir **kalıcı yönetici**, atanan rollerinin her zaman etkin. Azure AD Privileged Identity Management kavramını sunmaktadır bir **uygun yönetici**. Uygun Yöneticiler ayrıcalıklı olması gereken kullanıcılar şimdi yapıp ancak günlük değil, her gün erişimi olması gerekir. Etkinleştirme işlemini tamamlamak ve önceden belirlenen bir süre için etkin bir yönetim hale sonra kullanıcı, erişmesi kadar rolü etkin değil. Daha da fazla kuruluşlar, azaltma veya ayrıcalıklı rolleri "durumu yönetici erişimi" ortadan kaldırmak için bu yaklaşımı kullanmak seçme.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Privileged Identity Management dizininiz için etkinleştirme

Azure AD Privileged Identity Management kullanmaya başlayabilmeniz için [Azure portal](https://portal.azure.com/).

> [!NOTE]
> Bir kurumsal hesap ile genel yönetici olmanız gerekir (örneğin, @yourdomain.com), bir Microsoft hesabı (örneğin, @outlook.com), bir dizin için Azure AD Privileged Identity Management etkinleştirmek için.

1. [Azure portalında](https://portal.azure.com/) dizininizin genel yöneticisi olarak oturum açın.
2. Kuruluşunuz birden fazla dizine sahipse Azure portalının sağ üst köşesinde kullanıcı adınızı seçin. Azure AD Privileged Identity Management burada kullanacağı dizini seçin.
3. **Tüm hizmetler** seçeneğini belirleyin ve **Azure AD Privileged Identity Management** araması yapmak için Filtre metin kutusunu kullanın.
4. **Panoya sabitle**'yi işaretleyin ve ardından **Oluştur**’a tıklayın. Privileged Identity Management uygulaması açılır.

Dizininizde Azure AD Privileged Identity Management kullanmak için ilk kişi olduğunuz ve Azure AD directory rollere gidin ve Azure AD directory rollere gidin bir [Güvenlik Sihirbazı](active-directory-privileged-identity-management-security-wizard.md) ilk yol gösterir atama deneyimi. Bundan sonra otomatik olarak hale ilk **Güvenlik Yöneticisi** ve **ayrıcalıklı Rol Yöneticisi** dizin.

Azure AD rolleri için ayrıcalıklı Rol Yöneticisi rolünde olan bir kullanıcı Azure AD PIM diğer yöneticiler atamaları yönetebilirsiniz. Yapabilecekleriniz [diğer kullanıcılara PIM Dizin rolleri yönetme olanağı vermek](active-directory-privileged-identity-management-how-to-give-access-to-pim.md). Genel Yöneticiler, güvenlik yöneticileri ve güvenlik okuyucular Azure AD rol atamalarını Azure AD PIM görüntüleyebilirsiniz.
Azure RBAC rolleri için yalnızca bir abonelik Yöneticisi, bir kaynak sahibi veya bir kaynak kullanıcı erişimi Yöneticisi diğer yöneticileri Azure AD PIM atamaları yönetebilirsiniz.  Ayrıcalıklı rol Yöneticiler, güvenlik yöneticileri veya güvenlik okuyucular kullanıcılar varsayılan olarak Azure AD PIM Azure RBAC rol atamalarını görüntüleme erişimine sahiptir.

## <a name="privileged-identity-management-overview-entry-point"></a>Privileged Identity Management genel bakış (giriş noktası)

Azure AD Privileged Identity Management Azure kaynakları (Önizleme) Azure AD directory rolleri ve rol yönetimini destekler. Azure kaynakları için rolleri işlevi, Azure AD'de yönetici rollerini farklı. Azure kaynak rolleri atanmışlarsa kaynak ve (devralma da bilinir) kaynak hiyerarşideki tüm alt kaynaklar için ayrıntılı izinler sağlar. [Daha fazla hakkında RBAC, kaynak hiyerarşisi ve devralma öğrenin](role-based-access-control-configure.md). PIM hem Azure AD directory rolleri için ve Azure kaynakları (Önizleme) PIM genel bakış giriş noktası sol gezinti menüsünün Yönet bölümünde altındaki ilgili bağlantıyı erişerek yönetilebilir.

PIM rolleri etkinleştirmek için bekleyen etkinleştirmeleri/istekler, bekleyen onayları (Azure AD directory roller için) görüntülemek için uygun erişim sağlar ve yanıtınız sol gezinti menüsünde görevleri bölümünden bekleyen inceler.

Görevler menüsü öğeleri genel bakış giriş noktasından erişirken, sonuçta elde edilen görünümü sonuçları Azure AD directory roller ve Azure kaynak rolleri (Önizleme) içerir.

![Hızlı başlangıç](./media/active-directory-privileged-identity-management-configure/quick-start.png)

My rolleri Azure AD directory rolleri ve Azure kaynak rolleri (Önizleme) etkin ve uygun rol atamalarını listesini içerir. [Uygun rol atamalarını etkinleştirme hakkında daha fazla bilgi](active-directory-privileged-identity-management-how-to-activate-role.md).

(Önizleme) Azure kaynakları için rol etkinleştirme, etkinleştirme için gelecekteki bir tarih/saat zamanlayın ve yöneticiler tarafından izin verilen maksimum içinde belirli etkinleştirme süresi seçmek uygun rolü üyelerinin sağlayan yeni bir deneyim sunar.

![](./media/active-directory-privileged-identity-management-configure/activations.png)

Zamanlanmış bir etkinleştirme artık gerekli olması durumunda sol gezinti menüsünde giderek bekleyen istek ve iptal düğmesi satır içi bu istek ile tıklatarak kullanıcılar kendi bekleyen isteği iptal edebilirsiniz.

![Bekleyen istekler](./media/active-directory-privileged-identity-management-configure/pending-requests.png)

## <a name="privileged-identity-management-admin-dashboard"></a>Privileged Identity Management Yönetim Panosu

Azure AD Privileged Identity Manager gibi önemli bilgiler sağlayan bir Yönetim Panosu sağlar:

* Güvenliğini artırmak için fırsatları noktası uyarıları
* Her bir ayrıcalıklı role atanan kullanıcıların sayısı  
* Uygun ve kalıcı admins sayısı
* Ayrıcalıklı rolü etkinleştirme dizininizde grafiği
*   Zaman içinde sadece, zaman sınırlı ve kalıcı sayısı Azure kaynak rolleri (Önizleme) atamaları
*   Son 30 gündeki (Azure kaynak rolleri) yeni rol atamaları olan kullanıcılar ve gruplar


![PIM Pano - ekran görüntüsü][2]

## <a name="privileged-role-management"></a>Ayrıcalıklı rol yönetimi

Azure AD Privileged Identity Management'ı ekleyerek veya kaldırarak her rol için Azure AD directory rolleri kalıcı veya uygun yöneticilere Yöneticiler yönetebilir. Azure kaynakları (Önizleme) için PIM ile sahipleri, kullanıcı erişim yöneticileri ve abonelik yönetimi olarak kiracısındaki etkinleştirmek genel Yöneticileri kullanıcıların veya grupların Azure kaynak rolleri için uygun olarak atayabilirsiniz (zaman içinde sadece erişim), ya da zaman sınırlı () bir başlangıç ve bitiş tarih/saat veya (rolü ayarlarında etkinse) kalıcı erişimle etkinleştirme) gerekli değildir.

![PIM Ekle/Kaldır yöneticileri - ekran görüntüsü][3]

## <a name="configure-the-role-activation-settings"></a>Rol etkinleştirme ayarlarını yapılandırma

Kullanarak [rol ayarlarını](active-directory-privileged-identity-management-how-to-change-default-settings.md) dahil olmak üzere Azure AD directory rolleri için uygun rol etkinleştirme özelliklerini yapılandırabilirsiniz:

* Rol etkinleştirme süresi süresi
* Rol etkinleştirme bildirimi
* Bir kullanıcı rolünü etkinleştirme işlemi sırasında sağlamak için gereken bilgileri
* Hizmet bileti veya olay numarası
* [Onay iş akışı gereksinimleri - Önizleme](./privileged-identity-management/azure-ad-pim-approval-workflow.md)

![PIM ayarları - yönetici etkinleştirme - ekran görüntüsü][4]

Görüntüde düğmeleri unutmayın **çok faktörlü kimlik doğrulaması** devre dışı bırakılır. Belirli, yüksek ayrıcalıklı rolleri, size daha fazla koruma için MFA gerekir.

Azure kaynak rolleri (Önizleme) rol ayarlarını sadece zaman ve de dahil olmak üzere doğrudan atanmasına ayarlarını yapılandırmak Yöneticiler izin ver:

- Kullanıcı veya grupları bir son tarih (kalıcı atama) olmadan rollere atama olanağı
- Varsayılan süre (değil kalıcı olduğunda) atama
- (Ne zaman uygun rolü üyesi etkinleştirir) en fazla etkinleştirme süresi
- Bir kullanıcı rol etkinleştirme sırasında sağlamak için gereken bilgileri (zaman içinde sadece atamaları) ya da atama işlemi (doğrudan atamaları)

![](./media/active-directory-privileged-identity-management-configure/role-settings-details.png)

## <a name="role-activation"></a>Rol etkinleştirmesi

İçin [bir rolü etkinleştirmesi](active-directory-privileged-identity-management-how-to-activate-role.md), bir zaman sınırlı "etkinleştirme" rolü için uygun yönetici ister. Etkinleştirme kullanarak istenebilir **my rolünü etkinleştirmek** Azure AD Privileged Identity Management seçeneği.

Bir rolü etkinleştirmesi isteyen bir yönetici, Azure portalında Azure AD Privileged Identity Management başlatması gerekir.

Rol etkinleştirme özelleştirilebilir. PIM ayarlarında etkinleştirme ve ne uzunluğu belirleyebilirsiniz bilgi yönetici rolünü etkinleştirmek için sağlamanız gerekir.

![PIM yönetici isteği rol etkinleştirme - ekran görüntüsü][5]

## <a name="review-role-activity"></a>Gözden geçirme rol etkinliği

Çalışanlar ve yöneticilerin ayrıcalıklı rolleri nasıl kullandığını izlemek için iki yolu vardır. İlk seçeneği kullanılarak [Directory rolleri denetim geçmişi](active-directory-privileged-identity-management-how-to-use-audit-log.md). Ayrıcalıklı rol atamaları, rol etkinleştirme geçmiş, değişiklikleri izle denetim geçmişi günlüklerini ve ve Azure kaynak rolleri (Önizleme) için ayarlarına yapılan değişiklikler. 

![PIM etkinleştirme geçmişi - ekran görüntüsü][6]

İkinci seçenek normal ayarlamaktır [erişim incelemeler](active-directory-privileged-identity-management-how-to-start-security-review.md). Bu erişim incelemeler tarafından gerçekleştirilen ve İnceleme (örneğin, bir takım Yöneticisi) atanmış veya çalışanları kendilerini gözden geçirebilirsiniz. Kimin hala erişim gerektirir ve artık kilitlenmiyor izlemek için en iyi yolu budur.

## <a name="azure-ad-pim-at-subscription-expiration"></a>Abonelik sona erme adresindeki Azure AD PIM

Kiracı bir Azure AD Premium P2 (ya da EMS E5) deneme aboneliği veya Ücretli abonelik Azure AD PIM kullanmadan önce kendi Kiracı olması gerekir.  Ayrıca, lisansları Kiracı Yöneticiler için atanmış olması gerekir.  Özellikle, lisansları yöneticileri Azure AD rollerdeki atanmalıdır Azure AD PIM yönetilen, yöneticiler Azure RBAC rollerdeki yönetilen Azure AD PIM ve erişim incelemeler gerçekleştiren yönetici olmayan kullanıcılar aracılığıyla.
Kuruluşunuz Azure AD Premium P2 yenilemez veya deneme süresi, bu Azure AD PIM özellikleri kiracınızda kullanılabilir, uygun rol atamalarını kaldırılır ve bu kullanıcıların rollerini etkinleştirebilir. Daha fazla bilgi edinebilirsiniz [Azure AD PIM abonelik gereksinimleri](./privileged-identity-management/subscription-requirements.md)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Admin_Overview.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_Settings_w_Approval_Disabled.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
