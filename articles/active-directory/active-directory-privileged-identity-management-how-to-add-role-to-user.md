---
title: Bir kullanıcı rolü ekleme veya kaldırma nasıl | Microsoft Docs
description: Azure Active Directory Privileged Identity Management uygulaması ile ayrıcalıklı kimlikleri rollerini ekleme hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 01/03/2018
ms.author: rolyon
ms.openlocfilehash: 438a2fcd511fe62fa1dc7ba603c3770d5bcb56a4
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446852"
---
# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure AD Privileged Identity Management: Kullanıcı rolü ekleme ve kaldırma
Azure Active Directory (AD ile), bir genel yönetici (veya şirket Yöneticisi) kullanıcıların güncelleştirebilirsiniz **kalıcı olarak** Azure AD'de rol atanmış. Bu gibi PowerShell cmdlet'leriyle yapılır `Add-MsolRoleMember` ve `Remove-MsolRoleMember`. Ya da açıklandığı gibi Azure portalını kullanabilirsiniz [Azure Active Directory'de yönetici rolleri atama](active-directory-assign-admin-roles.md).

Ayrıcalıklı rol yöneticileri de kalıcı rol atamaları yapmak Azure AD Privileged Identity Management uygulaması sağlar. Ayrıca, ayrıcalıklı rol Yöneticileri kullanıcıların yapabileceğini **uygun** yönetici rolleri için. Uygun yönetici rolü, ihtiyaç duydukları ve bunlar bitirdiğinizde izinlerini süresi dolacak etkinleştirebilirsiniz.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>Azure portalında rollerini PIM ile yönetme
Kuruluşunuzdaki kullanıcılar için farklı yönetim rolleri Azure AD'de Office 365 ve diğer Microsoft Hizmetleri ve uygulamaları atayabilirsiniz.  Kullanılabilir roller hakkında daha fazla ayrıntı şu adreste bulunabilir: [Azure AD PIM rolleri](active-directory-privileged-identity-management-roles.md).

Bir kullanıcı Privileged Identity Management'ı kullanarak bir rolde ekleyip PIM Pano taşıyın. Ardından ya da **kullanıcılar Yönetim rollerindeki** düğmesini ya da rolleri tablodaki belirli bir rol (örneğin, genel yönetici) seçin.

> [!NOTE]
> Git, PIM Azure portalında henüz etkinleştirmediyseniz [Azure AD PIM ile çalışmaya başlama](active-directory-privileged-identity-management-getting-started.md) Ayrıntılar için.

PIM için başka bir kullanıcı erişim vermek istiyorsanız, kullanıcının PIM gerektiren roller daha ayrıntılı açıklanır [PIM erişimi verme nasıl](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="add-a-user-to-a-role"></a>Bir role kullanıcı ekleme
1. İçinde [Azure portalında](https://portal.azure.com/)seçin **Azure AD Privileged Identity Management** Panoda kutucuk.
2. Seçin **ayrıcalıklı rollerimi Yönet**.
3. İçinde **Rol Özeti** tablo, yönetmek istediğiniz rolü seçin.
4. Rol dikey penceresinde, seçin **Ekle**.
5. Tıklayın **Seçili kullanıcılar** ve kullanıcı için arama **kullanıcıları seçin** dikey penceresi.  
6. Kullanıcı Arama sonuçları listesinden seçip tıklayın **Bitti**.
7. Tıklayın **Tamam** seçiminizi kaydetmek için. Seçtiğiniz kullanıcı rolü için uygun olarak listesinde görünür.

> [!NOTE]
> Yeni kullanıcılar bir rol, yalnızca varsayılan olarak rol için uygundur. Rol kalıcı hale getirmek isterseniz, kullanıcı listesinde'e tıklayın. Kullanıcının bilgileri yeni bir dikey pencerede görüntülenir. Seçin **yapma izni** kullanıcı bilgileri menüsünde.  
> Bir kullanıcı Azure multi-Factor Authentication (MFA) kaydedilemiyor veya bir Microsoft hesabı kullanarak (genellikle @outlook.com), kendi rolleri içinde kalıcı yapmanız gerekir. Uygun Yöneticiler, etkinleştirme sırasında MFA'ya kaydetmeniz istenir.

Kullanıcının bir rol için uygun olduğundan, bunlar bu yönergeleri göre etkinleştirmelerini bildirmek [etkinleştirmek veya bir rolü devre dışı bırakma](active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="remove-a-user-from-a-role"></a>Bir kullanıcıyı rolden Kaldır
Uygun rol ataması kullanıcıları kaldırmak, ancak her zaman kalıcı bir genel yönetici olan en az bir kullanıcı olduğundan emin olun.

Belirli bir kullanıcıyı rolden kaldırmak için aşağıdaki adımları izleyin:

1. Rol listesi rolünde bir rol Azure AD PIM panosunda seçerek veya tıklayarak gidin **kullanıcılar Yönetim rollerindeki** düğmesi.
2. Kullanıcı listesinde kullanıcı tıklayın.
3. Tıklayın **Kaldır**. Bir ileti onaylamanızı ister.
4. Tıklayın **Evet** kullanıcı rolünü kaldırmak için.

Hangi kullanıcıların rol atamalarının yine emin değilseniz sonra yapabilecekleriniz [rol için erişim değerlendirmesi başlatma](active-directory-privileged-identity-management-how-to-start-security-review.md).

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

