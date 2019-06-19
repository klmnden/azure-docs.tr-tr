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
ms.openlocfilehash: b24888934d7e89a13b1b07b7138be476575fc306
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204609"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-managed-domains"></a>Öğretici: Yönetilen etki alanları için hibrit Azure Active Directory katılımını Yapılandır

Kuruluşunuzdaki bir kullanıcı gibi bir cihaz korumak istediğiniz bir çekirdek kimliğidir. Dilediğiniz zaman ve herhangi bir konumdan kaynaklarınızı korumak için bir cihazın kimliğini kullanabilirsiniz. Bu hedef, cihaz kimliklerini getiren ve bunları aşağıdaki yöntemlerden birini kullanarak Azure Active Directory (Azure AD) yöneterek gerçekleştirebilirsiniz:

- Azure AD'ye katılım
- Hibrit Azure AD'ye katılım
- Azure AD kaydı

Azure AD'ye kendi aygıtlarını getiren bulut arasında çoklu oturum açma (SSO) kullanıcı üretkenliği en üst düzeye çıkarır ve şirket içi kaynaklara. İle Bulut ve şirket kaynaklarına erişim güvenliğini sağlayabilirsiniz [koşullu erişim](../active-directory-conditional-access-azure-portal.md) aynı anda.

Bu öğreticide, yönetilen bir ortamda hibrit Azure AD'ye katılma Active Directory etki alanına katılmış bilgisayarları ve cihazları için yapılandırma konusunda bilgi edinin. 

Yönetilen bir ortam olabilir aracılığıyla dağıtılan [parola karma eşitlemesi (PHS)](../hybrid/whatis-phs.md) veya [geçişli kimlik doğrulaması (PTA)](../hybrid/how-to-connect-pta.md) ile [sorunsuz çoklu oturum açma](../hybrid/how-to-connect-sso.md). Bu senaryolar, bir federasyon sunucusu kimlik doğrulaması için yapılandırmak gerekli değildir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Hibrit Azure AD'ye katılımı yapılandırma
> * Windows alt düzey cihazlarını etkinleştirme
> * Katılmış cihazları doğrulama
> * Sorun giderme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, bu makaleler ile ilgili bilgi sahibi olduğunuz varsayılır:

- [Bir cihaz Kimliği nedir?](overview.md)
- [Hibrit Azure AD'ye katılma uygulamanızı planlama](hybrid-azuread-join-plan.md)
- [Hibrit Azure AD'ye katılma denetimli doğrulama yapma](hybrid-azuread-join-control.md)

> [!NOTE]
> Azure AD, akıllı kart veya sertifika yönetilen etki alanlarında desteklemiyor.

Bu makaledeki senaryoda yapılandırmak için ihtiyacınız [Azure AD Connect'in en son sürümünü](https://www.microsoft.com/download/details.aspx?id=47594) (1.1.819.0 veya üstü) yüklü.

Azure AD Connect cihazların hibrit Azure AD'ye Azure AD'ye katılmış olmasını istediğiniz bilgisayar nesnelerinin eşitlenen olun. Bilgisayar nesnelerinin belirli kuruluş birimine (OU) aitse, OU'ları Azure AD Connect eşitleme yapılandırmanız da gerekir. Azure AD Connect kullanarak bilgisayar nesneleri eşitleme hakkında daha fazla bilgi için bkz: [Azure AD Connect kullanarak filtreleme yapılandırma](../hybrid/how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering).

Azure AD Connect 1.1.819.0 sürümünden başlayarak, hibrit Azure AD'ye katılım'ı yapılandırmak için kullanabileceğiniz bir sihirbaz içerir. Sihirbaz yapılandırma işlemini önemli ölçüde basitleştirir. Sihirbaz, cihaz kaydı için hizmet bağlantı noktaları (SCP) yapılandırır.

Yapılandırma adımları bu makalede Azure AD Connect Sihirbazı'nı kullanarak temel alır.

Hibrit Azure AD'ye katılım, kuruluşunuzun ağ içinde aşağıdaki Microsoft kaynaklarına erişimi cihaz gerektirir:  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com` (Kullanın veya sorunsuz çoklu oturum açma kullanmayı planlıyorsanız)

Microsoft, kuruluşunuzun bir giden proxy üzerinden internet erişimi gerektirip gerektirmediğini önerir [Web Proxy Otomatik Bulma (WPAD) uygulama](https://docs.microsoft.com/previous-versions/tn-archive/cc995261(v%3dtechnet.10)) Azure AD ile cihaz kaydı için Windows 10 bilgisayarlarını etkinleştirmek için. Yapılandırma ve yönetme WPAD sorunlarla karşılaşırsanız bkz [otomatik algılama sorunlarını giderme](https://docs.microsoft.com/previous-versions/tn-archive/cc302643(v=technet.10)). 

Bilgisayarınızda proxy ayarlarını yapılandırmak için WPAD ve gereksinim kullanmıyorsanız, bu nedenle Windows 10 1709'ile başlayan yapabilirsiniz. Daha fazla bilgi için [WinHTTP yapılandırma ayarları Grup İlkesi nesnesi (GPO) kullanarak](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/).

> [!NOTE]
> WinHTTP ayarlarını kullanarak bilgisayarınızda proxy ayarlarını yapılandırın, yapılandırılan proxy sunucusuna bağlanılamıyor bilgisayarlara internet'e bağlanmak başarısız olur.

Kuruluşunuzda giden kimliği doğrulanmış bir ara sunucu üzerinden internet erişimi gerekiyorsa, Windows 10 bilgisayarlarınızı giden proxy sunucusuna başarıyla kimlik doğrulayabildiğini emin olmanız gerekir. Windows 10 bilgisayarlar, makine bağlamını kullanarak cihaz kaydı çalıştığından, makine bağlamını kullanarak giden proxy kimlik doğrulaması yapılandırmanız gerekir. Yapılandırma gereksinimleri için giden bağlantı proxy'si sağlayıcınızı izleyin.

## <a name="configure-hybrid-azure-ad-join"></a>Hibrit Azure AD'ye katılımı yapılandırma

Azure AD Connect kullanarak bir hibrit Azure AD'ye katılımı yapılandırmak için şunlar gereklidir:

- Azure AD kiracınız için genel yönetici kimlik bilgileri
- Her bir orman için Kurumsal yönetici kimlik bilgileri

**Azure AD Connect kullanarak hibrit Azure AD'ye katılma yapılandırmak için:**

1. Azure AD Connect başlatın ve ardından **yapılandırma**.

   ![Hoş Geldiniz](./media/hybrid-azuread-join-managed-domains/11.png)

1. Üzerinde **ek görevler** sayfasında **cihaz seçeneklerini yapılandır**ve ardından **sonraki**.

   ![Ek görevler](./media/hybrid-azuread-join-managed-domains/12.png)

1. Üzerinde **genel bakış** sayfasında **sonraki**.

   ![Genel Bakış](./media/hybrid-azuread-join-managed-domains/13.png)

1. **Azure AD'ye Bağlanma** sayfasında Azure AD kiracınızın genel yöneticisinin kimlik bilgilerini girin.  

   ![Azure AD'ye Bağlanma](./media/hybrid-azuread-join-managed-domains/14.png)

1. Üzerinde **cihaz seçenekleri** sayfasında **yapılandırma hibrit Azure AD'ye katılma**ve ardından **sonraki**.

   ![Cihaz seçenekleri](./media/hybrid-azuread-join-managed-domains/15.png)

1. Üzerinde **SCP** sayfasında, Azure AD Connect'in SCP'yi yapılandırmasını, aşağıdaki adımları tamamlayın ve ardından istediğiniz her bir orman için **sonraki**:

   ![SCP](./media/hybrid-azuread-join-managed-domains/16.png)

   1. Ormanı seçin.
   1. Kimlik doğrulama hizmetini seçin.
   1. Seçin **Ekle** kuruluş yöneticisi kimlik bilgilerini girmek için.

1. Üzerinde **cihaz işletim sistemlerinin** sayfasında, Active Directory ortamında kullanımınız cihazların işletim sistemlerini seçin ve ardından **sonraki**.

   ![Cihaz işletim sistemi](./media/hybrid-azuread-join-managed-domains/17.png)

1. Üzerinde **yapılandırma için hazır** sayfasında **yapılandırma**.

   ![Yapılandırma için hazır](./media/hybrid-azuread-join-managed-domains/19.png)

1. Üzerinde **yapılandırmasını tamamlamak** sayfasında **çıkış**.

   ![Yapılandırma tamamlandı](./media/hybrid-azuread-join-managed-domains/20.png)

## <a name="enable-windows-downlevel-devices"></a>Windows alt düzey cihazları etkinleştirme

Etki alanına katılmış cihazlarınızı bazıları Windows alt düzey cihazlar varsa, şunları yapmalısınız:

- Cihaz kaydı için yerel intranet ayarlarını yapılandırma
- Sorunsuz çoklu oturum açmayı yapılandırın
- Yükleme için Microsoft çalışma Windows katılın alt düzey bilgisayarlar

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Cihaz kaydı için yerel intranet ayarlarını yapılandırma

o Windows alt düzey cihazlarınızı hibrit Azure AD'ye katılımı başarıyla tamamlayın ve cihazları Azure AD'ye kimlik doğrulaması sırasında sertifika istemleri önlemek için bir ilke aşağıdaki URL'lere Internet, yerel intranet bölgesine eklemek için etki alanına katılmış cihazlarınıza gönderebilirsiniz Gezgini:

- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com`

Ayrıca etkinleştirmelisiniz **izin vermek için durum çubuğu komut dosyası aracılığıyla güncelleştirmeleri** kullanıcının yerel intranet bölgesi.

### <a name="configure-seamless-sso"></a>Sorunsuz çoklu oturum açmayı yapılandırın

[PHS] kullanan yönetilen bir etki alanındaki Windows alt düzey cihazlarınızı hibrit Azure AD katılımı başarıyla tamamlamak için... /Hybrid/whatis-phs.MD) veya [PTA](../hybrid/how-to-connect-pta.md) ayrıca Azure AD bulut kimlik doğrulama yöntemi olarak gerekir [sorunsuz çoklu oturum açmayı yapılandırma](../hybrid/how-to-connect-sso-quick-start.md#step-2-enable-the-feature).

### <a name="install-microsoft-workplace-join-for-windows-downlevel-computers"></a>Yükleme için Microsoft çalışma Windows katılın alt düzey bilgisayarlar

Windows alt düzey cihazları kaydetmek için kuruluşlar yüklemelisiniz [Microsoft Workplace Join Windows 10 bilgisayarlar için](https://www.microsoft.com/download/details.aspx?id=53554). Microsoft Workplace Join Windows 10 bilgisayarları için Microsoft Download Center'da kullanılabilir.

Gibi bir yazılım dağıtım sistemi kullanarak pakete dağıtabilirsiniz [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager). Standart sessiz yükleme seçenekleriyle paketi destekler `quiet` parametresi. Geçerli dal Configuration Manager'ın önceki sürümlerinde, tamamlanan kayıtları izleme yeteneği gibi üzerinden avantaj sunar.

Yükleyici, kullanıcı bağlamında çalışacak sistemdeki zamanlanmış bir görev oluşturur. Windows için kullanıcının oturum açtığı zaman görevi tetiklenir. Görev, Azure AD ile kimlik doğrulaması yaptıktan sonra kullanıcı kimlik bilgilerini kullanarak Azure AD ile cihazın sessizce birleştirir.

## <a name="verify-the-registration"></a>Kaydı doğrulama

Cihaz kayıt durumu, Azure kiracınızdaki doğrulamak için kullanabileceğiniz **[Get-MsolDevice](/powershell/msonline/v1/get-msoldevice)** cmdlet'inde [Azure Active Directory PowerShell Modülü](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

Kullanırken **Get-MSolDevice** cmdlet'ini hizmet ayrıntılarını kontrol edin:

- Bir nesne ile **cihaz kimliği** istemci bulunmalıdır Windows kimliği eşleşir.
- **DeviceTrustType** değerinin **Etki Alanına Katılmış** olması gerekir. Bu ayar eşdeğerdir **hibrit Azure AD'ye katıldı** üzerinde durum **cihazları** Azure AD portalında sayfa.
- Koşullu erişim, değeri kullanılan cihazlar için **etkin** olmalıdır **True** ve **DeviceTrustLevel** olmalıdır **yönetilen**.

**Hizmet Ayrıntıları kontrol etmek için**:

1. Windows PowerShell'i yönetici olarak açın.
1. Girin `Connect-MsolService` Azure kiracınıza bağlamak için.  
1. `get-msoldevice -deviceId <deviceId>` yazın.
1. **Enabled** değerinin **True** olarak ayarlandığını doğrulayın.

## <a name="troubleshoot-your-implementation"></a>Uygulamanızda sorun giderme

Etki alanına katılmış Windows cihazlar için hibrit Azure AD'ye katılma tamamlama ile sorunlarla karşılaşırsanız, bkz:

- [Hibrit Azure AD'ye katılım'ı Windows cihazları için sorun giderme](troubleshoot-hybrid-join-windows-current.md)
- [Hibrit Azure AD'ye katılım'ı Windows alt düzey cihazları için sorun giderme](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [Azure portalını kullanarak cihaz ikizlerini yönetme](device-management-azure-portal.md).
