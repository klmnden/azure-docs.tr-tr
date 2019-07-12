---
title: AppSource için Azure Active Directory sertifikası alma | Microsoft Docs
description: Uygulamanızın AppSource için Azure Active Directory sertifikası alma hakkında ayrıntılar.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 21206407-49f8-4c0b-84d1-c25e17cd4183
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/21/2018
ms.author: ryanwi
ms.reviewer: andret
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: cc42ab8a8cfb0d182c69bd0940e23cffdb2be0af
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807243"
---
# <a name="how-to-get-appsource-certified-for-azure-active-directory"></a>Azure Active Directory için AppSource sertifikalı alma

[Microsoft AppSource](https://appsource.microsoft.com/) keşfedin, deneyin ve satır iş kolu SaaS uygulamaları (tek başına SaaS ve mevcut Microsoft SaaS ürünlerine eklenti) yönetmek iş kullanıcıları için bir hedef.

Bağımsız bir SaaS uygulamasında appsource'ta listelemek için uygulamanızı herhangi bir şirket veya Azure Active Directory (Azure AD) sahip kuruluş iş hesaplarından çoklu oturum açmayı kabul etmelisiniz. Oturum açma işlemi kullanmalısınız [Openıd Connect](v1-protocols-openid-connect-code.md) veya [OAuth 2.0](v1-protocols-oauth-code.md) protokoller. AppSource sertifikası için SAML tümleştirme kabul edilmedi.

## <a name="guides-and-code-samples"></a>Kılavuzlar ve kod örnekleri

Tümleştirme hakkında bilgi edinmek istiyorsanız Open ID kullanarak Azure AD uygulamanızla bağlanmak, kılavuzlarımızı izleyin ve kod örneklerinde [Azure Active Directory Geliştirici Kılavuzu](v1-overview.md#get-started "için Azure AD ile çalışmaya başlama geliştiriciler").

## <a name="multi-tenant-applications"></a>Çok kiracılı uygulamalar

A *çok kiracılı uygulama* ayrı örneği, yapılandırma ve dağıtım gerek kalmadan Azure AD'si olan kullanıcıların herhangi bir şirket veya kuruluş oturum açma işlemlerini kabul eden bir uygulamadır. AppSource önerir uygulamaları etkinleştirmek için çok kiracılı uygulama *tek tıklamayla* ücretsiz deneme sürümü deneyimi.

Uygulamanızda çok kiracılı modeli etkinleştirme için bu adımları izleyin:
1. Ayarlama `Multi-Tenanted` özelliğini `Yes` içindeki uygulama kaydı 's bilgileri [Azure portalında](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps). Varsayılan olarak, Azure portalında oluşturulan uygulamaların olarak yapılandırılmış  *[tek kiracılı](#single-tenant-applications)* .
1. İstekler göndermek için kodunuzu güncelleştirin `common` uç noktası. Bunu yapmak için uç noktasından güncelleştirme `https://login.microsoftonline.com/{yourtenant}` için `https://login.microsoftonline.com/common*`.
1. ASP .NET gibi bazı platformlar için de birden çok verenler kabul edecek şekilde güncelleştirmeniz gerekir.

Çok kiracılı modeli hakkında daha fazla bilgi için bkz: [çok kiracılı uygulama desenini kullanarak istediğiniz bir Azure Active Directory (Azure AD) kullanıcısı ile oturum açma](howto-convert-app-to-be-multi-tenant.md).

### <a name="single-tenant-applications"></a>Tek kiracılı uygulamalar

A *tek kiracılı uygulama* yalnızca tanımlı bir Azure kullanıcıları oturum açma işlemleri kabul eden bir uygulama olan AD örneği. Dış kullanıcılar (iş veya Okul hesapları diğer kuruluşlardan ya da kişisel hesaplar dahil) her bir kullanıcı, uygulama kayıtlı Azure AD örneği için bir Konuk hesabı olarak ekledikten sonra tek kiracılı bir uygulama için oturum açabilir. 

Azure ad ile Konuk hesapları olarak kullanıcıları ekleyebilirsiniz [Azure AD B2B işbirliği](../b2b/what-is-b2b.md) ve bunu yapabilirsiniz [program aracılığıyla](../../active-directory-b2c/code-samples.md). B2B kullanırken, kullanıcıların oturum açmak için bir davet gerektirmeyen bir Self Servis portalı oluşturabilirsiniz. Daha fazla bilgi için bkz. [Self Servis portalı için Azure AD B2B işbirliği kaydolma](https://docs.microsoft.com/azure/active-directory/b2b/self-service-portal).

Tek kiracılı uygulamalar etkinleştirebilir *benimle iletişim kurun* karşılaşabilirsiniz, ancak AppSource önerir tek tıklayın/ücretsiz deneme sürümü deneyimi etkinleştirmek istiyorsanız, çok kiracılı, uygulamanızın üzerinde yerine etkinleştirin.

## <a name="appsource-trial-experiences"></a>AppSource deneme deneyimleri

### <a name="free-trial-customer-led-trial-experience"></a>Ücretsiz deneme sürümü (deneme sürümü deneyimi müşteri tarafından yönetilen)

Müşteri destekli deneme sürümü, uygulamanız için bir tek tıklamayla erişim sunar, AppSource önerir deneyimidir. Aşağıdaki örnek, bu deneyimin nasıl göründüğünü gösterir:

<table >
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step1.png" width="85%" alt-text="Shows Free trial for customer-led trial experience"/><ul><li>Kullanıcı, uygulamanızın AppSource Web sitesinde bulur.</li><li>'Ücretsiz deneme sürümü' seçeneğini belirler</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step2.png" width="85%" alt-text="Shows how user is redirected to a URL in your web site"/><ul><li>AppSource, web sitenizde bir URL'ye kullanıcı yönlendirir.</li><li>Web sitesi başlatıldığında <i>çoklu oturum açma</i> işlem otomatik olarak (üzerinde sayfa yükleme)</li></ul></td>
    <td valign="top" width="33%">3.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step3.png" width="85%" alt-text="Shows the Microsoft sign-in page"/><ul><li>Kullanıcı Microsoft oturum açma sayfasına yönlendirilir.</li><li>Kullanıcı oturum açmak için kimlik bilgilerini sağlar.</li></ul></td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step4.png" width="85%" alt-text="Example: Consent page for an application"/><ul><li>Kullanıcı, uygulamanız için izninizi verir.</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%" alt-text="Shows the experience the user sees when redirected back to your site"/><ul><li>Oturum açma tamamlandıktan ve kullanıcı, web sitesine yönlendirilir</li><li>Ücretsiz deneme kullanıcı başlatır</li></ul></td>
    <td></td>
</tr>
</table>

### <a name="contact-me-partner-led-trial-experience"></a>Bana (iş ortağı destekli deneme sürümü deneyimi)

Örneğin, uygulamanızın kullanıcı/şirket--sağlamalıdır sanal makineler, veritabanı örnekleri ve tamamlanması çok uzun süren işlemleri sağlamak olmasını el ile veya uzun süreli bir işlem ihtiyacı olduğunda iş ortağı deneme deneyimini kullanabilir. Bu durumda, kullanıcı seçer sonra **istek deneme** düğme ve bir form doldurur, AppSource, kullanıcının kişi bilgileri gönderir. Bu bilgiler aldığınızda, daha sonra ortamı sağlamak ve deneme sürümü deneyimi nasıl kullanıcı yönergeleri gönderin:<br/><br/>

<table valign="top">
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step1.png" width="85%" alt-text="Shows Contact me for partner-led trial experience"/><ul><li>Kullanıcı, uygulamanızın AppSource web sitesinde bulur.</li><li>'Benimle iletişim kurun' seçeneğini belirler</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step2.png" width="85%" alt-text="Shows an example form with contact info"/><ul><li>İletişim bilgileri ile bir formunu doldurur.</li></ul></td>
     <td valign="top" width="33%">3.<br/><br/>
        <table bgcolor="#f7f7f7">
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/UserContact.png" width="55%" alt-text="Shows placeholder for user information"/></td>
            <td>Kullanıcı bilgilerini alma</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/SetupEnv.png" width="55%" alt-text="Shows placeholder for setup environment info"/></td>
            <td>Ortamı Kurulumu</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/ContactCustomer.png" width="55%" alt-text="Shows placeholder for trial info"/></td>
            <td>Deneme bilgileri ile ilgili kullanıcı</td>
        </tr>
        </table><br/><br/>
        <ul><li>Kullanıcının bilgi ve Kurulum deneme örneği Al</li><li>Kullanıcı uygulamanıza erişmek için köprü Gönder</li></ul>
    </td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step3.png" width="85%" alt-text="Shows the application sign-in screen"/><ul><li>Kullanıcı uygulamanızı erişir ve çoklu oturum açma işlemini tamamlayın</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step4.png" width="85%" alt-text="Shows an example consent page for an application"/><ul><li>Kullanıcı, uygulamanız için izninizi verir.</li></ul></td>
    <td valign="top" width="33%">6.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%" alt-text="Shows the experience the user sees when redirected back to your site"/><ul><li>Oturum açma tamamlandıktan ve kullanıcı, web sitesine yönlendirilir</li><li>Ücretsiz deneme kullanıcı başlatır</li></ul></td>
   
</tr>
</table>

### <a name="more-information"></a>Daha fazla bilgi

AppSource deneme deneyimi hakkında daha fazla bilgi için bkz. [bu videoyu](https://aka.ms/trialexperienceforwebapps). 

## <a name="next-steps"></a>Sonraki Adımlar

- Azure AD oturum açma işlemleri destekleyen uygulamalar derleme hakkında daha fazla bilgi için bkz. [Azure AD için kimlik doğrulama senaryoları](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).
- SaaS uygulamanızı appsource'ta listeleyin hakkında daha fazla bilgi için bkz: Git [AppSource iş ortağı bilgileri](https://appsource.microsoft.com/partners)

## <a name="get-support"></a>Destek alın

Azure AD tümleştirmesi için kullandığımız [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-active-directory+appsource) desteklemek için toplulukla birlikte.

Sorularınızı Stack Overflow sitesinde ilk sormak ve mevcut sorunları birisi önce sorunuzu sormadığını görmek için Gözat öneririz. Sorularınızı ve yorumlarınızı ile etiketlendiğinden emin [ `[azure-active-directory]` ve `[appsource]` ](https://stackoverflow.com/questions/tagged/azure-active-directory+appsource).

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.

<!--Reference style links -->
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]:authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: v1-overview.md
[AAD-Howto-Multitenant-Overview]: howto-convert-app-to-be-multi-tenant.md
[AAD-QuickStart-Web-Apps]: v1-overview.md#get-started

<!--Image references-->
