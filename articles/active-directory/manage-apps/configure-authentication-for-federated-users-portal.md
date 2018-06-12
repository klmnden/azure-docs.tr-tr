---
title: Oturum açma otomatik-hızlandırma giriş bölgesi bulma İlkesi'ni kullanarak bir uygulama için yapılandırma | Microsoft Docs
description: Açıklar hangi Azure AD kiracısı olan ve Azure Active Directory üzerinden Azure yönetme.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: it-pro
ms.date: 06/08/2018
ms.author: barbkess
ms.openlocfilehash: 86b7d8aa9d1c0d770f7b145377f49c5dc2c694a2
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35303915"
---
# <a name="configure-azure-active-directory-sign-in-behavior-for-an-application-by-using-a-home-realm-discovery-policy"></a>Giriş bölgesi bulma İlkesi kullanarak bir uygulama için davranış Azure Active Directory oturum açma yapılandırın

Aşağıdaki belge Federasyon kullanıcıları için Azure Active Directory kimlik doğrulama davranışını yapılandırma için bir giriş sağlar.   Federasyon etki alanlarındaki kullanıcılar için otomatik hızlandırma ve kimlik doğrulama kısıtlamaları yapılandırılmasını kapsar.

## <a name="home-realm-discovery"></a>Giriş bölgesi bulma
Giriş bölgesi bulma (HRD) oturum açma zaman Azure Active Directory (burada bir kullanıcı kimlik doğrulaması yapması gereken belirlemek için Azure AD) izin veren işlemidir.  Bir kullanıcı bir kaynağa erişmek için Azure AD kiracısı veya Azure AD genel oturum açma sayfasında oturum açtığında, bunlar bir kullanıcı adı (UPN) yazın. Azure AD, burada oturum açmak kullanıcının gereken bulmak için kullanır. 

Kullanıcı, aşağıdaki konumlardan birine doğrulanması için yapılması gerekebilir:

- Ev Kiracı kullanıcının (kullanıcının erişmeye çalıştığı kaynak olarak aynı Kiracı olabilir). 

- Microsoft hesabı.  Kullanıcı Konuk kaynak kiracısı içinde değil.

-  Active Directory Federasyon Hizmetleri (AD FS) gibi bir şirket içi kimlik sağlayıcısı.

- Azure AD kiracısı ile Federasyon başka bir kimlik sağlayıcısı.

## <a name="auto-acceleration"></a>Otomatik hızlandırma 
Bazı kuruluşlar kendi Azure Active Directory Kiracı Kullanıcı kimlik doğrulaması için AD FS gibi başka bir IDP ile birleştirmek için etki alanlarını yapılandırın.  

Bir kullanıcı bir uygulamaya oturum açtığında, ilk ile Azure AD oturum açma sayfası sunulur. Bir Federasyon etki alanında olmaları durumunda kullanıcıların UPN yazdıktan sonra bu etki alanına hizmet bunlar ardından IDP oturum açma sayfasına yönlendirilirsiniz. Belirli koşullar altında Yöneticiler, bunlar belirli uygulamalar için oturum açtığınız zaman, kullanıcıların oturum açma sayfasına doğrudan isteyebilirsiniz. 

Sonuç olarak kullanıcılar ilk Azure Active Directory sayfasında atlayabilirsiniz. Bu işlem "oturum açma otomatik-hızlandırma." olarak adlandırılır

Oturum açma için başka bir IDP için Kiracı burada federe durumlarda otomatik hızlandırma kullanıcının daha verimli oturum açma sağlar.  Tek tek uygulamalar için otomatik ivmesini yapılandırabilirsiniz.

>[!NOTE]
>Bir uygulama için otomatik hızlandırma yapılandırırsanız, Konuk kullanıcılar oturum açamaz. Düz bir Federasyon IDP kimlik doğrulaması için bir kullanıcı izlerseniz, bunlar Azure Active Directory oturum açma sayfasına geri dönmek için için yolu yoktur. Giriş bölgesi bulma adımını atlamanın çünkü diğer kiracılar veya bir Microsoft hesabı gibi bir dış IDP yönlendirilmesi gerekebilir, Konuk kullanıcılar o uygulamaya oturum açamaz.  

