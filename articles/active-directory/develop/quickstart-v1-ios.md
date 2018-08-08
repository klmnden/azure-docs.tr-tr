---
title: Azure AD iOS kullanmaya başlama | Microsoft Docs
description: Azure AD oturum açma ve Azure AD aramaları için ile tümleşen bir iOS uygulamasının nasıl oluşturulacağını, OAuth kullanarak API'leri korumalı.
services: active-directory
documentationcenter: ios
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/30/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: jmprieur
ms.openlocfilehash: c370a90cf050a88e66ea0417f018429f7815b7c9
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39592246"
---
# <a name="azure-ad-ios-getting-started"></a>Azure AD iOS kullanmaya başlama
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Azure Active Directory (Azure AD), korunan kaynaklara erişmesi gereken iOS istemcileri için Active Directory Authentication Library veya ADAL sağlar. ADAL erişim belirteçleri almak için uygulamanızı kullanan işlemini basitleştirir. Ne kadar kolay olduğunu göstermek için bu makaledeki ki Objective C Yapılacaklar listesi uygulaması oluşturacaksınız:

* Alır erişim belirteçlerini kullanarak Azure AD Graph API'sini çağırmak için [OAuth 2.0 kimlik doğrulama protokolü](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Belirli bir takma ad ile kullanıcılar için bir dizini arar.

Tam çalışma uygulamayı oluşturmak için şunları yapmanız:

1. Uygulamanızı Azure AD'ye kaydedin.
2. Yükleme ve ADAL'ı yapılandırın.
3. Azure AD belirteçlerini almak için ADAL'ı kullanın.

Başlamak için [uygulama çatıyı indirmeniz](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) veya [tamamlanan örnek indirme](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip). Ayrıca kullanıcılar oluşturur ve bir uygulamayı kaydetme Azure AD kiracısı gerekir. Bir kiracı yoksa [edinebileceğinizi öğrenin](quickstart-create-new-tenant.md).


