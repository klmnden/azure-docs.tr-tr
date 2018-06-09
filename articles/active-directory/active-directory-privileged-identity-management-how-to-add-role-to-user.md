---
title: Bir kullanıcı rolü ekleme veya kaldırma nasıl | Microsoft Docs
description: Azure Active Directory Privileged Identity Management uygulaması ile ayrıcalıklı kimlikleri rolleri eklemek öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: protection
ms.date: 01/03/2018
ms.author: rolyon
ms.openlocfilehash: 856fdc69bd5ce582ca772c01f8af615fbc455887
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233229"
---
# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure AD Privileged Identity Management: Kullanıcı rolü ekleme ve kaldırma
Azure Active Directory (AD ile), bir genel yönetici (veya şirket Yöneticisi) kullanıcıları olan güncelleştirebilirsiniz **kalıcı olarak** Azure AD'de rollerine atanmış. Bu PowerShell cmdlet'leri gibi gerçekleştirilir `Add-MsolRoleMember` ve `Remove-MsolRoleMember`. Ya da açıklandığı gibi Azure portalını kullanabilirsiniz [Azure Active Directory'de yönetici rolleri atama](active-directory-assign-admin-roles.md).

Azure AD Privileged Identity Management uygulaması kalıcı rol atamaları, de yapmak ayrıcalıklı rol yöneticilerinin sağlar. Ayrıca, ayrıcalıklı rol Yöneticiler kullanıcılar yapabilir **uygun** yönetici rolleri. Uygun bir yönetici rolü ihtiyaç duydukları ve bunlar bitirdiğinizde izinlerini sona etkinleştirebilirsiniz.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>PIM rolleriyle Azure portalında Yönet
Kuruluşunuzdaki kullanıcılar Azure AD'de Office 365 ve diğer Microsoft Hizmetleri ve uygulamaları için farklı yönetim rolleri atayabilirsiniz.  Kullanılabilir roller hakkında daha fazla ayrıntı bulunabilir [Azure AD PIM rolleri](active-directory-privileged-identity-management-roles.md).

Kullanıcı ayrıcalıklı Kimlik Yönetimi'ni kullanarak bir rolde ekleyip PIM Pano getirin. Ya da tıklatın **yönetici rolleri** düğmesini veya rolleri tablosundan (örneğin, genel yönetici) belirli bir rol seçin.

> [!NOTE]
> PIM Azure portalında henüz etkinleştirmediyseniz, Git [Azure AD PIM ile çalışmaya başlama](active-directory-privileged-identity-management-getting-started.md) Ayrıntılar için.

PIM için başka bir kullanıcı erişimi vermek istiyorsanız, PIM kullanıcının gerektiren roller daha ayrıntılı açıklanır [PIM için erişim vermek nasıl](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="add-a-user-to-a-role"></a>Bir role kullanıcı ekleme
1. İçinde [Azure portal](https://portal.azure.com/)seçin **Azure AD Privileged Identity Management** döşeme Panoda.
2. Seçin **yönetmek ayrıcalıklı rolleri**.
3. İçinde **Rol Özeti** tablo, yönetmek istediğiniz rolü seçin.
4. Rol dikey penceresinde, seçin **Ekle**.
5. Tıklatın **kullanıcı seçin** ve kullanıcı arama **kullanıcı seçin** dikey.  
6. Kullanıcı Arama sonuçları listesinden seçip'ı tıklatın **Bitti**.
7. Tıklatın **Tamam** seçiminizi kaydetmek için. Seçtiğiniz kullanıcı rolü için uygun olarak listesinde görünür.

> [!NOTE]
> Bir rolü yeni kullanıcılar yalnızca varsayılan olarak rol için uygundur. Rol kalıcı hale getirmek istiyorsanız, kullanıcı listesinde'ı tıklatın. Kullanıcının bilgileri yeni bir dikey pencerede görüntülenir. Seçin **yapma izni** kullanıcı bilgileri menüsünde.  
> Bir kullanıcı Azure çok faktörlü kimlik doğrulama (MFA) kaydedilemiyor veya bir Microsoft hesabı kullanıyorsanız (genellikle @outlook.com), kendi tüm rollerde kalıcı hale getirmek gerekir. Uygun yöneticileri etkinleştirme sırasında MFA için kaydolması istenir.

Kullanıcı rolü için uygun olup, bunlar yönergelerine göre etkinleştirilebilmesi olduğunu bildirmek [etkinleştirin veya bir rolü devre dışı bırakma](active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="remove-a-user-from-a-role"></a>Bir kullanıcıyı rolden kaldırır
Uygun rol atamaları Kullanıcıları Kaldır, ancak her zaman kalıcı genel yönetici olan en az bir kullanıcı olduğundan emin olun.

Belirli bir kullanıcıyı rolden kaldırmak için aşağıdaki adımları izleyin:

1. Azure AD PIM Panoda bir rolü seçerek veya tıklayarak rolü listesinde rol gidin **yönetici rolleri** düğmesi.
2. Kullanıcı listesinde kullanıcı tıklayın.
3. Tıklatın **kaldırmak**. Bir ileti onaylamanızı ister.
4. Tıklatın **Evet** kullanıcı rolünü kaldırmak için.

Hangi kullanıcıların yine rol atamalarını gerekir emin değilseniz sonra şunları yapabilirsiniz [rolü için bir erişim incelemesi başlatma](active-directory-privileged-identity-management-how-to-start-security-review.md).

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

