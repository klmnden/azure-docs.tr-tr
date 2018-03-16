---
title: "Azure AD Connect: Sorunsuz çoklu oturum açma - hızlı başlangıç | Microsoft Docs"
description: "Bu makalede, Azure Active Directory sorunsuz çoklu oturum açma ile çalışmaya başlamak açıklar"
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
ms.date: 02/23/2017
ms.author: billmath
ms.openlocfilehash: 67f6ca36c334a60b634094f07e5d9696a6961eb8
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Azure Active Directory sorunsuz çoklu oturum açma: Hızlı Başlangıç

## <a name="deploy-seamless-single-sign-on"></a>Sorunsuz çoklu oturum açma dağıtma

Azure Active Directory (Azure AD) sorunsuz çoklu oturum açma (sorunsuz SSO) şirket ağınıza bağlı şirket masaüstlerine üzerinde olduğunda kullanıcıların otomatik olarak imzalar. Sorunsuz SSO herhangi bir şirket içinde ek bileşenini gerek kalmadan, kullanıcılarınızın bulut tabanlı uygulamalarınızı kolay erişim sağlar.

Sorunsuz SSO dağıtmak için aşağıdaki adımları izleyin.

## <a name="step-1-check-the-prerequisites"></a>1. adım: Önkoşul denetimi

Aşağıdaki önkoşulların yerine getirildiğinden emin olun:

* **Azure AD Connect sunucusu kurma**: kullanırsanız [doğrudan kimlik doğrulama](active-directory-aadconnect-pass-through-authentication.md) , oturum açma yöntemi, hiçbir ek Önkoşul denetimi gereklidir. Kullanırsanız [parola karması eşitlemesi](active-directory-aadconnectsync-implement-password-synchronization.md) , oturum açma yöntemi olarak ve Azure AD Connect ve Azure AD arasında bir güvenlik duvarı varsa, emin olun:
   - Sürüm 1.1.644.0 kullanın veya Azure AD Connect sonraki. 
   - Güvenlik Duvarı veya proxy bağlantıları için DNS uygulamaları, da beyaz liste güvenilir listeye almayı izin veriyorsa  **\*. msappproxy.net** URL'leri bağlantı noktası 443 üzerinden. Aksi durumda, erişim izni [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653), hangi haftalık güncelleştirildi. Yalnızca özelliğini etkinleştirdiğinizde bu geçerli bir önkoşuldur. Asıl kullanıcı oturum açma işlemleri için gerekli değildir.

    >[!NOTE]
    >Azure AD Connect sürümleri 1.1.557.0, 1.1.558.0, 1.1.561.0 ve 1.1.614.0 parola karması eşitlemesi için ilgili bir sorun var. Varsa, _yok_ parola karması eşitlemesi okuma doğrudan kimlik doğrulama ile birlikte kullanmayı düşündüğünüz [Azure AD Connect sürüm notları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470) daha fazla bilgi için.

* **Etki alanı yönetici kimlik bilgilerini ayarlayın**: her Active Directory orman için etki alanı yönetici kimlik bilgilerine sahip olması gerekir:
    * Azure AD Connect aracılığıyla Azure ad eşitleme.
    * Sorunsuz SSO için etkinleştirmek istediğiniz kullanıcıları içerir.

## <a name="step-2-enable-the-feature"></a>2. adım: özelliğini etkinleştir

Sorunsuz SSO aracılığıyla etkinleştirmek [Azure AD Connect](active-directory-aadconnect.md).

Azure AD Connect yeni bir yüklemesidir yaptığınız kaldığınızda [özel yükleme yolu](active-directory-aadconnect-get-started-custom.md). Konumundaki **kullanıcı oturum açma** sayfasında, **etkinleştirmek çoklu oturum açma** seçeneği.

![Azure AD Connect: Kullanıcı oturum açma](./media/active-directory-aadconnect-sso/sso8.png)

Azure AD Connect yüklemesi zaten varsa, seçin **değiştirme kullanıcı oturum açma** sayfa Azure AD Connect ve ardından **sonraki**.

![Azure AD Connect: kullanıcı oturum açma değiştirme](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

İçin ulaşana kadar Sihirbazı kullanarak devam **etkinleştirmek çoklu oturum açma** sayfası. Her Active Directory etki alanı yönetici kimlik bilgilerini orman sağlar:
    * Azure AD Connect aracılığıyla Azure ad eşitleme.
    * Sorunsuz SSO için etkinleştirmek istediğiniz kullanıcıları içerir.

Sihirbaz tamamlandıktan sonra Kiracı'sorunsuz SSO etkin.

>[!NOTE]
> Azure AD Connect veya Azure AD etki alanı yönetici kimlik bilgileri depolanmaz. Bunlar yalnızca özelliğini etkinleştirmek için kullanılır.

Sorunsuz SSO doğru etkinleştirdiğinizden emin doğrulamak için bu yönergeleri izleyin:

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) kiracınız için genel yönetici kimlik bilgilerine sahip.
2. Seçin **Azure Active Directory** sol bölmede.
3. Seçin **Azure AD Connect**.
4. Doğrulayın **sorunsuz çoklu oturum açma** özelliği görüntülenir olarak **etkin**.

