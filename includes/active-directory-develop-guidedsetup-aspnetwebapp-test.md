---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: bfdc89d9bc5d5a07c04e857c1a46e4b988c125ab
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36943564"
---
## <a name="test-your-code"></a>Kodunuzu test

Visual Studio'da uygulamanızı test etmek için basın **F5** projeyi çalıştırın. Http:// tarayıcı açar<span></span>localhost: {port} konumu ve bkz **oturum oturum Microsoft** düğmesi. Oturum açma işlemini başlatmak için düğmesini seçin.

Testinizi çalıştırmak hazır olduğunuzda, bir Microsoft Azure Active Directory (Azure AD) hesabı (iş veya Okul hesabı) veya kişisel Microsoft hesabı kullanın (<span>Canlı.</span> com veya <span>outlook.</span> com) oturum açın.

![Microsoft ile oturum açın](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Microsoft hesabınızda oturum açın](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

#### <a name="view-application-results"></a>Uygulama sonuçları görüntüleme
Kullanıcı oturum açtıktan sonra Web sitenizin giriş sayfasına yönlendirilir. Uygulama kayıt bilgilerinizi Microsoft uygulama kayıt Portalı'nda belirtilen HTTPS URL'sini giriş sayfasıdır. Giriş sayfasına Hoş Geldiniz iletisi içeren *"Merhaba \<kullanıcı >,"* bağlantısı oturumu kapatın ve kullanıcının talepleri görüntülemek için bir bağlantı. Kullanıcının talepleri için bağlantı gözatar için *talep* daha önce oluşturduğunuz denetleyicisi.

### <a name="browse-to-see-the-users-claims"></a>Kullanıcının talepleri görmek için Gözat
Kullanıcının talepleri görmek için yalnızca kimliği doğrulanmış kullanıcılar için kullanılabilir denetleyicisi görünümüne gitmek için bağlantıyı seçin.

#### <a name="view-the-claims-results"></a>Talep sonuçları görüntüleme
Denetleyici görünümüne göz sonra kullanıcı için temel özelliklerini içeren bir tablo görmeniz gerekir:

|Özellik |Değer |Açıklama |
|---|---|---|
|**Ad** |Kullanıcının tam adı | Kullanıcı adı ve soyadı.
|**Kullanıcı Adı** |Kullanıcı<span>@domain.com</span> | Kullanıcıyı tanımlamak için kullanılan kullanıcı adı.
|**Konu** |Konu |Kullanıcı web üzerinden benzersiz olarak tanımlayan bir dize.|
|**Kiracı kimliği** |Guid | A **GUID** benzersiz olarak temsil eden kullanıcının Azure AD kuruluş.|

Ayrıca, kimlik doğrulama isteğine olan tüm talepler tablosu görmeniz gerekir. Daha fazla bilgi için bkz: [bir Azure AD kimlik belirtecinde bulunan talepleri listesi](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).


### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>(İsteğe bağlı) bir Authorize özniteliğine sahip bir yöntem test erişimi
Erişim denetleyicisi adsız bir kullanıcı ile korumalı olarak test etmek için `Authorize` özniteliği, şu adımları izleyin:
1. Out kullanıcı oturum açabilir ve oturum kapatma işlemini tamamlamak için bağlantıyı seçin.
2. Tarayıcınızda http:// yazın<span></span>localhost: {port} / ile korunan denetleyicinizi erişmek talepleri `Authorize` özniteliği.

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Korumalı bir denetleyicinin erişimini sonra beklenen sonuçları
Korumalı denetleyicisi görünümü kullanmak için kimlik doğrulaması yapmak istenir.

## <a name="advanced-options"></a>Gelişmiş seçenekler

<!--start-collapse-->
### <a name="protect-your-entire-website"></a>Web sitenizin tamamını koruma
İçindeki tüm Web sitenizin, korunacak **Global.asax** dosya, ekleme `AuthorizeAttribute` özniteliğini `GlobalFilters` filtrelemek `Application_Start` yöntemi:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

### <a name="restrict-who-can-sign-in-to-your-application"></a>Uygulamanıza oturum açabilir kimin kısıtla
Varsayılan olarak bu kılavuzu tarafından oluşturulan uygulamasını derlerken, uygulamanızın (outlook.com, live.com ve diğerleri dahil) kişisel hesaplarıyla oturum açmaları yanı sıra iş kabul edin ve Okul hesapları herhangi bir şirket veya ile tümleşik olan Kuruluş Azure Active Directory. SaaS uygulamaları için önerilen seçenek budur.

Uygulamanız için kullanıcı oturum açma erişimini kısıtlamak için birden çok seçenek kullanılabilir:

#### <a name="option-1-restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>Seçenek 1: uygulamanıza (tek-Kiracı) oturum açmak için yalnızca bir kuruluşun Active Directory örneğinden kullanıcılarını kısıtlayın.

Bu seçenek için ortak bir senaryodur *LOB uygulamaları*: oturum açma işlemleri yalnızca belirli bir Azure Active Directory örneğine ait hesaplarından kabul etmek için uygulamanızın isteyip istemediğinizi (de dahil olmak üzere *konuk hesaplarını*bu örneğinin) aşağıdakileri yapın:

1. İçinde **web.config** dosya, değerini değiştirin `Tenant` parametresinden `Common` Kiracı adına kuruluşun gibi `contoso.onmicrosoft.com`.
2. İçinde [OWIN başlangıç sınıfı](#configure-the-authentication-pipeline)ayarlayın `ValidateIssuer` bağımsız değişkeni `true`.

#### <a name="option-2-restrict-access-to-your-application-to-users-in-a-specific-list-of-organizations"></a>Seçenek 2: uygulamanıza kuruluşların belirli bir listesi ile kullanıcılara erişimi kısıtlama

İzin verilen kuruluşlar listesinde bir Azure AD kuruluşta olan tek kullanıcı hesaplarının oturum açma erişimi kısıtlayabilirsiniz:
1. İçinde [OWIN başlangıç sınıfı](#configure-the-authentication-pipeline)ayarlayın `ValidateIssuer` bağımsız değişkeni `true`.
2. Değerini `ValidIssuers` parametresi izin verilen kuruluşların listesi.

#### <a name="option-3-use-a-custom-method-to-validate-issuers"></a>Seçenek 3: özel bir yöntem verenler doğrulamak için kullanın.
Özel bir yöntem kullanarak verenler doğrulamak için uygulayabileceğiniz **IssuerValidator** parametresi. Bu parametre kullanma hakkında daha fazla bilgi için bilgiyi [tokenvalidationparameters değerini sınıfı](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx) konusuna bakın.

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
