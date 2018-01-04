---
title: "Azure AD iOS Başlarken | Microsoft Docs"
description: "Oturum açma ve Azure AD aramalar için Azure AD ile tümleşen bir iOS uygulamasının nasıl oluşturulacağını, OAuth kullanılarak API'leri korunan."
services: active-directory
documentationcenter: ios
author: brandwe
manager: mtillman
editor: 
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 11/30/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: 36c6f6d2449d1e137f85e0f657f0399f9df8ee55
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="azure-ad-ios-getting-started"></a>Azure AD iOS Başlarken
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Azure Active Directory (Azure AD), korunan kaynaklara erişim için gereken iOS istemciler için Active Directory kimlik doğrulama kitaplığı ya da ADAL sağlar. ADAL erişim belirteci edinmek için uygulamanızın kullandığı işlemini basitleştirir. Ne kadar kolay olduğunu, göstermek için bu makaledeki biz bir hedefi C Yapılacaklar listesi uygulaması, oluşturma:

* Alır erişim belirteçleri kullanarak Azure AD Graph API çağırma [OAuth 2.0 kimlik doğrulama protokolü](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Belirtilen diğer ada sahip kullanıcılar için bir dizin arar.

Tam çalışan uygulama oluşturmak için aktarmanız gerekir:

1. Uygulamanızı Azure AD ile kaydedin.
2. Yükleyin ve ADAL yapılandırın.
3. ADAL, Azure AD'den belirteçleri almak için kullanın.

Başlamak için [uygulama çatıyı indirmeniz](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) veya [tamamlanan örnek indirme](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip). Ayrıca kullanıcı oluşturma ve bir uygulamayı kaydetme Azure AD kiracısı gerekir. Bir kiracı yoksa [edinebileceğinizi öğrenin](active-directory-howto-tenant.md).


> [!TIP]
> Bizim yeni önizlemesini denemek [Geliştirici Portalı](https://identity.microsoft.com/Docs/iOS) yardımcı olan birkaç dakika içinde Azure AD ile başlamak ve çalıştırmak. Geliştirici Portalı bir uygulamayı kaydetme ve Azure AD kodunuza tümleştirme işleminde size kılavuzluk eder. Kullanıcıların kiracınızda doğrulanabilir basit bir uygulamanız varsa ve bir arka uç tamamlanmış olduğunuzda, belirteçleri kabul edebilir ve doğrulama yapmak. 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a>1. İOS için URI'dir, yönlendirme belirleme
Güvenli bir şekilde uygulamalarınızın belirli SSO senaryolarda başlatmak için oluşturmalısınız bir *yeniden yönlendirme URI'si* belirli bir biçimde. Yeniden yönlendirme URI'si belirteçleri kendileri için sorulan doğru uygulama dönmek emin olmak için kullanılır.


İOS için bir yeniden yönlendirme URI'si biçimidir:

```
<app-scheme>://<bundle-id>
```

* **Uygulama düzeni** -Bu, XCode projenizde kayıtlı değil. Bu, nasıl diğer uygulamaları çağırabilirsiniz olur. Bulabileceğiniz bu Info.plist altında -> URL türleri -> URL tanımlayıcısı. Bir veya daha fazla yapılandırılmış henüz yoksa bir oluşturmanız gerekir.
* **paket kimliği** -XCode projesi ayarlarınızın "kimlik" altında bulunan paket tanımlayıcısı budur.

Bu hızlı başlangıç kodun bir örnek: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>2. DirectorySearcher uygulamayı Kaydet
Belirteçleri almak için uygulamanızı ayarlayın için önce Azure AD kiracınızda kaydolun ve Azure AD grafik API'sine erişim izni vermeniz gerekir:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda hesabınızı tıklatın. Altında **Directory** listesinde, Active Directory Kiracı uygulamanızı kaydetmek istediğiniz yeri seçin.
3. Tıklatın **daha Hizmetleri** Soldaki gezinti bölmesi ve ardından **Azure Active Directory**.
4. Tıklatın **uygulama kayıtlar**ve ardından **Ekle**.
5. Yeni bir oluşturmak için istemleri izleyin **yerel istemci uygulaması**.
  * **Adı** uygulamanızı son kullanıcıların uygulamayı açıklar.
  * **Yeniden yönlendirme URI'si** belirteci yanıtları döndürmek için Azure AD kullanır ve dize şeması bir birleşimidir.  Uygulamanıza özeldir ve önceki yeniden yönlendirme URI'si bilgiyi temel alan bir değer girin.
6. Azure AD kaydı'nı tamamladıktan sonra uygulamanızı bir benzersiz uygulama kimliği atar.  Bu değer gerekir sonraki bölümlerde, bu nedenle uygulama sekmesinden kopyalayın.
7. Gelen **ayarları** sayfasında, **gerekli izinler** ve ardından **Ekle**. Seçin **Microsoft Graph** API olarak ve ardından ekleyin **dizin verilerini okuma** altında izni **izinlere temsilci**.  Bu kullanıcılar için Azure AD Graph API sorgulamak için uygulamanızı ayarlar.

## <a name="3-install-and-configure-adal"></a>3. Yükleme ve yapılandırma ADAL
Azure AD'de bir uygulamanız varsa, ADAL yükleyin ve kimlikle ilgili kodunuzu yazın.  Azure AD ile iletişim kurmak ADAL için uygulama kaydınızı hakkında bazı bilgiler ile sağlamanız gerekir.

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

3. Şimdi CocoaPods kullanarak podfile dosyasını yükleyin. Bu adım, yüklediğiniz yeni bir XCode çalışma alanı oluşturur.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. Hızlı başlangıç projesi plist dosyasını açın `settings.plist`.  Azure portalında girdiğiniz değerleri yansıtacak şekilde bölümdeki öğelerinin değerlerini değiştirin. ADAL kullandığında kodunuzu bu değerleri başvurur.
  * `tenant` Azure AD kiracınız, örneğin, contoso.onmicrosoft.com etki alanıdır.
  * `clientId` Portalından kopyalandığından, uygulamanızın istemci kimliği.
  * `redirectUri` Portalı'nda kayıtlı yeniden yönlendirme URL'si.

## <a name="4----use-adal-to-get-tokens-from-azure-ad"></a>4.    Azure AD'den belirteçleri almak için ADAL'ı kullanın
Temel ADAL arkasında uygulamanızı bir erişim belirteci her gerektiğinde, bu yalnızca bir completionBlock çağırdığı ilkesidir `+(void) getToken : `, ve ADAL rest yapar.  

1. İçinde `QuickStart` proje, açık `GraphAPICaller.m` ve bulun `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` ilk açıklama.  ADAL burada geçirdiğiniz budur Azure AD ile iletişim kurmak ve belirteçleri önbelleğe almak nasıl bildirmek için bir CompletionBlock aracılığıyla koordinatları.

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

2. Şimdi biz grafikte kullanıcıları aramak için bu belirteci kullanmanız gerekir. Bul `// TODO: implement SearchUsersList` açıklama. Bu yöntem, UPN verilen arama terimiyle kullanıcıları için Azure AD Graph API sorgu için bir GET isteği yapar.  Azure AD Graph API sorgulamak için bir access_token ile eklemeniz gerekir `Authorization` isteği üstbilgisi. ADAL nereden geldiğini budur.

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


3. Uygulamanızı isteğinde bulunduğunda bir belirteç çağırarak `getToken(...)`, ADAL kullanıcı kimlik bilgilerinin sorulmasına olmadan bir simge döndürür dener.  ADAL kullanıcı bir belirteç almak üzere oturum açmak gerektiğini belirlerse, oturum açma için bir iletişim kutusu görüntüler, kullanıcının kimlik bilgilerini toplamak ve sonra bir belirteç başarılı kimlik doğrulamasından sonra geri dönüp.  ADAL herhangi bir nedenle bir simge döndürür mümkün değilse, oluşturur bir `AdalException`.

> [!Note] 
> `AuthenticationResult` Nesnesini içeren bir `tokenCacheStoreItem` uygulamanız gerekebilir bilgi toplamak için kullanılan nesne. Hızlı Başlangıç içinde `tokenCacheStoreItem` kimlik doğrulamanın zaten yapılıp belirlemek için kullanılır.
>
>

## <a name="5-build-and-run-the-application"></a>5. Uygulamayı derleme ve çalıştırma
Tebrikler! Artık çalışan bir iOS uygulama kullanıcıların kimlik doğrulaması, güvenli bir şekilde OAuth 2.0 kullanarak Web API'leri çağırmak ve kullanıcı hakkındaki temel bilgileri elde var.  Henüz yapmadıysanız, bazı kullanıcılar ile Kiracı doldurmak için zaman sunulmuştur.  Hızlı Başlangıç uygulamanızı başlatın ve ardından kullanıcılar bir oturum açın.  Diğer kullanıcıların UPN Değerlerinin üzerinde göre arayın.  Uygulamayı kapatın ve yeniden başlatın.  Kullanıcının oturumunu değişmeden kalır dikkat edin.

ADAL, bu ortak kimlik özelliklerin tümü, uygulamanıza eklemenizi kolaylaştırır.  Bu tüm dirty iş sizin için önbellek yönetimi gibi OAuth protokol desteği, kullanıcı oturum açma, için bir kullanıcı Arabirimi ile sunan mvc'deki ve yenileme belirteçleri süresi.  Gerçekten bilmeniz gereken tek şey tek bir API çağrısı `getToken`.

Başvuru için üzerinde tamamlanan örnek (yapılandırma değerleriniz olmadan) sağlanır [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="next-steps"></a>Sonraki adımlar
Şimdi ilave senaryolar da taşıyabilirsiniz.  Denemek isteyebilirsiniz:

* [Node.JS Web API'si Azure AD ile güvenli](active-directory-devquickstarts-webapi-nodejs.md)
* Bilgi [uygulamalar arası SSO'nun ADAL kullanarak iOS etkinleştirme](active-directory-sso-ios.md)  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

