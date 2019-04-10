---
title: Oturum açma için otomatik hızlandırmayı bir giriş bölgesi bulma ilke kullanan bir uygulama için yapılandırma | Microsoft Docs
description: Azure Active Directory kimlik doğrulaması için otomatik hızlandırmayı ve etki alanı ipuçları da dahil olmak üzere, Federasyon kullanıcıları için giriş bölgesi bulma ilke yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: celested
ms.custom: seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: d82ccf7c2983051597ff634117be81311c4c78a9
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59360942"
---
# <a name="configure-azure-active-directory-sign-in-behavior-for-an-application-by-using-a-home-realm-discovery-policy"></a>Bir giriş bölgesi bulma ilke kullanarak Azure Active Directory oturum davranışı bir uygulama için yapılandırma

Bu makalede, Federasyon kullanıcıları için Azure Active Directory kimlik doğrulama davranışı yapılandırmak için bir giriş sağlar. Federasyon etki alanlarındaki kullanıcılar için otomatik hızlandırmayı ve kimlik doğrulama kısıtlamaları yapılandırılmasını kapsar.

## <a name="home-realm-discovery"></a>Giriş Bölgesi Bulma
Giriş bölgesi bulma (HRD) oturum açma zaman Azure Active Directory'de (bir kullanıcının kimliğini doğrulamak gereken yere belirlemek için Azure AD) sağlayan işlemidir.  Bunlar, bir kullanıcı bir kaynağa erişmek için Azure AD kiracısı veya Azure AD genel oturum açma sayfasında oturum açtığında, kullanıcı adı (UPN) yazın. Azure AD, kullanıcının oturum açmak gereken yere bulmak için kullanır. 

Kullanıcı, aşağıdaki konumlardan birine doğrulanmasını alınması gerekebilir:

- Giriş Kiracı kullanıcının (kullanıcının erişmeye çalıştığı kaynak olarak aynı kiracıda olabilir). 

- Microsoft hesabı.  Kaynak kiracıya bir konuk kullanıcıdır.

-  Active Directory Federasyon Hizmetleri (AD FS) gibi bir şirket içi kimlik sağlayıcısı.

- Azure AD kiracınız ile Federasyon başka bir kimlik sağlayıcısı.

## <a name="auto-acceleration"></a>Otomatik hızlandırma 
Bazı kuruluşlar, Azure Active Directory kiracısındaki başka bir IDP, örneğin, kullanıcı kimlik doğrulaması için AD FS ile federasyona eklemek için etki alanları yapılandırın.  

Kullanıcı bir uygulamayı açtığında, onlara ilk ile Azure AD oturum açma sayfası gösterilir. Bir Federasyon etki alanında olmaları durumunda, UPN yazdıktan sonra etki alanı hizmet bunlar ardından Idp'nin oturum açma sayfasına yönlendirilirsiniz. Belirli koşullar altında Yöneticiler, bunlar belirli uygulamalar için oturum açtığınız zaman, kullanıcıları oturum açma sayfasına yönlendirmek isteyebilirsiniz. 

Sonuç olarak kullanıcılar, ilk Azure Active Directory sayfasında atlayabilirsiniz. Bu işlem "oturum açma için otomatik hızlandırmayı." olarak adlandırılır

Kiracı için oturum açma için başka bir IDP burada Federasyon durumlarda, otomatik hızlandırmayı kullanıcı daha rahat bir oturum açma sağlar.  Tek tek uygulamalar için otomatik hızlandırmayı yapılandırabilirsiniz.

>[!NOTE]
>Bir uygulama için otomatik hızlandırmayı yapılandırma, Konuk kullanıcılar oturum açamaz. Düz bir Federasyon IDP kimlik doğrulaması için bir kullanıcı alırsa, bunları Azure Active Directory oturum açma sayfasına geri dönmek için hiçbir yolu yoktur. Giriş bölgesi bulma adımı atlanıyor çünkü diğer kiracılar veya bir Microsoft hesabı gibi bir dış IDP yönlendirilebilir gerekebilir, Konuk kullanıcılar o uygulamaya oturum açamazsınız.  

Bir Federasyon IDP için otomatik hızlandırmayı denetlemek için iki yolu vardır:   

