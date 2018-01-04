---
title: "Kullanım iOS SDK'sı Azure mobil uygulamalar için nasıl"
description: "Kullanım iOS SDK'sı Azure mobil uygulamalar için nasıl"
services: app-service\mobile
documentationcenter: ios
author: ysxu
manager: yochayk
editor: 
ms.assetid: 4e8e45df-c36a-4a60-9ad4-393ec10b7eb9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: yuaxu
ms.openlocfilehash: 63dd283605553297a7dc8feab90c8bcbd716d5de
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Nasıl yapılır kullanım iOS için Azure Mobile Apps istemci kitaplığı
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

En son kullanılarak yaygın senaryolar gerçekleştirmek için bu kılavuzu öğretilmektedir [Azure Mobile Apps iOS SDK'sı][1]. Azure Mobile Apps yeniyseniz, ilk tamamlamak [Azure Mobile Apps Hızlı Başlangıç] bir arka uç oluşturmak için bir tablo oluşturun ve önceden derlenmiş iOS Xcode projenizi indirin. Bu kılavuzda, istemci-tarafı iOS SDK'sı odaklanın. Arka uç için sunucu tarafı SDK'sı hakkında daha fazla bilgi için sunucu SDK HOWTOs bakın.

## <a name="reference-documentation"></a>Başvuru belgeleri
İOS istemci SDK'sı için başvuru belgeleri burada bulunur: [Azure Mobile Apps iOS istemci başvurusu][2].

## <a name="supported-platforms"></a>Desteklenen platformlar
İOS SDK'sı iOS 8.0 veya sonraki sürümleri için Objective-C projeleri, Swift 2.2 projeler ve Swift 2.3 projeleri destekler.

"Sunucu akış" kimlik doğrulaması bir Web görünümü için sunulan kullanıcı arabirimini kullanır.  Kimlik doğrulama başka bir yöntem gereklidir cihaz WebView Arabirim sunmak mümkün değilse, ürün kapsamı dışında olmasıdır.  
Bu SDK böylece Gözcü türü veya benzer şekilde kısıtlı aygıtlar için uygun değil.

## <a name="Setup"></a>Kurulum ve Önkoşullar
Bu kılavuz, bir tablo ile bir arka uç oluşturduğunuzu varsayar. Bu kılavuz tablo bu öğreticiler aynı şemaya tabloları sahip olduğunu varsayar. Bu kılavuz Ayrıca, kodunuzda, başvuru varsayar `MicrosoftAzureMobile.framework` ve içeri aktarma `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

## <a name="create-client"></a>Nasıl yapılır: istemci oluşturma
Azure Mobile Apps arka projenize erişmek için oluşturma bir `MSClient`. Değiştir `AppUrl` uygulama URL'si. Bırakabilir `gatewayURLString` ve `applicationKey` boş. Kimlik doğrulaması için bir ağ geçidi ayarladıysanız, doldurmak `gatewayURLString` ağ geçidi URL'si.

**Objective-C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**SWIFT**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


## <a name="table-reference"></a>Nasıl yapılır: Tablo başvurusu oluşturma
Verilere erişmek veya verileri güncelleştirmek için arka uç tablosuna başvuru oluşturun. `TodoItem` ifadesini tablonuzun adıyla değiştirin

**Objective-C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**SWIFT**:

```
let table = client.tableWithName("TodoItem")
```


## <a name="querying"></a>Nasıl yapılır: veri sorgulama
Bir veritabanı sorgusu oluşturmak için sorgu `MSTable` nesnesi. Aşağıdaki sorgu tüm öğeleri alır `TodoItem` ve her öğenin metnini günlüğe kaydeder.

**Objective-C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**SWIFT**:

```
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

## <a name="filtering"></a>Nasıl yapılır: Filtre veri döndürdü
Sonuçları filtrelemek için kullanılabilir pek çok seçenek vardır.

Bir koşulu kullanarak filtrelemek için kullanmak bir `NSPredicate` ve `readWithPredicate`. Aşağıdaki filtreler yalnızca tamamlanmamış Yapılacaklar öğelerini bulmak için veri döndürdü.

**Objective-C**:

```
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

**SWIFT**:

```
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

## <a name="query-object"></a>Nasıl yapılır: MSQuery kullanın
Karmaşık bir sorgu (sıralama dahil ve disk belleği) gerçekleştirmek oluşturmak için bir `MSQuery` nesnesi, doğrudan veya bir koşulu kullanarak:

**Objective-C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**SWIFT**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`birkaç sorgu davranışları denetlemenize olanak tanır.

* Sonuçları sırasını belirtin
* Döndürülecek alanları sınırı
* Döndürülecek kaç kayıtları sınırla
* Yanıtta toplam sayısı belirtin
* İstekte özel sorgu dizesi parametreleri belirtin
* Ek işlevler Uygula

Yürütme bir `MSQuery` çağırarak sorgu `readWithCompletion` nesne üzerinde.

## <a name="sorting"></a>Nasıl yapılır: MSQuery ile verileri sıralama
Sonuçları sıralamak için bir örneğe bakalım. Alan 'text' artan sonra 'Tamamlandı' azalan göre sıralamak için çağırma `MSQuery` sözlüğüdür:

**Objective-C**:

```
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

**SWIFT**:

```
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


## <a name="selecting"></a><a name="parameters"></a>Nasıl yapılır: alanları sınırlayabilir ve sorgu dizesi parametreleri MSQuery ile genişletin
Sorguda döndürülecek alanları sınırlamak için alanların adlarını belirtin **selectFields** özelliği. Bu örnek yalnızca metin ve tamamlanan alanları döndürür:

**Objective-C**:

```
query.selectFields = @[@"text", @"complete"];
```

**SWIFT**:

```
query.selectFields = ["text", "complete"]
```

(Örneğin, özel bir sunucu tarafı komut dosyası bunları kullandığından) sunucu istek ek sorgu dizesi parametreleri dahil, doldurmak için `query.parameters` sözlüğüdür:

**Objective-C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**SWIFT**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Nasıl yapılır: sayfa boyutu yapılandırın
Azure Mobile Apps ile sayfa boyutu arka uç tablolar aynı anda çekilen kayıt sayısını denetler. Çağrı `pull` veri çekmek için daha fazla kayıt kadar bu sayfa boyutuna göre verileri, ardından toplu.

Sayfa boyutu kullanarak yapılandırılması mümkündür **MSPullSettings** aşağıda gösterildiği gibi. Varsayılan sayfa boyutu 50'dir ve isteğe bağlı olarak aşağıdaki örnek 3'e dönüşür.

Farklı bir sayfa boyutu performans nedenleriyle yapılandırabilirsiniz. Çok sayıda küçük veri kayıt varsa, yüksek sayfa boyutu sunucu gidiş dönüş sayısını azaltır.

Bu ayar, yalnızca istemci tarafında sayfa boyutu denetler. İstemci Mobile Apps arka uç desteklediğinden daha büyük sayfa için bir boyut isterse, sayfa boyutu arka uç desteklemek üzere yapılandırılmış üst sınırda tutulabilir.

Bu ayar ayrıca olan *numarası* veri kayıtlarının değil *bayt boyutu*.

İstemci sayfa boyutunu artırmak için sunucu üzerindeki sayfa boyutunu artırmanız gerekir. Bkz: ["nasıl yapılır: Tablo disk belleği boyutunu Ayarla"](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) bunu yapma adımları için.

**Objective-C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                }
                           }];
```


**SWIFT**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    }
}
```

## <a name="inserting"></a>Nasıl yapılır: Veri Ekle
Yeni bir tablo satır eklemek için Oluştur bir `NSDictionary` ve çağırma `table insert`. Varsa [dinamik şema] olan etkinse, Azure App Service mobil arka uç otomatik olarak yeni sütunlar göre oluşturur `NSDictionary`.

Varsa `id` arka uç otomatik olarak yeni bir benzersiz kimlik oluşturur değil sağlanır Kendi sağlamak `id` e-posta kullanmak için kullanıcı adları, adresleri veya kendi özel değerler kimliği. Kendi Kimliğinizi sağlayarak, birleştirmeler ve işle ilgili veritabanı mantığı kolaylaştırır.

`result` Eklenen yeni öğe içeriyor. Sunucu mantığınızı bağlı olarak, sunucuya geçirilen için ne kıyasla ek veya değiştirilmiş veri olabilir.

**Objective-C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**SWIFT**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

## <a name="modifying"></a>Nasıl yapılır: verileri değiştirme
Varolan bir satır güncelleştirmek için bir öğe ve çağrı değiştirmeniz `update`:

**Objective-C**:

```
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

