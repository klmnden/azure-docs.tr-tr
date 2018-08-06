---
title: Windows 10 oturum açma ekranından Azure AD SSPR
description: Bu öğreticide yardım masası çağrılarının azaltılması için Windows 10 oturum açma ekranında parola sıfırlamayı etkinleştireceksiniz.
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: eefb07136215d79b7c351dd4498bfeb79b6833de
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413775"
---
# <a name="tutorial-azure-ad-password-reset-from-the-login-screen"></a>Öğretici: Oturum açma ekranından Azure AD parola sıfırlama

Bu öğreticide kullanıcıların parolalarını Windows 10 oturum açma ekranından sıfırlamasını sağlayacaksınız. Yeni Windows 10 Nisan 2018 Güncelleştirmesi ile, cihazları **Azure AD'ye katılmış** veya **hibrit Azure AD’ye katılmış** olan kullanıcılar oturum açma ekranlarında “Parolayı sıfırla” bağlantısını kullanabilirler. Kullanıcılar bu bağlantıya tıkladıklarında, bildikleri self servis parola sıfırlama (SSPR) deneyimine ulaşırlar.

> [!div class="checklist"]
> * Intune'u kullanarak Parolayı sıfırla bağlantısını yapılandırma
> * Windows Kayıt Defteri ile isteğe bağlı yapılandırma
> * Kullanıcılarınızın göreceği seçenekleri anlama

## <a name="prerequisites"></a>Ön koşullar

