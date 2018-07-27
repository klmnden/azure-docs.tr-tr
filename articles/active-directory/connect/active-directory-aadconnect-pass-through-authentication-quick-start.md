---
title: Azure AD doğrudan kimlik doğrulama - hızlı başlangıç | Microsoft Docs
description: Bu makalede Azure Active Directory (Azure AD) geçişli kimlik doğrulaması ile çalışmaya başlama işlemini açıklamaktadır.
services: active-directory
keywords: Azure AD, SSO, Azure AD Connect geçişli kimlik doğrulaması, Active Directory Yükleme gerekli bileşenleri çoklu oturum açma
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 3389fed86fba8059db82816a6fc752f5374369a7
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283377"
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Azure Active Directory geçişli kimlik doğrulaması: Hızlı Başlangıç

## <a name="deploy-azure-ad-pass-through-authentication"></a>Azure AD geçişli kimlik doğrulaması'nı dağıtma

Azure Active Directory (Azure AD) geçişli kimlik doğrulaması sayesinde kullanıcılarınız şirket içi ve bulut tabanlı uygulamalar için aynı parolayı kullanarak oturum açın. Geçişli kimlik doğrulaması kullanıcıların parolalarını şirket içi Active Directory karşı doğrudan doğrulayarak oturum açar.

>[!IMPORTANT]
>Geçişli kimlik doğrulaması için AD FS (veya diğer Federasyon teknolojileri) geçiriyorsanız, yayımlanan ayrıntılı dağıtım kılavuzunu izleyin öneririz [burada](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx).

Kiracınızda geçişli kimlik doğrulaması dağıtmak için aşağıdaki yönergeleri izleyin:

## <a name="step-1-check-the-prerequisites"></a>1. adım: önkoşulları denetleme

Aşağıdaki önkoşulların yerinde olduğundan emin olun.

### <a name="in-the-azure-active-directory-admin-center"></a>Azure Active Directory Yönetim Merkezi'nde

1. Azure AD kiracınızda yalnızca bulut genel yönetici hesabı oluşturun. Bu şekilde şirket içi hizmetlerinizi başarısız veya kullanılamaz hale kiracınızın yapılandırmasını yönetebilirsiniz. Hakkında bilgi edinin [bir yalnızca bulut genel yönetici hesabı ekleyerek](../active-directory-users-create-azure-portal.md). Bu adım tamamlandıktan, kiracınızın dışında kilitli kalmamanızı emin olmak için kritik öneme sahiptir.
2. Bir veya daha fazla Ekle [özel etki alanı adlarını](../active-directory-domains-add-azure-portal.md) Azure AD kiracınız için. Kullanıcılarınızın bu etki alanı adlarından birini bilgilerinizle oturum açabilirsiniz.

### <a name="in-your-on-premises-environment"></a>Şirket içi ortamınızda