**SWIFT**:

```
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

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**SWIFT**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

En azından `id` güncelleştirmeleri yaparken özniteliği ayarlanmalıdır.

## <a name="deleting"></a>Nasıl yapılır: verileri Sil
Bir öğeyi silmek için çağırma `delete` öğe:

**Objective-C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**SWIFT**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Alternatif olarak, satır kimliği sağlayarak Sil:

**Objective-C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**SWIFT**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

En azından `id` yapma sildiğinde özniteliği ayarlanmalıdır.

## <a name="customapi"></a>Nasıl yapılır: özel API çağrısı
Özel bir API ile herhangi bir arka uç işlevsellik getirebilir. Bir tablo işlemi eşlemek sahip değil. Yalnızca ileti üzerinde daha fazla denetim kazanmak yapın, hatta okuma/kümesi üstbilgileri ve yanıt gövdesi biçimini değiştirin. Arka uçta özel bir API oluşturmayı öğrenmek için okuma [özel API'leri](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Özel bir API çağrısı için çağrı `MSClient.invokeAPI`. İstek ve yanıt içerik JSON olarak kabul edilir. Diğer medya türleri kullanılacak [bir aşırı yüklemesini kullanın `invokeAPI` ] [ 5].  Yapmak için bir `GET` yerine isteği bir `POST` isteği, kümesi parametresi `HTTPMethod` için `"GET"` ve parametre `body` için `nil` (GET istekleri İleti gövdeleri değil olduğundan.) Özel API'nizi diğer HTTP fiilleri destekliyorsa, değiştirme `HTTPMethod` uygun şekilde.

**Objective-C**:

```
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

