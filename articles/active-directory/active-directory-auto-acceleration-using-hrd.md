---
title: "Oturum açma otomatik-hızlandırma giriş bölgesi bulma İlkesi'ni kullanarak bir uygulama için yapılandırma | Microsoft Docs"
description: "Azure AD kiracısının ne olduğu ve Azure'ın, Azure Active Directory üzerinden nasıl yönetileceği açıklanmaktadır."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: it-pro
ms.date: 11/09/2017
ms.author: billmath
ms.openlocfilehash: b1f0e5da09ef1d6acd234b72878cc71d66bb551e
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="configure-sign-in-auto-acceleration-for-an-application-using-home-realm-discovery-hrd-policy"></a>Oturum açma otomatik-hızlandırma giriş bölgesi bulma (HRD) İlkesi'ni kullanarak bir uygulama için yapılandırma

## <a name="introduction-to-home-realm-discovery-and-auto-acceleration"></a>Giriş bölgesi bulma ve otomatik hızlandırma giriş
Aşağıdaki belge giriş bölgesi bulma ve otomatik hızlandırma tanıtılmaktadır.

### <a name="home-realm-discovery"></a>Giriş bölgesi bulma
Giriş bölgesi bulma olduğu bir kullanıcı kimlik doğrulaması yapması gereken, oturum açma aynı anda belirlemek Azure Active Directory veren işlemidir.  Bir kaynak veya Azure AD genel oturum açma sayfasına erişmek için Azure AD kiracısı için oturum açarken kullanıcı bir kullanıcı adı (UPN) yazar.  Azure AD, burada oturum açmak kullanıcının gereken bulmak için kullanır.   Kullanıcı, aşağıdakilerden birini doğrulanması için yapılması gerekebilir:

- Ev Kiracı kullanıcının (kullanıcı kaynak erişmeye çalışıyor gibi aynı Kiracı olabilir). 
- Microsoft hesabı.   Bir konuk kaynak Kiracı kullanıcıdır
- Başka bir kimlik sağlayıcısı ile Azure AD kiracısı federe.  Örneğin, AD FS şirket içi kimlik sağlayıcısı örneği için.

### <a name="auto-acceleration"></a>Otomatik hızlandırma 
Bazı kuruluşlar, kullanıcıların Azure Active Directory Kiracı Kullanıcı kimlik doğrulaması için AD FS gibi başka bir IDP ile birleştirmek için yapılandırın.  Bu durumlarda, bir uygulamaya imzalarken bir Azure AD oturum açma sayfası ve ardından IDP oturum açma sayfasına gerçekleştirilecek UPN Değerlerinin yazdınız sonra kullanıcı ilk sunulur.  Burada mantıklıdır durumlarda yöneticiler belirli uygulamalar için oturum açarken doğrudan oturum açma sayfasına yönlendirilmiş kullanıcıların ilk Azure Active Directory sayfasında atlanıyor isteyebilirsiniz. Bu, "oturum açma otomatik-hızlandırma" denir.

Oturum açma için başka bir IDP için Kiracı burada federe durumlarda otomatik hızlandırma etkinleştirme kullanıcı burada herkes oturum açma tarafından bu IDP doğrulanabilir bildiğiniz durumlarda daha verimli oturum açma yapar.  Tek tek uygulamalar için otomatik ivmesini yapılandırabilirsiniz.

>[!NOTE]
>Bir uygulama için otomatik hızlandırma yapılandırırsanız, Konuk kullanıcılar oturum açma olamaz. Olduğundan Azure Active Directory oturum açma sayfasına geri dönmek için hiçbir şekilde düz bir Federasyon IDP kimlik doğrulaması için kullanıcı alma tek yönlü Sokak aynıdır.  Diğer kiracılar ya da Microsoft hesabı gibi bir dış IDP kimliğinin doğrulanmasını yönlendirilmesi gerekebilir, Konuk kullanıcılar giriş bölgesi bulma adımı atlandı olarak bu uygulamada oturum açmak mümkün olmayacaktır.  

Bir Federasyon IDP için otomatik hızlandırma denetlemek için iki yolu vardır.  Ya da:   

- Bir etki alanı ipucu kimlik doğrulama isteklerini bir uygulama için kullanın. 
- Otomatik ivmesini etkinleştirmek üzere HomeRealmDiscovery ilkesini yapılandırma

