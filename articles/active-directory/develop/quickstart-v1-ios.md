---
title: Oturum açma için Azure AD ile tümleştirilen ve OAuth 2.0 kullanarak korumalı API'leri çağıran bir iOS uygulaması oluşturma | Microsoft Docs
description: iOS uygulamanızdan kullanıcıların oturumunu açmayı ve Microsoft Graph API'sini çağırmayı öğrenin.
services: active-directory
documentationcenter: ios
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: brandwe
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d986ccbf92192c1fb7375e9db1fb398ed86a829
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60299048"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-app"></a>Hızlı Başlangıç: Kullanıcılar oturum ve bir iOS uygulamasından Microsoft Graph API çağırma

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Azure Active Directory (Azure AD), korumalı kaynaklara erişmesi gereken iOS istemcileri için Active Directory Authentication Library (ADAL) sağlar. ADAL, uygulamanızın erişim belirteçlerini almak için kullandığı işlemi basitleştirir. 

Bu hızlı başlangıçta, aşağıdakileri yapan bir Objective C To-Do List uygulaması oluşturacaksınız:

* OAuth 2.0 kimlik doğrulama protokolünü kullanarak Azure AD Graph API'sini çağırmak için erişim belirteçlerini alma
* Belirli bir diğer adla dizinde kullanıcılar için arama yapma

Eksiksiz, çalışan bir uygulama oluşturmak için şunları yapmalısınız:

1. Uygulamanızı Azure AD'ye kaydedin.
1. ADAL'ı yükleyin ve yapılandırın.
1. ADAL'ı kullanarak Azure AD'den belirteçleri alın.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için şu önkoşulları tamamlayın:

* [Uygulama çatısını indirin](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) veya [tamamlanmış örneği indirin](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).
* Altında kullanıcıları oluşturabileceğiniz ve uygulamayı kaydedebileceğiniz bir Azure AD kiracınız olsun. Henüz kiracınız yoksa, [nasıl alabileceğinizi öğrenin](quickstart-create-new-tenant.md).

