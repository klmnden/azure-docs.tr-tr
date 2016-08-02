<properties
    pageTitle="Azure Active Directory B2C Önizlemesi: Bir iOS uygulamasından web API'si çağırma | Microsoft Azure"
    description="Bu makale size OAuth 2.0 taşıyıcı belirteçlerini kullanarak Node.js web API'si çağıran iOS 'yapılacaklar listesi' uygulamasının nasıl oluşturulacağını gösterir. iOS uygulaması ve web API'si, kullanıcı kimliklerini yönetmek ve kullanıcıların kimliğini doğrulamak için Azure Active Directory B2C kullanır."
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags ms.service="active-directory-b2c" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic="hero-article"

    ms.date="05/31/2016"
    ms.author="brandwe"/>

# Azure AD B2C önizlemesi: iOS uygulamasından web API'si çağrıma

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Azure Active Directory (Azure AD) B2C kullanarak iOS uygulamalarınıza ve web API'lerinize birkaç adımda güçlü self servis kimlik yönetimi özellikleri ekleyebilirsiniz. Bu makale size OAuth 2.0 taşıyıcı belirteçlerini kullanarak Node.js web API'si çağıran iOS "yapılacaklar listesi" uygulamasının nasıl oluşturulacağını gösterir. iOS uygulaması ve web API'si, kullanıcı kimliklerini yönetmek ve kullanıcıların kimliğini doğrulamak için Azure AD B2C kullanır.

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

> [AZURE.NOTE]
    Bu Hızlı Başlangıç'ın tam olarak çalışması için Azure AD B2C tarafından korunan web API'sine sahip olması gerekir. Hem .NET hem de Node.js için kullanabileceğiniz bir API oluşturduk. Bu kılavuz Node.js web API'si örneğinin yapılandırılmış olduğunu varsayar. Daha fazla bilgi için bkz. [Node.js için Azure Active Directory web API'si](active-directory-b2c-devquickstarts-api-node.md)
).

> [AZURE.NOTE]
    Bu makale, Azure AD B2C kullanarak oturum açma, kaydolma ve profil yönetimi uygulama konularını kapsamaz. Kullanıcının kimliği doğrulandıktan sonra web API'leri çağırmaya odaklanır. Henüz başlamadıysanız Azure AD B2C ile ilgili temel bilgileri öğrenmek için [.NET web uygulaması ile çalışmaya başlama öğreticisi](active-directory-b2c-devquickstarts-web-dotnet.md) ile başlamanız gerekir.

## Azure AD B2C dizini alma

Azure AD B2C'yi kullanabilmek için önce dizin veya kiracı oluşturmanız gerekir. Dizin; tüm kullanıcılarınız, uygulamalarınız, gruplarınız ve daha fazlası için bir kapsayıcıdır. Henüz yoksa devam etmeden önce [bir B2C dizini oluşturun](active-directory-b2c-get-started.md).

## Uygulama oluşturma

Ardından B2C dizininizde uygulama oluşturmanız gerekir. Bu, uygulamanız ile güvenli bir şekilde iletişim kurması için gereken bilgileri Azure AD'ye verir. Bu durumda, tek bir mantıksal uygulama içerdikleri için hem uygulama hem de web API'si tek bir **Uygulama Kimliği** ile temsil edilir. Bir uygulama oluşturmak için [bu talimatları](active-directory-b2c-app-registration.md) izleyin. Şunları yaptığınızdan emin olun:

- Uygulamaya bir **web uygulaması/web API'si** ekleyin.
- **Yanıt URL'si** olarak `http://localhost:3000/auth/openid/return` adresini girin. Bu URL, bu kod örneği için varsayılan URL'dir.
- Uygulamanız için bir **Uygulama gizli anahtarı** oluşturun ve bunu kopyalayın. Buna daha sonra ihtiyacınız olacak.
- Uygulamanıza atanan **Uygulama Kimliği**'ni kopyalayın. Buna da daha sonra ihtiyacınız olacak.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## İlkelerinizi oluşturma

