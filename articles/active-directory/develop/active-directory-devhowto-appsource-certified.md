---
title: "Azure Active Directory için sertifikalı AppSource alma | Microsoft Docs"
description: "Uygulamanız Azure Active Directory için sertifikalı AppSource alma hakkında ayrıntılar."
services: active-directory
documentationcenter: 
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 21206407-49f8-4c0b-84d1-c25e17cd4183
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/03/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: ca5105ff8ec45e703a3fb8e0cbb8666eb0b2c38e
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="how-to-get-appsource-certified-for-azure-active-directory"></a>Azure Active Directory AppSource sertifikalı alma
[Microsoft AppSource](https://appsource.microsoft.com/) bulmak, deneyin ve iş kolu satır SaaS uygulamaları (tek başına SaaS ve var olan Microsoft SaaS ürünlerinde eklentiye) yönetmek İşletme kullanıcıları için bir hedef.

Tek başına bir SaaS uygulaması AppSource üzerinde listelemek için uygulamanızın çoklu oturum açma herhangi bir şirket veya Azure Active Directory sahip kuruluş iş hesaplarından kabul etmelisiniz. Oturum açma işlemi kullanmalısınız [Openıd Connect](./active-directory-protocols-openid-connect-code.md) veya [OAuth 2.0](./active-directory-protocols-oauth-code.md) protokoller. SAML tümleştirme AppSource sertifika için kabul edilmedi.

## <a name="guides-and-code-samples"></a>Kılavuzlar ve kod örnekleri
Nasıl tümleştirileceği hakkında bilgi edinmek istiyorsanız Open ID kullanarak Azure Active Directory ile uygulamanızı bağlanın, kılavuzlarımız izleyin ve kod örneklerinde [Azure Active Directory Geliştirici Kılavuzu](./active-directory-developers-guide.md#get-started "Azure ile çalışmaya başlama Geliştiriciler için AD").

## <a name="multi-tenant-applications"></a>Çok kiracılı uygulamalar

Oturum açma işlemleri ayrı örneği, yapılandırma veya dağıtım gerek kalmadan Azure Active Directory sahip kullanıcıların herhangi bir şirket veya kuruluş kabul eden bir uygulama olarak bilinen bir *çok kiracılı uygulama*. AppSource önerir uygulamalarını etkinleştirmek için çok kiracılı uygulamak *tek tıklamayla* ücretsiz deneme sürümü deneyimi.

Bu, uygulamanızın üzerinde çoklu kiracı etkinleştirmek için:
- Ayarlama `Multi-Tenanted` özelliğine `Yes` içindeki uygulama kaydı ait bilgileri [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (varsayılan olarak, Azure Portalı'nda oluşturulan uygulamaların olarak yapılandırılmış *tek Kiracı*)
- İstekler göndermek için kodunuzu güncelleştirin '`common`' uç noktası (uç noktasından güncelleştirme *https://login.microsoftonline.com/ {yourtenant}* için *https://login.microsoftonline.com/common*)
- ASP.NET gibi bazı platformlar için de birden çok verenler kabul etmek için kodunuzu güncelleştirin gerekir

Çoklu kiracı hakkında daha fazla bilgi için bkz: [çok kiracılı uygulama desenini kullanarak herhangi bir Azure Active Directory (AD) kullanıcı oturum nasıl](./active-directory-devhowto-multi-tenant-overview.md).

### <a name="single-tenant-applications"></a>Tek Kiracı uygulamaları
Yalnızca oturum açma işlemleri tanımlı bir Azure Active Directory örneğinin kullanıcılardan kabul uygulamaları olarak bilinen *tek kiracılı uygulama*. Dış kullanıcılar (diğer kuruluşlardan iş veya Okul hesapları veya kişisel hesap dahil) uygulamasında oturum açabilir her bir kullanıcı olarak ekledikten sonra bir tek kiracılı uygulama *Konuk hesabı* Azure Active Directory'ye örneği uygulama kaydedilir. Bir Azure Active Directory Konuk hesapları olarak kullanıcıları ekleyebilirsiniz [ *Azure AD B2B işbirliği* ](../active-directory-b2b-what-is-azure-ad-b2b.md) - ve yapılabilir [programlı şekilde](../active-directory-b2b-code-samples.md). Kullanıcı konuk hesabıyla bir Azure Active Directory'ye eklediğinizde, davet e-posta bağlantısını tıklatarak daveti kabul etmek için sahip kullanıcı, bir davet e-posta gönderilir. Ayrıca iş ortağı kuruluşun üyesi olduğu bir davet kuruluştaki ek bir kullanıcıya gönderilen davetleri oturum açmak için bir daveti kabul etmek için gerekli değildir.

Tek Kiracı uygulamaları etkinleştirebilir *kişi benim* deneyimi, ancak AppSource önerir tek tıklamayla / ücretsiz deneme sürümü deneyimi etkinleştirmek istiyorsanız, bu, uygulamanızın üzerinde çoklu kiracı yerine etkinleştirin.


## <a name="appsource-trial-experiences"></a>AppSource deneme deneyimleri

### <a name="free-trial-customer-led-trial-experience"></a>Ücretsiz deneme (deneme sürümü deneyimi müşteri öncülük) 
*Müşteri öncülük deneme* , uygulamanız için bir tek tıklamalı erişim sunar olarak AppSource önerir deneyimidir. Nasıl bir çizimi bu deneyim şuna benzer:<br/><br/>

<table >
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step1.png" width="85%"/><ul><li>Kullanıcı uygulamanızı AppSource Web sitesini bulur.</li><li>'Ücretsiz deneme sürümü' seçeneğini belirler</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step2.png" width="85%" /><ul><li>Kullanıcı, web sitenizdeki bir URL AppSource yönlendirir</li><li>Web sitesini başlatır <i>çoklu oturum açma</i> işlem otomatik olarak (üzerinde sayfa yükleme)</li></ul></td>
    <td valign="top" width="33%">3.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step3.png" width="85%"/><ul><li>Kullanıcı Microsoft oturum açma sayfasına yeniden yönlendirildiği</li><li>Kullanıcı oturum açmak için kimlik bilgilerini sağlar</li></ul></td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step4.png" width="85%"/><ul><li>Kullanıcı, uygulamanız için izin verir</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Oturum açma işlemi tamamlandıktan ve kullanıcının, web sitesine yönlendirildiği</li><li>Ücretsiz deneme kullanıcı başlatır</li></ul></td>
    <td></td>
</tr>
</table>

### <a name="contact-me-partner-led-trial-experience"></a>Bana (deneme sürümü deneyimi iş ortağı öncülük) başvurun
*Ortak deneme sürümü deneyimi* el ile veya uzun vadeli işlemi kullanıcı sağlamak için yapılması gerektiğinde kullanılabilir / şirket: Örneğin, sanal makine, veritabanı örnekleri ya da işlemleri sağlamak uygulamanız gereken tamamlanması çok zaman alır. Bu durumda, kullanıcı seçer sonra *'İstek deneme'* düğmesi ve form doldurur, AppSource, kullanıcının kişi bilgileri gönderir. Bu bilgileri alır almaz, ardından ortamı sağlamanıza ve kullanıcı deneme sürümü deneyimi erişim hakkında yönergeler gönderin:<br/><br/>

<table valign="top">
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step1.png" width="85%"/><ul><li>Kullanıcı uygulamanızı AppSource web sitesini bulur.</li><li>'Kişi benim' seçeneğini belirler</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step2.png" width="85%"/><ul><li>Form kişi bilgilerle doldurur.</li></ul></td>
     <td valign="top" width="33%">3.<br/><br/>
        <table bgcolor="#f7f7f7">
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/UserContact.png" width="55%"/></td>
            <td>Kullanıcı bilgilerini alma</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/SetupEnv.png" width="55%"/></td>
            <td>Kurulum ortamı</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/ContactCustomer.png" width="55%"/></td>
            <td>Deneme bilgileri ile ilgili kişi kullanıcı</td>
        </tr>
        </table><br/><br/>
        <ul><li>Kullanıcının bilgileri ve Kurulum deneme örnek alma</li><li>Kullanıcının uygulamanıza erişmek için köprü Gönder</li></ul>
    </td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step3.png" width="85%"/><ul><li>Kullanıcı, uygulamanızın erişir ve çoklu oturum açma işlemini tamamlayın</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step4.png" width="85%"/><ul><li>Kullanıcı, uygulamanız için izin verir</li></ul></td>
    <td valign="top" width="33%">6.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Oturum açma işlemi tamamlandıktan ve kullanıcının, web sitesine yönlendirildiği</li><li>Ücretsiz deneme kullanıcı başlatır</li></ul></td>
   
</tr>
</table>

### <a name="more-information"></a>Daha fazla bilgi
AppSource deneme sürümü deneyimi hakkında daha fazla bilgi için bkz: [bu videoyu](https://aka.ms/trialexperienceforwebapps). 
 
## <a name="next-steps"></a>Sonraki Adımlar

- Azure Active Directory oturum açma işlemleri destekleyen uygulamalar oluşturma ile ilgili daha fazla bilgi için bkz: [Azure AD için kimlik doğrulama senaryoları](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios) 

- SaaS uygulamanızda AppSource listesi hakkında daha fazla bilgi için bkz: Git [AppSource iş ortağı bilgileri](https://appsource.microsoft.com/partners)


## <a name="get-support"></a>Destek alın
Azure Active Directory tümleştirme için kullandığımız [yığın taşması](http://stackoverflow.com/questions/tagged/azure-active-directory+appsource) desteği sağlamak üzere topluluğuyla. 

Yığın taşması sorularınızı ilk isteyin ve birisi sorunuzu önce sorup sormadığını görmek için mevcut sorunlar Gözat önerilir. Sorularınızı ve yorumlarınızı ile etiketlenir emin olun [ `[azure-active-directory]` ve `[appsource]` ](http://stackoverflow.com/questions/tagged/azure-active-directory+appsource).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.

<!--Reference style links -->
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]: ./active-directory-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: ./active-directory-developers-guide.md#get-started


<!--Image references-->