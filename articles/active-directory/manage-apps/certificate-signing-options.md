---
title: Gelişmiş Sertifika İmzalama Seçenekleri Azure Active Directory'de önceden tümleştirilmiş uygulamalar için SAML belirtecinde | Microsoft Docs
description: Gelişmiş Sertifika İmzalama Seçenekleri Azure Active Directory'de önceden tümleştirilmiş uygulamalar için SAML belirtecinde kullanmayı öğrenin
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: mimart
ms.reviewer: jeedes
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1baf4af41fd5dcb6723b6ee2827ae91b43b072d9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65780959"
---
# <a name="advanced-certificate-signing-options-in-the-saml-token-for-gallery-apps-in-azure-active-directory"></a>Gelişmiş seçenekleri, Azure Active Directory'de galeri uygulamalar için SAML belirtecinde imzalama sertifikası

Bugün Azure Active Directory (Azure AD), Azure Active Directory Uygulama galerisinde önceden tümleştirilmiş binlerce uygulamasına destekler. Çoklu oturum açma desteği kullanarak uygulamaların 500'den [Security Assertion Markup Language](https://wikipedia.org/wiki/Security_Assertion_Markup_Language) (SAML) 2.0 protokolü, gibi [muhasebesi](https://azuremarketplace.microsoft.com/marketplace/apps/aad.netsuite) uygulama. Bir müşteri SAML kullanarak bir uygulamaya Azure AD üzerinden kimliğini doğrular, Azure AD belirteç (bir HTTP POST) aracılığıyla uygulamaya gönderir. Uygulama doğrular ve belirteci bir kullanıcı adı ve parola bilgilerini isteyen yerine müşteri imzalamak için kullanır. Bu SAML belirteçlerini, Azure AD'de ve belirli standart algoritmalarda tarafından oluşturulan benzersiz sertifika ile imzalanmış.

Azure AD galeri uygulamalar için bazı varsayılan ayarları kullanır. Varsayılan değerleri üzerinde uygulama gereksinimlerine göre ayarlanır.

Azure AD'de imzalama seçenekleri sertifika ve sertifika imzalama algoritmasını ayarlayabilirsiniz.

## <a name="certificate-signing-options"></a>Sertifika imzalama seçenekleri

Azure AD'ye üç sertifika imzalama seçenekleri destekler:

* **SAML onaylaması oturum**. Bu varsayılan seçeneği galeri uygulamalarının çoğu için ayarlanır. Bu seçeneği belirlerseniz, Azure AD'yi kimlik sağlayıcısı (IDP) olarak SAML onaylama işlemi ve sertifika ile imzalar [X.509](https://wikipedia.org/wiki/X.509) uygulamanın sertifika.

* **SAML yanıtını oturum**. Bu seçeneği belirlerseniz, bir Azure AD'ye SAML yanıtını uygulamanın X.509 sertifikası ile imzalar.

* **SAML yanıtını ve onayını imzala**. Bu seçeneği belirlerseniz, bir Azure AD uygulama X.509 sertifikası ile tüm SAML belirteci imzalar.

## <a name="certificate-signing-algorithms"></a>Sertifika imzalama algoritmaları

Azure AD, iki imzalama algoritmaları veya SAML yanıtını oturum açmak için güvenli karma algoritması (SHA) destekler:

* **SHA-256'YI**. Azure AD, SAML yanıtını oturum açmak için bu varsayılan algoritma kullanır. Bu yeni algoritma ve SHA-1'den daha güvenlidir. Uygulamaların çoğu, SHA-256 algoritmasını destekler. Bir uygulama yalnızca SHA-1 olarak imza algoritmasını destekliyorsa, bunu değiştirebilirsiniz. Aksi takdirde, SAML yanıtını imzalamak için SHA-256 algoritmasını kullanmanızı öneririz.

* **SHA-1**. Bu algoritma eski ve daha az kabul edilir SHA-256'yı güvenlidir. Bir uygulama bu imza algoritmasını destekliyorsa, bu seçeneği seçebilirsiniz **imzalama algoritması** aşağı açılan listesi. Ardından Azure AD ile SHA-1 algoritmasını SAML yanıtını imzalar.

## <a name="change-certificate-signing-options-and-signing-algorithm"></a>İmzalama seçenekleri ve imza algoritması sertifikasını değiştirme

Uygulama SAML sertifika imzalama seçenekleri ve sertifika imzalama algoritmasını değiştirmek için söz konusu uygulamayı seçin:

1. İçinde [Azure Active Directory portalında](https://aad.portal.azure.com/), hesabınızda oturum açın. **Azure Active Directory Yönetim Merkezi** sayfası görüntülenir.
1. Sol bölmede **Kurumsal uygulamalar**’ı seçin. Hesabınızdaki kurumsal uygulamalar listesi görüntülenir.
1. Bir uygulama seçin. Uygulama için bir genel bakış sayfası görüntülenir.

   ![Uygulama genel bakış sayfası](./media/certificate-signing-options/application-overview-page.png)

Ardından, sertifika imzalama seçenekleri bu uygulama için SAML belirtecinde değiştirin:

1. Uygulama genel bakış sayfasının sol bölmesinde seçin **çoklu oturum açma**.

2. Varsa **yukarı çoklu oturum açma SAML - Preview ile ayarlanmış** sayfası görüntülenirse, 5. adıma gidin.

3. Varsa **tek bir oturum açma yönteminizi seçmeniz** sayfa görünmüyorsa, seçin **değiştirme tek oturum açma modları** sayfayı görüntülemek için.

4. İçinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML** varsa. (Varsa **SAML** değilse kullanılabilir, uygulamanın SAML da desteklemiyor ve bu yordam ve makalenin geri kalanı göz ardı edebilirsiniz.)

5. İçinde **yukarı çoklu oturum açma SAML - Preview ile ayarlanmış** sayfasında, bulmak **SAML imzalama sertifikası** seçin ve başlık **Düzenle** simgesi (Kalem). **SAML imzalama sertifikası** sayfası görüntülenir.

   ![SAML imzalama sayfası](./media/certificate-signing-options/saml-signing-page.png)

6. İçinde **imzalama seçeneği** aşağı açılan listesinde **oturum SAML yanıtını**, **oturum SAML onayı**, veya **oturum SAML yanıtını ve onayını**. Bu seçeneklerin açıklamaları görünür bu makalenin önceki kısımlarında [sertifika imzalama seçenekleri](#certificate-signing-options).

7. İçinde **imzalama algoritması** aşağı açılan listesinde **SHA-1** veya **SHA-256'yı**. Bu seçeneklerin açıklamaları görünür bu makalenin önceki kısımlarında [algoritmaları imzalama sertifikası](#certificate-signing-algorithms) bölümü.

8. Seçimlerinizi ile memnun kalırsanız seçin **Kaydet** yeni SAML imzalama sertifikası ayarlarını uygulamak için. Aksi takdirde seçin **X** değişiklikleri atmak için.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory Uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açma yapılandırma](configure-federated-single-sign-on-non-gallery-applications.md)
* [SAML tabanlı çoklu oturum açma sorunlarını giderme](../develop/howto-v1-debug-saml-sso-issues.md)
