---
title: "Azure AD Connect - doğrudan kimlik doğrulama - yükseltme auth aracıları | Microsoft Docs"
description: "Bu makalede, Azure Active Directory (Azure AD) doğrudan kimlik doğrulama yapılandırması yükseltme açıklar."
services: active-directory
keywords: "Azure AD Connect doğrudan kimlik doğrulama, Active Directory yükleyin gerekli bileşenleri Azure AD, SSO, çoklu oturum açma"
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2018
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: f0a254b7216ca6fda40e26bafb7de57e796a5218
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="azure-active-directory-pass-through-authentication-upgrade-preview-authentication-agents"></a>Azure Active Directory doğrudan kimlik doğrulaması: Yükseltme Önizleme kimlik doğrulama Aracısı

## <a name="overview"></a>Genel Bakış

Bu makalede Önizleme aracılığıyla Azure AD geçişli kimlik doğrulaması kullanan müşteriler içindir. Biz son yükseltme (ve rebranded) kimlik doğrulama Aracısı yazılım. Yapmanız _el ile_ yükseltme Önizleme kimlik doğrulaması, şirket içi sunucularınızda yüklü aracıları. Bir kerelik eylem yalnızca bu el ile yükseltmedir. Kimlik doğrulama aracılara tüm gelecekteki güncelleştirmeleri otomatik olarak yapılır. Yükseltme nedenleri aşağıdaki gibidir:

- Kimlik Doğrulama Aracısı Önizleme sürümleri, herhangi bir ek güvenlik veya hata düzeltmeleri almazsınız.
-   Kimlik Doğrulama Aracısı Önizleme sürümleri, yüksek kullanılabilirlik için ek sunuculara yüklenemez.

## <a name="check-versions-of-your-authentication-agents"></a>Kimlik doğrulama aracılarınızı sürümlerini denetleyin

### <a name="step-1-check-where-your-authentication-agents-are-installed"></a>1. adım: kimlik doğrulaması aracılarınızı yüklendiği onay

Kimlik doğrulama aracılarınızı yüklendiği denetlemek için aşağıdaki adımları izleyin:

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) kiracınız için genel yönetici kimlik bilgilerine sahip.
2. Seçin **Azure Active Directory** sol gezinti çubuğunda.
3. Seçin **Azure AD Connect**. 
4. Seçin **doğrudan kimlik doğrulama**. Bu dikey kimlik doğrulaması aracılarınızı yüklendiği sunucuları listeler.