- Bir etki alanı ipucu kimlik doğrulama istekleri için bir uygulama kullanın. 
- Otomatik hızlandırmayı etkinleştirmek için giriş bölgesi bulma ilkesini yapılandırın.

### <a name="domain-hints"></a>Etki alanı ipuçları    
Etki alanı ipuçları bir uygulamadan kimlik doğrulama isteği dahil edilen yönergeleridir. Kullanıcının kendi Federasyon Idp'nin oturum açma sayfasına hızlandırmak için kullanılabilir. Veya çok kiracılı bir uygulama tarafından kullanıcı doğrudan markalı hızlandırmak için kullanılabilmesi için kendi Kiracı için Azure AD oturum açma sayfası.  

Örneğin, "largeapp.com" uygulama "contoso.largeapp.com." özel bir URL, uygulamaya erişmek, müşterilerine etkinleştirebileceğiniz Uygulama, kimlik doğrulama isteğine contoso.com etki alanı ipucu de içerebilir. 

Etki alanı ipucu sözdizimi kullanılan protokole bağlı olarak değişir ve genellikle uygulamada yapılandırılır.

**WS-Federation**: Sorgu dizesinde whr=contoso.com.

**SAML**:  Bir etki alanı ipucu veya bir sorgu dizesi whr=contoso.com içeren ya da bir SAML kimlik doğrulama isteği.

**Open ID Connect**: Bir sorgu dizesi domain_hint=contoso.com. 

Bir etki alanı ipucu uygulamadaki kimlik doğrulama isteği dahildir ve kiracının bu etki alanı ile birleştirilmiş, Azure AD oturum açma yeniden yönlendirme etki alanında yapılandırılmış IDP için çalışır. 

Doğrulanmış bir Federasyon etki alanına etki alanı ipucu başvurmadığından, göz ardı edilir ve normal giriş bölgesi bulma çağrılır.

Azure Active Directory tarafından desteklenen etki alanı ipuçlarını kullanarak otomatik hızlandırmayı hakkında daha fazla bilgi için bkz: [Enterprise Mobility + Security blogu](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/11/using-azure-ad-to-land-users-on-their-custom-login-page-from-within-your-app/).

>[!NOTE]
>Bir etki alanı ipucu bir kimlik doğrulama isteğine dahil edilirse, kendi durum HRD İlkesi'nde uygulama için ayarlanan otomatik hızlandırmayı geçersiz kılar.

### <a name="home-realm-discovery-policy-for-auto-acceleration"></a>Otomatik hızlandırmayı için giriş bölgesi bulma İlkesi
Bazı uygulamalar, kimlik doğrulama isteği yaydıkları yapılandırmak için bir yol sağlamaz. Bu durumlarda, etki alanı ipuçlarını otomatik hızlandırmayı denetlemek için kullanmak mümkün değildir. Otomatik hızlandırmayı aynı davranışı elde etmek için ilke aracılığıyla yapılandırılabilir.  

## <a name="enable-direct-authentication-for-legacy-applications"></a>Eski uygulamalar için doğrudan kimlik doğrulamayı etkinleştirme
Uygulamalar için kullanıcıların kimliğini doğrulamak için AAD kitaplıkları ve etkileşimli oturum açma kullanmak en iyi uygulamadır. Kitaplıklar birleştirilmiş kullanıcı akışlarını ilgileniriz.  Bazen eski uygulamaları için Federasyon anlamak için yazılmış değildir. Bunlar, giriş bölgesi bulmayı gerçekleştirme ve bir kullanıcının kimliğini doğrulamak için doğru bir Federasyon uç noktası ile etkileşimde bulunmazlar. İsterseniz, doğrudan Azure Active Directory ile kimlik doğrulaması için kullanıcı adı/parola kimlik gönderme belirli eski uygulamaları etkinleştirmek için HRD İlkesi'ni kullanabilirsiniz. Parola karma eşitlemesi etkinleştirilmiş olmalıdır. 

> [!IMPORTANT]
> Yalnızca doğrudan kimlik doğrulaması, parola karma eşitlemesi etkinleştirilmiş olması ve bu uygulama, şirket içi IDP tarafından uygulanan tüm ilkeleri olmadan kimlik doğrulaması uygundur biliyorsanız etkinleştirin. Parola Karması eşitleme devre dışı etkinleştirmek ya da herhangi bir nedenle AD Connect ile dizin eşitlemesi kapatmak, doğrudan kimlik doğrulaması eski parola karması kullanma olasılığını önlemek için bu ilkeyi kaldırmanız gerekir.

