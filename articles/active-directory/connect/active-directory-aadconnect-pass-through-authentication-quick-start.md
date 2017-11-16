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
ms.openlocfilehash: e0b58142a2ed17d2cd4749b33e9e80ff1a01662a
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Azure Active Directory doğrudan kimlik doğrulaması: Hızlı Başlangıç

## <a name="how-to-deploy-azure-ad-pass-through-authentication"></a>Azure AD doğrudan kimlik doğrulama dağıtma

Azure Active Directory (Azure AD) doğrudan kimlik doğrulaması, kullanıcıların şirket içi ve bulut tabanlı uygulamalar aynı parola kullanarak oturum açmak olanak tanır. Bu kullanıcıların parolalarını şirket içi Active Directory'nizi karşı doğrudan doğrulayarak açmaktadır.

>[!IMPORTANT]
>Bu özellik Önizleme aracılığıyla kullanmakta olduğunuz kimlik doğrulama Aracısı Önizleme sürümleri yükseltme sağlanan yönergeleri kullanarak olmanız [burada](./active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md).

Doğrudan kimlik doğrulamayı dağıtmak için bu yönergeleri izlemeniz gerekir:

## <a name="step-1-check-prerequisites"></a>1. adım: Önkoşul denetimi

Aşağıdaki önkoşulların yerine getirildiğinden emin olun:

### <a name="on-the-azure-active-directory-admin-center"></a>Azure Active Directory Yönetim Merkezi'nde

1. Azure AD kiracınıza bir yalnızca bulut genel yönetici hesabı oluşturun. Bu şekilde, şirket içi hizmetlerinizi başarısız veya kullanılamaz hale kiracınızın yapılandırmasını yönetebilirsiniz. Hakkında bilgi edinin [bir yalnızca bulut genel yönetici hesabı ekleme](../active-directory-users-create-azure-portal.md). Bu adımı gerçekleştirmeden, kiracınızın dışında erişebilmenizin emin olmak için kritik öneme sahiptir.
2. Bir veya daha fazla Ekle [özel etki alanı adları](../active-directory-domains-add-azure-portal.md) Azure AD kiracınız için. Kullanıcılarınız, bu etki alanı adlarından birini kullanarak oturum açın.

### <a name="in-your-on-premises-environment"></a>Şirket içi ortamınızda

