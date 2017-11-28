---
title: "Azure AD doğrudan kimlik doğrulama - hızlı başlangıç | Microsoft Docs"
description: "Bu makalede, Azure Active Directory (Azure AD) doğrudan kimlik doğrulamayı kullanmaya başlama açıklar."
services: active-directory
keywords: "Azure AD Connect doğrudan kimlik doğrulama, Active Directory yükleyin gerekli bileşenleri Azure AD, SSO, çoklu oturum açma"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: 8adca38cc5a783abfe29725dbb0a201de4183dad
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Azure Active Directory doğrudan kimlik doğrulaması: Hızlı Başlangıç

## <a name="deploy-azure-ad-pass-through-authentication"></a>Azure AD doğrudan kimlik doğrulama dağıtma

Azure Active Directory (Azure AD) doğrudan kimlik doğrulaması, kullanıcıların şirket içi ve bulut tabanlı uygulamalar için aynı parolayı kullanarak oturum açın olanak tanır. Doğrudan kimlik doğrulama kullanıcıların parolalarını şirket içi Active Directory karşı doğrudan doğrulayarak açmaktadır.

>[!IMPORTANT]
>Bu özellik Önizleme sürümü ile kullanırsanız, sağlanan yönergeleri kullanarak kimlik doğrulaması aracıları Önizleme sürümleri yükseltme olun [Azure Active Directory doğrudan kimlik doğrulaması: yükseltme Önizleme Kimlik Doğrulama Aracısı](./active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md).

Doğrudan kimlik doğrulamayı dağıtmak için bu yönergeleri izleyin:

## <a name="step-1-check-the-prerequisites"></a>1. adım: Önkoşul denetimi

Aşağıdaki önkoşulların yerine getirildiğinden emin olun.

### <a name="in-the-azure-active-directory-admin-center"></a>Azure Active Directory Yönetim Merkezi'nde

1. Azure AD kiracınıza bir yalnızca bulut genel yönetici hesabı oluşturun. Bu şekilde, şirket içi hizmetlerinizi başarısız veya kullanılamaz hale kiracınızın yapılandırmasını yönetebilirsiniz. Hakkında bilgi edinin [bir yalnızca bulut genel yönetici hesabı ekleme](../active-directory-users-create-azure-portal.md). Bu adımı tamamladıktan, kiracınızın dışında erişebilmenizin emin olmak için kritik öneme sahiptir.
2. Bir veya daha fazla Ekle [özel etki alanı adlarını](../active-directory-domains-add-azure-portal.md) Azure AD kiracınız için. Kullanıcılarınız bu etki alanı adlarından birini oturum oturum açabilir.

### <a name="in-your-on-premises-environment"></a>Şirket içi ortamınızda