## <a name="set-hrd-policy"></a>HRD İlkesi ayarlama
Bir uygulama için Federasyon oturum açma için otomatik hızlandırmayı veya doğrudan bulut tabanlı uygulama ayarı HRD ilkesi için üç adım vardır:

1. HRD ilkesi oluşturun.

2. İlke eklemek hizmet sorumlusu bulun.

3. Hizmet sorumlusuna ilke ekleme. 

Bir hizmet sorumlusu eklendiğinde ilkeleri, yalnızca belirli bir uygulama için geçerli olur. 

Yalnızca bir HRD İlkesi herhangi bir zamanda hizmet sorumlusu üzerinde etkin olabilir.  

HRD ilkesi oluşturma ve yönetme için Microsoft Azure Active Directory Graph API'sini doğrudan veya Azure Active Directory PowerShell cmdlet'lerini kullanabilirsiniz.

İlke yöneten Graph API'sini açıklanan [ilke işlemleri](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) MSDN makalesi.

HRD İlkesi tanımı bir örnek aşağıda verilmiştir:
    
 ```
   {  
    "HomeRealmDiscoveryPolicy":
    {  
    "AccelerateToFederatedDomain":true,
    "PreferredDomain":"federated.example.edu",
    "AllowCloudPasswordValidation":true
    }
   }
```

İlke türü olduğundan "HomeRealmDiscoveryPolicy."

**AccelerateToFederatedDomain** isteğe bağlıdır. Varsa **AccelerateToFederatedDomain** ilkesi için otomatik hızlandırmayı üzerinde hiçbir etkisi yanlış. Varsa **AccelerateToFederatedDomain** tek doğrulanır ve Federasyon etki alanında Kiracı, sonra kullanıcıların alınır düz Federasyon Idp'nin oturum açma için doğru ve var olan. True ise ve bir kiracıda birden fazla doğrulanan etki alanı varsa **PreferredDomain** belirtilmesi gerekir.

**PreferredDomain** isteğe bağlıdır. **PreferredDomain** hangi hızlandırmak bir etki alanı belirtmeniz gerekir. Kiracı yalnızca bir Federasyon etki alanı varsa atlanabilir.  Atlanır ve var. birden fazla Federasyon etki alanını doğruladıysanız, ilkeyi bir etkisi yoktur.

 Varsa **PreferredDomain** belirtilirse, kiracınız için doğrulanmış, Federasyon etki alanı eşleşen gerekir. Uygulamanın tüm kullanıcılar bu etki alanında oturum açamıyor olması gerekir.

**AllowCloudPasswordValidation** isteğe bağlıdır. Varsa **AllowCloudPasswordValidation** sonra uygulamanın bir Federasyon kullanıcısı kullanıcı adı/parola kimlik bilgilerini doğrudan Azure Active Directory belirteç uç noktası sunarak kimliğini doğrulamak için izin verilen geçerlidir. Bu, yalnızca parola karması eşitleme etkinleştirildiğinde çalışır.

### <a name="priority-and-evaluation-of-hrd-policies"></a>Öncelik ve değerlendirme HRD ilkeleri
HRD ilkeleri oluşturulabilir ve ardından belirli kuruluşlar ve hizmet sorumluları atanmış. Bu, belirli bir uygulama için birden çok ilke için mümkün olduğu anlamına gelir. Etkinleşir HRD İlkesi bu kurallara uygun olmalıdır:


- Ardından kimlik doğrulama isteğine bir etki alanı ipucu varsa, herhangi bir HRD ilkesi için otomatik hızlandırmayı göz ardı edilir. Etki alanı ipucu tarafından belirtilen davranışı kullanılır.

- Aksi takdirde, hizmet sorumlusuna açıkça bir ilke atanırsa, zorlanır. 

- Hiçbir etki alanı ipucu yoktur ve hiçbir ilke açıkça hizmet sorumlusuna atanmış, hizmet sorumlusu üst kuruluşa açıkça atanan bir ilke uygulanmaz. 

- Hiçbir etki alanı ipucu yoktur ve hiçbir ilke hizmet sorumlusu veya kuruluş için atanmış olan varsayılan HRD davranışı kullanılır.