1. Windows Server 2012 R2 çalıştıran bir sunucu tanımlayın veya daha sonra Azure AD Connect çalıştırılacak. Parolaları doğrulanması gereken kullanıcılar aynı AD ormana sunucu ekleyin.
2. Yükleme [Azure AD Connect'in en son sürümünü](https://www.microsoft.com/download/details.aspx?id=47594) önceki adımda belirlediğiniz sunucusunda. Azure AD Connect çalışan zaten varsa, sürüm 1.1.644.0 olduğundan emin olun veya sonraki bir sürümü.

    >[!NOTE]
    >Azure AD Connect 1.1.557.0, 1.1.558.0, 1.1.561.0 ve 1.1.614.0 sürümlerde ilgili bir sorunu **parola karması eşitlemesi**. Varsa, _yok_ parola karması eşitlemesi okuma doğrudan kimlik doğrulama ile birlikte kullanmayı düşündüğünüz [Azure AD Connect sürüm notları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470) daha fazla bilgi için.

3. Windows Server 2012 R2 çalıştıran ek bir sunucu tanımlayın veya tek başına bir kimlik doğrulama Aracısı çalıştırmak için daha sonra. Kimlik Doğrulama Aracısı sürüm 1.5.193.0 olması gerekir veya sonraki bir sürümü. Bu sunucu, oturum açma isteklerinin yüksek kullanılabilirlik sağlamak için gereklidir. Parolaları doğrulanması gereken kullanıcılar aynı AD ormana sunucu ekleyin.
4. Sunucular ve Azure AD arasında bir güvenlik duvarı varsa, aşağıdaki öğelerin yapılandırılması gerekir:
   - Kimlik Doğrulama Aracısı yaptığınızdan emin olun **giden** istekleri aşağıdaki bağlantı noktaları üzerinden Azure ad:
   
   | Bağlantı noktası numarası | Nasıl kullanılır |
   | --- | --- |
   | **80** | İndirme sertifika iptal listeleri (CRL'ler SSL sertifikası doğrulanırken) |
   | **443** | Tüm giden iletişim hizmetimizdeki |
   
   Duvarınız kaynak kullanıcılar kuralları zorlar, ağ hizmeti olarak çalışan Windows hizmetlerinden trafik için bu bağlantı noktalarını açın.
   - Güvenlik Duvarı veya proxy DNS uygulamaları, da beyaz liste bağlantıları için güvenilir listeye almayı sağlar,  **\*. msappproxy.net** ve  **\*. servicebus.windows.net**. Aksi durumda, erişim izni [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653), hangi haftalık güncelleştirildi.
   - Kimlik doğrulama aracılarınızı erişmesi **login.windows.net** ve **login.microsoftonline.com** ilk kaydı için bu URL'ler için için Güvenlik Duvarı'nı açın.
   - Sertifika doğrulama için aşağıdaki URL'ler engellemesini: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80** ve **www.microsoft.com:80**. Engeli kaldırılmış bu URL'leri zaten olabilir şekilde bu URL'leri diğer Microsoft ürünleri ile sertifika doğrulaması için kullanılır.

## <a name="step-2-enable-exchange-activesync-support-optional"></a>2. adım: Exchange ActiveSync desteği (isteğe bağlı) etkinleştir

Exchange ActiveSync desteğini etkinleştirmek için bu yönergeleri izleyin:

1. Kullanım [Exchange PowerShell](https://technet.microsoft.com/library/mt587043(v=exchg.150).aspx) şu komutu çalıştırın:
```
Get-OrganizationConfig | fl per*
```

2. Değerini denetleyin `PerTenantSwitchToESTSEnabled` ayarı. Değer ise **doğru**, kiracınızın'ın düzgün yapılandırıldığından - bu genellikle çoğu müşteri için bir durumdur. Değer ise **yanlış**, aşağıdaki komutu çalıştırın:
```
Set-OrganizationConfig -PerTenantSwitchToESTSEnabled:$true
```

3. Doğrulayın değerini `PerTenantSwitchToESTSEnabled` şimdi ayar **doğru**. Sonraki adıma geçmeden önce bir saat bekleyin.

Bu adım sırasında sorunları yüz, denetleyin bizim [sorun giderme kılavuzu](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#exchange-activesync-configuration-issues) daha fazla bilgi için.

## <a name="step-3-enable-the-feature"></a>3. adım: özelliğini etkinleştir

Doğrudan kimlik doğrulama kullanarak etkinleştirilebilir [Azure AD Connect](active-directory-aadconnect.md).

>[!IMPORTANT]
>Azure AD Connect birincil veya hazırlama sunucusunda doğrudan kimlik doğrulama etkinleştirilebilir. Birincil sunucudan etkinleştirmeniz önerilir.

Azure AD Connect ilk kez yüklüyorsanız seçin [özel yükleme yolu](active-directory-aadconnect-get-started-custom.md). Konumundaki **kullanıcı oturum açma** sayfasında, **doğrudan kimlik doğrulama** oturum açma yöntemi olarak. Başarılı tamamlama sonrasında, Azure AD Connect ile aynı sunucuda doğrudan kimlik doğrulama Aracısı yüklenir. Ayrıca, doğrudan kimlik doğrulama özelliği, Kiracı'etkindir.

![Azure AD Connect - kullanıcı oturum açma](./media/active-directory-aadconnect-sso/sso3.png)

Azure AD Connect zaten yüklü değilse (kullanarak [hızlı yükleme](active-directory-aadconnect-get-started-express.md) veya [özel yükleme](active-directory-aadconnect-get-started-custom.md) yol) seçin **değiştirme kullanıcı oturum açma** görev Azure AD hakkında Bağlayın ve tıklatın **sonraki**. Ardından **doğrudan kimlik doğrulama** oturum açma yöntemi olarak. Başarıyla tamamlandığında, doğrudan kimlik doğrulama Aracısı Azure AD Connect ile aynı sunucuda yüklü olduğundan ve Kiracı ' özelliği etkin.

![Azure AD Connect - değişiklik kullanıcı oturum açma](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>Doğrudan kimlik doğrulaması, bir kiracı düzeyi özelliğidir. Açma etkiler oturum açma için kullanıcıları arasında _tüm_ kiracınız yönetilen etki alanları. Doğrudan kimlik doğrulama için AD FS geçiş yapıyorsanız, öneririz, AD FS altyapınızın kapatılmadan önce en az 12 saat bekleyin - bu bekleme süresi kullanıcılar Exchange ActiveSync'e geçişi sırasında oturum tutmak olduğunu doğrulamaktır.

## <a name="step-4-test-the-feature"></a>4. adım: Test özelliği

Doğrudan kimlik doğrulama doğru etkinleştirdiğinizden emin doğrulamak için bu yönergeleri izleyin:

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) kiracınız için genel yönetici kimlik bilgilerine sahip.
2. Seçin **Azure Active Directory** sol gezinti çubuğunda.
3. Seçin **Azure AD Connect**.
4. Doğrulayın **doğrudan kimlik doğrulama** özelliği gösterildiğinden **etkin**.
5. Seçin **doğrudan kimlik doğrulama**. Bu dikey kimlik doğrulaması aracılarınızı yüklendiği sunucuları listeler.

![Azure Active Directory Yönetim Merkezi - Azure AD Connect dikey penceresi](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory Yönetim Merkezi - doğrudan kimlik doğrulama dikey penceresi](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

Bu aşamada, etki alanlarındaki kullanıcılar tüm yönetilen kiracınızda doğrudan kimlik doğrulaması kullanarak oturum açabilirsiniz. Ancak, Federasyon etki alanlarındaki kullanıcılar Active Directory Federasyon Hizmetleri (AD FS) ya da daha önce yapılandırdığınız başka bir Federasyon sağlayıcı kullanarak oturum geçin. Yönetilen federe bir etki alanından dönüştürürseniz, o etki alanındaki tüm kullanıcıların geçişli kimlik doğrulaması kullanarak oturum açmayı otomatik olarak başlar. Yalnızca bulut kullanıcıları doğrudan kimlik doğrulama özelliği tarafından etkilenmez.

## <a name="step-5-ensure-high-availability"></a>5. adım: yüksek oranda kullanılabilirlik emin olun

Bir üretim ortamında doğrudan kimlik doğrulama dağıtmayı planlıyorsanız, tek başına bir kimlik doğrulama aracısını yüklemeniz gerekir. Bu ikinci kimlik doğrulama Aracısı bir sunucuya yüklemek _diğer_ bir çalışan Azure AD Connect ve ilk kimlik doğrulama Aracısı daha. Bu kurulum, yüksek kullanılabilirlik, oturum açma isteklerinin sağlar. Tek başına bir kimlik doğrulama Aracısı dağıtmak için bu yönergeleri izleyin:

1. **Kimlik Doğrulama Aracısı'nın en son sürümü karşıdan yüklemek (sürümleri 1.5.193.0 veya üstü)**: oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) , kiracının genel yönetici kimlik bilgilerine sahip.
2. Seçin **Azure Active Directory** sol gezinti çubuğunda.
3. Seçin **Azure AD Connect** ve ardından **doğrudan kimlik doğrulama**. Seçip **Yükleme Aracısı**.
4. Tıklatın **koşullarını kabul & Yükle** düğmesi.
5. **Kimlik Doğrulama Aracısı en son sürümünü yüklemek**: Yukarıdaki adımda indirdiğiniz yürütülebilir dosyayı çalıştırmak. İstendiğinde, kiracının genel Yöneticisi kimlik bilgilerini sağlayın.

![Azure Active Directory Yönetim Merkezi - kimlik doğrulama Aracısı Yükle düğmesi](./media/active-directory-aadconnect-pass-through-authentication/pta9.png)

![Azure Active Directory Yönetim Merkezi - aracıyı indir dikey penceresi](./media/active-directory-aadconnect-pass-through-authentication/pta10.png)

>[!NOTE]
>Kimlik Doğrulama Aracısı'ndan de indirebilirsiniz [burada](https://aka.ms/getauthagent). Gözden geçirin ve kimlik doğrulama aracısının kabul olun [hizmet koşulları](https://aka.ms/authagenteula) _önce_ yükleme.

## <a name="next-steps"></a>Sonraki adımlar
- [**Akıllı kilitleme** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -Kiracı üzerinde yapılandırma akıllı kilitleme özelliğini kullanıcı hesaplarını korumak için.
- [**Geçerli sınırlamalar** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -hangi senaryoları desteklenir ve hangilerinin olmayan öğrenin.
- [**Teknik derinlemesine** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -bu özellik nasıl çalıştığını anlayın.
- [**Sık sorulan sorular** ](active-directory-aadconnect-pass-through-authentication-faq.md) -sık sorulan sorulara yanıtlar.
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -özelliği ile ilgili genel sorunları çözmeyi öğrenin.
- [**Güvenlik derinlemesine** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -özelliği hakkında ek ayrıntılı teknik bilgi.
- [**Azure AD sorunsuz SSO** ](active-directory-aadconnect-sso.md) -tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleri dosyalama için.
