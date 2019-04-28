---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 8795c9ab0a4dbb76327d0ead48ed33fb0cff9e86
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60298248"
---
## <a name="test-your-code"></a>Kodunuzu test etme

Uygulamanızı Visual Studio'da Test etmek için basın **F5** projeyi çalıştırın. Http:// tarayıcı açılır<span></span>localhost: {port} konumu göreceksiniz **Microsoft'ta oturum açma** düğmesi. Oturum açma işlemini başlatmak için düğmeyi seçin.

Testinizi çalıştırmaya hazır olduğunuzda, bir Microsoft Azure Active Directory (Azure AD) hesabı (iş veya Okul hesabı) veya kişisel Microsoft hesabı kullanın (<span>Canlı.</span> com veya <span>outlook.</span> com) oturum açın.

![Microsoft'ta oturum açma](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Microsoft hesabınızda oturum açın](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

#### <a name="view-application-results"></a>Uygulama sonuçlarını görüntüle

Oturum açtıktan sonra kullanıcı, Web sitesinin giriş sayfasına yönlendirilir. Uygulama kayıt bilgilerinizi Microsoft uygulama kayıt Portalı'nda belirtilen HTTPS URL'sini giriş sayfasıdır. Giriş sayfasına Hoş Geldiniz iletisi içeren *"Hello \<kullanıcı >,"* oturumu kapatmak için kullanıcı talepleri görüntülemek için bir bağlantı ve. Kullanıcı talepleri için bağlantı gözatar için *talep* daha önce oluşturduğunuz denetleyicisi.

### <a name="browse-to-see-the-users-claims"></a>Kullanıcı talepleri görmek için Gözat

Kullanıcı talepleri görmek için yalnızca kimliği doğrulanmış kullanıcılar için kullanılabilir olan denetleyici görünüme göz atmak için bağlantıyı seçin.

#### <a name="view-the-claims-results"></a>Talep sonuçlarını görüntüleme

Denetleyici görünüme Gözat sonra kullanıcı için temel özelliklerini içeren bir tablo görmeniz gerekir:

|Özellik |Değer |Açıklama |
|---|---|---|
|**Ad** |Kullanıcının tam adı | Kullanıcı adı ve soyadı.
|**Kullanıcı Adı** |Kullanıcı<span>@domain.com</span> | Kullanıcıyı tanımlamak için kullanılan kullanıcı adı.
|**Konu** |Subject |Kullanıcı Web'de benzersiz olarak tanımlayan bir dize.|
|**Kiracı kimliği** |Guid | A **GUID** , benzersiz kullanıcının Azure AD kuruluşu temsil eder.|

Ayrıca, kimlik doğrulama isteğine olan tüm talep tablosu görmeniz gerekir. Daha fazla bilgi için [olan bir Azure AD kimlik belirteci talepler listesinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).

### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>(İsteğe bağlı) bir Authorize özniteliği olan bir yönteme erişimi test etme

Erişim ile bir denetleyici için anonim kullanıcı protected olarak test etmek için `Authorize` öznitelik, aşağıdaki adımları izleyin:

1. Kullanıcının oturumunu kapatıp oturum kapatma işlemini tamamlamak için bağlantıyı seçin.
2. Tarayıcınızda http:// yazın<span></span>localhost: {port} / denetleyicinizi ile korunan erişmek talepleri `Authorize` özniteliği.

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Korumalı bir denetleyicinin erişimini sonra beklenen sonuçları

Korumalı denetleyici görünümü kullanmak için kimlik doğrulaması istenir.

## <a name="advanced-options"></a>Gelişmiş seçenekler

<!--start-collapse-->
### <a name="protect-your-entire-website"></a>Web sitenizin tamamını koruyun
İçinde tüm Web sitenizin, korunacak **Global.asax** ekleyin `AuthorizeAttribute` özniteliğini `GlobalFilters` filtrelemek `Application_Start` yöntemi:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

### <a name="restrict-who-can-sign-in-to-your-application"></a>Kısıtlama kimin uygulamanıza oturum açabilirsiniz

Varsayılan olarak bu kılavuzu tarafından oluşturulan uygulama oluşturduğunuzda uygulamanız yanı sıra iş oturum açma işlemleri kişisel hesabı (outlook.com, live.com ve diğerleri dahil) kabul edin ve Okul hesapları herhangi bir şirket veya kuruluş ile tümleştirilmiştir Azure Active Directory. Bu, SaaS uygulamaları için önerilen bir seçenektir.

Uygulamanızın oturum açma kullanıcı erişimini kısıtlamak için birden çok seçenek kullanılabilir:

#### <a name="option-1-restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>1. seçenek: Uygulamanızı yalnızca bir kuruluşun Active Directory örneğindeki kullanıcıların oturum açabileceği şekilde kısıtlama (tek kiracılı)

Bu seçenek için sık karşılaşılan bir senaryodur *LOB uygulamaları*: Oturum açma işlemleri yalnızca belirli bir Azure Active Directory örneğine ait hesapları kabul etmek için uygulamanızı isterseniz (dahil olmak üzere *konuk hesaplarını* bu örneğinin) aşağıdakileri yapın:

1. İçinde **web.config** dosya, değerini `Tenant` parametresinden `Common` kuruluşun Kiracı adı gibi `contoso.onmicrosoft.com`.
2. İçinde [OWIN başlangıç sınıfı](#configure-the-authentication-pipeline)ayarlayın `ValidateIssuer` bağımsız değişkeni `true`.

#### <a name="option-2-restrict-access-to-your-application-to-users-in-a-specific-list-of-organizations"></a>2. seçenek: Kuruluşların belirli bir listedeki kullanıcılar, uygulama erişimi kısıtlama

İzin verilen kuruluşların listede bir Azure AD kuruluş yalnızca kullanıcı hesapları için oturum açma erişimi kısıtlayabilirsiniz:
1. İçinde [OWIN başlangıç sınıfı](#configure-the-authentication-pipeline)ayarlayın `ValidateIssuer` bağımsız değişkeni `true`.
2. Değerini `ValidIssuers` parametresi izin verilen kuruluşların listesi.

#### <a name="option-3-use-a-custom-method-to-validate-issuers"></a>Seçenek 3: Verenler doğrulamak için özel bir yöntem kullanın.

Verenler kullanarak doğrulamak için özel bir yöntem uygulayabilirsiniz **IssuerValidator** parametresi. Bu parametre kullanımı hakkında daha fazla bilgi için okuyun [tokenvalidationparameters değerini sınıfı](/previous-versions/visualstudio/dn464192(v=vs.114)).
