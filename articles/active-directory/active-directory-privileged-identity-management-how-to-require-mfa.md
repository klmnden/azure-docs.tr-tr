---
title: "Çok faktörlü kimlik doğrulama gerektirecek şekilde nasıl | Microsoft Docs"
description: "Azure Active Directory Privileged Identity Management uzantısı ile ayrıcalıklı kimlikleri için çok faktörlü kimlik doğrulaması (MFA) zorunlu kılma öğrenin."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 1e3dc4ad-3a6a-4a52-8417-3ca4f84ae05c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: b778774fa23be8219db3f716d79bac324a7de9d3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-require-mfa-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management MFA nasıl gerektirilir
Çok faktörlü kimlik doğrulaması (MFA) tüm yöneticilerinizi için ihtiyaç duyduğunuz öneririz. Bu, güvenliği aşılmış bir parola nedeniyle bir saldırı riskini azaltır.

Kullanıcılar oturum açtığında kullanıcılar bir MFA testini tamamlamanız gerekli kılabilirsiniz. Blog postası [MFA Office 365 ve Azure MFA için](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) Office ve Azure abonelikleri Microsoft Azure multi-Factor Authentication Sunumda içerdiği özelliklerle içereceği karşılaştırır.

Ayrıca, Azure AD PIM rolünde etkinleştirdiğinizde kullanıcılar bir MFA testini tamamlamanız isteyebilirsiniz. Kullanıcının oturum açıp MFA testini tamamlanmadı, bu şekilde, bunlar Bunu yapmak için PIM tarafından istenir.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management MFA gerektirme
Ayrıcalıklı rol yöneticisi olarak PIM kimlikleri yönetirken, ayrıcalıklı hesaplar için MFA önerilir uyarıları görebilirsiniz. Güvenlik Uyarısı PIM panosunda tıklayın ve MFA gerektirecek yönetici hesaplarını listesiyle yeni bir dikey pencere açılır.  Birden çok rolü seçerek ve ardından MFA'ya gerek duyabilirsiniz **düzeltme** düğmesini veya bireysel rolleri yanındaki üç noktaya tıklayın ve ardından **düzeltme** düğmesi.

> [!IMPORTANT]
> Şimdi, Azure MFA yalnızca works iş veya Okul hesapları, değil Microsoft hesapları (Skype, Xbox, Outlook.com, vb. gibi Microsoft hizmetlerinde oturum açmak için kullanılan genellikle bir kişisel hesap.) sağ. MFA rollerine etkinleştirmek için kullanamazlar olduğundan bu nedenle, herhangi bir Microsoft hesabı kullanarak uygun yönetim olamaz. Bu kullanıcılar bir Microsoft hesabı kullanarak iş yükleri yönetmeye devam etmek gerekiyorsa, şu an için kalıcı Yöneticiler Yükselt.
> 
> 

Ayrıca, belirli bir rol için MFA gereksinimi PIM Pano roller bölümünde tıklayarak değiştirebilirsiniz. Ardından, tıklatın **ayarları** rol dikey ve ardından seçme **etkinleştirmek** çok faktörlü kimlik doğrulaması altında.

## <a name="how-azure-ad-pim-validates-mfa"></a>Azure AD PIM MFA nasıl doğrular
Bir kullanıcı bir rolünü etkinleştirirken MFA doğrulamak için iki seçenek vardır.

Basit bir ayrıcalıklı rolü etkinleştirme kullanıcılar için Azure MFA yararlanmayı seçenektir. Bunu yapmak için önce bu kullanıcıların gerekirse lisanslanır ve Azure MFA için kayıtlı denetleyin. Bunun nasıl yapılacağı hakkında daha fazla bilgi yer [bulutta Azure multi-Factor Authentication kullanmaya Başlarken](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). Bu önerilir, ancak gerekli değildir, bunlar oturum açtığında bu kullanıcılar için MFA zorlamak için Azure AD yapılandırın. Azure AD PIM tarafından kendisini MFA denetimleri yapılacağından budur.

Alternatif olarak, kullanıcıların şirket içi kimlik doğrulaması, MFA için sorumlu kimlik sağlayıcınız olabilir. Örneğin, AD Federasyon Hizmetleri Azure AD erişmeden önce akıllı kart tabanlı kimlik doğrulama gerektirecek şekilde yapılandırılmışsa [güvenliğini sağlama bulut kaynakları Azure multi-Factor Authentication ve AD FS ile](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) yönergeleri içerir Azure ad talepleri göndermek için AD FS yapılandırma. Bir kullanıcı bir rolünü etkinleştirmek çalıştığında Azure AD PIM uygun talep aldıktan sonra MFA zaten kullanıcıdan doğrulandı olduğunu kabul eder.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