Bir Federasyon IDP için otomatik hızlandırma denetlemek için iki yolu vardır:   

- Bir etki alanı ipucu kimlik doğrulama istekleri için bir uygulama kullanın. 
- Otomatik ivmesini etkinleştirmek için bir giriş bölgesi bulma ilkesi yapılandırın.

### <a name="domain-hints"></a>Etki alanı ipuçları    
Etki alanı ipuçları uygulamadan kimlik doğrulama isteği içinde yer alan yönergeleri. Kullanıcı kendi Federasyon IDP oturum açma sayfasına hızlandırmak için kullanılabilir. Ya da çok kiracılı uygulama tarafından kullanıcı düz markalı hızlandırmak için kullanılabilmesi için kendi Kiracı için Azure AD oturum açma sayfası.  

Örneğin, "largeapp.com" uygulama "contoso.largeapp.com." özel bir URL, uygulamaya erişmek müşterilerine etkinleştirmek Uygulama kimlik doğrulama isteğine contoso.com etki alanı ipucu içeriyor olabilir. 

Etki alanı ipucu sözdizimi kullanılan protokol bağlı olarak değişir ve genellikle uygulama içinde yapılandırılır.

**WS-Federasyon**: Sorgu dizesinde whr=contoso.com.

**SAML**: içeren bir etki alanı ipucu veya bir sorgu dizesi whr=contoso.com ya da bir SAML kimlik doğrulama isteği.

**Açık ID Connect**: bir sorgu dizesi domain_hint=contoso.com. 

Bir etki alanı ipucu uygulama kimlik doğrulama isteğini dahildir ve Kiracı o etki alanı ile Federasyon, Azure AD etki alanında yapılandırılmış IDP yönlendirileceği oturum açma çalışır. 

Doğrulanmış bir Federasyon etki alanına etki alanı ipucu başvurmadığından, göz ardı edilir ve normal giriş bölgesi bulma çağrılır.

Azure Active Directory tarafından desteklenen etki alanı ipuçlarını kullanarak otomatik hızlandırma hakkında daha fazla bilgi için bkz: [Enterprise Mobility + Security blog](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/11/using-azure-ad-to-land-users-on-their-custom-login-page-from-within-your-app/).

>[!NOTE]
>Bir etki alanı ipucu bir kimlik doğrulama isteğine dahil edilirse, kendi durum HRD İlkesi uygulama ayarlanan otomatik hızlandırma geçersiz kılar.

### <a name="home-realm-discovery-policy-for-auto-acceleration"></a>Otomatik hızlandırma için giriş bölgesi bulma İlkesi
Bazı uygulamalar, bunlar yayma kimlik doğrulama isteği yapılandırmak için bir yol sağlamaz. Bu durumlarda, etki alanı ipuçlarını otomatik hızlandırma denetlemek için kullanmak mümkün değil. Otomatik hızlandırma aynı davranışı elde etmek için ilke aracılığıyla yapılandırılabilir.  

## <a name="enable-direct-authentication-for-legacy-applications"></a>Eski uygulamalar için doğrudan kimlik doğrulamasını etkinleştir
Uygulamaların kullanıcıların kimlik doğrulaması için AAD kitaplıkları ve etkileşimli oturum açma kullanmak için en iyi uygulamadır. Kitaplıklar Federasyon kullanıcısı akışları dikkatli olun.  Bazen eski uygulamaları Federasyon anlamak için yazılmış değil. Bunlar giriş bölgesi bulmayı gerçekleştirme ve bir kullanıcının kimliğini doğrulamak için doğru Federasyon uç nokta ile etkileşim değil. İsterseniz, doğrudan Azure Active Directory ile kimlik doğrulaması için kullanıcı adı/parola kimlik gönderme belirli eski uygulamaları etkinleştirmek için HRD İlkesi'ni kullanabilirsiniz. Parola karma eşitlemesi etkinleştirilmiş olması gerekir. 

