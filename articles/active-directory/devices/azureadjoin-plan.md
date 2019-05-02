---
title: Azure Active Directory (Azure AD) birleştirme sürecinizi planlamak nasıl | Microsoft Docs
description: Ortamınızda katılmış cihazların Azure AD'ye uygulamanız için gerekli adımları açıklar.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.subservice: devices
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2018
ms.author: joflore
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 12d603ddbba9e36d562c8dcd6e3844af28c91255
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64918827"
---
# <a name="how-to-plan-your-azure-ad-join-implementation"></a>Nasıl yapılır: Azure AD katılımınızı uygulamayı planlama


Azure AD'ye katılım, Azure AD kullanıcılarınızın üretken ve güvende tutarken şirket içi Active Directory'ye katılmasını gerek kalmadan doğrudan cihazları birleştirmek olanak tanır. Kurumsal kullanıma hazır Azure AD'ye katılım ölçekli hem de kapsamlı dağıtımları için.   

Bu makalede, Azure AD join sürecinizi planlamak ihtiyacınız olan bilgileri ile sunulmaktadır.

 
## <a name="prerequisites"></a>Önkoşullar

Bu makalede, aşina olduğunuzu varsayar [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md).



## <a name="plan-your-implementation"></a>Uygulamanızı planlama

Azure AD join sürecinizi planlamak için ile kendinizi alıştırın:

|   |   |
|---|---|
|![İşaretli][1]|Senaryolarınız gözden geçirin|
|![İşaretli][1]|Kimlik altyapınızı gözden geçirin|
|![İşaretli][1]|Cihaz yönetimini değerlendirme|
|![İşaretli][1]|Uygulamalar ve kaynaklar için yapılacak değerlendirmeleri anlamaktır|
|![İşaretli][1]|Sağlama seçeneklerinizi anlayın|
|![İşaretli][1]|Kurumsal durumda Dolaşım yapılandırın|
|![İşaretli][1]|Koşullu erişimi yapılandırma|







## <a name="review-your-scenarios"></a>Senaryolarınız gözden geçirin 

Hibrit Azure AD'ye katılım olabilir, belirli senaryoları tercih edilir olsa da Azure AD join, Windows bulut öncelikli modeliyle doğrultusunda geçiş sağlar. Cihazların yönetimini modernleştirin ve cihaz ile ilgili BT maliyetlerini azaltmak planlıyorsanız, Azure AD join söz konusu amaçlar doğrultusunda harika bir temel sunar.  

 
Hedeflerinizi aşağıdaki ölçütleri ile hizalamak Azure AD'ye katılım düşünmelisiniz:

- Microsoft 365 kullanıcılarınız için üretkenliği paketi olarak artıyor.

- Bulut cihaz yönetimi çözümü ile cihazları yönetmek istiyorsunuz.

- Coğrafi olarak dağıtılmış kullanıcılar için cihaz sağlama basitleştirmek isteyebilirsiniz.

- Uygulama altyapınız modernize etme planlayın.
 

 

## <a name="review-your-identity-infrastructure"></a>Kimlik altyapınızı gözden geçirin  

Azure AD'ye katılım hem yönetilen hem de Federasyon ortamlar ile çalışır.  


### <a name="managed-environment"></a>Yönetilen ortam

Yönetilen bir ortam olabilir aracılığıyla dağıtılan [parola karma eşitlemesi](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization) veya [aracılığıyla kimlik doğrulaması başarılı](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta-quick-start) sorunsuz çoklu oturum açma ile.

Bu senaryolar, bir federasyon sunucusu kimlik doğrulaması için yapılandırmak gerekli değildir.


### <a name="federated-environment"></a>Federasyon ortamı

Birleştirilmiş bir ortamda hem WS-Güven ve WS-Federasyon protokollerini destekleyen bir kimlik sağlayıcısına sahip olmalıdır:

- **WS-Federasyon:** Bu protokol, bir cihaz Azure AD'ye için gereklidir.

- **WS-Güven:** Bu protokol, bir Azure AD alanına katılmış cihaz oturum açmak için gereklidir. 

Kimlik sağlayıcınız bu protokolleri desteklemiyorsa, Azure AD'ye katılım yerel olarak çalışmaz. Windows 10 1809 ile başlayarak, kullanıcılarınızın bir Azure AD alanına katılmış cihaz bir SAML tabanlı kimlik sağlayıcısı ile oturum açabilirsiniz [web oturum açma Windows 10](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1809#web-sign-in-to-windows-10). Şu anda web oturumu açma bir yalnızca önizleme özelliğidir.


