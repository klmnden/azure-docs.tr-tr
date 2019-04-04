---
title: 'Azure AD Connect: Sorunsuz çoklu oturum açma - nasıl çalışır? | Microsoft Docs'
description: Bu makalede, Azure Active Directory sorunsuz çoklu oturum açma özelliğinin nasıl çalıştığı açıklanır.
services: active-directory
keywords: Azure AD, SSO, gerekli bileşenleri yükleme Active Directory, Azure AD Connect nedir çoklu oturum açma
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 813ab2a349ba843e9f41675234e395470bef9740
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896134"
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Azure Active Directory sorunsuz çoklu oturum açma: Ayrıntılı Teknik İnceleme

Bu makalede, Azure Active Directory sorunsuz çoklu oturum açma (sorunsuz SSO) özelliğinin nasıl çalıştığı ile teknik ayrıntılar sunar.

## <a name="how-does-seamless-sso-work"></a>Sorunsuz çoklu oturum açma nasıl çalışır?

Bu bölüm, üç bölümü vardır:

1. Sorunsuz çoklu oturum açma özelliği kurulumu.
2. Nasıl bir tek kullanıcı işlemi bir web tarayıcısında oturum açma sorunsuz çoklu oturum açma ile çalışır.
3. Nasıl sorunsuz SSO ile oturum açma tek bir kullanıcı işlemi yerel bir istemci üzerinde çalışır.

### <a name="how-does-set-up-work"></a>Nasıl iş ayarlayamayan dilbilimsel?

Sorunsuz çoklu oturum açma etkin gösterildiği gibi Azure AD Connect kullanarak [burada](how-to-connect-sso-quick-start.md). Bu özellik etkinleştirilirken, aşağıdaki adımlar oluşur:

- Bir bilgisayar hesabı (`AZUREADSSOACC`) şirket içi (Azure AD Connect kullanarak) Azure AD ile eşitlemek için her AD ormanında Active Directory (AD) oluşturulur.
- Ayrıca, bir Kerberos hizmet asıl adı (SPN) sayısı, Azure AD oturum açma işlemi sırasında kullanılacak oluşturulur.
- Bilgisayar hesabının Kerberos şifre çözme anahtarı güvenli bir şekilde Azure AD ile paylaşılır. Birden fazla AD ormanına varsa, her bir bilgisayar hesabının kendi benzersiz Kerberos şifre çözme anahtarı gerekir.

>[!IMPORTANT]
> `AZUREADSSOACC` Bilgisayar hesabını güvenlik nedenleriyle kesin korunması gerekir. Yalnızca Domain Admins bilgisayar hesabını yönetmek görebilmeniz gerekir. Bilgisayar hesabının Kerberos temsilcisi seçmeyi devre dışı emin olun. Bilgisayar hesabı, bir kuruluş birimi (yanlışlıkla silinmekten güvenli oldukları OU) Store. Bilgisayar hesabının Kerberos şifre çözme anahtarı da hassas olarak düşünülmelidir. Yüksek oranda olmasını öneririz, [Kerberos şifre çözme anahtarını başa döndürmek](how-to-connect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account) , `AZUREADSSOACC` en az 30 günde bir bilgisayar hesabı.

Kurulum tamamlandıktan sonra sorunsuz çoklu oturum açma herhangi diğer tümleşik Windows kimlik doğrulaması (IWA) kullanan oturum aynı şekilde çalışır.

### <a name="how-does-sign-in-on-a-web-browser-with-seamless-sso-work"></a>Nasıl iş sorunsuz SSO ile bir web tarayıcısında oturum?

Bir web tarayıcısında oturum açma akışı şu şekildedir:

