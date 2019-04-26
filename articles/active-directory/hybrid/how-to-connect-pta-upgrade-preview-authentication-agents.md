---
title: Azure AD Connect - geçişli kimlik doğrulaması - yükseltme kimlik doğrulama aracılarının | Microsoft Docs
description: Bu makalede, Azure Active Directory (Azure AD) geçişli kimlik doğrulaması yapılandırmanızı yükseltileceği açıklanır.
services: active-directory
keywords: Azure AD, SSO, Azure AD Connect geçişli kimlik doğrulaması, Active Directory Yükleme gerekli bileşenleri çoklu oturum açma
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/27/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 494ccc3b90b8c249ee935087dcf0f0b5264b02ca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60386780"
---
# <a name="azure-active-directory-pass-through-authentication-upgrade-preview-authentication-agents"></a>Azure Active Directory geçişli kimlik doğrulaması: Önizleme kimlik doğrulama aracılarını yükseltme

## <a name="overview"></a>Genel Bakış

Bu makale önizlemesi ile Azure AD geçişli kimlik doğrulaması kullanan müşteriler içindir. Size yakın zamanda yükseltme (ve yeni marka adları verilmiştir) kimlik doğrulama Aracısı yazılım. Şunları yapmanız _el ile_ yükseltme Önizleme kimlik doğrulama aracılarının şirket içi sunucularınız üzerinde yüklü. Bu el ile yükseltme tek seferlik bir eylem var. Kimlik doğrulama aracılarının gelecekteki tüm güncelleştirmeler otomatik olarak yapılır. Yükseltme nedenleri aşağıdaki gibidir:

- Kimlik doğrulama aracılarının Önizleme sürümleri, herhangi bir ek güvenlik veya hata düzeltmesi almazsınız.
-   Kimlik doğrulama aracılarının Önizleme sürümleri, yüksek kullanılabilirlik için ek sunuculara yüklenemez.

## <a name="check-versions-of-your-authentication-agents"></a>Kimlik doğrulama aracılarının sunucu sürümlerini denetleyin

### <a name="step-1-check-where-your-authentication-agents-are-installed"></a>1. Adım: Kimlik doğrulama aracılarının yüklü olduğu denetleyin

Kimlik doğrulama aracılarının yüklü olduğu denetlemek için aşağıdaki adımları izleyin:

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) kiracınız için genel yönetici kimlik bilgileriyle.
2. Seçin **Azure Active Directory** sol gezinti.
3. Seçin **Azure AD Connect**. 
4. Seçin **geçişli kimlik doğrulaması**. Bu dikey pencere, kimlik doğrulama aracılarının yüklü olduğu sunucuları listeler.

![Azure Active Directory Yönetim Merkezi - geçişli kimlik doğrulaması dikey penceresi](./media/how-to-connect-pta-upgrade-preview-authentication-agents/pta8.png)

### <a name="step-2-check-the-versions-of-your-authentication-agents"></a>2. Adım: Kimlik doğrulama aracılarının sunucu sürümlerini denetleyin

Önceki adımda belirlenen her bir sunucuda, kimlik doğrulama aracılarının sürümlerini denetlemek için bu yönergeleri izleyin:

1. Git **Denetim Masası -> Programlar -> Programlar ve Özellikler** şirket içi sunucusunda.
2. Bir giriş için ise "**Microsoft Azure AD Connect kimlik doğrulaması Aracısı**", bu sunucu üzerinde herhangi bir eylemde bulunmanız gerekmez.
3. Bir giriş için ise "**Microsoft Azure AD uygulama ara sunucusu Bağlayıcısı**", bu sunucuda el ile yükseltmeniz gerekir.

![Kimlik doğrulaması Aracısı Önizleme sürümü](./media/how-to-connect-pta-upgrade-preview-authentication-agents/pta6.png)

## <a name="best-practices-to-follow-before-starting-the-upgrade"></a>Yükseltme işlemine başlamadan önce izlenecek en iyi yöntemler

Yükseltmeden önce aşağıdakileri sağladığınızdan emin olun:

1. **Yalnızca bulut genel yöneticisi hesabı daha oluşturmanızı**: Burada, geçişli kimlik doğrulama aracılarının düzgün çalışmıyor acil durumlarda kullanmak üzere bir yalnızca bulut genel yönetici hesabı zorunda kalmadan yükseltmeyin. Hakkında bilgi edinin [bir yalnızca bulut genel yönetici hesabı ekleyerek](../active-directory-users-create-azure-portal.md). Bu adımı uygulamadan kritik öneme sahiptir ve bu ayar, kiracınızın dışında kilitli kalmamanızı sağlar.
2.  **Yüksek kullanılabilirlik sağlamak**: Daha önce tamamlanmamış, ikinci bir tek başına bu kullanarak oturum açma istekleri için yüksek kullanılabilirlik sağlamak için kimlik doğrulama aracısı yükleyin [yönergeleri](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability).

