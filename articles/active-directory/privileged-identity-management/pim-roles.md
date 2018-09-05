---
title: Azure AD dizin rollerini PIM içinde yönetebileceğiniz | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) yönetebileceğiniz Azure AD Dizin rolleri açıklanır.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 07/23/2018
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.openlocfilehash: fc45cde1a5f0f287274302541ac0115569e2239d
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666345"
---
# <a name="azure-ad-directory-roles-you-can-manage-in-pim"></a>PIM'de yönetebilmeniz için azure AD Dizin rolleri
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

1. Lisansları ile ilişkili olan ve çalışmak istediğiniz Azure AD dizini seçin.

1. Sol gezinti bölmesinde tıklayın **Azure Active Directory**.

1. Tıklayın **lisansları**. Kullanılabilir lisanslar listesi görüntülenir.

    ![Azure Active Directory lisansları](./media/pim-roles/licenses-overview.png)

1. ' A tıklayın, **ürün**.

1. Dağıtmak istediğiniz lisansları içeren lisans plana tıklayın.

    ![Lisans ürünleri](./media/pim-roles/licenses-products.png)

1. Tıklayın **atama** Ata lisans bölmesini açmak için.

    ![Lisanslı kullanıcılar](./media/pim-roles/licenses-licensed-users.png)

1. Kullanıcı veya bir lisansı atamak istediğiniz grubu seçin.

    ![Lisans ata](./media/pim-roles/licenses-assign-license.png)

1. Tıklayın **atama seçenekleri** atama seçenekleri yapılandırmak için.

    ![Atama seçenekleri](./media/pim-roles/licenses-assignment-options.png)

1. Tıklayın **atama** lisans atamak için. Kullanıcı, artık lisansına sahiptir.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar

- [PIM kullanmaya başlayın](pim-getting-started.md)
- [Azure AD dizin rollerini PIM atayın](pim-how-to-add-role-to-user.md)

