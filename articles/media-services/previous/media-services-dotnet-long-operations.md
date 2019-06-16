---
title: Uzun süre çalışan işlemleri yoklama | Microsoft Docs
description: Bu konuda, uzun süre çalışan işlemleri yoklamak gösterilmektedir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 9a68c4b1-6159-42fe-9439-a3661a90ae03
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 752c502268ef53d3c0575d92e75ce6a965fccd9f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61464989"
---
# <a name="delivering-live-streaming-with-azure-media-services"></a>Azure Media Services ile canlı akış sunma

## <a name="overview"></a>Genel Bakış

Microsoft Azure Media Services istekleri gönderme işlemleri başlatmak için Media Services için API'ler sunar (örneğin: oluşturma, başlatma, durdurma veya bir kanalı Sil). Bu uzun süre çalışan işlemlerdir.

Media Services .NET SDK'sı, istek gönderebilir ve işlemin tamamlanmasını bekleyin API'ler sağlar (dahili olarak, API işlem ilerleme durumunu için bazı aralıklarla yoklama). Örneğin, kanal çağırdığınızda. Kanal başlatıldıktan sonra Start() yöntemi döndürür. Zaman uyumsuz sürümü de kullanabilirsiniz: kanal bekler. StartAsync() (görev tabanlı zaman uyumsuz desen hakkında daha fazla bilgi için bkz. [DOKUNUN](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)). Bir operation isteğini gönderin ve ardından, işlemi tamamlanana kadar durumunu yoklamak API'leri "yoklama yöntemleri" adı verilir. Bu yöntemler (özellikle zaman uyumsuz sürümü) zengin istemci uygulamaları ve/veya durum bilgisi olan hizmetler için önerilir.

Burada bir uygulamayı uzun süre çalışan bir http isteği için sabırsızlanıyoruz ve işlemi ilerleme durumu el ile yoklamaya istediği senaryo vardır. Örnek durum bilgisi olmayan web hizmetiyle etkileşim kurmanın bir tarayıcı olacaktır: tarayıcı bir kanal oluşturmak istediğinde, web hizmeti uzun süren bir işlem başlatır ve işlem kimliği tarayıcıya döndürür. Tarayıcı kimliğini temel alan işlem durumunu almak için web hizmeti ardından sorabilirsiniz Media Services .NET SDK, bu senaryo için faydalı olan API'leri sağlar. Bu API'ler, "yoklama olmayan yöntemleri" adı verilir.
"Yoklama olmayan yöntemler" aşağıdaki adlandırma deseni sahiptir: Gönderme*OperationName*işlemi (örneğin, SendCreateOperation). Gönderme*OperationName*işlemi yöntemleri dönüş **IOperation** nesne; döndürülen nesnesi işlemi izlemek için kullanılan bilgileri içerir. Gönderme*OperationName*OperationAsync yöntemleri dönüş **görev\<IOperation >** .

Şu anda aşağıdaki sınıflar, yoklama olmayan yöntemleri destekler:  **Kanal**, **StreamingEndpoint**, ve **Program**.

İçin işlem durumunu yoklamak için **GetOperation** metodunda **OperationBaseCollection** sınıfı. İşlem durumunu denetlemek için şu aralıklar kullanın: için **kanal** ve **StreamingEndpoint** işlemleri, 30 saniye kullanın; **Program** işlemleri, 10 saniye kullanın.

## <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun.

## <a name="example"></a>Örnek

Aşağıdaki örnek adlı bir sınıf tanımlar **ChannelOperations**. Bu sınıf tanımı, web hizmeti sınıfı tanımınız için bir başlangıç noktası olabilir. Kolaylık olması için aşağıdaki örnekleri yöntemlerinin olmayan zaman uyumsuz sürümlerini kullanın.

Bu sınıf istemci nasıl kullanacağınız örnek ayrıca gösterir.

### <a name="channeloperations-class-definition"></a>ChannelOperations sınıf tanımı

```csharp
using Microsoft.WindowsAzure.MediaServices.Client;
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Net;

/// <summary> 
/// The ChannelOperations class only implements 
/// the Channel’s creation operation. 
/// </summary> 
public class ChannelOperations
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

    // Field for service context.
    private static CloudMediaContext _context = null;

    public ChannelOperations()
    {
        AzureAdTokenCredentials tokenCredentials = 
            new AzureAdTokenCredentials(_AADTenantDomain,
                new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                AzureEnvironments.AzureCloudEnvironment);

        var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

        _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
    }

    /// <summary>  
    /// Initiates the creation of a new channel.  
    /// </summary>  
    /// <param name="channelName">Name to be given to the new channel</param>  
    /// <returns>  
    /// Operation Id for the long running operation being executed by Media Services. 
    /// Use this operation Id to poll for the channel creation status. 
    /// </returns> 
    public string StartChannelCreation(string channelName)
    {
        var operation = _context.Channels.SendCreateOperation(
            new ChannelCreationOptions
            {
                Name = channelName,
                Input = CreateChannelInput(),
                Preview = CreateChannelPreview(),
                Output = CreateChannelOutput()
            });

        return operation.Id;
    }

    /// <summary> 
    /// Checks if the operation has been completed. 
    /// If the operation succeeded, the created channel Id is returned in the out parameter.
    /// </summary> 
    /// <param name="operationId">The operation Id.</param> 
    /// <param name="channel">
    /// If the operation succeeded, 
    /// the created channel Id is returned in the out parameter.</param>
    /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
    public bool IsCompleted(string operationId, out string channelId)
    {
        IOperation operation = _context.Operations.GetOperation(operationId);
        bool completed = false;

        channelId = null;

        switch (operation.State)
        {
            case OperationState.Failed:
                // Handle the failure. 
                // For example, throw an exception. 
                // Use the following information in the exception: operationId, operation.ErrorMessage.
                break;
            case OperationState.Succeeded:
                completed = true;
                channelId = operation.TargetEntityId;
                break;
            case OperationState.InProgress:
                completed = false;
                break;
        }
        return completed;
    }

    private static ChannelInput CreateChannelInput()
    {
        return new ChannelInput
        {
            StreamingProtocol = StreamingProtocol.RTMP,
            AccessControl = new ChannelAccessControl
            {
                IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelInput001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
            }
        };
    }

    private static ChannelPreview CreateChannelPreview()
    {
        return new ChannelPreview
        {
            AccessControl = new ChannelAccessControl
            {
                IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelPreview001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
            }
        };
    }

    private static ChannelOutput CreateChannelOutput()
    {
        return new ChannelOutput
        {
            Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
        };
    }
}
```

### <a name="the-client-code"></a>İstemci kodu

```csharp
ChannelOperations channelOperations = new ChannelOperations();
string opId = channelOperations.StartChannelCreation("MyChannel001");

string channelId = null;
bool isCompleted = false;

while (isCompleted == false)
{
    System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
    isCompleted = channelOperations.IsCompleted(opId, out channelId);
}

// If we got here, we should have the newly created channel id.
Console.WriteLine(channelId);
```

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