1. Windows Server 2012 R2 çalıştıran sunucu bir veya daha sonra Azure AD Connect çalıştırmak için belirleyin. Parolaları doğrulamak için gereken kullanıcı olarak aynı Active Directory ormanına sunucu ekleyin.
2. Yükleme [Azure AD Connect'in en son sürümünü](https://www.microsoft.com/download/details.aspx?id=47594) önceki adımda tanımlanan sunucusunda. Azure AD Connect çalışan zaten varsa, sürüm 1.1.644.0 olduğundan emin olun veya sonraki bir sürümü.

    >[!NOTE]
    >Azure AD Connect sürümleri 1.1.557.0, 1.1.558.0, 1.1.561.0 ve 1.1.614.0 parola karması eşitlemesi için ilgili bir sorun var. Varsa, _yok_ parola karması eşitlemesi okuma doğrudan kimlik doğrulama ile birlikte kullanmayı düşündüğünüz [Azure AD Connect sürüm notları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470).

3. Ek bir sunucu belirleyin (Windows Server 2012 R2 çalıştıran veya sonrası) çalıştırabileceğiniz tek başına bir kimlik doğrulama Aracısı. Kimlik Doğrulama Aracısı sürüm 1.5.193.0 olması gerekir veya sonraki bir sürümü. Bu ek sunucu, oturum açmak için istekleri yüksek kullanılabilirliğini sağlamak için gereklidir. Parolaları doğrulamak için gereken kullanıcı olarak aynı Active Directory ormanına sunucu ekleyin.
4. Sunucular ve Azure AD arasında bir güvenlik duvarı varsa, aşağıdaki öğeleri yapılandırın:
   - Kimlik Doğrulama Aracısı yaptığınızdan emin olun *giden* istekleri aşağıdaki bağlantı noktaları üzerinden Azure ad:
   
    | Bağlantı noktası numarası | Nasıl kullanılır |
    | --- | --- |
    | **80** | SSL sertifikası doğrulanırken sertifika iptal listeleri (CRL'ler) yükler |
    | **443** | Tüm giden iletişim hizmeti ile işleme |
   
    Güvenlik duvarı kuralları kaynak kullanıcılara göre zorlarsa, ağ hizmeti olarak çalışan Windows hizmetlerinden trafik için bu bağlantı noktalarını açın.
   - Güvenlik Duvarı veya proxy DNS uygulamaları, da beyaz liste bağlantıları için güvenilir listeye almayı sağlar,  **\*. msappproxy.net** ve  **\*. servicebus.windows.net**. Aksi durumda, erişim izni [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653), hangi haftalık güncelleştirildi.
   - Kimlik doğrulama aracılarınızı erişmesi **login.windows.net** ve **login.microsoftonline.com** ilk kaydı için. Bu URL'ler için Güvenlik Duvarı'nı açın.
   - Sertifika doğrulama için aşağıdaki URL'ler engellemesini: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80**, ve  **www.microsoft.com:80**. Bu URL'leri, diğer Microsoft ürünleri ile sertifika doğrulaması için kullanılır. Engeli kaldırılmış bu URL'leri zaten sahip olabilir.

## <a name="step-2-enable-exchange-activesync-support-optional"></a>2. adım: Exchange ActiveSync desteği (isteğe bağlı) etkinleştir

Exchange ActiveSync desteğini etkinleştirmek için bu yönergeleri izleyin:

1. Kullanım [Exchange PowerShell](https://technet.microsoft.com/library/mt587043(v=exchg.150).aspx) şu komutu çalıştırın:
```
Get-OrganizationConfig | fl per*
```

2. Değerini denetleyin `PerTenantSwitchToESTSEnabled` ayarı. Değer ise **doğru**, Kiracı düzgün şekilde yapılandırılmış. Bu genellikle çoğu müşteri için geçerlidir. Değer ise **yanlış**, aşağıdaki komutu çalıştırın:
```
Set-OrganizationConfig -PerTenantSwitchToESTSEnabled:$true
```

3. Doğrulayın değerini `PerTenantSwitchToESTSEnabled` şimdi ayar **doğru**. Sonraki adıma geçmeden önce bir saat bekleyin.

Bu adım sırasında sorunları yüz kontrol edin [sorun giderme kılavuzu](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#exchange-activesync-configuration-issues).

## <a name="step-3-enable-the-feature"></a>3. adım: özelliğini etkinleştir

Doğrudan kimlik doğrulamasını etkinleştir [Azure AD Connect](active-directory-aadconnect.md).

>[!IMPORTANT]
>Azure AD Connect birincil veya hazırlama sunucusunda doğrudan kimlik doğrulamasını etkinleştirebilirsiniz. Birincil sunucudan etkinleştirmelisiniz.

Azure AD Connect ilk kez yüklüyorsanız seçin [özel yükleme yolu](active-directory-aadconnect-get-started-custom.md). Konumundaki **kullanıcı oturum açma** sayfasında, **doğrudan kimlik doğrulama** olarak **oturum açma yönetimini**. Başarılı tamamlama sonrasında, Azure AD Connect ile aynı sunucuda doğrudan kimlik doğrulama Aracısı yüklenir. Ayrıca, doğrudan kimlik doğrulama özelliği, Kiracı'etkindir.

![Azure AD Connect: Kullanıcı oturum açma](./media/active-directory-aadconnect-sso/sso3.png)

Zaten Azure AD Connect kullanarak yüklediyseniz [hızlı yükleme](active-directory-aadconnect-get-started-express.md) veya [özel yükleme](active-directory-aadconnect-get-started-custom.md) yolunu seçin **değiştirme kullanıcı oturum açma** görev Azure AD hakkında Bağlanın ve ardından **sonraki**. Ardından **doğrudan kimlik doğrulama** oturum açma yöntemi olarak. Başarıyla tamamlandığında, doğrudan kimlik doğrulama Aracısı Azure AD Connect ile aynı sunucuda yüklü olduğundan ve Kiracı ' özelliği etkin.

![Azure AD Connect: kullanıcı oturum açma değiştirme](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>Doğrudan kimlik doğrulaması, bir kiracı düzeyi özelliğidir. Oturum açma için kullanıcıları arasında etkileri kapatma _tüm_ kiracınız yönetilen etki alanları. Doğrudan kimlik doğrulama için Active Directory Federasyon Hizmetleri (AD FS) geçiş yapıyorsanız, AD FS altyapınızı kapatılmadan önce en az 12 saat beklemeniz gerekir. Bu bekleme süresi, kullanıcılar Exchange ActiveSync'e geçişi sırasında oturum tutmak olduğunu doğrulamaktır.

## <a name="step-4-test-the-feature"></a>4. adım: Test özelliği

Doğrudan kimlik doğrulama doğru etkinleştirdiğinizden emin doğrulamak için bu yönergeleri izleyin:

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) kiracınız için genel yönetici kimlik bilgilerine sahip.
2. Seçin **Azure Active Directory** sol bölmede.
3. Seçin **Azure AD Connect**.
4. Doğrulayın **doğrudan kimlik doğrulama** özelliği görüntülenir olarak **etkin**.
5. Seçin **doğrudan kimlik doğrulama**. **Doğrudan kimlik doğrulama** bölmesi, kimlik doğrulama aracılarınızı yüklü olduğu sunucuları listeler.

![Azure Active Directory Yönetim Merkezi: Azure AD Connect bölmesi](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory Yönetim Merkezi: doğrudan kimlik doğrulama bölmesi](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

Bu aşamada, tüm yönetilen etki alanları kiracınızdaki kullanıcıların geçişli kimlik doğrulaması kullanarak oturum açabilir. Ancak, AD FS veya önceden yapılandırılmış başka bir Federasyon sağlayıcı kullanarak oturum açmak Federasyon etki alanlarındaki kullanıcılar devam. Yönetilen federe bir etki alanından dönüştürürseniz, o etki alanındaki tüm kullanıcıların geçişli kimlik doğrulaması kullanarak oturum açmayı otomatik olarak başlar. Doğrudan kimlik doğrulama özelliği, yalnızca bulut kullanıcıları etkilemez.

## <a name="step-5-ensure-high-availability"></a>5. adım: yüksek oranda kullanılabilirlik emin olun

Bir üretim ortamında doğrudan kimlik doğrulama dağıtmayı planlıyorsanız, tek başına bir kimlik doğrulama aracısını yüklemeniz gerekir. Bu ikinci kimlik doğrulama Aracısı bir sunucuya yüklemek _diğer_ bir çalışan Azure AD Connect ve ilk kimlik doğrulama Aracısı daha. Bu kurulum, oturum açmak istekleri için yüksek kullanılabilirlik sağlar. Tek başına bir kimlik doğrulama Aracısı dağıtmak için bu yönergeleri izleyin:

1. Kimlik Doğrulama Aracısı'nın en son sürümü karşıdan yükle (sürüm 1.5.193.0 veya üstü). Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) , kiracının genel yönetici kimlik bilgilerine sahip.
2. Seçin **Azure Active Directory** sol bölmede.
3. Seçin **Azure AD Connect**seçin **doğrudan kimlik doğrulama**ve ardından **aracıyı indir**.
4. Seçin **koşullarını kabul & Yükle** düğmesi.
5. Önceki adımda indirilen yürütülebilir çalıştırarak kimlik doğrulama Aracısı en son sürümünü yükleyin. İstendiğinde, kiracının genel Yöneticisi kimlik bilgilerini sağlayın.

![Azure Active Directory Yönetim Merkezi: kimlik doğrulama Aracısı Yükle düğmesi](./media/active-directory-aadconnect-pass-through-authentication/pta9.png)

![Azure Active Directory Yönetim Merkezi: aracıyı indir bölmesi](./media/active-directory-aadconnect-pass-through-authentication/pta10.png)

>[!NOTE]
>Ayrıca indirebilirsiniz [Azure Active Directory kimlik doğrulama Aracısı](https://aka.ms/getauthagent). Gözden geçirin ve kimlik doğrulama aracısının kabul olun [hizmet koşulları](https://aka.ms/authagenteula) _önce_ yükleme.

## <a name="next-steps"></a>Sonraki adımlar
- [Akıllı kilitleme](active-directory-aadconnect-pass-through-authentication-smart-lockout.md): kullanıcı hesapları korumak için Kiracı akıllı kilitleme özelliği yapılandırmayı öğrenin.
- [Geçerli sınırlamalar](active-directory-aadconnect-pass-through-authentication-current-limitations.md): hangi senaryoları ile doğrudan kimlik doğrulama desteklenir ve hangilerinin olmayan öğrenin.
- [Teknik derinlemesine](active-directory-aadconnect-pass-through-authentication-how-it-works.md): doğrudan kimlik doğrulaması özelliğinin nasıl çalıştığını anlayın.
- [Sık sorulan sorular](active-directory-aadconnect-pass-through-authentication-faq.md): Bul için sık sorulan sorulara yanıtlar.
- [Sorun giderme](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): doğrudan kimlik doğrulama özelliği ile ortak sorunları çözmeyi öğrenin.
- [Güvenlik derinlemesine](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): doğrudan kimlik doğrulama özelliği hakkında teknik bilgi alın.
- [Azure AD sorunsuz SSO](active-directory-aadconnect-sso.md): tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): yeni özellik istekleri dosya için Azure Active Directory Forumu kullanın.

