---
title: Azure AD Connect - geçişli kimlik doğrulaması - yükseltme kimlik doğrulama aracılarının | Microsoft Docs
description: Bu makalede, Azure Active Directory (Azure AD) geçişli kimlik doğrulaması yapılandırmanızı yükseltileceği açıklanır.
services: active-directory
keywords: Azure AD, SSO, Azure AD Connect geçişli kimlik doğrulaması, Active Directory Yükleme gerekli bileşenleri çoklu oturum açma
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: be76965e99a20c1f7164187255e26f6463926c2f
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214736"
---
# <a name="azure-active-directory-pass-through-authentication-upgrade-preview-authentication-agents"></a>Azure Active Directory geçişli kimlik doğrulaması: Yükseltme Önizleme kimlik doğrulama aracılarının

## <a name="overview"></a>Genel Bakış

Bu makale önizlemesi ile Azure AD geçişli kimlik doğrulaması kullanan müşteriler içindir. Size yakın zamanda yükseltme (ve yeni marka adları verilmiştir) kimlik doğrulama Aracısı yazılım. Şunları yapmanız _el ile_ yükseltme Önizleme kimlik doğrulama aracılarının şirket içi sunucularınız üzerinde yüklü. Bu el ile yükseltme tek seferlik bir eylem var. Kimlik doğrulama aracılarının gelecekteki tüm güncelleştirmeler otomatik olarak yapılır. Yükseltme nedenleri aşağıdaki gibidir:

- Kimlik doğrulama aracılarının Önizleme sürümleri, herhangi bir ek güvenlik veya hata düzeltmesi almazsınız.
-   Kimlik doğrulama aracılarının Önizleme sürümleri, yüksek kullanılabilirlik için ek sunuculara yüklenemez.

## <a name="check-versions-of-your-authentication-agents"></a>Kimlik doğrulama aracılarının sunucu sürümlerini denetleyin

### <a name="step-1-check-where-your-authentication-agents-are-installed"></a>1. adım: onay, kimlik doğrulama aracılarının yüklü olduğu

Kimlik doğrulama aracılarının yüklü olduğu denetlemek için aşağıdaki adımları izleyin:

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) kiracınız için genel yönetici kimlik bilgileriyle.
2. Seçin **Azure Active Directory** sol gezinti.
3. Seçin **Azure AD Connect**. 
4. Seçin **geçişli kimlik doğrulaması**. Bu dikey pencere, kimlik doğrulama aracılarının yüklü olduğu sunucuları listeler.