## <a name="domain-hints"></a>Etki alanı ipuçları 
Etki alanı ipuçları yönergeleri uygulamadan kimlik doğrulama isteği bulunan.  Kullanıcı kendi Federasyon IDP oturum açma sayfasına hızlandırmak için kullanılabilir ya da çok kiracılı uygulama tarafından kullanıcı düz markalı hızlandırmak için kullanılabilmesi için kendi Kiracı için Azure AD oturum açma sayfası.  Örneğin, bir uygulama largeapp.com müşterilerine özel bir URL contoso.largeapp.com uygulamayı erişmek etkinleştirebileceğiniz ve Contoso.com etki alanı ipucu kimlik doğrulama isteğine dahil olabilir. Etki alanı ipucu sözdizimi kullanılan protokole bağlı olarak değişir ve genellikle bir uygulama içinde yapılandırılır.

**WS-Federasyon**: Sorgu dizesinde whr=contoso.com

**SAML**: etki alanı ipucu ya da bir sorgu dizesi whr=contoso.com içeren ya da bir SAML kimlik doğrulama isteği

**Açık ID Connect**: bir sorgu dizesi domain_hint=contoso.com 

Bir etki alanı ipucu uygulama kimlik doğrulama isteğini dahildir ve Kiracı o etki alanı ile Federasyon, Azure AD etki alanında yapılandırılmış IDP yönlendirileceği oturum açma çalışır.  Doğrulanmış bir Federasyon etki alanına etki alanı ipucu başvurmadığından, göz ardı edilir ve normal giriş bölgesi bulmayı çağrılır.

Bu bkz [blog gönderisi](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/11/using-azure-ad-to-land-users-on-their-custom-login-page-from-within-your-app/) otomatik hızlandırma Azure Active Directory tarafından desteklenen etki alanı ipuçlarını kullanma hakkında daha fazla bilgi için.

>[!NOTE]
>Bir etki alanı ipucu bir kimlik doğrulama isteğine dahil edilirse, varlığı uygulama ayarlama herhangi HRD ilkesini geçersiz kılar.

## <a name="home-realm-discovery-hrd-policy"></a>Giriş bölgesi bulma (HRD) ilkesi
Bazı uygulamalar, bunlar yayma kimlik doğrulama isteği yapılandırmak için bir yöntem sağlamaz, ve bu gibi durumlarda otomatik hızlandırma denetlemek için etki alanı ipuçları kullanmayı mümkün değildir.   Otomatik hızlandırma aynı davranışı elde etmek için ilke aracılığıyla yapılandırılabilir.  

### <a name="setting-hrd-policy"></a>HRD ilkesini ayarlama
Oturum açma otomatik-hızlandırma bir uygulama ayarı için üç adım vardır


1. Otomatik hızlandırma için bir HRD ilkesi oluşturma
2. İlke ekleneceği hizmet ilkesi bulma
3. İlke hizmet ilkesi ekleniyor.  Bir kiracı ilkeleri oluşturulmuş olabilir ancak bir varlığa eklenen kadar herhangi bir etkisi bulunmuyor.  HRD ilke için bir hizmet sorumlusu eklenebilecek ve herhangi bir anda yalnızca bir HRD ilke belirli bir varlık üzerinde etkin olabilir.  

Otomatik hızlandırma ayarlamak için Microsoft Azure Active Directory grafik API'sini doğrudan ya da Azure Active Directory PowerShell cmdlet'lerini kullanabilirsiniz HRD İlkesi'ni kullanma

İlke yöneten grafik API'si açıklanan [burada](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations).

Örnek HRD ilke tanımı aşağıda verilmiştir:
    
 ```
   {  
    "HomeRealmDiscoveryPolicy":
    {  
    "AccelerateToFederatedDomain":true,
    "PreferredDomain":"federated.example.edu"
    }
   }
```

"HomeRealmDiscoveryPolicy" ilke türüdür.

Varsa **AccelerateToFederatedDomain** ilkenin hiçbir etkisi olmaz sonra yanlış **PreferredDomain** için hızlandırmak için bir etki alanı belirtmeniz gerekir ve bir ve yalnızca bir Federasyon Kiracı gerekmesi durumunda atlanabilir etki alanı.  Atlanır ve birden fazla Federasyon etki alanını doğruladıysanız yoksa ilkenin hiçbir etkisi olmaz.

