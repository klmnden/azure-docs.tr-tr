---
title: 'Azure AD Connect: Sorunsuz çoklu oturum açma - hızlı başlangıç | Microsoft Docs'
description: Bu makalede Azure Active Directory sorunsuz çoklu oturum açma ile çalışmaya başlama işlemini açıklamaktadır.
services: active-directory
keywords: Azure AD, SSO, gerekli bileşenleri yükleme Active Directory, Azure AD Connect nedir çoklu oturum açma
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: f8639cbb5c7ba86b4786f3d0b913d64bad59ad66
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917525"
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Azure Active Directory sorunsuz çoklu oturum açma: Hızlı Başlangıç

## <a name="deploy-seamless-single-sign-on"></a>Sorunsuz çoklu oturum açma dağıtma

Azure Active Directory (Azure AD) sorunsuz çoklu oturum açma (sorunsuz çoklu oturum açma) şirket ağınıza bağlı olan kurumsal masaüstü üzerinde olduğunda kullanıcıların otomatik olarak imzalar. Sorunsuz çoklu oturum açma, ek şirket içi bileşenleri gerek kalmadan kullanıcılarınız ile bulut tabanlı uygulamalarınızı kolayca erişmenizi sağlar.

Sorunsuz çoklu oturum açma dağıtmak için aşağıdaki adımları izleyin.

## <a name="step-1-check-the-prerequisites"></a>1. adım: önkoşulları denetleme

Aşağıdaki önkoşulların karşılandığından emin olun:

* **Azure AD Connect sunucunuzu kurarken**: kullanırsanız [geçişli kimlik doğrulaması](active-directory-aadconnect-pass-through-authentication.md) hiçbir ek bir önkoşul denetimi, oturum açma yöntemi gereklidir. Kullanırsanız [parola karması eşitleme](active-directory-aadconnectsync-implement-password-hash-synchronization.md) , oturum açma yöntemi olarak Azure AD Connect ve Azure AD arasında bir güvenlik duvarı varsa, emin olun:
   - Sürüm 1.1.644.0 kullanın veya Azure AD Connect sonraki bir sürümü. 
   - Güvenlik Duvarı veya proxy bağlantıları için DNS beyaz listeye ekleme, beyaz liste izin veriyorsa  **\*. msappproxy.net** URL'leri bağlantı noktası 443 üzerinden. Aksi takdirde, erişim izni [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653), hangi haftalık güncelleştirildi. Yalnızca özelliğini etkinleştirdiğinizde bu önkoşul geçerlidir. Gerçek kullanıcı oturum açma işlemleri için gerekli değildir.

    >[!NOTE]
    >Azure AD Connect sürüm 1.1.557.0, 1.1.558.0 1.1.561.0 ve 1.1.614.0 parola karması eşitleme için ilgili bir sorun var. Varsa, _yoksa_ okuma geçişli kimlik doğrulaması ile birlikte parola karması eşitleme kullanmayı düşündüğünüz [Azure AD Connect sürüm notları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470) daha fazla bilgi için.

* **Etki alanı yönetici kimlik bilgilerini ayarla**: her Active Directory orman için etki alanı yönetici kimlik bilgilerine sahip olmanız gerekir:
    * Azure AD Connect aracılığıyla Azure AD'ye eşitleyin.
    * Sorunsuz çoklu oturum açma için etkinleştirmek istediğiniz kullanıcıları içerir.

## <a name="step-2-enable-the-feature"></a>2. adım: özellik etkinleştirme

Sorunsuz çoklu oturum açma aracılığıyla etkinleştirme [Azure AD Connect](active-directory-aadconnect.md).

Yeni Azure AD Connect yüklemesini yapıyorsanız seçin [özel bir yükleme yolu](active-directory-aadconnect-get-started-custom.md). Konumunda **kullanıcı oturum açma** sayfasında **etkinleştirme çoklu oturum açma** seçeneği.

![Azure AD Connect: Kullanıcı oturum açma](./media/active-directory-aadconnect-sso/sso8.png)

Azure AD Connect yüklemesi zaten varsa, seçin **değiştirme kullanıcı oturum açma** sayfa Azure AD Connect ve ardından **sonraki**.

