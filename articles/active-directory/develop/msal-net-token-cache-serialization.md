---
title: .NET için Microsoft kimlik doğrulama Kitaplığı'nda önbellek serileştirme belirteç | Azure
description: Serileştirme ve müşteri serileştirilmesi Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanarak belirteç önbelleği hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: celested
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/25/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9f1e9a48b114d328e0405a2f03764df4ce29b166
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65407050"
---
# <a name="token-cache-serialization-in-msalnet"></a>MSAL.NET belirteç önbelleği serileştirme
Sonra bir [belirteci alınan](msal-acquire-cache-tokens.md), Microsoft kimlik doğrulama kitaplığı (MSAL) tarafından önbelleğe alınır.  Uygulama kodu, başka bir yöntemle bir belirteç alınırken önce önbellekten bir belirteç almak üzere denemelisiniz.  Bu makalede, varsayılan ve özel serileştirilmesi MSAL.NET belirteç önbelleğe açıklanır.

Bu makalede, MSAL.NET için olan 3.x. MSAL.NET ilgileniyorsanız 2.x bkz [belirteç önbelleği serileştirme MSAL.NET 2.x](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Token-cache-serialization-2x).

## <a name="default-serialization-for-mobile-platforms"></a>Mobil platformlar için varsayılan seri hale getirme

MSAL.NET içinde bir bellek içi belirteç önbelleği varsayılan olarak sağlanır. Seri hale getirme, güvenli depolama platformunun bir parçası bir kullanıcı için kullanılabilir olduğu platformları için varsayılan olarak sağlanır. Evrensel Windows Platformu (UWP), Xamarin.iOS ve Xamarin.Android için durum budur.