**SWIFT**:

```
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

## <a name="templates"></a>Nasıl yapılır: platformlar arası bildirimleri göndermek için kayıt anında şablonları
Geçişi şablonlarıyla şablonlarını kaydetmek için **client.push registerDeviceToken** istemci uygulamanızı yöntemi.

**Objective-C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**SWIFT**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

Şablonlarınızı türü NSDictionary ve şu biçimde birden fazla şablon içerebilir:

**Objective-C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**SWIFT**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Tüm etiketleri güvenlik için istek kaldırılır.  Yüklemeleri veya şablonları yüklemeleri içinde etiketleri eklemek için bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş][4].  Kayıtlı bu şablonları kullanarak bildirim göndermek için çalışmak [bildirim hub'ları API'leri][3].

## <a name="errors"></a>Nasıl yapılır: hata işleme
Azure uygulama hizmeti mobil arka uç çağırdığınızda, tamamlama bloğu içeren bir `NSError` parametresi. Hata oluştuğunda, bu parametre nil olmayan olur. Kodunuzda bu parametreyi denetleyin ve önceki kod parçacıkları gösterildiği gibi gerektiği gibi hatayı işleyebilir.

Dosya [ `<WindowsAzureMobileServices/MSError.h>` ] [ 6] sabitleri tanımlar `MSErrorResponseKey`, `MSErrorRequestKey`, ve `MSErrorServerItemKey`. Hata ile ilgili daha fazla veri almak için:

**Objective-C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**SWIFT**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Ayrıca, dosyanın her hata kodunu sabitleri tanımlar:

**Objective-C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**SWIFT**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Nasıl yapılır: kullanıcıların Active Directory kimlik doğrulama kitaplığı ile kimlik doğrulaması
Kullanıcılar Azure Active Directory'yi kullanarak uygulamanıza oturum için Active Directory Authentication Library (ADAL) kullanabilirsiniz. Bir kimlik sağlayıcısı SDK kullanarak istemci akış kimlik doğrulaması kullanılması tercih `loginWithProvider:completion:` yöntemi.  İstemci akış kimlik doğrulaması daha yerel bir UX fikir sağlar ve ek özelleştirme için sağlar.

1. İzleyerek, mobil uygulamanızın arka ucuna AAD oturum açma için yapılandırma [uygulama hizmeti Active Directory oturum açma için yapılandırma] [ 7] Öğreticisi. İsteğe bağlı bir adım yerel istemci uygulaması kaydı tamamlamak emin olun. İOS için biçiminde yeniden yönlendirme öneririz `<app-scheme>://<bundle-id>`. Daha fazla bilgi için bkz: [ADAL iOS quickstart][8].
2. ADAL Cocoapods kullanarak yükleyin. Aşağıdaki tanımını eklemek için Podfile Düzenle değiştirme **YOUR proje** Xcode projenizi adı:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   ve Pod:

        pod 'ADALiOS'
