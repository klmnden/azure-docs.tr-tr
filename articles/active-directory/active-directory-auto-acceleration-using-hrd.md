---
title: "Oturum açma otomatik-hızlandırma giriş bölgesi bulma İlkesi'ni kullanarak bir uygulama için yapılandırma | Microsoft Docs"
description: "Açıklar hangi Azure AD kiracısı olan ve Azure Active Directory üzerinden Azure yönetme."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: it-pro
ms.date: 11/09/2017
ms.author: billmath
<<<<<<< HEAD
ms.openlocfilehash: b1f0e5da09ef1d6acd234b72878cc71d66bb551e
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: HT
=======
ms.openlocfilehash: e2e6e5c40dc4a9f67f94c45f8394512db3f777f5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="configure-sign-in-auto-acceleration-for-an-application-by-using-a-home-realm-discovery-policy"></a>Oturum açma otomatik-hızlandırma bir uygulama için bir giriş bölgesi bulma İlkesi kullanarak yapılandırma

Aşağıdaki belge giriş bölgesi bulma ve otomatik hızlandırma tanıtılmaktadır.

## <a name="home-realm-discovery"></a>Giriş bölgesi bulma
Giriş bölgesi bulma (HRD) Azure Active Directory (belirlemek için Azure AD) izin veren zaman oturum açma, burada bir kullanıcı kimlik doğrulaması yapması gereken işlemidir.  Bir kullanıcı bir kaynağa erişmek için Azure AD kiracısı veya Azure AD genel oturum açma sayfasında oturum açtığında, bunlar bir kullanıcı adı (UPN) yazın. Azure AD, burada oturum açmak kullanıcının gereken bulmak için kullanır. 

Kullanıcı, aşağıdaki konumlardan birine doğrulanması için yapılması gerekebilir:

- Ev Kiracı kullanıcının (kullanıcının erişmeye çalıştığı kaynak olarak aynı Kiracı olabilir). 

- Microsoft hesabı.  Kullanıcı Konuk kaynak kiracısı içinde değil.

- Azure AD kiracısı ile Federasyon başka bir kimlik sağlayıcısı.

-  Active Directory Federasyon Hizmetleri (AD FS) gibi bir şirket içi kimlik sağlayıcısı.

## <a name="auto-acceleration"></a>Otomatik hızlandırma 
Bazı kuruluşlar, kullanıcıların Azure Active Directory Kiracı Kullanıcı kimlik doğrulaması için AD FS gibi başka bir IDP ile birleştirmek için yapılandırın.  

Bu durumlarda, kullanıcı işaretlerini uygulamaya bağlı olarak, bunlar ilk önce ile Azure AD oturum açma sayfası sunulur. UPN Değerlerinin yazdıktan sonra sonra IDP oturum açma sayfasına yönlendirilirsiniz. Belirli koşullar altında Yöneticiler, bunlar belirli uygulamalar için oturum açtığınız zaman, kullanıcıların oturum açma sayfasına doğrudan isteyebilirsiniz. 

Bu, kullanıcılar ilk Azure Active Directory sayfasında atlayabilirsiniz anlamına gelir. Bu işlem "oturum açma otomatik-hızlandırma." olarak adlandırılır

Oturum açma için başka bir IDP için Kiracı burada federe durumlarda otomatik hızlandırma kullanıcının daha verimli oturum açma sağlar.  Tek tek uygulamalar için otomatik ivmesini yapılandırabilirsiniz.

>[!NOTE]
>Bir uygulama için otomatik hızlandırma yapılandırırsanız, Konuk kullanıcılar oturum açamaz. Düz bir Federasyon IDP kimlik doğrulaması için bir kullanıcı izlerseniz, bunlar Azure Active Directory oturum açma sayfasına geri dönmek için için yolu yoktur. Giriş bölgesi bulma adımını atlamanın çünkü diğer kiracılar veya bir Microsoft hesabı gibi bir dış IDP yönlendirilmesi gerekebilir, Konuk kullanıcılar o uygulamaya oturum açamaz.  

Bir Federasyon IDP için otomatik hızlandırma denetlemek için iki yolu vardır:   

- Bir etki alanı ipucu kimlik doğrulama istekleri için bir uygulama kullanın. 
- Otomatik ivmesini etkinleştirmek için bir giriş bölgesi bulma ilkesi yapılandırın.

## <a name="domain-hints"></a>Etki alanı ipuçları 
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
>Bir etki alanı ipucu bir kimlik doğrulama isteğine dahil edilirse, kendi durum uygulama ayarlama herhangi HRD ilkesini geçersiz kılar.

