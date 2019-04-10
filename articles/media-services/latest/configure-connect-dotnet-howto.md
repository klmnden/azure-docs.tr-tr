---
title: Azure Media Services v3 API'sine - .NET bağlanma
description: .NET ile Media Services v3 API bağlanma hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2019
ms.author: juliako
ms.openlocfilehash: 8f8a1434af768180e34afcaacd6e92ab402ad8cd
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59361247"
---
# <a name="connect-to-media-services-v3-api---net"></a>Media Services v3 API'sine - .NET bağlanma

Bu makalede hizmet sorumlusu oturum açma yöntemini kullanarak Azure Media Services v3 .NET SDK'sına nasıl gösterir.

## <a name="prerequisites"></a>Önkoşullar

- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). Kaynak grubu adı ve Media Services hesap adını hatırlamak emin olun
- .NET geliştirme için kullanmak istediğiniz bir aracı yükleyin. Bu makaledeki adımlarda kullanmayı gösteren [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Visual Studio Code'u, bkz: [çalışma C# ](https://code.visualstudio.com/docs/languages/csharp). Ya da farklı bir kod Düzenleyicisi'ni kullanabilirsiniz.

## <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

1. Visual Studio’yu çalıştırın. 
1. Gelen **dosya** menüsünde tıklatın **yeni** > **proje**. 
1. Oluşturma bir **.NET Core** konsol uygulaması.

Bu konuda, örnek uygulamayı hedeflediğinden `netcoreapp2.0`. 'Sürümünden itibaren kullanılabilir olan kod kullandığı async main', C# 7.1. Bkz. Bu [blog](https://blogs.msdn.microsoft.com/benwilli/2017/12/08/async-main-is-available-but-hidden/) daha fazla ayrıntı için.

## <a name="add-required-nuget-packages"></a>Gerekli NuGet paketleri Ekle

1. Visual Studio'da **Araçları** > **NuGet Paket Yöneticisi** > **NuGet Yöneticisi konsolunu**.
2. İçinde **Paket Yöneticisi Konsolu** penceresinde kullanım `Install-Package` aşağıdaki NuGet paketlerini eklemek için komutu. Örneğin, `Install-Package Microsoft.Azure.Management.Media`.

|Paket|Açıklama|
|---|---|
|`Microsoft.Azure.Management.Media`|Azure Media Services SDK. <br/>En son Azure Media Services paketini kullandığınızdan emin olmak için kontrol [Microsoft.Azure.Management.Media](https://www.nuget.org/packages/Microsoft.Azure.Management.Media).|
|`Microsoft.Rest.ClientRuntime.Azure.Authentication`|AĞ için Azure SDK'sı için ADAL kimlik doğrulaması kitaplığı|
|`Microsoft.Extensions.Configuration.EnvironmentVariables`|Yapılandırma değerleri, ortam değişkenleri ve yerel JSON dosyalarından okuma|
|`Microsoft.Extensions.Configuration.Json`|Yapılandırma değerleri, ortam değişkenleri ve yerel JSON dosyalarından okuma
|`WindowsAzure.Storage`|Depolama SDK'sı|

## <a name="create-and-configure-the-app-settings-file"></a>Uygulama ayarları dosyası oluşturma ve yapılandırma

### <a name="create-appsettingsjson"></a>AppSettings.JSON oluşturma

1. Go Git **genel** > **metin dosyası**.
1. "Appsettings.json" olarak adlandırın.
1. .json dosyasını "Yeniyse Kopyala" "Çıktı dizinine Kopyala" özelliğini ayarlayın (uygulama yayımlandığında erişebilmesi için böylece).

### <a name="set-values-in-appsettingsjson"></a>Appsettings.json içinde değerlerini ayarlama

Çalıştırma `az ams account sp create` komutu açıklandığı [API'lere](access-api-cli-how-to.md). Komut, "appsettings.json" kopyalamalısınız json döndürür.
 
## <a name="add-configuration-file"></a>Yapılandırma dosyasını ekleme

Kolaylık olması için "appsettings.json" değerlerinden okumaktan sorumlu olan bir yapılandırma dosyası ekleyin.

1. Yeni bir .cs sınıf projenize ekleyin. Bunu, `ConfigWrapper` olarak adlandırın. 
1. Bu dosyaya aşağıdaki kodu yapıştırın (Bu örnekte ad alanına sahip olduğunuz varsayılır olan `ConsoleApp1`).

```csharp
using System;

using Microsoft.Extensions.Configuration;

namespace ConsoleApp1
{
    public class ConfigWrapper
    {
        private readonly IConfiguration _config;

        public ConfigWrapper(IConfiguration config)
        {
            _config = config;
        }

        public string SubscriptionId
        {
            get { return _config["SubscriptionId"]; }
        }

        public string ResourceGroup
        {
            get { return _config["ResourceGroup"]; }
        }

        public string AccountName
        {
            get { return _config["AccountName"]; }
        }

        public string AadTenantId
        {
            get { return _config["AadTenantId"]; }
        }

        public string AadClientId
        {
            get { return _config["AadClientId"]; }
        }

        public string AadSecret
        {
            get { return _config["AadSecret"]; }
        }

        public Uri ArmAadAudience
        {
            get { return new Uri(_config["ArmAadAudience"]); }
        }

        public Uri AadEndpoint
        {
            get { return new Uri(_config["AadEndpoint"]); }
        }

        public Uri ArmEndpoint
        {
            get { return new Uri(_config["ArmEndpoint"]); }
        }

        public string Region
        {
            get { return _config["Region"]; }
        }
    }
}
```

## <a name="connect-to-the-net-client"></a>.NET istemci için bağlanma

.NET ile Media Services API’lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturmanız gerekir. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Aşağıdaki kod, yerel yapılandırma dosyasında sağlanan kimlik bilgileri temel ServiceClientCredentials nesnesi GetCredentialsAsync işlevi oluşturur.

1. Açık `Program.cs`.
1. Aşağıdaki kodu yapıştırın:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;

using Microsoft.Azure.Management.Media;
using Microsoft.Azure.Management.Media.Models;
using Microsoft.Extensions.Configuration;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;

namespace ConsoleApp1
{
    class Program
    {
        public static async Task Main(string[] args)
        {
            
            ConfigWrapper config = new ConfigWrapper(new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
                .AddEnvironmentVariables()
                .Build());

            try
            {
                IAzureMediaServicesClient client = await CreateMediaServicesClientAsync(config);
                Console.WriteLine("connected");
            }
            catch (Exception exception)
            {
                if (exception.Source.Contains("ActiveDirectory"))
                {
                    Console.Error.WriteLine("TIP: Make sure that you have filled out the appsettings.json file before running this sample.");
                }

                Console.Error.WriteLine($"{exception.Message}");

                ApiErrorException apiException = exception.GetBaseException() as ApiErrorException;
                if (apiException != null)
                {
                    Console.Error.WriteLine(
                        $"ERROR: API call failed with error code '{apiException.Body.Error.Code}' and message '{apiException.Body.Error.Message}'.");
                }
            }

            Console.WriteLine("Press Enter to continue.");
            Console.ReadLine();
        }
 
        private static async Task<ServiceClientCredentials> GetCredentialsAsync(ConfigWrapper config)
        {
            // Use ApplicationTokenProvider.LoginSilentWithCertificateAsync or UserTokenProvider.LoginSilentAsync to get a token using service principal with certificate
            //// ClientAssertionCertificate
            //// ApplicationTokenProvider.LoginSilentWithCertificateAsync

            // Use ApplicationTokenProvider.LoginSilentAsync to get a token using a service principal with symetric key
            ClientCredential clientCredential = new ClientCredential(config.AadClientId, config.AadSecret);
            return await ApplicationTokenProvider.LoginSilentAsync(config.AadTenantId, clientCredential, ActiveDirectoryServiceSettings.Azure);
        }

        private static async Task<IAzureMediaServicesClient> CreateMediaServicesClientAsync(ConfigWrapper config)
        {
            var credentials = await GetCredentialsAsync(config);

            return new AzureMediaServicesClient(config.ArmEndpoint, credentials)
            {
                SubscriptionId = config.SubscriptionId,
            };
        }

    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Karşıya yükleme, kodlama ve akışını videoları - .NET](stream-files-tutorial-with-api.md) 
- [Öğretici: Stream Canlı Media Services v3 - .NET](stream-live-tutorial-with-api.md)
- [Öğretici: Media Services v3 - .NET ile videoları analiz etme](analyze-videos-tutorial-with-api.md)
- [Yerel bir dosyadan - .NET iş girdisi oluşturma](job-input-from-local-file-how-to.md)
- [Bir HTTPS URL'si - .NET iş girdisi oluşturma](job-input-from-http-how-to.md)
- [-.NET gibi özel bir dönüşüm ile kodlayın](customize-encoder-presets-how-to.md)
- [AES-128 dinamik şifreleme ve anahtar teslim hizmeti - .NET](protect-with-aes128.md)
- [DRM dinamik şifreleme ve lisans teslimat hizmeti - .NET](protect-with-drm.md)
- [Mevcut ilkeden - .NET bir imzalama anahtarı alma](get-content-key-policy-dotnet-howto.md)
- [Medya Hizmetleri - .NET ile filtre oluşturma](filters-dynamic-manifest-dotnet-howto.md)
- [Azure işlevler v2 Media Services v3 ile talep üzerine örnekleri video Gelişmiş](https://aka.ms/ams3functions)

## <a name="see-also"></a>Ayrıca bkz.

[.NET başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/mediaservices/management?view=azure-dotnet)