![Azure Active Directory Yönetim Merkezi - geçişli kimlik doğrulaması dikey penceresi](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

### <a name="step-2-check-the-versions-of-your-authentication-agents"></a>2. adım: kimlik doğrulaması aracılarınızı sunucu sürümlerini denetleyin

Önceki adımda belirlenen her bir sunucuda, kimlik doğrulama aracılarının sürümlerini denetlemek için bu yönergeleri izleyin:

1. Git **Denetim Masası -> Programlar -> Programlar ve Özellikler** şirket içi sunucusunda.
2. Bir giriş için ise "**Microsoft Azure AD Connect kimlik doğrulaması Aracısı**", bu sunucu üzerinde herhangi bir eylemde bulunmanız gerekmez.
3. Bir giriş için ise "**Microsoft Azure AD uygulama ara sunucusu Bağlayıcısı**", bu sunucuda el ile yükseltmeniz gerekir.

![Kimlik doğrulaması Aracısı Önizleme sürümü](./media/active-directory-aadconnect-pass-through-authentication/pta6.png)

## <a name="best-practices-to-follow-before-starting-the-upgrade"></a>Yükseltme işlemine başlamadan önce izlenecek en iyi yöntemler

Yükseltmeden önce aşağıdakileri sağladığınızdan emin olun:

1. **Yalnızca bulut genel yöneticisi hesabı daha oluşturmanızı**: Burada, geçişli kimlik doğrulama aracılarının çalışmıyor düzgün acil durumlarda kullanmak üzere bir yalnızca bulut genel yönetici hesabı zorunda kalmadan yükseltmeyin. Hakkında bilgi edinin [bir yalnızca bulut genel yönetici hesabı ekleyerek](../active-directory-users-create-azure-portal.md). Bu adımı uygulamadan kritik öneme sahiptir ve bu ayar, kiracınızın dışında kilitli kalmamanızı sağlar.
2.  **Yüksek kullanılabilirlik sağlamak**: daha önce tamamlanmamış, ikinci bir tek başına bu kullanarak oturum açma istekleri için yüksek kullanılabilirlik sağlamak için kimlik doğrulama aracısı yükleyin [yönergeleri](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability).

## <a name="upgrading-the-authentication-agent-on-your-azure-ad-connect-server"></a>Azure AD Connect sunucunuzda kimlik doğrulaması Aracısı'nı yükseltme

Aynı sunucuda kimlik doğrulaması Aracısı yükseltmeden önce Azure AD Connect'e yükseltmenin. Hem birincil hem de Azure AD Connect sunucuları hazırlama üzerinde aşağıdaki adımları izleyin:

1. **Azure AD Connect'e yükseltmenin**: izleyin [makale](./active-directory-aadconnect-upgrade-previous-version.md) ve en son Azure AD Connect sürümünüzü yükseltin.
2. **Kimlik doğrulaması Aracısı Önizleme sürümü kaldırmalı**: indirme [bu PowerShell Betiği](https://aka.ms/rmpreviewagent) ve sunucuda yönetici olarak çalıştırın.
3. **Kimlik doğrulaması Aracısı'nın son sürümünü indirin (sürümleri 1.5.193.0 veya üzeri)**: oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) kiracınızın genel yönetici kimlik bilgilerine sahip. Seçin **Azure Active Directory -> Azure AD Connect geçişli kimlik doğrulaması -> indirme aracı ->**. Kabul [hizmet koşullarını](https://aka.ms/authagenteula) ve kimlik doğrulaması Aracısı'nın son sürümünü indirin. Ayrıca kimlik doğrulaması Aracısı'ndan indirebilirsiniz [burada](https://aka.ms/getauthagent).
4. **Kimlik doğrulaması Aracısı'nın en son sürümü yükleyin**: 3. adımda indirdiğiniz yürütülebilir dosyayı çalıştırın. İstendiğinde, kiracınızın genel yönetici kimlik bilgilerini sağlayın.
5. **En son sürümü yüklü olduğunu doğrulamak**: önce gösterildiği gibi Git **Denetim Masası -> Programlar -> Programlar ve Özellikler** ve için bir giriş olduğundan emin olun "**Microsoft Azure AD Connect Kimlik Doğrulama Aracısı**".

>[!NOTE]
>Geçişli kimlik doğrulaması blade onaylarsanız [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) adımlar önceki tamamladıktan sonra sunucu - kimlik doğrulaması gösteren bir giriş başına iki kimlik doğrulama Aracısı girdi görürsünüz. Aracı olarak **etkin** ve diğer **etkin olmayan**. Bu _beklenen_. **Inactive** giriş birkaç gün sonra otomatik olarak bırakıldı.

## <a name="upgrading-the-authentication-agent-on-other-servers"></a>Diğer sunucularda kimlik doğrulaması Aracısı'nı yükseltme

(Azure AD Connect değil yüklendiği) diğer tüm sunucularda kimlik doğrulaması aracıları yükseltmek için aşağıdaki adımları izleyin:

1. **Kimlik doğrulaması Aracısı Önizleme sürümü kaldırmalı**: indirme [bu PowerShell Betiği](https://aka.ms/rmpreviewagent) ve sunucuda yönetici olarak çalıştırın.
2. **Kimlik doğrulaması Aracısı'nın son sürümünü indirin (sürümleri 1.5.193.0 veya üzeri)**: oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) kiracınızın genel yönetici kimlik bilgilerine sahip. Seçin **Azure Active Directory -> Azure AD Connect geçişli kimlik doğrulaması -> indirme aracı ->**. Hizmet koşullarını kabul edin ve en son sürümünü indirin.
3. **Kimlik doğrulaması Aracısı'nın en son sürümü yükleyin**: 2. adımda indirdiğiniz yürütülebilir dosyayı çalıştırın. İstendiğinde, kiracınızın genel yönetici kimlik bilgilerini sağlayın.
4. **En son sürümü yüklü olduğunu doğrulamak**: önce gösterildiği gibi Git **Denetim Masası -> Programlar -> Programlar ve Özellikler** ve adlı bir giriş olduğundan emin olun **Microsoft Azure AD Connect Kimlik Doğrulama Aracısı**.

>[!NOTE]
>Geçişli kimlik doğrulaması blade onaylarsanız [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) adımlar önceki tamamladıktan sonra sunucu - kimlik doğrulaması gösteren bir giriş başına iki kimlik doğrulama Aracısı girdi görürsünüz. Aracı olarak **etkin** ve diğer **etkin olmayan**. Bu _beklenen_. **Inactive** giriş birkaç gün sonra otomatik olarak bırakıldı.

## <a name="next-steps"></a>Sonraki adımlar
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -özelliği ile ilgili yaygın sorunları çözmeyi öğrenin.
