---
title: "Azure AD Connect: Sorunsuz çoklu oturum açma - hızlı başlangıç | Microsoft Docs"
description: "Bu makalede, Azure Active Directory sorunsuz çoklu oturum açma ile çalışmaya başlama açıklar."
services: active-directory
keywords: "Azure AD, SSO, gerekli bileşenleri yükleme Active Directory, Azure AD Connect nedir çoklu oturum açma"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: 8975a82c5573cc0c284e1fc76cd0ef2c19fbbd72
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Azure Active Directory sorunsuz çoklu oturum açma: Hızlı Başlangıç

## <a name="how-to-deploy-seamless-sso"></a>Sorunsuz SSO dağıtma

Azure Active Directory sorunsuz çoklu oturum açma (Azure AD sorunsuz SSO) otomatik olarak oturum açtığında şirket ağınıza bağlı kurumsal masaüstü üzerinde olduklarında kullanıcıların. Şirket içinde ek bileşenleri gerek kalmadan, kullanıcılarınızın bulut tabanlı uygulamalar için kolay erişim sağlar.

Sorunsuz SSO dağıtmak için şu adımları izlemesi gerekir:

## <a name="step-1-check-prerequisites"></a>1. adım: Önkoşul denetimi

Aşağıdaki önkoşulların yerine getirildiğinden emin olun:

1. Azure AD Connect sunucusunu ayarlama: kullanırsanız [doğrudan kimlik doğrulama](active-directory-aadconnect-pass-through-authentication.md) , oturum açma yöntemi, hiçbir ek Önkoşul denetimi gereklidir. Kullanırsanız [parola karması eşitlemesi](active-directory-aadconnectsync-implement-password-synchronization.md) , oturum açma yöntemi olarak ve Azure AD Connect ve Azure AD arasında bir güvenlik duvarı varsa, emin olun:
- Sürümleri 1.1.644.0 kullanıyorsanız veya Azure AD Connect sonraki. 
- Güvenlik Duvarı veya proxy DNS uygulamaları, da beyaz liste bağlantıları için güvenilir listeye almayı sağlar,  **\*. msappproxy.net** URL'leri bağlantı noktası 443 üzerinden. Aksi durumda, erişim izni [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653), hangi haftalık güncelleştirildi. Yalnızca asıl kullanıcı oturum açma işlemleri için özelliğini etkinleştirdiğinizde bu geçerli bir önkoşuldur.

    >[!NOTE]
    >Azure AD Connect sürümleri 1.1.557.0, 1.1.558.0, 1.1.561.0 ve 1.1.614.0 parola karması eşitlemesi için ilgili bir sorun var. Varsa, _yok_ parola karması eşitlemesi okuma doğrudan kimlik doğrulama ile birlikte kullanmayı düşündüğünüz [Azure AD Connect sürüm notları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470) daha fazla bilgi için.