### <a name="smartcards-and-certificate-based-authentication"></a>Akıllı kart ve sertifika tabanlı kimlik doğrulaması

Cihazları Azure AD'ye için akıllı kart veya sertifika tabanlı kimlik doğrulaması'nı kullanamazsınız. Ancak, akıllı kartlar, yapılandırılmış AD FS varsa Azure AD'ye katılmış cihazlar için oturum açmak için kullanılabilir.

**Öneri:** Windows iş için Hello güçlü, parola olmadan kimlik doğrulaması için Windows 10 cihazları uygulayın.


### <a name="user-configuration"></a>Kullanıcı Yapılandırması

Kullanıcıların oluşturursanız:

- **Şirket içi Active Directory**, bunları kullanarak Azure AD eşitleme yapmanız [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis). 

- **Azure AD**, ek kurulum gerekli değildir.

Azure AD UPN farklı UPN Azure AD'ye katılmış cihazlarda desteklenmez şirket içi. Kullanıcılarınız şirket içi UPN kullanıyorsanız, Azure AD'de birincil UPN kullanmaya geçmek planlamanız gerekir.



## <a name="assess-your-device-management"></a>Cihaz yönetimini değerlendirme

### <a name="supported-devices"></a>Desteklenen cihazlar

Azure AD'ye katılım:

- Yalnızca Windows 10 cihazları için geçerlidir. 

- Windows veya diğer işletim sistemlerinin önceki sürümleri için geçerli değildir. Windows 7/8.1 cihazınız varsa, Azure AD'ye katılım'ı dağıtmak için Windows 10'a yükseltmeniz gerekir.

- TPM'ye sahip cihazlar FIPS modunda desteklenmiyor.
 
**Öneri:** Güncelleştirilmiş özelliklerden yararlanmak için her zaman en son Windows 10 sürüm kullanın.


### <a name="management-platform"></a>Yönetim platformu

Cihaz Yönetimi Azure AD'ye katılmış cihazlar için Intune ve MDM CSP'ler gibi bir MDM platformu temel alır. Windows 10 ile uyumlu tüm MDM çözümlerinde çalışır bir yerleşik MDM Aracısı var.

> [!NOTE]
> Grup ilkeleri, şirket içi Active Directory'ye bağlı değil olarak Azure AD'ye katılmış cihazlar desteklenmez. Azure AD'ye katılmış cihazların yönetimini yalnızca MDM mümkündür


Yönetme Azure AD'ye katılmış cihazlar için iki yaklaşım vardır:

- **Yalnızca MDM** -bir cihaz özel bir Intune gibi MDM sağlayıcısı tarafından yönetilir. Tüm ilkeler, MDM kayıt işleminin bir parçası teslim edilir. Azure AD Premium veya EMS müşterileri için MDM kaydı bir Azure AD katılımı parçası olan otomatikleştirilmiş bir adımdır.

- **Ortak yönetim** -bir cihaz bir MDM sağlayıcısına ve SCCM tarafından yönetilir. Bu yaklaşımda, SCCM aracının belirli yönlerini yönetmek için bir MDM ile yönetilen cihaza yüklenir.