> [!TIP]
> Azure AD ile yalnızca birkaç dakika içinde çalışır duruma gelmek için [geliştirici portalını](https://identity.microsoft.com/Docs/iOS) deneyin. Geliştirici portalı, uygulamayı kaydetme ve Azure AD'yi kodunuzla tümleştirme işleminde size yol gösterir. Bitirdiğinizde, kiracınızda kullanıcıların kimliklerini doğrulayabilen basit bir uygulamanız ve belirteçleri kabul eden ve doğrulama gerçekleştiren bir arka ucunuz olacaktır.

## <a name="step-1-determine-what-your-redirect-uri-is-for-ios"></a>1. Adım: İOS için URI'dir, yönlendirme belirleme

Bazı SSO senaryolarında uygulamalarınızı güvenle başlatmak için, belirli bir biçimde *yeniden yönlendirme URI*'si oluşturmanız gerekir. Yeniden yönlendirme URI'si, belirteçlerin bunları isteyen doğru uygulamaya döndürüldüğünden emin olmak için kullanılır.

Yeniden yönlendirme URI'sinin iOS biçimi şöyledir:

```
<app-scheme>://<bundle-id>
```

* **app-scheme**: XCode projenizde kayıtlıdır ve diğer uygulamaların sizi nasıl çağırabileceğini gösterir. app-scheme'i **Info.plist > URL türleri > URL Tanımlayıcısı** altında bulabilirsiniz. Yapılandırılmış bir veya birden çok app-scheme'iniz yoksa oluşturun.
* **bundle-id**: XCode proje ayarlarınızdaki **kimliğin** altında bulunan Paket Tanımlayıcısıdır.

Bu hızlı başlangıç kodu için bir örnek:

***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="step-2-register-the-directorysearcher-application"></a>2. Adım: DirectorySearcher uygulamayı kaydetme

Uygulamanızı belirteçleri alacak şekilde ayarlamak için, uygulamayı Azure AD kiracınıza kaydetmeli ve uygulamaya Azure AD Graph API'sine erişim izni vermelisiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubukta hesabınızı seçin. **Dizin** listesi altında, uygulamanızı kaydetmek istediğiniz Azure Active Directory kiracısını seçin.
3. En soldaki gezinti bölmesinde **Tüm hizmetler**'i seçin ve ardından **Azure Active Directory**'yi seçin.
4. **Uygulama kayıtları**'nı ve ardından **Ekle**'yi seçin.
5. İstemleri izleyerek yeni bir **Yerel** istemci uygulaması oluşturun.
    * **Ad**, uygulamanın adıdır ve uygulamanızı son kullanıcılara açıklar.
    * **Yeniden Yönlendirme URI'si**, Azure AD'nin belirteç yanıtlarını döndürmek için kullandığı şema ve dize bileşimidir. Uygulamanıza özgü olan ve önceki yeniden yönlendirme URI'si bilgilerini temel alan bir değer girin.
6. Kaydı tamamladıktan sonra, Azure AD uygulamanıza benzersiz bir uygulama kimliği atar. Bu değeri sonraki bölümlerde kullanacağınız için, uygulama sekmesinden kopyalayın.
7. **Ayarlar** sayfasında **Gerekli izinler > Ekle > Microsoft Graph**'ı seçin ve **Temsilcili izinler**'in altına **Dizin verilerini okuma** iznini ekleyin. Bu izin, uygulamanızı Azure AD Graph API'sini kullanıcılar için sorgulayacak şekilde ayarlar.

## <a name="step-3-install-and-configure-adal"></a>3. Adım: Yükleme ve ADAL'ı yapılandırma

Artık Azure AD'de bir uygulamanız olduğuna göre, ADAL'ı yükleyebilir ve kimlikle ilgili kodunuzu yazabilirsiniz. ADAL'ın Azure AD ile iletişim kurabilmesi için, ona uygulama kaydınız hakkında bazı bilgiler sağlamanız gerekir.

1. Başlangıç olarak, CocoaPods kullanarak ADAL'ı DirectorySearcher projesine ekleyin.

    ```
    $ vi Podfile
    ```
1. Aşağıdakileri bu podfile dosyaya ekleyin:

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

1. CocoaPods kullanarak podfile dosyasını yükleyin. Bu adımda, yüklediğiniz yeni bir XCode çalışma alanı oluşturulur.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

1. QuickStart projesinde plist dosyasını (`settings.plist`) açın.
1. Bölümde öğelerin değerlerini, Azure portalında girdiğiniz değerlerin aynılarını kullanacak şekilde değiştirin. Kodunuz ADAL'ı her kullandığında bu değerlere başvurur.
    * `tenant`, Azure AD kiracınızın etki alanıdır (örneğin, contoso.onmicrosoft.com).
    * `clientId`, portaldan kopyaladığınız uygulamanızın istemci kimliğidir.
    * `redirectUri`, portalda kaydettiğiniz yeniden yönlendirme URL'sidir.

## <a name="step-4-use-adal-to-get-tokens-from-azure-ad"></a>4. Adım: Azure AD belirteçlerini almak için ADAL'ı kullanın

ADAL'ın ardındaki temel ilke, uygulamanızın bir erişim belirtecine her ihtiyacı olduğunda basitçe bir completionBlock `+(void) getToken :` çağrısı yapması ve kalan işleri ADAL'ın üstlenmesidir.

1. `QuickStart` projesinde, `GraphAPICaller.m` dosyasını açın ve üst kısımdaki `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` açıklamasını bulun.

    Burası, ADAL'a Azure AD ile iletişim kurması için CompletionBlock aracılığıyla koordinatları geçirdiğiniz ve belirteçleri nasıl önbelleğe alacağını belirttiğiniz yerdir.

    ```ObjC
    +(void) getToken : (BOOL) clearCache
               parent:(UIViewController*) parent
    completionHandler:(void (^) (NSString*, NSError*))completionBlock;
    {
        AppData* data = [AppData getInstance];
        if(data.userItem){
            completionBlock(data.userItem.accessToken, nil);
            return;
        }

        ADAuthenticationError *error;
        authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
        authContext.parentController = parent;
        NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:data.resourceId
                                     clientId:data.clientId
                                  redirectUri:redirectUri
                               promptBehavior:AD_PROMPT_AUTO
                                       userId:data.userItem.userInformation.userId
                        extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                             completionBlock:^(ADAuthenticationResult *result) {

                                  if (result.status != AD_SUCCEEDED)
                                  {
                                     completionBlock(nil, result.error);
                                  }
                                  else
                                  {
                                      data.userItem = result.tokenCacheStoreItem;
                                      completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                                  }
                             }];
    }

    ```

2. Grafta kullanıcılar için arama yaparken bu belirteci kullanmanız gerekir. `// TODO: implement SearchUsersList` açıklamasını bulun. Bu yöntemde, UPN'leri verilen arama terimiyle başlayan kullanıcıları sorgulamak için Azure AD Graph API'sinden bir GET isteğinde bulunulur. 

    Azure AD Graph API'sini sorgulamak için, isteğin `Authorization` üst bilgisine bir access_token eklemelisiniz. ADAL işte burada devreye girer.

    ```ObjC
    +(void) searchUserList:(NSString*)searchString
                    parent:(UIViewController*) parent
          completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
    {
        if (!loadedApplicationSettings)
       {
            [self readApplicationSettings];
        }
        
        AppData* data = [AppData getInstance];

        NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];

        [self craftRequest:[self.class trimString:graphURL]
                    parent:parent
         completionHandler:^(NSMutableURLRequest *request, NSError *error) {

             if (error != nil)
             {
                 completionBlock(nil, error);
             }
             else
             {

                 NSOperationQueue *queue = [[NSOperationQueue alloc]init];

                 [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                     if (error == nil && data != nil){

                         NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                         // We can grab the JSON node at the top to get our graph data.
                         NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                         // Don't be thrown off by the key name being "value". It really is the name of the
                         // first node. :-)

                         // Each object is a key value pair
                         NSDictionary *keyValuePairs;
                         NSMutableArray* Users = [[NSMutableArray alloc]init];

                         for(int i =0; i < graphDataArray.count; i++)
                         {
                             keyValuePairs = [graphDataArray objectAtIndex:i];

                             User *s = [[User alloc]init];
                             s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                             s.name =[keyValuePairs valueForKey:@"givenName"];

                             [Users addObject:s];
                         }

                         completionBlock(Users, nil);
                     }
                     else
                     {
                         completionBlock(nil, error);
                     }

                }];
             }
         }];

    }

    ```

3. Uygulamanız `getToken(...)` çağrısı yaparak bir belirteç istediğinde, ADAL kullanıcıdan kimlik bilgilerini istemeden bir belirteç döndürmeyi dener. ADAL kullanıcının belirteci almak için oturum açması gerektiğine karar verirse, oturum açmak için bir iletişim kutusu görüntüler, kullanıcının kimlik bilgilerini toplar ve kimlik doğrulaması başarılı olduktan sonra belirteci döndürür. ADAL herhangi bir nedenle belirteci döndüremezse, `AdalException` oluşturur.

> [!NOTE]
> `AuthenticationResult` nesnesi, uygulamanıza gerekebilecek bilgileri toplamak için kullanabileceğiniz bir `tokenCacheStoreItem` nesnesi içerir. QuickStart'ta, kimlik doğrulamasının zaten yapılıp yapılmadığını saptamak için `tokenCacheStoreItem` kullanılır.

## <a name="step-5-build-and-run-the-application"></a>5. Adım: Uygulamayı derleme ve çalıştırma

Tebrikler! Artık kullanıcıların kimliklerini doğrulayabileceğiniz, OAuth 2.0 kullanarak Web API'lerini güvenle çağırabileceğiniz ve kullanıcı hakkında temel bilgiler alabileceğiniz çalışan bir iOS uygulamanız vardır.

Henüz yapmadıysanız, kiracınızı biraz kullanıcıyla doldurmanın zamanı geldi.

1. QuickStart uygulamanızı başlatın ve bu kullanıcılardan biriyle oturum açın.
1. UPN'lerine göre diğer kullanıcılar için arama yapın.
1. Uygulamayı kapatın ve ardından yeniden başlatın. Kullanıcının oturumunun olduğu gibi kaldığına dikkat edin.

ADAL, bu yaygın kimlik özelliklerinin tümünü uygulamanıza eklemenizi kolaylaştırır. Önbellek yönetimi, OAuth protokol desteği, kullanıcıya oturum açma kullanıcı arabirimini gösterme ve süresi dolmuş belirteçleri yenileme gibi tüm sıkıcı işleri sizin yerinize yapar. Gerçekten tek bilmeniz gereken, tek bir API çağrısıdır (`getToken`).

Tamamlanan örnek, başvuru için (yapılandırma değerleriniz olmadan) [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip)'da sağlanır.

## <a name="next-steps"></a>Sonraki adımlar

Artık ek senaryolara geçebilirsiniz. Bundan sonra aşağıdakileri denemenizi öneririz:

* [Azure AD ile Node.JS Web API'sinin güvenliğini sağlama](quickstart-v1-nodejs-webapi.md)
* [ADAL kullanarak iOS'ta uygulamalar arası SSO'yu etkinleştirmeyi](howto-v1-enable-sso-ios.md) öğrenme  