## <a name="home-realm-discovery-policy"></a>Giriş bölgesi bulma İlkesi
Bazı uygulamalar, bunlar yayma kimlik doğrulama isteği yapılandırmak için bir yol sağlamaz. Bu durumlarda, etki alanı ipuçlarını otomatik hızlandırma denetlemek için kullanmak mümkün değil. Otomatik hızlandırma aynı davranışı elde etmek için ilke aracılığıyla yapılandırılabilir.  

### <a name="set-hrd-policy"></a>HRD ilkesini ayarlama
Oturum açma otomatik-hızlandırma bir uygulama ayarı için üç adım vardır:


1. Otomatik hızlandırma için bir HRD ilkesi oluşturma.

2. İlke ekleneceği hizmet ilkesi bulunuyor.

3. İlke hizmet ilkesi ekleniyor. Bir kiracı ilkeleri oluşturulmuş olabilir, ancak bir varlığa eklenen kadar herhangi bir etkisi yok. 

HRD ilke için bir hizmet sorumlusu eklenebilecek ve herhangi bir anda yalnızca bir HRD ilke belirli bir varlık üzerinde etkin olabilir.  

Otomatik hızlandırma ayarlamak için Microsoft Azure Active Directory grafik API'sini doğrudan ya da Azure Active Directory PowerShell cmdlet'lerini kullanabilirsiniz HRD İlkesi'ni kullanarak.

İlke yöneten grafik API'si açıklanan [İlkesi işlemleri](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) MSDN makalesinde.

Bir örnek HRD ilke tanımı aşağıda verilmiştir:
    
 ```
   {  
    "HomeRealmDiscoveryPolicy":
    {  
    "AccelerateToFederatedDomain":true,
    "PreferredDomain":"federated.example.edu"
    }
   }
```

İlke türüdür "HomeRealmDiscoveryPolicy."

Varsa **AccelerateToFederatedDomain** ilkenin hiçbir etkisi false olur.

**PreferredDomain** hızlandırmak için etki alanına belirtmeniz gerekir. Kiracı yalnızca tek bir Federasyon etki alanı varsa atlanabilir.  Atlanır ve var. birden fazla Federasyon etki alanını doğruladıysanız, ilkenin hiçbir etkisi olmaz.

Varsa **PreferredDomain** belirtilirse, Kiracı için doğrulanmış, federe bir etki alanı eşleşen gerekir. Uygulamasının tüm kullanıcıların bu etki alanına oturum açabilir olması gerekir.

### <a name="priority-and-evaluation-of-hrd-policies"></a>Öncelik ve değerlendirme HRD ilkeleri
HRD ilkeleri oluşturulur ve belirli kuruluşlar ve hizmet asıl adı atanmış. Bu, belirli bir uygulama için birden çok ilke mümkün olduğunu gösterir. Etkinleşir HRD ilke bu kurallar aşağıdaki gibidir:


- Bir etki alanı ipucu kimlik doğrulama isteği varsa, herhangi bir HRD ilke göz ardı edilir. Etki alanı ipucu tarafından belirtilen davranışı kullanılır.

- Aksi halde, bir ilke açıkça hizmet sorumlusu atanmışsa zorlanır. 

- Hiçbir etki alanı ipucu yoktur ve hizmet asıl açıkça hiçbir ilke atanmamışsa, hizmet sorumlusu üst kuruluşa açıkça atanmış bir ilke uygulanır. 

- Hiçbir etki alanı ipucu yoktur ve hiçbir ilke hizmet sorumlusu veya kuruluştan bir başkasına atanmış olan varsayılan HRD davranışı kullanılır.

## <a name="tutorial-for-setting-sign-in-auto-acceleration-on-an-application-by-using-an-hrd-policy"></a>Oturum açma otomatik-hızlandırma HRD İlkesi kullanarak bir uygulama ayarı Öğreticisi
Dahil olmak üzere birkaç Senaryoları yol için Azure AD PowerShell cmdlet'leri kullanırız:


- Tek bir Federasyon etki alanına sahip bir kiracı için bir uygulama için otomatik ivmesini ayarlama.

- Kiracınız için doğrulanır birkaç etki alanlarından biri için bir uygulama için otomatik ivmesini ayarlama.

- Bir ilke yapılandırıldığı uygulamaların listesi.

### <a name="prerequisites"></a>Ön koşullar
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

