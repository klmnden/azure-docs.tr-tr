---
title: Azure Active Directory parolasız oturum açma (önizlemede) yapılandırın
description: FIDO2 güvenlik anahtarları veya Microsoft Authenticator uygulamasını (Önizleme) kullanarak Azure AD'ye parolasız oturum açmayı etkinleştir
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5ba2545467ebfbd032408aeee25b82b92a628f2a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712078"
---
# <a name="enable-passwordless-sign-in-for-azure-ad-preview"></a>Parolasız oturum açma için Azure AD (Önizleme) etkinleştirme

## <a name="requirements"></a>Gereksinimler

* Azure Multi-Factor Authentication
* Birleşik kayıt Önizleme
* Uyumlu FIDO2 güvenlik anahtarları FIDO2 güvenlik anahtar Önizleme gerektirir
* WebAuthN Microsoft Edge Windows 10 sürüm 1809 veya üzerini gerektirir
* FIDO2 Azure AD tabanlı Windows oturum açma gerektirir alanına katılmış Windows 10 sürüm 1809 veya üzeri

## <a name="prepare-devices-for-preview"></a>Cihazlar Önizleme için hazırlama

İle Pilot cihazların Windows 10 sürüm 1809 veya üzerini çalıştırıyor olması gerekir. Windows 10'da 1903 veya üzeri bir sürümü en iyi deneyimidir.

## <a name="enable-security-keys-for-windows-sign-in"></a>Windows oturum açma güvenlik tuşları etkinleştir

Kuruluşların kullanmayı seçebilir veya daha fazla bilgi için Windows güvenlik anahtarları kullanımını etkinleştirmek için aşağıdaki yöntemlerden birini oturum açın.

### <a name="enable-credential-provider-via-intune"></a>Kimlik bilgisi sağlayıcısı Intune aracılığıyla etkinleştirme

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Gözat **Intune** > **cihaz kaydı** > **Windows kayıt** > **Windows Hello İş için** > **özellikleri**.
1. Altında **ayarları** ayarlamak **oturum açmak için güvenlik tuşlarını** için **etkin**.

Oturum açma, güvenlik tuşları Yapılandırması Windows iş için Hello yapılandırma konusunda bağımlı değil.

#### <a name="enable-targeted-intune-deployment"></a>Intune dağıtımı hedeflenen etkinleştir

Kimlik bilgisi sağlayıcısını etkinleştirmek için belirli cihaz grupları hedeflemek için Intune aracılığıyla aşağıdaki özel ayarları kullanın. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Gözat **Intune** > **cihaz Yapılandırması** > **profilleri** > **profilioluşturma**.
1. Aşağıdaki ayarlarla yeni profili yapılandırma
   1. Ad: Windows oturum açma için güvenlik anahtarları
   1. Açıklama: Windows oturum açma sırasında kullanılacak FIDO güvenlik anahtarları sağlar
   1. Platform: Windows 10 ve üzeri
   1. Platform türü: Özel
   1. Özel OMA-URI ayarları:
      1. Ad: Windows oturum açma için FIDO güvenlik tuşlarını etkinleştir
      1. OMA-URI: ./Device/Vendor/MSFT/PassportForWork/SecurityKey/UseSecurityKeyForSignin
      1. Veri türü: Tamsayı
      1. Değer: 1. 
