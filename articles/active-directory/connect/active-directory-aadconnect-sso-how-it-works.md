---
title: 'Azure AD Connect: Sorunsuz çoklu oturum açma - nasıl çalışır? | Microsoft Docs'
description: Bu makalede, Azure Active Directory sorunsuz çoklu oturum açma özelliğinin nasıl çalıştığı açıklanmaktadır.
services: active-directory
keywords: Azure AD, SSO, gerekli bileşenleri yükleme Active Directory, Azure AD Connect nedir çoklu oturum açma
documentationcenter: ''
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: bcd9ec44eafd586648ba964c5cba248a184a8ec3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34591570"
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Azure Active Directory sorunsuz çoklu oturum açma: teknik derinlemesine bakış

Bu makalede, Azure Active Directory sorunsuz çoklu oturum açma (sorunsuz SSO) özelliğinin nasıl çalıştığı içine teknik ayrıntılar sağlar.

## <a name="how-does-seamless-sso-work"></a>Sorunsuz SSO nasıl çalışır?

Bu bölümde ona üç bölümden oluşur:
1. Sorunsuz SSO özelliği kurulumu.
2. Nasıl bir web tarayıcısı bir tek bir kullanıcı oturum açma işleme ile sorunsuz SSO çalışır.
3. Nasıl bir tek kullanıcı işlemi yerel bir istemcide oturum açma ile sorunsuz SSO çalışır.

### <a name="how-does-set-up-work"></a>Nasıl iş ayarlamak?

Sorunsuz SSO etkin gösterildiği gibi Azure AD Connect'i kullanarak [burada](active-directory-aadconnect-sso-quick-start.md). Özellik etkinleştirirken, aşağıdaki adımlardan oluşur:
- Adlı bir bilgisayar hesabı `AZUREADSSOACC` (Azure AD temsil eden), şirket içi Active Directory (AD) oluşturulur.
- Bilgisayar hesabının Kerberos şifre çözme anahtarı güvenli bir şekilde Azure AD ile paylaşılır.
- Ayrıca, iki Kerberos hizmet asıl adları (SPN), Azure AD oturum açma sırasında kullanılan iki URL'ler temsil etmek üzere oluşturulur.

>[!NOTE]
> Bilgisayar hesabını ve Kerberos SPN'lerinin (Azure AD Connect'i kullanarak) Azure AD ile eşitleyebilir ve olan kullanıcılar için sorunsuz SSO istediğiniz her AD ormanında oluşturulur. Taşıma `AZUREADSSOACC` bilgisayar hesabı için bir kuruluş birimi (aynı şekilde yönetilir ve silinmez emin olmak için diğer bilgisayar hesapları depolandığı OU).

>[!IMPORTANT]
>Yüksek oranda öneririz, [Kerberos şifre çözme anahtarı alma](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account) , `AZUREADSSOACC` bilgisayar hesabı en az 30 günde.

Kurulum tamamlandıktan sonra sorunsuz SSO herhangi diğer oturum tümleşik Windows kimlik doğrulaması (IWA) kullanan açma aynı şekilde çalışır.

### <a name="how-does-sign-in-on-a-web-browser-with-seamless-sso-work"></a>Nasıl oturum sorunsuz SSO iş web tarayıcısında açma?

Web tarayıcısında oturum açma akışı aşağıdaki gibidir:

1. Kullanıcı bir web uygulamasına erişmeyi denediğinde (örneğin, Outlook Web App - https://outlook.office365.com/owa/) bir etki alanına katılmış kurumsal bir CİHAZDAN Kurumsal ağınızdaki.
2. Kullanıcı zaten oturum açmamış, kullanıcının Azure AD oturum açma sayfasına yönlendirilir.
3. Kullanıcı adı oturum açma Azure AD sayfasına kullanıcı türler.

  >[!NOTE]
  >İçin [belirli uygulamaları](./active-directory-aadconnect-sso-faq.md#what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso), 2 ve 3. adımları atlanır.

4. JavaScript arka planda kullanarak, Azure AD tarayıcı bir Kerberos anahtarı sağlamak için bir 401 Yetkisiz yanıtı yoluyla sınar.
5. Tarayıcı için Active Directory'den bir bilet sırayla ister `AZUREADSSOACC` (Azure AD temsil eden) bilgisayar hesabı.
6. Active Directory bilgisayar hesabının bulur ve bilgisayar hesabının parolası ile şifrelenmiş tarayıcıya bir Kerberos anahtarı döndürür.
7. Tarayıcı, edinilen Kerberos bileti Azure AD ile Active Directory'den iletir.
8. Azure AD önceden paylaşılan anahtar kullanan kurumsal cihaz imzalı kullanıcının kimliğini içeren Kerberos bileti şifresini çözer.
9. Değerlendirme sonra Azure AD uygulama için bir belirteç döndüren ya da çok faktörlü kimlik doğrulaması gibi ek sağlamaları gerçekleştirmek için kullanıcıya sorar.
10. Kullanıcı oturum açma başarılı olursa, kullanıcı uygulamaya erişim mümkün değil.

Aşağıdaki diyagram, tüm bileşenler ve adımlarını gösterir.

![Sorunsuz çoklu oturum açma On - Web uygulama akışı](./media/active-directory-aadconnect-sso/sso2.png)

Sorunsuz SSO fırsatçılıktan, başarısız olursa, oturum açma deneyimini normal davranışını - yani, kullanıcı oturum açmak için parolalarını girmeleri gerekir geri döner. anlamına gelir.

### <a name="how-does-sign-in-on-a-native-client-with-seamless-sso-work"></a>Nasıl oturum sorunsuz SSO iş yerel bir istemcide açma?

Yerel bir istemcide oturum açma akışı aşağıdaki gibidir:

1. Kullanıcı bir etki alanına katılmış kurumsal bir CİHAZDAN Kurumsal ağınızdaki (örneğin, Outlook istemcisi) yerel bir uygulamaya erişmeye çalışır.
2. Kullanıcı zaten oturum açmışsa değil, yerel uygulama cihazın Windows oturumu kullanıcının kullanıcı adı alır.
3. Uygulama için Azure AD kullanıcı adı gönderir ve kiracınıza ait WS-Trust MEX bitiş noktası alır.
4. Uygulama tümleşik olup, kimlik doğrulama uç noktası kullanılabilir WS-Trust MEX bitiş noktası ardından sorgular.
5. 4. adım başarılı olursa, bir Kerberos challenge verilir.
6. Uygulama Kerberos anahtarını almak mümkün ise, bu kadar Azure AD ile tümleşik kimlik doğrulaması endpoint iletir.
7. Azure AD Kerberos biletini şifresini çözer ve bunu doğrular.
8. Azure AD kullanıcı oturum açtığında ve uygulamaya bir SAML belirteci verir.
9. Uygulama, ardından Azure AD OAuth2 belirteç uç noktası için SAML belirteci gönderir.
10. Azure AD SAML belirteci doğrular ve bir erişim belirteci ve belirtilen kaynak için bir yenileme belirteci ve bir kimliği belirteci uygulamaya sorunları.
11. Kullanıcı uygulamanın kaynağa erişim alır.

Aşağıdaki diyagram, tüm bileşenler ve adımlarını gösterir.

![Sorunsuz tek oturum açma - yerel uygulama akışı](./media/active-directory-aadconnect-sso/sso14.png)

## <a name="next-steps"></a>Sonraki adımlar

- [**Hızlı Başlangıç** ](active-directory-aadconnect-sso-quick-start.md) - hale getirmek ve Azure AD sorunsuz SSO çalışıyor.
- [**Sık sorulan sorular** ](active-directory-aadconnect-sso-faq.md) -sık sorulan sorulara yanıtlar.
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-sso.md) -özelliği ile ilgili genel sorunları çözmeyi öğrenin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleri dosyalama için.