> [!TIP]
> Yeni önizlemeyi denemek [Geliştirici Portalı](https://identity.microsoft.com/Docs/iOS) yardımcı olan birkaç dakika içinde Azure AD ile çalışır duruma alın. Geliştirici Portalı bir uygulamayı kaydetme ve kodunuza Azure AD tümleştirme işleminde size kılavuzluk eder. Ne zaman, kiracınızdaki kullanıcıların kimliğini doğrulamak basit bir uygulama gerekir ve bir arka uç işiniz bittiğinde, belirteçleri kabul edebilir ve doğrulama gerçekleştirin. 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a>1. İOS için URI'dir, yönlendirme belirleme
Uygulamalarınızı belirli SSO senaryolarda güvenli bir şekilde başlamak için oluşturmalısınız bir *yeniden yönlendirme URI'si* belirli bir biçimde. Yeniden yönlendirme URI'si belirteçleri için sorulan doğru uygulama döndürdüğünden emin olmak için kullanılır.


İOS bir yeniden yönlendirme URI'si biçimi:

```
<app-scheme>://<bundle-id>
```

* **uygulama düzenini** -bu XCode projenizde kaydedilir. Bu, nasıl başka uygulamalar çağırabilirsiniz olur. Bulabilirsiniz Info.plist altında bu -> URL türleri -> URL tanımlayıcısı. Bir veya daha fazla yapılandırılmış henüz yoksa bir oluşturmanız gerekir.
* **paket kimliği** -paket tanımlayıcısı XCode proje ayarlarınızın "kimlik" altında bulunan budur.

Bu hızlı başlangıç kod örneği: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>2. DirectorySearcher uygulamayı kaydetme
Belirteçlerini almak için uygulamanızı ayarlayın için ilk Azure AD kiracınıza kaydetme ve Azure AD Graph API'sine erişim izni vermeniz gerekir:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda, hesabınıza tıklayın. Altında **dizin** listesinde, istediğiniz uygulamanızı kaydetmek için Active Directory kiracısı seçin.
3. Tıklayın **tüm hizmetleri** en soldaki Gezinti bölmesinde ve ardından **Azure Active Directory**.
4. Tıklayın **uygulama kayıtları**ve ardından **Ekle**.
5. Yeni bir oluşturmak için istemleri izleyin **yerel istemci uygulaması**.
  * **Adı** uygulamanız son kullanıcılara uygulamayı açıklar.
  * **Yeniden yönlendirme URI'si** belirteç yanıtlarını döndürmek için Azure AD kullanan bir şema ve dize birleşiminden oluşur. Uygulamanıza özgü olan ve önceki yeniden yönlendirme URI'si bilgiyi temel alan bir değer girin.
6. Kayıt tamamlandıktan sonra Azure AD uygulamanızı benzersiz bir uygulama kimliği atar. Bu değer gerekir sonraki bölümlerde, bu nedenle uygulama sekmesinden kopyalayın.
7. Gelen **ayarları** sayfasında **gerekli izinler** seçip **Ekle**. Seçin **Microsoft Graph** API olarak ve ardından eklemek **dizin verilerini okuma** altında izin **Temsilcili izinler**. Bu kullanıcılar için Azure AD Graph API'si sorgulamak için uygulamanızı ayarlar.

## <a name="3-install-and-configure-adal"></a>3. Yükleme ve ADAL'ı yapılandırma
Azure AD'de bir uygulamanız olduğuna göre ADAL'ı yükleyebilir ve kimlikle ilgili kodunuzu yazın. Azure AD ile iletişim kurmak için ADAL, uygulama kaydınızı hakkında bazı bilgiler vermeniz gerekir.

1. CocoaPods kullanarak ADAL DirectorySearcher projeye ekleyerek başlayın.

    ```
    $ vi Podfile
    ```
2. Aşağıdakileri bu podfile dosyaya ekleyin:

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

3. Şimdi CocoaPods kullanarak podfile dosyasını yükleyin. Bu adımda yüklediğiniz yeni bir XCode çalışma alanı oluşturulur.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. Hızlı başlangıç projesi plist dosyasını açın `settings.plist`. Azure portalında girdiğiniz değerleri yansıtacak şekilde bölümünde öğe değerlerini değiştirin. ADAL kullandığında, kodunuzun bu değerleri başvuruyor.
  * `tenant` Contoso.onmicrosoft.com gibi Azure AD kiracınızın etki alanıdır.
  * `clientId` Portaldan kopyaladığınız uygulamanızın istemci kimliği.
  * `redirectUri` Portalı'nda kayıtlı yeniden yönlendirme URL'si.

## <a name="4-use-adal-to-get-tokens-from-azure-ad"></a>4. Azure AD belirteçlerini almak için ADAL'ı kullanın
Uygulamanızı bir erişim belirteci gerektiğinde, yalnızca bir completionBlock çağırır, ADAL ardındaki temel buradaki prensip şudur: `+(void) getToken : `, ve ADAL geri kalanını yapar. 

1. İçinde `QuickStart` projesini açarsanız `GraphAPICaller.m` bulun `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` üst yorum. ADAL burada geçirdiğiniz budur aracılığıyla Azure AD ile iletişim kurmak ve belirteçleri önbelleğe alınacağını bildirmek için bir CompletionBlock koordinatlar.

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

2. Artık grafiğin kullanıcıları aramak için bu belirteci kullanmasına izin ihtiyacımız var. Bulma `// TODO: implement SearchUsersList` açıklaması. Bu yöntem, UPN verilen arama terimiyle başlayan kullanıcıları için Azure AD Graph API için sorgu için bir GET isteği yapar. Bir access_token dahil istediğiniz Azure AD Graph API'si sorgulamak için `Authorization` isteği üstbilgisi. Bu, ADAL burada devreye girer.

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


3. Uygulamanız istediğinde bir belirteç çağırarak `getToken(...)`, kullanıcı kimlik bilgilerinin sorulmasına gerek kalmadan bir belirteç döndürecek şekilde ADAL çalışır. ADAL bir belirteç almak üzere oturum açmak kullanıcının gerektiğini belirlerse, oturum açmak için bir iletişim kutusu, kullanıcının kimlik bilgilerini toplamak ve daha sonra başarılı kimlik doğrulamasından sonra bir belirteç döndürür. ADAL herhangi bir nedenle bir belirteç döndürecek şekilde mümkün değilse, oluşturur bir `AdalException`.

> [!Note] 
> `AuthenticationResult` Nesne içeren bir `tokenCacheStoreItem` uygulamanız gereken bilgileri toplamak için kullanılan nesne. Hızlı başlangıçta, `tokenCacheStoreItem` kimlik doğrulaması zaten yapıldığını belirlemek için kullanılır.
>
>

## <a name="5-build-and-run-the-application"></a>5. Uygulamayı derleme ve çalıştırma
Tebrikler! Artık çalışan bir iOS uygulama kullanıcıların kimlik doğrulaması, güvenli bir şekilde OAuth 2.0 kullanarak Web API'leri çağırmak, ve kullanıcı ile ilgili temel bilgileri alın var. Henüz yapmadıysanız, kiracınızın bazı kullanıcılar ile doldurmak için zaman sunulmuştur. Hızlı Başlangıç uygulamanızı başlatın ve sonra bu kullanıcılardan birinin oturum oturum. Kendi UPN'e bağlı diğer kullanıcılar için arama yapın. Uygulamayı kapatın ve yeniden başlatın. Kullanıcının oturumunu değişmeden kalır dikkat edin.

ADAL, bu ortak kimlik özelliklerin tümü, uygulamanıza eklemenize kolaylaştırır. Bunu tüm kirli çalışması, önbellek yönetimi gibi OAuth protokol desteği, kullanıcı oturum açma, bir kullanıcı Arabirimi ile sunma üstlenir ve yenileme belirteçleri süresi doldu. Gerçekten bilmeniz gereken her şey tek bir API çağrısı `getToken`.

Başvuru için tamamlanmış bir örnek (olmadan yapılandırma değerlerinize) üzerinde sağlanan [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip). 

## <a name="next-steps"></a>Sonraki adımlar
Artık için ilave senaryolar da taşıyabilirsiniz. Denemek isteyebilirsiniz:

* [Node.JS Web API'si Azure AD ile güvenli hale getirme](quickstart-v1-nodejs-webapi.md)
* Bilgi [iOS ADAL kullanarak uygulamalar arası SSO'yu etkinleştirme](howto-v1-enable-sso-ios.md)  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