![Azure portalında: Azure AD Connect bölmesi](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-the-feature"></a>3. adım: özelliği alma

Kullanıcılarınız için özelliği geri dönmek için Active Directory'de Grup İlkesi kullanarak kullanıcıların Intranet bölgesi ayarlarını aşağıdaki Azure AD URL eklemeniz gerekir:

- https://autologon.microsoftazuread-sso.com


Ayrıca, bir Intranet bölgesi İlkesi adlı ayarını etkinleştirmeniz gerekir **izin durum çubuğunda komut dosyası aracılığıyla güncelleştirmeleri** Grup İlkesi aracılığıyla. 

>[!NOTE]
> (Bu bir kümesini güvenilen site URL'lerin Internet Explorer ile paylaşır) aşağıdaki yönergeleri yalnızca Internet Explorer ve Google Chrome Windows üzerinde çalışır. Mozilla Firefox ve Mac üzerinde Google Chrome ayarlama hakkında yönergeler için sonraki bölüme okuma

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a>Neden kullanıcıların Intranet bölgesi ayarlarını değiştirmeniz gerekiyor mu?

Varsayılan olarak, tarayıcı doğru bölgesi, Internet veya Intranet, belirli bir URL'den otomatik olarak hesaplar. Örneğin, "http://contoso/" ise Intranet bölgesine eşlemeleri "http://intranet.contoso.com/" (URL nokta içerdiğinden) Internet bölgesine eşler. URL tarayıcının Intranet bölgesine açıkça eklemedikçe tarayıcılar Kerberos biletleri için Azure AD URL gibi bir bulut uç noktası göndermez.

### <a name="detailed-steps"></a>Ayrıntılı adımlar

1. Grup İlkesi Yönetimi Düzenleyicisi aracını açın.
2. Bazı uygulanan Grup İlkesi düzenleme veya tüm kullanıcılar. Bu örnekte **varsayılan etki alanı ilkesi**.
3. Gözat **Kullanıcı Yapılandırması** > **Yönetim Şablonları** > **Windows bileşenleri**  >   **Internet Explorer** > **Internet Denetim Masası** > **güvenlik sayfası**. Ardından **siteyi bölgeye ataması listesi**.
    ![Çoklu oturum açma](./media/active-directory-aadconnect-sso/sso6.png)
4. İlke etkinleştirin ve ardından iletişim kutusunda aşağıdaki değerleri girin:
   - **Değer adı**: Azure AD URL'si Kerberos biletleri yere iletilmez.
   - **Değer** (veri): **1** Intranet bölgesine gösterir.

    Sonuç şöyle görünür:

    Değer:https://autologon.microsoftazuread-sso.com
  
    Veriler: 1

   >[!NOTE]
   > Bazı kullanıcıların sorunsuz SSO kullanarak (örneğin, bu kullanıcılar paylaşılan bilgi noktaları üzerinde oturum açarsa) engellemek istiyorsanız, önceki değerlerini ayarlamak **4**. Bu eylem, Azure AD URL'si Yasak bölgeye ekler ve sorunsuz SSO her zaman başarısız olur.
   >

5. Seçin **Tamam**ve ardından **Tamam** yeniden.

    ![Çoklu oturum açma](./media/active-directory-aadconnect-sso/sso7.png)

6. Gözat **Kullanıcı Yapılandırması** > **Yönetim Şablonları** > **Windows bileşenleri**  >   **Internet Explorer** > **Internet Denetim Masası** > **güvenlik sayfası** > **Intranet bölgesine**. Ardından **izin durum çubuğunda komut dosyası aracılığıyla güncelleştirmeleri**.

    ![Çoklu oturum açma](./media/active-directory-aadconnect-sso/sso11.png)

7. İlke ayarını etkinleştirin ve ardından **Tamam**.

    ![Çoklu oturum açma](./media/active-directory-aadconnect-sso/sso12.png)

### <a name="browser-considerations"></a>Tarayıcı konuları

#### <a name="mozilla-firefox-all-platforms"></a>Mozilla Firefox (tüm platformlar)

Mozilla Firefox otomatik olarak Kerberos kimlik doğrulaması kullanmaz. Her kullanıcı el ile Firefox ayarlarına Azure AD URL aşağıdaki adımları kullanarak eklemeniz gerekir:
1. Firefox çalıştırın ve girin `about:config` adres çubuğundaki. Gördüğünüz herhangi bir bildirim yok sayın.
2. Arama **network.negotiate auth.trusted URI'ler** tercih. Bu tercih Kerberos kimlik doğrulaması için Firefox'in Güvenilen siteler listelenir.
3. Sağ tıklatıp **Değiştir**.
4. Girin https://autologon.microsoftazuread-sso.com alanında.
5. Seçin **Tamam** ve tarayıcıyı kapatıp yeniden açın.

#### <a name="safari-mac-os"></a>Safari (Mac OS)

Mac OS çalıştıran makine için AD alanına katılmış olmasını sağlayın. AD katılma ile ilgili yönergeler için bkz: [Active Directory ile tümleştirme OS X için en iyi uygulamaları](http://www.isaca.org/Groups/Professional-English/identity-management/GroupDocuments/Integrating-OS-X-with-Active-Directory.pdf).

#### <a name="google-chrome-all-platforms"></a>Google Chrome (tüm platformlar)

Kılınmadı varsa [AuthNegotiateDelegateWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthNegotiateDelegateWhitelist) veya [AuthServerWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthServerWhitelist) ilke ayarları, ortamınızdaki olun Azure AD URL'sini ekleyin (https://autologon.microsoftazuread-sso.com) bunlara de.

#### <a name="google-chrome-mac-os-only"></a>Google Chrome (yalnızca Mac OS)

Mac OS ve diğer Windows olmayan platformlarda Google Chrome için başvurmak [Chromium proje ilke listesi](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) nasıl beyaz liste ile kimlik doğrulaması için Azure AD URL tümleşik hakkında bilgi için.

Azure AD URL Firefox ve Google Chrome Mac Kullanıcıları geri dönmek için üçüncü taraf Active Directory Grup İlkesi uzantıları bu makalenin kapsamı dışındadır kullanılır.

#### <a name="known-browser-limitations"></a>Bilinen tarayıcı sınırlamaları

Sorunsuz SSO Firefox ve kenar tarayıcılarda özel gözatma modunda çalışmıyor. Tarayıcı geliştirilmiş korumalı modda çalışıyorsa, ayrıca Internet Explorer, işe yaramaz.

## <a name="step-4-test-the-feature"></a>4. adım: Test özelliği

Belirli bir kullanıcı için özelliğini sınamak için aşağıdaki tüm koşulların karşılandığından emin olun:
  - Kurumsal bir cihazda oturum açtığında.
  - Cihaz, Active Directory etki alanına katılır.
  - Cihazın şirket kablolu veya kablosuz ağı veya bir VPN bağlantısı gibi bir uzaktan erişim bağlantısı aracılığıyla doğrudan bağlantı oluşturmak için etki alanı denetleyicisi (DC) gerekir.
  - Sahip olduğunuz [özelliği alındı](##step-3-roll-out-the-feature) bu kullanıcıya Grup İlkesi aracılığıyla.

Burada yalnızca kullanıcı adı ve parola kullanıcının girdiği senaryoyu test etmek için:
   - Oturum https://myapps.microsoft.com/ yeni bir özel tarayıcı oturumunda.

Burada kullanıcının kullanıcı adı veya parola girmesi gerekmez senaryoyu test etmek için aşağıdaki adımlardan birini kullanın: 
   - Oturum https://myapps.microsoft.com/contoso.onmicrosoft.com yeni bir özel tarayıcı oturumunda. Değiştir *contoso* , kiracının ada sahip.
   - Oturum https://myapps.microsoft.com/contoso.com yeni bir özel tarayıcı oturumunda. Değiştir *contoso.com* kiracınız üzerinde doğrulanmış bir etki alanıyla (bir Federasyon etki alanı değil).

## <a name="step-5-roll-over-keys"></a>5. adım: anahtarlar üzerinden alma

2. adımda, Azure AD Connect (Azure AD temsil eder) bilgisayar hesapları sorunsuz SSO etkin tüm Active Directory ormanlarında oluşturur. Daha fazla bilgi için bkz: [Azure Active Directory sorunsuz çoklu oturum açma: teknik derinlemesine](active-directory-aadconnect-sso-how-it-works.md). Gelişmiş güvenlik için Kerberos düzenli aralıklarla bu bilgisayar hesaplarının şifre çözme anahtarlarını alma öneririz. Anahtarlar üzerinden alma hakkında daha fazla yönerge için bkz: [Azure Active Directory sorunsuz çoklu oturum açma: sık sorulan sorular](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account).

>[!IMPORTANT]
>Bu adım gerekmez _hemen_ özelliği etkinleştirdikten sonra. Kerberos şifre çözme anahtarları 30 günde en az bir kez alma.

## <a name="next-steps"></a>Sonraki adımlar

- [Teknik derinlemesine](active-directory-aadconnect-sso-how-it-works.md): sorunsuz çoklu oturum açma özelliği nasıl çalıştığını anlayın.
- [Sık sorulan sorular](active-directory-aadconnect-sso-faq.md): Get hakkında sorunsuz çoklu oturum açma sık sorulan soruları yanıtlar.
- [Sorun giderme](active-directory-aadconnect-troubleshoot-sso.md): sorunsuz çoklu oturum açma özelliği ile ortak sorunları çözmeyi öğrenin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): yeni özellik istekleri dosya için Azure Active Directory Forumu kullanın.
