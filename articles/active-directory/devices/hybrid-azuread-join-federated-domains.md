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
ms.openlocfilehash: b5cd03098f4b4698c40966ceb79d5263b456a979
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430316"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-federated-domains"></a>Eğitim: hibrit Azure Active Directory join Federasyon etki alanları için yapılandırma

Benzer şekilde bir kullanıcı, bir cihaz korumak ve ayrıca istediğiniz zamanda ve konumda kaynaklarınızı korumak için kullanmak istediğiniz başka bir kimlik gelmektedir. Aşağıdaki yöntemlerden birini kullanarak Azure AD'de cihazlarınızın kimlikleri taşıyarak bu hedefe gerçekleştirebilirsiniz:

- Azure AD'ye katılım
- Hibrit Azure AD'ye katılma
- Azure AD kaydı

Cihazlarınızı Azure AD'ye taşıyarak, çoklu oturum açma (SSO) aracılığıyla kullanıcılarınızın üretkenlik Bulut ve şirket içi kaynaklarınız genelinde en üst düzeye çıkarın. Aynı anda ile Bulut ve şirket kaynaklarına erişim güvenliğini sağlayabilirsiniz [koşullu erişim](../active-directory-conditional-access-azure-portal.md).

Bu öğreticide, hibrit Azure AD'ye katılımı ADFS kullanarak Federasyon cihazlar için yapılandırma konusunda bilgi edinin.

> [!div class="checklist"]
> * Karma Azure AD katılımını Yapılandır
> * Windows alt düzey cihazları etkinleştirme
> * Kayıt doğrulayın
> * Sorun giderme


## <a name="prerequisites"></a>Önkoşullar

Bu öğretici, aşina olduğunuzu varsayar:

-  [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md)

