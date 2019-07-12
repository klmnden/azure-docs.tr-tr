---
title: Bir Azure kiracısı için çok kiracılı bir uygulama oluşturma
description: Azure Active Directory ile tümleştirme, bağımsız yazılım satıcıları için yönergeler
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: 69cc625500af60a753ad8e7db0363954088f3307
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659586"
---
# <a name="create-an-azure-tenant-for-a-multi-tenant-application"></a>Bir Azure kiracısı için çok kiracılı bir uygulama oluşturma  

Çok kiracılı uygulamanızın erişim sağlamak için uygulamayı kaydetme ve müşterinizin Kimlik Federasyonu etkinleştirmek için bir Azure Active Directory Kiracı oluşturmanız gerekir. Bkz: [çok kiracılı uygulamanız için doğru Federasyon protokolünü seçme](isv-choose-multi-tenant-federation.md). Bu Kiracı, uygulamanızı ve Federasyon müşteriler Azure AD ortamınızı benzer bir ortamda test etmek izin verir.

## <a name="costs-of-hosting-a-multi-tenant-application"></a>Çok kiracılı bir uygulama barındırma maliyetlerini

Azure Active Directory, üç SKU'ları, ücretsiz, temel ve Premium'da kullanılabilir. [Ayrıntılı bir karşılaştırmasını görmek](https://azure.microsoft.com/pricing/details/active-directory/).

Azure aboneliğinizi ve Azure active directory ücretsiz olarak oluşturun ve temel özellikleri kullanın.

## <a name="create-your-tenant"></a>Kiracı oluşturma

1. Kiracınızın oluşturun. Bkz: [geliştirme ortamını ayarlamak](../develop/quickstart-create-new-tenant.md).

2. Etkinleştirin ve uygulamanıza çoklu oturum açma erişimi test etme

   a. **OIDC veya Oath uygulamalarınız için**, [uygulamanızı kaydetmeniz](../develop/quickstart-register-app.md) çok kiracılı bir uygulama olarak. Tüm kurumsal dizinde ve kişisel Microsoft hesapları seçeneği desteklenen hesap türlerinde hesaplarını seçin

   b. **SAML ve WS-Fed-tabanlı uygulamalar için**, size [yapılandırma SAML tabanlı çoklu oturum açma](configure-single-sign-on-non-gallery-applications.md) Azure AD'de SAML genel bir şablon kullanan uygulamalar.

Ayrıca [çok kiracılı tek kiracılı bir uygulamaya dönüştürme](../develop/howto-convert-app-to-be-multi-tenant.md) gerekirse.

## <a name="next-steps"></a>Sonraki Adımlar

[Uygulamanıza SSO tümleştirin](isv-sso-content.md)
