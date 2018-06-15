---
title: .NET ile Azure Media Services API erişmek için kimlik doğrulamasını kullan Azure AD | Microsoft Docs
description: Bu konu, Azure Active Directory (Azure AD) ile kimlik doğrulaması .NET ile Azure Media Services (AMS) API erişmek için nasıl kullanılacağını gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: juliako
ms.openlocfilehash: b8f58f4010590dc40d5e8dc7ac1b634f161a807d
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33789470"
---
# <a name="use-azure-ad-authentication-to-access-azure-media-services-api-with-net"></a>.NET ile Azure Media Services API erişmek için Azure AD kimlik doğrulaması kullanın

4.0.0.4 windowsazure.mediaservices ile başlayarak, Azure Media Services, Azure Active Directory (Azure AD) tabanlı kimlik doğrulamasını destekler. Bu konu Azure Media Services API Microsoft .NET ile erişmek için Azure AD kimlik doğrulaması kullanmayı gösterir.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure hesabı. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 
- Bir Media Services hesabı. Daha fazla bilgi için bkz: [Azure portalını kullanarak Azure Media Services hesabı oluşturma](media-services-portal-create-account.md).
- En son [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) paket.
- Konu aşina [AAD kimlik doğrulamasına genel bakış ile Azure Media Services API erişme](media-services-use-aad-auth-to-access-ams-api.md). 

Azure Media Services ile Azure AD kimlik doğrulaması kullanırken, aşağıdaki iki yöntemden biriyle doğrulayabilir:

- **Kullanıcı kimlik doğrulaması** uygulama etkileşim kurmak için Azure Media Services kaynaklarla kullanan bir kişinin kimliğini doğrular. Etkileşimli uygulama önce kullanıcıdan kimlik bilgilerini ister. Kodlama işleri izlemek veya canlı akış için yetkili kullanıcılar tarafından kullanılan bir yönetim konsol uygulaması örneğidir. 
- **Hizmet asıl kimlik doğrulaması** bir hizmetin kimliğini doğrular. Genellikle bu kimlik doğrulama yöntemini kullanan uygulamalar, arka plan programı services, orta katman Hizmetleri veya web uygulamaları, işlev uygulamalarının, logic apps, API veya mikro gibi zamanlanmış işleri çalıştırma uygulamalardır.

>[!IMPORTANT]
>Azure medya hizmeti şu anda Azure erişim denetimi hizmeti kimlik doğrulama modelini destekler. Ancak, erişim denetimi yetkilendirme 22 Haziran 2018 üzerinde de kullanım dışı zordur. Bir Azure Active Directory kimlik doğrulama modeline mümkün olan en kısa sürede geçirmek öneririz.

## <a name="get-an-azure-ad-access-token"></a>Bir Azure AD erişim belirteci alma

Azure AD kimlik doğrulaması ile Azure Media Services API bağlanmak için bir Azure AD erişim belirteci istemek istemci uygulaması gerekir. Media Services .NET İstemci SDK kullandığınızda, bir Azure AD erişim belirtecini almak üzere nasıl kullanılacağı hakkındaki ayrıntıları çoğunu Sarmalanan ve uygulamasında Basitleştirilmiş [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) ve [AzureAdTokenCredentials](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) sınıfları. 

Örneğin, Azure AD yetkilisi, Media Services kaynak URI'si ya da yerel sağlamanız gerekmez Azure AD uygulama ayrıntıları. Bunlar Azure AD erişim belirteci sağlayıcısı sınıfı tarafından zaten yapılandırılmış iyi bilinen değerlerdir. 

Azure medya hizmeti .NET SDK'sı kullanmıyorsanız kullanmanız önerilir [Azure AD kimlik doğrulama Kitaplığı](../../active-directory/develop/active-directory-authentication-libraries.md). Azure AD kimlik doğrulama kitaplığı ile kullanmak için gereken parametreleri için değerleri almak için bkz: [Azure AD kimlik doğrulama ayarlarına erişmek için Azure portal'ı kullanmanızı](media-services-portal-get-started-with-aad.md).

Varsayılan uygulaması değiştirme seçeneğiniz de **AzureAdTokenProvider** kendi uygulaması.

## <a name="install-and-configure-azure-media-services-net-sdk"></a>Yükleme ve Azure Media Services .NET SDK'sını yapılandırma

>[!NOTE] 
>Media Services .NET SDK ile Azure AD kimlik doğrulaması kullanmak için en son gerekir [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) paket. Ayrıca, bir başvuru ekleyin **Microsoft.IdentityModel.Clients.activedirectory tarafından** derleme. Mevcut bir uygulamayı kullanıyorsanız dahil **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** derleme. 

1. Visual Studio'da yeni bir C# konsol uygulaması oluşturun.
2. Kullanım [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) yüklemek için NuGet paketi **Azure Media Services .NET SDK'sı**. 

    NuGet kullanarak başvurular eklemek için aşağıdaki adımları uygulayın: içinde **Çözüm Gezgini**proje adına sağ tıklayın ve ardından **Manage NuGet paketleri**. Ardından, arama **windowsazure.mediaservices** seçip **yükleme**.
    
    -veya-

    Aşağıdaki komutu çalıştırın **Paket Yöneticisi Konsolu** Visual Studio.

        Install-Package windowsazure.mediaservices -Version 4.0.0.4

3. Ekleme **kullanarak** kaynak kodunuz için.

        using Microsoft.WindowsAzure.MediaServices.Client; 