Azure AD B2C'de her kullanıcı deneyimi, bir [ilke](active-directory-b2c-reference-policies.md) ile tanımlanır. Bu uygulama üç kimlik deneyimi içerir: Kaydolma, oturum açma ve Facebook ile oturum açma. Her tür için [ilke başvurusu makalesinde](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy) tanımlanan şekilde bir ilke oluşturmanız gerekir. Üç ilkeyi oluştururken şunları yaptığınızdan emin olun:

- Kaydolma ilkenizde **Görünen adı** ve kaydolma özniteliklerini seçin.
- Her ilkede uygulamanın talep ettiği **Görünen ad** ve **Nesne Kimliği** öğelerini seçin. Diğer talepleri de seçebilirsiniz.
- Oluşturduktan sonra her ilkenin **Adını** kaydedin. `b2c_1_` önekine sahip olmalıdır.  Bu ilke adlarına daha sonra ihtiyacınız olacak.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Üç ilkenizi oluşturduktan sonra uygulamanızı oluşturmaya hazırsınız.

Bu makalenin, oluşturduğunuz ilkeleri nasıl kullanacağınıza yönelik bilgileri kapsamadığını unutmayın. İlkelerin Azure AD B2C'de nasıl çalıştığını öğrenmeye [.NET web uygulaması ile çalışmaya başlama öğreticisi](active-directory-b2c-devquickstarts-web-dotnet.md) ile başlayın.

## Kodu indirme

