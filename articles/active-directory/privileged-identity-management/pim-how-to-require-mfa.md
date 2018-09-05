---
title: Çok faktörlü kimlik doğrulaması (MFA) ve PIM - Azure | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) çok faktörlü kimlik doğrulaması (MFA) nasıl doğrular öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 08/31/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: cfa092e8aebe92192ecca8aec2721282e603cc5b
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669283"
---
# <a name="multi-factor-authentication-mfa-and-pim"></a>Çok faktörlü kimlik doğrulaması (MFA) ve PIM

Tüm Yöneticiler için çok faktörlü kimlik doğrulaması (MFA) gerekli öneririz. Bu, gizliliği bozulan parola nedeniyle bir saldırı riskini azaltır.

Kullanıcılar oturum açtığında MFA testini tamamlamanız gerektirebilir. Ayrıca, Azure AD Privileged Identity Management (PIM) rolünde etkinleştirdiğinizde kullanıcılar'ın bir MFA testini tamamlamanız gerekebilir. Kullanıcı MFA testini tamamlanmadıysa, bunlar oturum açarken bu şekilde, bunu yapmak için PIM tarafından istenir.

> [!IMPORTANT]
> Şimdi, Azure MFA yalnızca çalışır, iş veya Okul hesapları, Microsoft hesapları (Skype, Xbox, Outlook.com vb. gibi Microsoft hizmetlerinde oturum açmak için kullanılan genellikle bir kişisel hesap.) değil sağ. Kendi rollerini etkinleştirebilmelerini MFA kullanamazsınız çünkü bu nedenle, herhangi bir Microsoft hesabı kullanarak uygun yönetici olamaz. Bu kullanıcılar, bir Microsoft hesabı kullanarak iş yüklerini yönetmeye devam etmek gerekiyorsa, şu an için kalıcı yöneticileri Yükselt.

## <a name="how-pim-validates-mfa"></a>PIM MFA nasıl doğrular

Bir kullanıcı bir rolünü etkinleştirirken MFA doğrulama için iki seçenek vardır.

Ayrıcalıklı rol etkinleştirme kullanıcılar için Azure MFA'yı etmenin en basit seçenek var. Bunu yapmak için önce bu kullanıcılara, gerekirse lisanslanır ve Azure MFA için kaydolduğunu denetleyin. Azure mfa'yı dağıtma hakkında daha fazla bilgi için bkz. [Azure multi-Factor Authentication'ı bulut tabanlı dağıtım](../authentication/howto-mfa-getstarted.md). Bu önerilir, ancak gerekli değildir, Azure AD oturum açtığında, bu kullanıcılar için MFA zorlamak için yapılandırın. MFA denetimleri PIM tarafından yapılacak olmasıdır.

Alternatif olarak, kullanıcılar şirket içi kimlik doğrulaması, kimlik sağlayıcınız için mfa'yı sorumlu olabilir. Örneğin, AD Federasyon Hizmetleri, Azure AD erişmeden önce akıllı kart tabanlı kimlik doğrulama isteyecek şekilde yapılandırdıysanız [Azure multi-Factor Authentication ve AD FS ile bulut kaynaklarını güvenli hale getirme](../authentication/howto-mfa-adfs.md) yönergeleri içerir. AD FS'yi Azure AD talepleri gönderecek şekilde yapılandırma. Bir kullanıcı bir rolünü etkinleştirmek çalıştığında, PIM uygun talep aldıktan sonra MFA zaten kullanıcının doğrulandığını kabul eder.

## <a name="next-steps"></a>Sonraki adımlar

- [PIM'de Azure AD dizini rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
- [PIM'de Azure kaynak rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
