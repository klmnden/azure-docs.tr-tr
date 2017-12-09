---
title: Azure AD Privileged Identity Management rollerinde | Microsoft Docs
description: "Azure Privileged Identity Management uzantısı ile ayrıcalıklı kimlikleri için hangi rolleri kullanılan öğrenin."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: ac812ccc-cf4e-4ac2-b981-69598056c9ed
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.openlocfilehash: e3f67b978ff66cbb71709f2f8d66986a33149ae6
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="different-administrative-role-in-azure-active-directory-pim"></a>Azure Active Directory PIM farklı yönetim rolü
<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Azure AD'de farklı yönetim rolleri, kuruluşunuzdaki kullanıcılar atayabilirsiniz. Bu rol atamaları ekleme veya kullanıcıları kaldırma ya da hizmet ayarlarını değiştirme gibi görevleri, kullanıcıların Azure üzerinde AD, Office 365 ve diğer Microsoft Online Services ve bağlı uygulamaların gerçekleştirebilir denetler.  

> [!IMPORTANT]
> Microsoft, Azure AD’yi bu makalede bahsedilen Klasik Azure Portalı yerine Azure portalındaki [Azure AD yönetim merkezini](https://aad.portal.azure.com) kullanarak yönetmenizi öneriyor.

Genel yönetici olan kullanıcılar güncelleştirebilirsiniz **kalıcı olarak** PowerShell cmdlet'lerini kullanarak Azure AD'de rol atanmış `Add-MsolRoleMember` ve `Remove-MsolRoleMember`, ya da açıklandığı gibi Klasik portal üzerinden [ Azure Active Directory'de yönetici rolleri atama](active-directory-assign-admin-roles-azure-portal.md).

Azure AD Privileged Identity Management (PIM) Azure AD'de kullanıcıları için ayrıcalıklı erişim ilkelerini yönetir. PIM kullanıcılar için bir veya daha fazla rol Azure AD'de atar ve kalıcı olarak rolünü veya rol için uygun olması birine atayın. Ne zaman bir kullanıcı bir role kalıcı olarak atanmış veya Azure Active Directory, Office 365 ve diğer uygulamalar rollerine atanmış izinlerle yönetebilecekleri sonra uygun rol atama etkinleştirir.

Kalıcı bir uygun rol ataması karşı kimseler verilen erişim fark yoktur. Tek fark bazı kişiler bu erişim her zaman gerekmemesidir. Rolü için uygun yapılır ve bunu etkinleştirebilirsiniz ve devre dışı olduğunda gerekir.

## <a name="roles-managed-in-pim"></a>PIM içinde yönetilen roller
Privileged Identity Management kullanıcılar dahil olmak üzere genel yönetici rolleri atama olanak sağlar:

* **Genel yönetici** (şirket yönetici olarak da bilinir) tüm yönetim özelliklerine erişebilir. Kuruluşunuzda birden çok genel yönetici olabilir. Office 365 otomatik olarak satın almak için kaydolan kişi bir genel yönetici olur
* **Ayrıcalıklı Rol Yöneticisi** Azure AD PIM yönetir ve diğer kullanıcılar için rol atamalarını güncelleştirir.  
* **Faturalama Yöneticisi** satın alma işlemleri yapar, abonelikleri yönetir, destek biletlerini yönetir ve hizmetin sistem durumunu izler.
* **Parola Yöneticisi** parolaları sıfırlar, hizmet isteklerini yönetir ve hizmetin sistem durumunu izler. Parola yöneticileri, kullanıcılar için parola sıfırlama için sınırlıdır.
* **Hizmet Yöneticisi** hizmet isteklerini yönetir ve hizmetin sistem durumunu izler.
  
  > [!NOTE]
  > Office 365 kullanıyorsanız, sonra bir kullanıcı, Hizmet Yöneticisi rolü atamadan önce ilk kullanıcı yönetim izinleri Exchange Online gibi bir hizmet atayın.
  > 
  > 
* **Kullanıcı Yönetimi Yöneticisi** parolaları sıfırlar, hizmetin sistem durumunu izler ve kullanıcı hesapları, kullanıcı grupları ve hizmet isteklerini yönetir. Kullanıcı Yönetimi yönetim genel yönetici silinemez, diğer yönetici rolleri oluşturabilir veya fatura, genel ve hizmet yöneticileri için parolaları sıfırlayın.
* **Exchange Yöneticisi** Exchange yönetici merkezini (Seçmeye) Exchange Online yönetici erişimi olan ve neredeyse her görevi'te Exchange Online gerçekleştirebilirsiniz.
* **SharePoint Yöneticisi** SharePoint Online Yönetim Merkezi SharePoint Online yönetim erişimi vardır ve neredeyse her görev SharePoint Online'da gerçekleştirebilirsiniz.
* **İş Yöneticisi Skype** yönetim Skype kurumsal iş Yönetim Merkezi Skype üzerinden erişimi ve neredeyse her görev Skype Kurumsal çevrimiçi gerçekleştirebilirsiniz.

Daha fazla ayrıntı için bu makaleler okuyun [Azure AD'de yönetici rolleri atama](active-directory-assign-admin-roles-azure-portal.md) ve [Office 365'te yönetici rolleri atama](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


PIM yapabilecekleriniz [kullanıcıya bu roller atama](active-directory-privileged-identity-management-how-to-add-role-to-user.md) kullanıcı böylece [gerektiğinde rolünü etkinleştirmek](active-directory-privileged-identity-management-how-to-activate-role.md).

PIM yönetmek için başka bir kullanıcı erişimi vermek istiyorsanız, PIM kullanıcının gerektiren roller daha ayrıntılı açıklanır [PIM için erişim vermek nasıl](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>PIM içinde yönetilmeyen rolleri
Exchange Online veya SharePoint Online içinde rolleri olanlar, yukarıda belirtilen dışında Azure AD'de temsil edilmez ve bu nedenle PIM görünür değildir. Bu Office 365 Hizmetleri hassas rol atamalarını değiştirme hakkında daha fazla bilgi için bkz: [Office 365'te izinleri](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Ayrıca Azure Abonelikleriniz ve kaynak gruplarınız Azure AD'de temsil edilmez. Azure Aboneliklerini yönetmek için bkz: [eklemek veya Azure yönetici rollerini değiştirmek nasıl](../billing/billing-add-change-azure-subscription-administrator.md) ve Azure RBAC bakın hakkında daha fazla bilgi için [Azure rol tabanlı erişim denetimi](role-based-access-control-configure.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Kullanıcı rolleri ve oturum açma
Bazı Microsoft Hizmetleri ve uygulamaları için bir kullanıcı rol atama o kullanıcının yönetici olmasını sağlamak yeterli olmayabilir.

Kullanıcı Azure Aboneliklerini yönetmek gerekmez dahi, Klasik Azure portalına erişim Hizmet Yöneticisi veya bir Azure aboneliğinin ortak yönetici kullanıcı olmasını gerektirir.  Örneğin, Azure AD Klasik portalında için yapılandırma ayarlarını yönetmek için bir kullanıcı Azure AD genel yönetici ve bir Azure aboneliği abonelik ortak yönetici olması gerekir.  Kullanıcılar Azure aboneliklerine eklemeyi öğrenmek için bkz: [eklemek veya Azure yönetici rollerini değiştirmek nasıl](../billing/billing-add-change-azure-subscription-administrator.md).

Kullanıcı ayrıca atanabilir bir lisans hizmetin portalını açın veya yönetim görevlerini gerçekleştirmek için önce Microsoft Online Services erişimi gerektirebilir.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Azure AD'de bir kullanıcıya bir lisans atama
1. Oturum [Klasik Azure portalı](http://manage.windowsazure.com) bir genel yönetici hesabını veya bir ortak yönetici hesabı.
2. Seçin **tüm öğeleri** ana menüden.
3. Çalışmak istediğiniz dizini seçin ve onunla ilişkili lisans sahip.
4. Seçin **lisansları**. Kullanılabilir lisans listesi görüntülenir.
5. Dağıtmak istediğiniz lisansları içeren lisans planını seçin.
6. Seçin **kullanıcı atama**.
7. Bir lisans atamak istediğiniz kullanıcıyı seçin.
8. tıklatın **atamak** düğmesi.  Kullanıcı artık Azure'a oturum açabilir.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

