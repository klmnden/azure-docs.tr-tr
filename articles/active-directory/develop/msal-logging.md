---
title: MSAL uygulamalarda oturum | Azure
description: Microsoft Authentication Library (MSAL) uygulamalarında günlüğe kaydetme hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/22/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 58f18995d46ca61ae68a7b226bbfc9a286e73a0b
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544105"
---
# <a name="logging"></a>Günlüğe kaydetme
Yardımcı olabilecek günlük iletilerini oluşturmak için Microsoft kimlik doğrulama kitaplığı (MSAL) uygulama sorunlarını tanılamak ve ayrıntıları sağlayın. Bir uygulamayı birkaç kod satırıyla günlüğe kaydetmeyi yapılandırmak ve kişisel ve kurumsal verileri bırakmadığınıza kaydedilir ve ayrıntı düzeyini özel denetime sahip. Bir MSAL günlüğü geri aramasını ayarlamasını ve kullanıcıların kimlik doğrulama sorunları içerdiğinde günlükleri göndermek bir yol sunmak önerilir.

## <a name="logging-levels"></a>Günlüğe kaydetme düzeyleri

MSAL'ın Günlükçü için birkaç ayrıntı düzeyleri yakalama olmasını sağlar:

- Hata: Bir sorun oluştu ve bir hata oluştu gösterir. Hata ayıklama ve sorunları tanımlamak için kullanın.
- Uyarı: Soru ve uygulama olayları hakkında daha fazla bilgi gerekiyor. Var. olmak zorunda olmayan bir hata veya başarısız olan ancak için tanılama ve sunulan sorunlara yönelik.
- Bilgi: MSAL, hata ayıklama için mutlaka hedeflenen bilgilendirme amaçlı yönelik olayları günlüğe kaydeder.
- ayrıntılı: Varsayılan. MSAL, büyük miktarda bilgiyi oturum ve hangi kitaplığı davranışı tam ayrıntıları verin.

## <a name="personal-and-organizational-data"></a>Kişisel ve kurumsal veriler
Varsayılan olarak, son derece hassas kişisel veya kurumsal veri MSAL Günlükçü yakalamaz. Kitaplık Bunu yapmak karar verirseniz, kişisel ve kurumsal veri günlüğü etkinleştirme seçeneği sunar.

## <a name="logging-in-msalnet"></a>MSAL.NET günlüğüne
MSAL, 3.x günlük uygulama oluşturma kullanarak uygulama başına ayarlanır `.WithLogging` Oluşturucu değiştiricisi. Bu yöntem, isteğe bağlı parametreler isteyen:

- *Düzey* hangi günlüğe kaydetme düzeyini istediğiniz karar vermenize olanak sağlar. Bu ayar hataları için yalnızca hataları alırsınız
- *PiiLoggingEnabled* kişisel ve kurumsal veriler varsa oturum açmanızı sağlayan true olarak ayarlanmış. Böylece, uygulamanızın kişisel verileri günlüğe kaydetmez varsayılan olarak bu false olarak ayarlanır.
- *LogCallback* günlüğe yapan bir temsilciye ayarlanır. Varsa *PiiLoggingEnabled* true, bu yöntemin alacağını ileti iki kez: kez *containsPii* parametresi yanlış ve bu iletiyi kişisel veriler ve ileikincidefaolmadaneşittir*containsPii* parametresi true olarak eşittir ve ileti kişisel verileri içerebilir. (İleti kişisel verileri içermediğinde) bazı durumlarda aynı ileti olacaktır.
- *DefaultLoggingEnabled* platformu için varsayılan günlük kaydını etkinleştirir. Varsayılan olarak false şeklindedir. Bunu true olarak ayarlarsanız Masaüstü/UWP uygulamalarında NSLog iOS ve android'de logcat olay izlemeyi kullanır.

```csharp
class Program
 {
  private static void Log(LogLevel level, string message, bool containsPii)
  {
     if (containsPii)
     {
        Console.ForegroundColor = ConsoleColor.Red;
     }
     Console.WriteLine($"{level} {message}");
     Console.ResetColor();
  }

  static void Main(string[] args)
  {
    var scopes = new string[] { "User.Read" };

    var application = PublicClientApplicationBuilder.Create("<clientID>")
                      .WithLogging(Log, LogLevel.Info, true)
                      .Build();

    AuthenticationResult result = application.AcquireTokenInteractive(scopes)
                                             .ExecuteAsnc();
  }
 }
 ```

 ## <a name="logging-in-msaljs"></a>MSAL.js günlüğüne

 Yapılandırma, oluşturma sırasında bir Günlükçü nesnesi geçirerek MSAL.js günlüğü etkinleştirebilirsiniz bir `UserAgentApplication` örneği. Bu Günlükçü nesne, aşağıdaki özelliklere sahiptir:

- *localCallback*: kullanma ve günlükleri özel bir şekilde yayımlamak için geliştirici tarafından sağlanan bir geri çağırma örneği. Nasıl günlükleri yeniden yönlendirmek istediğinize bağlı olarak localCallback metodu uygulayın.

- *düzey* (isteğe bağlı): yapılandırılabilir günlük düzeyi. Desteklenen günlük düzeyleri şunlardır: Hata, uyarı, ayrıntılı bilgi. Bilgi varsayılan değerdir.

- *piiLoggingEnabled* (isteğe bağlı): kişisel ve kurumsal veriler varsa oturum açmanızı sağlayan true olarak ayarlanmış. Böylece, uygulamanızın kişisel verileri günlüğe kaydetmez varsayılan olarak bu false olarak ayarlanır. Kişisel veri günlüklerini varsayılan çıktılar konsol, Logcat veya NSLog gibi hiçbir zaman yazılır. Varsayılan false olarak ayarlanır.

- *Correlationıd* (isteğe bağlı): hata ayıklama amacıyla bir yanıt istekle eşlemek için kullanılan benzersiz bir tanımlayıcı. Varsayılan olarak RFC4122 sürüm 4 GUID (128 bit).

```javascript

function loggerCallback(logLevel, message, containsPii) {
   console.log(message);
}

var msalConfig = {
    auth: {
        clientId: “abcd-ef12-gh34-ikkl-ashdjhlhsdg”,
    },
    system: {
        logger: {
            localCallback: loggerCallback,
            level: Msal.LogLevel.Verbose,
            piiLoggingEnabled: false,
            correlationId: '1234'
        }
    }
}

var UserAgentApplication = new Msal.UserAgentApplication(msalConfig);
```
