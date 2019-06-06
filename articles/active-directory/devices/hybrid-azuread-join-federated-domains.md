---
title: Hibrit Azure Active Directory join Federasyon etki alanları için yapılandırma | Microsoft Docs
description: Hibrit Azure Active Directory join Federasyon etki alanları için yapılandırmayı öğrenin.
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
ms.openlocfilehash: 600d6b9f1eb8d8073e1658dd5b8196a3d8137e42
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66733712"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-federated-domains"></a>Öğretici: Federasyon etki alanları için hibrit Azure Active Directory katılımını Yapılandır

Benzer şekilde bir kullanıcı, bir cihaz korumak ve dilediğiniz zaman ve herhangi bir konumdan kaynaklarınızı korumak için kullanmak istediğiniz başka bir çekirdek kimliktir. Bu hedefe getiren ve aşağıdaki yöntemlerden birini kullanarak Azure AD'de cihaz kimliklerini yönetme görevleri gerçekleştirebilirsiniz:

- Azure AD'ye katılım
- Hibrit Azure AD'ye katılım
- Azure AD kaydı

Cihazlarınızı Azure AD'ye taşıyarak, çoklu oturum açma (SSO) özelliği sayesinde bulut ve şirket içi kaynaklarınız genelinde kullanıcılarınızın üretkenliğini en üst düzeye çıkarırsınız. Ayrıca, [koşullu erişim](../active-directory-conditional-access-azure-portal.md) ile bulut ve şirket içi kaynaklarınıza erişimin güvenliği sağlayabilirsiniz.

Bu öğreticide, AD FS kullanarak birleştirilmiş bir ortamda hibrit Azure AD'ye katılma AD etki alanına katılmış bilgisayarları ve cihazları için yapılandırma konusunda bilgi edinin.

> [!NOTE]
> Ortamınız federe bir kimlik sağlayıcısı dışında AD FS kullanıyorsanız, kimlik sağlayıcınız WS-Trust Protokolü desteklediğinden emin olmak gerekir. WS-Trust, Windows kimlik doğrulaması için gereken geçerli hibrit Azure AD'ye katılmış cihazların Azure AD ile. Ayrıca, alt düzey hibrit Azure AD'ye katılmasını sağlamaya gereksinim duyan cihazları Windows varsa, kimlik sağlayıcınız WIAORMULTIAUTHN iddianızın gerekir. 


> [!div class="checklist"]
> * Hibrit Azure AD'ye katılımı yapılandırma
> * Windows alt düzey cihazlarını etkinleştirme
> * Kaydı doğrulama
> * Sorun giderme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, şu konularda bilgi sahibi olduğunuz varsayılır:

- [Azure Active Directory'de cihaz kimlik yönetimine giriş](../device-management-introduction.md)
- [Hibrit Azure Active Directory'ye katılma uygulamanızı planlama](hybrid-azuread-join-plan.md)
- [Hibrit Azure AD'ye katılma denetimli doğrulama yapma](hybrid-azuread-join-control.md)

Bu öğreticide senaryoyu yapılandırmak için şunlar gereklidir:

- AD FS içeren Windows Server 2012 R2
- [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) 1.1.819.0 veya sonraki sürümü.

1.1.819.0 sürümünden itibaren Azure AD Connect hibrit Azure AD'ye katılımı yapılandırmak için bir sihirbaz sağlar. Sihirbaz, yapılandırma işlemini önemli ölçüde basitleştirebilmenizi sağlar. İlgili sihirbaz:

- Cihaz kaydı için hizmet bağlantı noktalarını (SCP) yapılandırır
- Mevcut Azure AD bağlı olan tarafınıza yedekleme yapar
- Azure AD Trust'ınızdaki talep kurallarını güncelleştirir

Bu makaledeki yapılandırma adımları, bu sihirbazı temel alır. Azure AD Connect'in daha önceki bir sürümü yüklüyse 1.1.819 veya sonraki bir sürüme yükseltmeniz gerekir. Azure AD Connect'in en son sürümünü yüklemek sizin için bir seçenek değilse, bkz. [hibrit Azure AD'ye katılma el ile yapılandırma](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-manual).

Hibrit Azure AD'ye katılım cihazların kuruluşunuzun ağındaki şu Microsoft kaynaklarına erişim sağlamasını gerektirir:  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- Kuruluşunuza ait STS (federasyon etki alanları)
- `https://autologon.microsoftazuread-sso.com` (Sorunsuz SSO kullanıyorsanız veya kullanmayı planlıyorsanız)

AD FS'yi kullanarak Federasyon ortam için anlık hibrit Azure AD'ye katılma başarısız olursa, Windows 10 1803'ile başlayarak, bilgisayar nesnesi sonradan için hibrit Azure AD'ye cihaz kaydı tamamlamak için kullanılan Azure ad eşitleme için Azure AD Connect'i bağımlı olduğumuz katılın. Azure AD Connect'in hibrit Azure AD'ye katılmış olmasını istediğiniz cihazların bilgisayar nesnelerini Azure AD'ye eşitlediğini doğrulayın. Bilgisayar nesneleri belirli kuruluş birimlerine (OU) aitse bu kuruluş birimlerinin Azure AD Connect'te eşitleme için de yapılandırılmış olması gerekir. Azure AD Connect kullanarak bilgisayar nesneleri eşitleme hakkında daha fazla bilgi için makaleye bakın [Azure AD Connect kullanarak filtreleme yapılandırma](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-configure-filtering#organizational-unitbased-filtering).

Microsoft, kuruluşunuzun bir giden proxy üzerinden Internet erişimi gerektirip gerektirmediğini önerir [Web Proxy Otomatik Bulma (WPAD) uygulama](https://docs.microsoft.com/previous-versions/tn-archive/cc995261(v%3dtechnet.10)) ile Azure AD cihaz kaydı yapmak Windows 10 bilgisayarlarını etkinleştirmek için. Yapılandırma ve WPAD yönetme konusunda sorun yaşıyorsanız, Git [otomatik algılama sorun giderme](https://docs.microsoft.com/previous-versions/tn-archive/cc302643(v=technet.10)). 

WPAD kullanmıyorsanız ve bilgisayarınızda proxy ayarlarını yapılandırmanız gerekir, böylece ile Windows 10 1709 göre başlangıç yapabilirsiniz [bir Grup İlkesi nesnesi (GPO) kullanarak WinHTTP ayarlarının yapılandırılması](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/).

> [!NOTE]
> WinHTTP ayarlarını kullanarak bilgisayarınızda proxy ayarlarını yapılandırın, yapılandırılan Ara sunucuya bağlanamıyorsanız tüm bilgisayarlar İnternet'e başarısız olur.

Kuruluşunuz, kimliği doğrulanmış bir giden bağlantı proxy'si aracılığıyla İnternete erişimi gerektiriyorsa Windows 10 bilgisayarlarınızın giden bağlantı proxy'sine başarıyla kimlik doğrulayabildiğinden emin olmanız gerekir. Windows 10 bilgisayarlar makine bağlamını kullanarak cihaz kaydını çalıştırdığından makine bağlamı ile giden bağlantı proxy'sinin yapılandırılması gerekir. Yapılandırma gereksinimleri için giden bağlantı proxy'si sağlayıcınızı izleyin.

## <a name="configure-hybrid-azure-ad-join"></a>Hibrit Azure AD'ye katılımı yapılandırma

Azure AD Connect kullanarak bir hibrit Azure AD'ye katılımı yapılandırmak için şunlar gereklidir:

- Azure AD kiracınız için genel bir yöneticinin kimlik bilgileri.  
- Her bir orman için kuruluş yöneticisinin kimlik bilgileri.
- AD FS yöneticinizin kimlik bilgileri.

**Azure AD Connect kullanarak bir hibrit Azure AD'ye katılımı yapılandırmak için:**

1. Azure AD Connect'i başlatın ve ardından **Yapılandır** seçeneğine tıklayın.

   ![Hoş Geldiniz](./media/hybrid-azuread-join-federated-domains/11.png)

1. **Ek görevler** sayfasında **Cihaz seçeneklerini yapılandır** öğesini seçin ve ardından **İleri** seçeneğine tıklayın.

   ![Ek görevler](./media/hybrid-azuread-join-federated-domains/12.png)

1. **Genel Bakış** sayfasında **İleri** seçeneğine tıklayın.

   ![Genel Bakış](./media/hybrid-azuread-join-federated-domains/13.png)

1. **Azure AD'ye Bağlanma** sayfasında Azure AD kiracınızın genel yöneticisinin kimlik bilgilerini girin ve ardından **İleri** seçeneğine tıklayın.

   ![Azure AD'ye Bağlanma](./media/hybrid-azuread-join-federated-domains/14.png)

1. **Cihaz seçenekleri** sayfasında **Hibrit Azure AD'ye katılımı yapılandır** öğesini seçin ve ardından **İleri** seçeneğine tıklayın.

   ![Cihaz seçenekleri](./media/hybrid-azuread-join-federated-domains/15.png)

1. **SCP** sayfasında aşağıdaki adımları gerçekleştirin ve ardından **İleri** seçeneğine tıklayın:

   ![SCP](./media/hybrid-azuread-join-federated-domains/16.png)

   1. Ormanı seçin.
   1. Kimlik doğrulama hizmetini seçin. Kuruluşunuzda yalnızca Windows 10 istemciler yoksa ve bilgisayar/cihaz eşitleme yapılandırmasını yapılandırmadıysanız veya kuruluşunuz SeamlessSSO kullanmıyorsa AD FS sunucusunu seçmeniz gerekir.
   1. Kuruluş yöneticisinin kimlik bilgilerini girmek için **Ekle** seçeneğine tıklayın.

1. **Cihaz işletim sistemleri** sayfasında Active Directory ortamınızdaki cihazlar tarafından kullanılan işletim sistemlerini seçin ve ardından **İleri** seçeneğine tıklayın.

   ![Cihaz işletim sistemi](./media/hybrid-azuread-join-federated-domains/17.png)

1. **Federasyon yapılandırması** sayfasında AD FS yöneticinizin kimlik bilgilerini girin ve ardından **İleri** seçeneğine tıklayın.

   ![Federasyon yapılandırması](./media/hybrid-azuread-join-federated-domains/18.png)

1. **Yapılandırma için hazır** sayfasında **Yapılandır** seçeneğine tıklayın.

   ![Yapılandırma için hazır](./media/hybrid-azuread-join-federated-domains/19.png)

1. **Yapılandırma tamamlandı** sayfasında **Çıkış** seçeneğine tıklayın.

   ![Yapılandırma tamamlandı](./media/hybrid-azuread-join-federated-domains/20.png)

## <a name="enable-windows-down-level-devices"></a>Windows alt düzey cihazlarını etkinleştirme

Bazı etki alanına katılmış cihazlar Windows alt düzey cihazlarıysa şunları gerçekleştirmeniz gerekir:

- Cihaz kaydı için yerel intranet ayarlarını yapılandırma
- Yükleme için Microsoft çalışma Windows katılın alt düzey bilgisayarlar

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Cihaz kaydı için yerel intranet ayarlarını yapılandırma

Windows aşağı düzey cihazlarınızın karma Azure AD'ye katılmasını başarıyla tamamlamak ve cihazlar Azure AD'de kimlik doğrularken sertifika istemlerini atlamak için etki alanına katılmış cihazlarınıza Internet Explorer'da Yerel Intranet bölgesine aşağıdaki URL'leri eklemek üzere bir ilke gönderebilirsiniz:

- `https://device.login.microsoftonline.com`
- Kuruluşunuzun Güvenlik Belirteci Hizmeti (STS - federasyon etki alanları)
- `https://autologon.microsoftazuread-sso.com` (Sorunsuz SSO için).

Ayrıca, kullanıcının yerel intranet bölgesinde **Betik yoluyla durum çubuğu güncelleştirmelerine izin ver** seçeneğini etkinleştirmeniz gerekir.

### <a name="install-microsoft-workplace-join-for-windows-down-level-computers"></a>Yükleme için Microsoft çalışma Windows katılın alt düzey bilgisayarlar

Windows alt düzey cihazları kaydetmek için kuruluşlar yüklemelisiniz [Microsoft Workplace Join Windows 10 bilgisayarlar için](https://www.microsoft.com/download/details.aspx?id=53554) Microsoft Download Center üzerinde kullanılabilir.

Gibi bir yazılım dağıtım sistemi kullanarak pakete dağıtabilirsiniz [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager). Paket sessiz parametresiyle standart sessiz yükleme seçeneklerini destekler. Geçerli dal Configuration Manager'ın önceki sürümlerinde, tamamlanan kayıtları izleme yeteneği gibi üzerinden avantaj sunar.

Yükleyici, kullanıcı bağlamında çalışacak sistemdeki zamanlanmış bir görev oluşturur. Görev, kullanıcı bir oturum için Windows yapar durumlarda tetiklenir. Görev, kullanıcı kimlik bilgileriyle Azure AD ile Azure AD kimliklerini doğruladıktan sonra cihazla sessizce birleştirir.

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

<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