3. Terminal kullanarak çalıştırmanız `pod install` dizinden projenizi içeren ve oluşturulan Xcode çalışma alanı (projeye değil) açın.
4. Kullanmakta olduğunuz diline göre uygulamanıza aşağıdaki kodu ekleyin. Her, bu değişiklikleri yapın:

   * Değiştir **INSERT yetkilisi burada** uygulamanızı sağlanan Kiracı adı. Https://login.microsoftonline.com/contoso.onmicrosoft.com biçiminde olmalıdır. Bu değer, Azure Active Directory etki alanı sekmesinden kopyalanabilir [Azure portal].
   * Değiştir **Ekle-RESOURCE-kimliği-Buraya** , mobil uygulamanızın arka ucuna için istemci kimliği. İstemci kimliği elde edebilirsiniz **Gelişmiş** altında sekmesinde **Azure Active Directory ayarları** Portalı'nda.
   * Değiştir **Ekle-istemci-kimliği-Buraya** yerel istemci uygulamasından kopyaladığınız istemci kimliği.
   * Değiştir **Ekle-REDIRECT-URI-Buraya** sitenizin ile */.auth/login/done* uç noktasını, HTTPS şeması kullanarak. Bu değer benzer olmalıdır *https://contoso.azurewebsites.net/.auth/login/done*.

**Objective-C**:

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


**SWIFT**:

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

## <a name="facebook-sdk"></a>Nasıl yapılır: kullanıcılar iOS için Facebook SDK'sı ile kimlik doğrulaması
İOS için Facebook SDK'sı, kullanıcıların Facebook kullanarak uygulamanıza oturum için kullanabilirsiniz.  Bir istemci akış kimlik doğrulaması kullanarak kullanılması tercih `loginWithProvider:completion:` yöntemi.  İstemci akış doğrulaması daha yerel bir UX fikir sağlar ve ek özelleştirme için sağlar.

1. İzleyerek, mobil uygulamanızın arka ucuna Facebook oturum açma için yapılandırma [Facebook oturum açma için uygulama hizmeti yapılandırmak nasıl] [ 9] Öğreticisi.
2. İOS için Facebook SDK'sı izleyerek yükleyin [Facebook SDK'sı iOS - Getting Started] [ 10] belgeleri. Bir uygulama oluşturmak yerine var olan kaydınızı iOS platformu ekleyebilirsiniz.
3. Facebook'ın belgeleri uygulama temsilcisi bazı Objective-C kodunu içerir. Kullanıyorsanız **Swift**, aşağıdaki çevirileri AppDelegate.swift için kullanabilirsiniz:

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
4. Ekleme yanı sıra `FBSDKCoreKit.framework` projeniz için de bir başvuru ekleyin `FBSDKLoginKit.framework` aynı şekilde.
5. Kullanmakta olduğunuz diline göre uygulamanıza aşağıdaki kodu ekleyin.

**Objective-C**:

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

**SWIFT**:

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

## <a name="twitter-fabric"></a>Nasıl yapılır: iOS için kullanıcılara Twitter doku ile kimlik doğrulaması
Twitter kullanarak uygulamanıza kullanıcılar oturum için yapı iOS için kullanabilirsiniz. İstemci akış kimlik doğrulaması kullanılması tercih `loginWithProvider:completion:` şekliyle yöntemi daha yerel bir UX fikir sağlar ve ek özelleştirme için sağlar.