Varsa **PreferredDomain** belirtilen Kiracı için doğrulanmış, federe bir etki alanı eşleşmelidir ve söz konusu uygulamanın tüm kullanıcılarının bu etki alanında oturum açabilir.

### <a name="priority-and-evaluation-of-hrd-policies"></a>Öncelik ve değerlendirme HRD ilkeleri
HRD ilkeleri oluşturulur ve belirli kuruluşlar ve hizmet asıl adı atanmış. Bu, belirli bir uygulama için birden çok ilke mümkün olduğunu gösterir. Etkinleşir HRD ilke bu kurallar aşağıdaki gibidir:


- Bir etki alanı ipucu kimlik doğrulama isteği varsa, herhangi bir HRD ilke göz ardı edilir ve etki alanı ipucu tarafından belirtilen davranışı kullanılır.
- Aksi halde, bir ilke açıkça hizmet sorumlusu atanmışsa zorlanır. 
- Hiçbir etki alanı ipucu ve hizmet asıl açıkça hiçbir ilke atanmamışsa, açıkça hizmet sorumlusu üst kuruluşa atanan bir ilke uygulanır. 
- Hiçbir etki alanı ipucu yoktur ve hiçbir ilke hizmet sorumlusu veya kuruluştan bir başkasına atanmış olan varsayılan HRD davranışı kullanılır.

## <a name="tutorial-for-setting-sign-in-auto-acceleration-on-an-application-using-hrd-policy-with-azure-active-directory-powershell-cmdlets"></a>Oturum açma otomatik-hızlandırma HRD İlkesi ile Azure Active Directory PowerShell cmdlet'lerini kullanarak bir uygulama ayarı Öğreticisi
Biz de dahil olmak üzere birkaç senaryolar üzerinden yol:


- Tek bir Federasyon etki alanına sahip bir kiracı için bir uygulama için otomatik ivmesini ayarlama
- Kiracınız için doğrulanır birkaç etki alanlarından biri için bir uygulama için otomatik ivmesini ayarlama
- Bir ilke yapılandırıldığı uygulamaları listeleme

### <a name="prerequisites"></a>Ön koşullar
Aşağıdaki örneklerde oluştur, Güncelleştir, bağlantı ve Azure AD'de uygulama hizmet asıl adı ilkelerini Sil.

1.  Başlamak için en son Azure AD PowerShell Cmdlet Önizleme indirin. 
2.  Azure AD PowerShell cmdlet'leri olduktan sonra Azure AD Yönetici hesabınızla oturum için Bağlan komutunu çalıştırın.

    ``` powershell
    Connect-AzureAD -Confirm
    ```
3.  Kuruluşunuzdaki tüm ilkeleri görmek için aşağıdaki komutu çalıştırın.

    ``` powershell
    Get-AzureADPolicy
    ```

Hiçbir şey döndürülmezse, kiracınızda oluşturulan hiçbir ilkelerine sahip

### <a name="example-setting-auto-acceleration-for-an-application"></a>Örnek: bir uygulama için otomatik ivmesini ayarlama 
Bu örnekte otomatik-kullanıcıların AD FS oturum açma ekranına uygulamaya oturum açarken hızlandırır bir ilke oluşturun.  Bu, bunları bir kullanıcı adı Azure AD oturum açma sayfasında ilk girmek zorunda gerçekleştirilir. 

#### <a name="step-1-create-an-hrd-policy"></a>1. adım: bir HRD ilkesi oluşturma
``` powershell
New-AzureADPoly -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true}}") -DisplayName BasicAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```

Uygulamalar için kullanıcıların kimliğini doğrulayan tek bir Federasyon etki alanınız varsa, yalnızca bir HRD İlkesi oluşturmanız gerekir.  
Kendi objectID alın ve Yeni ilkenizle görmek için aşağıdaki komutu çalıştırın.

``` powershell
Get-AzureADPolicy
```


Otomatik-ivmesini etkinleştirmek için bir HRD İlkesi olduktan sonra birden çok uygulama hizmeti ilkeleri atayabilirsiniz.

