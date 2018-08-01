---
title: Karma yapılandırmak için Azure Active Directory'ye katılmış cihazlarda nasıl | Microsoft Docs
description: Hibrit Azure Active Directory'ye katılmış cihazlarda yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: b9acc829439578f2f86dfbd51164cb3eaf923c2a
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39369313"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-managed-domains"></a>Öğretici: yönetilen etki alanları için hibrit Azure Active Directory join yapılandırın

Benzer şekilde bir kullanıcı, bir cihaz korumak ve ayrıca istediğiniz zamanda ve konumda kaynaklarınızı korumak için kullanmak istediğiniz başka bir kimlik gelmektedir. Aşağıdaki yöntemlerden birini kullanarak Azure AD'de cihazlarınızın kimlikleri taşıyarak bu hedefe gerçekleştirebilirsiniz:

- Azure AD'ye katılım
- Hibrit Azure AD'ye katılma
- Azure AD kaydı

Cihazlarınızı Azure AD'ye taşıyarak, çoklu oturum açma (SSO) aracılığıyla kullanıcılarınızın üretkenlik Bulut ve şirket içi kaynaklarınız genelinde en üst düzeye çıkarın. Aynı anda ile Bulut ve şirket kaynaklarına erişim güvenliğini sağlayabilirsiniz [koşullu erişim](../active-directory-conditional-access-azure-portal.md).

Bu öğreticide, hibrit Azure AD'ye katılım cihazlar için yönetilen etki alanlarında yapılandırma konusunda bilgi edinin.

> [!div class="checklist"]
> * Karma Azure AD katılımını Yapılandır
> * Windows alt düzey cihazları etkinleştirme
> * Katılan cihazlar doğrulayın 
> * Sorun giderme 


## <a name="prerequisites"></a>Önkoşullar

Bu öğretici, aşina olduğunuzu varsayar:
    
-  [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md)
    
-  [Hibrit Azure Active Directory join uygulamanızı planlama](hybrid-azuread-join-plan.md)

