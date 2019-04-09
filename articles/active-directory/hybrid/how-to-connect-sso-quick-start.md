---
title: 'Azure AD Connect: Sorunsuz çoklu oturum açma - hızlı başlangıç | Microsoft Docs'
description: Bu makalede Azure Active Directory sorunsuz çoklu oturum açma ile çalışmaya başlama işlemini açıklamaktadır.
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
ms.date: 04/08/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f8483eb0ce8f5ea890e453828d36afda61ef86f
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59256898"
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Azure Active Directory sorunsuz çoklu oturum açma: Hızlı başlangıç

## <a name="deploy-seamless-single-sign-on"></a>Sorunsuz çoklu oturum açma dağıtma

Azure Active Directory (Azure AD) sorunsuz çoklu oturum açma (sorunsuz çoklu oturum açma) şirket ağınıza bağlı olan kurumsal masaüstü üzerinde olduğunda kullanıcıların otomatik olarak imzalar. Sorunsuz çoklu oturum açma, ek şirket içi bileşenleri gerek kalmadan kullanıcılarınız ile bulut tabanlı uygulamalarınızı kolayca erişmenizi sağlar.

Sorunsuz çoklu oturum açma dağıtmak için aşağıdaki adımları izleyin.

## <a name="step-1-check-the-prerequisites"></a>1. Adım: Önkoşulları denetleme

Aşağıdaki önkoşulların karşılandığından emin olun:

* **Azure AD Connect sunucunuzu kurarken**: Kullanırsanız [geçişli kimlik doğrulaması](how-to-connect-pta.md) hiçbir ek bir önkoşul denetimi, oturum açma yöntemi gereklidir. Kullanırsanız [parola karması eşitleme](how-to-connect-password-hash-synchronization.md) , oturum açma yöntemi olarak Azure AD Connect ve Azure AD arasında bir güvenlik duvarı varsa, emin olun:
   - Sürüm 1.1.644.0 kullanın veya Azure AD Connect sonraki bir sürümü. 
   - Güvenlik Duvarı veya proxy bağlantıları için DNS beyaz listeye ekleme, beyaz liste izin veriyorsa  **\*. msappproxy.net** URL'leri bağlantı noktası 443 üzerinden. Aksi takdirde, erişim izni [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653), hangi haftalık güncelleştirildi. Yalnızca özelliğini etkinleştirdiğinizde bu önkoşul geçerlidir. Gerçek kullanıcı oturum açma işlemleri için gerekli değildir.

    >[!NOTE]
    >Azure AD Connect sürüm 1.1.557.0, 1.1.558.0 1.1.561.0 ve 1.1.614.0 parola karması eşitleme için ilgili bir sorun var. Varsa, _yoksa_ okuma geçişli kimlik doğrulaması ile birlikte parola karması eşitleme kullanmayı düşündüğünüz [Azure AD Connect sürüm notları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470) daha fazla bilgi için.

* **Desteklenen bir kullanan Azure AD Connect topolojisi**: Azure AD Connect'in desteklenen topolojiler açıklanan birini kullandığınızdan emin olun [burada](plan-connect-topologies.md).

    >[!NOTE]
    >Olup olmadığını AD güvenleri arasında veya birden fazla AD ormanına sorunsuz çoklu oturum açmayı destekler.

* **Etki alanı yönetici kimlik bilgilerini ayarla**: Her Active Directory orman için etki alanı yönetici kimlik bilgilerine sahip olmanız gerekir:
    * Azure AD Connect aracılığıyla Azure AD'ye eşitleyin.
    * Sorunsuz çoklu oturum açma için etkinleştirmek istediğiniz kullanıcıları içerir.
    
* **Modern kimlik doğrulamasını etkinleştirme**: Etkinleştirmek gereken [modern kimlik doğrulaması](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016) kiracınıza bu özelliğin çalışması için.

* **Office 365 istemcileri en son sürümlerini kullanan**: Bir sessiz oturum açma deneyimi ile Office 365 istemcileri (Outlook, Word, Excel ve diğerleri) almak için kullanıcılarınızın sürümleri 16.0.8730.xxxx kullanmanız gerekir veya üzeri.

## <a name="step-2-enable-the-feature"></a>2. Adım: Özellik etkinleştirme

Sorunsuz çoklu oturum açma aracılığıyla etkinleştirme [Azure AD Connect](whatis-hybrid-identity.md).