## <a name="tutorial-for-setting-hrd-policy-on-an-application"></a>Öğretici, bir uygulama HRD İlkesi ayarlamak için 
Dahil olmak üzere birkaç senaryolar üzerinden yürütmek için Azure AD PowerShell cmdlet'leri kullanacağız:


- Bir uygulamada tek bir Federasyon etki alanına sahip bir kiracı için otomatik hızlandırmayı yapmak için HRD İlkesi ayarlama.

- Kiracınız için otomatik hızlandırmayı doğrulanan birden fazla etki alanlarından biri bir uygulamaya yapmak için HRD İlkesi ayarlama.

- Kullanıcı adı/parola kimlik doğrulaması Azure Active Directory Federasyon kullanıcısı için yönlendirmek eski bir uygulamayı etkinleştirmek için HRD İlkesi ayarlama.

- Bir ilke yapılandırıldığı uygulamaların listesi.


### <a name="prerequisites"></a>Önkoşullar
Aşağıdaki örneklerde, oluşturma, güncelleştirme, bağlamak ve Azure AD'de uygulama hizmet sorumlularını ilkeleri silin.

1.  Başlamak için en son Azure AD PowerShell cmdlet'ini önizlemeyi indirin. 

2.  Azure AD PowerShell cmdlet'leri indirdikten sonra Azure AD Yönetici hesabınızla oturum açmak için Connect komutu çalıştırın:

    ``` powershell
    Connect-AzureAD -Confirm
    ```
3.  Kuruluşunuzdaki tüm ilkeleri görmek için aşağıdaki komutu çalıştırın:

    ``` powershell
    Get-AzureADPolicy
    ```

Hiçbir şey döndürülmezse, kiracınızda oluşturulan ilkeniz yok anlamına gelir.

### <a name="example-set-hrd-policy-for-an-application"></a>Örnek: Bir uygulama için HRD İlkesi ayarlama 

Bir uygulamaya ya da atandığında, bu örnekte, bir ilke oluşturun: 
- Otomatik-kullanıcılar için AD FS oturum açma ekranı kiracınızda tek bir etki alanı olduğunda bunların bir uygulamada oturum açarken zaman hızlandırır. 
- Kiracınızda birden fazla Federasyon etki alanı kullanıcılara bir AD FS oturum açma ekranı var. otomatik hızlandırır var.
- Etkileşimli olmayan kullanıcı adı/parola oturum açma ilkesi için atanan uygulamaları için Federasyon kullanıcıları için Azure Active Directory için doğrudan sağlar.

#### <a name="step-1-create-an-hrd-policy"></a>1. Adım: HRD ilkesi oluşturma

Aşağıdaki ilke otomatik-kullanıcılar için AD FS oturum açma ekranı kiracınızda tek bir etki alanı olduğunda bunların bir uygulamada oturum açarken zaman hızlandırır.

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true}}") -DisplayName BasicAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```
Aşağıdaki ilke otomatik-kullanıcıların kiracınızdaki bir Federasyon etki alanı'daha fazla var olan bir AD FS oturum açma ekranı hızlandırır. Birden fazla uygulamalar için kullanıcıların kimliğini doğrulayan Federasyon etki alanı varsa, otomatik hızlandırmak için etki alanı belirtmeniz gerekir.

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true, `"PreferredDomain`":`"federated.example.edu`"}}") -DisplayName MultiDomainAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```

Federe kullanıcılar için kullanıcı adı/parola kimlik doğrulamasını doğrudan belirli uygulamalar için Azure Active Directory ile etkinleştirmek için bir ilke oluşturmak için aşağıdaki komutu çalıştırın:

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AllowCloudPasswordValidation`":true}}") -DisplayName EnableDirectAuthPolicy -Type HomeRealmDiscoveryPolicy
```


Yeni ilkeniz görmek ve almak için kendi **objectID**, aşağıdaki komutu çalıştırın:

``` powershell
Get-AzureADPolicy
```


Oluşturduktan sonra HRD ilkesi uygulamak için birden çok uygulama hizmet sorumlularını atayabilirsiniz.

#### <a name="step-2-locate-the-service-principal-to-which-to-assign-the-policy"></a>2. Adım: İlkeyi atamak hizmet sorumlusu bulun  
Gereksinim duyduğunuz **objectID** ilkeyi atamak istediğiniz hizmet sorumlularını. Bulmak için birkaç şekilde **objectID** hizmet sorumluları.    