## <a name="use-user-authentication"></a>Kullanıcı kimlik doğrulamasını kullan

Kullanıcı kimlik doğrulaması seçeneği ile Azure medya hizmeti API'si bağlanmak için aşağıdaki parametreleri kullanarak bir Azure AD belirteç istemek istemci uygulamanın gerekir:  

- Azure AD Kiracı uç noktası. Kiracı bilgileri Azure portalından alınabilir. Sağ üst köşesindeki oturum açmış kullanıcı üzerine gelerek.
- Media Services kaynak URI'si.
- Media Services (yerel) uygulama istemci kimliği 
- Media Services (yerel) uygulama yeniden yönlendirme URI'si. 

Bu parametreler için değerler bulunabilir **AzureEnvironments.AzureCloudEnvironment**. **AzureEnvironments.AzureCloudEnvironment** yardımcı sağ ortamında ortak bir Azure veri merkezi için değişken ayarlarını almak için .NET SDK'sındaki sabittir. 

Önceden tanımlanmış ortam ayarları ortak veri merkezlerinde yalnızca Media Services erişmek için içerir. Sovereign veya kamu bulut bölgeleri için kullandığınız **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, veya **AzureGermanCloudEnvironment** sırasıyla.

Aşağıdaki kod örneğinde bir belirteç oluşturur:
    
    var tokenCredentials = new AzureAdTokenCredentials("microsoft.onmicrosoft.com", AzureEnvironments.AzureCloudEnvironment);
    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
  
Oluşturmanıza gerek Media Services karşı programlama başlatmak için bir **CloudMediaContext** sunucu bağlamı temsil eden örneği. **CloudMediaContext** işleri, varlıklar, dosyaları, erişim ilkeleri ve bulucular dahil olmak üzere önemli koleksiyonları başvurular içerir. 

Geçirmeniz gereken **kaynak medya REST Hizmetleri için URI** için **CloudMediaContext** Oluşturucusu. Kaynak URI için Azure Media Services hesabınız Media REST Hizmetleri, Azure portalında oturum açma seçin almak için seçin **API erişimini**ve ardından **kullanıcı kimlik doğrulaması ile Azure Media Services'e bağlanma**. 

Aşağıdaki kod örneği oluşturur bir **CloudMediaContext** örneği:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);

Aşağıdaki örnek, Azure AD belirteci ve bağlam nasıl oluşturulacağını gösterir:

    namespace AADAuthSample
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Specify your Azure AD tenant domain, for example "microsoft.onmicrosoft.com".
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR AAD TENANT DOMAIN HERE}", AzureEnvironments.AzureCloudEnvironment);
    
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
>Bildiren bir özel durum alırsanız "uzak sunucusu bir hata döndürdü: (401) yetkisiz" bkz [erişim denetimi](media-services-use-aad-auth-to-access-ams-api.md#access-control) erişme Azure Media Services API bölümle Azure AD kimlik doğrulamasına genel bakış.

## <a name="use-service-principal-authentication"></a>Hizmet asıl kimlik doğrulaması kullanma
    
Azure Media Services API (web API veya web uygulaması) gereksinimlerine göre hizmet asıl seçeneğiyle orta katman uygulamanızı bağlanmak için aşağıdaki parametrelerle bir Azure AD belirteci ister:  

- Azure AD Kiracı uç noktası. Kiracı bilgileri Azure portalından alınabilir. Sağ üst köşesindeki oturum açmış kullanıcı üzerine gelerek.
- Media Services kaynak URI'si.
- Azure AD uygulama değerleri: **istemci kimliği** ve **gizli**.

Değerleri **istemci kimliği** ve **gizli** parametreler Azure Portalı'nda bulunabilir. Daha fazla bilgi için bkz: [Azure portalını kullanarak Azure AD kimlik doğrulaması ile çalışmaya başlama](media-services-portal-get-started-with-aad.md).

Aşağıdaki kod örneği kullanarak bir belirteç oluşturur **AzureAdTokenCredentials** alan oluşturucu **AzureAdClientSymmetricKey** bir parametre olarak: 
    
    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                AzureEnvironments.AzureCloudEnvironment);

    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

Ayrıca belirtebilirsiniz **AzureAdTokenCredentials** alan oluşturucu **AzureAdClientCertificate** bir parametre olarak. 

Azure AD tarafından kullanılan bir biçimde bir sertifika oluşturulacağı ve yapılandırılacağı hakkında yönergeler için bkz: [sertifikalar - elle yapılandırma adımları arka plan programı uygulamalarla Azure AD kimlik doğrulaması](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).

    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientCertificate("{YOUR CLIENT ID HERE}", "{YOUR CLIENT CERTIFICATE THUMBPRINT}"), 
                                AzureEnvironments.AzureCloudEnvironment);

Oluşturmanıza gerek Media Services karşı programlama başlatmak için bir **CloudMediaContext** sunucu bağlamı temsil eden örneği. Geçirmeniz gereken **kaynak medya REST Hizmetleri için URI** için **CloudMediaContext** Oluşturucusu. Alma **kaynak medya REST Hizmetleri için URI** de Azure portalından değeri.

Aşağıdaki kod örneği oluşturur bir **CloudMediaContext** örneği:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
Aşağıdaki örnek, Azure AD belirteci ve bağlam nasıl oluşturulacağını gösterir:

    namespace AADAuthSample
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
