---
title: Yönetilen etki alanları için hibrit Azure Active Directory katılımını Yapılandır | Microsoft Docs
description: Hibrit Azure Active Directory join yönetilen etki alanları için yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: joflore
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: e45b3b19821644142176c5c0cc7646c4643fe17c
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235215"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-managed-domains"></a>Öğretici: Yönetilen etki alanları için hibrit Azure Active Directory katılımını Yapılandır

Kullanıcıya benzer şekilde bir cihaz, korumak istediğiniz ve her yerde ve zamanda kaynaklarınızı korumak için kullandığınız başka bir kimlik alır. Aşağıdaki yöntemlerden biri ile cihazlarınızın kimliklerini Azure AD'ye getirerek bu hedefi gerçekleştirebilirsiniz:

- Azure AD'ye katılım
- Hibrit Azure AD'ye katılım
- Azure AD kaydı

Cihazlarınızı Azure AD'ye taşıyarak, çoklu oturum açma (SSO) özelliği sayesinde bulut ve şirket içi kaynaklarınız genelinde kullanıcılarınızın üretkenliğini en üst düzeye çıkarırsınız. Ayrıca, [koşullu erişim](../active-directory-conditional-access-azure-portal.md) ile bulut ve şirket içi kaynaklarınıza erişimin güvenliği sağlayabilirsiniz.

Bu öğreticide, yönetilen etki alanlarındaki cihazlar için hibrit Azure AD'ye katılımı nasıl yapılandıracağınızı öğrenirsiniz.

> [!div class="checklist"]
> * Hibrit Azure AD'ye katılımı yapılandırma
> * Windows alt düzey cihazlarını etkinleştirme
> * Katılmış cihazları doğrulama 
> * Sorun gider 


## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, şu konularda bilgi sahibi olduğunuz varsayılır:
    
-  [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md)
    