## <a name="upgrading-the-authentication-agent-on-your-azure-ad-connect-server"></a>Azure AD Connect sunucunuzda kimlik doğrulaması Aracısı'nı yükseltme

Aynı sunucuda kimlik doğrulaması Aracısı yükseltmeden önce Azure AD Connect'e yükseltmenin. Hem birincil hem de Azure AD Connect sunucuları hazırlama üzerinde aşağıdaki adımları izleyin:

1. **Yükseltme Azure AD Connect**: İzleyin [makale](how-to-upgrade-previous-version.md) ve en son Azure AD Connect sürümünüzü yükseltin.
2. **Kimlik doğrulaması Aracısı Önizleme sürümü kaldırmalı**: İndirme [bu PowerShell Betiği](https://aka.ms/rmpreviewagent) ve sunucuda yönetici olarak çalıştırın.
3. **Kimlik doğrulaması Aracısı'nın son sürümünü indirin (sürümleri 1.5.389.0 veya üzeri)**: Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) kiracınızın genel yönetici kimlik bilgilerine sahip. Seçin **Azure Active Directory -> Azure AD Connect geçişli kimlik doğrulaması -> indirme aracı ->**. Kabul [hizmet koşullarını](https://aka.ms/authagenteula) ve kimlik doğrulaması Aracısı'nın son sürümünü indirin. Ayrıca kimlik doğrulaması Aracısı'ndan indirebilirsiniz [burada](https://aka.ms/getauthagent).
4. **Kimlik doğrulaması Aracısı'nın en son sürümü yükleyin**: 3. adımda indirdiğiniz yürütülebilir dosyayı çalıştırın. İstendiğinde, kiracınızın genel yönetici kimlik bilgilerini sağlayın.
5. **En son sürümü yüklü olduğunu doğrulamak**: Önce gösterildiği gibi Git **Denetim Masası -> Programlar -> Programlar ve Özellikler** ve için bir giriş olduğundan emin olun "**Microsoft Azure AD Connect kimlik doğrulaması Aracısı**".

>[!NOTE]
>Geçişli kimlik doğrulaması blade onaylarsanız [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) adımlar önceki tamamladıktan sonra sunucu - kimlik doğrulaması gösteren bir giriş başına iki kimlik doğrulama Aracısı girdi görürsünüz. Aracı olarak **etkin** ve diğer **etkin olmayan**. Bu _beklenen_. **Inactive** giriş birkaç gün sonra otomatik olarak bırakıldı.

## <a name="upgrading-the-authentication-agent-on-other-servers"></a>Diğer sunucularda kimlik doğrulaması Aracısı'nı yükseltme

(Azure AD Connect değil yüklendiği) diğer tüm sunucularda kimlik doğrulaması aracıları yükseltmek için aşağıdaki adımları izleyin:

1. **Kimlik doğrulaması Aracısı Önizleme sürümü kaldırmalı**: İndirme [bu PowerShell Betiği](https://aka.ms/rmpreviewagent) ve sunucuda yönetici olarak çalıştırın.
2. **Kimlik doğrulaması Aracısı'nın son sürümünü indirin (sürümleri 1.5.389.0 veya üzeri)**: Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) kiracınızın genel yönetici kimlik bilgilerine sahip. Seçin **Azure Active Directory -> Azure AD Connect geçişli kimlik doğrulaması -> indirme aracı ->**. Hizmet koşullarını kabul edin ve en son sürümünü indirin.
3. **Kimlik doğrulaması Aracısı'nın en son sürümü yükleyin**: 2. adımda indirdiğiniz yürütülebilir dosyayı çalıştırın. İstendiğinde, kiracınızın genel yönetici kimlik bilgilerini sağlayın.
4. **En son sürümü yüklü olduğunu doğrulamak**: Önce gösterildiği gibi Git **Denetim Masası -> Programlar -> Programlar ve Özellikler** ve adlı bir giriş olduğundan emin olun **Microsoft Azure AD Connect kimlik doğrulaması Aracısı**.

>[!NOTE]
>Geçişli kimlik doğrulaması blade onaylarsanız [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) adımlar önceki tamamladıktan sonra sunucu - kimlik doğrulaması gösteren bir giriş başına iki kimlik doğrulama Aracısı girdi görürsünüz. Aracı olarak **etkin** ve diğer **etkin olmayan**. Bu _beklenen_. **Inactive** giriş birkaç gün sonra otomatik olarak bırakıldı.

## <a name="next-steps"></a>Sonraki adımlar
- [**Sorun giderme** ](tshoot-connect-pass-through-authentication.md) -özelliği ile ilgili yaygın sorunları çözmeyi öğrenin.
