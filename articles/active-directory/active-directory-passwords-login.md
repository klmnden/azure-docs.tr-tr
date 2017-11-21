---
title: "Windows 10 oturum açma ekranından Azure AD SSPR | Microsoft Docs"
description: "Windows 10 oturum açma ekranını Azure AD parola sıfırlama ve PIN'imi unuttum istemleri için yapılandırma"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/08/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 9f7fdb97fd121eecf9e7b2f4edc1b568c8114869
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="azure-ad-password-reset-from-the-login-screen"></a>Oturum açma ekranından Azure AD parola sıfırlama

Azure AD self servis parola sıfırlamayı (SSPR) zaten dağıtmıştınız ancak kullanıcılarınız parolalarını unuttuklarında yardım masasını aramaya devam ediyorlar. Yardım masasını arıyorlar çünkü SSPR'ye erişmek için web tarayıcısına ulaşamıyorlar.

Yeni Windows 10 Fall Creators Update ile, cihazları Azure AD'ye katılmış olan kullanıcılar oturum açma ekranlarında “Parolayı sıfırla” bağlantısını görebilirler. Bu bağlantıya tıkladıklarında, bildikleri self servis parola sıfırlama (SSPR) deneyimine ulaşırlar 

Kullanıcıların Windows 10 oturum açma ekranından Azure AD parolalarını sıfırlamalarına olanak tanımak için, aşağıdaki gereksinimler karşılanmalıdır:

* Azure AD Etki Alanına katılmış, Windows 10 sürüm 1709 veya daha yeni bir istemci.
* Azure AD self servis parola sıfırlama etkinleştirilmelidir.
* Parolayı sıfırla bağlantısını etkinleştirmek için aşağıdaki yöntemlerden birini kullanarak ayarı yapılandırın ve dağıtın:
   * [Intune cihaz yapılandırma profili](active-directory-passwords-login.md#configure-reset-password-link-using-intune)
   * [Kayıt defteri anahtarı](active-directory-passwords-login.md#configure-reset-password-link-using-the-registry)

## <a name="configure-reset-password-link-using-intune"></a>Intune'u kullanarak Parolayı sıfırla bağlantısını yapılandırma

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

1. [Azure Portal](https://portal.azure.com)'da oturum açın ve **Azure Active Directory**'ye tıklayın.
2. **Kullanıcılar ve gruplar** > **Tüm gruplar** > **Yeni grup** öğesine gidin
3. Grup için bir ad girin ve **Üyelik türü**'nün altında **Atandı**'yı seçin 
   * **Üyeler**'in altında, ilkeyi uygulamak istediğiniz, Azure AD'ye katılmış Windows 10 cihazlarını seçin.
   * **Seç**'e tıklayın
4. **Oluştur**'a tıklayın

[Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md) makalesinde, grup oluşturma hakkında daha fazla bilgi bulabilirsiniz.

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

1. Yönetici kimlik bilgilerini kullanarak Azure AD Etki Alanına katılmış cihazda oturum açın
2. Yönetici olarak **regedit** komutunu çalıştırın
3. Aşağıdaki kayıt defteri anahtarını ayarlayın
   * `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      * `"AllowPasswordReset"=dword:00000001`

## <a name="what-do-users-see"></a>Kullanıcıların ne görecek

Artık ilke yapılandırıldığı ve atandığına göre, kullanıcı açısından değişen ne olur? Oturum açma ekranında parolalarını sıfırlayabileceklerini nasıl anlayacaklar?

![Oturum Açma Ekranı][LoginScreen]

Kullanıcılar oturum açmayı denediklerinde, artık oturum açma ekranında self servis parola sıfırlama deneyimini açan bir Parolayı sıfırla bağlantısı görürler. Bu işlev kullanıcıların web tarayıcısına erişmek için başka bir cihaz kullanmalarına gerek kalmadan parolalarını sıfırlamalarına olanak tanır.

Kullanıcılarınız bu özelliği kullanma yönergelerini [İş veya okul parolanızı sıfırlama](active-directory-passwords-update-your-own-password.md#reset-password-at-login) konusunda bulabilirler

## <a name="common-issues"></a>Genel sorunlar

Hyper-V kullanarak bu işlevi test ederken, "Parolayı sıfırla" bağlantısı gösterilmiyor.

* Test etmek için kullandığınız sanal makineye gidin, **Görünüm**'e tıklayın ve **Gelişmiş oturum**'un işaretini kaldırın.

Uzak Masaüstü kullanarak bu işlevi test ederken, "Parolayı sıfırla" bağlantısı gösterilmiyor

* Şu anda Uzak Masaüstü'nden parola sıfırlama desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantılar, Azure AD kullanarak parola sıfırlama ile ilgili ek bilgiler sağlar

* [SSPR'yi nasıl dağıtabilirim?](active-directory-passwords-best-practices.md)
* [Oturum açma ekranından PIN sıfırlamayı nasıl etkinleştirebilirim?](https://docs.microsoft.com/intune/device-windows-pin-reset)
* [MDM kimlik doğrulama ilkeleri hakkında daha fazla bilgi](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-authentication)

[CreateProfile]: ./media/active-directory-passwords-login/create-profile.png "Windows 10 oturum açma ekranında Parolayı sıfırla bağlantısını etkinleştirmek için Intune cihaz yapılandırma profili oluşturma"
[Assignment]: ./media/active-directory-passwords-login/profile-assignment.png "Bir grup Windows 10 cihazına Intune cihaz yapılandırma ilkesi atama"
[LoginScreen]: ./media/active-directory-passwords-login/logon-reset-password.png "Windows 10 oturum açma ekranında Parolayı sıfırla bağlantısı"
