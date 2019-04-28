---
title: .NET ile Azure Media Services API'sine erişmek için Azure AD kullanım kimlik doğrulaması | Microsoft Docs
description: Bu konuda, Azure Active Directory (Azure AD) kimlik doğrulaması .NET ile Azure Media Services (AMS) API'sine erişmek için nasıl kullanılacağı gösterilmektedir.
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
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 15b986d4e7567be48c582e4a39b727ab110de2be
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61230956"
---
# <a name="use-azure-ad-authentication-to-access-azure-media-services-api-with-net"></a>.NET ile Azure Media Services API'sine erişmek için Azure AD kimlik doğrulaması kullanın.

4.0.0.4 windowsazure.mediaservices ile başlayarak, Azure Media Services, Azure Active Directory (Azure AD) göre kimlik doğrulamasını destekler. Bu konuda Microsoft .NET ile Azure Media Services API'sine erişmek için Azure AD kimlik doğrulaması kullanmayı gösterir.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure hesabı. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 
- Bir Media Services hesabı. Daha fazla bilgi için [Azure portalını kullanarak bir Azure Media Services hesabı oluşturma](media-services-portal-create-account.md).
- En son [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) paket.
- Konu konusunda [Azure AD kimlik doğrulamasına genel bakış ile Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md). 

Azure Media Services ile Azure AD kimlik doğrulamasını kullanırken, iki yoldan biriyle kimlik doğrulaması yapabilir:

- **Kullanıcı kimlik doğrulaması** kullanan uygulama etkileşim kurmak için Azure Media Services kaynaklarla bir kişinin kimliğini doğrular. Etkileşimli uygulama, kullanıcıdan kimlik bilgilerini ilk isteyecektir. Kodlama işi izlemek veya canlı akış için yetkili kullanıcılar tarafından kullanılan bir yönetim konsol uygulaması örneğidir. 
- **Hizmet sorumlusu kimlik doğrulaması** bir hizmetin kimliğini doğrular. Genellikle bu kimlik doğrulama yöntemi kullanan uygulamalar, daemon Hizmetleri, orta katman Hizmetleri ve web uygulamaları, işlev uygulamaları, logic apps, API veya mikro hizmetler gibi zamanlanmış işler çalışan uygulamalardır.

>[!IMPORTANT]
>Azure medya Hizmetleri şu anda bir Azure Access Control Service kimlik doğrulama modelini destekler. Ancak, erişim denetimi yetkilendirme 22 Haziran 2018'de kullanım dışı olacaktır. Bir Azure Active Directory kimlik doğrulama modeline olabildiğince çabuk geçirme öneririz.

## <a name="get-an-azure-ad-access-token"></a>Azure AD erişim belirteci alma

Azure AD kimlik doğrulamasıyla Azure Media Services API'sine bağlanmak için bir Azure AD erişim belirteci istemek istemci uygulaması gerekir. Media Services .NET İstemci SDK'sı kullandığınızda, Azure AD erişim belirteci alma konusunda ayrıntıların birçoğu sarmalanmış ve size için Basitleştirilmiş [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) ve [AzureAdTokenCredentials](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs)sınıfları. 

Örneğin, Azure AD yetkilisini, Media Services kaynağı URI ya da yerel sağlamanıza gerek kalmaması Azure AD uygulama ayrıntıları. Bunlar Azure AD erişim belirteci sağlayıcısı sınıfı tarafından zaten yapılandırılmış iyi bilinen değerlerdir. 

Azure medya hizmeti .NET SDK'sı kullanmıyorsanız kullanmanızı öneririz [Azure AD kimlik doğrulama Kitaplığı](../../active-directory/develop/active-directory-authentication-libraries.md). Azure AD kimlik doğrulama kitaplığı ile kullanmak için gereken parametreler için değerleri almak için bkz: [Azure AD kimlik doğrulama ayarlarına erişmek için Azure portal'ı kullanmanızı](media-services-portal-get-started-with-aad.md).

Varsayılan uygulamasını değiştirme seçeneği de **AzureAdTokenProvider** kendi uygulaması.

## <a name="install-and-configure-azure-media-services-net-sdk"></a>Yükleme ve Azure Media Services .NET SDK'sını yapılandırma

>[!NOTE] 
>Media Services .NET SDK ile Azure AD kimlik doğrulamasını kullanmak için en son gerekir [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) paket. Ayrıca, bir başvuru ekleyin **Microsoft.IdentityModel.Clients.activedirectory** derleme. Mevcut bir uygulamayı kullanıyorsanız dahil **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** derleme. 

