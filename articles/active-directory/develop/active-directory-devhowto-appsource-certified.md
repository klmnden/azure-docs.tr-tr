---
title: AppSource için Azure Active Directory sertifikası alma | Microsoft Docs
description: Uygulamanızın AppSource için Azure Active Directory sertifikası alma hakkında ayrıntılar.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 21206407-49f8-4c0b-84d1-c25e17cd4183
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/03/2017
ms.author: celested
ms.reviewer: andret
ms.custom: aaddev
ms.openlocfilehash: 8b23d99b838449681f83ff2e88bd96ee90502404
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578866"
---
# <a name="how-to-get-appsource-certified-for-azure-active-directory"></a>Azure Active Directory için AppSource sertifikalı alma
[Microsoft AppSource](https://appsource.microsoft.com/) keşfedin, deneyin ve satır iş kolu SaaS uygulamaları (tek başına SaaS ve mevcut Microsoft SaaS ürünlerine eklenti) yönetmek iş kullanıcıları için bir hedef.

Bağımsız bir SaaS uygulamasında appsource'ta listelemek için uygulamanızı herhangi bir şirket veya Azure Active Directory sahip kuruluş iş hesaplarından çoklu oturum açmayı kabul etmelisiniz. Oturum açma işlemi kullanmalısınız [Openıd Connect](v1-protocols-openid-connect-code.md) veya [OAuth 2.0](v1-protocols-oauth-code.md) protokoller. AppSource sertifikası için SAML tümleştirme kabul edilmedi.

## <a name="guides-and-code-samples"></a>Kılavuzlar ve kod örnekleri
Tümleştirme hakkında bilgi edinmek istiyorsanız Open ID kullanarak Azure Active Directory ile uygulamanızı bağlanmak, kılavuzlarımızı izleyin ve kod örneklerinde [Azure Active Directory Geliştirici Kılavuzu](azure-ad-developers-guide.md#get-started "Azure ile çalışmaya başlama Geliştiriciler için AD").

## <a name="multi-tenant-applications"></a>Çok kiracılı uygulamalar

Oturum açma işlemleri ayrı örneği, yapılandırma ve dağıtım gerek kalmadan Azure Active Directory sahip kullanıcılar herhangi bir şirket veya kuruluş kabul eden bir uygulama olarak bilinen bir *çok kiracılı uygulama*. AppSource önerir uygulamaları etkinleştirmek için çok kiracılı uygulama *tek tıklamayla* ücretsiz deneme sürümü deneyimi.

Uygulamanızda çok kiracılı modeli etkinleştirme için:
- Ayarlama `Multi-Tenanted` özelliğini `Yes` içindeki uygulama kaydı 's bilgileri [Azure portalı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (varsayılan olarak, Azure portalında oluşturulan uygulamaların olarak yapılandırılmış *tek kiracılı*)
- İstekler göndermek için kodunuzu güncelleştirin '`common`' uç noktası (uç noktasından güncelleştirme *https://login.microsoftonline.com/{yourtenant}* için *https://login.microsoftonline.com/common*)
- ASP.NET gibi bazı platformlar için de birden çok verenler kabul etmek için kodunuzu güncelleştirmeniz gerekir

Çok kiracılı modeli hakkında daha fazla bilgi için bkz: [çok kiracılı uygulama desenini kullanarak istediğiniz bir Azure Active Directory (AD) kullanıcısı ile oturum açma](howto-convert-app-to-be-multi-tenant.md).

### <a name="single-tenant-applications"></a>Tek kiracılı uygulamalar
Yalnızca kullanıcı tanımlı bir Azure Active Directory örneği oturum açma işlemleri kabul edin. uygulamalar olarak bilinir *tek kiracılı uygulama*. Dış kullanıcıları (diğer kuruluşlardan iş veya Okul hesapları veya kişisel hesabı dahil) uygulamasında oturum açabilir her bir kullanıcı olarak ekledikten sonra tek kiracılı bir uygulama *Konuk hesabı* , Azure Active Directory örneği uygulama kaydedilir. Azure Active Directory Konuk hesapları olarak kullanıcıları ekleyebilirsiniz [ *Azure AD B2B işbirliği* ](../b2b/what-is-b2b.md) - ve onu yapılabilir [programlı şekilde](../b2b/code-samples.md). Bir Azure Active Directory'ye Konuk hesabı olarak bir kullanıcı eklediğinizde, davet e-postadaki bağlantıya tıklayarak daveti kabul etmek için olan kullanıcı, davet e-posta gönderilir. Ayrıca iş ortağı kuruluşun üyesi olan bir davet eden kuruluştaki ek bir kullanıcıya gönderilen davet oturum açmak için bir daveti kabul etmek için gerekli değildir.

Tek kiracılı uygulamalar etkinleştirebilir *benimle iletişim kurun* karşılaşabilirsiniz, ancak AppSource önerir tek tıklamayla / ücretsiz deneme sürümü deneyimi etkinleştirmek istiyorsanız, çok kiracılı, uygulamanızın üzerinde yerine etkinleştirin.


## <a name="appsource-trial-experiences"></a>AppSource deneme deneyimleri

### <a name="free-trial-customer-led-trial-experience"></a>Ücretsiz deneme sürümü (deneme sürümü deneyimi müşteri tarafından yönetilen) 
*Müşteri destekli deneme sürümü* uygulamanız için bir tek tıklamayla erişim sunar, AppSource önerir deneyimidir. Nasıl bir gösterimi bu deneyim şu şekilde görünür:<br/><br/>

<table >
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step1.png" width="85%"/><ul><li>Kullanıcı, uygulamanızın AppSource Web sitesinde bulur.</li><li>'Ücretsiz deneme sürümü' seçeneğini belirler</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step2.png" width="85%" /><ul><li>AppSource, web sitenizde bir URL'ye kullanıcı yönlendirir.</li><li>Web sitesi başlatıldığında <i>çoklu oturum açma</i> işlem otomatik olarak (üzerinde sayfa yükleme)</li></ul></td>
    <td valign="top" width="33%">3.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step3.png" width="85%"/><ul><li>Kullanıcı Microsoft oturum açma sayfasına yönlendirilir.</li><li>Kullanıcı oturum açmak için kimlik bilgilerini sağlar.</li></ul></td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step4.png" width="85%"/><ul><li>Kullanıcı, uygulamanız için izninizi verir.</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Oturum açma tamamlandıktan ve kullanıcı, web sitesine yönlendirilir</li><li>Ücretsiz deneme kullanıcı başlatır</li></ul></td>
    <td></td>
</tr>
</table>

### <a name="contact-me-partner-led-trial-experience"></a>Bana (iş ortağı destekli deneme sürümü deneyimi)
*Deneme sürümü deneyimi iş ortağı* el ile veya uzun süreli bir işlemi kullanıcının sağlamak için yapılması gerektiğinde kullanılabilir / company: sanal makineler, veritabanı örnekleri veya işlemleri sağlamak, örneğin, uygulamanızın ihtiyaç duyduğu çok fazla zaman alır. Bu durumda, kullanıcı seçer sonra *'İstek deneme'* düğme ve bir form doldurur, AppSource, kullanıcının kişi bilgileri gönderir. Bu bilgiler alır almaz, daha sonra ortamı sağlamak ve deneme sürümü deneyimi nasıl kullanıcı yönergeleri gönderin:<br/><br/>

<table valign="top">
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step1.png" width="85%"/><ul><li>Kullanıcı, uygulamanızın AppSource web sitesinde bulur.</li><li>'Benimle iletişim kurun' seçeneğini belirler</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step2.png" width="85%"/><ul><li>İletişim bilgileri ile bir formunu doldurur.</li></ul></td>
     <td valign="top" width="33%">3.<br/><br/>
        <table bgcolor="#f7f7f7">
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/UserContact.png" width="55%"/></td>
            <td>Kullanıcı bilgilerini alma</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/SetupEnv.png" width="55%"/></td>
            <td>Ortamı Kurulumu</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/ContactCustomer.png" width="55%"/></td>
            <td>Deneme bilgileri ile ilgili kullanıcı</td>
        </tr>
        </table><br/><br/>
        <ul><li>Kullanıcının bilgi ve Kurulum deneme örneği Al</li><li>Kullanıcı uygulamanıza erişmek için köprü Gönder</li></ul>
    </td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step3.png" width="85%"/><ul><li>Kullanıcı uygulamanızı erişir ve çoklu oturum açma işlemini tamamlayın</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step4.png" width="85%"/><ul><li>Kullanıcı, uygulamanız için izninizi verir.</li></ul></td>
    <td valign="top" width="33%">6.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Oturum açma tamamlandıktan ve kullanıcı, web sitesine yönlendirilir</li><li>Ücretsiz deneme kullanıcı başlatır</li></ul></td>
   
</tr>
</table>

### <a name="more-information"></a>Daha fazla bilgi
AppSource deneme deneyimi hakkında daha fazla bilgi için bkz. [bu videoyu](https://aka.ms/trialexperienceforwebapps). 
 
## <a name="next-steps"></a>Sonraki Adımlar

- Azure Active Directory oturum açma işlemleri destekleyen uygulamalar derleme hakkında daha fazla bilgi için bkz. [Azure AD için kimlik doğrulama senaryoları](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios) 

- SaaS uygulamanızı appsource'ta listeleyin hakkında daha fazla bilgi için bkz: Git [AppSource iş ortağı bilgileri](https://appsource.microsoft.com/partners)


## <a name="get-support"></a>Destek alın
Azure Active Directory Tümleştirmesi için kullandığımız [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory+appsource) desteklemek için toplulukla birlikte. 

Sorularınızı Stack Overflow sitesinde ilk sormak ve mevcut sorunları birisi önce sorunuzu sormadığını görmek için Gözat öneririz. Sorularınızı ve yorumlarınızı ile etiketlendiğinden emin [ `[azure-active-directory]` ve `[appsource]` ](http://stackoverflow.com/questions/tagged/azure-active-directory+appsource).

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.

<!--Reference style links -->
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]:authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: azure-ad-developers-guide.md
[AAD-Howto-Multitenant-Overview]: howto-convert-app-to-be-multi-tenant.md
[AAD-QuickStart-Web-Apps]: azure-ad-developers-guide.md#get-started


<!--Image references-->