![Azure AD Connect: kullanıcı oturum açma değiştirme](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Sihirbazda gelene kadar devam **etkinleştirme çoklu oturum açma** sayfası. Her Active Directory etki alanı yönetici kimlik bilgileri, orman sağlar:
    * Azure AD Connect aracılığıyla Azure AD'ye eşitleyin.
    * Sorunsuz çoklu oturum açma için etkinleştirmek istediğiniz kullanıcıları içerir.

Sihirbaz tamamlandıktan sonra kiracınızda sorunsuz çoklu oturum açma etkin.

>[!NOTE]
> Etki alanı yönetici kimlik bilgileri, Azure AD CONNECT'te veya Azure AD'de depolanmaz. Bunlar yalnızca özelliği etkinleştirmek için kullanılır.

Sorunsuz çoklu oturum açma doğru etkinleştirdiğinizden emin doğrulamak için aşağıdaki yönergeleri izleyin:

1. Oturum [Azure Active Directory Yönetim Merkezi'ni](https://aad.portal.azure.com) kiracınız için genel yönetici kimlik bilgileriyle.
2. Seçin **Azure Active Directory** sol bölmesinde.
3. Seçin **Azure AD Connect**.
4. Doğrulayın **sorunsuz çoklu oturum açma** özellik olarak görünür **etkin**.

![Azure portalı: Azure AD Connect bölmesi](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-the-feature"></a>3. adım: özelliği kullanıma alma

Kullanıcılarınıza özelliği kullanıma alma için Active Directory'de Grup İlkesi'ni kullanarak kullanıcıların Intranet bölgesi ayarlarını Azure AD aşağıdaki URL'yi eklemeniz gerekir:

- https://autologon.microsoftazuread-sso.com


Ayrıca, bir Intranet bölgesi İlkesi adlı ayarını etkinleştirmeniz gerekir **izin vermek için durum çubuğu komut dosyası aracılığıyla güncelleştirmeleri** Grup İlkesi aracılığıyla. 

>[!NOTE]
> (Bir dizi güvenilen site URL'leri Internet Explorer ile paylaştığı) aşağıdaki yönergeler yalnızca Internet Explorer ve Google Chrome'un Windows üzerinde çalışır. Mozilla Firefox ve Google Chrome Mac'te ayarlama hakkında yönergeler için sonraki bölüme okuyun

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a>Kullanıcıların Intranet bölge ayarlarını değiştirmek neden ihtiyacınız var?

Varsayılan olarak, tarayıcı, doğru bölgeyi, Internet veya Intranet, belirli bir URL'den otomatik olarak hesaplar. Örneğin, "http://contoso/"eşler Intranet bölgesine ise"http://intranet.contoso.com/" (URL bir dönemi içerdiğinden) Internet bölgesine eşler. Tarayıcının Intranet bölgesine açıkça URL eklemediğiniz sürece tarayıcılar Kerberos biletleri Azure AD URL'yi gibi bir bulut uç noktasına gönderir.

### <a name="detailed-steps"></a>Ayrıntılı adımlar

1. Grup İlkesi Yönetimi Düzenleyicisi Aracı'nı açın.
2. Bazı uygulanan Grup ilkesini düzenleyin veya tüm kullanıcılar. Bu örnekte **varsayılan etki alanı ilkesi**.
3. Gözat **Kullanıcı Yapılandırması** > **Yönetim Şablonları** > **Windows bileşenleri**  >   **Internet Explorer** > **Internet Denetim Masası** > **güvenlik sayfası**. Ardından **siteyi bölgeye ataması Listesi'ni**.
    ![Çoklu oturum açma](./media/active-directory-aadconnect-sso/sso6.png)
4. İlkeyi etkinleştirin ve sonra da iletişim kutusunda aşağıdaki değerleri girin:
   - **Değer adı**: Azure AD URL'si nerede Kerberos biletleri iletilir.
   - **Değer** (veriler): **1** Intranet gösterir.

    Sonucu şöyle görünür:

    Değer:https://autologon.microsoftazuread-sso.com
  
    Veriler: 1

   >[!NOTE]
   > Bazı kullanıcıların (örneğin, bu kullanıcılar üzerinde paylaşılan bilgi noktaları oturum açarsanız) kullanarak sorunsuz çoklu oturum açmayı engellemek istiyorsanız, önceki değerlerini ayarlamak **4**. Bu eylem, Azure AD URL'si için kısıtlı bölge ekler ve her zaman sorunsuz çoklu oturum açma başarısız.
   >

5. Seçin **Tamam**ve ardından **Tamam** yeniden.

    ![Çoklu oturum açma](./media/active-directory-aadconnect-sso/sso7.png)

6. Gözat **Kullanıcı Yapılandırması** > **Yönetim Şablonları** > **Windows bileşenleri**  >   **Internet Explorer** > **Internet Denetim Masası** > **güvenlik sayfası** > **Intranet bölgesine**. Ardından **izin vermek için durum çubuğu komut dosyası aracılığıyla güncelleştirmeleri**.

    ![Çoklu oturum açma](./media/active-directory-aadconnect-sso/sso11.png)

7. İlke ayarını etkinleştirin ve ardından **Tamam**.

    ![Çoklu oturum açma](./media/active-directory-aadconnect-sso/sso12.png)

### <a name="browser-considerations"></a>Tarayıcı konuları

#### <a name="mozilla-firefox-all-platforms"></a>Mozilla Firefox (tüm platformlar)

Mozilla Firefox, otomatik olarak Kerberos kimlik doğrulaması kullanmaz. Her kullanıcı el ile Firefox ayarlarına Azure AD URL'si aşağıdaki adımları kullanarak eklemeniz gerekir:
1. Firefox çalıştırın ve girin `about:config` adres çubuğundaki. Gördüğünüz herhangi bir bildirim kapatın.
2. Arama **network.negotiate auth.trusted URI'ler** tercihi. Bu tercih, Kerberos kimlik doğrulaması için Firefox'ın Güvenilen siteler listelenir.
3. Sağ tıklayıp **Değiştir**.
4. Girin https://autologon.microsoftazuread-sso.com alanında.
5. Seçin **Tamam** tarayıcıyı kapatıp açın.

#### <a name="safari-mac-os"></a>Safari (Mac OS)

Mac OS çalıştıran makinenin AD'ye katıldığından emin olun. AD katılma ile ilgili yönergeler için bkz: [Active Directory ile tümleştirme OS X için en iyi](http://www.isaca.org/Groups/Professional-English/identity-management/GroupDocuments/Integrating-OS-X-with-Active-Directory.pdf).

#### <a name="google-chrome-all-platforms"></a>Google Chrome (tüm platformlar)

Geçersiz kılınan varsa [AuthNegotiateDelegateWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthNegotiateDelegateWhitelist) veya [AuthServerWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthServerWhitelist) ilke ayarları, ortamınızda olun Azure AD'nin URL'si ekleyin (https://autologon.microsoftazuread-sso.com) onlara de.

#### <a name="google-chrome-mac-os-only"></a>Google Chrome (yalnızca Mac OS)

Mac OS ve diğer Windows olmayan platformlar için Google Chrome, başvurmak [Chromium proje Policy List](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) nasıl beyaz liste ile kimlik doğrulaması için Azure AD URL'sini tümleştirilmiş hakkında bilgi.

Bu makalenin kapsamı dışında Azure AD URL Google Chrome ve Firefox ile Mac kullanıcılarının içine almak için üçüncü taraf Active Directory Grup İlkesi uzantıları kullanılır.

#### <a name="known-browser-limitations"></a>Bilinen tarayıcı sınırlamaları

Sorunsuz çoklu oturum açma, Firefox ve Edge tarayıcılarda özel tarama modunda çalışmıyor. Tarayıcı Gelişmiş korumalı modda çalışıyorsa aynı zamanda Internet Explorer'da çalışmaz.

## <a name="step-4-test-the-feature"></a>4. adım: Test özelliği

Özellik belirli bir kullanıcı için test etmek için aşağıdaki tüm koşulların karşılandığından emin olun:
  - Kurumsal bir cihazda oturum açtığında kullanıcı.
  - Cihaz, Active Directory etki alanına katıldı.
  - Cihaz, Kurumsal kablolu veya kablosuz ağda veya bir VPN bağlantısı gibi bir uzaktan erişim bağlantısı aracılığıyla etki alanı denetleyicisine (DC) doğrudan bir bağlantı vardır.
  - Sahip olduğunuz [özelliği kullanıma alındı](##step-3-roll-out-the-feature) Grup İlkesi aracılığıyla bu kullanıcı için.

Burada yalnızca kullanıcı adı ve parola kullanıcının girdiği senaryoyu test etmek için:
   - Oturum https://myapps.microsoft.com/ yeni bir özel tarayıcı oturumunda.

Kullanıcının kullanıcı adı veya parola girmek için burada yoksa senaryoyu test etmek için aşağıdaki adımlardan birini kullanın: 
   - Oturum https://myapps.microsoft.com/contoso.onmicrosoft.com yeni bir özel tarayıcı oturumunda. Değiştirin *contoso* kiracınızın ada sahip.
   - Oturum https://myapps.microsoft.com/contoso.com yeni bir özel tarayıcı oturumunda. Değiştirin *contoso.com* kiracınıza doğrulanmış bir etki alanı (bir Federasyon etki alanı değil).

## <a name="step-5-roll-over-keys"></a>5. adım: anahtarları

Adım 2'de, Azure AD Connect sorunsuz çoklu oturum açma etkin tüm Active Directory ormanlarında bilgisayar hesaplarının (Azure AD temsil eden) oluşturur. Daha fazla bilgi için bkz. [Azure Active Directory sorunsuz çoklu oturum açma: teknik yakından bakışın](active-directory-aadconnect-sso-how-it-works.md). Gelişmiş güvenlik için Kerberos düzenli aralıklarla bu bilgisayar hesaplarının şifre çözme anahtarları alma öneririz. Anahtarları konusunda yönergeler için bkz. [Azure Active Directory sorunsuz çoklu oturum açma: sık sorulan sorular](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account).

>[!IMPORTANT]
>Bu adımda yapmanız gerekmez _hemen_ özelliği etkinleştirdikten sonra. Kerberos şifre çözme 30 günde bir en az bir kez anahtarları.

## <a name="next-steps"></a>Sonraki adımlar

- [Teknik yakından bakışın](active-directory-aadconnect-sso-how-it-works.md): sorunsuz çoklu oturum açma özelliği nasıl çalıştığını anlayın.
- [Sık sorulan sorular](active-directory-aadconnect-sso-faq.md): hakkında sorunsuz çoklu oturum açma sık sorulan soruların yanıtlarını alın.
- [Sorun giderme](active-directory-aadconnect-troubleshoot-sso.md): sorunsuz çoklu oturum açma özelliği ile ortak sorunları çözmeyi öğrenin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): dosya yeni özellik istekleri için Azure Active Directory Forumu kullanın.
