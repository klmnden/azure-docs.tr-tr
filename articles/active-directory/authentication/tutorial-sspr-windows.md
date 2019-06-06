---
title: Windows 10 oturum açma ekranından Azure AD SSPR
description: Bu öğreticide yardım masası çağrılarının azaltılması için Windows 10 oturum açma ekranında parola sıfırlamayı etkinleştireceksiniz.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2a4bdaba45c466b7f1f6fb8e91033f9a7665e034
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730541"
---
# <a name="tutorial-azure-ad-password-reset-from-the-login-screen"></a>Öğretici: Oturum açma ekranından Azure AD parola sıfırlama

Bu öğreticide kullanıcıların parolalarını Windows 10 oturum açma ekranından sıfırlamasını sağlayacaksınız. Yeni Windows 10 Nisan 2018 Güncelleştirmesi ile, cihazları **Azure AD'ye katılmış** veya **hibrit Azure AD’ye katılmış** olan kullanıcılar oturum açma ekranlarında “Parolayı sıfırla” bağlantısını kullanabilirler. Kullanıcılar bu bağlantıya tıkladıklarında, bildikleri self servis parola sıfırlama (SSPR) deneyimine ulaşırlar. Bir kullanıcıya kilitlenmişse bu işlem şirket içi Active Directory'de hesapları kilidini değil.

> [!div class="checklist"]
> * Intune'u kullanarak Parolayı sıfırla bağlantısını yapılandırma
> * Windows Kayıt Defteri ile isteğe bağlı yapılandırma
> * Kullanıcılarınızın göreceği seçenekleri anlama

## <a name="prerequisites"></a>Önkoşullar