>[!NOTE]
> Ayrıca [PowerShell kullanarak sorunsuz SSO etkinleştirme](tshoot-connect-sso.md#manual-reset-of-the-feature) Azure AD Connect, gereksinimlerinizi karşılamaması durumunda. Active Directory ormanı birden fazla etki alanında olması ve sorunsuz çoklu oturum açma için etkinleştirmek istediğiniz etki alanı hakkında daha fazla hedeflenen istiyorsanız bu seçeneği kullanın.

Yeni Azure AD Connect yüklemesini yapıyorsanız seçin [özel bir yükleme yolu](how-to-connect-install-custom.md). Konumunda **kullanıcı oturum açma** sayfasında **etkinleştirme çoklu oturum açma** seçeneği.

>[!NOTE]
> Oturum açma yöntemi ise yalnızca seçeneğin seçime uygun olması **parola karması eşitleme** veya **geçişli kimlik doğrulaması**.

![Azure AD Connect: Kullanıcı oturumu açma](./media/how-to-connect-sso-quick-start/sso8.png)

Azure AD Connect yüklemesi zaten varsa, seçin **değiştirme kullanıcı oturum açma** sayfa Azure AD Connect ve ardından **sonraki**. Azure AD Connect sürüm 1.1.880.0 kullanıyorsanız veya yukarıdaki **etkinleştirme çoklu oturum açma** seçeneği varsayılan olarak seçilir. Azure AD Connect'in eski sürümleri kullanıyorsanız **etkinleştirme çoklu oturum açma** seçeneği.

![Azure AD Connect: Kullanıcı oturum açma](./media/how-to-connect-sso-quick-start/changeusersignin.png)

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

![Azure portalı: Azure AD Connect bölmesi](./media/how-to-connect-sso-quick-start/sso10.png)

>[!IMPORTANT]
> Sorunsuz çoklu oturum açma adlı bir bilgisayar hesabı oluşturur `AZUREADSSOACC` şirket içi AD ormanındaki Active Directory (AD) içinde. `AZUREADSSOACC` Bilgisayar hesabını güvenlik nedenleriyle kesin korunması gerekir. Yalnızca Domain Admins bilgisayar hesabını yönetmek görebilmeniz gerekir. Bilgisayar hesabının Kerberos temsilcisi seçmeyi devre dışı emin olun. Bilgisayar hesabının bir kuruluş birimi (OU) içinde yanlışlıkla silinmekten güvenli olduğu ve yalnızca etki alanı yöneticileri erişimi Store.

>[!NOTE]
> Pass--Hash ve kimlik bilgisi Hırsızlıklarını azaltma mimarileri, şirket içi ortamınızda kullanıyorsanız, emin olmak için gerekli değişiklikleri yapmanızı `AZUREADSSOACC` bilgisayar hesabı bitmiyor karantina kapsayıcısında. 

## <a name="step-3-roll-out-the-feature"></a>3. Adım: Özelliği kullanıma alma

Aşağıda sağlanan yönergeleri kullanarak kullanıcılarınıza kademeli olarak sorunsuz çoklu oturum açma geri alabilirsiniz. Tüm Azure AD aşağıdaki URL'yi ekleyerek başlattığınızda veya Active Directory'de Grup İlkesi'ni kullanarak seçili kullanıcıların Intranet bölgesi ayarları:

- `https://autologon.microsoftazuread-sso.com`

Ayrıca, bir Intranet bölgesi İlkesi adlı ayarını etkinleştirmeniz gerekir **izin vermek için durum çubuğu komut dosyası aracılığıyla güncelleştirmeleri** Grup İlkesi aracılığıyla. 

>[!NOTE]
> (Bir dizi güvenilen site URL'leri Internet Explorer ile paylaştığı) aşağıdaki yönergeler yalnızca Internet Explorer ve Google Chrome'un Windows üzerinde çalışır. Mozilla Firefox ve Google Chrome Macos'ta ayarlama hakkında yönergeler için sonraki bölüme okuyun.

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a>Kullanıcıların Intranet bölge ayarlarını değiştirmek neden ihtiyacınız var?

Varsayılan olarak, tarayıcı, doğru bölgeyi, Internet veya Intranet, belirli bir URL'den otomatik olarak hesaplar. Örneğin, `http://contoso/` ise Intranet bölgesine eşler `http://intranet.contoso.com/` (URL bir dönemi içerdiğinden) Internet bölgesine eşler. Tarayıcının Intranet bölgesine açıkça URL eklemediğiniz sürece tarayıcılar Kerberos biletleri Azure AD URL'yi gibi bir bulut uç noktasına gönderir.

Kullanıcıların Intranet bölge ayarlarını değiştirmek için iki yolu vardır:

| Seçenek | Yönetici önemli noktalar | Kullanıcı deneyimi |
| --- | --- | --- |
| Grup ilkesi | Intranet bölgesi ayarlarını düzenleme aşağı yönetim kilitleri | Kullanıcılar kendi ayarlarını değiştiremez. |
| Grup İlkesi tercihi |  Intranet bölgesi ayarlarını düzenleme yönetim sağlar. | Kullanıcılar kendi ayarlarını değiştirebilirsiniz. |

### <a name="group-policy-option---detailed-steps"></a>"Grup İlkesi" seçeneği - ayrıntılı adımlar

1. Grup İlkesi Yönetimi Düzenleyicisi Aracı'nı açın.
2. Bazı uygulanan Grup ilkesini düzenleyin veya tüm kullanıcılar. Bu örnekte **varsayılan etki alanı ilkesi**.
3. Gözat **Kullanıcı Yapılandırması** > **ilke** > **Yönetim Şablonları** > **Windows Bileşenleri** > **Internet Explorer** > **Internet Denetim Masası** > **güvenlik sayfası**. Ardından **siteyi bölgeye ataması Listesi'ni**.
    ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso6.png)
4. İlkeyi etkinleştirin ve sonra da iletişim kutusunda aşağıdaki değerleri girin:
   - **Değer adı**: Azure AD, Kerberos biletleri iletildiği URL'si.
   - **Değer** (veriler): **1** Intranet gösterir.

     Sonucu şöyle görünür:

     Değer adı: `https://autologon.microsoftazuread-sso.com`
  
     Değer (veriler): 1

   >[!NOTE]
   > Bazı kullanıcıların (örneğin, bu kullanıcılar üzerinde paylaşılan bilgi noktaları oturum açarsanız) kullanarak sorunsuz çoklu oturum açmayı engellemek istiyorsanız, önceki değerlerini ayarlamak **4**. Bu eylem, Azure AD URL'si için kısıtlı bölge ekler ve her zaman sorunsuz çoklu oturum açma başarısız.
   >

5. Seçin **Tamam**ve ardından **Tamam** yeniden.

    ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso7.png)

