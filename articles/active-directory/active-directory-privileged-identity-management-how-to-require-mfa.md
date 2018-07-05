---
title: Çok faktörlü kimlik doğrulaması isteme | Microsoft Docs
description: Azure Active Directory Privileged Identity Management uzantısı ile ayrıcalıklı kimlikleri için çok faktörlü kimlik doğrulaması (MFA) gerektirme öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 727147673a527f2c28c9ca01ad17b30db292b6c0
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447226"
---
# <a name="how-to-require-mfa-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management MFA gerektirme
Çok faktörlü kimlik doğrulaması (MFA) tüm yöneticiler için ihtiyacınız olan öneririz. Bu, gizliliği bozulan parola nedeniyle bir saldırı riskini azaltır.

Kullanıcılar oturum açtığında MFA testini tamamlamanız gerektirebilir. Blog gönderisinde [Office 365 ve Azure MFA için mfa'yı](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) Office ve Azure abonelikleri Microsoft Azure multi-Factor Authentication teklifinde yer alan özelliklerle eklenir karşılaştırır.

Ayrıca, Azure AD PIM rol etkinleştirdiğinizde kullanıcılar'ın bir MFA testini tamamlamanız gerekebilir. Kullanıcı MFA testini tamamlanmadıysa, bunlar oturum açarken bu şekilde, bunu yapmak için PIM tarafından istenir.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management, MFA gerektirme
Ayrıcalıklı rol yöneticisi olarak PIM kimlikleri yönetirken, ayrıcalıklı hesaplar için mfa'yı öneririz uyarılar görebilirsiniz. PIM panosunda güvenlik uyarıya tıklayın ve MFA gerektiriyorsa yönetici hesaplarının listesi ile yeni bir dikey pencere açılır.  Birden çok rolü seçerek ve ardından MFA'ya gerek duyabilirsiniz **düzeltme** düğme veya bireysel rolleri yanındaki üç noktaya tıklayın ve ardından **düzeltme** düğmesi.

> [!IMPORTANT]
> Şimdi, Azure MFA yalnızca çalışır, iş veya Okul hesapları, Microsoft hesapları (Skype, Xbox, Outlook.com vb. gibi Microsoft hizmetlerinde oturum açmak için kullanılan genellikle bir kişisel hesap.) değil sağ. Kendi rollerini etkinleştirebilmelerini MFA kullanamazsınız çünkü bu nedenle, herhangi bir Microsoft hesabı kullanarak uygun yönetici olamaz. Bu kullanıcılar, bir Microsoft hesabı kullanarak iş yüklerini yönetmeye devam etmek gerekiyorsa, şu an için kalıcı yöneticileri Yükselt.
> 
> 

Ayrıca, belirli bir rol için MFA gereksinimi, PIM panonun roller bölümünden tıklayarak değiştirebilirsiniz. Ardından **ayarları** rol dikey ve ardından seçerek **etkinleştirme** çok faktörlü kimlik doğrulaması altında.

## <a name="how-azure-ad-pim-validates-mfa"></a>Azure AD PIM MFA nasıl doğrular
Bir kullanıcı bir rolünü etkinleştirirken MFA doğrulama için iki seçenek vardır.

Ayrıcalıklı rol etkinleştirme kullanıcılar için Azure MFA'yı etmenin en basit seçenek var. Bunu yapmak için önce bu kullanıcılara, gerekirse lisanslanır ve Azure MFA için kaydolduğunu denetleyin. Bunun nasıl yapılacağı hakkında daha fazla bilgi yer [bulutta Azure multi Factor Authentication kullanmaya başlama](authentication/howto-mfa-getstarted.md). Bu önerilir, ancak gerekli değildir, Azure AD oturum açtığında, bu kullanıcılar için MFA zorlamak için yapılandırın. MFA denetimleri, Azure AD PIM tarafından kendisine yapılacak olmasıdır.

Alternatif olarak, kullanıcılar şirket içi kimlik doğrulaması, kimlik sağlayıcınız için mfa'yı sorumlu olabilir. Örneğin, AD Federasyon Hizmetleri, Azure AD erişmeden önce akıllı kart tabanlı kimlik doğrulama isteyecek şekilde yapılandırdıysanız [Azure multi-Factor Authentication ve AD FS ile bulut kaynaklarını güvenli hale getirme](authentication/howto-mfa-adfs.md) yönergeleri içerir. AD FS'yi Azure AD talepleri gönderecek şekilde yapılandırma. Bir kullanıcı bir rolünü etkinleştirmek çalıştığında, Azure AD PIM uygun talep aldıktan sonra MFA zaten kullanıcının doğrulandığını kabul eder.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

