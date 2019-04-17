---
title: Azure AD doğrudan kimlik doğrulama - hızlı başlangıç | Microsoft Docs
description: Bu makalede Azure Active Directory (Azure AD) geçişli kimlik doğrulaması ile çalışmaya başlama işlemini açıklamaktadır.
services: active-directory
keywords: Azure AD, SSO, Azure AD Connect geçişli kimlik doğrulaması, Active Directory Yükleme gerekli bileşenleri çoklu oturum açma
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/15/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: ba5455680647b90b113d31c55816a2e0b0131b33
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59617810"
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Azure Active Directory geçişli kimlik doğrulaması: Hızlı başlangıç

## <a name="deploy-azure-ad-pass-through-authentication"></a>Azure AD geçişli kimlik doğrulaması'nı dağıtma

Azure Active Directory (Azure AD) geçişli kimlik doğrulaması sayesinde kullanıcılarınız şirket içi ve bulut tabanlı uygulamalar için aynı parolayı kullanarak oturum açın. Geçişli kimlik doğrulaması kullanıcıların parolalarını şirket içi Active Directory karşı doğrudan doğrulayarak oturum açar.

>[!IMPORTANT]
>Geçişli kimlik doğrulaması için AD FS (veya diğer Federasyon teknolojileri) geçiriyorsanız, yayımlanan ayrıntılı dağıtım kılavuzunu izleyin öneririz [burada](https://aka.ms/adfstoPTADPDownload).

Kiracınızda geçişli kimlik doğrulaması dağıtmak için aşağıdaki yönergeleri izleyin:

## <a name="step-1-check-the-prerequisites"></a>1. Adım: Önkoşulları denetleme

Aşağıdaki önkoşulların yerinde olduğundan emin olun.

### <a name="in-the-azure-active-directory-admin-center"></a>Azure Active Directory Yönetim Merkezi'nde

1. Azure AD kiracınızda yalnızca bulut genel yönetici hesabı oluşturun. Bu şekilde şirket içi hizmetlerinizi başarısız veya kullanılamaz hale kiracınızın yapılandırmasını yönetebilirsiniz. Hakkında bilgi edinin [bir yalnızca bulut genel yönetici hesabı ekleyerek](../active-directory-users-create-azure-portal.md). Bu adım tamamlandıktan, kiracınızın dışında kilitli kalmamanızı emin olmak için kritik öneme sahiptir.
2. Bir veya daha fazla Ekle [özel etki alanı adlarını](../active-directory-domains-add-azure-portal.md) Azure AD kiracınız için. Kullanıcılarınızın bu etki alanı adlarından birini bilgilerinizle oturum açabilirsiniz.

### <a name="in-your-on-premises-environment"></a>Şirket içi ortamınızda

1. Windows Server 2012 R2 çalıştıran sunucuya bir veya daha sonra Azure AD Connect çalıştırmak için bu seçeneği belirleyin. Zaten etkinleştirilmezse [sunucuda TLS 1.2 etkinleştirin](./how-to-connect-install-prerequisites.md#enable-tls-12-for-azure-ad-connect). Parolaları doğrulamanız gereken kullanıcılar aynı Active Directory ormanında bir sunucu ekleyin.
2. Yükleme [Azure AD Connect'in en son sürümünü](https://www.microsoft.com/download/details.aspx?id=47594) önceki adımda tanımlanan sunucusunda. Azure AD Connect çalıştırma zaten varsa, sürüm 1.1.750.0 olduğundan emin olun veya üzeri.

    >[!NOTE]
    >Azure AD Connect sürüm 1.1.557.0, 1.1.558.0 1.1.561.0 ve 1.1.614.0 parola karması eşitleme için ilgili bir sorun var. Varsa, _yoksa_ okuma geçişli kimlik doğrulaması ile birlikte parola karması eşitleme kullanmayı düşündüğünüz [Azure AD Connect sürüm notları](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-version-history#116470).

3. Bir veya daha fazla ek sunucularını belirleyin (Windows Server 2012 R2 çalıştıran veya üstünü, TLS 1.2 etkin) burada çalıştırabileceğiniz tek başına kimlik doğrulama aracılarının. Bu ek sunucular, oturum açmak için istekleri yüksek kullanılabilirliğini sağlamak için gereklidir. Sunucular aynı Active Directory ormanında parolaları doğrulamanız gereken kullanıcılar ekleyin.

    >[!IMPORTANT]
    >Üretim ortamlarında, en az 3 kimlik doğrulama aracılarının yüklü kiracınızda çalıştırmanızı öneririz. Kiracı başına 40 kimlik doğrulama aracılarının sistem sınırı yoktur. Ve katman 0 sistemleri gibi kimlik doğrulama aracılarının çalıştıran tüm sunucuların en iyi uygulama olarak değerlendir (bkz [başvuru](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)).

4. Sunucular ve Azure AD arasında bir güvenlik duvarı varsa, aşağıdakileri yapılandırın:
   - Kimlik doğrulama aracılarının yaptığınızdan emin olun *giden* aşağıdaki bağlantı noktaları üzerinden Azure AD'ye istekler:

     | Bağlantı noktası numarası | Nasıl kullanılır |
     | --- | --- |
     | **80** | SSL sertifikası doğrulanırken sertifika iptal listelerini (CRL'ler) indirmeleri |
     | **443** | Giden iletişimin hizmeti ile işleme |
     | **8080** (isteğe bağlı) | Bağlantı noktası 443 kullanılamıyorsa, kimlik doğrulama aracılarının durumlarını on dakikada bir, 8080 bağlantı noktası üzerinden bildirin. Bu durum Azure AD portalında görüntülenir. Bağlantı noktası 8080 _değil_ kullanıcı oturum açma işlemleri için kullanılır. |
     
     Güvenlik duvarınızı kurallara göre kaynak kullanıcılar zorunlu kılarsa ağ hizmeti olarak çalışan Windows hizmetlerinden trafik için bu bağlantı noktalarını açın.
   - Güvenlik Duvarı veya proxy DNS beyaz listeye ekleme, beyaz liste bağlanmasını sağlar,  **\*. msappproxy.net** ve  **\*. servicebus.windows.net**. Aksi takdirde, erişim izni [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653), hangi haftalık güncelleştirildi.
   - Kimlik doğrulama aracılarının erişmesi **login.windows.net** ve **login.microsoftonline.com** ilk kayıt için. Bu URL'ler için Güvenlik Duvarı'nı açın.
   - Sertifika doğrulaması için aşağıdaki URL'ler engellemesini: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80**, ve **www \.microsoft.com:80**. Bu URL'ler, diğer Microsoft ürünleri ile sertifika doğrulama için kullanıldığından bu zaten URL'leri engeli kaldırılmış olabilir.

## <a name="step-2-enable-the-feature"></a>2. Adım: Özellik etkinleştirme

Geçişli kimlik doğrulaması aracılığıyla etkinleştirme [Azure AD Connect](whatis-hybrid-identity.md).

>[!IMPORTANT]
>Azure AD Connect'i birincil ya da hazırlama sunucusunda geçişli kimlik doğrulamasını etkinleştirebilirsiniz. Birincil sunucudan etkinleştirmeniz önerilir. Bir Azure AD Connect'i Hazırlama sunucusu gelecekte ayarlıyorsanız, **gerekir** geçişli kimlik doğrulaması oturum açma seçeneği olarak seçmek için devam; başka bir seçenek seçme **devre dışı** Geçişli kimlik doğrulaması Kiracı ve birincil sunucu ayarı geçersiz kılma.

Azure AD Connect ilk kez yüklüyorsanız, seçin [özel bir yükleme yolu](how-to-connect-install-custom.md). Konumunda **kullanıcı oturum açma** sayfasında **geçişli kimlik doğrulaması** olarak **oturum açma yöntemini**. Geçişli kimlik doğrulaması Aracısı başarıyla tamamlandığında, Azure AD Connect ile aynı sunucuya yüklenir. Ayrıca, kiracınızda geçişli kimlik doğrulaması özelliği etkin.

![Azure AD Connect: Kullanıcı oturumu açma](./media/how-to-connect-pta-quick-start/sso3.png)

Zaten Azure AD Connect kullanarak yüklediyseniz [hızlı yükleme](how-to-connect-install-express.md) veya [özel yükleme](how-to-connect-install-custom.md) yolunu seçin **değiştirme kullanıcı oturum açma** Azure AD'de görevi Bağlanmak ve ardından **sonraki**. Ardından **geçişli kimlik doğrulaması** oturum açma yöntemi olarak. Başarıyla tamamlandığında, geçişli kimlik doğrulaması Aracısı, Azure AD Connect ile aynı sunucuda yüklü ve kiracınızda özelliği etkinleştirilir.

![Azure AD Connect: Kullanıcı oturumunu değiştir](./media/how-to-connect-pta-quick-start/changeusersignin.png)

>[!IMPORTANT]
>Geçişli kimlik doğrulaması, bir kiracı düzeyinde özelliğidir. Oturum açmak için kullanıcılar arasında etkiler üzerinde kapatma _tüm_ kiracınızdaki yönetilen etki alanları. Geçişli kimlik doğrulaması için Active Directory Federasyon Hizmetleri (AD FS) geçiş yapıyorsanız, AD FS altyapınızı kapatmadan önce en az 12 saat beklemeniz gerekir. Bu bekleme süresi, kullanıcılar için Exchange ActiveSync geçiş sırasında oturum açarken tutmak, sağlamaktır. AD FS'den doğrudan kimlik doğrulamaya geçiş ile ilgili daha fazla yardım için yayımlanan ayrıntılı dağıtım planımız denetleyin [burada](https://aka.ms/adfstoptadpdownload).

## <a name="step-3-test-the-feature"></a>3. Adım: Bu özelliği sınama

Geçişli kimlik doğrulaması doğru şekilde etkinleştirdiğinizden emin doğrulamak için aşağıdaki yönergeleri izleyin:

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) kiracınız için genel yönetici kimlik bilgileriyle.
2. Seçin **Azure Active Directory** sol bölmesinde.
3. Seçin **Azure AD Connect**.
4. Doğrulayın **geçişli kimlik doğrulaması** özellik olarak görünür **etkin**.
5. Seçin **geçişli kimlik doğrulaması**. **Geçişli kimlik doğrulaması** bölmesi, kimlik doğrulama aracılarının yüklü olduğu sunucuları listeler.

![Azure Active Directory Yönetim Merkezi: Azure AD Connect bölmesi](./media/how-to-connect-pta-quick-start/pta7.png)

![Azure Active Directory Yönetim Merkezi: Geçişli kimlik doğrulaması bölmesi](./media/how-to-connect-pta-quick-start/pta8.png)

Bu aşamada, geçişli kimlik doğrulaması kullanarak, kiracınızdaki tüm yönetilen etki alanlarındaki kullanıcılar oturum açabilir. Ancak, Federasyon etki alanlarındaki kullanıcılar AD FS veya daha önce yapılandırmış olduğunuz başka bir Federasyon sağlayıcısı kullanarak oturum açmak devam edin. Yönetilen Federasyon oluşturan etki alanından dönüştürürseniz, geçişli kimlik doğrulaması kullanarak oturum açarken bu etki alanındaki tüm kullanıcıları otomatik olarak başlatın. Geçişli kimlik doğrulaması özelliği yalnızca bulut kullanıcılarına etkilemez.

## <a name="step-4-ensure-high-availability"></a>4. Adım: Yüksek kullanılabilirlik sağlayın

Geçişli kimlik doğrulaması, bir üretim ortamına dağıtmayı planlıyorsanız, ek bir tek başına kimlik doğrulama aracılarının yüklemeniz gerekir. Bu kimlik doğrulama Aracısı sunucularında yükleme _diğer_ bir çalışan Azure AD connect'ten. Bu ayar, kullanıcı oturum açma istekleri için yüksek kullanılabilirlik sağlar.

>[!IMPORTANT]
>Üretim ortamlarında, en az 3 kimlik doğrulama aracılarının yüklü kiracınızda çalıştırmanızı öneririz. Kiracı başına 40 kimlik doğrulama aracılarının sistem sınırı yoktur. Ve katman 0 sistemleri gibi kimlik doğrulama aracılarının çalıştıran tüm sunucuların en iyi uygulama olarak değerlendir (bkz [başvuru](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)).

Birden fazla geçişli kimlik doğrulama aracılarının yüklenmesiyle, yüksek oranda kullanılabilir, ancak arasında kimlik doğrulama aracılarının belirleyici olmayan yük dengelemesi sağlar. Kiracınız için gereken kimlik doğrulama aracılarının kaç belirlemek için kiracınızda görmeyi beklediğiniz oturum açma isteklerinin ortalama yük ve yoğun göz önünde bulundurun. Bir ölçüt tek bir kimlik doğrulama Aracısı ikinci bir standart bir 4 çekirdekli CPU, 16 GB RAM sunucu kimlik doğrulama işlemi 300-400 başa çıkabilir.

Ağ trafiğini tahmin etmek için aşağıdaki boyutlandırma yönergeleri kullanın:
- Her isteğin yükünün boyutuna sahip (0.5K + 1 K * num_of_agents) bayt; yani, verileri Azure ad kimlik doğrulama Aracısı. Burada, kimlik doğrulama aracılarının sayısını kiracınızda kayıtlı "num_of_agents" gösterir.
- Her yanıt yükü boyut olan 1 K bayta sahip; Azure AD'ye başka bir deyişle, veriler kimlik doğrulaması Aracısı'ndan.

Çoğu müşteri için toplam üç kimlik doğrulama aracılarının, yüksek kullanılabilirlik ve kapasite için yeterlidir. Oturum açma gecikme süresini iyileştirmek için etki alanı denetleyicilerinizin yakın kimlik doğrulama aracılarının yüklemeniz gerekir.

Başlamak için kimlik doğrulama Aracısı yazılımını indirmek için bu yönergeleri izleyin:

1. Kimlik Doğrulama Aracısı en son sürümünü indirmek için (sürüm 1.5.193.0 veya üzeri), oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) kiracınızın genel yönetici kimlik bilgilerine sahip.
2. Seçin **Azure Active Directory** sol bölmesinde.
3. Seçin **Azure AD Connect**seçin **geçişli kimlik doğrulaması**ve ardından **aracısını indir**.
4. Seçin **koşulları kabul et ve indir** düğmesi.

![Azure Active Directory Yönetim Merkezi: Kimlik Doğrulama Aracısı düğmesi indirin](./media/how-to-connect-pta-quick-start/pta9.png)

![Azure Active Directory Yönetim Merkezi: Aracı bölmesinde indirin](./media/how-to-connect-pta-quick-start/pta10.png)

>[!NOTE]
>Ayrıca doğrudan yapabilecekleriniz [kimlik doğrulama Aracısı yazılımını](https://aka.ms/getauthagent). Gözden geçirin ve kimlik doğrulaması aracının kabul [hizmet kullanım koşulları](https://aka.ms/authagenteula) _önce_ yükleme.

Tek başına bir kimlik doğrulama Aracısı dağıtmak için iki yolu vardır:

İlk olarak etkileşimli olarak yalnızca yürütülebilir indirilen kimlik doğrulaması Aracısı'nı çalıştıran ve istendiğinde, kiracınızın genel yönetici kimlik bilgilerini sağlayarak bunu yapabilirsiniz.

İkinci olarak, oluşturma ve katılımsız dağıtım betiğini çalıştırın. Aynı anda birden çok kimlik doğrulama aracılarının dağıtın veya kimlik doğrulama aracılarının, etkin kullanıcı arabirimi yoksa veya Uzak Masaüstü ile erişemiyor Windows sunucuları yüklemek istediğinizde bu kullanışlıdır. Bu yaklaşımı kullanmak yönergeler şunlardır:

1. Bir kimlik doğrulama aracısı yüklemek için aşağıdaki komutu çalıştırın: `AADConnectAuthAgentSetup.exe REGISTERCONNECTOR="false" /q`.
2. Kimlik Doğrulama Aracısı hizmetimiz ile Windows PowerShell kullanarak kaydedebilirsiniz. Bir PowerShell kimlik bilgileri nesnesi oluşturma `$cred` kiracınız için genel yönetici kullanıcı adı ve parola içeren. Aşağıdaki komutu çalıştırın değiştirerek *\<kullanıcıadı\>* ve  *\<parola\>*:

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
3. Git **C:\Program Files\Microsoft Azure AD Connect kimlik doğrulaması Aracısı** ve kullanarak aşağıdaki betiği çalıştırın `$cred` oluşturduğunuz nesnesi:

        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft Azure AD Connect Authentication Agent\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred -Feature PassthroughAuthentication

>[!IMPORTANT]
>Bir kimlik doğrulama Aracısı sanal makinede yüklü değilse sanal makinenin başka bir kimlik doğrulama Aracısı Kurulum kopyalanamıyor. Bu yöntem **desteklenmeyen**.

## <a name="step-5-configure-smart-lockout-capability"></a>5. Adım: Akıllı kilitleme özelliğini yapılandırma

Akıllı kilitleme kötü aktörleri kullanıcılarınızın parolaları tahmin çalışan kilitleme veya almak için deneme yanılma yöntemlerle yardımcı olur. Active Directory ulaşmadan önce şirket içi Active Directory'de Azure AD'de ayarları akıllı kilitleme ve / veya uygun kilitleme ayarlarını yapılandırarak saldırıları filtrelenebilen. Okuma [bu makalede](../authentication/howto-password-smart-lockout.md) kiracınızda kullanıcı hesaplarınızı korumak için akıllı kilitleme ayarlarını yapılandırma hakkında daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
- [AD FS'den doğrudan kimlik doğrulamaya geçiş](https://aka.ms/adfstoptadp) -geçişli kimlik doğrulaması için AD FS (veya diğer Federasyon teknolojileri) geçirmek için ayrıntılı bir kılavuz.
- [Akıllı kilitleme](../authentication/howto-password-smart-lockout.md): Akıllı kilitleme özelliğini kiracınızda kullanıcı hesapları korumak için yapılandırmayı öğrenin.
- [Geçerli sınırlamalar](how-to-connect-pta-current-limitations.md): Geçişli kimlik doğrulaması ile hangi senaryolar desteklenir ve hangilerinin olmayan öğrenin.
- [Teknik yakından bakışın](how-to-connect-pta-how-it-works.md): Geçişli kimlik doğrulaması özelliği nasıl çalıştığını anlayın.
- [Sık sorulan sorular](how-to-connect-pta-faq.md): Sık sorulan soruların yanıtlarını bulun.
- [Sorun giderme](tshoot-connect-pass-through-authentication.md): Geçişli kimlik doğrulaması özelliği olan yaygın sorunların nasıl çözümleneceğini öğrenin.
- [Güvenliğe derinlemesine bakış](how-to-connect-pta-security-deep-dive.md): Geçişli kimlik doğrulaması özelliğini teknik bilgilerini edinin.
- [Azure AD sorunsuz çoklu oturum açma](how-to-connect-sso.md): Tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Yeni özellik istekleriniz dosya için Azure Active Directory Forumu kullanın.