Grup ilkeleri kullanıyorsanız, MDM İlkesi eşlik kullanarak değerlendirmek [MDM geçiş analiz Aracı (MMAT)](https://github.com/WindowsDeviceManagement/MMAT). 

Bir MDM çözümüne yerine grup ilkelerini kullanıp kullanmadığını tespit ettiğinizden desteklenen ve desteklenmeyen ilkelerini gözden geçirin. Desteklenmeyen ilkeleri için aşağıdakileri göz önünde bulundurun:

- Desteklenmeyen ilkeleri, cihazları veya kullanıcıları Azure AD'ye katılmış için gerekli mi?

- Desteklenmeyen ilkeleri dağıtım yönetilen bir bulutta geçerlidir?

MDM çözümünüz Azure AD uygulama galerisinde kullanılabilir durumda değilse, bunu bölümünde açıklanan işlemi izleyerek ekleyebilirsiniz [MDM ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/windows/client-management/mdm/azure-active-directory-integration-with-mdm). 

Ortak yönetim SCCM ilkeleri, MDM platformu teslim edilir olsa bazı yönlerini cihazlarınızı yönetmek için kullanabilirsiniz. Microsoft Intune SCCM ile ortak yönetim sağlar. Daha fazla bilgi için [ortak yönetim için Windows 10 cihazları](https://docs.microsoft.com/sccm/core/clients/manage/co-management-overview). Intune dışında bir MDM ürünüyle kullanırsanız, Lütfen geçerli bir ortak yönetim senaryolarında MDM sağlayıcınız ile denetleyin.

**Öneri:** Yönetimi için Azure AD'ye katılmış cihazlar yalnızca MDM göz önünde bulundurun.



## <a name="understand-considerations-for-applications-and-resources"></a>Uygulamalar ve kaynaklar için yapılacak değerlendirmeleri anlamaktır

Biz geçiş yapmanızı öneririz. şirket içi uygulamalardan bulut için daha iyi bir kullanıcı deneyimi ve erişim denetimi. Ancak, Azure AD'ye katılmış cihazları sorunsuz bir şekilde hem şirket içi hem de bulut erişimi sağlar uygulamalar. Daha fazla bilgi için [nasıl SSO için şirket içi kaynakları çalışır Azure AD alanına katılmış cihazlar](azuread-join-sso.md).

Aşağıdaki bölümlerde farklı türlerde uygulamalar ve kaynaklar için konuları listeler.

### <a name="cloud-based-applications"></a>Bulut tabanlı uygulamalar

Bir uygulama için Azure AD uygulama galerisinde eklenirse, kullanıcılar Azure AD'ye katılmış cihazlar SSO alır. Ek yapılandırma gereklidir. Kullanıcılar, Microsoft Edge ve Chrome tarayıcı SSO alır. Chrome için dağıtmanız gereken [Windows 10 hesapları uzantısı](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji). 

Tüm Win32 uygulamalarını:

- Azure AD'ye katılmış cihazlarda da get SSO belirteci istekleri için Web Hesap Yöneticisi (WAM) kullanır. 

- Üzerinde güvenmeyin WAM kimlik doğrulaması için kullanıcılara sor. 


### <a name="on-premises-web-applications"></a>Şirket içi web uygulamaları

Uygulamalarınızın özel olması durumunda yerleşik ve/veya şirket içinde barındırılan tarayıcınızın güvenilen siteler için bunları eklemeniz gerekir:

- İş için Windows tümleşik kimlik doğrulamasını etkinleştir 
- Kullanıcılar için istem yok, SSO bir deneyim sağlar. 

AD FS kullanıyorsanız, bkz. [doğrulayın ve çoklu oturum açma AD FS ile yönetme](https://docs.microsoft.com/previous-versions/azure/azure-services/jj151809(v%3dazure.100)). 

**Öneri:** (Örneğin, Azure) bulutta barındırma ve daha iyi bir deneyim için Azure AD ile tümleştirme göz önünde bulundurun.

### <a name="on-premises-applications-relying-on-legacy-protocols"></a>Eski protokollerine bağlı olan şirket içi uygulamalar

Cihaz erişimi için bir etki alanı denetleyicisi varsa kullanıcılarınızın Azure ad SSO katılmış cihazlar. 

**Öneri:** Dağıtma [Azure AD uygulama ara sunucusu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) uygulamalar için güvenli erişim sağlamak için.


### <a name="on-premises-network-shares"></a>Şirket içi ağ paylaşımları

Bir cihazda bir şirket içi etki alanı denetleyicisine erişimi varsa, kullanıcılarınızın Azure AD'ye katılmış cihazların SSO sahip.

### <a name="printers"></a>Yazıcılar

Yazıcılar için dağıtmanız gereken [hibrit bulut yazdırma](https://docs.microsoft.com/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy) katılmış cihazlarda Azure AD'de yazıcılarını bulma. 

Yazıcılar otomatik olarak yalnızca bulut ortamında bulunamıyorsa olsa da kullanıcılarınızın yazıcıların UNC yolu doğrudan bunları eklemek için de kullanabilirsiniz. 


### <a name="on-premises-applications-relying-on-machine-authentication"></a>Şirket içi makine kimlik doğrulamasını bağlı olan uygulamalar

Bağlı olan makine kimlik doğrulamasını şirket içi uygulamaların Azure AD'ye katılmış cihazları desteklemez. 

**Öneri:** Bu uygulamaları devre dışı bırakma ve bunların modern alternatifleri taşımayı düşünün.

### <a name="remote-desktop-services"></a>Uzak Masaüstü Hizmetleri

Konak makine ya da Azure AD'ye katılmış olması ya da Azure AD'ye katılmış karma Azure AD'ye katılmış cihazlar için Uzak Masaüstü bağlantısı gerektirir. Uzak Masaüstü katılmamış veya Windows olmayan bir CİHAZDAN desteklenmiyor. Daha fazla bilgi için [uzak Azure ad Connect katılmış bilgisayar](https://docs.microsoft.com/windows/client-management/connect-to-remote-aadj-pc)


## <a name="understand-your-provisioning-options"></a>Sağlama seçeneklerinizi anlayın

Aşağıdaki yaklaşımlardan kullanarak Azure AD'ye katılım sağlayabilirsiniz:

- **Self Servis OOBE/ayarlarında** - modunda Self Servis kullanıcıları Azure AD'ye katılım aracılığıyla Git ayarları ya da deneyimi (OOBE) veya Windows dışı sırasında Windows işlem. Daha fazla bilgi için [iş cihazınızın, kuruluşunuzun ağına katılın](https://docs.microsoft.com/azure/active-directory/user-help/user-help-join-device-on-network). 

- **Windows Autopilot** -Windows Autopilot cihazların Azure AD'ye katılma işlemi için OOBE içinde daha sorunsuz bir deneyim için yapılandırma öncesi sağlar. Daha fazla bilgi için [Windows Autopilot genel bakış](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot). 

- **Toplu kayıt** -toplu kayıt cihazları yapılandırmak için bir toplu sağlama aracı kullanarak bir yönetici yönetilen Azure AD'ye katılmasını sağlamaya olanak tanır. Daha fazla bilgi için [Windows cihazlar için toplu kayıt](https://docs.microsoft.com/intune/windows-bulk-enroll).
 
 


Bu üç yaklaşımların bir karşılaştırması aşağıdadır. 

 
||Self Servis Kurulumu|Windows Autopilot|Toplu kayıt|
|---|---|---|---|
|Ayarlamak için kullanıcı etkileşimi gerektirir|Evet|Evet|Hayır|
|BT çaba gerektirir|Hayır|Evet|Evet|
|Geçerli akışlar|OOBE & ayarları|Yalnızca OOBE|Yalnızca OOBE|
|Birincil kullanıcı için yerel yönetici hakları|Evet, varsayılan olarak|Yapılandırılabilir|Hayır|
|Cihaz OEM desteği gerektirir|Hayır|Evet|Hayır|
|Desteklenen sürümler|1511+|1709+|1703+|
 
Yukarıdaki tabloda gözden geçirme ve her iki yaklaşımı benimsemeyi aşağıdaki konuları gözden geçirerek, dağıtım yaklaşımını veya yaklaşımları seçin:  

- Kullanıcıların teknik kurulumunu girebilmeleri için deneyimli misiniz? 

    - Self Servis, bu kullanıcılar için en iyi çalışabilir. Windows Autopilot'ı kullanıcı deneyimini iyileştirmek için göz önünde bulundurun.  

- Kullanıcılarınızın, uzak ya da şirket içi içinde misiniz? 

    - Self Servis veya uzak kullanıcılar için sorunsuz bir kurulum için en iyi Autopilot iş. 
 
- Yönetilen bir kullanıcı veya yönetici tarafından yönetilen bir yapılandırma tercih ediyorsunuz? 

    - Toplu kayıt için yönetici kullanıcılara vermekten önce cihazları ayarlamak için dağıtım temelli daha iyi çalışır.     

- 1-2 OEM'ler cihazlar satın veya OEM cihazların geniş bir dağıtım sahip?  

    - Ayrıca Autopilot desteği sınırlı OEM'ler satın alma, Autopilot ile sıkı tümleştirme yararlı olabilir. 
 

## <a name="configure-your-device-settings"></a>Cihaz ayarlarını yapılandırma

Azure portalı, kuruluşunuzda katılmış cihazların Azure AD'ye dağıtımını denetleme sağlar. İlgili ayarları yapılandırmak için **Azure Active Directory sayfasında**seçin `Devices > Device settings`.

### <a name="users-may-join-devices-to-azure-ad"></a>Kullanıcılar cihazları Azure AD'ye ekleyebilir

Bu seçenek kümesine **tüm** veya **seçili** dağıtımınızın kapsamına göre ve bir Azure AD kurulumu için izin isteyen cihazı alanına katılmış. 

![Kullanıcılar cihazları Azure AD'ye ekleyebilir](./media/azureadjoin-plan/01.png)

### <a name="additional-local-administrators-on-azure-ad-joined-devices"></a>Azure AD'ye katılan cihazlarda ek yerel yöneticiler

Seçin **seçili** ve tüm Azure AD'ye katılmış cihazlarda yerel Yöneticiler grubuna eklemek istediğiniz kullanıcıları seçer. 

![Azure AD'ye katılan cihazlarda ek yerel yöneticiler](./media/azureadjoin-plan/02.png)


### <a name="require-multi-factor-auth-to-join-devices"></a>Cihazları eklemek çok faktörlü kimlik doğrulaması gerektir

Seçin **"Evet** kullanıcılar cihazları Azure AD'ye katılma sırasında MFA gerçekleştirmek ihtiyaç duyuyorsanız. Mfa'yı kullanarak Azure AD'de cihazları katılan kullanıcıları için 2 Faktörlü cihaz haline gelir.

![Cihazları eklemek çok faktörlü kimlik doğrulaması gerektir](./media/azureadjoin-plan/03.png)




## <a name="configure-your-mobility-settings"></a>Mobility ayarlarınızı yapılandırın

Mobility ayarlarınızı yapılandırabilmek için önce ilk olarak bir MDM sağlayıcısı eklemek olabilir.

**Bir MDM sağlayıcısı eklemek için**:

1. Üzerinde **Azure Active Directory sayfasında**, **Yönet** bölümünde `Mobility (MDM and MAM)`. 

2. Tıklayın **uygulama ekleme**.

3. MDM sağlayıcınız listeden seçin.

    ![Uygulama ekleme](./media/azureadjoin-plan/04.png)

İlişkili ayarları yapılandırmak için MDM sağlayıcınızı seçin. 

### <a name="mdm-user-scope"></a>MDM kullanıcı kapsamı

Seçin **bazı** veya **tüm** dağıtımınızın kapsamına göre. 

![MDM kullanıcı kapsamı](./media/azureadjoin-plan/05.png)

Kendi kapsamına göre aşağıdakilerden biri gerçekleşir: 

- **Kullanıcı, MDM kapsamda**: MDM kaydı bir Azure AD Premium aboneliğiniz varsa, Azure AD katılımı ile birlikte otomatik hale getirilmiştir. Tüm kapsamlı kullanıcılar, Mdm için uygun bir lisansı olması gerekir Bu senaryoda, MDM kaydı başarısız olursa, Azure AD join Ayrıca geri alınacak.
    
- **Kullanıcı MDM kapsamda değil**: Kullanıcılar, MDM kapsamda değildir, herhangi bir MDM kaydı Azure AD'ye katılım tamamlar. Bu yönetilmeyen bir cihazda sonuçlanır.


### <a name="mdm-urls"></a>MDM URL'leri

MDM yapılandırması ile ilgili üç URL vardır:

- MDM kullanım koşulları URL'si

- MDM bulma URL'si 

- MDM uyumluluk URL'si


![Uygulama ekleme](./media/azureadjoin-plan/06.png)


Her URL, önceden tanımlanmış varsayılan bir değeri yok. Bu alanlar boşsa, aynı zamanda daha fazla bilgi için lütfen MDM sağlayıcınıza başvurun.

### <a name="mam-settings"></a>MAM ayarları

MAM Azure AD'ye katılmak için geçerli değildir. 


## <a name="configure-enterprise-state-roaming"></a>Kurumsal durumda Dolaşım yapılandırın

Kullanıcı ayarlarını cihazlar arasında eşitleyebilirsiniz. böylece, Azure AD'ye durumda Dolaşım etkinleştirmek istiyorsanız, bkz. [etkinleştirme Kurumsal durumu dolaşım, Azure Active Directory'de](https://docs.microsoft.com/azure/active-directory/devices/enterprise-state-roaming-enable). 

**Öneri**: Hibrit Azure AD'ye katılmış cihazlar için bile bu ayarı etkinleştirin.


## <a name="configure-conditional-access"></a>Koşullu erişimi yapılandırma

Varsa Azure AD için yapılandırılmış bir MDM sağlayıcısına katılmış cihazlarda, cihaz Yönetimi altında olarak sağlayıcı cihaz uyumsuz olarak işaretler. 

![Uyumlu cihaz](./media/azureadjoin-plan/46.png)

Bu uygulama için kullanabileceğiniz [yönetilen cihazlar için koşullu erişim ile bulut uygulama erişimi gerektiren](../conditional-access/require-managed-devices.md).




## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [İlk çalıştırma sırasında Azure AD ile yeni bir Windows 10 cihazını ekleme](azuread-joined-devices-frx.md)
> [iş cihazınızın, kuruluşunuzun ağa katılmasını sağlayın.](https://docs.microsoft.com/azure/active-directory/user-help/user-help-join-device-on-network)


<!--Image references-->
[1]: ./media/azureadjoin-plan/12.png