1. Kullanıcı bir web uygulamasına erişmeye çalışır (örneğin, Outlook Web App - https://outlook.office365.com/owa/) bir etki alanına katılmış Kurumsal CİHAZDAN Kurumsal ağınızdaki.
2. Kullanıcı zaten oturum açmamış, kullanıcının Azure AD oturum açma sayfasına yönlendirilir.
3. Kullanıcı türleri kendi Azure AD oturum açma sayfasında kullanıcı adı.

   >[!NOTE]
   >İçin [belirli uygulamaları](./how-to-connect-sso-faq.md#what-applications-take-advantage-of-domain_hint-or-login_hint-parameter-capability-of-seamless-sso), adım 2 ve 3 atlanır.

4. JavaScript arka planda kullanarak, Azure AD aracılığıyla bir Kerberos anahtarı sağlamak için 401 Yetkisiz yanıt, tarayıcının sınar.
5. Tarayıcı için Active Directory'den bir bilet sırayla ister `AZUREADSSOACC` (Azure AD temsil eden) bilgisayar hesabı.
6. Active Directory bilgisayar hesabına bulur ve bir Kerberos anahtarı bilgisayar hesabının parolası ile şifrelenmiş tarayıcıya döndürür.
7. Tarayıcı, alınan Kerberos anahtarı, Active Directory'den Azure AD'ye iletir.
8. Azure AD, önceden paylaşılan anahtar kullanan kurumsal cihazda oturum açan kullanıcının kimliğini içeren Kerberos anahtarı şifresini çözer.
9. Değerlendirme sonra Azure AD belirteç uygulamaya geri döndürür veya çok faktörlü kimlik doğrulaması gibi ek kanıtları gerçekleştirmek için kullanıcıya sorar.
10. Kullanıcı oturum açma başarılı olursa, uygulamaya erişmeye çalıştığında bir kullanıcıdır.

Aşağıdaki diyagram, tüm bileşenleri ve ilgili adımları gösterir.

![Sorunsuz çoklu oturum açma uygulama akışı On - Web](./media/how-to-connect-sso-how-it-works/sso2.png)

Sorunsuz çoklu oturum açma başarısız olursa, oturum açma deneyimini normal davranışını - yani, kullanıcının oturum açmak için parola girmeniz gerekiyorsa geri döner. fırsatçı, yani.

### <a name="how-does-sign-in-on-a-native-client-with-seamless-sso-work"></a>Nasıl sorunsuz çoklu oturum açma çalışma ile yerel bir istemcide oturum?

Yerel bir istemci oturum açma akışı aşağıdaki gibidir:

1. Kullanıcı bir etki alanına katılmış Kurumsal CİHAZDAN Kurumsal ağınızdaki (örneğin, Outlook istemcisi) yerel bir uygulamaya erişmeye çalışır.
2. Kullanıcı zaten oturum açmamış, yerel uygulama cihazın Windows oturumu kullanıcı adını alır.
3. Uygulama, kullanıcı adını Azure AD'ye gönderir ve kiracınızın MEX WS-Trust uç noktasını alır. Bu WS-Trust uç noktası yalnızca sorunsuz çoklu oturum açma özelliği tarafından kullanılan ve WS-Trust Protokolü Azure AD'de genel bir uygulama değildir.
4. Uygulama daha sonra WS-Trust MEX uç nokta kimlik doğrulama uç noktası kullanılabilir Tümleşik olmadığını görmek için sorgular. Tümleşik kimlik doğrulaması uç noktası yalnızca sorunsuz çoklu oturum açma özelliği tarafından kullanılır.
5. Adım 4 başarılı olursa, bir Kerberos challenge verilir.
6. Uygulama Kerberos anahtarını almak mümkün ise, bunu Azure AD'nin tümleşik kimlik doğrulama uç noktası kadar iletir.
7. Azure AD, Kerberos anahtarı şifresini çözer ve bunu doğrular.
8. Azure AD kullanıcının oturum açtığı ve uygulamaya bir SAML belirteci verir.
9. Uygulama, Azure AD'nin OAuth2 belirteç uç noktası için SAML belirtecinde ardından gönderir.
10. Azure AD, SAML belirteci doğrular ve uygulamaya bir erişim belirteci ve yenileme belirteci için belirtilen kaynak ve bir kimlik belirteci verir.
11. Kullanıcı, uygulamanın kaynağına erişimi alır.

Aşağıdaki diyagram, tüm bileşenleri ve ilgili adımları gösterir.

![Sorunsuz tek oturum açma - yerel uygulama akışı](./media/how-to-connect-sso-how-it-works/sso14.png)

## <a name="next-steps"></a>Sonraki adımlar

- [**Hızlı Başlangıç** ](how-to-connect-sso-quick-start.md) - getirmek ve Azure AD sorunsuz çoklu oturum açma çalışıyor.
- [**Sık sorulan sorular** ](how-to-connect-sso-faq.md) -sık sorulan soruların yanıtları.
- [**Sorun giderme** ](tshoot-connect-sso.md) -özelliği ile ilgili yaygın sorunları çözmeyi öğrenin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleriniz dosyalama için.
