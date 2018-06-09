---
title: Çok faktörlü kimlik doğrulama gerektirecek şekilde nasıl | Microsoft Docs
description: Azure Active Directory Privileged Identity Management uzantısı ile ayrıcalıklı kimlikleri için çok faktörlü kimlik doğrulaması (MFA) zorunlu kılma öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: protection
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: d0a9abc145a4d108e48bc81cbb6a849c62e5862b
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234021"
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

Basit bir ayrıcalıklı rolü etkinleştirme kullanıcılar için Azure MFA yararlanmayı seçenektir. Bunu yapmak için önce bu kullanıcıların gerekirse lisanslanır ve Azure MFA için kayıtlı denetleyin. Bunun nasıl yapılacağı hakkında daha fazla bilgi yer [bulutta Azure multi-Factor Authentication kullanmaya Başlarken](authentication/howto-mfa-getstarted.md). Bu önerilir, ancak gerekli değildir, bunlar oturum açtığında bu kullanıcılar için MFA zorlamak için Azure AD yapılandırın. Azure AD PIM tarafından kendisini MFA denetimleri yapılacağından budur.

Alternatif olarak, kullanıcıların şirket içi kimlik doğrulaması, MFA için sorumlu kimlik sağlayıcınız olabilir. Örneğin, AD Federasyon Hizmetleri Azure AD erişmeden önce akıllı kart tabanlı kimlik doğrulama gerektirecek şekilde yapılandırılmışsa [güvenliğini sağlama bulut kaynakları Azure multi-Factor Authentication ve AD FS ile](authentication/howto-mfa-adfs.md) yönergeleri içerir Azure ad talepleri göndermek için AD FS yapılandırma. Bir kullanıcı bir rolünü etkinleştirmek çalıştığında Azure AD PIM uygun talep aldıktan sonra MFA zaten kullanıcıdan doğrulandı olduğunu kabul eder.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

