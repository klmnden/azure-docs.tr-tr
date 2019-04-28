---
title: .NET ile Media Services geliştirmeye yönelik bilgisayar ayarlama
description: .NET için Media Services SDK'sını kullanarak Media Services için önkoşulları hakkında bilgi edinin. Ayrıca bir Visual Studio uygulamasının nasıl oluşturulacağını öğrenin.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: ec2804c7-c656-4fbf-b3e4-3f0f78599a7f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 47db5ba826b94422672dd46b191556da43b70b02
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61465006"
---
# <a name="media-services-development-with-net"></a>.NET ile Media Services Geliştirme 
[!INCLUDE [media-services-selector-setup](../../../includes/media-services-selector-setup.md)]

Bu makalede, .NET ile Media Services uygulama geliştirmeye başlamanın anlatılmaktadır.

**Azure Media Services .NET SDK'sı** kitaplığı .NET kullanarak Media Services karşı programlama yapmanızı sağlar. .NET ile geliştirme daha da kolay hale getirmek için **Azure Media Services .NET SDK uzantıları** kitaplığı sağlanır. Bu kitaplık, genişletme yöntemleri ve .NET kodunuzu basitleştirerek yardımcı işlevler kümesi içerir. Her iki kitaplıkları aracılığıyla kullanılabilir **NuGet** ve **GitHub**.

## <a name="prerequisites"></a>Önkoşullar
* Yeni veya mevcut bir Azure aboneliğinde bir Media Services hesabı. Makaleye göz atın [Media Services hesabı oluşturma](media-services-portal-create-account.md).
* İşletim sistemleri: Windows 10, Windows 7, Windows 2008 R2 veya Windows 8.
* .NET framework 4.5 veya üzeri.
* Visual Studio.

## <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma
Bu bölümde Visual Studio'da bir proje oluşturun ve Media Services Geliştirme döngünüzün kümesi gösterilmektedir.  Bu durumda, projedir bir C# Windows konsol uygulaması, ancak burada gösterilen aynı kurulum adımları diğer (örneğin, bir Windows Forms uygulaması veya ASP.NET Web uygulaması) için Media Services uygulamaları oluşturma proje türleri için geçerlidir.

Bu bölümde, nasıl kullanılacağını gösterir **NuGet** Media Services .NET SDK uzantıları ve diğer bağımlı kitaplıkların eklemek için.

Alternatif olarak, Github'dan son Media Services .NET SDK'sı bitleri alabilirsiniz ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) veya [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)) çözümü oluşturun ve istemci projesi başvuruları ekleyin. Gereken tüm bağımlılıkları indirilir ve otomatik olarak ayıklanan.

1. Visual Studio’da yeni bir C# Konsol Uygulaması oluşturun. Girin **adı**, **konumu**, ve **çözüm adı**ve ardından Tamam'a tıklayın.
2. Çözümü derleyin.
3. Kullanım **NuGet** yüklemek ve eklemek için **Azure Media Services .NET SDK uzantıları** (**windowsazure.mediaservices.extensions**). Bu paketin yüklenmesiyle **Media Services .NET SDK** da yüklenir ve diğer tüm gerekli bağımlılıklar eklenir.
   
    En yeni sürümünü NuGet yüklü olduğundan emin olun. Daha fazla bilgi ve yükleme yönergeleri için bkz. [NuGet](https://nuget.codeplex.com/).

    1. Çözüm Gezgini'nde proje adına sağ tıklayın ve seçin **NuGet paketlerini Yönet**.

    2. NuGet paketlerini Yönet iletişim kutusu görüntülenir.

    3. Çevrimiçi Galerisi, Azure MediaServices uzantıları, arama düğmesini **Azure Media Services .NET SDK uzantıları** (**windowsazure.mediaservices.extensions**) ve ardından  **Yükleme** düğmesi.
   
    4. Proje değiştirildi ve Media Services .NET SDK uzantıları, Media Services .NET SDK'sı ve diğer bağımlı derlemeler için başvurular eklenir.
4. Temiz bir geliştirme ortamı yükseltmek için NuGet paketi geri yüklemeyi etkinleştirme göz önünde bulundurun. Daha fazla bilgi için [NuGet paketi geri yüklemeyi "](https://docs.nuget.org/consume/package-restore).
5. Bir başvuru ekleyin **System.Configuration** derleme. Bu derleme System.Configuration içerir. **ConfigurationManager** yapılandırma dosyaları (örneğin, App.config) erişmek için kullanılan sınıf.
   
    1. Başvuruları Yönet iletişim kutusunu kullanarak başvurular eklemek için Çözüm Gezgini'nde proje adına sağ tıklayın. ' A tıklayarak **Ekle**, ardından **başvurusu...** .
   
    2. Başvuruları Yönet iletişim kutusu görüntülenir.
    3. .NET framework derlemeleri altındaki bulun ve System.Configuration bütünleştirilmiş koduna seçip ENTER tuşuna **Tamam**.
6. App.config dosyasını açın ve eklemek bir **appSettings** dosyasına bölümü. Media Services API'sine bağlanmak için gerekli değerleri ayarlayın. Daha fazla bilgi için [Azure AD kimlik doğrulamasıyla Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md). 

    Kullanarak bağlanmak için gerekli değerleri ayarlamak **hizmet sorumlusu** kimlik doğrulama yöntemi.

        ```csharp
                <configuration>
                ...
                    <appSettings>
                        <add key="AMSAADTenantDomain" value="tenant"/>
                        <add key="AMSRESTAPIEndpoint" value="endpoint"/>
                        <add key="AMSClientId" value="id"/>
                        <add key="AMSClientSecret" value="secret"/>
                    </appSettings>
                </configuration>
        ```

7. Ekleme **System.Configuration** projenize bir başvuru.
8. Var olanın üzerine yaz **kullanarak** başında deyimlerini Program.cs dosyasının aşağıdaki kod ile:

    ```csharp      
            using System;
            using System.Configuration;
            using System.IO;
            using Microsoft.WindowsAzure.MediaServices.Client;
            using System.Threading;
            using System.Collections.Generic;
            using System.Linq;
    ```

    Bu noktada, Media Services uygulama geliştirmeye başlamak hazırsınız.    

## <a name="example"></a>Örnek

AMS API'ye bağlanan ve tüm kullanılabilir medya işlemcileri listeler küçük bir örnek aşağıda verilmiştir.

```csharp
        class Program
        {
            // Read values from the App.config file.

            private static readonly string _AADTenantDomain =
                ConfigurationManager.AppSettings["AMSAADTenantDomain"];
            private static readonly string _RESTAPIEndpoint =
                ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
            private static readonly string _AMSClientId =
                ConfigurationManager.AppSettings["AMSClientId"];
            private static readonly string _AMSClientSecret =
                ConfigurationManager.AppSettings["AMSClientSecret"];
        
            private static CloudMediaContext _context = null;
            static void Main(string[] args)
            {
                AzureAdTokenCredentials tokenCredentials = 
                    new AzureAdTokenCredentials(_AADTenantDomain,
                        new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                        AzureEnvironments.AzureCloudEnvironment);

                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
        
                // List all available Media Processors
                foreach (var mp in _context.MediaProcessors)
                {
                    Console.WriteLine(mp.Name);
                }
        
            }
 ```

## <a name="next-steps"></a>Sonraki adımlar

Artık [AMS API'ye bağlanabilir](media-services-use-aad-auth-to-access-ams-api.md) ve başlangıç [geliştirme](media-services-dotnet-get-started.md).


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

