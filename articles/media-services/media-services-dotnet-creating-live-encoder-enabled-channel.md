---
title: ".NET ile çoklu bit hızına sahip akışlar oluşturmak için Azure Media Services&quot;ı kullanarak canlı akış gerçekleştirme | Microsoft Belgeleri"
description: "Bu öğreticide, tek bit hızında bir canlı akışı alıp .NET SDK kullanarak çoklu bit hızında akışa kodlayan bir Kanal oluşturulması adım adım anlatılmaktadır."
services: media-services
documentationcenter: 
author: anilmur
manager: erikre
editor: 
ms.assetid: 4df5e690-ff63-47cc-879b-9c57cb8ec240
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/05/2017
ms.author: juliako;anilmur
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 5c26aaea6acfab8c4c60478968e0b68543086a9d
ms.lasthandoff: 03/14/2017


---
# <a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-net"></a>.NET çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services’i kullanarak canlı akış gerçekleştirme
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
> * [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
> * [REST API](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
> 
> 

## <a name="overview"></a>Genel Bakış
Bu öğreticide, tek bit hızında bir canlı akışı alıp çoklu bit hızında akışa kodlayan bir **Kanal** oluşturulması adım adım anlatılmaktadır.

Gerçek zamanlı kodlama için etkinleştirilmiş Kanallar ile ilgili daha fazla kavramsal bilgi için bkz. [Çoklu bit hızı akışları oluşturmak için Azure Media Services kullanarak canlı akış](media-services-manage-live-encoder-enabled-channels.md).

## <a name="common-live-streaming-scenario"></a>Ortak Canlı Akış Senaryosu
Aşağıdaki adımlar, ortak canlı akış uygulamaları oluşturmak için gerekli olan görevleri açıklamaktadır.

> [!NOTE]
> Canlı bir etkinlik için önerilen en uzun süre şu anda 8 saattir. Daha uzun bir süre için bir Kanal çalıştırmanız gerekiyorsa lütfen amslived@microsoft.com adresine başvurun.
> 
> 

1. Bilgisayara bir video kamera bağlayın. Şu protokollerin birinde tek bit hızlı bir akış çıkışı sağlayabilecek şirket içi bir gerçek zamanlı kodlayıcı başlatıp bunu yapılandırın: RTMP, Kesintisiz Akış veya RTP (MPEG-TS). Daha fazla bilgi için bkz. [Azure Media Services RTMP Desteği ve Gerçek Zamanlı Kodlayıcılar](http://go.microsoft.com/fwlink/?LinkId=532824).

    Bu adım, Kanalınızı oluşturduktan sonra da gerçekleştirilebilir.

2. Bir Kanal oluşturup başlatın.
3. Kanal alma URL’sini alın.

    Alma URL’si gerçek zamanlı kodlayıcı tarafından akışı Kanala göndermek için kullanılır.

4. Kanal önizleme URL’sini alın.

    Kanalınızın canlı akışı düzgün şekilde aldığını doğrulamak için bu URL’yi kullanın.

5. Bir varlık oluşturun.
6. Varlığın kayıttan yürütme sırasında dinamik olarak şifrelenmesini istiyorsanız aşağıdakileri yapın:
7. Bir içerik anahtarı oluşturun.
8. İçerik anahtarının yetkilendirme ilkesini yapılandırın.
9. Varlık teslim ilkesini (dinamik paketleme ve dinamik şifreleme tarafından kullanılır) yapılandırın.
10. Bir program oluşturun ve oluşturduğunuz varlığın kullanılacağını belirtin.
11. Bir OnDemand bulucu oluşturarak programla ilişkili varlığı yayımlayın.

    >[!NOTE]
    >AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumunda olması gerekir. 

12. Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda programı başlatın.
13. İsteğe bağlı olarak, gerçek zamanlı kodlayıcıya bir reklam başlatması bildirilebilir. Reklam, çıktı akışına eklenir.
14. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun.
15. Programı silin (ve isteğe bağlı olarak varlığı da silin).

## <a name="what-youll-learn"></a>Öğrenecekleriniz
Bu konuda, Media Services .NET SDK'sını kullanarak kanallar ve programlarda farklı işlemlerin nasıl yürütüleceği gösterilmektedir. İşlemlerin çoğu uzun süre çalışacağından, uzun süre çalışan işlemleri yöneten .NET API'leri kullanılmaktadır.

Bu konuda aşağıdakilerin nasıl gerçekleştirileceği gösterilmektedir.

1. Bir kanal oluşturup başlatın. Uzun süre çalışan API'ler kullanılacaktır.
2. Kanalın alma (giriş) uç noktasını alın. Bu uç noktası, tek bit hızlı bir canlı akış gönderebilen kodlayıcıya sağlanmalıdır.
3. Önizleme uç noktasını alın. Bu uç noktası, akışınızın önizlemesini gerçekleştirmek için kullanılır.
4. İçeriğinizi depolamak için kullanılacak bir varlık oluşturun. Bu örnekte gösterilen şekilde varlık teslim ilkeleri de yapılandırılmalıdır.
5. Bir program oluşturun ve daha önce oluşturulan varlığın kullanılacağını belirtin. Programı başlatın. Uzun süre çalışan API'ler kullanılacaktır.
6. İçeriğin yayımlanabilmesi ve istemcilerinize gönderilebilmesi için varlığa ilişkin bir bulucu oluşturun.
7. Maskeleme görüntülerini gösterin ve gizleyin. Reklamları başlatın ve durdurun. Uzun süre çalışan API'ler kullanılacaktır.
8. Kanalınızı ve ilişkili tüm kaynakları temizleyin.

## <a name="prerequisites"></a>Önkoşullar
Öğreticiyi tamamlamak için aşağıdakiler gereklidir.

* Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir.

Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Ücretli Azure hizmetlerini denemek için kullanabileceğiniz krediler alırsınız. Krediler bitmiş olsa bile hesabı sürdürebilir ve Azure App Service’deki Web Apps özelliği gibi ücretsiz Azure hizmetlerinden faydalanabilirsiniz.

* Bir Media Services hesabı. Media Services hesabı oluşturma konusunda bilgi edinmek için bkz. [Hesap Oluşturma](media-services-portal-create-account.md).
* Visual Studio 2010 SP1 (Professional, Premium, Ultimate veya Express) veya sonraki sürümleri.
* Media Services .NET SDK sürüm 3.2.0.0 veya daha yeni bir sürümünü kullanmanız gerekir.
* Bir web kamerası ve tek bit hızlı bir canlı akış gönderebilen bir kodlayıcı.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* Canlı bir etkinlik için önerilen en uzun süre şu anda 8 saattir. Daha uzun bir süre için bir Kanal çalıştırmanız gerekiyorsa lütfen amslived@microsoft.com adresine başvurun.
* Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). Daha fazla bilgi için [bu](media-services-dotnet-manage-entities.md#limit-access-policies) konu başlığına bakın.


## <a name="download-sample"></a>Örnek indirme
[Buradan](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/) bir örnek alarak çalıştırın.

## <a name="set-up-for-development-with-media-services-sdk-for-net"></a>.NET için Media Services SDK ile geliştirme amaçlı ayarlama
1. Visual Studio'yu kullanarak bir konsol uygulaması oluşturun.
2. Media Services NuGet paketini kullanarak .NET için Media Services SDK'sını konsol uygulamanıza ekleyin.

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak
En iyi uygulama olarak, bir app.config dosyası kullanarak Media Services adını ve hesap anahtarını depolamanız gerekir.

> [!NOTE]
> Adı ve Anahtar değerlerini bulmak için Azure portal’a gidin ve hesabınızı seçin. Sağda Ayarlar penceresi görüntülenir. Ayarlar penceresinde Anahtarlar’ı seçin. Her bir metin kutusunun yanındaki simgeye tıklandığında söz konusu değer sistem panosuna kopyalanır.
> 
> 

App.config dosyasına appSettings bölümünü ekleyin ve Media Services hesap adı ve hesap anahtarınızın değerlerini ayarlayın.

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
          <add key="MediaServicesAccountName" value="YouMediaServicesAccountName" />
          <add key="MediaServicesAccountKey" value="YouMediaServicesAccountKey" />
      </appSettings>
    </configuration>


## <a name="code-example"></a>Kod örneği

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Net;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;

    namespace EncodeLiveStreamWithAmsClear
    {
        class Program
        {
            private const string ChannelName = "channel001";
            private const string AssetlName = "asset001";
            private const string ProgramlName = "program001";

            // Read values from the App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];

            // Field for service context.
            private static CloudMediaContext _context = null;
            private static MediaServicesCredentials _cachedCredentials = null;


            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                IChannel channel = CreateAndStartChannel();

                // The channel's input endpoint:
                string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();

                Console.WriteLine("Intest URL: {0}", ingestUrl);


                // Use the previewEndpoint to preview and verify 
                // that the input from the encoder is actually reaching the Channel. 
                string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();

                Console.WriteLine("Preview URL: {0}", previewEndpoint);

                // When Live Encoding is enabled, you can now get a preview of the live feed as it reaches the Channel. 
                // This can be a valuable tool to check whether your live feed is actually reaching the Channel. 
                // The thumbnail is exposed via the same end-point as the Channel Preview URL.
                string thumbnailUri = new UriBuilder
                {
                    Scheme = Uri.UriSchemeHttps,
                    Host = channel.Preview.Endpoints.FirstOrDefault().Url.Host,
                    Path = "thumbnails/input.jpg"
                }.Uri.ToString();

                Console.WriteLine("Thumbain URL: {0}", thumbnailUri);

                // Once you previewed your stream and verified that it is flowing into your Channel, 
                // you can create an event by creating an Asset, Program, and Streaming Locator. 
                IAsset asset = CreateAndConfigureAsset();

                IProgram program = CreateAndStartProgram(channel, asset);

                ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);

                // You can use slates and ads only if the channel type is Standard.  
                StartStopAdsSlates(channel);

                // Once you are done streaming, clean up your resources.
                Cleanup(channel);

            }

            public static IChannel CreateAndStartChannel()
            {
                var channelInput = CreateChannelInput();
                var channePreview = CreateChannelPreview();
                var channelEncoding = CreateChannelEncoding();


                ChannelCreationOptions options = new ChannelCreationOptions
                {
                    EncodingType = ChannelEncodingType.Standard,
                    Name = ChannelName,
                    Input = channelInput,
                    Preview = channePreview,
                    Encoding = channelEncoding
                };

                Log("Creating channel");
                IOperation channelCreateOperation = _context.Channels.SendCreateOperation(options);
                string channelId = TrackOperation(channelCreateOperation, "Channel create");

                IChannel channel = _context.Channels.Where(c => c.Id == channelId).FirstOrDefault();

                Log("Starting channel");
                var channelStartOperation = channel.SendStartOperation();
                TrackOperation(channelStartOperation, "Channel start");

                return channel;
            }

            /// <summary>
            /// Create channel input, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelInput CreateChannelInput()
            {
                return new ChannelInput
                {
                    StreamingProtocol = StreamingProtocol.RTPMPEG2TS,
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

            /// <summary>
            /// Create channel preview, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
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

            /// <summary>
            /// Create channel encoding, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelEncoding CreateChannelEncoding()
            {
                return new ChannelEncoding
                {
                    SystemPreset = "Default720p",
                    IgnoreCea708ClosedCaptions = false,
                    AdMarkerSource = AdMarkerSource.Api,
                    // You can only set audio if streaming protocol is set to StreamingProtocol.RTPMPEG2TS.
                    AudioStreams = new List<AudioStream> { new AudioStream { Index = 103, Language = "eng" } }.AsReadOnly()
                };
            }

            /// <summary>
            /// Create an asset and configure asset delivery policies.
            /// </summary>
            /// <returns></returns>
            public static IAsset CreateAndConfigureAsset()
            {
                IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);

                IAssetDeliveryPolicy policy =
                    _context.AssetDeliveryPolicies.Create("Clear Policy",
                    AssetDeliveryPolicyType.NoDynamicEncryption,
                    AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);

                asset.DeliveryPolicies.Add(policy);

                return asset;
            }

            /// <summary>
            /// Create a Program on the Channel. You can have multiple Programs that overlap or are sequential;
            /// however each Program must have a unique name within your Media Services account.
            /// </summary>
            /// <param name="channel"></param>
            /// <param name="asset"></param>
            /// <returns></returns>
            public static IProgram CreateAndStartProgram(IChannel channel, IAsset asset)
            {
                IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
                Log("Program created", program.Id);

                Log("Starting program");
                var programStartOperation = program.SendStartOperation();
                TrackOperation(programStartOperation, "Program start");

                return program;
            }

            /// <summary>
            /// Create locators in order to be able to publish and stream the video.
            /// </summary>
            /// <param name="asset"></param>
            /// <param name="ArchiveWindowLength"></param>
            /// <returns></returns>
            public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
            {
                 // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                var locator = _context.Locators.CreateLocator
                    (
                        LocatorType.OnDemandOrigin,
                        asset,
                        _context.AccessPolicies.Create
                            (
                                "Live Stream Policy",
                                ArchiveWindowLength,
                                AccessPermissions.Read
                            )
                    );

                return locator;
            }

            /// <summary>
            /// Perform operations on slates.
            /// </summary>
            /// <param name="channel"></param>
            public static void StartStopAdsSlates(IChannel channel)
            {
                int cueId = new Random().Next(int.MaxValue);
                var path = Path.GetFullPath(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"..\\..\\SlateJPG\\DefaultAzurePortalSlate.jpg"));

                Log("Creating asset");
                var slateAsset = _context.Assets.Create("Slate test asset " + DateTime.Now.ToString("yyyy-MM-dd HH-mm"), AssetCreationOptions.None);
                Log("Slate asset created", slateAsset.Id);

                Log("Uploading file");
                var assetFile = slateAsset.AssetFiles.Create("DefaultAzurePortalSlate.jpg");
                assetFile.Upload(path);
                assetFile.IsPrimary = true;
                assetFile.Update();

                Log("Showing slate");
                var showSlateOpeartion = channel.SendShowSlateOperation(TimeSpan.FromMinutes(1), slateAsset.Id);
                TrackOperation(showSlateOpeartion, "Show slate");

                Log("Hiding slate");
                var hideSlateOperation = channel.SendHideSlateOperation();
                TrackOperation(hideSlateOperation, "Hide slate");

                Log("Starting ad");
                var startAdOperation = channel.SendStartAdvertisementOperation(TimeSpan.FromMinutes(1), cueId, false);
                TrackOperation(startAdOperation, "Start ad");

                Log("Ending ad");
                var endAdOperation = channel.SendEndAdvertisementOperation(cueId);
                TrackOperation(endAdOperation, "End ad");

                Log("Deleting slate asset");
                slateAsset.Delete();
            }

            /// <summary>
            /// Clean up resources associated with the channel.
            /// </summary>
            /// <param name="channel"></param>
            public static void Cleanup(IChannel channel)
            {
                IAsset asset;
                if (channel != null)
                {
                    foreach (var program in channel.Programs)
                    {
                        asset = _context.Assets.Where(se => se.Id == program.AssetId)
                                                .FirstOrDefault();

                        Log("Stopping program");
                        var programStopOperation = program.SendStopOperation();
                        TrackOperation(programStopOperation, "Program stop");

                        program.Delete();

                        if (asset != null)
                        {
                            Log("Deleting locators");
                            foreach (var l in asset.Locators)
                                l.Delete();

                            Log("Deleting asset");
                            asset.Delete();
                        }
                    }

                    Log("Stopping channel");
                    var channelStopOperation = channel.SendStopOperation();
                    TrackOperation(channelStopOperation, "Channel stop");

                    Log("Deleting channel");
                    var channelDeleteOperation = channel.SendDeleteOperation();
                    TrackOperation(channelDeleteOperation, "Channel delete");
                }
            }


            /// <summary>
            /// Track long running operations.
            /// </summary>
            /// <param name="operation"></param>
            /// <param name="description"></param>
            /// <returns></returns>
            public static string TrackOperation(IOperation operation, string description)
            {
                string entityId = null;
                bool isCompleted = false;

                Log("starting to track ", null, operation.Id);
                while (isCompleted == false)
                {
                    operation = _context.Operations.GetOperation(operation.Id);
                    isCompleted = IsCompleted(operation, out entityId);
                    System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
                }
                // If we got here, the operation succeeded.
                Log(description + " in completed", operation.TargetEntityId, operation.Id);

                return entityId;
            }

            /// <summary> 
            /// Checks if the operation has been completed. 
            /// If the operation succeeded, the created entity Id is returned in the out parameter.
            /// </summary> 
            /// <param name="operationId">The operation Id.</param> 
            /// <param name="channel">
            /// If the operation succeeded, 
            /// the entity Id associated with the sucessful operation is returned in the out parameter.</param>
            /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
            private static bool IsCompleted(IOperation operation, out string entityId)
            {

                bool completed = false;

                entityId = null;

                switch (operation.State)
                {
                    case OperationState.Failed:
                        // Handle the failure. 
                        // For example, throw an exception. 
                        // Use the following information in the exception: operationId, operation.ErrorMessage.
                        Log("operation failed", operation.TargetEntityId, operation.Id);
                        break;
                    case OperationState.Succeeded:
                        completed = true;
                        entityId = operation.TargetEntityId;
                        break;
                    case OperationState.InProgress:
                        completed = false;
                        Log("operation in progress", operation.TargetEntityId, operation.Id);
                        break;
                }
                return completed;
            }


            private static void Log(string action, string entityId = null, string operationId = null)
            {
                Console.WriteLine(
                    "{0,-21}{1,-51}{2,-51}{3,-51}",
                    DateTime.Now.ToString("yyyy'-'MM'-'dd HH':'mm':'ss"),
                    action,
                    entityId ?? string.Empty,
                    operationId ?? string.Empty);
            }
        }
    }    


## <a name="next-step"></a>Sonraki adım
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