Bu öğretici için kod [GitHub'da korunur](https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS). İşlemlere devam ederken örneği oluşturmak için [çatı projesini .zip dosyası olarak indirebilirsiniz](https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS/archive/skeleton.zip). Ayrıca çatıyı kopyalayabilirsiniz:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS.git
```

> [AZURE.NOTE] **Bu öğreticiyi tamamlamak için çatıyı indirmeniz gerekir.** iOS'a tam olarak çalışan bir uygulamayı uygulamanın karmaşıklığı nedeniyle **çatıda**, öğreticiyi tamamladıktan sonra çalıştıracağınız kullanıcı deneyimi kod bulunur. Bu, geliştirici için zaman kazandıran bir ölçüttür. Kullanıcı deneyimi kodu, bir iOS uygulamasına B2C ekleme konu başlığıyla ilgili değildir.

Tamamlanan uygulama aynı zamanda [.zip dosyası olarak](https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS/archive/complete.zip) veya `complete` aynı deponun dalı üzerinde de kullanılabilir.

Ardından CocoaPods kullanarak `podfile` yükleyin. Bu, yükleyeceğiniz yeni bir Xcode çalışma alanı oluşturur. CocoaPods'unuz yoksa [kurmak için web sitesini ziyaret edin](https://cocoapods.org).

```
$ pod install
...
$ open Microsoft Tasks for Consumers.xcworkspace
```

## iOS görev uygulamasını yapılandırma

Azure AD B2C ile iletişim kurma amacıyla iOS görev uygulamasını almak için bazı ortak parametreler sağlamanız gerekir. `Microsoft Tasks` klasörü içinde proje kökündeki `settings.plist` dosyasını açın ve `<dict>` bölümündeki değerleri değiştirin. Uygulama boyunca bu değerler kullanılacak.

```
<dict>
    <key>authority</key>
    <string>https://login.microsoftonline.com/<your tenant name>.onmicrosoft.com/</string>
    <key>clientId</key>
    <string><Enter the Application Id assigned to your app by the Azure portal, e.g.580e250c-8f26-49d0-bee8-1c078add1609></string>
    <key>scopes</key>
    <array>
        <string><Enter the Application Id assigned to your app by the Azure portal, e.g.580e250c-8f26-49d0-bee8-1c078add1609></string>
    </array>
    <key>additionalScopes</key>
    <array>
    </array>
    <key>redirectUri</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>taskWebAPI</key>
    <string>http://localhost/tasks:3000</string>
    <key>emailSignUpPolicyId</key>
    <string><Enter your sign up policy name, e.g.g b2c_1_sign_up></string>
    <key>faceBookSignInPolicyId</key>
    <string><your sign in policy for FB></string>
    <key>emailSignInPolicyId</key>
    <string><Enter your sign in policy name, e.g. b2c_1_sign_in></string>
    <key>fullScreen</key>
    <false/>
    <key>showClaims</key>
    <true/>
</dict>
</plist>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

## Erişim belirteci alma ve görev API'si çağırma

Bu bölümde, Microsoft'un kitaplıklarını ve çerçevelerini kullanarak bir web uygulamasında OAuth 2.0 belirteç değişimini nasıl tamamlayabileceğiniz anlatılmaktadır. Yetkilendirme kodları ve erişim belirteçleri ile ilgili bilginiz yoksa [OAuth 2.0 protokolü başvurusu](active-directory-b2c-reference-protocols.md) kapsamında daha fazla bilgi edinebilirsiniz.

### Yöntemleri kullanarak üst bilgi dosyaları oluşturma

Seçilen ilkeniz ile belirteç almanız ve ardından görev sunucunuzu çağırmanız için yöntemlere ihtiyaç duyacaksınız. Bu yöntemleri şimdi ayarlayın.

Xcode projenizde `/Microsoft Tasks` kısmında `samplesWebAPIConnector.h` adlı bir dosya oluşturun.

Yapmanız gerekeni tanımlamak için aşağıdaki kodu ekleyin:

```
#import <Foundation/Foundation.h>
#import "samplesTaskItem.h"
#import "samplesPolicyData.h"
#import "ADALiOS/ADAuthenticationContext.h"

@interface samplesWebAPIConnector : NSObject<NSURLConnectionDataDelegate>

+(void) getTaskList:(void (^) (NSArray*, NSError* error))completionBlock
             parent:(UIViewController*) parent;

+(void) addTask:(samplesTaskItem*)task
         parent:(UIViewController*) parent
completionBlock:(void (^) (bool, NSError* error)) completionBlock;

+(void) deleteTask:(samplesTaskItem*)task
            parent:(UIViewController*) parent
   completionBlock:(void (^) (bool, NSError* error)) completionBlock;

+(void) doPolicy:(samplesPolicyData*)policy
         parent:(UIViewController*) parent
completionBlock:(void (^) (ADProfileInfo* userInfo, NSError* error)) completionBlock;

+(void) signOut;

@end
```

Bunlar `doPolicy` yöntemidir ve API'nizdeki basit oluştur, oku,güncelleştir ve sil (CRUD) işlemleridir. Bu yöntemi kullanarak istediğiniz ilkeyi içeren bir belirteç elde edebilirsiniz.

İki üst bilgi dosyasının daha tanımlanması gerektiğini unutmayın. Bunlar, görev öğesini ve ilke verilerini tutacaktır. Bunları şimdi oluşturun:

Aşağıdaki kod ile `samplesTaskItem.h` dosyasını oluşturun:

```
#import <Foundation/Foundation.h>

@interface samplesTaskItem : NSObject

@property NSString *itemName;
@property NSString *ownerName;
@property BOOL completed;
@property (readonly) NSDate *creationDate;

@end
```

Ayrıca ilke verilerinizi tutmak için `samplesPolicyData.h` dosyasını oluşturun:

```
#import <Foundation/Foundation.h>

@interface samplesPolicyData : NSObject

@property (strong) NSString* policyName;
@property (strong) NSString* policyID;

+(id) getInstance;

@end
```
### Görev ve ilke öğeleriniz için uygulama ekleme

Artık üst bilgi dosyalarınız olduğuna göre, örneğiniz için kullanacağınız verileri depolamak üzere kod yazabilirsiniz.

Aşağıdaki kod ile `samplesPolicyData.m` dosyasını oluşturun:

```
#import <Foundation/Foundation.h>
#import "samplesPolicyData.h"

@implementation samplesPolicyData

+(id) getInstance
{
    static samplesPolicyData *instance = nil;
    static dispatch_once_t onceToken;

    dispatch_once(&onceToken, ^{
        instance = [[self alloc] init];
        NSDictionary *dictionary = [NSDictionary dictionaryWithContentsOfFile:[[NSBundle mainBundle] pathForResource:@"settings" ofType:@"plist"]];
        instance.policyName = [dictionary objectForKey:@"policyName"];
        instance.policyID = [dictionary objectForKey:@"policyID"];


    });

    return instance;
}


@end
```

### iOS için ADAL'a ilişkin çağrınıza yönelik kurulum kodu yazma

Kullanıcı arabirimine ilişkin nesnelerinizi saklamak için hızlı kod artık tamamlandı. Ardından `settings.plist` içine girdiğiniz parametreleri kullanarak iOS'a ilişkin Active Directory Kimlik Doğrulaması Kitaplığı'na (ADAL) erişmek için kod yazmanız gerekir. Bu işlem görev sunucunuza erişim sağlamak için bir erişim belirteci alır.

Tüm çalışmalarınız `samplesWebAPIConnector.m` içinde gerçekleştirilir.

Öncelikle `samplesWebAPIConnector.h` üst bilgi dosyanızda yazdığınız `doPolicy()` uygulamasını oluşturun:

```
+(void) doPolicy:(samplesPolicyData *)policy
         parent:(UIViewController*) parent
completionBlock:(void (^) (ADProfileInfo* userInfo, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    [self getClaimsWithPolicyClearingCache:NO policy:policy params:nil parent:parent completionHandler:^(ADProfileInfo* userInfo, NSError* error) {

        if (userInfo == nil)
        {
            completionBlock(nil, error);
        }

        else {

            completionBlock(userInfo, nil);
        }
    }];

}


```

Bu yöntem basittir. Oluşturduğunuz `samplesPolicyData` nesnesini, `ViewController` üst nesnesini ve bir geri aramayı giriş olarak alır. Geri arama özel bir konudur ve bunu ayrıntılı olarak işleyeceğiz.

- `completionBlock` yönteminin tür olarak `userInfo` nesnesini kullanarak döndürülen `ADProfileInfo` nesnesine sahip olduğuna dikkat edin. `ADProfileInfo` nesnesi, talepler dahil sunucu kaynaklı tüm yanıtları tutan türdür.
- `readApplicationSettings` yöntemine de dikkat edin. Bu, `settings.plist` içinde sağlanan verileri okur.
- Sonunda geniş bir `getClaimsWithPolicyClearingCache` yöntemine sahipsiniz. Bu, iOS için ADAL'a ilişkin yazmanız gereken gerçek çağrıdır. Buna daha sonra döneceğiz.

Ardından geniş `getClaimsWithPolicyClearingCache` yönteminizi yazın. Bu, kendi bölümüne yetecek kadar geniştir.

### iOS için ADAL çağrınızı oluşturma

Çatıyı GitHub'dan indirdiğinizde örnek uygulama ile ilgili yardım sunmak için bu çağrılardan bir çoğuna sahip olduğumuzu görebilirsiniz. Bunların tümü `get(Claims|Token)With<verb>ClearningCache` desenini izler. Objective C kurallarını kullanarak daha çok İngilizce gibi okurlar. Örneğin, "Sağladığım ek parametreleri içeren belirteci al ve önbelleği temizle", `getTokenWithExtraParamsClearingCache()` kalıbıdır.

"Sağladığım ilkeyi içeren talepleri al ve önbelleği temizleme" veya `getClaimsWithPolicyClearingCache` yazacaksınız. Belirteci her zaman ADAL'dan geri alırsınız, yani yöntemde "talepler ve belirteç" belirtmek gerekli değildir. Ancak bazen istekleri ayrıştırma ek yükü olmadan belirteci istersiniz, size çatıda istekleri içermeyen `getTokenWithPolicyClearingCache` adlı bir yöntem sunarız.

Şimdi bu kodu yazın:

```
+(void) getClaimsWithPolicyClearingCache  : (BOOL) clearCache
                           policy:(samplesPolicyData *)policy
                           params:(NSDictionary*) params
                           parent:(UIViewController*) parent
                completionHandler:(void (^) (ADProfileInfo*, NSError*))completionBlock;
{
    SamplesApplicationData* data = [SamplesApplicationData getInstance];


    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    if(!data.correlationId ||
       [[data.correlationId stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]] length] == 0)
    {
        authContext.correlationId = [[NSUUID alloc] initWithUUIDString:data.correlationId];
    }

    [ADAuthenticationSettings sharedInstance].enableFullScreen = data.fullScreen;
    [authContext acquireTokenWithScopes:data.scopes
                      additionalScopes: data.additionalScopes
                              clientId:data.clientId
                           redirectUri:redirectUri
                            identifier:[ADUserIdentifier identifierWithId:data.userItem.profileInfo.username type:RequiredDisplayableId]
                            promptBehavior:AD_PROMPT_ALWAYS
                  extraQueryParameters: params.urlEncodedString
                                policy: policy.policyID
                       completionBlock:^(ADAuthenticationResult *result) {

                           if (result.status != AD_SUCCEEDED)
                           {
                               completionBlock(nil, result.error);
                           }                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.profileInfo, nil);
                              }
                          }];
}


```

İlk kısmı tanıdık gelecektir.

- `settings.plist` içinde sağlanan ayarları yükleyin ve `data` öğesine atayın.
- iOS'tan gelen tüm hataları alan `ADAuthenticationError` öğesini ayarlayın.
- ADAL çağrınızı ayarlayan `authContext` öğesini oluşturun. İşlemleri başlatmak için yetkinizi ona geçirin.
- Geri dönebilmek için `authContext` nesnesine üst denetleyici için referans verin.
- `settings.plist` içinde dize olan `redirectURI` öğesini ADAL'ın beklediği NSURL'ye dönüştürün.
- `correlationId` öğesini ayarlayın. Bu, çağrıyı istemci boyunca sunucuya kadar ve geriye doğru izleyebilen bir UUID'dir. Hata ayıklama için faydalıdır.

Ardından ADAL gerçek çağrısını alırsınız. Burada, çağrı iOS için ADAL'ın önceki kullanımlarında görmeyi beklediğinizden farklı hale gelir:

```
[authContext acquireTokenWithScopes:data.scopes
                      additionalScopes: data.additionalScopes
                              clientId:data.clientId
                           redirectUri:redirectUri
                            identifier:[ADUserIdentifier identifierWithId:data.userItem.profileInfo.username type:RequiredDisplayableId]
                            promptBehavior:AD_PROMPT_ALWAYS
                  extraQueryParameters: params.urlEncodedString
                                policy: policy.policyID
                       completionBlock:^(ADAuthenticationResult *result) {

```

Çağrının oldukça basit olduğunu görebilirsiniz.

`scopes`: Oturum açan bir kullanıcı için sunucudan talep etmek istediğiniz sunucuya geçirdiğiniz kapsamlar. B2C önizlemesi için `client_id` öğesini geçirin. Yine de gelecekte kapsamları okumada değişiklik bekleniyor. Bu belgeyi daha sonra güncelleştirmeyi planlıyoruz.
`additionalScopes`: Bunlar, uygulamanız için kullanmak isteyebileceğiniz ek kapsamlardır. Gelecekte kullanılmaları bekleniyor.
`clientId`: Portaldan aldığınız Uygulama Kimliği.
`redirectURI`: Belirtecin geri gönderilmesini beklediğiniz konuma yeniden yönlendirme.
`identifier`: Önbellekte kullanılabilir belirteç olup olmadığını görebilmeniz için kullanıcı tanımlamaya yönelik yol. Bu, sunucudan her zaman başka belirteç istenmesini önler. Bu işlem `ADUserIdentifier` adlı tür tarafından gerçekleştirilir ve kimlik olarak kullanmak istediğiniz öğeyi belirtebilirsiniz. `username` kullanmanız gerekir.
`promptBehavior`: Bu kullanım dışıdır. `AD_PROMPT_ALWAYS` olmalıdır.
`extraQueryParameters`: Sunucuya geçirmek istediğiniz URL kodlanmış biçimindeki her ek öğe.
`policy`: Çağırdığınız ilke. Söz konusu ilke, bu kılavuzun en önemli kısmıdır.

`ADAuthenticationResult` öğesini geçirdiğiniz `completionBlock` bölümünde görebilirsiniz. (Çağrı başarılı ise) belirtece ve profil bilgilerinize sahip olur.

Yukarıdaki kodu kullanarak, sağladığınız ilke için bir belirteç elde edebilirsiniz. Böylece API'yi çağırmak için bu belirteci kullanabilirsiniz.

### Sunucuya yönelik görev çağrılarınızı oluşturma

Artık bir belirtece sahip olduğunuza göre bu belirteci yetkilendirme için API'nize sağlamanız gerekir.

Uygulanması gereken üç yöntem vardır:

```
+(void) getTaskList:(void (^) (NSArray*, NSError* error))completionBlock
             parent:(UIViewController*) parent;

+(void) addTask:(samplesTaskItem*)task
         parent:(UIViewController*) parent
completionBlock:(void (^) (bool, NSError* error)) completionBlock;

+(void) deleteTask:(samplesTaskItem*)task
            parent:(UIViewController*) parent
   completionBlock:(void (^) (bool, NSError* error)) completionBlock;
```

`getTasksList` sunucunuzdaki görevleri temsil eden bir dizi sağlar. `addTask` ve `deleteTask` sonraki eylemleri gerçekleştirir ve başarılı olurlarsa `true` veya `false` döndürülür.

Öncelikle `getTaskList` yazın:

```

+(void) getTaskList:(void (^) (NSArray*, NSError*))completionBlock
             parent:(UIViewController*) parent;
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    SamplesApplicationData* data = [SamplesApplicationData getInstance];

    [self craftRequest:[self.class trimString:data.taskWebApiUrlString]
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

                    NSArray *tasks = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                    //each object is a key value pair
                    NSDictionary *keyValuePairs;
                    NSMutableArray* sampleTaskItems = [[NSMutableArray alloc]init];

                    for(int i =0; i < tasks.count; i++)
                    {
                        keyValuePairs = [tasks objectAtIndex:i];

                        samplesTaskItem *s = [[samplesTaskItem alloc]init];
                        s.itemName = [keyValuePairs valueForKey:@"task"];

                        [sampleTaskItems addObject:s];
                    }

                    completionBlock(sampleTaskItems, nil);
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

Görev kodu tartışması bu gözden geçirme kapsamının ötesindedir. Ancak ilgi çekici bir şey fark etmiş olabilirsiniz: Görev URL'nizi alan bir `craftRequest` yöntemi. Bu yöntem, aldığınız erişim belirtecini kullanarak sunucu için istek oluşturmaya yönelik kullandığınız yöntemdir. Şimdi onu yazın.

`samplesWebAPIConnector.m` dosyasına aşağıdaki kodu ekleyin:

```
+(void) craftRequest : (NSString*)webApiUrlString
               parent:(UIViewController*) parent
    completionHandler:(void (^)(NSMutableURLRequest*, NSError* error))completionBlock
{
    [self getClaimsWithPolicyClearingCache:NO parent:parent completionHandler:^(NSString* accessToken, NSError* error){

        if (accessToken == nil)
        {
            completionBlock(nil,error);
        }
        else
        {
            NSURL *webApiURL = [[NSURL alloc]initWithString:webApiUrlString];

            NSMutableURLRequest *request = [[NSMutableURLRequest alloc]initWithURL:webApiURL];

            NSString *authHeader = [NSString stringWithFormat:@"Bearer %@", accessToken];

            [request addValue:authHeader forHTTPHeaderField:@"Authorization"];

            completionBlock(request, nil);
        }
    }];
}
```

Bu kod, web tekdüzen kaynak tanımlayıcısı (URI) alır; HTTP'deki `Bearer` üst bilgisini kullanarak belirteci ona ekler ve size geri döndürür. `getTokenClearingCache` API'sini çağırırsınız. Bu tuhaf görünebilir ancak bu çağrıyı sadece önbellekten belirteç almak ve hala geçerli olduğunu doğrulamak için kullanırsınız. (`getToken` çağrıları ADAL isteyerek bunu sizin için yapar.) Bu kodu her çağrıda kullanırsınız. Ardından ek görev yöntemlerinizi gerçekleştirin.

`addTask` yazın:

```
+(void) addTask:(samplesTaskItem*)task
         parent:(UIViewController*) parent
completionBlock:(void (^) (bool, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    SamplesApplicationData* data = [SamplesApplicationData getInstance];
    [self craftRequest:data.taskWebApiUrlString parent:parent completionHandler:^(NSMutableURLRequest* request, NSError* error){

        if (error != nil)
        {
            completionBlock(NO, error);
        }
        else
        {
            NSDictionary* taskInDictionaryFormat = [self convertTaskToDictionary:task];

            NSData* requestBody = [NSJSONSerialization dataWithJSONObject:taskInDictionaryFormat options:0 error:nil];

            [request setHTTPMethod:@"POST"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request setHTTPBody:requestBody];

            NSString *myString = [[NSString alloc] initWithData:requestBody encoding:NSUTF8StringEncoding];

            NSLog(@"Request was: %@", request);
            NSLog(@"Request body was: %@", myString);

            NSOperationQueue *queue = [[NSOperationQueue alloc]init];

            [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                NSString* content = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
                NSLog(@"%@", content);

                if (error == nil){

                    completionBlock(true, nil);
                }
                else
                {
                    completionBlock(false, error);
                }
            }];
        }
    }];
}
```

Bu işlem aynı deseni takip eder ancak aynı zamanda uygulamanız gereken son yöntemi de sunar: `convertTaskToDictionary`. Bu komut, dizinizi alır ve onu bir sözlük nesnesi haline getirir. Bu nesne sunucuya geçirmeniz gereken sorgu parametrelerine daha kolay şekilde dönüşür. Kod basittir:

```
// Here we have some conversation helpers that allow us to parse passed items into dictionaries for URLEncoding later.

+(NSDictionary*) convertTaskToDictionary:(samplesTaskItem*)task
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

    if (task.itemName){
        [dictionary setValue:task.itemName forKey:@"task"];
    }

    return dictionary;
}