Portalı kullanabilirsiniz veya sorgulayabilirsiniz [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). Ayrıca gidebilirsiniz [Graph Gezgini aracını](https://developer.microsoft.com/graph/graph-explorer) ve oturum açma için Azure AD hesabınız kuruluşunuzun tüm hizmet sorumlularını görmek için. PowerShell kullanıldığı için hizmet sorumlularını ve kimliklerini listelemek için get-azureadserviceprincipal cmdlet'ini cmdlet'ini kullanabilirsiniz.

#### <a name="step-3-assign-the-policy-to-your-service-principal"></a>3. adım: Hizmet sorumlunuzu ilke atama  
Sonra **objectID** otomatik hızlandırmayı yapılandırmak istediğiniz uygulama hizmet sorumlusu, aşağıdaki komutu çalıştırın. Bu komut, 2. adımda bulduğunuz hizmet sorumlusu ile 1. adımda oluşturduğunuz HRD İlkesi'ni ilişkilendirir.

``` powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
```

İlkeyi eklemek istediğiniz her bir hizmet sorumlusu için bu komutu yineleyebilirsiniz.

Bir uygulama zaten bir HomeRealmDiscovery ilkesi atanmamış olduğu durumda, ikinci bir eklemek mümkün olmayacaktır.  Bu durumda, ek parametreler eklemek için uygulamaya atanmış giriş bölgesi bulma ilke tanımını değiştirin.

#### <a name="step-4-check-which-application-service-principals-your-hrd-policy-is-assigned-to"></a>4. Adım: HRD ilkenizi atandığı hangi uygulama hizmet sorumlularını denetleyin
Hangi uygulamaların denetleneceği yapılandırılmış HRD ilkesi varsa, kullanmak **Get-AzureADPolicyAppliedObject** cmdlet'i. Geçirin **objectID** denetlemek istediğiniz ilke.

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
#### <a name="step-5-youre-done"></a>5. Adım: Hazırsınız!
Yeni ilke çalışıp çalışmadığını denetlemek için uygulamanın deneyin.

### <a name="example-list-the-applications-for-which-hrd-policy-is-configured"></a>Örnek: İçin hangi HRD İlkesi yapılandırılmış uygulamaların listesi

#### <a name="step-1-list-all-policies-that-were-created-in-your-organization"></a>1. Adım: Kuruluşunuzda oluşturulan tüm ilkeleri listesi 

``` powershell
Get-AzureADPolicy
```

Not **objectID** atamalarını listelemek için istediğiniz ilke.

#### <a name="step-2-list-the-service-principals-to-which-the-policy-is-assigned"></a>2. Adım: İlkenin atandığı hizmet sorumlularını listelemek  

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```

### <a name="example-remove-an-hrd-policy-for-an-application"></a>Örnek: Bir uygulama için bir HRD İlkesi Kaldır
#### <a name="step-1-get-the-objectid"></a>1. Adım: ObjectID alın
Önceki örnekte almak için kullanın **objectID** ilke ve istediğiniz kaldırmak, uygulama/hizmet sorumlusu. 

#### <a name="step-2-remove-the-policy-assignment-from-the-application-service-principal"></a>2. Adım: Uygulama hizmet sorumlusundan ilke atamasını kaldırın  

``` powershell
Remove-AzureADApplicationPolicy -ObjectId <ObjectId of the Service Principal>  -PolicyId <ObjectId of the policy>
```

#### <a name="step-3-check-removal-by-listing-the-service-principals-to-which-the-policy-is-assigned"></a>3. adım: Temizleme İlkesi atandığı hizmet sorumlularını listeleyerek kontrol edin. 

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
## <a name="next-steps"></a>Sonraki adımlar
- Kimlik doğrulaması Azure AD'de nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure AD için kimlik doğrulama senaryoları](../develop/authentication-scenarios.md).
- Kullanıcı çoklu oturum açma hakkında daha fazla bilgi için bkz: [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma](configure-single-sign-on-portal.md).
- Ziyaret [Active Directory Geliştirici Kılavuzu](../develop/v1-overview.md) Geliştirici ile ilgili tüm içeriği genel bakış.
