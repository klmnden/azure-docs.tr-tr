---
title: İos'ta katılım API kullanma
description: En son iOS SDK - iOS katılım API kullanma
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 1fb4509e-3804-46c1-949f-1cf727f91f9f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 189a3029449a3161da2a20f940b77a5bb63bd1ef
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="how-to-use-the-engagement-api-on-ios"></a>İos'ta katılım API kullanma
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

Bu belge belgeye bir eklentidir tümleştirmek katılım iOS nasıl: katılım API uygulama istatistikleri rapor için nasıl kullanılacağı hakkında derinliği ayrıntıları sağlar.

Uygulamanızın oturumları, etkinlikleri, kilitlenme ve teknik bilgileri raporlamak için katılım yalnızca istiyorsanız, sonra en basit yolu, tüm özel yapmak için olduğunu aklınızda bulundurun `UIViewController` nesneleri devralır denk gelen `EngagementViewController` sınıfı.

Daha fazla bilgi için uygulama belirli olaylar, hatalar ve işleri, rapor gerekiyorsa örnek yapmak istiyorsanız veya uygulamanızın etkinlikleri uygulanan bir daha farklı bir şekilde bildirmek varsa `EngagementViewController` sınıfları yeniden katılım API'sini kullanmanız gerekiyor.

Katılım API'si tarafından sağlanan `EngagementAgent` sınıfı. Bu sınıfın örneğini çağırarak alınabilir `[EngagementAgent shared]` statik yöntemi (unutmayın `EngagementAgent` döndürülen tek nesnesidir).

