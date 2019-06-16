---
title: Nasıl yapılır kullanım iOS için Azure Mobile Apps SDK'sı
description: Nasıl yapılır kullanım iOS için Azure Mobile Apps SDK'sı
services: app-service\mobile
documentationcenter: ios
author: conceptdev
editor: ''
ms.assetid: 4e8e45df-c36a-4a60-9ad4-393ec10b7eb9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: b6f93cc3c35ab18ecd50ccd6b3090985497baabf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62122464"
---
# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Nasıl yapılır kullanım iOS için Azure Mobile Apps istemci kitaplığı

[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Bu kılavuzu kullanarak en yaygın senaryoları gerçekleştirmek için öğretir [Azure Mobile Apps iOS SDK'sı][1]. Azure Mobile Apps için yeni başladıysanız, ilk olarak işlemini [Azure Mobile Apps Hızlı Başlangıç] bir arka ucu oluşturmak için tablo oluşturma ve önceden oluşturulmuş iOS Xcode projenizi indirin. Bu kılavuzda, biz istemci-tarafı iOS SDK'sı odaklanın. Arka uç sunucu tarafı SDK'sı hakkında daha fazla bilgi için sunucu SDK'sı HOWTOs bakın.

## <a name="reference-documentation"></a>Başvuru belgeleri

Başvuru belgeleri için iOS istemci SDK'yı şuradan ulaşabilirsiniz: [Azure Mobile Apps iOS istemci başvurusu][2].

## <a name="supported-platforms"></a>Desteklenen platformlar

İOS SDK'sı, iOS 8.0 veya üzeri sürümler için projeleri Objective-C, Swift 2.2 projeleri ve Swift 2.3 projeleri destekler.

"Sunucu-flow" kimlik doğrulaması, kullanıcı Arabiriminde sunulan bir WebView kullanır.  Cihaz bir WebView Arabirim sunmak mümkün değilse başka bir kimlik doğrulama yöntemi sonra ürün kapsamı dışında olmasıdır.  
Bu SDK'sı böylece izleme türü veya benzer şekilde kısıtlı cihazlar için uygun değil.

## <a name="Setup"></a>Kurulum ve Önkoşullar

Bu kılavuz, bir arka ucu içeren bir tablo oluşturdunuz varsayılır. Bu kılavuz, tablo bu öğreticilerde tabloların aynı şemaya sahip olduğunu varsayar. Bu kılavuz, ayrıca kodunuzda, başvuru varsayar `MicrosoftAzureMobile.framework` ve içeri aktarma `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

## <a name="create-client"></a>Nasıl Yapılır: İstemcisi oluşturma

Bir Azure Mobile Apps arka ucu, projenizdeki erişmek için oluşturun bir `MSClient`. Değiştirin `AppUrl` app URL'si. Bırakabilir `gatewayURLString` ve `applicationKey` boş. Kimlik doğrulaması için bir ağ geçidi ayarlarsanız doldurmak `gatewayURLString` ile ağ geçidi URL'si.

**Objective-C**:

```objc
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Swift**:

```swift
let client = MSClient(applicationURLString: "AppUrl")
```

## <a name="table-reference"></a>Nasıl Yapılır: Tablo başvurusu

Verilere erişmek veya verileri güncelleştirmek için arka uç tablosuna başvuru oluşturun. `TodoItem` ifadesini tablonuzun adıyla değiştirin

**Objective-C**:

```objc
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Swift**:

```swift
let table = client.tableWithName("TodoItem")
```

## <a name="querying"></a>Nasıl Yapılır: Verileri sorgulama

Veritabanı sorgusu oluşturmak için sorgu `MSTable` nesne. Aşağıdaki sorgu tüm öğeleri alır `TodoItem` ve her bir öğenin metnini günlüğe kaydeder.

**Objective-C**:

```objc
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occurred
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```swift
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="filtering"></a>Nasıl Yapılır: Filtre veri döndürdü.

Sonuçları filtrelemek için kullanılabilir pek çok seçenek vardır.

Koşulu kullanarak filtrelemek için kullanmak bir `NSPredicate` ve `readWithPredicate`. Aşağıdaki filtreler yalnızca tamamlanmamış Todo öğelerini bulmak için veri döndürdü.

**Objective-C**:

```objc
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```swift
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="query-object"></a>Nasıl Yapılır: MSQuery kullanın

(Sıralama dahil olmak üzere ve disk belleği) karmaşık bir sorgu gerçekleştirmek oluşturmak için bir `MSQuery` doğrudan veya bir koşulu kullanarak, nesne:

**Objective-C**:

```objc
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Swift**:

```swift
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery` birkaç sorgu davranışları denetlemenize olanak tanır.

* Sonuçları sırasını belirtin
* Döndürülecek alanları sınırı
* Döndürülecek kaç kayıtları sınırla
* Yanıtta toplam sayısı belirtin
* İstekte özel sorgu dizesi parametrelerini belirtin
* Ek işlevler Uygula

Yürütme bir `MSQuery` çağırarak sorgu `readWithCompletion` nesne üzerinde.

## <a name="sorting"></a>Nasıl Yapılır: MSQuery ile verileri sıralama

Sonuçları sıralamak için bir örneğe göz atalım. Alan 'text' artan sonra da 'Tamamlandı' azalan düzende sıralamak için çağırma `MSQuery` şu şekilde:

**Objective-C**:

```objc
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```swift
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="selecting"></a><a name="parameters"></a>Nasıl Yapılır: Alanları sınırlayacak ve sorgu dizesi parametreleri MSQuery ile genişletin

Bir sorguda döndürülecek alanları sınırlayacak şekilde alanlarda adlarını belirtin **selectFields** özelliği. Bu örnekte, yalnızca metin ve tamamlanan alanları döndürür:

**Objective-C**:

```objc
query.selectFields = @[@"text", @"complete"];
```

**Swift**:

```swift
query.selectFields = ["text", "complete"]
```

(Örneğin, özel bir sunucu tarafı komut dosyası bunları kullandığından) sunucusu isteğe ek sorgu dizesi parametreleri ekleyin, doldurmak için `query.parameters` şu şekilde:

**Objective-C**:

```objc
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Swift**:

```swift
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Nasıl Yapılır: Sayfa boyutunu yapılandırma

Azure Mobile Apps ile sayfa boyutu, arka uç tabloları aynı anda çekilen kayıt sayısını denetler. Bir çağrı `pull` veri kalmayana kadar daha fazla kayıt çekmek için bu sayfa boyutuna göre verileri, ardından batch.

Sayfa boyutu kullanarak yapılandırmak mümkündür **MSPullSettings** aşağıda gösterildiği gibi. Varsayılan sayfa boyutu 50'dir ve isteğe bağlı olarak aşağıdaki örnekte 3 olarak değiştirir.

Performansı artırmak için farklı sayfa boyutu yapılandırabilirsiniz. Çok sayıda küçük veri kayıtlarının varsa, sunucu gidiş dönüş sayısının yüksek sayfa boyutunu azaltır.

Bu ayar, yalnızca istemci tarafında sayfa boyutunu denetler. Mobile Apps arka desteklediğinden daha daha büyük bir sayfa boyutu için istemci isterse, sayfa boyutu arka uç destekleyecek şekilde yapılandırılmış ücret, tavan.

Bu ayar aynı zamanda olan *numarası* veri kayıtlarının değil *bayt boyutu*.

İstemci sayfa boyutunu artırmak için sunucuda sayfa boyutunu da yükseltmeniz gerekir. Bkz: ["nasıl yapılır: Tablo disk belleği boyutunu Ayarla"](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Bunu yapmak adımlar.

**Objective-C**:

```objc
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                }
                           }];
```

**Swift**:

```swift
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    }
}
```

## <a name="inserting"></a>Nasıl Yapılır: Veri ekleme

Yeni bir tablo satır eklemek için oluşturun bir `NSDictionary` ve çağırma `table insert`. Varsa [Dinamik düzen] olan etkin, Azure App Service mobil arka ucu otomatik olarak temel yeni sütunları oluşturur `NSDictionary`.

Varsa `id` arka uç otomatik olarak yeni bir benzersiz kimlik oluşturur değil sağlanır Kendi sağlamak `id` e-posta kullanmak için kullanıcı adları, adresleri veya kendi özel değerler kimliği. Kendi Kimliğinizi sağlayarak, birleştirmeler ve işle ilgili veritabanı mantığı kolaylaştırmak.

`result` Eklenmiş yeni bir öğe içeriyor. Sunucu mantığınızı bağlı olarak, sunucuya geçirilen için ne kıyasla ek veya değiştirilmiş verileri bulunabilir.

**Objective-C**:

```objc
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```swift
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

## <a name="modifying"></a>Nasıl Yapılır: Verileri değiştirme

Mevcut bir satırı güncelleştirmek için bir öğe ve çağrı değiştirin `update`:

**Objective-C**:

```objc
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```swift
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Alternatif olarak, satır kimliği ve güncelleştirilen alan sağlayın:

**Objective-C**:

```objc
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```swift
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

En azından `id` güncelleştirmeler yaparken özniteliği ayarlanmalıdır.

## <a name="deleting"></a>Nasıl Yapılır: Verileri Sil

Bir öğeyi silmek için çağırma `delete` öğe:

**Objective-C**:

```objc
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```swift
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Alternatif olarak, satır kimliği sağlayarak silin:

**Objective-C**:

```objc
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```swift
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

En azından `id` yapma işlemi sildiğinde özniteliği ayarlanmalıdır.

## <a name="customapi"></a>Nasıl Yapılır: Özel API çağırma

Özel bir API ile herhangi bir arka uç işlevsellik kullanıma sunabilirsiniz. Bir tablo işlemine eşlemeye sahip değil. Yalnızca, ileti üzerinde daha fazla denetim elde edebilir, üstelik okuma/set üst bilgileri ve yanıt gövdesi biçiminin değiştirin. Arka uçta özel API oluşturma konusunda bilgi edinmek için [özel API'ler](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Özel API çağırmak için çağrı `MSClient.invokeAPI`. İstek ve yanıt içeriği JSON olarak kabul edilir. Diğer medya türleri kullanılacak [bir aşırı yüklemesini kullanmanız `invokeAPI` ] [ 5].  Yapmak için bir `GET` yerine isteği bir `POST` kümesi parametre istek `HTTPMethod` için `"GET"` ve parametre `body` için `nil` (GET istekleri İleti gövdeleri olmadığından.) Özel API'nizi diğer HTTP fiilleri destekliyorsa, değiştirme `HTTPMethod` uygun şekilde.

**Objective-C**:

```objc
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Swift**:

```swift
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

## <a name="templates"></a>Nasıl Yapılır: Çapraz platform bildirimleri göndermek için kayıt anında iletme şablonları

Şablonları kaydetmek için şablonlarıyla geçirmek, **client.push registerDeviceToken** istemci uygulamanıza yöntemi.

**Objective-C**:

```objc
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Swift**:

```swift
client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
    if let err = error {
        print("ERROR ", err)
    }
})
```

Şablonlarınızı NSDictionary türüdür ve birden fazla şablon aşağıdaki biçimde içerebilir:

**Objective-C**:

```objc
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Swift**:

```swift
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Tüm etiketleri, istek güvenlik kaldırılır.  Yüklemeleri veya yüklemeleri şablonlarında etiket eklemek için bkz [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma][4].  Kayıtlı bu şablonları kullanarak bildirim göndermek için çalışmak [bildirim hub'ları API][3].

## <a name="errors"></a>Nasıl Yapılır: Hatalarını işleme

Azure App Service mobil arka ucu çağırdığınızda tamamlama bloğu içeren bir `NSError` parametresi. Bir hata oluştuğunda, bu parametre nil olmayan içindir. Kodunuzda bu parametreyi denetleyin ve önceki kod parçacıklarında gösterildiği gibi gerektiği gibi hatayı işleyebilir.

Dosya [ `<WindowsAzureMobileServices/MSError.h>` ] [ 6] sabitlerini tanımlar `MSErrorResponseKey`, `MSErrorRequestKey`, ve `MSErrorServerItemKey`. Hatayla ilgili daha fazla veri almak için:

**Objective-C**:

```objc
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Swift**:

```swift
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Ayrıca, dosyanın her hata kodunu sabitlerini tanımlar:

**Objective-C**:

```objc
if (error.code == MSErrorPreconditionFailed) {
```

**Swift**:

```swift
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Nasıl Yapılır: Kullanıcıların Active Directory kimlik doğrulama kitaplığı ile kimlik doğrulaması

Kullanıcıların uygulamanızla Azure Active Directory'yi kullanarak oturum açmak için Active Directory Authentication Library (ADAL) kullanabilirsiniz. Bir kimlik sağlayıcısına SDK'sını kullanarak istemci akışı kimlik doğrulaması kullanılması tercih `loginWithProvider:completion:` yöntemi.  İstemci akışı kimlik doğrulaması, daha doğal bir UX görünümünü sağlar ve için ek özelleştirme yapmanıza izin verir.

1. AAD oturum açma için mobil uygulama arka ucunuzu izleyerek yapılandırın [App Service, Active Directory oturum açma için yapılandırma] [ 7] öğretici. Yerel istemci uygulaması kaydetme isteğe bağlı bir adım tamamladığınızdan emin olun. İOS için yeniden yönlendirme URI'si biçimi olan öneririz `<app-scheme>://<bundle-id>`. Daha fazla bilgi için [ADAL iOS hızlı][8].
2. Cocoapods kullanarak ADAL'ı yükleyin. Aşağıdaki tanım, dahil etmek için Podfile Düzenle değiştirerek **YOUR PROJECT** Xcode projenizin adı:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   ve Pod:

        pod 'ADALiOS'

3. Terminali kullanarak çalıştırma `pod install` dizinden projenizi içeren ve oluşturulan Xcode çalışma alanı (projeye değil) açın.
4. Kullanmakta olduğunuz diline göre uygulamanız için aşağıdaki kodu ekleyin. Her, bu değişiklik yapın:

   * Değiştirin **INSERT yetkilisi burada** uygulamanızı sağlanan Kiracı adı. Biçim olmalıdır https://login.microsoftonline.com/contoso.onmicrosoft.com. Bu değer, Azure Active Directory etki alanı sekmesinden kopyalanabilir [Azure portal].
   * Değiştirin **Ekle-RESOURCE-kimliği-Buraya** mobil uygulamanızın arka ucu için istemci kimliği. İstemci kimliği edinebilirsiniz **Gelişmiş** sekmesinde altında **Azure Active Directory ayarları** portalında.
   * Değiştirin **istemci kimliği burayı INSERT** yerel istemci uygulamasından kopyaladığınız istemci kimliği.
   * Değiştirin **ekleme-yeniden yönlendirme-URI-Buraya** sitenizin ile */.auth/login/done* uç noktasını, HTTPS düzenini kullanarak. Bu değer, aşağıdakine benzer olmalıdır *https://contoso.azurewebsites.net/.auth/login/done* .

**Objective-C**:

```objc
#import <ADALiOS/ADAuthenticationContext.h>
#import <ADALiOS/ADAuthenticationSettings.h>
// ...
- (void) authenticate:(UIViewController*) parent
            completion:(void (^) (MSUser*, NSError*))completionBlock;
{
    NSString *authority = @"INSERT-AUTHORITY-HERE";
    NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
    NSString *clientId = @"INSERT-CLIENT-ID-HERE";
    NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
    ADAuthenticationError *error;
    ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
    authContext.parentController = parent;
    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:resourceId
                                    clientId:clientId
                                redirectUri:redirectUri
                            completionBlock:^(ADAuthenticationResult *result) {
                                if (result.status != AD_SUCCEEDED)
                                {
                                    completionBlock(nil, result.error);;
                                }
                                else
                                {
                                    NSDictionary *payload = @{
                                                            @"access_token" : result.tokenCacheStoreItem.accessToken
                                                            };
                                    [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                }
                            }];
}
```

**Swift**:

```swift
// add the following imports to your bridging header:
//        #import <ADALiOS/ADAuthenticationContext.h>
//        #import <ADALiOS/ADAuthenticationSettings.h>

func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
    let authority = "INSERT-AUTHORITY-HERE"
    let resourceId = "INSERT-RESOURCE-ID-HERE"
    let clientId = "INSERT-CLIENT-ID-HERE"
    let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
    var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
    let authContext = ADAuthenticationContext(authority: authority, error: error)
    authContext.parentController = parent
    ADAuthenticationSettings.sharedInstance().enableFullScreen = true
    authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
            if result.status != AD_SUCCEEDED {
                completion(nil, result.error)
            }
            else {
                let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                client.loginWithProvider("aad", token: payload, completion: completion)
            }
        }
}
```

## <a name="facebook-sdk"></a>Nasıl Yapılır: Kullanıcılar iOS için Facebook SDK ile kimlik doğrulaması

Uygulamanıza Facebook kullanarak kullanıcıların oturum açmak için iOS için Facebook SDK'sını kullanabilirsiniz.  Bir istemci akışı kimlik doğrulaması kullanarak kullanılması tercih `loginWithProvider:completion:` yöntemi.  İstemci akışı kimlik doğrulaması, daha doğal bir UX görünümünü sağlar ve için ek özelleştirme yapmanıza izin verir.

1. Facebook oturum açma için mobil uygulama arka ucunuzu izleyerek yapılandırın [App Service Facebook oturum açma için yapılandırma] [ 9] öğretici.
2. İOS için Facebook SDK izleyerek yükleme [Facebook SDK'sı iOS - Başlarken] [ 10] belgeleri. Bir uygulama oluşturmak yerine, varolan bir kaydı için iOS platformunu ekleyebilirsiniz.
3. Facebook'ın belgeleri, uygulama Temsilcide bazı Objective-C kodunu içerir. Kullanıyorsanız **Swift**, aşağıdaki çevirileri AppDelegate.swift için kullanabilirsiniz:

    ```swift
    // Add the following import to your bridging header:
    //        #import <FBSDKCoreKit/FBSDKCoreKit.h>

    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
        FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
        // Add any custom logic here.
        return true
    }

    func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
        let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
        // Add any custom logic here.
        return handled
    }
    ```
4. Ekleme yanı sıra `FBSDKCoreKit.framework` projenize bir başvuru da ekleyin `FBSDKLoginKit.framework` aynı şekilde.
5. Kullanmakta olduğunuz diline göre uygulamanız için aşağıdaki kodu ekleyin.

    **Objective-C**:

    ```objc
    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
                completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
            logInWithReadPermissions: @[@"public_profile"]
            fromViewController:parent
            handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
                if (error) {
                    completionBlock(nil, error);
                } else if (result.isCancelled) {
                    completionBlock(nil, error);
                } else {
                    NSDictionary *payload = @{
                                            @"access_token":result.token.tokenString
                                            };
                    [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
                }
            }];
    }
    ```

    **Swift**:

    ```swift
    // Add the following imports to your bridging header:
    //        #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //        #import <FBSDKCoreKit/FBSDKAccessToken.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }
    ```

## <a name="twitter-fabric"></a>Nasıl Yapılır: İOS için kullanıcılara Fabric Twitter ile kimlik doğrulaması

İOS için Fabric uygulamanıza Twitter kullanarak kullanıcıların oturum açmak için kullanabilirsiniz. İstemci akışı kimlik doğrulaması kullanılması tercih `loginWithProvider:completion:` metot, daha doğal bir UX görünümünü sağlar ve için ek özelleştirme yapmanıza izin verir.

1. Twitter oturum açma için mobil uygulama arka ucunuzu izleyerek yapılandırın [App Service Twitter oturum açma için yapılandırma](../app-service/configure-authentication-provider-twitter.md) öğretici.
2. İzleyerek projenize doku ekleme [Fabric iOS - Başlarken] belgeler ve TwitterKit ayarı.

   > [!NOTE]
   > Varsayılan olarak, doku bir Twitter uygulaması sizin için oluşturur. Tüketici anahtarı ve tüketici gizli daha önce aşağıdaki kod parçacıkları kullanarak oluşturduğunuz kayıt olarak uygulama oluşturma önleyebilirsiniz.    Alternatif olarak, App Service içinde gördüğünüz değerleri sağlayan tüketici anahtarı ve tüketici gizli bilgisi değerleri değiştirebilirsiniz [Yapı Panosu]. Bu seçeneği seçerseniz, geri çağırma URL'si için bir yer tutucu değerini, aşağıdaki gibi ayarladığınızdan emin olması `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.

    Daha önce oluşturduğunuz gizli dizileri kullanmayı seçerseniz, uygulama temsilciniz aşağıdaki kodu ekleyin:

    **Objective-C**:

    ```objc
    #import <Fabric/Fabric.h>
    #import <TwitterKit/TwitterKit.h>
    // ...
    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
        [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
        [Fabric with:@[[Twitter class]]];
        // Add any custom logic here.
        return YES;
    }
    ```

    **Swift**:

    ```swift
    import Fabric
    import TwitterKit
    // ...
    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
        Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
        Fabric.with([Twitter.self])
        // Add any custom logic here.
        return true
    }
    ```

3. Kullanmakta olduğunuz diline göre uygulamanız için aşağıdaki kodu ekleyin.

    **Objective-C**:

    ```objc
    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }
    ```

    **Swift**:

    ```swift
    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }
    ```

## <a name="google-sdk"></a>Nasıl Yapılır: Kullanıcılar iOS için Google oturum açma SDK ile kimlik doğrulaması

İOS için Google oturum açma SDK, kullanıcıların uygulamanıza bir Google hesabı kullanarak oturum açmak için kullanabilirsiniz.  Google OAuth güvenlik ilkelerini değişiklikleri kısa süre önce duyurulan.  Bu ilke değişikliklerini gelecekte Google SDK'sının kullanılması gerekir.

1. Google oturum açma için mobil uygulama arka ucunuzu izleyerek yapılandırın [App Service Google oturum açma için yapılandırma](../app-service/configure-authentication-provider-google.md) öğretici.
2. İOS için Google SDK izleyerek yükleme [Google oturum açma - iOS için başlangıç tümleştirme](https://developers.google.com/identity/sign-in/ios/start-integrating) belgeleri. "Kimlik doğrulaması ile bir arka uç sunucu" bölümüne atlayabilirsiniz.
3. Aşağıdakileri ekleyin, temsilcinin `signIn:didSignInForUser:withError:` yöntemi, kullanmakta olduğunuz diline göre.

    **Objective-C**:
    ```objc
    NSDictionary *payload = @{
                                @"id_token":user.authentication.idToken,
                                @"authorization_code":user.serverAuthCode
                                };

    [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
        // ...
    }];
    ```

    **Swift**:

    ```swift
    let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
    client.loginWithProvider("google", token: payload) { (user, error) in
        // ...
    }
    ```

4. Şu şekilde de eklediğinizden emin olun `application:didFinishLaunchingWithOptions:` "SERVER_CLIENT_ID" Adım 1 App Service'ı yapılandırmak için kullanılan aynı kimliği yerine uygulama temsilcinizde.

    **Objective-C**:

    ```objc
    [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
    ```

     **Swift**:

    ```swift
    GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"
    ```

5. Aşağıdaki kod uygulayan bir uıviewcontroller'ı uygulamanıza eklemek `GIDSignInUIDelegate` protokolü, kullanmakta olduğunuz diline göre.  Yeniden oturum açmayı önce oturum kapatıldı ve kimlik bilgilerinizi yeniden girmeniz gerekmez ancak bir onay iletişim kutusunu görürsünüz.  Oturum belirteci süresi dolduğunda, yalnızca bu yöntemi çağırın.

   **Objective-C**:

    ```objc
    #import <Google/SignIn.h>
    // ...
    - (void)authenticate
    {
            [GIDSignIn sharedInstance].uiDelegate = self;
            [[GIDSignIn sharedInstance] signOut];
            [[GIDSignIn sharedInstance] signIn];
    }
    ```

   **Swift**:

    ```swift
    // ...
    func authenticate() {
        GIDSignIn.sharedInstance().uiDelegate = self
        GIDSignIn.sharedInstance().signOut()
        GIDSignIn.sharedInstance().signIn()
    }
    ```

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure Mobile Apps hızlı başlangıç]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode
[Azure portal]: https://portal.azure.com/
[Handling Expired Tokens]: https://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: https://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: https://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dinamik düzen]: https://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: https://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: https://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: https://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: /cli/azure/get-started-with-az-cli2
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Yapı Panosu]: https://www.fabric.io/home
[Fabric iOS - Başlarken]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: https://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: https://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: ../app-service/configure-authentication-provider-aad.md
[8]:../active-directory/develop/quickstart-v1-ios.md
[9]: ../app-service/configure-authentication-provider-facebook.md
[10]: https://developers.facebook.com/docs/ios/getting-started