### <a name="example-set-auto-acceleration-for-an-application"></a>Örnek: bir uygulama için otomatik ivmesini ayarlama 
Bu örnekte otomatik-kullanıcıların AD FS oturum açma ekranına bunlar bir uygulamaya oturum açarken hızlandırır bir ilke oluşturun. Kullanıcılar ilk Azure AD oturum açma sayfasında bir kullanıcı adı girmek zorunda kalmadan AD FS oturum açabilir. 

#### <a name="step-1-create-an-hrd-policy"></a>1. adım: bir HRD ilkesi oluşturma
``` powershell
New-AzureADPoly -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true}}") -DisplayName BasicAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```

Uygulamalar için kullanıcıların kimliğini doğrulayan tek bir Federasyon etki alanınız varsa, yalnızca bir HRD İlkesi oluşturmanız gerekir.  

Yeni ilke bakın ve almak için kendi **objectID**, aşağıdaki komutu çalıştırın:

``` powershell
Get-AzureADPolicy
```


HRD ilke aldıktan sonra otomatik ivmesini etkinleştirmek için birden çok uygulama hizmeti ilkeleri atayabilirsiniz.

#### <a name="step-2-locate-the-service-principal-to-which-to-assign-the-policy"></a>2. adım: ilkeyi atamak hizmet asıl bulun  
Gereksinim duyduğunuz **objectID** ilke atamak istediğiniz hizmet sorumluları. Bulmanın birkaç yolu vardır **objectID** hizmet sorumluları.    

Portal kullanabilirsiniz veya sorgulayabilirsiniz [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). Ayrıca gidebilirsiniz [grafik Gezgini aracı](https://graphexplorer.cloudapp.net/) ve hesabınızda oturum açın Azure AD için kuruluşunuzun tüm hizmet asıl adı görmek için. PowerShell kullandığından hizmeti ilkeleri ve kimlikleri listelemek için get-AzureADServicePrincipal cmdlet'ini kullanabilirsiniz.

#### <a name="step-3-assign-the-policy-to-your-service-principal"></a>3. adım: ilke, hizmet sorumlusu atayın.  
Sonra **objectID** otomatik hızlandırma yapılandırmak istediğiniz uygulama hizmet sorumlusu, aşağıdaki komutu çalıştırın. Bu komut, 2. adımda bulduğunuz hizmet asıl 1. adımda oluşturduğunuz HRD İlkesi ilişkilendirir.

``` powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
```

Bu komut ilke eklemek istediğiniz her hizmet sorumlusu için yineleyebilirsiniz.

#### <a name="step-4-check-which-application-service-principals-your-auto-acceleration-policy-is-assigned-to"></a>4. adım: otomatik hızlandırma ilkeniz atandığı hangi uygulama hizmet asıl adı denetleyin
Hangi uygulamaların denetlemek için yapılandırılan otomatik hızlandırma İlkesi yoksa, kullanın **Get-AzureADPolicyAppliedObject** cmdlet'i. Bunu iletirsiniz **objectID** denetlemek istediğiniz ilke.

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
#### <a name="step-5-youre-done"></a>5. adım: Tamamladınız!
Yeni ilke çalıştığını denetlemek için uygulamayı deneyin.

### <a name="example-list-the-applications-for-which-an-auto-acceleration-policy-is-configured"></a>Örnek: bir otomatik hızlandırma İlkesi yapılandırıldığı uygulamaları Listele

#### <a name="step-1-list-all-policies-that-were-created-in-your-organization"></a>1. adım: kuruluşunuzda oluşturulan tüm ilkeleri listesi 

``` powershell
Get-AzureADPolicy
```

Not **objectID** listesi atamaları istediğiniz ilke.

#### <a name="step-2-list-the-service-principals-to-which-the-policy-is-assigned"></a>2. adım: ilke atanan hizmet asıl adı listesi  

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```

### <a name="example-remove-an-auto-acceleration-policy-for-an-application"></a>Örnek: bir uygulama için bir otomatik hızlandırma ilkesini Kaldır
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
- Kimlik doğrulaması Azure AD'de nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure AD için kimlik doğrulama senaryoları](develop/active-directory-authentication-scenarios.md).
- Kullanıcı çoklu oturum açma hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile](active-directory-enterprise-apps-manage-sso.md).
- Ziyaret [Active Directory Geliştirici Kılavuzu](develop/active-directory-developers-guide.md) Geliştirici ilişkili tüm içeriği genel bakış.