Her API çağrıları önce `EngagementAgent` yöntemini çağırarak nesne başlatılmalı `[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

## <a name="engagement-concepts"></a>Engagement kavramları
Aşağıdaki bölümleri yaygın İyileştir [Mobile Engagement kavramları](mobile-engagement-concepts.md) iOS platformu için.

### <a name="session-and-activity"></a>`Session` ve `Activity`
Bir *etkinlik* yani genellikle uygulamanın bir ekran ile ilişkilendirilen *etkinlik* ekranı görüntülenir ve ekran kapatıldığında durdurduğunda başlatır: Engagement SDK'sını kullanarak tümleştirildiğinde bu durumda `EngagementViewController` sınıfları.

Ancak *etkinlikleri* de el ile katılım API'si kullanılarak denetlenebilir. Bu, birkaç alt bölümde bu ekrana (örneğin bilinen ne sıklıkta ve ne kadar süreyle iletişim kutuları içinde bu ekran kullanılır) kullanımı hakkında daha fazla bilgi almak için belirli bir ekran bölmek için sağlar.

## <a name="reporting-activities"></a>Raporlama etkinlikleri
### <a name="user-starts-a-new-activity"></a>Kullanıcı yeni bir etkinlik başlatır
            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

Çağırmanız gerekir `startActivity()` kullanıcı etkinliği değişiklikleri her zaman. Bu işlev ilk çağrıda yeni bir kullanıcı oturumu başlatır.

### <a name="user-ends-his-current-activity"></a>Kullanıcı kendi geçerli etkinliği sona erer
            [[EngagementAgent shared] endActivity];

> [!WARNING]
> Yapmanız gerekenler **hiçbir zaman** birkaç oturumları uygulamanıza bir kullanımını bölmek istiyorsanız, bu işlev tarafından dışında çağrısını kendiniz: Bu işlev çağrısı geçerli oturumu hemen, bu nedenle, bitecek sonraki çağrı `startActivity()` yeni bir oturum açmaları. Uygulamanızı kapalı olduğunda bu işlev otomatik olarak SDK tarafından çağrılır.
> 
> 

## <a name="reporting-events"></a>Raporlama olayları
### <a name="session-events"></a>Oturum olayları
Oturum olaylar, genellikle kendi oturumu sırasında bir kullanıcı tarafından gerçekleştirilen eylemleri bildirmek için kullanılır.

**Ek veriler olmadan örneği:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**Örnek ek veriler ile:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a>Tek başına olayları
Oturum olayları aykırı tek başına olay bir oturum bağlamı dışında kullanılabilir.

**Örnek:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

## <a name="reporting-errors"></a>Hata Raporlama
### <a name="session-errors"></a>Oturum hataları
Oturum hatalar genellikle kendi oturumu sırasında kullanıcı etkileyen hatalarını bildirmek için kullanılır.

**Örnek:**

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a>Tek başına hataları
Oturum hataları aykırı tek başına hata bir oturum bağlamı dışında kullanılabilir.

**Örnek:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

## <a name="reporting-jobs"></a>Raporlama işleri
**Örnek:**

Oturum açma işleminiz süresini rapor istediğinizi varsayalım:

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a>Bir işi sırasında hatalarını raporla
Geçerli kullanıcı oturumuyla ilgili yerine çalıştırılan bir iş hataları ile ilgili olabilir.

**Örnek:**

Oturum açma işleminiz sırasında bir hata raporu istediğinizi varsayalım:

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a>Bir işi sırasında olayları
Geçerli kullanıcı oturumuyla ilgili yerine çalıştırılan bir iş olayları ile ilgili olabilir.

**Örnek:**

Sosyal ağ sahibiz ve rapor için bir iş sunucusuna bağlı kullanıcı sırasında toplam süre kullanırız varsayalım. Kullanıcı kendi arkadaşlarınızdan ileti alabilir, bu proje bir olaydır.

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

## <a name="extra-parameters"></a>Ek parametreler
Rastgele veriler, olaylar, hatalar, etkinlikler ve işler eklenebilir.

Bu verileri yapılandırılmış, iOS'ın NSDictionary sınıfını kullanır.

Ek özellikler içerebileceğini unutmayın `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` veya diğer `NSDictionary` örnekleri.

> [!NOTE]
> Ek parametre JSON'de seri değildir. Yukarıda açıklananlardan farklı nesneleri geçirmek istiyorsanız, aşağıdaki yöntemi sınıfınızda uygulamanız gerekir:
> 
> -(NSString*) JSONRepresentation;
> 
> Yöntem bir JSON temsili nesnenizin döndürmelidir.
> 
> 

### <a name="example"></a>Örnek
    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Sınırlar
#### <a name="keys"></a>Anahtarlar
Her anahtarında `NSDictionary` şu normal ifadeyle eşleşen gerekir:

`^[a-zA-Z][a-zA-Z_0-9]*`

Anahtarları harfler, sayılar veya alt çizgi izlemelidir en az bir harf ile başlamalıdır anlamına gelir (\_).

#### <a name="size"></a>Boyut
Ek özellikler sınırlı **1024** (kez JSON'de katılım aracı tarafından kodlanmış) çağrı başına karakter.

Önceki örnekte, sunucuya gönderilen JSON 58 karakter olacak:

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a>Uygulama bilgilerini raporlama
İzleme bilgileri (veya diğer uygulama belirli bilgileri) kullanarak el ile raporlayabilirsiniz `sendAppInfo:` işlevi.

Bu bilgi artımlı olarak gönderilebilir Not: belirli bir aygıt için belirli bir anahtar için yalnızca en son değeri korunur.

Olay ek özellikler gibi `NSDictionary` sınıfı uygulama bilgilerini soyut, dizileri veya alt sözlükleri (JSON serileştirmesi kullanan) düz dize olarak değerlendirilir not için kullanılır.

**Örnek:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Sınırlar
#### <a name="keys"></a>Anahtarlar
Her anahtarında `NSDictionary` şu normal ifadeyle eşleşen gerekir:

`^[a-zA-Z][a-zA-Z_0-9]*`

Anahtarları harfler, sayılar veya alt çizgi izlemelidir en az bir harf ile başlamalıdır anlamına gelir (\_).

#### <a name="size"></a>Boyut
Uygulama bilgilerini sınırlı **1024** (kez JSON'de katılım aracı tarafından kodlanmış) çağrı başına karakter.

Önceki örnekte, sunucuya gönderilen JSON 44 karakter olacak:

    {"birthdate":"1983-12-07","gender":"female"}
