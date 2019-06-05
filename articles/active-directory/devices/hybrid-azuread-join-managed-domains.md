---
title: Yönetilen etki alanları için hibrit Azure Active Directory katılımını Yapılandır | Microsoft Docs
description: Hibrit Azure Active Directory join yönetilen etki alanları için yapılandırmayı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: efa653ecf306f5ac5eefaddd61d98e81f919876d
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66513297"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-managed-domains"></a>Öğretici: Yönetilen etki alanları için hibrit Azure Active Directory katılımını Yapılandır

Benzer şekilde bir kullanıcıya, bir cihaz korumak ve ayrıca istediğiniz zaman ve herhangi bir konumdan kaynaklarınızı korumak için kullanmak istediğiniz başka bir çekirdek kimliğidir. Bu hedefe getiren ve aşağıdaki yöntemlerden birini kullanarak Azure AD'de cihaz kimliklerini yönetme görevleri gerçekleştirebilirsiniz:

- Azure AD'ye katılım
- Hibrit Azure AD'ye katılım
- Azure AD kaydı

Cihazlarınızı Azure AD'ye taşıyarak, çoklu oturum açma (SSO) özelliği sayesinde bulut ve şirket içi kaynaklarınız genelinde kullanıcılarınızın üretkenliğini en üst düzeye çıkarırsınız. Ayrıca, [koşullu erişim](../active-directory-conditional-access-azure-portal.md) ile bulut ve şirket içi kaynaklarınıza erişimin güvenliği sağlayabilirsiniz.

Bu öğreticide, yönetilen bir ortamda hibrit Azure AD'ye katılımı AD etki alanına katılmış bilgisayarları ve cihazları için yapılandırma konusunda bilgi edinin. 

Yönetilen bir ortam olabilir aracılığıyla dağıtılan [parola karması eşitleme (PHS)](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-phs) veya [geçirmek aracılığıyla kimlik doğrulaması (PTA)](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta) ile [sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso).
Bu senaryolar, bir federasyon sunucusu kimlik doğrulaması için yapılandırmak gerekli değildir.

> [!div class="checklist"]
> * Hibrit Azure AD'ye katılımı yapılandırma
> * Windows alt düzey cihazlarını etkinleştirme
> * Katılmış cihazları doğrulama
> * Sorun giderme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, şu konularda bilgi sahibi olduğunuz varsayılır:

- [Azure Active Directory'de cihaz kimlik yönetimine giriş](../device-management-introduction.md)
- [Hibrit Azure Active Directory'ye katılma uygulamanızı planlama](hybrid-azuread-join-plan.md)
- [Hibrit Azure AD'ye katılma denetimli doğrulama yapma](hybrid-azuread-join-control.md)

> [!NOTE]
> Azure AD, yönetilen etki alanlarında akıllı kartlar veya sertifikaları desteklemez.

Bu makaledeki senaryoyu yapılandırmak için, [en yeni Azure AD Connect sürümünün](https://www.microsoft.com/download/details.aspx?id=47594) (1.1.819.0 veya sonraki) yüklü olması gerekir.

Azure AD Connect'in hibrit Azure AD'ye katılmış olmasını istediğiniz cihazların bilgisayar nesnelerini Azure AD'ye eşitlediğini doğrulayın. Bilgisayar nesneleri belirli kuruluş birimlerine (OU) aitse bu kuruluş birimlerinin Azure AD Connect'te eşitleme için de yapılandırılmış olması gerekir. Azure AD Connect kullanarak bilgisayar nesneleri eşitleme hakkında daha fazla bilgi için makaleye bakın [Azure AD Connect kullanarak filtreleme yapılandırma](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-configure-filtering#organizational-unitbased-filtering).

1.1.819.0 sürümünden itibaren Azure AD Connect hibrit Azure AD'ye katılımı yapılandırmak için bir sihirbaz sağlar. Sihirbaz, yapılandırma işlemini önemli ölçüde basitleştirebilmenizi sağlar. İlgili sihirbaz, cihaz kaydı için hizmet bağlantı noktalarını (SCP) yapılandırır.

Bu makaledeki yapılandırma adımları, bu sihirbazı temel alır.

Hibrit Azure AD'ye katılım cihazların kuruluşunuzun ağındaki şu Microsoft kaynaklarına erişim sağlamasını gerektirir:  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com` (Sorunsuz SSO kullanıyorsanız veya kullanmayı planlıyorsanız)

Microsoft, kuruluşunuzun bir giden proxy üzerinden Internet erişimi gerektirip gerektirmediğini önerir [Web Proxy Otomatik Bulma (WPAD) uygulama](https://docs.microsoft.com/previous-versions/tn-archive/cc995261(v%3dtechnet.10)) ile Azure AD cihaz kaydı yapmak Windows 10 bilgisayarlarını etkinleştirmek için. [Otomatik algılama sorunlarını gidermek için] yapılandırma ve WPAD yönetme konusunda sorun yaşıyorsanız, Git (https://docs.microsoft.com/previous-versions/tn-archive/cc302643(v=technet.10). 

WPAD kullanmıyorsanız ve bilgisayarınızda proxy ayarlarını yapılandırmanız gerekir, böylece ile Windows 10 1709 göre başlangıç yapabilirsiniz [bir Grup İlkesi nesnesi (GPO) kullanarak WinHTTP ayarlarının yapılandırılması](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/).

> [!NOTE]
> WinHTTP ayarlarını kullanarak bilgisayarınızda proxy ayarlarını yapılandırın, yapılandırılan Ara sunucuya bağlanamıyorsanız tüm bilgisayarlar İnternet'e başarısız olur.

Kuruluşunuz, kimliği doğrulanmış bir giden bağlantı proxy'si aracılığıyla İnternete erişimi gerektiriyorsa Windows 10 bilgisayarlarınızın giden bağlantı proxy'sine başarıyla kimlik doğrulayabildiğinden emin olmanız gerekir. Windows 10 bilgisayarlar makine bağlamını kullanarak cihaz kaydını çalıştırdığından makine bağlamı ile giden bağlantı proxy'sinin yapılandırılması gerekir. Yapılandırma gereksinimleri için giden bağlantı proxy'si sağlayıcınızı izleyin.

## <a name="configure-hybrid-azure-ad-join"></a>Hibrit Azure AD'ye katılımı yapılandırma

Azure AD Connect kullanarak bir hibrit Azure AD'ye katılımı yapılandırmak için şunlar gereklidir:

- Azure AD kiracınız için genel bir yöneticinin kimlik bilgileri.  
- Her bir orman için kuruluş yöneticisinin kimlik bilgileri.

**Azure AD Connect kullanarak bir hibrit Azure AD'ye katılımı yapılandırmak için:**

1. Azure AD Connect'i başlatın ve ardından **Yapılandır** seçeneğine tıklayın.

   ![Hoş Geldiniz](./media/hybrid-azuread-join-managed-domains/11.png)

1. **Ek görevler** sayfasında **Cihaz seçeneklerini yapılandır** öğesini seçin ve ardından **İleri** seçeneğine tıklayın.

   ![Ek görevler](./media/hybrid-azuread-join-managed-domains/12.png)

1. **Genel Bakış** sayfasında **İleri** seçeneğine tıklayın.

   ![Genel Bakış](./media/hybrid-azuread-join-managed-domains/13.png)

1. **Azure AD'ye Bağlanma** sayfasında Azure AD kiracınızın genel yöneticisinin kimlik bilgilerini girin.  

   ![Azure AD'ye Bağlanma](./media/hybrid-azuread-join-managed-domains/14.png)

1. **Cihaz seçenekleri** sayfasında **Hibrit Azure AD'ye katılımı yapılandır** öğesini seçin ve ardından **İleri** seçeneğine tıklayın.

   ![Cihaz seçenekleri](./media/hybrid-azuread-join-managed-domains/15.png)

1. **SCP** sayfasında, Azure AD Connect'in SCP'yi yapılandırmasını istediğiniz her bir orman için aşağıdaki adımları uygulayın ve ardından **İleri** seçeneğine tıklayın:

   ![SCP](./media/hybrid-azuread-join-managed-domains/16.png)

   1. Ormanı seçin.
   1. Kimlik doğrulama hizmetini seçin.
   1. Kuruluş yöneticisinin kimlik bilgilerini girmek için **Ekle** seçeneğine tıklayın.

1. **Cihaz işletim sistemleri** sayfasında Active Directory ortamınızdaki cihazlar tarafından kullanılan işletim sistemlerini seçin ve ardından **İleri** seçeneğine tıklayın.

   ![Cihaz işletim sistemi](./media/hybrid-azuread-join-managed-domains/17.png)

1. **Yapılandırma için hazır** sayfasında **Yapılandır** seçeneğine tıklayın.

   ![Yapılandırma için hazır](./media/hybrid-azuread-join-managed-domains/19.png)

1. **Yapılandırma tamamlandı** sayfasında **Çıkış** seçeneğine tıklayın.

   ![Yapılandırma tamamlandı](./media/hybrid-azuread-join-managed-domains/20.png)

## <a name="enable-windows-down-level-devices"></a>Windows alt düzey cihazlarını etkinleştirme

Bazı etki alanına katılmış cihazlar Windows alt düzey cihazlarıysa şunları gerçekleştirmeniz gerekir:

- Cihaz kaydı için yerel intranet ayarlarını yapılandırma
- Sorunsuz çoklu oturum açma (SSO) yapılandırma
- Yükleme için Microsoft çalışma Windows katılın alt düzey bilgisayarlar

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Cihaz kaydı için yerel intranet ayarlarını yapılandırma

Windows aşağı düzey cihazlarınızın karma Azure AD'ye katılmasını başarıyla tamamlamak ve cihazlar Azure AD'de kimlik doğrularken sertifika istemlerini atlamak için etki alanına katılmış cihazlarınıza Internet Explorer'da Yerel Intranet bölgesine aşağıdaki URL'leri eklemek üzere bir ilke gönderebilirsiniz:

- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com`

Ayrıca, kullanıcının yerel intranet bölgesinde **Betik yoluyla durum çubuğu güncelleştirmelerine izin ver** seçeneğini etkinleştirmeniz gerekir.

### <a name="configure-seamless-sso"></a>Sorunsuz çoklu oturum açmayı yapılandırın

Windows alt düzey cihazlarınızın kullanan bir yönetilen etki alanında başarıyla tamamlanması hibrit Azure AD'ye katılma [parola karması eşitleme (PHS)](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-phs) veya [geçirmek aracılığıyla kimlik doğrulaması (PTA)](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta) olarak Azure AD bulut kimlik doğrulama yöntemi, şunları da yapmanız gerekir [sorunsuz çoklu oturum açmayı yapılandırma](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso-quick-start#step-2-enable-the-feature).

### <a name="install-microsoft-workplace-join-for-windows-down-level-computers"></a>Yükleme için Microsoft çalışma Windows katılın alt düzey bilgisayarlar

Windows alt düzey cihazları kaydetmek için kuruluşlar yüklemelisiniz [Microsoft Workplace Join Windows 10 bilgisayarlar için](https://www.microsoft.com/download/details.aspx?id=53554) Microsoft Download Center üzerinde kullanılabilir.

Gibi bir yazılım dağıtım sistemi kullanarak pakete dağıtabilirsiniz [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager). Paket sessiz parametresiyle standart sessiz yükleme seçeneklerini destekler. Geçerli dal Configuration Manager'ın önceki sürümlerinde, tamamlanan kayıtları izleme yeteneği gibi üzerinden avantaj sunar.

Yükleyici, kullanıcı bağlamında çalışacak sistemdeki zamanlanmış bir görev oluşturur. Windows için kullanıcının oturum açtığı zaman görevi tetiklenir. Görev, kullanıcı kimlik bilgileriyle Azure AD ile Azure AD kimliklerini doğruladıktan sonra cihazla sessizce birleştirir.

## <a name="verify-the-registration"></a>Kaydı doğrulama

Azure kiracınızda cihaz kaydı durumunu doğrulamak için, **[Azure Active Directory PowerShell modülünde](/powershell/azure/install-msonlinev1?view=azureadps-2.0)** **[Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice)** cmdlet öğesini kullanabilirsiniz.

Hizmet ayrıntılarını kontrol etmek için **Get-MSolDevice** cmdlet kullanırken:

- Bir nesne ile **cihaz kimliği** istemci bulunmalıdır Windows kimliği eşleşir.
- **DeviceTrustType** değerinin **Etki Alanına Katılmış** olması gerekir. Bu, Azure AD portalında Cihazlar sayfasındaki **Hibrit Azure AD'ye katılmış** durumuna eşdeğerdir.
- Koşullu erişimde kullanılan cihazlar için **Enabled** değerinin **True**, **DeviceTrustLevel** değerinin de **Managed** olması gerekir.

**Hizmet ayrıntılarını kontrol etmek için:**

1. Yönetici olarak **Windows PowerShell** açın.
1. Azure kiracınıza bağlanmak için `Connect-MsolService` yazın.  
1. `get-msoldevice -deviceId <deviceId>`yazın.
1. **Enabled** değerinin **True** olarak ayarlandığını doğrulayın.

## <a name="troubleshoot-your-implementation"></a>Uygulamanızda sorun giderme

Etki alanına katılmış Windows cihazları için hibrit Azure AD'ye katılımı tamamlama sırasında sorun yaşıyorsanız:

- [Windows geçerli cihazları için Hibrit Azure AD'ye katılım sorunlarını giderme](troubleshoot-hybrid-join-windows-current.md)
- [Windows alt düzey cihazları için Hibrit Azure AD'ye katılım sorunlarını giderme](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD portalında cihaz kimliklerini yönetme hakkında daha fazla bilgi için bkz. [Azure portalını kullanarak cihaz kimliklerini yönetme](device-management-azure-portal.md).