> [!IMPORTANT]
> Yalnızca doğrudan kimlik doğrulaması parola karması eşitlemesi açık olduğunda varsa ve bu uygulama, şirket içi IDP tarafından uygulanan tüm ilkeler olmadan kimlik doğrulaması uygundur bildiğiniz etkinleştirin. Parola karma eşitlemesi devre dışı bırakma ya da herhangi bir nedenle AD Connect dizin eşitlemesi kapatmak, eski parola karması kullanarak doğrudan kimlik doğrulama olasılığını önlemek için bu ilkeyi kaldırmanız gerekir.

## <a name="set-hrd-policy"></a>HRD ilkesini ayarlama
Federe oturum açma otomatik-hızlandırma için bir uygulama veya doğrudan bulut tabanlı uygulamalar ayarı HRD ilkesi için üç adım vardır:

1. HRD ilkesi oluşturun.

2. Hizmet sorumlusu İlkesi ekleneceği bulun.

3. Hizmet sorumlusu ilkesi ekleyin. 

Bir hizmet sorumlusu eklendiğinde ilkelerini yalnızca belirli bir uygulama için geçerli olur. 

Yalnızca bir HRD İlkesi herhangi bir anda bir hizmet sorumlusu etkin olabilir.  

Oluşturup HRD ilkesini yönetmek için Microsoft Azure Active Directory grafik API'sini doğrudan ya da Azure Active Directory PowerShell cmdlet'lerini kullanabilirsiniz.

İlke yöneten grafik API'si açıklanan [İlkesi işlemleri](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) MSDN makalesinde.

Bir örnek HRD ilke tanımı aşağıda verilmiştir:
    
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

İlke türüdür "HomeRealmDiscoveryPolicy."

**AccelerateToFederatedDomain** isteğe bağlıdır. Varsa **AccelerateToFederatedDomain** ilkenin hiçbir etkisi otomatik hızlandırma false olur. Varsa **AccelerateToFederatedDomain** tek doğrulanır ve Kiracı sonra kullanıcılar Federasyon etki alanına alınır düz oturum açmak için Federasyon IDP için doğru ve var olan. True ise ve Kiracı içinde birden fazla doğrulanan etki alanı ise **PreferredDomain** belirtilmesi gerekir.

**PreferredDomain** isteğe bağlıdır. **PreferredDomain** hızlandırmak için etki alanına belirtmeniz gerekir. Kiracı yalnızca tek bir Federasyon etki alanı varsa atlanabilir.  Atlanır ve var. birden fazla Federasyon etki alanını doğruladıysanız, ilkenin hiçbir etkisi olmaz.

 Varsa **PreferredDomain** belirtilirse, Kiracı için doğrulanmış, federe bir etki alanı eşleşen gerekir. Uygulamasının tüm kullanıcıların bu etki alanına oturum açabilir olması gerekir.

**AllowCloudPasswordValidation** isteğe bağlıdır. Varsa **AllowCloudPasswordValidation** kullanıcı adı/parola kimlik bilgilerini doğrudan Azure Active Directory token son nokta sunarak bir Federasyon kullanıcısı kimlik doğrulaması için verilen uygulama sonra geçerlidir. Bu, yalnızca parola karması eşitlemesi etkinleştirildiğinde çalışır.

### <a name="priority-and-evaluation-of-hrd-policies"></a>Öncelik ve değerlendirme HRD ilkeleri
HRD ilkeleri oluşturulur ve belirli kuruluşlar ve hizmet asıl adı atanmış. Bu, belirli bir uygulama için birden çok ilke mümkün olduğunu gösterir. Etkinleşir HRD ilke bu kurallar aşağıdaki gibidir:


- Bir etki alanı ipucu kimlik doğrulama isteği varsa, herhangi bir HRD ilkesi için otomatik ivmesini yoksayılır. Etki alanı ipucu tarafından belirtilen davranışı kullanılır.

- Aksi halde, bir ilke açıkça hizmet sorumlusu atanmışsa zorlanır. 

- Hiçbir etki alanı ipucu yoktur ve hizmet asıl açıkça hiçbir ilke atanmamışsa, hizmet sorumlusu üst kuruluşa açıkça atanmış bir ilke uygulanır. 

- Hiçbir etki alanı ipucu yoktur ve hiçbir ilke hizmet sorumlusu veya kuruluştan bir başkasına atanmış olan varsayılan HRD davranışı kullanılır.

