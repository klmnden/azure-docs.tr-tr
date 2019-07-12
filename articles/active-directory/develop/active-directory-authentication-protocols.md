---
title: Azure Active Directory kimlik doğrulama protokolleri | Microsoft Docs
description: Azure Active Directory (AD tarafından) desteklenen kimlik doğrulama protokolleri genel bakış
documentationcenter: dev-center-name
author: rwike77
services: active-directory
manager: CelesteDG
editor: ''
ms.assetid: 7a838ae2-c24c-4304-b6c0-e77fb888e6c0
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: hirsin
ms.collection: M365-identity-device-management
ms.openlocfilehash: f964c5882432ae0637039e32ca961008e8223b6b
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612574"
---
# <a name="azure-active-directory-authentication-protocols"></a>Azure Active Directory kimlik doğrulama protokolleri
Azure Active Directory (Azure AD) çeşitli en yaygın olarak kullanılan kimlik doğrulama ve yetkilendirme protokolünü destekler. Bu bölümdeki konularda, desteklenen protokoller ve bunların uygulama Azure AD'de açıklanmaktadır. Konular, desteklenen talep türleri, Federasyon meta verilerinin kullanımı Giriş incelenmesi dahil, OAuth 2.0 ayrıntılı. ve SAML 2.0 protokolü başvurusu belgeleri ve sorun giderme bölümü.

## <a name="authentication-protocols-articles-and-reference"></a>Makaleler ve başvuru kimlik doğrulama protokolleri
* [Önemli bilgiler hakkında imzalama anahtar geçişi Azure AD'de](active-directory-signing-key-rollover.md) – Azure AD'nin imzalama anahtar geçişi uyumu, anahtarı otomatik olarak güncelleştirmek için yaptığınız değişiklikler ve tartışma için en yaygın uygulama senaryoları güncelleştirme hakkında bilgi edinin.
* [Desteklenen belirteç ve talep türleri](v1-id-and-access-tokens.md) -Azure AD verdiği belirteçlere talep hakkında bilgi edinin.
* [Federasyon meta verileri](azure-ad-federation-metadata.md) -bulun ve Azure AD oluşturan meta veri belgelerini yorumlama hakkında bilgi edinin.
* [OAuth 2.0, Azure AD'de](v1-protocols-oauth-code.md) -OAuth 2.0, Azure AD'de uygulama hakkında bilgi edinin.
* [Openıd Connect 1.0](v1-protocols-openid-connect-code.md) -kimlik doğrulaması için OAuth 2.0 Yetkilendirme Protokolü kullanmayı öğrenin.
* [İstemci kimlik bilgileri ile hizmetten hizmete çağrılar](v1-oauth2-client-creds-grant-flow.md) -hizmet çağrıları için OAuth 2.0 istemci kimlik bilgileri verme akışı kullanmayı öğrenin.
* [Hizmetten hizmete çağrılar On-Behalf-Of Flow ile](v1-oauth2-on-behalf-of-flow.md) -hizmet çağrıları için OAuth 2.0 On-Behalf-Of akış'ı kullanmayı öğrenin.
* [SAML Protokolü başvurusu](active-directory-saml-protocol-reference.md) -Azure AD çoklu oturum açma ve çoklu oturum kapatma SAML profilleri hakkında bilgi edinin.

## <a name="see-also"></a>Ayrıca Bkz.
[Azure Active Directory Geliştirici Kılavuzu](v1-overview.md)

[Active Directory kod örnekleri](sample-v1-code.md)