1. Windows Server 2012 R2 çalıştıran sunucuya bir veya daha sonra Azure AD Connect çalıştırmak için bu seçeneği belirleyin. Parolaları doğrulamanız gereken kullanıcılar aynı Active Directory ormanında bir sunucu ekleyin.
2. Yükleme [Azure AD Connect'in en son sürümünü](https://www.microsoft.com/download/details.aspx?id=47594) önceki adımda tanımlanan sunucusunda. Azure AD Connect çalıştırma zaten varsa, sürüm 1.1.644.0 olduğundan emin olun veya üzeri.

    >[!NOTE]
    >Azure AD Connect sürüm 1.1.557.0, 1.1.558.0 1.1.561.0 ve 1.1.614.0 parola karması eşitleme için ilgili bir sorun var. Varsa, _yoksa_ okuma geçişli kimlik doğrulaması ile birlikte parola karması eşitleme kullanmayı düşündüğünüz [Azure AD Connect sürüm notları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470).

3. Bir veya daha fazla ek sunucularını belirleyin (Windows Server 2012 R2 çalıştıran veya üzeri) burada çalıştırabileceğiniz tek başına kimlik doğrulama aracılarının. Bu ek sunucular, oturum açmak için istekleri yüksek kullanılabilirliğini sağlamak için gereklidir. Sunucular aynı Active Directory ormanında parolaları doğrulamanız gereken kullanıcılar ekleyin.

    >[!IMPORTANT]
    >Üretim ortamlarında, en az 3 kimlik doğrulama aracılarının yüklü kiracınızda çalıştırmanızı öneririz. Kiracı başına 12 kimlik doğrulama aracılarının sistem sınırı yoktur. Ve katman 0 sistemleri gibi kimlik doğrulama aracılarının çalıştıran tüm sunucuların en iyi uygulama olarak değerlendir (bkz [başvuru](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)).

4. Sunucular ve Azure AD arasında bir güvenlik duvarı varsa, aşağıdakileri yapılandırın:
   - Kimlik doğrulama aracılarının yaptığınızdan emin olun *giden* aşağıdaki bağlantı noktaları üzerinden Azure AD'ye istekler:
   
    | Bağlantı noktası numarası | Nasıl kullanılır |
    | --- | --- |
    | **80** | SSL sertifikası doğrulanırken sertifika iptal listelerini (CRL'ler) indirmeleri |
    | **443** | Giden iletişimin hizmeti ile işleme |
   
    Güvenlik duvarınızı kurallara göre kaynak kullanıcılar zorunlu kılarsa ağ hizmeti olarak çalışan Windows hizmetlerinden trafik için bu bağlantı noktalarını açın.
   - Güvenlik Duvarı veya proxy DNS beyaz listeye ekleme, beyaz liste bağlanmasını sağlar,  **\*. msappproxy.net** ve  **\*. servicebus.windows.net**. Aksi takdirde, erişim izni [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653), hangi haftalık güncelleştirildi.
   - Kimlik doğrulama aracılarının erişmesi **login.windows.net** ve **login.microsoftonline.com** ilk kayıt için. Bu URL'ler için Güvenlik Duvarı'nı açın.
   - Sertifika doğrulaması için aşağıdaki URL'ler engellemesini: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80**, ve  **www.microsoft.com:80**. Bu URL'ler, diğer Microsoft ürünleri ile sertifika doğrulama için kullanıldığından bu zaten URL'leri engeli kaldırılmış olabilir.

## <a name="step-2-enable-the-feature"></a>2. adım: özellik etkinleştirme

Geçişli kimlik doğrulaması aracılığıyla etkinleştirme [Azure AD Connect](active-directory-aadconnect.md).

>[!IMPORTANT]
>Azure AD Connect'i birincil ya da hazırlama sunucusunda geçişli kimlik doğrulamasını etkinleştirebilirsiniz. Birincil sunucudan etkinleştirmeniz önerilir. Bir Azure AD Connect'i Hazırlama sunucusu gelecekte ayarlıyorsanız, **gerekir** geçişli kimlik doğrulaması oturum açma seçeneği olarak seçmek için devam; başka bir seçenek seçme **devre dışı** Geçişli kimlik doğrulaması Kiracı ve birincil sunucu ayarı geçersiz kılma.

Azure AD Connect ilk kez yüklüyorsanız, seçin [özel bir yükleme yolu](active-directory-aadconnect-get-started-custom.md). Konumunda **kullanıcı oturum açma** sayfasında **geçişli kimlik doğrulaması** olarak **oturum açma yöntemini**. Geçişli kimlik doğrulaması Aracısı başarıyla tamamlandığında, Azure AD Connect ile aynı sunucuya yüklenir. Ayrıca, kiracınızda geçişli kimlik doğrulaması özelliği etkin.

![Azure AD Connect: Kullanıcı oturum açma](./media/active-directory-aadconnect-sso/sso3.png)

Zaten Azure AD Connect kullanarak yüklediyseniz [hızlı yükleme](active-directory-aadconnect-get-started-express.md) veya [özel yükleme](active-directory-aadconnect-get-started-custom.md) yolunu seçin **değiştirme kullanıcı oturum açma** Azure AD'de görevi Bağlanmak ve ardından **sonraki**. Ardından **geçişli kimlik doğrulaması** oturum açma yöntemi olarak. Başarıyla tamamlandığında, geçişli kimlik doğrulaması Aracısı, Azure AD Connect ile aynı sunucuda yüklü ve kiracınızda özelliği etkinleştirilir.

![Azure AD Connect: Değişiklik kullanıcı oturum açma](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>Geçişli kimlik doğrulaması, bir kiracı düzeyinde özelliğidir. Oturum açmak için kullanıcılar arasında etkiler üzerinde kapatma _tüm_ kiracınızdaki yönetilen etki alanları. Geçişli kimlik doğrulaması için Active Directory Federasyon Hizmetleri (AD FS) geçiş yapıyorsanız, AD FS altyapınızı kapatmadan önce en az 12 saat beklemeniz gerekir. Bu bekleme süresi, kullanıcılar için Exchange ActiveSync geçiş sırasında oturum açarken tutmak, sağlamaktır. AD FS'den doğrudan kimlik doğrulamaya geçiş ile ilgili daha fazla yardım için yayımlanan ayrıntılı dağıtım kılavuzumuzu denetleyin [burada](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx).

## <a name="step-3-test-the-feature"></a>3. adım: Test özelliği

Geçişli kimlik doğrulaması doğru şekilde etkinleştirdiğinizden emin doğrulamak için aşağıdaki yönergeleri izleyin:

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) kiracınız için genel yönetici kimlik bilgileriyle.
2. Seçin **Azure Active Directory** sol bölmesinde.
3. Seçin **Azure AD Connect**.
4. Doğrulayın **geçişli kimlik doğrulaması** özellik olarak görünür **etkin**.
5. Seçin **geçişli kimlik doğrulaması**. **Geçişli kimlik doğrulaması** bölmesi, kimlik doğrulama aracılarının yüklü olduğu sunucuları listeler.

![Azure Active Directory Yönetim Merkezi: Azure AD Connect bölmesi](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory Yönetim Merkezi: geçişli kimlik doğrulaması bölmesi](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

Bu aşamada, geçişli kimlik doğrulaması kullanarak, kiracınızdaki tüm yönetilen etki alanlarındaki kullanıcılar oturum açabilir. Ancak, Federasyon etki alanlarındaki kullanıcılar AD FS veya daha önce yapılandırmış olduğunuz başka bir Federasyon sağlayıcısı kullanarak oturum açmak devam edin. Yönetilen Federasyon oluşturan etki alanından dönüştürürseniz, geçişli kimlik doğrulaması kullanarak oturum açarken bu etki alanındaki tüm kullanıcıları otomatik olarak başlatın. Geçişli kimlik doğrulaması özelliği yalnızca bulut kullanıcılarına etkilemez.

## <a name="step-4-ensure-high-availability"></a>4. adım: yüksek kullanılabilirlik sağlayın

Geçişli kimlik doğrulaması, bir üretim ortamına dağıtmayı planlıyorsanız, ek bir tek başına kimlik doğrulama aracılarının yüklemeniz gerekir. Bu kimlik doğrulama Aracısı sunucularında yükleme _diğer_ bir çalışan Azure AD connect'ten. Bu ayar, kullanıcı oturum açma istekleri için yüksek kullanılabilirlik sağlar.

>[!IMPORTANT]
>Üretim ortamlarında, en az 3 kimlik doğrulama aracılarının yüklü kiracınızda çalıştırmanızı öneririz. Kiracı başına 12 kimlik doğrulama aracılarının sistem sınırı yoktur. Ve katman 0 sistemleri gibi kimlik doğrulama aracılarının çalıştıran tüm sunucuların en iyi uygulama olarak değerlendir (bkz [başvuru](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)).

Kimlik Doğrulama Aracısı yazılımını indirmek için aşağıdaki yönergeleri izleyin:

1. Kimlik Doğrulama Aracısı en son sürümünü indirmek için (sürüm 1.5.193.0 veya üzeri), oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) kiracınızın genel yönetici kimlik bilgilerine sahip.
2. Seçin **Azure Active Directory** sol bölmesinde.
3. Seçin **Azure AD Connect**seçin **geçişli kimlik doğrulaması**ve ardından **aracısını indir**.
4. Seçin **koşulları kabul et ve indir** düğmesi.

![Azure Active Directory Yönetim Merkezi: kimlik doğrulama aracısını indir düğmesi](./media/active-directory-aadconnect-pass-through-authentication/pta9.png)

![Azure Active Directory Yönetim Merkezi: aracısını indir bölmesi](./media/active-directory-aadconnect-pass-through-authentication/pta10.png)

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

## <a name="next-steps"></a>Sonraki adımlar
- [AD FS'den doğrudan kimlik doğrulamaya geçiş](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx) -geçişli kimlik doğrulaması için AD FS (veya diğer Federasyon teknolojileri) geçirmek için ayrıntılı bir kılavuz.
- [Akıllı kilitleme](../authentication/howto-password-smart-lockout.md): kullanıcı hesapları korumak için kiracınızda akıllı kilitleme özelliğini yapılandırma hakkında bilgi edinin.
- [Geçerli sınırlamalar](active-directory-aadconnect-pass-through-authentication-current-limitations.md): hangi senaryolar geçişli kimlik doğrulaması ile desteklenir ve hangilerinin olmayan öğrenin.
- [Teknik yakından bakışın](active-directory-aadconnect-pass-through-authentication-how-it-works.md): geçişli kimlik doğrulaması özelliği nasıl çalıştığını anlayın.
- [Sık sorulan sorular](active-directory-aadconnect-pass-through-authentication-faq.md): Bul sık sorulan soruların yanıtları.
- [Sorun giderme](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): geçişli kimlik doğrulaması özelliği ile ortak sorunları çözmeyi öğrenin.
- [Güvenliğe derinlemesine bakış](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): geçişli kimlik doğrulaması özelliği hakkında teknik bilgi alın.
- [Azure AD sorunsuz çoklu oturum açma](active-directory-aadconnect-sso.md): tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): dosya yeni özellik istekleri için Azure Active Directory Forumu kullanın.