* En az çalıştırılması gereken Windows 10, sürüm Nisan 2018 Güncelleştirmesi (v1803) ve cihazları aşağıdakilerden biri olması gerekir:
   * [Azure AD'ye katılmış](../device-management-azure-portal.md) veya
   * [Hibrit Azure AD'ye katılmış](../device-management-hybrid-azuread-joined-devices-setup.md), bir etki alanı denetleyicisine ağ bağlantısı ile.
* Etkinleştirmeniz Azure AD Self Servis parola sıfırlama.
* Windows 10 cihazlarınızı bir proxy sunucusu veya güvenlik duvarı ise URL'leri eklemelisiniz `passwordreset.microsoftonline.com` ve `ajax.aspnetcdn.com` izin, HTTPS trafiği (bağlantı noktası 443) için URL'lerin listesi.
* Windows 10 için SSPR yalnızca makine düzeyinde proxy'leriyle desteklenir
* Aşağıdaki sınırlamalar, bu özellik ortamınızdaki denemeden önce gözden geçirin.
* Görüntü kullanıyorsanız, önce sysprep web önbelleği CopyProfile adımı gerçekleştirmeden önce yerleşik yönetici için temizlenmiş olduğundan emin olun. Bu konu hakkında daha fazla bilgi destek makalesinde bulunabilir [özel varsayılan kullanıcı profilini kullanırken performans düşük](https://support.microsoft.com/help/4056823/performance-issue-with-custom-default-user-profile).

## <a name="configure-reset-password-link-using-intune"></a>Intune'u kullanarak Parolayı sıfırla bağlantısını yapılandırma

Oturum açma ekranından parola sıfırlama yapılmasını sağlayan yapılandırma değişikliğinin Intune'dan dağıtılması en esnek yöntemdir. Intune, yapılandırma değişikliğini tanımladığınız belirli bir makine grubuna dağıtmanızı sağlar. Bu yöntem, cihazın Intune kaydını gerektirir.

### <a name="create-a-device-configuration-policy-in-intune"></a>Intune'da cihaz yapılandırma ilkesi oluşturma

1. [Azure Portal](https://portal.azure.com)'da oturum açın ve **Intune**'a tıklayın.
2. **Cihaz yapılandırması** > **Profiller** > **Profil Oluştur**'a giderek yeni bir cihaz yapılandırma profili oluşturun
   * Profil için anlamlı bir ad girin
   * İsteğe bağlı olarak, profil için anlamlı bir açıklama girin
   * Platform **Windows 10 ve üstü**
   * Profil türü **Özel**

3. **Ayarlar**'ı yapılandırın
   * Parolayı sıfırla bağlantısını etkinleştirmek için şu OMA-URI Ayarını **ekleyin**
      * Ayarın ne yaptığını açıklayan anlamlı bir ad girin
      * İsteğe bağlı olarak, ayar için anlamlı bir açıklama girin
      * **OMA-URI** olarak `./Vendor/MSFT/Policy/Config/Authentication/AllowAadPasswordReset` ayarlayın
      * **Veri türü** olarak **Tamsayı** ayarlayın
      * **Değer** olarak **1** ayarlayın
      * **Tamam**’a tıklayın.
   * **Tamam**’a tıklayın.
4. **Oluştur**'a tıklayın.

### <a name="assign-a-device-configuration-policy-in-intune"></a>Intune'da cihaz yapılandırma ilkesini atama

#### <a name="create-a-group-to-apply-device-configuration-policy-to"></a>Cihaz yapılandırma ilkesinin uygulanacağı grubu oluşturma

1. [Azure portal](https://portal.azure.com)'da oturum açın ve **Azure Active Directory**'ye tıklayın.
2. **Kullanıcılar ve gruplar** > **Tüm gruplar** > **Yeni grup** öğesine gidin
3. Grup için bir ad girin ve **Üyelik türü**'nün altında **Atandı**'yı seçin
   * **Üyeler**'in altında, ilkeyi uygulamak istediğiniz, Azure AD'ye katılmış Windows 10 cihazlarını seçin.
   * **Seç**'e tıklayın
4. **Oluştur**'a tıklayın.

[Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md) makalesinde, grup oluşturma hakkında daha fazla bilgi bulabilirsiniz.

#### <a name="assign-device-configuration-policy-to-device-group"></a>Cihaz grubuna cihaz yapılandırma ilkesi atama

1. [Azure Portal](https://portal.azure.com)'da oturum açın ve **Intune**'a tıklayın.
2. **Cihaz yapılandırması** > **Profiller**'e gidip daha önce oluşturulmuş olan cihaz yapılandırma profilini bulun ve bu profile tıklayın
3. Cihaz grubuna profili atama 
   * **Atamalar** > **Ekle** > **Eklenecek grupları seçin**'e tıklayın
   * Daha önce oluşturulmuş olan grubu seçin ve **Seç**'e tıklayın
   * **Kaydet**'e tıklayın

   ![Atama][Assignment]

Artık Intune kullanarak Parolayı sıfırla bağlantısını etkinleştirmek için bir cihaz yapılandırma ilkesi oluşturduğunuz ve atadınız.

## <a name="configure-reset-password-link-using-the-registry"></a>Kayıt defterini kullanarak Parolayı sıfırla bağlantısını yapılandırma

1. Windows bilgisayarı yönetim kimlik bilgilerini kullanarak oturum açın
2. Yönetici olarak **regedit** komutunu çalıştırın
3. Aşağıdaki kayıt defteri anahtarını ayarlayın
   * `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      * `"AllowPasswordReset"=dword:00000001`

## <a name="what-do-users-see"></a>Kullanıcıların ne görecek

Artık ilke yapılandırıldığı ve atandığına göre, kullanıcı açısından değişen ne olur? Oturum açma ekranında parolalarını sıfırlayabileceklerini nasıl anlayacaklar?

![Oturum Açma Ekranı][LoginScreen]

Kullanıcılar oturum açmaya çalıştığında deneyimi oturum açma ekranında Self Servis parola açan bir parolayı Sıfırla bağlantısını sıfırlama artık görürler. Bu işlev kullanıcıların web tarayıcısına erişmek için başka bir cihaz kullanmalarına gerek kalmadan parolalarını sıfırlamalarına olanak tanır.

Kullanıcılarınız bu özelliği kullanma yönergelerini [İş veya okul parolanızı sıfırlama](../user-help/active-directory-passwords-update-your-own-password.md#reset-password-at-sign-in) konusunda bulabilirler

Azure AD denetim günlüğü parola sıfırlamanın oluştuğu yerin IP adresi ve ClientType'ı hakkında bilgi içerir.

![Azure AD denetim günlüğünde oturum açma ekranı parola sıfırlaması örneği](media/tutorial-sspr-windows/windows-sspr-azure-ad-audit-log.png)

Kullanıcıların Windows 10 cihazının oturum açma ekranından parolalarını sıfırlama, "defaultuser1" adlı geçici düşük ayrıcalıklı hesap oluşturulur. Bu hesap, parola sıfırlama işlemi güvenli tutmak için kullanılır. Hesap rastgele oluşturulmuş bir parolası olup, cihaz oturum açma için göstermez ve kullanıcının parolasını sıfırlandıktan sonra otomatik olarak kaldırılacak. Birden çok "defaultuser" profili mevcut olabilir, ancak güvenle yoksayılabilir.

## <a name="limitations"></a>Sınırlamalar

Hesabının kilidini açmak, mobil uygulama bildirimi ve mobil uygulama kodu, SSPR Windows 10 için tarafından desteklenmez.

Hyper-V kullanarak bu işlevi test ederken, "Parolayı sıfırla" bağlantısı gösterilmiyor.

* Test etmek için kullandığınız sanal makineye gidin, **Görünüm**'e tıklayın ve **Gelişmiş oturum**'un işaretini kaldırın.

Uzak Masaüstü veya bir Gelişmiş VM oturumu kullanarak bu işlevi test ederken, "parolayı Sıfırla" bağlantısı görünmüyor.

* Şu anda Uzak Masaüstü'nden parola sıfırlama desteklenmiyor.

Windows 10 sürümleri v1809 önce İlkesi tarafından Ctrl + Alt + Del gerekiyorsa **parolayı Sıfırla** çalışmaz.

Kilitleme ekranı bildirimleri devre dışı ise **parolayı Sıfırla** çalışmaz.

Aşağıdaki ilke ayarlarını parolalarını sıfırlama olanağı müdahale bilinen

   * Etkin şekilde veya 1 HideFastUserSwitching olan
   * Etkin şekilde veya 1 DontDisplayLastUserName olan
   * Etkin şekilde veya 1 NoLockScreen olan
   * Cihazda EnableLostMode ayarlanır
   * Explorer.exe özel bir kabuk ile değiştirilir

Bu özellik, dağıtılan 802.1 x ağ kimlik doğrulaması ağlarla ve "Kullanıcı oturum açma işleminden hemen önce gerçekleştir" seçeneği için çalışmaz. 802.1 x ağ kimlik doğrulaması dağıtmış olan ağlar için bu özelliği etkinleştirmek için makine kimlik doğrulaması kullanmak için önerilir.

Karma etki alanına katılmış senaryoları için bir Active Directory etki alanı denetleyicisi gerek kalmadan SSPR iş akışı başarıyla tamamlanır. Bir Active Directory etki alanı denetleyicisi ile iletişim gibi uzaktan çalışma, mevcut olmadığında bir kullanıcı parola sıfırlama işlemi tamamlarsa, kullanıcı cihazı bir etki alanı denetleyicisiyle iletişim kurabildiğini kadar cihaza oturum mümkün olmayacaktır ve önbelleğe alınmış kimlik bilgilerini güncelleştirin. **Bir etki alanı denetleyicisiyle bağlantı ilk kez yeni parolayı kullanmak için gerekli olduğunu**.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide yapılandırdığınız işlevleri kullanmak istemediğinize karar verirseniz oluşturduğunuz Intune cihaz yapılandırma profilini veya kayıt defteri anahtarını silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide kullanıcıların parolalarını Windows 10 oturum açma ekranından sıfırlamasını sağladınız. Azure Kimlik Koruması özelliklerinin self servis parola sıfırlama ve Multi-Factor Authentication deneyimleriyle nasıl tümleştirileceğini görmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Oturum açma sırasında risk değerlendirmesi yapma](tutorial-risk-based-sspr-mfa.md)

[Assignment]: ./media/tutorial-sspr-windows/profile-assignment.png "Bir grup Windows 10 cihazına Intune cihaz yapılandırma ilkesi atama"
[LoginScreen]: ./media/tutorial-sspr-windows/logon-reset-password.png "Windows 10 oturum açma ekranında Parolayı sıfırla bağlantısı"