#### <a name="step-2-locate-the-service-principal-to-assign-the-policy-to"></a>2. adım: ilkeyi atamak için hizmet asıl adı bulunamadı.  
İlkeyi atamak istediğiniz hizmet asıl adı objectID gerekir. Hizmet sorumluları nesne kimliği bulmanın birkaç yolu vardır.    

Portalı kullanabilirsiniz, Sorgulayabileceğiniz [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) veya gitmek bizim [grafik Gezgini aracı](https://graphexplorer.cloudapp.net/) ve kuruluşunuzun tüm hizmet asıl adı görmek için Azure AD hesabınızda oturum açın veya beri kullanmakta olduğunuz PowerShell, hizmeti ilkeleri ve kimlikleri listelemek için get-AzureADServicePrincipal cmdlet'ini kullanabilirsiniz.

#### <a name="step-3-assign-the-policy-to-your-service-principal"></a>3. adım: ilke, hizmet sorumlusu atayın.  
Hizmet sorumlusu otomatik hızlandırması için yapılandırmak istediğiniz uygulamanın objectID olduktan sonra 2. adımda bulduğunuz hizmet asıl 1. adımda oluşturduğunuz HRD ilkeyi ilişkilendirmek için aşağıdaki komutu çalıştırın.

``` powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
```

Bu komut için her hizmet için ilke eklemek istediğiniz asıl yineleyebilirsiniz.

#### <a name="step-4-check-which-application-service-principals-your-auto-acceleration-policy-is-assigned-to"></a>4. adım: otomatik hızlandırma ilkeniz atandığı hangi uygulama hizmet asıl adı denetleyin
Hangi uygulamaların otomatik hızlandırma sahip denetlemek için yapılandırılmış ilke Get-AzureADPolicyAppliedObject cmdlet'ini kullanın ve denetlemek istediğiniz ilke objectID geçirin.

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
#### <a name="step-5-youre-done"></a>5. adım: Tamamladınız!
Yeni ilke çalıştığını denetlemek için uygulamayı deneyin.

### <a name="example-listing-the-applications-for-which-an-auto-acceleration-policy-is-configured"></a>Örnek: bir otomatik hızlandırma İlkesi yapılandırıldığı uygulamaları listeleme

#### <a name="step-1-list-all-policies-created-in-your-organization"></a>1. adım: kuruluşunuzda oluşturulan tüm ilkeler listesi 

``` powershell
Get-AzureADPolicy
```

Not **nesne kimliği** listesi atamaları istediğiniz ilke.

#### <a name="step-2-list-the-service-principals-the-policy-is-assigned-to"></a>2. adım: ilke atandığı hizmet asıl adı listeleyin.  

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```

### <a name="example-removing-an-auto-acceleration-policy-for-an-application"></a>Örnek: bir uygulama için bir otomatik hızlandırma ilkesini kaldırma
#### <a name="step-1-use-the-previous-example-to-get-the-objectid-of-the-policy-and-that-of-the-application-service-principal-you-wish-to-remove-it-from"></a>1. adım: ilke objectID ve, buradan kaldırmak istediğiniz uygulama hizmet sorumlusu almak için önceki örnekte kullanın
#### <a name="step-2-remove-the-policy-assignment-from-the-application-service-principal"></a>2. adım: uygulama hizmet sorumlusu ilke atamasını kaldırın.  

``` powershell
Remove-AzureADApplicationPolicy -ObjectId <ObjectId of the Service Principal>  -PolicyId <ObjectId of the policy>
```

#### <a name="step-3-check-removal-by-listing-the-service-principals-the-policy-is-assigned-to"></a>3. adım: hizmet asıl adı listeleme tarafından onay kaldırma İlkesi atanır 

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
## <a name="next-steps"></a>Sonraki adımlar
- Kimlik doğrulaması Azure AD'de nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure AD için kimlik doğrulama senaryoları](develop/active-directory-authentication-scenarios.md).
- Kullanıcı çoklu oturum açma hakkında daha fazla bilgi için bkz: [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma](active-directory-enterprise-apps-manage-sso.md)
- Ziyaret [Active Directory Geliştirici Kılavuzu](develop/active-directory-developers-guide.md) Geliştirici ilişkili tüm içeriği genel bakış.