1. Visual Studio'da yeni bir C# konsol uygulaması oluşturun.
2. Kullanım [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet paketini yüklemek için **Azure Media Services .NET SDK'sı**. 

    NuGet kullanarak başvuru eklemek için aşağıdaki adımları uygulayın: içinde **Çözüm Gezgini**proje adına sağ tıklayın ve ardından **NuGet paketlerini Yönet**. Ardından, arama **windowsazure.mediaservices** seçip **yükleme**.
    
    -veya-

    Aşağıdaki komutu çalıştırın **Paket Yöneticisi Konsolu** Visual Studio'da.

        Install-Package windowsazure.mediaservices -Version 4.0.0.4

3. Ekleme **kullanarak** kaynak kodunuz için.

        using Microsoft.WindowsAzure.MediaServices.Client; 

## <a name="use-user-authentication"></a>Kullanıcı kimlik doğrulaması kullan

Kullanıcı kimlik doğrulaması seçeneği ile Azure Media Service API'sine bağlanmak için aşağıdaki parametreleri kullanarak bir Azure AD belirteç istemek istemci uygulaması gerekir:  

- Azure AD Kiracı uç noktası. Kiracı bilgileri, Azure portalından alınabilir. Sağ üst köşedeki oturum açmış kullanıcı üzerine gelin.
- Media Services kaynak URI'si.
- Media Services (yerel) uygulama istemci kimliği 
- Media Services (yerel) uygulama yeniden yönlendirme URI'si. 

Bu parametrelerin değerleri bulunabilir **AzureEnvironments.AzureCloudEnvironment**. **AzureEnvironments.AzureCloudEnvironment** doğru ortamda genel bir Azure veri merkezi için değişken ayarları almak için .NET SDK'sındaki bir yardımcı sabittir. 

Media Services yalnızca ortak veri merkezlerinde erişmek için önceden tanımlı bir ortam ayarlarını içerir. Bağımsız veya kamu bulut bölgeleri için kullanabileceğiniz **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvironment**, veya **AzureGermanCloudEnvironment** sırasıyla.

Aşağıdaki kod örneği, bir belirteç oluşturur:
    
    var tokenCredentials = new AzureAdTokenCredentials("microsoft.onmicrosoft.com", AzureEnvironments.AzureCloudEnvironment);
    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
  
Media Services karşı programlama başlatmak için oluşturmak gereken bir **CloudMediaContext** sunucu bağlamı temsil eden örneği. **CloudMediaContext** işleri, varlıklar, dosyaları, erişim ilkeleri ve bulucular dahil olmak üzere önemli koleksiyonlar için başvurular içerir. 

Geçirmeniz gerekir **kaynak medya REST Hizmetleri için URI** için **CloudMediaContext** Oluşturucusu. Medya REST Hizmetleri, Azure portalında oturum açma, Azure Media Services hesabınızı seçin için Kaynak URI ulaşmak için **API erişimi**ve ardından **Azure Media Services ile kullanıcı kimlik doğrulamasıBağlan**. 

Aşağıdaki kod örneği oluşturur bir **CloudMediaContext** örneği:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);

Aşağıdaki örnek, Azure AD belirtecini ve bağlam oluşturma işlemi gösterilmektedir:

    namespace AzureADAuthSample
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Specify your Azure AD tenant domain, for example "microsoft.onmicrosoft.com".
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", AzureEnvironments.AzureCloudEnvironment);
    
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
                // Specify your REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".
                CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
                var assets = context.Assets;
                foreach (var a in assets)
                {
                    Console.WriteLine(a.Name);
                }
            }
    
        }
    }

>[!NOTE]
>Bildiren bir özel durum alırsanız "uzak sunucu bir hata döndürdü: Yetkisiz (401)"konusuna bakın [erişim denetimi](media-services-use-aad-auth-to-access-ams-api.md#access-control) Azure Media Services API'sine erişim bölümünü ile Azure AD kimlik doğrulamasına genel bakış.

## <a name="use-service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması kullanma
    
(Web API veya web uygulaması) gereksinimlerini hizmet sorumlusu seçeneğiyle orta katman uygulamanızı Azure Media Services API'sine bağlanma aşağıdaki parametrelerle bir Azure AD belirtecini ister:  

- Azure AD Kiracı uç noktası. Kiracı bilgileri, Azure portalından alınabilir. Sağ üst köşedeki oturum açmış kullanıcı üzerine gelin.
- Media Services kaynak URI'si.
- Azure AD uygulama değerlerini: **istemci kimliği** ve **gizli**.

Değerleri **istemci kimliği** ve **gizli** parametreleri, Azure portalında bulunabilir. Daha fazla bilgi için [Azure portalını kullanarak Azure AD kimlik doğrulamasını kullanmaya başlama](media-services-portal-get-started-with-aad.md).

Aşağıdaki kod örneği kullanarak bir belirteç oluşturur **AzureAdTokenCredentials** alan oluşturucu **AzureAdClientSymmetricKey** bir parametre olarak: 
    
    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                AzureEnvironments.AzureCloudEnvironment);

    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

Ayrıca belirtebileceğiniz **AzureAdTokenCredentials** alan oluşturucu **AzureAdClientCertificate** bir parametre olarak. 

Oluşturma ve Azure AD tarafından kullanılabilecek bir formda bir sertifika yapılandırmanız hakkında yönergeler için bkz: [sertifikalar - elle yapılandırma adımları ile arka plan programı uygulamalarında Azure AD'ye kimlik doğrulaması](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).

    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientCertificate("{YOUR CLIENT ID HERE}", "{YOUR CLIENT CERTIFICATE THUMBPRINT}"), 
                                AzureEnvironments.AzureCloudEnvironment);

Media Services karşı programlama başlatmak için oluşturmak gereken bir **CloudMediaContext** sunucu bağlamı temsil eden örneği. Geçirmeniz gerekir **kaynak medya REST Hizmetleri için URI** için **CloudMediaContext** Oluşturucusu. Alabileceğiniz **kaynak medya REST Hizmetleri için URI** de Azure portalından değeri.

Aşağıdaki kod örneği oluşturur bir **CloudMediaContext** örneği:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
Aşağıdaki örnek, Azure AD belirtecini ve bağlam oluşturma işlemi gösterilmektedir:

    namespace AzureADAuthSample
    {
    
        class Program
        {
            static void Main(string[] args)
            {
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                            new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                            AzureEnvironments.AzureCloudEnvironment);
            
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
                // Specify your REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".      
                CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
                var assets = context.Assets;
                foreach (var a in assets)
                {
                    Console.WriteLine(a.Name);
                }
    
                Console.ReadLine();
            }
    
        }
    }

## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya başlama [dosyaları hesabınıza Yükleniyor](media-services-dotnet-upload-files.md).
