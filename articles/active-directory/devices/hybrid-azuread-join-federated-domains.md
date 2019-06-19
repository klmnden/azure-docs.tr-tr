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
ms.openlocfilehash: 738b4f47054081f0fb1b1a530bdf21cbf07a7726
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204708"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-federated-domains"></a>Öğretici: Federasyon etki alanları için hibrit Azure Active Directory katılımını Yapılandır

Kuruluşunuzdaki bir kullanıcı gibi bir cihaz korumak istediğiniz bir çekirdek kimliğidir. Dilediğiniz zaman ve herhangi bir konumdan kaynaklarınızı korumak için bir cihazın kimliğini kullanabilirsiniz. Bu hedef, cihaz kimliklerini getiren ve bunları aşağıdaki yöntemlerden birini kullanarak Azure Active Directory (Azure AD) yöneterek gerçekleştirebilirsiniz:

- Azure AD'ye katılım
- Hibrit Azure AD'ye katılım
- Azure AD kaydı

Azure AD'ye kendi aygıtlarını getiren bulut arasında çoklu oturum açma (SSO) kullanıcı üretkenliği en üst düzeye çıkarır ve şirket içi kaynaklara. İle Bulut ve şirket kaynaklarına erişim güvenliğini sağlayabilirsiniz [koşullu erişim](../active-directory-conditional-access-azure-portal.md) aynı anda.

Bu öğreticide, Active Directory Federasyon Hizmetleri (AD FS) kullanarak birleştirilmiş bir ortamda hibrit Azure AD'ye katılım'ı Active Directory etki alanına katılmış bilgisayarları ve cihazları için yapılandırma konusunda bilgi edinin.

> [!NOTE]
> Ortamınız federe bir kimlik sağlayıcısı dışında AD FS kullanıyorsa, kimlik sağlayıcınız WS-Trust Protokolü desteklediğinden emin olmanız gerekir. WS-Trust gereklidir, Windows geçerli hibrit Azure AD'ye kimlik doğrulaması için cihazları Azure AD'ye katılmış. Hibrit Azure AD'ye katılmasını sağlamaya istediğiniz Windows alt düzey cihazlar varsa, kimlik sağlayıcınız WIAORMULTIAUTHN talep desteklemesi gerekir. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Hibrit Azure AD'ye katılımı yapılandırma
> * Windows alt düzey cihazları etkinleştirme
> * Kaydı doğrulama
> * Sorun giderme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, you'e bu makalelerde alışkın varsayılır:

- [Bir cihaz Kimliği nedir?](overview.md)
- [Hibrit Azure AD'ye katılma uygulamanızı planlama](hybrid-azuread-join-plan.md)
- [Hibrit Azure AD'ye katılma denetimli doğrulama yapma](hybrid-azuread-join-control.md)

Bu öğreticide senaryoyu yapılandırmak için şunlar gereklidir:

- AD FS içeren Windows Server 2012 R2
- [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) 1.1.819.0 sürümü veya üzeri

Azure AD Connect 1.1.819.0 sürümünden başlayarak, hibrit Azure AD'ye katılım'ı yapılandırmak için kullanabileceğiniz bir sihirbaz içerir. Sihirbaz yapılandırma işlemini önemli ölçüde basitleştirir. İlgili sihirbaz:

- Cihaz kaydı için hizmet bağlantı noktaları (SCP) yapılandırır.
- Mevcut Azure AD bağlı olan tarafınıza yedekleme yapar
- Azure AD Trust'ınızdaki talep kurallarını güncelleştirir

Yapılandırma adımları bu makalede, Azure AD Connect Sihirbazı'nı kullanarak temel alır. Azure AD Connect'i önceki bir sürümü varsa, bunu 1.1.819 veya daha sonra Sihirbazı'nı kullanmak için yükseltmeniz gerekir. Azure AD Connect'in en son sürümünü yüklemek sizin için bir seçenek değilse, bkz. [hibrit Azure AD'ye katılma el ile yapılandırma](hybrid-azuread-join-manual.md).

Hibrit Azure AD'ye katılım, kuruluşunuzun ağ içinde aşağıdaki Microsoft kaynaklarına erişimi cihaz gerektirir:  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- Kuruluşunuzun güvenlik belirteci hizmeti (STS) (için Federasyon etki alanları)
- `https://autologon.microsoftazuread-sso.com` (Kullanın veya sorunsuz çoklu oturum açma kullanmayı planlıyorsanız)

AD FS'yi kullanarak Federasyon bir ortam için anlık karma Azure AD join başarısız olursa, Windows 10 1803'ile başlayarak, bilgisayar nesnesi sonradan hibrit Azure için cihaz kaydını tamamlamak için kullanılan Azure ad eşitleme için Azure AD Connect'i bağımlı olduğumuz AD alanına katılın. Azure AD Connect cihazların hibrit Azure AD'ye Azure AD'ye katılmış olmasını istediğiniz bilgisayar nesnelerinin eşitlenen olun. Bilgisayar nesnelerinin belirli kuruluş birimine (OU) aitse, OU'ları Azure AD Connect eşitleme yapılandırmanız da gerekir. Azure AD Connect kullanarak bilgisayar nesneleri eşitleme hakkında daha fazla bilgi için bkz: [Azure AD Connect kullanarak filtreleme yapılandırma](../hybrid/how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering).

Microsoft, kuruluşunuzun bir giden proxy üzerinden internet erişimi gerektirip gerektirmediğini önerir [Web Proxy Otomatik Bulma (WPAD) uygulama](https://docs.microsoft.com/previous-versions/tn-archive/cc995261(v%3dtechnet.10)) Azure AD ile cihaz kaydı için Windows 10 bilgisayarlarını etkinleştirmek için. Yapılandırma ve yönetme WPAD sorunlarla karşılaşırsanız bkz [otomatik algılama sorunlarını giderme](https://docs.microsoft.com/previous-versions/tn-archive/cc302643(v=technet.10)). 

WPAD kullanmak ve bilgisayarınızda proxy ayarlarını yapılandırmak istiyorsanız bu nedenle Windows 10 1709'ile başlayan yapabilirsiniz. Daha fazla bilgi için [WinHTTP yapılandırma ayarları Grup İlkesi nesnesi (GPO) kullanarak](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/).

> [!NOTE]
> WinHTTP ayarlarını kullanarak bilgisayarınızda proxy ayarlarını yapılandırın, yapılandırılan proxy sunucusuna bağlanılamıyor bilgisayarlara internet'e bağlanmak başarısız olur.

Kuruluşunuzda giden kimliği doğrulanmış bir ara sunucu üzerinden internet erişimi gerekiyorsa, Windows 10 bilgisayarlarınızı giden proxy sunucusuna başarıyla kimlik doğrulayabildiğini emin olmanız gerekir. Windows 10 bilgisayarlar, makine bağlamını kullanarak cihaz kaydı çalıştığından, makine bağlamını kullanarak giden proxy kimlik doğrulaması yapılandırmanız gerekir. Yapılandırma gereksinimleri için giden bağlantı proxy'si sağlayıcınızı izleyin.

## <a name="configure-hybrid-azure-ad-join"></a>Hibrit Azure AD'ye katılımı yapılandırma

Azure AD Connect kullanarak hibrit Azure AD'ye katılma yapılandırmak için gerekir:

- Azure AD kiracınız için genel yönetici kimlik bilgileri  
- Her bir orman için Kurumsal yönetici kimlik bilgileri
- AD FS yönetici kimlik bilgileri

**Azure AD Connect kullanarak hibrit Azure AD'ye katılma yapılandırmak için**:

1. Azure AD Connect başlatın ve ardından **yapılandırma**.

   ![Hoş Geldiniz](./media/hybrid-azuread-join-federated-domains/11.png)

1. Üzerinde **ek görevler** sayfasında **cihaz seçeneklerini yapılandır**ve ardından **sonraki**.

   ![Ek görevler](./media/hybrid-azuread-join-federated-domains/12.png)

1. Üzerinde **genel bakış** sayfasında **sonraki**.

   ![Genel Bakış](./media/hybrid-azuread-join-federated-domains/13.png)

1. Üzerinde **Azure ad Connect** sayfa, Azure AD kiracınız için genel yönetici kimlik bilgilerini girin ve ardından **sonraki**.

   ![Azure AD'ye Bağlanma](./media/hybrid-azuread-join-federated-domains/14.png)

1. Üzerinde **cihaz seçenekleri** sayfasında **yapılandırma hibrit Azure AD'ye katılma**ve ardından **sonraki**.

   ![Cihaz seçenekleri](./media/hybrid-azuread-join-federated-domains/15.png)

1. Üzerinde **SCP** sayfasında, aşağıdaki adımları tamamlayın ve ardından **sonraki**:

   ![SCP](./media/hybrid-azuread-join-federated-domains/16.png)

   1. Ormanı seçin.
   1. Kimlik doğrulama hizmetini seçin. Seçmelisiniz **AD FS sunucusu** Kuruluşunuzda yalnızca Windows 10 istemcileri vardır ve bilgisayar/cihaz eşitleme yapılandırdığınız veya sorunsuz çoklu oturum açma kuruluşunuzun kullandığı sürece.
   1. Seçin **Ekle** kuruluş yöneticisi kimlik bilgilerini girmek için.

1. Üzerinde **cihaz işletim sistemlerinin** sayfasında, Active Directory ortamınızdaki cihazlar kullanan işletim sistemlerini seçin ve ardından **sonraki**.

   ![Cihaz işletim sistemi](./media/hybrid-azuread-join-federated-domains/17.png)

1. Üzerinde **Federasyon Yapılandırması** sayfasında, AD FS yönetici kimlik bilgilerini girin ve ardından **sonraki**.

   ![Federasyon yapılandırması](./media/hybrid-azuread-join-federated-domains/18.png)

1. Üzerinde **yapılandırma için hazır** sayfasında **yapılandırma**.

   ![Yapılandırma için hazır](./media/hybrid-azuread-join-federated-domains/19.png)

1. Üzerinde **yapılandırmasını tamamlamak** sayfasında **çıkış**.

   ![Yapılandırma tamamlandı](./media/hybrid-azuread-join-federated-domains/20.png)

## <a name="enable-windows-downlevel-devices"></a>Windows alt düzey cihazları etkinleştirme

Etki alanına katılmış cihazlarınızı bazıları Windows alt düzey cihazlar varsa, şunları yapmalısınız:

- Cihaz kaydı için yerel intranet ayarlarını yapılandırma
- Yükleme için Microsoft çalışma Windows katılın alt düzey bilgisayarlar

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Cihaz kaydı için yerel intranet ayarlarını yapılandırma

İçin Windows alt düzey cihazlarınızı hibrit Azure AD'ye katılımı başarıyla tamamlayın ve cihazları Azure AD'ye kimlik doğrulaması sırasında sertifika istemleri önlemek için bir ilke aşağıdaki URL'lere Internet, yerel intranet bölgesine eklemek için etki alanına katılmış cihazlarınıza gönderebilirsiniz Gezgini:

- `https://device.login.microsoftonline.com`
- Kuruluşunuzun STS (için Federasyon etki alanları)
- `https://autologon.microsoftazuread-sso.com` (İçin sorunsuz SSO)

Ayrıca etkinleştirmelisiniz **izin vermek için durum çubuğu komut dosyası aracılığıyla güncelleştirmeleri** kullanıcının yerel intranet bölgesi.

### <a name="install-microsoft-workplace-join-for-windows-downlevel-computers"></a>Yükleme için Microsoft çalışma Windows katılın alt düzey bilgisayarlar

Windows alt düzey cihazları kaydetmek için kuruluşlar yüklemelisiniz [Microsoft Workplace Join Windows 10 bilgisayarlar için](https://www.microsoft.com/download/details.aspx?id=53554). Microsoft Workplace Join Windows 10 bilgisayarları için Microsoft Download Center'da kullanılabilir.

Gibi bir yazılım dağıtım sistemi kullanarak pakete dağıtabilirsiniz [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager). Standart sessiz yükleme seçenekleriyle paketi destekler `quiet` parametresi. Geçerli dal Configuration Manager'ın önceki sürümlerinde, tamamlanan kayıtları izleme yeteneği gibi üzerinden avantaj sunar.

Yükleyici, kullanıcı bağlamında çalışacak sistemdeki zamanlanmış bir görev oluşturur. Windows için kullanıcının oturum açtığı zaman görevi tetiklenir. Görev, Azure AD ile kimlik doğrulaması yaptıktan sonra kullanıcı kimlik bilgilerini kullanarak Azure AD ile cihazın sessizce birleştirir.

## <a name="verify-the-registration"></a>Kaydı doğrulama

Cihaz kayıt durumu, Azure kiracınızdaki doğrulamak için kullanabileceğiniz **[Get-MsolDevice](/powershell/msonline/v1/get-msoldevice)** cmdlet'inde [Azure Active Directory PowerShell Modülü](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

Kullanırken **Get-MSolDevice** cmdlet'ini hizmet ayrıntılarını kontrol edin:

- Bir nesne ile **cihaz kimliği** istemci bulunmalıdır Windows kimliği eşleşir.
- **DeviceTrustType** değerinin **Etki Alanına Katılmış** olması gerekir. Bu ayar eşdeğerdir **hibrit Azure AD'ye katıldı** altındaki durum **cihazları** Azure AD portalında.
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

<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