6. Gözat **Kullanıcı Yapılandırması** > **Yönetim Şablonları** **ilke** > ** > **Windows bileşenleri**  >  **Internet Explorer** > **Internet Denetim Masası** > **güvenlik sayfası**  >   **Intranet bölgesi**. Ardından **izin vermek için durum çubuğu komut dosyası aracılığıyla güncelleştirmeleri**.

    ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso11.png)

7. İlke ayarını etkinleştirin ve ardından **Tamam**.

    ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso12.png)

### <a name="group-policy-preference-option---detailed-steps"></a>"Grup İlkesi tercih" seçeneği - ayrıntılı adımlar

1. Grup İlkesi Yönetimi Düzenleyicisi Aracı'nı açın.
2. Bazı uygulanan Grup ilkesini düzenleyin veya tüm kullanıcılar. Bu örnekte **varsayılan etki alanı ilkesi**.
3. Gözat **Kullanıcı Yapılandırması** > **tercihleri** > **Windows ayarları** > **kayıt defteri**  >  **Yeni** > **kayıt defteri öğesi**.

    ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso15.png)

4. Uygun alanlara aşağıdaki değerleri girin ve tıklayın **Tamam**.
   - **Anahtar yolu**: ***Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\microsoftazuread-sso.com\autologon***
   - **Değer adı**: ***https***.
   - **Değer türü**: ***REG_DWORD***.
   - **Değer verisi**: ***00000001***.
 
     ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso16.png)
 
     ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso17.png)

### <a name="browser-considerations"></a>Tarayıcı konuları

#### <a name="mozilla-firefox-all-platforms"></a>Mozilla Firefox (tüm platformlar)

Mozilla Firefox, otomatik olarak Kerberos kimlik doğrulaması kullanmaz. Her kullanıcı el ile Firefox ayarlarına Azure AD URL'si aşağıdaki adımları kullanarak eklemeniz gerekir:
1. Firefox çalıştırın ve girin `about:config` adres çubuğundaki. Gördüğünüz herhangi bir bildirim kapatın.
2. Arama **network.negotiate auth.trusted URI'ler** tercihi. Bu tercih, Kerberos kimlik doğrulaması için Firefox'ın Güvenilen siteler listelenir.
3. Sağ tıklayıp **Değiştir**.
4. Girin `https://autologon.microsoftazuread-sso.com` alanında.
5. Seçin **Tamam** tarayıcıyı kapatıp açın.

#### <a name="safari-macos"></a>Safari (macOS)

MacOS çalıştıran makinenin AD'ye katıldığından emin olun. Yönergeler AD katılma için macOS Cihazınızı bu makalenin kapsamı dışında ' dir.

#### <a name="google-chrome-all-platforms"></a>Google Chrome (tüm platformlar)

Siz kıldıysanız [AuthNegotiateDelegateWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthNegotiateDelegateWhitelist) veya [AuthServerWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthServerWhitelist) ilke ayarları, ortamınızda olun Azure AD'nin URL'si ekleyin (`https://autologon.microsoftazuread-sso.com`) onlara de.

