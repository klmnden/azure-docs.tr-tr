---
title: Gelişmiş Sertifika imzalama Azure Active Directory'de önceden tümleştirilen uygulamalar için SAML belirtecinde seçenekleri | Microsoft Docs
description: Gelişmiş Sertifika imzalama Azure Active Directory'de önceden tümleştirilen uygulamalar için SAML belirtecinde seçenekleri kullanmayı öğrenin
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: barbkess
ms.reviewer: jeedes
ms.custom: aaddev
ms.openlocfilehash: 77cf2b1fb6eb4f67c0d6595e244bb317b5e68462
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36332942"
---
# <a name="advanced-certificate-signing-options-in-the-saml-token-for-gallery-apps-in-azure-active-directory"></a>Gelişmiş Seçenekleri Azure Active Directory'de galeri uygulamalar için SAML belirtecinde imzalama sertifikası
Bugün Azure Active Directory (Azure AD) önceden tümleştirilmiş binlerce uygulamasına Azure Active Directory Uygulama galerisinde destekler. Bu sayı SAML 2.0 protokolü kullanarak çoklu oturum açma desteği 500'den fazla uygulama içerir. Bir kullanıcı Azure AD üzerinden bir uygulama SAML kullanarak kimlik doğrulaması gerçekleştirdiğinde, Azure AD bir belirteç (aracılığıyla bir HTTP POST) uygulamaya gönderir. Ardından, uygulama doğrular ve bir kullanıcı adı ve parola istemek yerine kullanıcı oturum belirteci kullanır. Bu SAML belirteçleri, Azure AD'de ve belirli standart algoritmaların tarafından oluşturulan benzersiz sertifikayla imzalanmış.

Azure AD galeri uygulamalar için bazı varsayılan ayarları kullanır. Varsayılan değerleri uygulamanın gereksinimlerine göre ayarlanır.

Azure AD ayarları imzalama Gelişmiş Sertifika destekler. Bu seçenekler seçmek için önce seçin **Göster gelişmiş sertifika imzalama ayarları** onay kutusunu:

![Gelişmiş sertifika imzalama ayarlarını göster](./media/certificate-signing-options/saml-advance-certificate.png)

Bu onay kutusunu işaretledikten sonra seçenekleri imzalama sertifikası ve sertifika imzalama algoritmasını ayarlayabilirsiniz.

## <a name="certificate-signing-options"></a>Sertifika İmzalama Seçenekleri

Azure AD üç sertifika imzalama seçeneklerini destekler:

* **Oturum SAML onayı**. Bu varsayılan seçeneği galeri uygulamalarının çoğu için ayarlanır. Bu seçenek belirlenirse, bir IDP olarak Azure AD ile SAML onayı ve X509 sertifikasıyla imzalar uygulamasının sertifikası. Ayrıca, seçili imza algoritmasını kullanan **imzalama algoritması** aşağı açılan liste.

* **SAML yanıtını oturum**. Bu seçenek belirlenirse, bir IDP olarak Azure AD X509 SAML Yanıtla imzalar uygulamasının sertifikası. Ayrıca, seçili imza algoritmasını kullanan **imzalama algoritması** aşağı açılan liste.

* **SAML yanıtını ve onaylama oturum**. Bu seçenek belirlenirse, bir IDP olarak Azure AD ile X509 tüm SAML belirteci imzalar uygulamasının sertifikası. Ayrıca, seçili imza algoritmasını kullanan **imzalama algoritması** aşağı açılan liste.

    ![Sertifika İmzalama Seçenekleri](./media/certificate-signing-options/saml-signing-options.png)

## <a name="certificate-signing-algorithms"></a>Algoritmalar imzalama sertifikası

Azure AD SAML yanıtını imzalamak için iki imzalama algoritmasını destekler:

* **SHA-256**. Azure AD bu varsayılan algoritma SAML yanıtını imzalamak için kullanır. En yeni algoritması ve daha fazla bilgi kabul edilir güvenli SHA-1'den. Uygulamaların çoğu SHA-256 algoritmasını destekler. Bir uygulamayı yalnızca SHA-1 olarak imzalama algoritmasını destekliyorsa, değiştirebilirsiniz. Aksi takdirde, SAML yanıtını imzalamak için SHA-256 algoritmasını kullanmanızı öneririz.

    ![SHA-256 sertifika imza algoritması](./media/certificate-signing-options/saml-signing-algo-sha256.png)

* **SHA-1**. Bu eski algoritması ve küçük işlem görür SH-256 güvenlidir. Bir uygulamayı yalnızca bu imzalama algoritmasını destekliyorsa, bu seçeneği seçebilirsiniz **imzalama algoritması** aşağı açılan liste. Ardından Azure AD SHA-1 algoritmasını SAML Yanıtla imzalar.

    ![SHA-1 sertifika imza algoritması](./media/certificate-signing-options/saml-signing-algo-sha1.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Active Directory'de uygulama yönetimi için makale dizini](../active-directory-apps-index.md)
* [Çoklu oturum açma Azure Active Directory Uygulama galerisinde bulunmayan uygulamalar için yapılandırma](../application-config-sso-how-to-configure-federated-sso-non-gallery.md)
* [SAML tabanlı çoklu oturum açma sorunlarını giderme](../develop/active-directory-saml-debugging.md)