![Azure Active Directory Yönetim Merkezi - doğrudan kimlik doğrulama dikey penceresi](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

### <a name="step-2-check-the-versions-of-your-authentication-agents"></a>2. adım: kimlik doğrulaması aracılarınızı sürümlerini denetleyin

Önceki adımda tanımlanan her bir sunucuda kimlik doğrulama aracılarınızı sürümlerini denetlemek için bu yönergeleri izleyin:

1. Git **Denetim Masası -> Programlar -> Programlar ve Özellikler** şirket içi sunucuda.
2. İçin bir giriş ise "**Microsoft Azure AD Connect kimlik doğrulama Aracısı**", bu sunucu üzerinde herhangi bir eylemde bulunmanız gerekmez.
3. İçin bir giriş ise "**Microsoft Azure AD uygulama ara sunucusu Bağlayıcısı**", sürüm 1.5.132.0 veya önceki sürümleri, bu sunucuda el ile yükseltmeniz gerekir.

![Kimlik Doğrulama Aracısı'nın Önizleme sürümü](./media/active-directory-aadconnect-pass-through-authentication/pta6.png)

## <a name="best-practices-to-follow-before-starting-the-upgrade"></a>Yükseltme işlemine başlamadan önce izlenecek en iyi yöntemler

Yükseltmeden önce aşağıdaki öğeleri yerinde olduğundan emin olun:

1. **Yalnızca bulut genel yönetici hesabı oluşturma**: Burada doğrudan kimlik doğrulama aracılarınızı değil düzgün çalıştığını acil durumlarda kullanmak üzere bir yalnızca bulut genel yönetici hesabı gerek kalmadan yükseltmeyin. Hakkında bilgi edinin [bir yalnızca bulut genel yönetici hesabı ekleme](../active-directory-users-create-azure-portal.md). Bu adımı uygulamadan önemlidir ve, kiracınızın dışında erişebilmenizin sağlar.
2.  **Yüksek kullanılabilirlik sağlamak**: daha önce tamamlanmamış, ikinci bir tek başına bu kullanarak oturum açma istekleri için yüksek kullanılabilirlik sağlamak için kimlik doğrulama aracısı yükleyin. [yönergeleri](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="upgrading-the-authentication-agent-on-your-azure-ad-connect-server"></a>Azure AD Connect sunucunuzda kimlik doğrulama Aracısı yükseltme

Azure AD Connect aynı sunucuda kimlik doğrulama Aracısı yükseltmeden önce yükseltmeniz. Hem birincil hem de Azure AD Connect sunucuları hazırlama üzerinde aşağıdaki adımları izleyin:

1. **Azure AD Connect yükseltme**: izleyin [makale](./active-directory-aadconnect-upgrade-previous-version.md) ve en son Azure AD Connect sürümüne yükseltme.
2. **Kimlik Doğrulama Aracısı önizleme sürümünü kaldırın**: indirme [bu PowerShell Betiği](https://aka.ms/rmpreviewagent) ve sunucuda yönetici olarak çalıştırın.
3. **Kimlik Doğrulama Aracısı'nın en son sürümü karşıdan yüklemek (sürümleri 1.5.193.0 veya üstü)**: oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) , kiracının genel yönetici kimlik bilgilerine sahip. Seçin **Azure Active Directory -> Azure AD Connect doğrudan kimlik doğrulama -> -> Yükleme Aracısı**. Kabul [hizmet koşulları](https://aka.ms/authagenteula) ve kimlik doğrulama Aracısı en son sürümünü yükleyin. Kimlik Doğrulama Aracısı'ndan de indirebilirsiniz [burada](https://aka.ms/getauthagent).
4. **Kimlik Doğrulama Aracısı en son sürümünü yüklemek**: adım 3'te indirdiğiniz yürütülebilir dosyayı çalıştırmak. İstendiğinde, kiracının genel Yöneticisi kimlik bilgilerini sağlayın.
5. **En son sürümü yüklü olduğunu doğrulayın**: önce gösterildiği gibi Git **Denetim Masası -> Programlar -> Programlar ve Özellikler** ve için bir giriş olduğundan emin olun "**Microsoft Azure AD Connect kimlik doğrulama Aracısı**".

>[!NOTE]
>Doğrudan kimlik doğrulama dikey işaretlerseniz [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) önceki tamamlama adımları sonra sunucu - kimlik doğrulama aracısı olarak gösteren bir giriş başına iki kimlik doğrulama Aracısı giriş görürsünüz **etkin** ve diğer olarak **devre dışı**. Bu _beklenen_. **Devre dışı** girişi birkaç gün sonra otomatik olarak bırakıldı.

## <a name="upgrading-the-authentication-agent-on-other-servers"></a>Diğer sunucularda kimlik doğrulama Aracısı yükseltme

(Burada Azure AD Connect yüklenmemiş) diğer sunucularda kimlik doğrulaması aracıları yükseltmek için aşağıdaki adımları izleyin:

1. **Kimlik Doğrulama Aracısı önizleme sürümünü kaldırın**: indirme [bu PowerShell Betiği](https://aka.ms/rmpreviewagent) ve sunucuda yönetici olarak çalıştırın.
2. **Kimlik Doğrulama Aracısı'nın en son sürümü karşıdan yüklemek (sürümleri 1.5.193.0 veya üstü)**: oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) , kiracının genel yönetici kimlik bilgilerine sahip. Seçin **Azure Active Directory -> Azure AD Connect doğrudan kimlik doğrulama -> -> Yükleme Aracısı**. Hizmet koşullarını kabul edin ve en son sürümü yükleyin.
3. **Kimlik Doğrulama Aracısı en son sürümünü yüklemek**: 2. adımda indirdiğiniz yürütülebilir dosyayı çalıştırmak. İstendiğinde, kiracının genel Yöneticisi kimlik bilgilerini sağlayın.
4. **En son sürümü yüklü olduğunu doğrulayın**: önce gösterildiği gibi Git **Denetim Masası -> Programlar -> Programlar ve Özellikler** ve adlı bir giriş olduğundan emin olun **Microsoft Azure AD Connect kimlik doğrulama Aracısı**.

>[!NOTE]
>Doğrudan kimlik doğrulama dikey işaretlerseniz [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) önceki tamamlama adımları sonra sunucu - kimlik doğrulama aracısı olarak gösteren bir giriş başına iki kimlik doğrulama Aracısı giriş görürsünüz **etkin** ve diğer olarak **devre dışı**. Bu _beklenen_. **Devre dışı** girişi birkaç gün sonra otomatik olarak bırakıldı.

## <a name="next-steps"></a>Sonraki adımlar
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -özelliği ile ilgili genel sorunları çözmeyi öğrenin.