1. Bu ilke, belirli kullanıcılar, cihazlar veya gruplarına atanabilir. Daha fazla bilgi makalesinde bulunabilir [Intune kullanıcı ve cihaz profillerini atama](https://docs.microsoft.com/intune/device-profile-assign).

![Intune özel cihaz yapılandırma ilkesi oluşturma](./media/howto-authentication-passwordless-enable/intune-custom-profile.png)

### <a name="enable-credential-provider-via-provisioning-package"></a>Sağlama paketi aracılığıyla kimlik bilgisi sağlayıcısını etkinleştir

Intune tarafından yönetilmeyen cihazlar için bir sağlama paketi işlevselliğini etkinleştirmek için yüklenebilir. Windows yapılandırma Tasarımcısı uygulamasında yüklenebilir [Microsoft Store](https://www.microsoft.com/store/apps/9nblggh4tx22).

1. Windows yapılandırma Tasarımcısı'nı başlatın.
1. Seçin **dosya** > **yeni proje**.
1. Projenize bir ad verin ve projenizin oluşturulduğu yolunu not alın.
1. **İleri**’yi seçin.
1. Bırakın **sağlama paketi** olarak **Seçili proje iş akışı** seçip **sonraki**.
1. Seçin **tüm Windows Masaüstü sürümleri** altında **görüntülemek ve yapılandırmak için hangi ayarları seçin** seçip **sonraki**.
1. **Son**’u seçin.
1. Yeni oluşturulan projeniz için Gözat **çalışma zamanı ayarlarını** > **WindowsHelloForBusiness** > **SecurityKeys**  >  **UseSecurityKeyForSignIn**.
1. Ayarlama **UseSecurityKeyForSignIn** için **etkin**.
1. Seçin **dışarı** > **sağlama paketi**
1. Varsayılan değerleri bırakın **derleme** penceresinin altında **sağlama paketini açıklayan** seçip **sonraki**.
1. Varsayılan değerleri bırakın **derleme** penceresinin altında **seçin sağlama paketi için güvenlik ayrıntılarını** seçip **sonraki**.
1. Not edin veya yolda değiştirmek **derleme** windows altında **sağlama paketinin kaydedileceği konumu seçin** seçip **sonraki**.
1. Seçin **derleme** üzerinde **sağlama paketi oluşturun** sayfası.
1. Oluşturulan iki dosyaları (ppkg ve cat) burada bunları makineler için daha sonra uygulayabilirsiniz bir konuma kaydedin.
1. Bu makaledeki yönergeleri [sağlama paketi uygulama](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-apply-package)oluşturduğunuz sağlama paketi uygulamak için.

## <a name="obtain-fido2-security-keys"></a>FIDO2 güvenlik anahtarları alma

Bu makalede FIDO2 güvenlik anahtarları bölümüne bakın [parolasız nedir?](concept-authentication-passwordless.md) desteklenen anahtarları ve üreticilerinin hakkında daha fazla bilgi.

> [!NOTE]
> Satın alma ve temel NFC güvenlik anahtarları kullanmayı planlıyorsanız, desteklenen bir NFC okuyucusu gerekir.

## <a name="enable-passwordless-authentication-methods"></a>Parolasız kimlik doğrulama yöntemleri etkinleştir

### <a name="enable-the-combined-registration-experience"></a>Birleşik kayıt deneyimine olanak tanıyın

Birleşik kayıt Preview'de FIDO2 güvenlik anahtarları için kayıt özellikleri kullanır. Birleşik kayıt önizlemesini etkinleştirmek için aşağıdaki adımları izleyin.

1. [Azure portalda](https://portal.azure.com) oturum açma
1. Gözat **Azure Active Directory** > **kullanıcı ayarları**
   1. Tıklayarak **erişim paneli Önizleme özellikleri için ayarları yönetin**
   1. Altında **kullanıcılar kaydetme ve Gelişmiş güvenlik bilgisi - yönetmek için Önizleme özelliklerini kullanabilir**.
      1. Seçin **seçili** önizlemede katılacak olan kullanıcılardan oluşan bir grubu seçin.
      1. Ya da seçin **tüm** dizininizdeki herkes için etkinleştirme.
1. **Kaydet**'e tıklayın.

### <a name="enable-new-passwordless-authentication-methods"></a>Yeni parolasız kimlik doğrulama yöntemleri etkinleştir

1. [Azure portalda](https://portal.azure.com) oturum açma
1. Gözat **Azure Active Directory** > **kimlik doğrulama yöntemleri** > **kimlik doğrulama yöntemi İlkesi (Önizleme)**
1. Her altında **yöntemi**, aşağıdaki seçenekleri belirleyin
   1. **Etkinleştirme** - Evet veya Hayır
   1. **Hedef** -tüm kullanıcılar veya seçilen kullanıcılar
1. **Kaydet** her yöntemi

> [!WARNING]
> FIDO2 "kısıtlama ilkeleri anahtar" henüz çalışmıyor. Bu işlevselliği kullanıma önce genel kullanıma sunulacaktır, lütfen bu ilkeleri varsayılan değiştirmeyin.

> [!NOTE]
> İçinde yöntemlerin her ikisi de parolasız için iyileştirilmiş gerekmez (yalnızca bir parolasız yöntemi Önizleme istiyorsanız, bu yöntem yalnızca etkinleştirebilirsiniz). Öneriyoruz her ikisi de kendi avantajları olduğundan hem metotları deneyin.

## <a name="user-registration-and-management-of-fido2-security-keys"></a>Kullanıcı kaydı ve Yönetimi FIDO2 güvenlik anahtarları

1. Göz atın [https://myprofile.microsoft.com](https://myprofile.microsoft.com)
1. Oturum oturum
1. Tıklayın **güvenlik bilgisi**
   1. Kullanıcı zaten kayıtlı olan en az bir Azure multi-Factor Authentication yöntemi varsa, bunlar hemen FIDO2 güvenlik anahtarı kaydedebilirsiniz.
   1. En az bir Azure multi-Factor Authentication yöntemi kayıtlı sahip değilseniz, bunların birini eklemeniz gerekir.
1. Tıklayarak FIDO2 güvenlik anahtarı ekleyin **yöntemi ekleyin** seçip **güvenlik anahtarı**
1. Seçin **USB cihazını** veya **NFC cihaz**
1. Hazır ve seçin, anahtara sahip **İleri**
1. Bir kutu görünür ve sizden oluşturma/PIN için güvenlik anahtarınızı girin ardından gerekli hareketi anahtarınız için aşağıdakilerden birini gerçekleştirin biyometrik ya da dokunun.
1. Birleşik kayıt deneyimi için döndürülür ve belirtecinizi için anlamlı bir ad sağlar, böylece birden fazla varsa, hangisinin tanımlayabilirsiniz istenir.           **İleri**'ye tıklayın.
1. Tıklayın **Bitti** işlemi tamamlamak için

### <a name="manage-security-key-biometric-pin-or-reset-security-key"></a>Güvenlik anahtarı biyometrik, PIN, yönetme veya güvenlik anahtarı sıfırlama

* Windows 10 sürüm 1809
   * Güvenlik anahtarı satıcıdan yazılım Yardımcısı gereklidir
* Windows 10 sürüm 1903 veya üzeri
   * Kullanıcılar **Windows ayarları** cihazlarında > **hesapları** > **güvenlik anahtarı**
   * Kullanıcılar kendi PIN'LERİNİ değiştirmesi, Biyometri güncelleştirebilir veya kendi güvenlik anahtarı sıfırlama

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>Kullanıcı kaydı ve yönetimi için Microsoft Authenticator uygulaması

Telefon oturum açma için Microsoft Authenticator uygulamasını yapılandırmak için bu makaledeki yönergeleri [hesaplarınız için Microsoft Authenticator uygulamasını kullanarak oturum açın](../user-help/user-help-auth-app-sign-in.md).

## <a name="sign-in-with-passwordless-credentials"></a>Parolasız kimlik bilgilerinizle oturum açın

### <a name="sign-in-at-the-lock-screen"></a>Kilit ekranında oturum açın

Bir kullanıcı aşağıdaki örnekte kendi FIDO2 güvenlik anahtarı zaten Bala Sandhu sağladığını. Bala, Windows 10 kilit ekranından güvenlik anahtar kimlik bilgisi sağlayıcı seçin ve Windows oturum için güvenlik anahtarı ekleyin.

![Güvenlik anahtarı Windows 10 kilit ekranında oturum](./media/howto-authentication-passwordless-enable/fido2-windows-10-1903-sign-in-lock-screen.png)

### <a name="sign-in-on-the-web"></a>Web üzerinde oturum açın

Bir kullanıcı aşağıdaki örnekte zaten FIDO2 anahtarının sağladığını. Web üzerinde FIDO2 anahtarının içinde Windows 10 sürüm 1809 veya üzeri Microsoft Edge tarayıcı ile oturum açmak kullanıcı seçebilir.

![Microsoft edge'de anahtar oturum güvenliği](./media/howto-authentication-passwordless-enable/fido2-windows-10-1903-edge-sign-in.png)

Microsoft Authenticator uygulamasını kullanarak imzalama hakkında bilgi için bkz [hesaplarınız için Microsoft Authenticator uygulamasını kullanarak oturum açın](../user-help/user-help-auth-app-sign-in.md).

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="fido2-security-keys"></a>FIDO2 güvenlik anahtarları

#### <a name="security-key-provisioning"></a>Güvenlik anahtar sağlama

Hazırlama ve sağlamayı güvenlik anahtarları Yöneticisi genel Önizleme sürümünde kullanılabilir değil.

#### <a name="hybrid-azure-ad-join"></a>Hibrit Azure AD'ye katılım

Yönetilen kimlik bilgilerini kullanın WIA SSO üzerinde bağlı olan kullanıcıların gibi FIDO2 güvenlik tuşları veya Microsoft Authenticator uygulamasını parolasız oturum SSO avantajlarından yararlanın karma katılın Windows 10 için gerekiyor. Ancak, Azure Active Directory'ye katılmış makineler için yalnızca iş için artık güvenlik anahtarları. Yalnızca saf Azure Active Directory'ye katılmış makinelerde Windows kilit ekranı FIDO2 güvenlik tuşları denemek öneririz. Bu sınırlama, web için geçerli değildir.

#### <a name="upn-changes"></a>UPN değişiklikleri

Karma AADJ UPN değişiklik ve AADJ cihazları sağlayan bir özelliği destekleme üzerinde çalışıyoruz. Bir kullanıcının UPN'sini değişirse, artık FIDO2 güvenlik anahtarları için hesap için değiştirebilirsiniz. Bu nedenle yalnızca Cihazınızı sıfırladığınızda yaklaşımdır ve kullanıcının yeniden kaydetmeniz gerekir.

### <a name="authenticator-app"></a>Authenticator uygulaması

#### <a name="ad-fs-integration"></a>AD FS tümleştirmesi

Bir kullanıcı Microsoft Authenticator parolasız kimlik bilgisi etkin olduğundan, bu kullanıcı için kimlik doğrulaması her zaman bir onay bildirimi göndermeyi varsayılan olacaktır. Bu mantık için ADFS oturum açma doğrulamanızı için ek bir adım kullanıcıya sorulmadan "Bunun yerine parolanızı kullanın."'ye yönlendirilmesini karma kiracısındaki kullanıcılar engeller. Bu işlem, ayrıca tüm şirket içi koşullu erişim ilkeleri ve geçişli kimlik doğrulaması akışları atlayacaktır. Bu işlem bir login_hint ise, belirtilen bir kullanıcı AD FS'ye autoforwarded ve parolasız kimlik bilgilerini kullanma seçeneğini atlama istisnadır.

#### <a name="azure-mfa-server"></a>Azure MFA sunucusu

Bir kuruluşun şirket içi Azure MFA sunucusu ile MFA için etkinleştirilen son kullanıcılar yine de oluşturabilir ve tek parolasız telefon oturum kimlik bilgisini kullanın. Bu değişiklik, kullanıcı kimlik bilgileriyle Microsoft Authenticator'ın birden çok yükleme (5 +) yükseltmek çalışırsa, bir hataya neden olabilir.  

#### <a name="device-registration"></a>Cihaz kaydı

Bu yeni ve güçlü kimlik bilgisi oluşturmak için gereken önkoşullar biri olan tek bir kullanıcıya Azure AD kiracısı içinde bulunduğu cihaz kaydedilir. Cihaz kayıt kısıtlamaları nedeniyle, bir cihazın yalnızca tek bir kiracıda kaydedilebilir. Bu sınır, Microsoft Authenticator uygulamasını tek bir iş veya Okul hesabı telefon oturum açma için etkinleştirilebilir anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

[Cihaz kaydı hakkında bilgi edinin](../devices/overview.md)

[Azure multi-Factor Authentication hakkında bilgi edinin](../authentication/howto-mfa-getstarted.md)