#### <a name="google-chrome-macos-and-other-non-windows-platforms"></a>Google Chrome (macOS ve Windows olmayan diğer platformları)

Mac OS ve diğer Windows olmayan platformlar için Google Chrome, başvurmak [Chromium proje Policy List](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) nasıl beyaz liste ile kimlik doğrulaması için Azure AD URL'sini tümleştirilmiş hakkında bilgi.

Bu makalenin kapsamı dışında Azure AD URL Google Chrome ve Firefox ile Mac kullanıcılarının içine almak için üçüncü taraf Active Directory Grup İlkesi uzantıları kullanılır.

#### <a name="known-browser-limitations"></a>Bilinen tarayıcı sınırlamaları

Sorunsuz çoklu oturum açma, Firefox ve Microsoft Edge tarayıcılarda özel tarama modunda çalışmıyor. Tarayıcı Gelişmiş korumalı modda çalışıyorsa aynı zamanda Internet Explorer'da çalışmaz.

## <a name="step-4-test-the-feature"></a>4. Adım: Bu özelliği sınama

Özellik belirli bir kullanıcı için test etmek için aşağıdaki tüm koşulların karşılandığından emin olun:
  - Kurumsal bir cihazda oturum açtığında kullanıcı.
  - Cihaz, Active Directory etki alanına katıldı. Cihaz _değil_ olmasına gerek [Azure AD katıldı](../active-directory-azureadjoin-overview.md).
  - Cihaz, Kurumsal kablolu veya kablosuz ağda veya bir VPN bağlantısı gibi bir uzaktan erişim bağlantısı aracılığıyla etki alanı denetleyicisine (DC) doğrudan bir bağlantı vardır.
  - Sahip olduğunuz [özelliği kullanıma alındı](##step-3-roll-out-the-feature) Grup İlkesi aracılığıyla bu kullanıcı için.

Burada yalnızca kullanıcı adı ve parola kullanıcının girdiği senaryoyu test etmek için:
   - Oturum `https://myapps.microsoft.com/` yeni bir özel tarayıcı oturumunda.

Kullanıcının kullanıcı adı veya parola girmek için burada yoksa senaryoyu test etmek için aşağıdaki adımlardan birini kullanın: 
   - Oturum `https://myapps.microsoft.com/contoso.onmicrosoft.com` yeni bir özel tarayıcı oturumunda. Değiştirin *contoso* kiracınızın ada sahip.
   - Oturum `https://myapps.microsoft.com/contoso.com` yeni bir özel tarayıcı oturumunda. Değiştirin *contoso.com* kiracınıza doğrulanmış bir etki alanı (bir Federasyon etki alanı değil).

## <a name="step-5-roll-over-keys"></a>5. Adım: Anahtarları

Adım 2'de, Azure AD Connect sorunsuz çoklu oturum açma etkin tüm Active Directory ormanlarında bilgisayar hesaplarının (Azure AD temsil eden) oluşturur. Daha fazla bilgi için bkz: [Azure Active Directory sorunsuz çoklu oturum açma: Teknik yakından bakışın](how-to-connect-sso-how-it-works.md).

>[!IMPORTANT]
>Bir bilgisayar hesabı Kerberos şifre çözme anahtarının sızmasına, kendi AD ormanında herhangi bir kullanıcı için Kerberos anahtarları oluşturmak için kullanılabilir. Kötü amaçlı aktörler sonra Azure AD oturum açma işlemleri riskli kullanıcıların kimliğine bürünebilir. Düzenli aralıklarla yenilediğinizden emin bu Kerberos şifre çözme anahtarları üzerinden - 30 günde bir en az bir kez önemle öneririz.

Anahtarları konusunda yönergeler için bkz: [Azure Active Directory sorunsuz çoklu oturum açma: Sık sorulan sorular](how-to-connect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account). Anahtarları otomatik toplama üzerinden tanıtmak için bir özellik çalışıyoruz.

>[!IMPORTANT]
>Bu adımda yapmanız gerekmez _hemen_ özelliği etkinleştirdikten sonra. Kerberos şifre çözme 30 günde bir en az bir kez anahtarları.

## <a name="next-steps"></a>Sonraki adımlar

- [Teknik yakından bakışın](how-to-connect-sso-how-it-works.md): Sorunsuz çoklu oturum açma özelliği nasıl çalıştığını anlayın.
- [Sık sorulan sorular](how-to-connect-sso-faq.md): Hakkında sorunsuz çoklu oturum açma sık sorulan soruların yanıtlarını alın.
- [Sorun giderme](tshoot-connect-sso.md): Sorunsuz çoklu oturum açma özelliği ile sık karşılaşılan sorunların nasıl çözümleneceğini öğrenin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Yeni özellik istekleriniz dosya için Azure Active Directory Forumu kullanın.