## <a name="tutorial-for-setting-hrd-policy-on-an-application"></a>HRD ilke bir uygulama ayarı için Öğreticisi 
Dahil olmak üzere birkaç Senaryoları yol için Azure AD PowerShell cmdlet'leri kullanırız:


- Bir uygulamada tek bir Federasyon etki alanına sahip bir kiracı için otomatik ivmesini yapmak için HRD ilkesini ayarlama.

- Kiracınız için doğrulanır birkaç etki alanlarından biri için bir uygulama için otomatik ivmesini yapmak için HRD ilkesini ayarlama.

- Kullanıcı adı/parola kimlik doğrulaması Azure Active Directory Federasyon kullanıcısı için yönlendirmek eski bir uygulamayı etkinleştirmek için daha fazla ayar HRD ilkesi.

- Bir ilke yapılandırıldığı uygulamaların listesi.


### <a name="prerequisites"></a>Önkoşullar
Aşağıdaki örneklerde, oluştur, Güncelleştir, bağlantı ve Azure AD'de uygulama hizmet asıl adı ilkelerini Sil.

1.  Başlamak için en son Azure AD PowerShell cmdlet Önizleme indirin. 

2.  Azure AD PowerShell cmdlet'leri indirdikten sonra Azure AD Yönetici hesabınızla oturum açmak için Bağlan komutu çalıştırın:

    ``` powershell
    Connect-AzureAD -Confirm
    ```
3.  Kuruluşunuzdaki tüm ilkeleri görmek için aşağıdaki komutu çalıştırın:

    ``` powershell
    Get-AzureADPolicy
    ```

Hiçbir şey döndürülmezse, kiracınızda oluşturulan hiçbir ilkelerine sahip anlamına gelir.

### <a name="example-set-hrd-policy-for-an-application"></a>Örnek: Set HRD ilke bir uygulama için 

Bir uygulamayı ya da atandığında, bu örnekte, bir ilke, oluşturun: 
- Otomatik-kullanıcıların AD FS oturum açma ekranına kiracınızda tek bir etki alanında olduğunda, bir uygulama için oturum açtığınız zaman hızlandırır. 
- Bir AD FS oturum açma ekranına var. otomatik hızlandırır kullanıcı birden fazla Federasyon etki alanını kiracınızda sayısıdır.
- Federe kullanıcılar için ilke atandığı uygulamaları için Azure Active Directory için doğrudan etkileşimli olmayan kullanıcı adı/parola oturum açma sağlar.

#### <a name="step-1-create-an-hrd-policy"></a>1. adım: bir HRD ilkesi oluşturma

Aşağıdaki ilke otomatik-kullanıcıların AD FS oturum açma ekranına kiracınızda tek bir etki alanında olduğunda, bir uygulama için oturum açtığınız zaman hızlandırır.

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true}}") -DisplayName BasicAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```
Aşağıdaki ilke otomatik-kullanıcıların bir Federasyon etki alanını kiracınızda fazla var olan bir AD FS oturum açma ekranını hızlandırır. Uygulamalar için kullanıcıların kimliğini doğrulayan birden fazla Federasyon etki alanınız varsa, otomatik hızlandırmak için etki alanı belirtmeniz.

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true, "PreferredDomain":"federated.example.edu"}}") -DisplayName MultiDomainAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```

Federasyon kullanıcıları için kullanıcı adı/parola kimlik doğrulaması doğrudan belirli uygulamalar için Azure Active Directory ile etkinleştirmek için bir ilke oluşturmak için aşağıdaki komutu çalıştırın:

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AllowCloudPasswordValidation`":true}}") -DisplayName EnableDirectAuthPolicy -Type HomeRealmDiscoveryPolicy
```


Yeni ilke bakın ve almak için kendi **objectID**, aşağıdaki komutu çalıştırın:

``` powershell
Get-AzureADPolicy
```


Oluşturduktan sonra HRD ilkeyi uygulamak için birden çok uygulama hizmet sorumluları atayabilirsiniz.

