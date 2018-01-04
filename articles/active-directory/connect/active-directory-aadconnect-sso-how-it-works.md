---
title: "Azure AD Connect: Sorunsuz çoklu oturum açma - nasıl çalışır? | Microsoft Docs"
description: "Bu makalede, Azure Active Directory sorunsuz çoklu oturum açma özelliğinin nasıl çalıştığı açıklanmaktadır."
services: active-directory
keywords: "Azure AD, SSO, gerekli bileşenleri yükleme Active Directory, Azure AD Connect nedir çoklu oturum açma"
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: 0a28cd9016588d266670aa5a7fcbdd854d7ebce0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Azure Active Directory sorunsuz çoklu oturum açma: teknik derinlemesine bakış

Bu makalede, Azure Active Directory sorunsuz çoklu oturum açma (sorunsuz SSO) özelliğinin nasıl çalıştığı içine teknik ayrıntılar sağlar.

## <a name="how-does-seamless-sso-work"></a>Sorunsuz SSO nasıl çalışır?

Bu bölüm iki bölümü vardır:
1. Sorunsuz SSO özelliği kurulumu.
2. Nasıl bir tek kullanıcı oturum açma işlemi ile sorunsuz SSO çalışır.

### <a name="how-does-set-up-work"></a>Nasıl iş ayarlamak?

Sorunsuz SSO etkin gösterildiği gibi Azure AD Connect'i kullanarak [burada](active-directory-aadconnect-sso-quick-start.md). Özellik etkinleştirirken, aşağıdaki adımlardan oluşur:
- Adlı bir bilgisayar hesabı `AZUREADSSOACC` (Azure AD temsil eden), şirket içi Active Directory (AD) oluşturulur.
- Bilgisayar hesabının Kerberos şifre çözme anahtarı güvenli bir şekilde Azure AD ile paylaşılır.
- Ayrıca, iki Kerberos hizmet asıl adları (SPN), Azure AD oturum açma sırasında kullanılan iki URL'ler temsil etmek üzere oluşturulur.

>[!NOTE]
> Bilgisayar hesabını ve Kerberos SPN'lerinin (Azure AD Connect'i kullanarak) Azure AD ile eşitleyebilir ve olan kullanıcılar için sorunsuz SSO istediğiniz her AD ormanında oluşturulur. Taşıma `AZUREADSSOACC` bilgisayar hesabı için bir kuruluş birimi (aynı şekilde yönetilir ve silinmez emin olmak için diğer bilgisayar hesapları depolandığı OU).

>[!IMPORTANT]
>Yüksek oranda öneririz, [Kerberos şifre çözme anahtarı alma](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account) , `AZUREADSSOACC` bilgisayar hesabı en az 30 günde.

### <a name="how-does-sign-in-with-seamless-sso-work"></a>Nasıl oturum sorunsuz SSO çalışmak açma?

Kurulum tamamlandıktan sonra sorunsuz SSO herhangi diğer oturum tümleşik Windows kimlik doğrulaması (IWA) kullanan açma aynı şekilde çalışır. Akışı aşağıdaki gibidir:

1. Kullanıcı bir uygulamaya erişmeye çalıştığında (örneğin, Outlook Web App - https://outlook.office365.com/owa/) bir etki alanına katılmış kurumsal bir CİHAZDAN Kurumsal ağınızdaki.
2. Kullanıcı zaten oturum açmamış, kullanıcının Azure AD oturum açma sayfasına yönlendirilir.

  >[!NOTE]
  >Azure AD oturum açma isteği içeriyorsa, bir `domain_hint` (örneğin, Kiracı - tanımlama, contoso.onmicrosoft.com) veya `login_hint` (kullanıcı - Örneğin, tanımlayan user@contoso.onmicrosoft.com veya user@contoso.com) parametre sonra 2. adım atlanır.

3. Kullanıcı adı oturum açma Azure AD sayfasına kullanıcı türler.
4. JavaScript arka planda kullanarak, Azure AD tarayıcı bir Kerberos anahtarı sağlamak için bir 401 Yetkisiz yanıtı yoluyla sınar.
5. Tarayıcı için Active Directory'den bir bilet sırayla ister `AZUREADSSOACC` (Azure AD temsil eden) bilgisayar hesabı.
6. Active Directory bilgisayar hesabının bulur ve bilgisayar hesabının parolası ile şifrelenmiş tarayıcıya bir Kerberos anahtarı döndürür.
7. Tarayıcı, edinilen Kerberos bileti Active Directory'den Azure AD'ye iletir (birinde [tarayıcının Intranet bölgesi ayarlarını daha önce eklenen Azure AD URL'leri](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).
8. Azure AD önceden paylaşılan anahtar kullanan kurumsal cihaz imzalı kullanıcının kimliğini içeren Kerberos bileti şifresini çözer.
9. Değerlendirme sonra Azure AD uygulama için bir belirteç döndüren ya da çok faktörlü kimlik doğrulaması gibi ek sağlamaları gerçekleştirmek için kullanıcıya sorar.
10. Kullanıcı oturum açma başarılı olursa, kullanıcı uygulamaya erişim mümkün değil.

Aşağıdaki diyagram, tüm bileşenler ve adımlarını gösterir.

![Sorunsuz çoklu oturum açma](./media/active-directory-aadconnect-sso/sso2.png)

Sorunsuz SSO fırsatçılıktan, başarısız olursa, oturum açma deneyimini normal davranışını - yani, kullanıcı oturum açmak için parolalarını girmeleri gerekir geri döner. anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

- [**Hızlı Başlangıç** ](active-directory-aadconnect-sso-quick-start.md) - hale getirmek ve Azure AD sorunsuz SSO çalışıyor.
- [**Sık sorulan sorular** ](active-directory-aadconnect-sso-faq.md) -sık sorulan sorulara yanıtlar.
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-sso.md) -özelliği ile ilgili genel sorunları çözmeyi öğrenin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleri dosyalama için.