1. İzleyerek, mobil uygulamanızın arka ucuna Twitter oturum açma için yapılandırma [Twitter oturum açma için uygulama hizmeti yapılandırmak nasıl](../app-service/app-service-mobile-how-to-configure-twitter-authentication.md) Öğreticisi.
2. Doku izleyerek projenize eklemek [iOS - Getting Started için doku] belgeleri ve TwitterKit ayarlama.

   > [!NOTE]
   > Varsayılan olarak, doku bir Twitter uygulaması sizin için oluşturur. Tüketici anahtarı ve tüketici gizli daha önce aşağıdaki kod parçacıkları kullanılarak oluşturulan kaydederek bir uygulama oluşturmaya önleyebilirsiniz.    Alternatif olarak, uygulama hizmeti içinde gördüğünüz değerlerle sağlamak tüketici anahtarı ve tüketici gizli değerleri değiştirebilirsiniz [doku Panosu]. Bu seçeneği seçerseniz, aşağıdaki gibi bir yer tutucu değerini için geri çağırma URL'si ayarladığınızdan emin olun `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.
   >
   >

    Daha önce oluşturduğunuz gizli anahtarları kullanmayı seçerseniz, uygulama temsilciniz aşağıdaki kodu ekleyin:

    **Objective-C**:

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

    **SWIFT**:

        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
3. Kullanmakta olduğunuz diline göre uygulamanıza aşağıdaki kodu ekleyin.

**Objective-C**:

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

**SWIFT**:

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

## <a name="google-sdk"></a>Nasıl yapılır: kullanıcılar iOS için Google oturum açma SDK ile kimlik doğrulaması
Kullanıcılar uygulamanıza bir Google hesabıyla oturum için oturum açma Google SDK iOS için kullanabilirsiniz.  Google OAuth güvenlik ilkelerini değişiklikler son duyurdu.  Bu ilke değişikliklerini Google SDK'sı kullanımını gelecekte gerektirir.

1. İzleyerek, mobil uygulamanızın arka ucuna Google oturum açma için yapılandırma [Google oturum açma için uygulama hizmeti yapılandırmak nasıl](../app-service/app-service-mobile-how-to-configure-google-authentication.md) Öğreticisi.
2. İOS için Google SDK'sı izleyerek yükleyin [Google oturum açma iOS için-başlangıç tümleştirme](https://developers.google.com/identity/sign-in/ios/start-integrating) belgeleri. "Kimlik doğrulaması ile bir arka uç sunucu" bölümüne atlayabilirsiniz.
3. Aşağıdaki, temsilcinin ekleyin `signIn:didSignInForUser:withError:` kullanmakta olduğunuz diline göre yöntemi.

**Objective-C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };

        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**SWIFT**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

1. Aşağıdakiler için de eklediğinizden emin olun `application:didFinishLaunchingWithOptions:` "SERVER_CLIENT_ID" 1. adımda uygulama hizmeti yapılandırmak için kullanılan aynı Kimliğe sahip değiştirerek uygulama temsilciniz olarak.

**Objective-C**:

         [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";

 **SWIFT**:

        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"


1. Arabirimini uygulayan bir UIViewController uygulamanızda aşağıdaki kodu ekleyin `GIDSignInUIDelegate` protokolü, kullanmakta olduğunuz diline göre.  Yeniden imzalanmakta önce oturumu kapattınız ve kimlik bilgilerinizi yeniden girmeniz gerekmez ancak, bir onay iletişim kutusu görebilirsiniz.  Oturum belirteci süresi dolan yalnızca bu yöntemi çağırın.

   **Objective-C**:

       #import <Google/SignIn.h>
       // ...
       - (void)authenticate
       {
               [GIDSignIn sharedInstance].uiDelegate = self;
               [[GIDSignIn sharedInstance] signOut];
               [[GIDSignIn sharedInstance] signIn];
        }

   **SWIFT**:

       // ...
       func authenticate() {
           GIDSignIn.sharedInstance().uiDelegate = self
           GIDSignIn.sharedInstance().signOut()
           GIDSignIn.sharedInstance().signIn()
       }

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
[Azure Mobile Apps Hızlı Başlangıç]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode
[Azure portal]: https://portal.azure.com/
[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[dinamik şema]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: /cli/azure/get-started-with-az-cli2
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[doku Panosu]: https://www.fabric.io/home
[iOS - Getting Started için doku]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md
[9]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