#### <a name="step-2-locate-the-service-principal-to-which-to-assign-the-policy"></a>2. adım: ilkeyi atamak hizmet asıl bulun  
Gereksinim duyduğunuz **objectID** ilke atamak istediğiniz hizmet sorumluları. Bulmanın birkaç yolu vardır **objectID** hizmet sorumluları.    

Portal kullanabilirsiniz veya sorgulayabilirsiniz [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). Ayrıca gidebilirsiniz [grafik Gezgini aracı](https://graphexplorer.cloudapp.net/) ve hesabınızda oturum açın Azure AD için kuruluşunuzun tüm hizmet asıl adı görmek için. PowerShell kullandığından, hizmet asıl adı ve onların kimliklerini listelemek için get-AzureADServicePrincipal cmdlet'ini kullanabilirsiniz.

#### <a name="step-3-assign-the-policy-to-your-service-principal"></a>3. adım: ilke, hizmet sorumlusu atayın.  
Sonra **objectID** otomatik hızlandırma yapılandırmak istediğiniz uygulama hizmet sorumlusu, aşağıdaki komutu çalıştırın. Bu komut, 2. adımda bulduğunuz hizmet asıl 1. adımda oluşturduğunuz HRD İlkesi ilişkilendirir.

``` powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
```

Bu komut ilke eklemek istediğiniz her hizmet sorumlusu için yineleyebilirsiniz.

Bir uygulama zaten atanmış bir HomeRealmDiscovery İlkesi sahip olduğu durumda, ikinci bir tane eklemek mümkün olmayacaktır.  Bu durumda, ek parametreler eklemek için uygulamaya atanan giriş bölgesi bulma İlkesi tanımını değiştirin.

#### <a name="step-4-check-which-application-service-principals-your-hrd-policy-is-assigned-to"></a>4. adım: HRD ilkeniz atandığı hangi uygulama hizmet asıl adı denetleyin
Hangi uygulamaların denetlemek için yapılandırılan HRD İlkesi yoksa, kullanın **Get-AzureADPolicyAppliedObject** cmdlet'i. Bunu iletirsiniz **objectID** denetlemek istediğiniz ilke.

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
#### <a name="step-5-youre-done"></a>5. adım: Tamamladınız!
Yeni ilke çalıştığını denetlemek için uygulamayı deneyin.

### <a name="example-list-the-applications-for-which-hrd-policy-is-configured"></a>Örnek: ilke hangi HRD için yapılandırılmış uygulamaların listesi

#### <a name="step-1-list-all-policies-that-were-created-in-your-organization"></a>1. adım: kuruluşunuzda oluşturulan tüm ilkeleri listesi 

``` powershell
Get-AzureADPolicy
```

Not **objectID** listesi atamaları istediğiniz ilke.

#### <a name="step-2-list-the-service-principals-to-which-the-policy-is-assigned"></a>2. adım: ilke atanan hizmet asıl adı listesi  

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```

### <a name="example-remove-an-hrd-policy-for-an-application"></a>Örnek: bir uygulama için bir HRD ilkesini Kaldır
#### <a name="step-1-get-the-objectid"></a>1. adım: objectID alın
Önceki örnekte almak için kullanın **objectID** ilke ve istediğiniz kaldırmak, uygulama hizmet sorumlusu. 

#### <a name="step-2-remove-the-policy-assignment-from-the-application-service-principal"></a>2. adım: uygulama hizmet sorumlusu ilke atamasını Kaldır  

``` powershell
Remove-AzureADApplicationPolicy -ObjectId <ObjectId of the Service Principal>  -PolicyId <ObjectId of the policy>
```

#### <a name="step-3-check-removal-by-listing-the-service-principals-to-which-the-policy-is-assigned"></a>3. adım: ilke atanan hizmet asıl adı listeleyerek kaldırma denetleyin. 

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
## <a name="next-steps"></a>Sonraki adımlar
- Kimlik doğrulaması Azure AD'de nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure AD için kimlik doğrulama senaryoları](../develop/active-directory-authentication-scenarios.md).
- Kullanıcı çoklu oturum açma hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile](configure-single-sign-on-portal.md).
- Ziyaret [Active Directory Geliştirici Kılavuzu](../develop/active-directory-developers-guide.md) Geliştirici ilişkili tüm içeriği genel bakış.
