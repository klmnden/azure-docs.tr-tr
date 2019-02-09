---
title: Gelişmiş Sertifika İmzalama Seçenekleri Azure Active Directory'de önceden tümleştirilmiş uygulamalar için SAML belirtecinde | Microsoft Docs
description: Gelişmiş Sertifika İmzalama Seçenekleri Azure Active Directory'de önceden tümleştirilmiş uygulamalar için SAML belirtecinde kullanmayı öğrenin
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.reviewer: jeedes
ms.custom: aaddev
ms.openlocfilehash: 667eb7204669d0d94903fa652956e7671a9a9651
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55963429"
---
# <a name="advanced-certificate-signing-options-in-the-saml-token-for-gallery-apps-in-azure-active-directory"></a>Gelişmiş seçenekleri, Azure Active Directory'de galeri uygulamalar için SAML belirtecinde imzalama sertifikası
Bugün Azure Active Directory (Azure AD), Azure Active Directory Uygulama galerisinde önceden tümleştirilmiş binlerce uygulamasına destekler. Bu sayı SAML 2.0 protokolünü kullanarak çoklu oturum açmayı destekleyen 500'den fazla uygulamalar içerir. SAML kullanarak bir kullanıcının bir uygulamayı Azure AD üzerinden kimlik, Azure AD belirteç (bir HTTP POST) aracılığıyla uygulamaya gönderir. Ardından, uygulamayı doğrular ve yerine bir kullanıcı adı ve parola bilgilerini isteyen kullanıcının oturum belirteci kullanır. Bu SAML belirteçlerini, Azure AD'de ve belirli standart algoritmalarda tarafından oluşturulan benzersiz sertifika ile imzalanmış.

Azure AD galeri uygulamalar için bazı varsayılan ayarları kullanır. Varsayılan değerleri üzerinde uygulama gereksinimlerine göre ayarlanır.

Azure AD, gelişmiş sertifika imzalama ayarlarını destekler. Bu seçenekleri için önce seçin **gelişmiş sertifika imzalama ayarlarını göster** onay kutusunu:

![Gelişmiş sertifika imzalama ayarlarını göster](./media/certificate-signing-options/saml-advance-certificate.png)

Bu onay kutusunu işaretledikten sonra sertifika imzalama seçenekleri ve sertifika imzalama algoritmasını en ayarlayabilirsiniz.

## <a name="certificate-signing-options"></a>Sertifika imzalama seçenekleri

Azure AD'ye üç sertifika imzalama seçenekleri destekler:

* **SAML onaylaması oturum**. Bu varsayılan seçeneği galeri uygulamalarının çoğu için ayarlanır. Bu seçenek belirlenirse, Azure AD'yi bir IDP olarak SAML onaylama işlemi ve X509 sertifikasıyla imzalar. uygulama sertifikası. Ayrıca, seçili imzalama algoritması kullanır **imzalama algoritması** aşağı açılan listesi.

* **SAML yanıtını oturum**. Bu seçenek belirlenirse, bir IDP olarak Azure AD ile X509 SAML yanıtını oturum açtığında uygulamanın sertifika. Ayrıca, seçili imzalama algoritması kullanır **imzalama algoritması** aşağı açılan listesi.

* **SAML yanıtını ve onayını imzala**. Bu seçenek belirlenirse, bir IDP olarak Azure AD ile X509 tüm SAML belirteci imzalar. uygulama sertifikası. Ayrıca, seçili imzalama algoritması kullanır **imzalama algoritması** aşağı açılan listesi.

    ![Sertifika imzalama seçenekleri](./media/certificate-signing-options/saml-signing-options.png)

## <a name="certificate-signing-algorithms"></a>Sertifika imzalama algoritmaları

Azure AD, SAML yanıtını oturum açmak için iki imzalama algoritmaları destekler:

* **SHA-256'YI**. Azure AD, SAML yanıtını oturum açmak için bu varsayılan algoritma kullanır. En yeni algoritma ve daha fazla bilgi kabul edilir güvenli SHA-1'den. Uygulamaların çoğu, SHA-256 algoritmasını destekler. Bir uygulama yalnızca SHA-1 olarak imza algoritmasını destekliyorsa, bunu değiştirebilirsiniz. Aksi takdirde, SAML yanıtını imzalamak için SHA-256 algoritmasını kullanmanızı öneririz.

    ![SHA-256'yı sertifika imza algoritması](./media/certificate-signing-options/saml-signing-algo-sha256.png)

* **SHA-1**. Bu eski algoritması ve daha az kabul edilir SH-256'yı güvenlidir. Bir uygulama bu imza algoritmasını destekliyorsa, bu seçeneği seçebilirsiniz **imzalama algoritması** aşağı açılan listesi. Ardından Azure AD ile SHA-1 algoritmasını SAML yanıtını imzalar.

    ![SHA-1 sertifika imza algoritması](./media/certificate-signing-options/saml-signing-algo-sha1.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Active Directory Uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açma yapılandırma](configure-federated-single-sign-on-non-gallery-applications.md)
* [SAML tabanlı çoklu oturum açma sorunlarını giderme](../develop/howto-v1-debug-saml-sso-issues.md)