```

Ardından `deleteTask` yazın:

```
+(void) deleteTask:(samplesTaskItem*)task
            parent:(UIViewController*) parent
   completionBlock:(void (^) (bool, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    SamplesApplicationData* data = [SamplesApplicationData getInstance];
    [self craftRequest:data.taskWebApiUrlString parent:parent completionHandler:^(NSMutableURLRequest* request, NSError* error){

        if (error != nil)
        {
            completionBlock(NO, error);
        }
        else
        {
            NSDictionary* taskInDictionaryFormat = [self convertTaskToDictionary:task];

            NSData* requestBody = [NSJSONSerialization dataWithJSONObject:taskInDictionaryFormat options:0 error:nil];

            [request setHTTPMethod:@"DELETE"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request setHTTPBody:requestBody];

            NSLog(@"%@", request);

            NSOperationQueue *queue = [[NSOperationQueue alloc]init];

            [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                NSString* content = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
                NSLog(@"%@", content);

                if (error == nil){

                    completionBlock(true, nil);
                }
                else
                {
                    completionBlock(false, error);
                }
            }];
        }
    }];
}
```

### Uygulamanıza oturum kapatma ekleme

Gerçekleştirilecek son adım uygulamanız için oturum kapatmayı uygulamaktır. Bu basit bir işlemdir. `sampleWebApiConnector.m` dosyasının içine:

```
+(void) signOut
{
    [authContext.tokenCacheStore removeAll:nil];

    NSHTTPCookie *cookie;

    NSHTTPCookieStorage *storage = [NSHTTPCookieStorage sharedHTTPCookieStorage];
    for (cookie in [storage cookies])
    {
        [storage deleteCookie:cookie];
    }
}
```

## Örnek uygulamayı çalıştırma

Son olarak Xcode'da uygulamayı oluşturun ve çalıştırın. Uygulamaya kaydolun veya uygulamada oturum açın ve oturum açan bir kullanıcı için görevler oluşturun. Oturumu kapatın, farklı bir kullanıcı olarak yeniden oturum açın ve o kullanıcı için görevler oluşturun.

API, aldığı erişim belirtecinden kullanıcının kimliğini ayıkladığı için görevlerin API üzerinde kullanıcı başına depolanmasına dikkat edin.

Başvuru için tam örnek [.zip dosyası olarak sağlanır](https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS/archive/complete.zip). Aynı zamanda GitHub'dan kopyalayabilirsiniz:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS```

## Sonraki adımlar

Artık daha ileri seviyeli B2C konu başlıklarına geçebilirsiniz: Deneyebilecekleriniz:

[Node.js web uygulamasından Node.js web API'si çağırma]()

[B2C uygulaması için kullanıcı deneyimini özelleştirme]()



<!---HONumber=Jun16_HO2-->