* Windows 10 Nisan 2018 Güncelleştirmesi veya aşağıdaki özelliklere sahip daha yeni bir istemci:
   * [Azure AD'ye katılmış](../device-management-azure-portal.md) veya 
   * [Hibrit Azure AD'ye katılmış](../device-management-hybrid-azuread-joined-devices-setup.md)
* Azure AD self servis parola sıfırlama etkinleştirilmelidir.

## <a name="configure-reset-password-link-using-intune"></a>Intune'u kullanarak Parolayı sıfırla bağlantısını yapılandırma

Oturum açma ekranından parola sıfırlama yapılmasını sağlayan yapılandırma değişikliğinin Intune'dan dağıtılması en esnek yöntemdir. Intune, yapılandırma değişikliğini tanımladığınız belirli bir makine grubuna dağıtmanızı sağlar. Bu yöntem, cihazın Intune kaydını gerektirir.

### <a name="create-a-device-configuration-policy-in-intune"></a>Intune'da cihaz yapılandırma ilkesi oluşturma

1. [Azure Portal](https://portal.azure.com)'da oturum açın ve **Intune**'a tıklayın.
2. **Cihaz yapılandırması** > **Profiller** > **Profil Oluştur**'a giderek yeni bir cihaz yapılandırma profili oluşturun
   * Profil için anlamlı bir ad girin
   * İsteğe bağlı olarak, profil için anlamlı bir açıklama girin
   * Platform **Windows 10 ve üstü**
   * Profil türü **Özel**

   ![CreateProfile][CreateProfile]

3. **Ayarlar**'ı yapılandırın
   * Parolayı sıfırla bağlantısını etkinleştirmek için şu OMA-URI Ayarını **ekleyin**
      * Ayarın ne yaptığını açıklayan anlamlı bir ad girin
      * İsteğe bağlı olarak, ayar için anlamlı bir açıklama girin
      * **OMA-URI** olarak `./Vendor/MSFT/Policy/Config/Authentication/AllowAadPasswordReset` ayarlayın
      * **Veri türü** olarak **Tamsayı** ayarlayın
      * **Değer** olarak **1** ayarlayın
      * **Tamam**’a tıklayın.
   * **Tamam**’a tıklayın.
4. **Oluştur**'a tıklayın

### <a name="assign-a-device-configuration-policy-in-intune"></a>Intune'da cihaz yapılandırma ilkesini atama

#### <a name="create-a-group-to-apply-device-configuration-policy-to"></a>Cihaz yapılandırma ilkesinin uygulanacağı grubu oluşturma

1. [Azure portal](https://portal.azure.com)'da oturum açın ve **Azure Active Directory**'ye tıklayın.
2. **Kullanıcılar ve gruplar** > **Tüm gruplar** > **Yeni grup** öğesine gidin
3. Grup için bir ad girin ve **Üyelik türü**'nün altında **Atandı**'yı seçin
   * **Üyeler**'in altında, ilkeyi uygulamak istediğiniz, Azure AD'ye katılmış Windows 10 cihazlarını seçin.
   * **Seç**'e tıklayın
4. **Oluştur**'a tıklayın

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

Bu yöntemi yalnızca ayar değişikliğini test etmek için kullanmanızı öneririz.

1. Yönetici kimlik bilgilerini kullanarak Windows bilgisayarda oturum açın
2. Yönetici olarak **regedit** komutunu çalıştırın
3. Aşağıdaki kayıt defteri anahtarını ayarlayın
   * `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      * `"AllowPasswordReset"=dword:00000001`

## <a name="what-do-users-see"></a>Kullanıcıların ne görecek

Artık ilke yapılandırıldığı ve atandığına göre, kullanıcı açısından değişen ne olur? Oturum açma ekranında parolalarını sıfırlayabileceklerini nasıl anlayacaklar?

![Oturum Açma Ekranı][LoginScreen]

Kullanıcılar oturum açmayı denediklerinde, artık oturum açma ekranında self servis parola sıfırlama deneyimini açan bir Parolayı sıfırla bağlantısı görürler. Bu işlev kullanıcıların web tarayıcısına erişmek için başka bir cihaz kullanmalarına gerek kalmadan parolalarını sıfırlamalarına olanak tanır.
Kullanıcılar oturum açmayı denediklerinde, artık oturum açma ekranında self servis parola sıfırlama deneyimini açan bir Parolayı sıfırla bağlantısı görürler. Bu işlev kullanıcıların web tarayıcısına erişmek için başka bir cihaz kullanmalarına gerek kalmadan parolalarını sıfırlamalarına olanak tanır.

Kullanıcılarınız bu özelliği kullanma yönergelerini [İş veya okul parolanızı sıfırlama](../user-help/active-directory-passwords-update-your-own-password.md#reset-password-at-sign-in) konusunda bulabilirler

## <a name="common-issues"></a>Genel sorunlar

Hyper-V kullanarak bu işlevi test ederken, "Parolayı sıfırla" bağlantısı gösterilmiyor.

* Test etmek için kullandığınız sanal makineye gidin, **Görünüm**'e tıklayın ve **Gelişmiş oturum**'un işaretini kaldırın.

Uzak Masaüstü kullanarak bu işlevi test ederken, "Parolayı sıfırla" bağlantısı gösterilmiyor.

* Şu anda Uzak Masaüstü'nden parola sıfırlama desteklenmiyor.

Windows kilit ekranı kayıt defteri anahtarı veya grup ilkesi ile devre dışı bırakılırsa **Parolayı sıfırla** özelliği kullanılamaz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide yapılandırdığınız işlevleri kullanmak istemediğinize karar verirseniz oluşturduğunuz Intune cihaz yapılandırma profilini veya kayıt defteri anahtarını silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide kullanıcıların parolalarını Windows 10 oturum açma ekranından sıfırlamasını sağladınız. Azure Kimlik Koruması özelliklerinin self servis parola sıfırlama ve Multi-Factor Authentication deneyimleriyle nasıl tümleştirileceğini görmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Oturum açma sırasında risk değerlendirmesi yapma](tutorial-risk-based-sspr-mfa.md)

[CreateProfile]: ./media/tutorial-sspr-windows/create-profile.png "Windows 10 oturum açma ekranında Parolayı sıfırla bağlantısını etkinleştirmek için Intune cihaz yapılandırma profili oluşturma"
[Assignment]: ./media/tutorial-sspr-windows/profile-assignment.png "Bir grup Windows 10 cihazına Intune cihaz yapılandırma ilkesi atama"
[LoginScreen]: ./media/tutorial-sspr-windows/logon-reset-password.png "Windows 10 oturum açma ekranında Parolayı sıfırla bağlantısı"