> [!Note]
> Bir Xamarin.Android projesi MSAL.NET geçirirken MSAL.NET için 1.x 3.x eklemek isteyebilirsiniz `android:allowBackup="false"` projenize kaçınmak için eski geri Visual Studio dağıtımları bir geri yükleme, yerel depolama tetiklediğinizde öğesinden gelen belirteçleri önbelleğe alınmış. Bkz: [#659 sorun](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/659#issuecomment-436181938).

## <a name="custom-serialization-for-windows-desktop-apps-and-web-appsweb-apis"></a>Windows Masaüstü uygulamaları ve web apps/web API'si için özel seri hale getirme

Unutmayın, özel seri hale getirme (UWP, Xamarin.iOS ve Xamarin.Android) mobil platformlarda kullanılamaz. MSAL, zaten bu platformlar için güvenli ve yüksek performanslı bir seri hale getirme mekanizmasını tanımlar. Mimariler, .NET Masaüstü ve .NET Core uygulamaları, ancak değiştirilen ve bir genel amaçlı serileştirme mekanizması MSAL uygulayamaz. Örneğin, web siteleri, Redis önbelleği veya Masaüstü uygulamaları şifrelenmiş bir dosya deposu belirteçleri belirteçlerini depolamak üzere seçebilirsiniz. Serileştirme kullanıma hazır şekilde sağlanmadı. .NET Masaüstü veya .NET Core kalıcı belirteç önbelleği uygulamanız için seri hale getirme özelleştirmek gerekir.

Aşağıdaki sınıflar ve arabirimler belirteç önbelleği serileştirme kullanılır:

- `ITokenCache`, belirteç önbelleği serileştirme isteklerinin yanı sıra yöntemlerinin serileştirmek veya seri durumdan çıkarılamıyor cache çeşitli biçimlerde, abone olmak için olayları tanımlar (ADAL v3.0, MSAL 2.x ve MSAL 3.x ADAL v5.0 =).
- `TokenCacheCallback` Serileştirme işleyebilmeniz bir geri çağırma olaylarına geçirilir. Türünde bağımsız değişkenlerle çağrılır `TokenCacheNotificationArgs`.
- `TokenCacheNotificationArgs` yalnızca sağlar `ClientId` uygulama ve kullanıcı için belirteç kullanılabildiği bir başvuru.

  ![Sınıf diyagramı](media/msal-net-token-cache-serialization/class-diagram.png)

> [!IMPORTANT]
> MSAL.NET belirteci önbellekler oluşturur ve size sağlar `IToken` önbelleğe uygulamanın çağırdığınızda `GetUserTokenCache` ve `GetAppTokenCache` yöntemleri. Kendiniz arabirimini uygulayan geç değil. Bir özel belirteç önbelleği serileştirme uyguladığınızda sizin sorumluluğunuzdadır olmaktır:
> - Tepki `BeforeAccess` ve `AfterAccess` "olaylar". `BeforeAccess` İse temsilci önbellek seri durumdan çıkarılacak sorumlu `AfterAccess` önbellek serileştirmek için sorumlu biridir.
> - Bu olayların bölümü depolamak veya yüklemek istediğiniz her depolama alanına geçirilen olay bağımsız değişkeni BLOB sayısı.

Stratejiler için bir belirteç önbelleği serileştirme yazıyorsanız farklı bağlı olarak bir [genel istemci uygulaması](msal-client-applications.md) (Masaüstü) veya bir [gizli bir istemci uygulaması](msal-client-applications.md)) (web uygulaması / web API'si, arka plan programı uygulama ).

### <a name="token-cache-for-a-public-client"></a>Genel bir istemci için belirteç önbelleği 

Bu yana MSAL.NET'ın v2.x genel bir istemci belirteç önbelleği serileştirmek için birkaç seçeneğiniz vardır. Önbellek (Birleşik biçimi önbellek MSAL ve platformlar arasında yaygındır) MSAL.NET biçimine serileştirebiliyorsa.  Ayrıca destekleyebilir [eski](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization) belirteç önbelleği serileştirme ADAL v3.

Çoklu oturum açma durumu ADAL.NET arasında paylaşmak için belirteç önbelleği serileştirme özelleştirme 3.x ADAL.NET 5.x yanı sıra, MSAL.NET açıklanmıştır bölümü aşağıdaki örnek: [active-directory-dotnet-v1-to-v2](https://github.com/Azure-Samples/active-directory-dotnet-v1-to-v2).

> [!Note]
> MSAL MSAL.NET 1.1.4-preview belirteç önbelleği biçimi artık desteklenmeyen 2.x. MSAL.NET yararlanarak uygulamalarınız varsa 1.x, kullanıcılarınızın yeniden-oturumu açma olacaktır. Alternatif olarak, ADAL 4.x (ve 3.x) geçişi desteklenir.

#### <a name="simple-token-cache-serialization-msal-only"></a>Basit bir belirteç önbelleği seri hale getirme (yalnızca MSAL)

Bir Masaüstü uygulamaları için bir belirteç önbelleği özel serileştirme naïve uygulaması örneği aşağıdadır. Burada, kullanıcı belirteç önbelleği uygulama ile aynı klasörde bir dosyadır.

Uygulamayı sonra çağırarak serileştirme etkinleştirmeniz `TokenCacheHelper.EnableSerialization()` yöntemi ve uygulama geçirme `UserTokenCache`.

```csharp
app = PublicClientApplicationBuilder.Create(ClientId)
    .Build();
TokenCacheHelper.EnableSerialization(app.UserTokenCache);
```

`TokenCacheHelper` Yardımcı sınıfı olarak tanımlanır:

```csharp
static class TokenCacheHelper
 {
  public static void EnableSerialization(ITokenCache tokenCache)
  {
   tokenCache.SetBeforeAccess(BeforeAccessNotification);
   tokenCache.SetAfterAccess(AfterAccessNotification);
  }

  /// <summary>
  /// Path to the token cache
  /// </summary>
  public static readonly string CacheFilePath = System.Reflection.Assembly.GetExecutingAssembly().Location + ".msalcache.bin3";

  private static readonly object FileLock = new object();


  private static void BeforeAccessNotification(TokenCacheNotificationArgs args)
  {
   lock (FileLock)
   {
    args.TokenCache.DeserializeMsalV3(File.Exists(CacheFilePath)
            ? ProtectedData.Unprotect(File.ReadAllBytes(CacheFilePath),
                                      null,
                                      DataProtectionScope.CurrentUser)
            : null);
   }
  }

  private static void AfterAccessNotification(TokenCacheNotificationArgs args)
  {
   // if the access operation resulted in a cache update
   if (args.HasStateChanged)
   {
    lock (FileLock)
    {
     // reflect changesgs in the persistent store
     File.WriteAllBytes(CacheFilePath,
                         ProtectedData.Protect(args.TokenCache.SerializeMsalV3(),
                                                 null,
                                                 DataProtectionScope.CurrentUser)
                         );
    }
   }
  }
 }
```

Genel istemci uygulamaları (Windows, Mac ve Linux'ta çalışan masaüstü uygulamalar için) bir ürün kalitesini belirteç önbelleği dosya tabanlı serileştirici önizlemesi kullanılabilir [Microsoft.Identity.Client.Extensions.Msal](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Msal) Açık Kaynak Kitaplığı. Uygulamalarınıza aşağıdaki nuget paketinden içerebilir: [Microsoft.Identity.Client.Extensions.Msal](https://www.nuget.org/packages/Microsoft.Identity.Client.Extensions.Msal/).

#### <a name="dual-token-cache-serialization-msal-unified-cache-and-adal-v3"></a>Çift belirteç önbelleği serileştirme (MSAL birleşik önbellek ve ADAL v3)

Belirteç önbelleği her iki ile serileştirme birleşik önbellek biçimini uygulamak istiyorsanız (ADAL.NET ortak 4.x, MSAL.NET 2.x ve diğer MSALs aynı nesil ya da daha eski, aynı platform), aşağıdaki kodu göz atın:

```csharp
string appLocation = Path.GetDirectoryName(Assembly.GetEntryAssembly().Location;
string cacheFolder = Path.GetFullPath(appLocation) + @"..\..\..\..");
string adalV3cacheFileName = Path.Combine(cacheFolder, "cacheAdalV3.bin");
string unifiedCacheFileName = Path.Combine(cacheFolder, "unifiedCache.bin");

IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
                                    .Build();
FilesBasedTokenCacheHelper.EnableSerialization(app.UserTokenCache,
                                               unifiedCacheFileName,
                                               adalV3cacheFileName);

```

Bu yardımcı sınıf olarak tanımlanan süre:

```csharp
using System;
using System.IO;
using System.Security.Cryptography;
using Microsoft.Identity.Client;

namespace CommonCacheMsalV3
{
 /// <summary>
 /// Simple persistent cache implementation of the dual cache serialization (ADAL V3 legacy
 /// and unified cache format) for a desktop applications (from MSAL 2.x)
 /// </summary>
 static class FilesBasedTokenCacheHelper
 {
  /// <summary>
  /// Get the user token cache
  /// </summary>
  /// <param name="adalV3CacheFileName">File name where the cache is serialized with the
  /// ADAL V3 token cache format. Can
  /// be <c>null</c> if you don't want to implement the legacy ADAL V3 token cache
  /// serialization in your MSAL 2.x+ application</param>
  /// <param name="unifiedCacheFileName">File name where the cache is serialized
  /// with the Unified cache format, common to
  /// ADAL V4 and MSAL V2 and above, and also across ADAL/MSAL on the same platform.
  ///  Should not be <c>null</c></param>
  /// <returns></returns>
  public static void EnableSerialization(ITokenCache cache, string unifiedCacheFileName, string adalV3CacheFileName)
  {
   usertokenCache = cache;
   UnifiedCacheFileName = unifiedCacheFileName;
   AdalV3CacheFileName = adalV3CacheFileName;

   usertokenCache.SetBeforeAccess(BeforeAccessNotification);
   usertokenCache.SetAfterAccess(AfterAccessNotification);
  }

  /// <summary>
  /// Token cache
  /// </summary>
  static ITokenCache usertokenCache;

  /// <summary>
  /// File path where the token cache is serialized with the unified cache format
  /// (ADAL.NET V4, MSAL.NET V3)
  /// </summary>
  public static string UnifiedCacheFileName { get; private set; }

  /// <summary>
  /// File path where the token cache is serialized with the legacy ADAL V3 format
  /// </summary>
  public static string AdalV3CacheFileName { get; private set; }

  private static readonly object FileLock = new object();

  public static void BeforeAccessNotification(TokenCacheNotificationArgs args)
  {
   lock (FileLock)
   {
    args.TokenCache.DeserializeAdalV3(ReadFromFileIfExists(AdalV3CacheFileName));
    try
    {
     args.TokenCache.DeserializeMsalV3(ReadFromFileIfExists(UnifiedCacheFileName));
    }
    catch(Exception ex)
    {
     // Compatibility with the MSAL v2 cache if you used one
     args.TokenCache.DeserializeMsalV2(ReadFromFileIfExists(UnifiedCacheFileName));
    }
   }
  }

  public static void AfterAccessNotification(TokenCacheNotificationArgs args)
  {
   // if the access operation resulted in a cache update
   if (args.HasStateChanged)
   {
    lock (FileLock)
    {
     WriteToFileIfNotNull(UnifiedCacheFileName, args.TokenCache.SerializeMsalV3());
     if (!string.IsNullOrWhiteSpace(AdalV3CacheFileName))
     {
      WriteToFileIfNotNull(AdalV3CacheFileName, args.TokenCache.SerializeAdalV3());
     }
    }
   }
  }

  /// <summary>
  /// Read the content of a file if it exists
  /// </summary>
  /// <param name="path">File path</param>
  /// <returns>Content of the file (in bytes)</returns>
  private static byte[] ReadFromFileIfExists(string path)
  {
   byte[] protectedBytes = (!string.IsNullOrEmpty(path) && File.Exists(path))
       ? File.ReadAllBytes(path) : null;
   byte[] unprotectedBytes = encrypt ?
       ((protectedBytes != null) ? ProtectedData.Unprotect(protectedBytes, null, DataProtectionScope.CurrentUser) : null)
       : protectedBytes;
   return unprotectedBytes;
  }

  /// <summary>
  /// Writes a blob of bytes to a file. If the blob is <c>null</c>, deletes the file
  /// </summary>
  /// <param name="path">path to the file to write</param>
  /// <param name="blob">Blob of bytes to write</param>
  private static void WriteToFileIfNotNull(string path, byte[] blob)
  {
   if (blob != null)
   {
    byte[] protectedBytes = encrypt
      ? ProtectedData.Protect(blob, null, DataProtectionScope.CurrentUser)
      : blob;
    File.WriteAllBytes(path, protectedBytes);
   }
   else
   {
    File.Delete(path);
   }
  }

  // Change if you want to test with an un-encrypted blob (this is a json format)
  private static bool encrypt = true;
 }
}
```

### <a name="token-cache-for-a-web-app-confidential-client-application"></a>Bir web uygulaması (gizli bir istemci uygulaması) için belirteç önbelleği

Web uygulamaları veya web API'leri önbellek oturumu, Redis önbelleği veya veritabanı yararlanabilir.

Önemli bir şey unutmayın, web uygulamaları ve web API'leri için olması gereken bir belirteç önbelleği (hesap) başına kullanıcı başına ' dir. Her hesap için belirteç önbelleği serileştirmek gerekir.

Belirteç önbellekler için web apps ve web API'leri nasıl kullanılacağını örnekleri kullanılabilir [ASP.NET Core web uygulaması Öğreticisi](https://ms-identity-aspnetcore-webapp-tutorial) aşamada [2-2 belirteç önbelleği](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-2-TokenCache). Uygulamaları aşağıdaki klasöre bakmak için [TokenCacheProviders](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web/TokenCacheProviders) içinde [microsoft-kimlik doğrulama-extensions-için-dotnet](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet) kitaplığı (içinde [ Microsoft.Identity.Client.Extensions.Web](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web) klasör. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki örnekler, belirteç önbelleği serileştirme göstermektedir.

| Örnek | Platform | Açıklama|
| ------ | -------- | ----------- |
|[Active-Directory-dotnet-Desktop-msgraph-v2](https://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) | Masaüstü (WPF) | Windows Masaüstü .NET (WPF) uygulaması, Microsoft Graph API çağırma. ![Topoloji](media/msal-net-token-cache-serialization/topology.png)|
|[Active-Directory-dotnet-v1-to-v2](https://github.com/Azure-Samples/active-directory-dotnet-v1-to-v2) | Masaüstü (konsol) | Visual Studio çözümleri için Azure AD v2.0 uygulamaları olarak da adlandırılan, Azure AD v1.0 uygulamaları (ADAL.NET kullanarak) geçişini gösteren bir dizi yakınsanmış uygulamalar (MSAL.NET kullanarak), özellikle [belirteç önbelleği geçiş](https://github.com/Azure-Samples/active-directory-dotnet-v1-to-v2/blob/master/TokenCacheMigration/README.md)|