Bu makaledeki senaryoda yapılandırmak için ihtiyacınız [Azure AD Connect'in en son sürümünü](https://www.microsoft.com/download/details.aspx?id=47594) (1.1.819.0 veya üzeri) yüklenecek. 
 

1.1.819.0 sürümünden başlayarak, Azure AD Connect ile hibrit Azure AD'ye katılım'ı yapılandırmak için bir sihirbaz sağlar. Sihirbaz, önemli ölçüde yapılandırma işlemini basitleştirmenize olanak sağlar. İlgili sihirbaz, cihaz kaydı için hizmet bağlantı noktaları (SCP) yapılandırır.

Bu makalede yapılandırma adımları, bu sihirbazın temel alır. 

Hibrit Azure AD'ye katılım, kuruluşunuzun ağ içinde aşağıdaki Microsoft kaynaklarına erişim sağlamak için cihazlar gerektirir:  

- https://enterpriseregistration.windows.net
- https://login.microsoftonline.com
- https://device.login.microsoftonline.com
- https://autologon.microsoftazuread-sso.com (Kullanarak veya sorunsuz çoklu oturum açmayı kullanmak planlama)

Kuruluşunuzda bir giden proxy üzerinden Internet erişimi gerekiyorsa, Windows 10 1709'ile başlayan, Ara sunucu ayarlarını bilgisayarınızda bir Grup İlkesi nesnesi (GPO) kullanarak yapılandırabilirsiniz. Bilgisayarınızı Windows 10 1709 ' daha eski bir şey olarak çalışıyorsa, Azure AD ile cihaz kaydı yapmak Windows 10 bilgisayarlarını etkinleştirmek için Web Proxy Otomatik Bulma (WPAD) uygulamanız gerekir. 

Kuruluşunuzda giden kimliği doğrulanmış bir ara sunucu üzerinden Internet erişimi gerekiyorsa, Windows 10 bilgisayarlarınızı giden proxy sunucusuna başarıyla kimlik doğrulayabildiğini emin olmanız gerekir. Windows 10 bilgisayarlar, makine bağlamını kullanarak cihaz kaydı çalıştığından, makine bağlamını kullanarak giden bağlantı proxy'si kimlik doğrulamasını yapılandırmak gereklidir. Giden proxy sağlayıcınız ile yapılandırma gereksinimlerini izleyin. 



## <a name="configure-hybrid-azure-ad-join"></a>Karma Azure AD katılımını Yapılandır

Azure AD Connect kullanarak bir hibrit Azure AD'ye katılım'ı yapılandırmak için gerekir:

- Azure AD kiracınız için genel yönetici kimlik bilgileri.  

- Kurumsal yönetici kimlik bilgileri her bir orman için.


**Azure AD Connect kullanarak bir hibrit Azure AD'ye katılma yapılandırmak için:**

1. Azure AD Connect başlatın ve ardından **yapılandırma**.

    ![Hoş Geldiniz](./media/hybrid-azuread-join-managed-domains/11.png)

2. Üzerinde **ek görevler** sayfasında **cihaz seçeneklerini yapılandır**ve ardından **sonraki**. 

    ![Ek görevler](./media/hybrid-azuread-join-managed-domains/12.png)

3. Üzerinde **genel bakış** sayfasında **sonraki**. 

    ![Genel Bakış](./media/hybrid-azuread-join-managed-domains/13.png)

4. Üzerinde **Azure ad Connect** sayfasında, Azure AD kiracınız için genel yönetici kimlik bilgilerini girin.  

    ![Azure AD'ye Bağlanma](./media/hybrid-azuread-join-managed-domains/14.png)

5. Üzerinde **cihaz seçenekleri** sayfasında **yapılandırma hibrit Azure AD'ye katılma**ve ardından **sonraki**. 

    ![Cihaz seçenekleri](./media/hybrid-azuread-join-managed-domains/15.png)

6. Üzerinde **SCP** sayfasında, istediğiniz Azure AD Connect'in SCP'yi için aşağıdaki adımları uygulayın ve ardından her bir orman için **sonraki**: 

    ![SCP](./media/hybrid-azuread-join-managed-domains/16.png)

    a. Orman seçin.

    b. Kimlik doğrulama hizmeti seçin.

    c. Tıklayın **Ekle** kuruluş yöneticisi kimlik bilgilerini girmek için.


7. Üzerinde **cihaz işletim sistemlerinin** sayfasında, Active Directory ortamınızdaki cihazlar tarafından kullanılan işletim sistemlerini seçin ve ardından **sonraki**. 

    ![Cihaz işletim sistemi](./media/hybrid-azuread-join-managed-domains/17.png)


8. Üzerinde **yapılandırma için hazır** sayfasında **yapılandırma**. 

    ![Yapılandırma için hazır](./media/hybrid-azuread-join-managed-domains/19.png)

9. Üzerinde **yapılandırmasını tamamlamak** sayfasında **çıkış**. 

    ![Yapılandırma tamamlandı](./media/hybrid-azuread-join-managed-domains/20.png)




## <a name="enable-windows-down-level-devices"></a>Windows alt düzey cihazları etkinleştirme

Windows alt düzey cihazlar etki alanına katılmış cihazlarınızı bazıları için gerekirse:

- Cihaz ayarlarını güncelleştir
 
- Cihaz kaydı için yerel intranet ayarlarını yapılandırma

### <a name="update-device-settings"></a>Cihaz ayarlarını güncelleştir 

Windows alt düzey cihazları kaydetmek için kullanıcıların cihazları Azure AD'ye kaydetme izin vermek için cihaz ayarlarının ayarlandığından emin olmanız gerekir. Azure portalında bu ayarı altında bulabilirsiniz:

`Home > [Name of your tenant] > Devices - Device settings`  


    
Aşağıdaki ilke ayarlanmalıdır **tüm**: **kullanıcıların cihazlarını Azure AD'ye kaydetme**

![Cihaz kaydetme](media/hybrid-azuread-join-managed-domains/23.png)



### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Cihaz kaydı için yerel intranet ayarlarını yapılandırma

Başarıyla eksiksiz hibrit Azure AD'ye katılmak, Windows alt düzey cihazları ve önlemek için cihaz kimlik doğrulaması sırasında sertifika istemleri yerel Intranet aşağıdaki URL'leri eklemek için etki alanına katılmış cihazlar için bir ilke gönderebilmek için Azure ad kimlik doğrulaması Internet Explorer'da bölgesi:

- `https://device.login.microsoftonline.com`

- `https://device.login.microsoftonline.com`

- `https://autologon.microsoftazuread-sso.com`.

Ayrıca, etkinleştirmek gereken **izin vermek için durum çubuğu komut dosyası aracılığıyla güncelleştirmeleri** kullanıcının yerel intranet bölgesi.

## <a name="verify-the-registration"></a>Kayıt doğrulayın

Cihaz kayıt durumu, Azure kiracınızdaki doğrulamak için kullanabileceğiniz **[Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice)** cmdlet'inde  **[Azure Active Directory PowerShell Modülü](/powershell/azure/install-msonlinev1?view=azureadps-2.0)**.

Kullanırken **Get-MSolDevice** cmdlet'ini hizmet ayrıntılarını kontrol edin:

- Bir nesne ile **cihaz kimliği** istemci bulunmalıdır Windows kimliği eşleşir.
- Değeri **DeviceTrustType** olmalıdır **etki alanına katılmış**. Bunun eşdeğeri olan **hibrit Azure AD'ye katıldı** cihazlar sayfasında Azure AD portalında durum.
- Değeri **etkin** olmalıdır **True** koşullu erişim kullanan cihazlar için. 


**Hizmet Ayrıntıları kontrol etmek için:**

1. Açık **Windows PowerShell** yönetici olarak.

2. Tür `Connect-MsolService` Azure kiracınıza bağlamak için.  

3. `get-msoldevice -deviceId <deviceId>` yazın.

6. Doğrulayın **etkin** ayarlanır **True**.





## <a name="troubleshoot-your-implementation"></a>Uygulamanızda sorun giderme

Sorunları yaşıyorsanız katılmış Windows cihazlar karma tamamlama ile Azure AD'ye katılım etki alanı için bkz:

- [Windows cihazları için sorun giderme hibrit Azure AD'ye katılma](../device-management-troubleshoot-hybrid-join-windows-current.md)
- [Windows alt düzey cihazları için sorun giderme hibrit Azure AD'ye katılma](../device-management-troubleshoot-hybrid-join-windows-legacy.md)



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Federasyon etki alanları için yapılandırma hibrit Azure Active Directory join](hybrid-azuread-join-federated-domains.md)
> [hibrit Azure Active Directory join el ile yapılandırma](../device-management-hybrid-azuread-joined-devices-setup.md)

