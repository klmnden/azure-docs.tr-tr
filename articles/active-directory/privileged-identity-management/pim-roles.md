---
title: Azure AD Privileged Identity Management rollerinde | Microsoft Docs
description: Azure Privileged Identity Management uzantısı ile ayrıcalıklı kimlikleri için hangi rollerin kullanıldığı hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 07/23/2018
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.openlocfilehash: 6553fdba463144c6eda1e35c0967e92a3c44aff6
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225585"
---
# <a name="directory-roles-you-can-manage-using-azure-ad-pim"></a>Dizin rolleri Azure AD PIM kullanarak yönetebilirsiniz.
<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Azure AD'de farklı yönetim rolleri, kuruluşunuzdaki kullanıcılara atayabilirsiniz. Ekleme veya kullanıcıları kaldırma ya da hizmet ayarlarını değiştirme gibi görevler, kullanıcıları Azure AD, Office 365 ve diğer Microsoft Online Services ve bağlı uygulamalar gerçekleştirebilir. Bu rol atamaları denetleyin.  

Genel yönetici olan kullanıcılar güncelleştirebilirsiniz **kalıcı olarak** açıklandığı portal üzerinden Azure AD'de rol atanmış [Azure Active Directory'de yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md) veya kullanma[ PowerShell komutlarını](/powershell/module/azuread#directory_roles).

Azure AD Privileged Identity Management (PIM), Azure AD'de kullanıcıları için ayrıcalıklı erişim ilkelerini yönetir. PIM kullanıcıların Azure AD'de bir veya daha fazla role atar ve birinin kalıcı olarak rolün veya rol için uygun olacak şekilde atayabilirsiniz. Ne zaman bir kullanıcı bir rol kalıcı olarak atanan veya bir uygun rol ataması, Azure Active Directory, Office 365 ve diğer uygulamalar, rollerine atanmış izinlere sahip yönetebilir etkinleştirir.

Kalıcı uygun rol atamasını karşı kimseler verilen erişimi hiçbir fark yoktur. Tek fark, bazıları her zaman bu erişim olmanızın gerekmemesidir. Rol için uygun olarak gerçekleştirilir ve bunu etkinleştirebilirsiniz ve devre dışı olduğunda gerekir.

## <a name="roles-managed-in-pim"></a>Yönetilen PIM rolleri
Privileged Identity Management kullanıcılar dahil olmak üzere genel yönetici rolleri atamanıza olanak tanır:

* **Genel yönetici** (şirket yöneticisi olarak da bilinir) tüm yönetim özelliklerine erişebilir. Kuruluşunuzda birden fazla genel yönetici olabilir. Kaydolan otomatik olarak Office 365 satın almak için kişi genel yönetici olur.
* **Ayrıcalıklı Rol Yöneticisi** Azure AD PIM yönetir ve diğer kullanıcılar için rol atamalarını güncelleştirir.  
* **Faturalama Yöneticisi** satın alma işlemleri yapar, abonelikleri yönetir, destek biletlerini yönetir ve hizmetin sistem durumunu izler.
* **Parola Yöneticisi** parolaları sıfırlar, hizmet isteklerini yönetir ve hizmetin sistem durumunu izler. Parola yöneticileri, kullanıcıların parolalarını için sınırlıdır.
* **Hizmet Yöneticisi** hizmet isteklerini yönetir ve hizmetin sistem durumunu izler.
  
  > [!NOTE]
  > Office 365 kullanıyorsanız, ardından bir kullanıcı, Hizmet Yöneticisi rolü atamadan önce ilk kullanıcı yönetici izinleri Exchange Online gibi bir hizmet atayın.
  > 
  > 
* **Kullanıcı Yönetimi Yöneticisi** parolaları sıfırlar, hizmetin sistem durumunu izler ve kullanıcı hesapları, kullanıcı grupları ve hizmet isteklerini yönetir. Kullanıcı Yönetimi Yöneticisi bir genel yöneticiyi silemez, başka yönetici rolleri oluşturamaz veya faturalama, genel ve hizmet yöneticileri için parolaları sıfırlama.
* **Exchange Yöneticisi** Exchange yönetici merkezini (EAC) Exchange Online yönetici erişimi olan ve hemen her görevi, Exchange Online'da gerçekleştirebilirsiniz.
* **SharePoint Yöneticisi (Önizleme)** SharePoint Online Yönetim Merkezi SharePoint Online yönetimsel erişim sahibi ve hemen her görevi SharePoint Online'da gerçekleştirebilirsiniz. Bu rol, şu anda Önizleme aşamasındadır. Uygun kullanıcılar PIM'de etkinleştirdikten sonra SharePoint içinde bu rolü kullanarak gecikme.
* **Skype Kurumsal Yöneticisi** Skype kurumsal iş Yönetim Merkezi aracılığıyla işletme için Skype yönetimsel erişim sahibi ve neredeyse her görev Skype Kurumsal çevrimiçi gerçekleştirebilirsiniz.

Daha fazla ayrıntı için bu makaleleri okuyun [Azure AD'de yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md) ve [Office 365'te yönetici rolleri atama](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


PIM, yapabilecekleriniz [kullanıcıya bu roller atama](pim-how-to-add-role-to-user.md) kullanıcı böylece [gerektiğinde rolü etkinleştirmek](pim-how-to-activate-role.md).

Kullanıcının PIM gerektiren rollerini PIM yönetmek için başka bir kullanıcı erişim vermek istiyorsanız, daha ayrıntılı açıklanır [PIM erişimi verme nasıl](pim-how-to-give-access-to-pim.md).

<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Yönetilmeyen PIM rolleri
Exchange Online veya SharePoint Online içinde bir rol, yukarıda belirtilen hariç Azure AD'de temsil edilmez ve bu nedenle PIM'de görünür değildir. Bu Office 365 hizmetlerindeki ayrıntılı rol atamalarını değiştirme hakkında daha fazla bilgi için bkz. [izinleri Office 365'te](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Ayrıca Azure Abonelikleriniz ve kaynak gruplarınız Azure AD'de temsil edilmez. Azure Aboneliklerini yönetmek için bkz: [Azure yöneticisi rollerini ekleme veya değiştirme yapma](../../billing/billing-add-change-azure-subscription-administrator.md) ve Azure RBAC hakkında daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi](../../role-based-access-control/role-assignments-portal.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Kullanıcı rolleri ve oturum açma
Bazı Microsoft Hizmetleri ve uygulamaları için kullanıcı rol atama yönetici kullanıcının etkinleştirmek yeterli olmayabilir.

Azure Aboneliklerini yönetmek kullanıcı gerekmiyor olsa bile bir Hizmet Yöneticisi veya Azure aboneliğinin ortak yönetici kullanıcı olması Azure portalına erişim gerektirir.  Örneğin, Azure AD için yapılandırma ayarlarını yönetmek için bir kullanıcı Azure AD'de genel yönetici hem de Azure aboneliğinin bir aboneliğin ortak yönetici olması gerekir.  Azure abonelikleri için kullanıcı ekleme konusunda bilgi almak için bkz: [Azure yöneticisi rollerini ekleme veya değiştirme yapma](../../billing/billing-add-change-azure-subscription-administrator.md).

Kullanıcı ayrıca atanmış bir lisansı hizmetin portalını açın veya yönetim görevlerini gerçekleştirmek için önce Microsoft Online hizmetlerine erişim gerektirebilir.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Azure AD'de bir kullanıcıya lisans atama
1. Oturum [Azure portalında](http://portal.azure.com) ile bir genel yönetici veya ortak yönetici hesabı.
3. Azure AD'yi seçin ve onunla ilişkili lisans ile çalışma ve, istediğiniz dizine sahip.
4. Seçin **lisansları** soldaki. Kullanılabilir lisanslar listesi görüntülenir.
5. Dağıtmak istediğiniz lisansları içeren lisans planını seçin.
6. Seçin **Kullanıcıları Ata**.
7. Bir lisansı atamak istediğiniz kullanıcıyı seçin.
8. Tıklayın **atama** düğmesi.  Kullanıcı artık Azure'da oturum açabilir.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]

