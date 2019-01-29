---
title: Azure Active Directory ile Active Directory’yi bağlayın. | Microsoft Docs
description: Azure AD Connect, şirket içi dizinlerinizi Azure Active Directory ile tümleştirir. Bu sayede Azure AD ile tümleşik Office 365, Azure ve SaaS uygulamaları için ortak bir kimlik oluşturabilirsiniz.
keywords: Azure AD Connect’e giriş, Azure AD Connect’e genel bakış, Azure AD Connect nedir, active directory yükleme
services: active-directory
author: billmath
manager: daveba
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.topic: get-started-article
ms.date: 11/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 0b6bb3f950c4a442e898a6251a89246b31af78bb
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55160432"
---
# <a name="what-is-hybrid-identity"></a>Hibrit kimlik nedir? 

Bugün, şirketler ve kuruluşlar bir çok daha fazla şirket içi bir karışımını olma olan ve bulut uygulamalarına.  Kullanıcılar bu uygulamaları hem şirket içi erişim gerektirir ve bulut. Bu gereksinim, zorlu bir senaryo haline gelmiştir. 

Microsoft'un kimlik çözümleri, şirket içi ve bulut tabanlı özellikleri kapsar.  Kimlik doğrulaması ve yetkilendirme konumdan bağımsız olarak tüm kaynaklara genel bir kullanıcı kimliği bu çözümler oluşturun. Bu diyoruz **karma kimlik**.

Karma kimlik elde etmek için üç kimlik doğrulama yöntemlerinden biri, senaryolarınıza bağlı olarak kullanılabilir.   Üç yöntem şunlardır: 

- **[Parola Karması eşitleme (PHS)](whatis-phs.md)**  
- **[Geçişli kimlik doğrulaması (PTA)](how-to-connect-pta.md)**  
- **[Federasyon](whatis-fed.md)** 

Bu kimlik doğrulama yöntemleri de sağlamak [çoklu oturum açma](how-to-connect-sso.md) özellikleri.  Çoklu oturum açma, kullanıcılarınızın şirket ağınıza bağlı cihazlarından Kurumsal olduklarında, otomatik olarak imzalar.

Ek bilgi için bkz: [, Azure Active Directory karma kimlik çözümü için doğru kimlik doğrulama yöntemini seçin](https://docs.microsoft.com/azure/security/azure-ad-choose-authn). 

## <a name="common-scenarios-and-recommendations"></a>Ortak senaryolar ve öneriler 

Aşağıda, bazı yaygın karma kimlik ve erişim yönetimi senaryoları, hangi karma kimlik seçeneğinin (veya seçeneklerinin) hangi senaryoya uygun olabileceği hakkında önerilerle birlikte verilmiştir. 

|Şunu istiyorum:|PHS ve SSO<sup>1</sup>| PTA ve SSO<sup>2</sup> | AD FS<sup>3</sup>| 
|-----|-----|-----|-----| 
|Şirket içi Active Directory ortamında oluşturulan yeni kullanıcı, kişi ve grup hesaplarının otomatik olarak bulutla eşitlenmesi.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Kiracımı Office 365 karma senaryoları için ayarlamak|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Kullanıcılarımın şirket içi parolalarıyla bulut hizmetlerinde oturum açması ve hizmetlere erişim sağlaması|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Kuruluş kimlik bilgileri ile çoklu oturum açmayı uygulamaya alma|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|  
|Parola karmalarının bulutta depolanmadığından emin olma| |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Bulutta çok faktörlü kimlik doğrulama çözümlerini etkinleştirme| |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Şirket içi ortamda çok faktörlü kimlik doğrulama çözümlerini etkinleştirme| | |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Kullanıcılarım için akıllı kart desteği sunma<sup>4</sup>| | |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Office Portalı'nda ve Windows 10 masaüstünde parola süre sonu bildirimleri görüntüleme| | |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 

> <sup>1</sup> Çoklu oturum açma ile parola karması eşitleme. 
> 
> <sup>2</sup> Doğrudan kimlik doğrulama ve çoklu oturum açma.  
> 
> <sup>3</sup> AD FS ile federasyon çoklu oturum açma.  
>  
> <sup>4</sup> AD FS, kurumsal PKI çözümünüzle tümleştirilerek sertifika ile oturum açma imkanı sunulabilir. Bu sertifikalar MDM veya GPO gibi güvenilen sağlama kanalları aracılığıyla dağıtılan yazılımsal sertifikalar, akıllı kart sertifikaları (PIV/CAC kartları dahil) veya İç için Hello (sertifika güveni) olabilir. Akıllı kart kimlik doğrulaması desteği hakkında daha fazla bilgi için [bu bloga](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/) bakın. 
> 

## <a name="next-steps"></a>Sonraki Adımlar 

- [Azure AD Connect ve Connect Health nedir?](whatis-azure-ad-connect.md) 
- [Parola Karması eşitleme (PHS) nedir?](whatis-phs.md) 
- [Geçişli kimlik doğrulaması (PTA) nedir?](how-to-connect-pta.md) 
- [Federasyon nedir?](whatis-fed.md) 
- [Üzerinde çoklu oturum nedir?](how-to-connect-sso.md) 

