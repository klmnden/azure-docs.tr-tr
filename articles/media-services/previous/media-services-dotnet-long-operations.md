---
title: Uzun süre çalışan işlemleri yoklama | Microsoft Docs
description: Bu konu, uzun süre çalışan işlemleri yoklamak gösterilmektedir.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.assetid: 9a68c4b1-6159-42fe-9439-a3661a90ae03
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2017
ms.author: juliako
ms.openlocfilehash: fa456a2d0bb427af80d67a0b971fe8bd41bb7868
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788427"
---
# <a name="delivering-live-streaming-with-azure-media-services"></a>Azure Media Services ile canlı akış teslim etme

## <a name="overview"></a>Genel Bakış

Microsoft Azure Media Services işlemleri başlatmak için Media Services istekleri göndermek API'ler sunar (örneğin: oluşturma, başlatma, durdurma veya kanal silme). Bu uzun süre çalışan işlemlerdir.

Media Services .NET SDK'sı işlemin tamamlanmasını bekleyin ve isteği Gönder API'ler sağlar (dahili olarak, API işlem ilerlemesi için bazı aralıklarla yoklama). Örneğin, kanal çağırdığınızda. Kanal başlatıldıktan sonra Start(), yöntemi döndürür. Zaman uyumsuz sürümü de kullanabilirsiniz: kanal bekler. StartAsync() (görev tabanlı zaman uyumsuz desen hakkında daha fazla bilgi için bkz: [DOKUNUN](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)). İşlem isteği gönderin ve işlemi tamamlanana kadar durumunun yoklaması API'leri "yoklama yöntemleri" adı verilir. Bu yöntemler (özellikle zaman uyumsuz sürümü) zengin istemci uygulamaları ve/veya durum bilgisi olan hizmetler için önerilir.

Burada bir uygulama uzun süre çalışan bir http isteği için Bekleyemez ve işlem ilerlemesi için el ile yoklamaya istediği senaryo vardır. Tipik bir örnek durum bilgisiz web hizmetiyle etkileşim kurarken bir tarayıcı olabilir: tarayıcı bir kanal oluşturmak isterse, web hizmeti uzun süren bir işlem başlatır ve işlem kimliği tarayıcıya döndürür. Tarayıcı sonra kimliğine göre işlem durumunu almak için web hizmeti isteyebilirsiniz Media Services .NET SDK'sı, bu senaryo için yararlı olan API'ler sağlar. Bu API'leri "yoklama olmayan yöntemleri" adı verilir.
Aşağıdaki adlandırma deseni "Yoklama olmayan yöntemler" sahiptir: gönderme*OperationName*işlemi (örneğin, SendCreateOperation). Gönderme*OperationName*işlemi yöntemleri döndürür **IOperation** nesne; döndürülen nesnesi işlemi izlemek için kullanılan bilgileri içerir. Gönderme*OperationName*OperationAsync yöntemleri döndürür **görev<IOperation>**.

Şu anda aşağıdaki sınıflar yoklama olmayan yöntemlerini destekler: **kanal**, **StreamingEndpoint**, ve **Program**.

İçin işlem durumunu yoklamak için **GetOperation** yöntemi **OperationBaseCollection** sınıfı. İşlem durumunu denetlemek için şu aralıklar kullanın: için **kanal** ve **StreamingEndpoint** işlemleri, 30 saniye kullanın; **Program** işlemleri, 10 saniye kullanın.

## <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun.

## <a name="example"></a>Örnek

Aşağıdaki örnek adlı bir sınıf tanımlar **ChannelOperations**. Bu sınıf tanımını web hizmet sınıf tanımı için bir başlangıç noktası olabilir. Kolaylık olması için aşağıdaki örneklerde yöntemlerinin olmayan zaman uyumsuz sürümlerini kullanın.

Örnek, ayrıca istemciyi bu sınıfın nasıl kullanabilir gösterir.

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

