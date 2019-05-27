---
title: Azure Active Directory ile karma kimlik nedir?
description: Karma kimlik hem şirket içi kimlik doğrulaması ve yetkilendirme için genel bir kullanıcı kimliği olan ve bulut.
keywords: Azure AD Connect’e giriş, Azure AD Connect’e genel bakış, Azure AD Connect nedir, active directory yükleme
services: active-directory
author: billmath
manager: daveba
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 05/17/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c43238d44b2309d105ef14e696a5a16848d0b58
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65896824"
---
# <a name="what-is-hybrid-identity-with-azure-active-directory"></a>Azure Active Directory ile karma kimlik nedir?

Bugün, şirketler ve kuruluşlar şirket içi bir karışımını daha gelmektedir ve bulut uygulamalarına.  Kullanıcılar bu uygulamaları hem şirket içi erişim gerektirir ve bulut. Hem şirket içi kullanıcıları yönetme ve senaryoları zor bulut yürütmelisiniz. 

Microsoft'un kimlik çözümleri, şirket içi ve bulut tabanlı özellikleri kapsar.  Kimlik doğrulaması ve yetkilendirme konumdan bağımsız olarak tüm kaynaklara genel bir kullanıcı kimliği bu çözümler oluşturun. Bu diyoruz **karma kimlik**.

Karma kimlik Azure AD'ye ve karma Kimlik Yönetimi bu senaryoları mümkün olur.

Azure AD ile karma kimlik elde etmek için üç kimlik doğrulama yöntemlerinden biri, senaryolarınıza bağlı olarak kullanılabilir.   Üç yöntem şunlardır: 

- **[Parola Karması eşitleme (PHS)](whatis-phs.md)**  
- **[Geçişli kimlik doğrulaması (PTA)](how-to-connect-pta.md)**  
- **[Federasyon (AD FS)](whatis-fed.md)** 

Bu kimlik doğrulama yöntemleri de sağlamak [çoklu oturum açma](how-to-connect-sso.md) özellikleri.  Çoklu oturum açma, kullanıcılarınızın şirket ağınıza bağlı cihazlarından Kurumsal olduklarında, otomatik olarak imzalar.

Ek bilgi için bkz: [, Azure Active Directory karma kimlik çözümü için doğru kimlik doğrulama yöntemini seçin](https://docs.microsoft.com/azure/security/azure-ad-choose-authn). 

## <a name="common-scenarios-and-recommendations"></a>Ortak senaryolar ve öneriler 

Aşağıda, bazı yaygın karma kimlik ve erişim yönetimi senaryoları, hangi karma kimlik seçeneğinin (veya seçeneklerinin) hangi senaryoya uygun olabileceği hakkında önerilerle birlikte verilmiştir. 

|Şunu istiyorum:|PHS ve SSO<sup>1</sup>| PTA ve SSO<sup>2</sup> | AD FS<sup>3</sup>| 
|-----|-----|-----|-----| 
|Şirket içi Active Directory ortamında oluşturulan yeni kullanıcı, kişi ve grup hesaplarının otomatik olarak bulutla eşitlenmesi.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Kiracıma Office 365 karma senaryolar için ayarlayın.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Kullanıcılarımın oturum açın ve şirket içi parolalarını kullanarak bulut hizmetlerine erişim sağlar.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Tek şirket kimlik bilgilerini kullanarak oturum açmayı uygulayın.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|  
|Bulutta hiç parola karmaları depolanır emin olun.| |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Çok faktörlü kimlik doğrulaması bulut tabanlı çözümler sağlar.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Şirket içi multi-Factor authentication çözümleri sağlar.| | |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Kullanıcılarım için akıllı kart kimlik doğrulamasını destekler. <sup>4</sup>| | |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Office Portalı'nda ve Windows 10 Masaüstü parola süre sonu bildirimleri görüntüleyin.| | |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 

> <sup>1</sup> Çoklu oturum açma ile parola karması eşitleme. 
> 
> <sup>2</sup> Doğrudan kimlik doğrulama ve çoklu oturum açma.  
> 
> <sup>3</sup> AD FS ile federasyon çoklu oturum açma.  
>  
> <sup>4</sup> AD FS, kurumsal PKI çözümünüzle tümleştirilerek sertifika ile oturum açma imkanı sunulabilir. Bu sertifikalar MDM veya GPO gibi güvenilen sağlama kanalları aracılığıyla dağıtılan yazılımsal sertifikalar, akıllı kart sertifikaları (PIV/CAC kartları dahil) veya İç için Hello (sertifika güveni) olabilir. Akıllı kart kimlik doğrulaması desteği hakkında daha fazla bilgi için [bu bloga](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/) bakın. 
> 

## <a name="license-requirements-for-using-azure-ad-connect"></a>Azure AD Connect kullanarak için lisans gereksinimleri

[!INCLUDE [active-directory-free-license.md](../../../includes/active-directory-free-license.md)]

## <a name="next-steps"></a>Sonraki Adımlar 

- [Azure AD Connect ve Connect Health nedir?](whatis-azure-ad-connect.md) 
- [Parola Karması eşitleme (PHS) nedir?](whatis-phs.md) 
- [Geçişli kimlik doğrulaması (PTA) nedir?](how-to-connect-pta.md) 
- [Federasyon nedir?](whatis-fed.md) 
- [Üzerinde çoklu oturum nedir?](how-to-connect-sso.md) 