-  [Hibrit Azure Active Directory'ye katılma uygulamanızı planlama](hybrid-azuread-join-plan.md)

-  [Cihazlarınızın hibrit Azure AD'ye katılımını denetleme](hybrid-azuread-join-control.md)

> [!NOTE]
> Azure AD, yönetilen etki alanlarında akıllı kartlar veya sertifikaları desteklemez.


Bu makaledeki senaryoda yapılandırmak için gerekir:

- [Azure AD Connect'in en son sürümünü](https://www.microsoft.com/download/details.aspx?id=47594) (1.1.819.0 veya üzeri) yüklü. 

Azure AD Connect'in hibrit Azure AD'ye katılmış olmasını istediğiniz cihazların bilgisayar nesnelerini Azure AD'ye eşitlediğini doğrulayın. Bilgisayar nesneleri belirli kuruluş birimlerine (OU) aitse bu kuruluş birimlerinin Azure AD Connect'te eşitleme için de yapılandırılmış olması gerekir.

1.1.819.0 sürümünden itibaren Azure AD Connect hibrit Azure AD'ye katılımı yapılandırmak için bir sihirbaz sağlar. Sihirbaz, yapılandırma işlemini önemli ölçüde basitleştirebilmenizi sağlar. İlgili sihirbaz, cihaz kaydı için hizmet bağlantı noktalarını (SCP) yapılandırır.

Bu makaledeki yapılandırma adımları, bu sihirbazı temel alır. 

Hibrit Azure AD'ye katılım cihazların kuruluşunuzun ağındaki şu Microsoft kaynaklarına erişim sağlamasını gerektirir:  

- https://enterpriseregistration.windows.net
- https://login.microsoftonline.com
- https://device.login.microsoftonline.com
- https://autologon.microsoftazuread-sso.com (Sorunsuz SSO kullanıyorsanız veya kullanmayı planlıyorsanız)

Kuruluşunuz Windows 10 1709 ile başlayan bir giden bağlantı proxy'si aracılığıyla İnternete erişimi gerektiriyorsa [bir grup ilkesi nesnesi (GPO) kullanarak proxy ayarlarını bilgisayarınızda yapılandırabilirsiniz](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/). Bilgisayarınız Windows 10 1709'dan önceki bir sürümü çalıştırıyorsa Windows 10 bilgisayarın Azure AD ile cihaz kaydını gerçekleştirebilmesi için Web Proxy Otomatik Bulmayı (WPAD) uygulamanız gerekir. 

Kuruluşunuz, kimliği doğrulanmış bir giden bağlantı proxy'si aracılığıyla İnternete erişimi gerektiriyorsa Windows 10 bilgisayarlarınızın giden bağlantı proxy'sine başarıyla kimlik doğrulayabildiğinden emin olmanız gerekir. Windows 10 bilgisayarlar makine bağlamını kullanarak cihaz kaydını çalıştırdığından makine bağlamı ile giden bağlantı proxy'sinin yapılandırılması gerekir. Yapılandırma gereksinimleri için giden bağlantı proxy'si sağlayıcınızı izleyin. 



## <a name="configure-hybrid-azure-ad-join"></a>Hibrit Azure AD'ye katılımı yapılandırma

Azure AD Connect kullanarak bir hibrit Azure AD'ye katılımı yapılandırmak için şunlar gereklidir:

- Azure AD kiracınız için genel bir yöneticinin kimlik bilgileri.  

- Her bir orman için kuruluş yöneticisinin kimlik bilgileri.


**Azure AD Connect kullanarak bir hibrit Azure AD'ye katılımı yapılandırmak için:**

1. Azure AD Connect'i başlatın ve ardından **Yapılandır** seçeneğine tıklayın.

    ![Hoş Geldiniz](./media/hybrid-azuread-join-managed-domains/11.png)

2. **Ek görevler** sayfasında **Cihaz seçeneklerini yapılandır** öğesini seçin ve ardından **İleri** seçeneğine tıklayın. 

    ![Ek görevler](./media/hybrid-azuread-join-managed-domains/12.png)

3. **Genel Bakış** sayfasında **İleri** seçeneğine tıklayın. 

    ![Genel Bakış](./media/hybrid-azuread-join-managed-domains/13.png)

4. **Azure AD'ye Bağlanma** sayfasında Azure AD kiracınızın genel yöneticisinin kimlik bilgilerini girin.  

    ![Azure AD'ye bağlan](./media/hybrid-azuread-join-managed-domains/14.png)

5. **Cihaz seçenekleri** sayfasında **Hibrit Azure AD'ye katılımı yapılandır** öğesini seçin ve ardından **İleri** seçeneğine tıklayın. 

    ![Cihaz seçenekleri](./media/hybrid-azuread-join-managed-domains/15.png)

6. **SCP** sayfasında, Azure AD Connect'in SCP'yi yapılandırmasını istediğiniz her bir orman için aşağıdaki adımları uygulayın ve ardından **İleri** seçeneğine tıklayın: 

    ![SCP](./media/hybrid-azuread-join-managed-domains/16.png)

    a. Ormanı seçin.

    b. Kimlik doğrulama hizmetini seçin.

    c. Kuruluş yöneticisinin kimlik bilgilerini girmek için **Ekle** seçeneğine tıklayın.


7. **Cihaz işletim sistemleri** sayfasında Active Directory ortamınızdaki cihazlar tarafından kullanılan işletim sistemlerini seçin ve ardından **İleri** seçeneğine tıklayın. 

    ![Cihaz işletim sistemi](./media/hybrid-azuread-join-managed-domains/17.png)


8. **Yapılandırma için hazır** sayfasında **Yapılandır** seçeneğine tıklayın. 

    ![Yapılandırma için hazır](./media/hybrid-azuread-join-managed-domains/19.png)

9. **Yapılandırma tamamlandı** sayfasında **Çıkış** seçeneğine tıklayın. 

    ![Yapılandırma tamamlandı](./media/hybrid-azuread-join-managed-domains/20.png)




## <a name="enable-windows-down-level-devices"></a>Windows alt düzey cihazlarını etkinleştirme

Bazı etki alanına katılmış cihazlar Windows alt düzey cihazlarıysa şunları gerçekleştirmeniz gerekir:

- Cihaz ayarlarını güncelleştirme
 
- Cihaz kaydı için yerel intranet ayarlarını yapılandırma

- Sorunsuz çoklu oturum açma (SSO) yapılandırma

- Windows alt düzey cihazlarını denetleme 


### <a name="update-device-settings"></a>Cihaz ayarlarını güncelleştir 

Windows alt düzey cihazlarını kaydetmek için, cihaz ayarlarının kullanıcıların Azure AD'de cihazları kaydedebilmesini sağlayacak şekilde ayarlandığından emin olmanız gerekir. Azure portal'da bu ayarı şu bölümde bulabilirsiniz:

`Home > [Name of your tenant] > Devices - Device settings`  


    
Aşağıdaki ilke ayarlanmalıdır **tüm**: **Kullanıcıların cihazlarını Azure AD'ye kaydedebilir**

![Cihaz kaydetme](media/hybrid-azuread-join-managed-domains/23.png)



### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Cihaz kaydı için yerel intranet ayarlarını yapılandırma

Windows aşağı düzey cihazlarınızın karma Azure AD'ye katılmasını başarıyla tamamlamak ve cihazlar Azure AD'de kimlik doğrularken sertifika istemlerini atlamak için etki alanına katılmış cihazlarınıza Internet Explorer'da Yerel Intranet bölgesine aşağıdaki URL'leri eklemek üzere bir ilke gönderebilirsiniz:

- `https://device.login.microsoftonline.com`

- `https://autologon.microsoftazuread-sso.com`.

Ayrıca, kullanıcının yerel intranet bölgesinde **Betik yoluyla durum çubuğu güncelleştirmelerine izin ver** seçeneğini etkinleştirmeniz gerekir.


### <a name="configure-seamless-sso"></a>Sorunsuz çoklu oturum açmayı yapılandırın

Ayrıca gerekir, sizin Windows başarıyla tamamlanması hibrit Azure AD'ye katılmak için kimlik doğrulama yöntemi Azure AD geçişli kimlik doğrulaması (PTA) ya da parola karması eşitleme (PHS) kullanan bir yönetilen etki alanındaki alt düzey cihazları bulut [yapılandırın Sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso-quick-start#step-2-enable-the-feature). Sorunsuz çoklu oturum açmayı yapılandırma ile ilgili sorunlar yaşıyorsanız, bkz [sorun giderme Azure Active Directory sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sso).


### <a name="control-windows-down-level-devices"></a>Windows alt düzey cihazlarını denetleme 

Windows alt düzey cihazlarını kaydetmek için İndirme Merkezi’nden bir Windows Installer paketi (.msi) indirip yüklemeniz gerekir. Daha fazla bilgi için bkz [cihazlarınızı hibrit Azure AD'ye katılma denetim](hybrid-azuread-join-control.md#control-windows-down-level-devices). 


## <a name="verify-the-registration"></a>Kaydı doğrulama

Azure kiracınızda cihaz kaydı durumunu doğrulamak için, **[Azure Active Directory PowerShell modülünde](/powershell/azure/install-msonlinev1?view=azureadps-2.0)** **[Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice)** cmdlet öğesini kullanabilirsiniz.

Hizmet ayrıntılarını kontrol etmek için **Get-MSolDevice** cmdlet kullanırken:

- Windows istemcisindeki kimlik ile eşleşen **cihaz kimliğine** sahip bir nesnenin bulunması gerekir.
- **DeviceTrustType** değerinin **Etki Alanına Katılmış** olması gerekir. Bu, Azure AD portalında Cihazlar sayfasındaki **Hibrit Azure AD'ye katılmış** durumuna eşdeğerdir.
- Koşullu erişimde kullanılan cihazlar için **Enabled** değerinin **True**, **DeviceTrustLevel** değerinin de **Managed** olması gerekir. 


**Hizmet ayrıntılarını kontrol etmek için:**

1. Yönetici olarak **Windows PowerShell** açın.

2. Azure kiracınıza bağlanmak için `Connect-MsolService` yazın.  

3. `get-msoldevice -deviceId <deviceId>`yazın.

6. **Enabled** değerinin **True** olarak ayarlandığını doğrulayın.





## <a name="troubleshoot-your-implementation"></a>Uygulamanızda sorun giderme

Etki alanına katılmış Windows cihazları için hibrit Azure AD'ye katılımı tamamlama sırasında sorun yaşıyorsanız:

- [Windows geçerli cihazları için Hibrit Azure AD'ye katılım sorunlarını giderme](troubleshoot-hybrid-join-windows-current.md)
- [Windows alt düzey cihazları için Hibrit Azure AD'ye katılım sorunlarını giderme](troubleshoot-hybrid-join-windows-legacy.md)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Federasyon etki alanları için hibrit Azure Active Directory'ye katılımı yapılandırma](hybrid-azuread-join-federated-domains.md)
> [Elle hibrit Azure Active Directory'ye katılımı yapılandırma](hybrid-azuread-join-manual.md)