-  [Hibrit Azure Active Directory'ye katılma uygulamanızı planlama](hybrid-azuread-join-plan.md)



Bu öğreticide senaryoyu yapılandırmak için gerekir:

- Windows Server 2012 R2 AD FS ile

- [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) 1.1.819.0 sürümü veya üzeri. 
 

1.1.819.0 sürümünden başlayarak, Azure AD Connect ile hibrit Azure AD'ye katılım'ı yapılandırmak için bir sihirbaz sağlar. Sihirbaz, önemli ölçüde yapılandırma işlemini basitleştirmenize olanak sağlar. İlgili Sihirbazı:

- Cihaz kaydı için hizmet bağlantı noktaları (SCP) yapılandırır.

- Mevcut Azure AD bağlı olan taraf güveni yedekler

- Azure AD güveninizin talep kurallarını güncelleştirir

Bu makalede yapılandırma adımları, bu sihirbazın temel alır. Daha eski bir Azure AD Connect'in yüklü sürümü varsa, sunucuyu 1.1.819 veya üzeri yükseltmeniz. Azure AD Connect'in en son sürümünü yüklemek sizin için bir seçenek değilse, bkz. [el ile cihaz kaydını yapılandırmak nasıl](../device-management-hybrid-azuread-joined-devices-setup.md).

Hibrit Azure AD'ye katılım, kuruluşunuzun ağ içinde aşağıdaki Microsoft kaynaklarına erişim sağlamak için cihazlar gerektirir:  

- https://enterpriseregistration.windows.net
- https://login.microsoftonline.com
- https://device.login.microsoftonline.com
- Kuruluşunuzun STS (Federasyon etki alanları)
- https://autologon.microsoftazuread-sso.com (Kullanarak veya sorunsuz çoklu oturum açmayı kullanmak planlama)

Kuruluşunuzda bir giden proxy üzerinden Internet erişimi gerekiyorsa, Windows 10 1709'ile başlayan, Ara sunucu ayarlarını bilgisayarınızda bir Grup İlkesi nesnesi (GPO) kullanarak yapılandırabilirsiniz. Bilgisayarınızı Windows 10 1709 ' daha eski bir şey olarak çalışıyorsa, Azure AD ile cihaz kaydı yapmak Windows 10 bilgisayarlarını etkinleştirmek için Web Proxy Otomatik Bulma (WPAD) uygulamanız gerekir. 

Kuruluşunuzda giden kimliği doğrulanmış bir ara sunucu üzerinden Internet erişimi gerekiyorsa, Windows 10 bilgisayarlarınızı giden proxy sunucusuna başarıyla kimlik doğrulayabildiğini emin olmanız gerekir. Windows 10 bilgisayarlar, makine bağlamını kullanarak cihaz kaydı çalıştığından, makine bağlamını kullanarak giden bağlantı proxy'si kimlik doğrulamasını yapılandırmak gereklidir. Giden proxy sağlayıcınız ile yapılandırma gereksinimlerini izleyin. 


## <a name="configure-hybrid-azure-ad-join"></a>Karma Azure AD katılımını Yapılandır

Azure AD Connect kullanarak bir hibrit Azure AD'ye katılım'ı yapılandırmak için gerekir:

- Azure AD kiracınız için genel yönetici kimlik bilgileri.  

- Kurumsal yönetici kimlik bilgileri her bir orman için.

- AD FS yönetici kimlik bilgileri. 


**Azure AD Connect kullanarak bir hibrit Azure AD'ye katılma yapılandırmak için:**

1. Azure AD Connect başlatın ve ardından **yapılandırma**.

    ![Hoş Geldiniz](./media/hybrid-azuread-join-federated-domains/11.png)

2. Üzerinde **ek görevler** sayfasında **cihaz seçeneklerini yapılandır**ve ardından **sonraki**. 

    ![Ek görevler](./media/hybrid-azuread-join-federated-domains/12.png)

3. Üzerinde **genel bakış** sayfasında **sonraki**. 

    ![Genel Bakış](./media/hybrid-azuread-join-federated-domains/13.png)

4. Üzerinde **Azure ad Connect** sayfa, Azure AD kiracınız için genel yönetici kimlik bilgilerini girin ve ardından **sonraki**.   

    ![Azure AD'ye Bağlanma](./media/hybrid-azuread-join-federated-domains/14.png)

5. Üzerinde **cihaz seçenekleri** sayfasında **yapılandırma hibrit Azure AD'ye katılma**ve ardından **sonraki**. 

    ![Cihaz seçenekleri](./media/hybrid-azuread-join-federated-domains/15.png)

6. Üzerinde **SCP** sayfasında, aşağıdaki adımları uygulayın ve ardından **sonraki**: 

    ![SCP](./media/hybrid-azuread-join-federated-domains/16.png)

    a. Orman seçin.

    b. Kimlik doğrulama hizmeti seçin.

    c. Tıklayın **Ekle** kuruluş yöneticisi kimlik bilgilerini girmek için.


7. Üzerinde **cihaz işletim sistemlerinin** sayfasında, Active Directory ortamınızdaki cihazlar tarafından kullanılan işletim sistemlerini seçin ve ardından **sonraki**. 

    ![Cihaz işletim sistemi](./media/hybrid-azuread-join-federated-domains/17.png)

8. Üzerinde **Federasyon Yapılandırması** sayfasında, AD FS yönetici kimlik bilgilerini girin ve ardından **sonraki**. 

    ![Federasyon yapılandırması](./media/hybrid-azuread-join-federated-domains/18.png)

9. Üzerinde **yapılandırma için hazır** sayfasında **yapılandırma**. 

    ![Yapılandırma için hazır](./media/hybrid-azuread-join-federated-domains/19.png)

10. Üzerinde **yapılandırmasını tamamlamak** sayfasında **çıkış**. 

    ![Yapılandırma tamamlandı](./media/hybrid-azuread-join-federated-domains/20.png)




## <a name="enable-windows-down-level-devices"></a>Windows alt düzey cihazları etkinleştirme

Windows alt düzey cihazlar etki alanına katılmış cihazlarınızı bazıları için gerekirse:

- Cihaz ayarlarını güncelleştir
 
- Cihaz kaydı için yerel intranet ayarlarını yapılandırma


### <a name="update-device-settings"></a>Cihaz ayarlarını güncelleştir 

Windows alt düzey cihazları kaydetmek için kullanıcıların cihazları Azure AD'ye kaydetme izin vermek için cihaz ayarlarının ayarlandığından emin olmanız gerekir. Azure portalında bu ayarı altında bulabilirsiniz:

`Home > [Name of your tenant] > Devices - Device settings`  


    
Aşağıdaki ilke ayarlanmalıdır **tüm**: **kullanıcıların cihazlarını Azure AD'ye kaydetme**

![Cihaz kaydetme](./media/hybrid-azuread-join-federated-domains/23.png)


### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Cihaz kaydı için yerel intranet ayarlarını yapılandırma

Başarıyla eksiksiz hibrit Azure AD'ye katılmak, Windows alt düzey cihazları ve önlemek için cihaz kimlik doğrulaması sırasında sertifika istemleri yerel Intranet aşağıdaki URL'leri eklemek için etki alanına katılmış cihazlar için bir ilke gönderebilmek için Azure ad kimlik doğrulaması Internet Explorer'da bölgesi:

- `https://device.login.microsoftonline.com`

- `https://device.login.microsoftonline.com`

- Kuruluşunuzun güvenlik belirteci hizmeti (STS - Federasyon etki alanları)

- `https://autologon.microsoftazuread-sso.com` (için sorunsuz SSO).

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

- [Windows cihazları için sorun giderme hibrit Azure AD'ye katılma](troubleshoot-hybrid-join-windows-current.md)
- [Windows alt düzey cihazları için sorun giderme hibrit Azure AD'ye katılma](troubleshoot-hybrid-join-windows-legacy.md)



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yönetilen etki alanları için yapılandırma hibrit Azure Active Directory join](hybrid-azuread-join-managed-domains.md)
> [hibrit Azure Active Directory join el ile yapılandırma](hybrid-azuread-join-manual-steps.md)




<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