2. (Azure AD Connect'i kullanarak) Azure AD eşitleme ve olan kullanıcılar için sorunsuz SSO etkinleştirmek istediğiniz her AD ormanı için etki alanı yönetici kimlik bilgileri gerekir.

## <a name="step-2-enable-the-feature"></a>2. adım: özelliğini etkinleştir

Sorunsuz SSO kullanarak etkinleştirilebilir [Azure AD Connect](active-directory-aadconnect.md).

Azure AD Connect yeni yüklemesini yapıyorsanız seçin [özel yükleme yolu](active-directory-aadconnect-get-started-custom.md). "Kullanıcı oturum açma" sayfanın "çoklu oturum açmayı etkinleştir" seçeneği işaretleyin.

![Azure AD Connect - kullanıcı oturum açma](./media/active-directory-aadconnect-sso/sso8.png)

Azure AD Connect yüklemesi zaten varsa, "Değişiklik kullanıcı oturum açma sayfasında" Azure AD Connect seçin ve "İleri" yi tıklatın. Ardından "çoklu oturum açmayı etkinleştir" seçeneği işaretleyin.

![Azure AD Connect - değişiklik kullanıcı oturum açma](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

"Çoklu oturum açmayı etkinleştir" sayfasına ulaşana kadar sihirbaza devam edin. (Azure AD Connect'i kullanarak) Azure AD eşitleme ve olan kullanıcılar için sorunsuz SSO etkinleştirmek istediğiniz her AD orman için etki alanı yönetici kimlik bilgilerini girin. 

Sihirbaz tamamlandıktan sonra Kiracı'sorunsuz SSO etkin.

>[!NOTE]
> Etki alanı yönetici kimlik bilgileri Azure AD Connect veya Azure AD depolanmaz, ancak yalnızca özelliğini etkinleştirmek için kullanılır.

Sorunsuz SSO doğru etkinleştirdiğinizden emin doğrulamak için bu yönergeleri izleyin:

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) kiracınız için genel yönetici kimlik bilgilerine sahip.
2. Seçin **Azure Active Directory** sol gezinti çubuğunda.
3. Seçin **Azure AD Connect**.
4. Doğrulayın **sorunsuz çoklu oturum açma** özelliği gösterildiğinden **etkin**.

![Azure portal - Azure AD Connect dikey penceresi](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-the-feature"></a>3. adım: özelliği alma

Kullanıcılarınız için özelliği geri dönmek için Active Directory'de Grup İlkesi kullanarak kullanıcıların Intranet bölge ayarlarını aşağıdaki Azure AD URL'leri eklemeniz gerekir:

- https://AutoLogon.microsoftazuread-sso.com
- https://aadg.Windows.NET.nsatc.NET

Ayrıca, "durum çubuğunda komut dosyası aracılığıyla güncelleştirmeleri izin ver" olarak adlandırılan (Grup İlkesi kullanarak) bir Intranet bölgesine ilkesi ayarını etkinleştirmeniz gerekir.

>[!NOTE]
> (Bunu kümesini güvenilen site URL'lerin Internet Explorer ile paylaşır) aşağıdaki yönergeleri yalnızca Internet Explorer ve Google Chrome için Windows üzerinde çalışır. Mozilla Firefox ve Mac üzerinde Google Chrome ayarlama yönergeleri için sonraki bölüme okuma

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a>Neden kullanıcıların Intranet bölgesi ayarlarını değiştirmeniz gerekiyor mu?

Varsayılan olarak, tarayıcının bir URL sağ bölgesi (Internet veya Intranet) otomatik olarak hesaplar. Örneğin, (URL nokta içerdiğinden) http://intranet.contoso.com/ Internet bölgesine eşlendi ancak http://contoso/ Intranet bölgesine eşlenir. URL'sini tarayıcının Intranet bölgesine açıkça eklenmediği sürece tarayıcıları bir bulut uç noktası için - iki Azure AD URL'leri gibi-Kerberos biletleri gönderme.

### <a name="detailed-steps"></a>Ayrıntılı adımlar

1. Grup İlkesi yönetim aracını açın.
2. Bazı uygulanan Grup İlkesi düzenleme veya tüm kullanıcılar. Bu örnekte, kullandığımız **varsayılan etki alanı ilkesi**.
3. Gidin **Kullanıcı Yapılandırması\Yönetim Şablonları\Windows Bileşenleri\Internet Explorer\Internet Denetim Panel\Security sayfası** seçip **siteyi bölgeye ataması listesi**.
![Çoklu oturum açma](./media/active-directory-aadconnect-sso/sso6.png)
4. İlkeyi etkinleştirin ve aşağıdaki değerleri (Azure AD URL'ler Kerberos biletleri yere iletilmez) ve veri girin (*1* Intranet bölgesine gösterir) iletişim kutusunda.

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
>[!NOTE]
> Bu kullanıcılar paylaşılan bilgi noktaları üzerinde - imzalama, bazı kullanıcıların sorunsuz SSO - Örneğin, kullanarak izin vermeyecek şekilde istiyorsanız önceki değerleri kümesine *4*. Bu eylem, Azure AD URL'leri kısıtlı bölgesine ekler ve sorunsuz SSO her zaman başarısız olur.

5. Tıklatın **Tamam** ve **Tamam** yeniden.
![Çoklu oturum açma](./media/active-directory-aadconnect-sso/sso7.png)
6. Gidin **Kullanıcı Yapılandırması\Yönetim Şablonları\Windows Bileşenleri\Internet Explorer\Internet Denetim Panel\Security Page\Intranet bölge** seçip **izin komut dizisiiledurumçubuğugüncelleştirmeleri**.
![Çoklu oturum açma](./media/active-directory-aadconnect-sso/sso11.png)
7. İlke ayarını etkinleştirin ve tıklatın **Tamam**.
![Çoklu oturum açma](./media/active-directory-aadconnect-sso/sso12.png)

### <a name="browser-considerations"></a>Tarayıcı konuları

#### <a name="mozilla-firefox"></a>Mozilla Firefox

Mozilla Firefox otomatik olarak Kerberos kimlik doğrulamasını yapmaz. Azure AD URL'leri Firefox ayarlarına aşağıdaki adımları kullanarak el ile eklemek her kullanıcının vardır:
1. Firefox çalıştırın ve girin `about:config` adres çubuğundaki. Gördüğünüz herhangi bir bildirim yok sayın.
2. Arama **network.negotiate auth.trusted URI'ler** tercih. Bu tercih Kerberos kimlik doğrulaması için Firefox'in Güvenilen siteler listelenir.
3. Sağ tıklayın ve "Değiştir"'i seçin.
4. Girin "https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net" alanında.
5. "Tamam" düğmesini tıklatın ve tarayıcıyı kapatıp yeniden açın.

#### <a name="safari-on-mac-os"></a>Mac OS Safari

Mac OS çalıştıran makine için AD alanına katılmış olmasını sağlayın. Bunu nasıl yönergeler bkz [burada](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).

#### <a name="google-chrome-on-mac-os"></a>Mac OS Google Chrome

Mac OS ve diğer Windows olmayan platformlarda Google Chrome için başvurmak [bu makalede](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) tümleşik kimlik doğrulaması için beyaz liste Azure AD URL'ler hakkında bilgi edinmek için.

Azure AD URL'lerine Firefox ve Google Chrome Mac Kullanıcıları geri dönmek için üçüncü taraf Active Directory Grup İlkesi uzantıları kullanarak bu makalenin kapsamı dışında değildir.

#### <a name="known-browser-limitations"></a>Bilinen tarayıcı sınırlamaları

Sorunsuz SSO Firefox ve kenar tarayıcılarda özel gözatma modunda çalışmıyor. Tarayıcı geliştirilmiş koruma modunda çalışıyorsa, ayrıca Internet Explorer, işe yaramaz.

>[!IMPORTANT]
>Biz son müşteri bildirilen sorunları araştırmak sınır desteği geri.

## <a name="step-4-test-the-feature"></a>4. adım: Test özelliği

Belirli bir kullanıcı için özelliğini sınamak için emin _tüm_ aşağıdaki koşulların karşılandığından:
  - Kullanıcı bir kurumsal aygıtta oturum açma.
  - Cihaz daha önce Active Directory (AD) etki alanına katıldı.
  - Cihaz için etki alanı denetleyicisi (DC), Kurumsal kablolu veya kablosuz ağ üzerindeki veya bir VPN bağlantısı gibi bir uzaktan erişim bağlantısı aracılığıyla doğrudan bir bağlantı vardır.
  - Sahip olduğunuz [özelliği alındı](##step-3-roll-out-the-feature) Grup İlkesi kullanarak bu kullanıcı için.

Burada yalnızca kullanıcı adı ve parola kullanıcının girdiği senaryoyu test etmek için:
- Oturum *https://myapps.microsoft.com/* yeni bir özel tarayıcı oturumunda.

Burada kullanıcının kullanıcı adı veya parola girmesi gerekmez senaryoyu test etmek için: 
- Oturum *https://myapps.microsoft.com/contoso.onmicrosoft.com* yeni bir özel tarayıcı oturumunda. Değiştir "*contoso*", kiracının ada sahip.
- Veya oturum *https://myapps.microsoft.com/contoso.com* yeni bir özel tarayıcı oturumunda. Değiştir "*contoso.com*" kiracınızda doğrulanmış bir etki alanıyla (bir Federasyon etki alanı değil).

## <a name="step-5-roll-over-keys"></a>5. adım: anahtarlar üzerinden alma

2. adımda, Azure AD Connect (Azure AD temsil eder) bilgisayar hesapları sorunsuz SSO etkin tüm ormanlardaki AD oluşturur. İçinde daha ayrıntılı bilgi [burada](active-directory-aadconnect-sso-how-it-works.md). Gelişmiş güvenlik için Kerberos düzenli aralıklarla bu bilgisayar hesaplarının şifre çözme anahtarlarını alma önerilir. Atla yönergeler olan [burada](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account).

>[!IMPORTANT]
>Bu adım gerekmez _hemen_ özelliği etkinleştirdikten sonra. En az 30 günde Kerberos şifre çözme anahtarlarını alma.

## <a name="next-steps"></a>Sonraki adımlar

- [Teknik derinlemesine](active-directory-aadconnect-sso-how-it-works.md) -bu özellik nasıl çalıştığını anlayın.
- [Sık sorulan sorular](active-directory-aadconnect-sso-faq.md) -sık sorulan sorulara yanıtlar.
- [Sorun giderme](active-directory-aadconnect-troubleshoot-sso.md) -özelliği ile ilgili genel sorunları çözmeyi öğrenin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleri dosyalama için.
