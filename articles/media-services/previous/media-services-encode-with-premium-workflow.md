---
title: Gelişmiş Media Encoder Premium iş akışı ile kodlama | Microsoft Docs
description: Media Encoder Premium iş akışı ile kodlama hakkında bilgi edinin. İçinde yazılan kod örneklerini C# ve .NET için Media Services SDK'sını kullanın.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: d20fc0cee6bce1389649e6287170b1963e799ccf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61463926"
---
# <a name="advanced-encoding-with-media-encoder-premium-workflow"></a>Gelişmiş Media Encoder Premium iş akışı ile kodlama
> [!NOTE]
> Bu makalede ele alınan Media Encoder Premium iş akışı medya işlemci Çin'de kullanılamaz.
>
>

Premium Kodlayıcı sorular için e-posta mepd@microsoft.com.

## <a name="overview"></a>Genel Bakış
Microsoft Azure Media Services ile tanışın **Media Encoder Premium iş akışı** Medya işleyicisi. Bu işlemci teklifler kodlama yetenekleri için premium isteğe bağlı akışlarınızı öncelikli.

İlgili ayrıntıları aşağıdaki konularda anahat **Media Encoder Premium iş akışı**:

* [Media Encoder Premium iş akışı tarafından desteklenen biçimleri](media-services-premium-workflow-encoder-formats.md) – Discusses dosya biçimleri ve codec bileşenleri tarafından desteklenen **Media Encoder Premium iş akışı**.
* [Genel bakış ve karşılaştırma Azure isteğe bağlı medya kodlayıcılarına](media-services-encode-asset.md) kodlama yeteneklerini karşılaştırır **Media Encoder Premium iş akışı** ve **Media Encoder Standard**.

Bu makalede ile kodlama yapmayı gösteren **Media Encoder Premium iş akışı** .NET kullanarak.

Görevler için kodlama **Media Encoder Premium iş akışı** bir iş akışı dosyası adlı ayrı bir yapılandırma dosyası, gerektirir. Bu dosyalar .workflow uzantısına sahiptir ve kullanılarak oluşturulan [iş akışı Tasarımcısı](media-services-workflow-designer.md) aracı.

Varsayılan iş akışı dosyalarını da edinebilirsiniz [burada](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Klasör, bu dosyaların açıklaması da içerir.

İş akışı dosyalarını Media Services hesabınıza bir varlık olarak yüklenmesi gerekir ve bu varlık için kodlama görevinin geçirilmelidir.

## <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 

## <a name="encoding-example"></a>Kodlama örneği

Aşağıdaki örnek ile kodlama yapmayı gösteren **Media Encoder Premium iş akışı**.

Aşağıdaki adımları gerçekleştirilir:

1. Bir varlık oluşturun ve bir iş akışı dosyasını karşıya yükleyin.
2. Bir varlık oluşturun ve bir kaynak medya dosyasını karşıya yükleyin.
3. "Medya Kodlayıcısı Premium iş akışı" medya işleyicisini alır.
4. Bir iş ve bir görev oluşturun.

    Çoğu durumda, yapılandırma görevi için boş dizedir (aşağıdaki örnekte ister). (Çalışma zamanı özellikleri dinamik olarak ayarlamak ihtiyacınız olan) bazı Gelişmiş senaryolar vardır, bu durumda kodlama görevi bir XML dizesini sağlar. Bu tür senaryoların örnekleri şunlardır: bir katman, birleştirme, subtitling paralel veya sıralı ortam oluşturma.
5. İki giriş varlıklar göreve ekleyin.

   1. 1 – iş akışı varlık.
   2. 2-video varlık.

      >[!NOTE]
      >İş akışı varlık görev medya varlığını önce eklenmesi gerekir.
      Bu görev için yapılandırma dizesi boş olmalıdır.
   
6. Kodlama işinin gönderin.

```csharp
using System;
using System.Linq;
using System.Configuration;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.MediaServices.Client;

namespace MediaEncoderPremiumWorkflowSample
{
    class Program
    {
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

        private static readonly string _supportFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _workflowFilePath =
            Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");

        private static readonly string _singleMP4InputFilePath =
            Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");

        static void Main(string[] args)
        {

            AzureAdTokenCredentials tokenCredentials =
                new AzureAdTokenCredentials(_AADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
            var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
            IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset);

        }

        static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
        {
            var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
            var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

            var fileName = Path.GetFileName(singleFilePath);

            var assetFile = asset.AssetFiles.Create(fileName);

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return asset;
        }

        static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Premium Workflow encoding job");
            // Get a media processor reference, and pass to it the name of the
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                processor,
                "",
                TaskOptions.None);

            // Specify the input asset to be encoded.
            task.InputAssets.Add(workflow);
            task.InputAssets.Add(video); // we add one asset
                                         // Add an output asset to contain the results of the job.
                                         // This output is specified as AssetCreationOptions.None, which
                                         // means the output asset is not encrypted.
            task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);

            // Use the following event handler to check job progress.  
            job.StateChanged += new
                    EventHandler<JobStateChangedEventArgs>(StateChanged);

            // Launch the job.
            job.Submit();

            // Check job execution and wait for job to finish.
            Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
            progressJobTask.Wait();

            // If job state is Error the event handling
            // method for job progress should log errors.  Here we check
            // for error state and exit if needed.
            if (job.State == JobState.Error)
            {
                throw new Exception("\nExiting method due to job error.");
            }

            return job.OutputMediaAssets[0];
        }

        static private void StateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);

            switch (e.CurrentState)
            {
                case JobState.Finished:
                    Console.WriteLine();
                    Console.WriteLine("Job is finished.");
                    Console.WriteLine();
                    break;
                case JobState.Canceling:
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    Console.WriteLine("Please wait...\n");
                    break;
                case JobState.Canceled:
                case JobState.Error:
                    // Cast sender as a job.
                    IJob job = (IJob)sender;
                    // Display or log error details as needed.
                    // LogJobStop(job.Id);
                    break;
                default:
                    break;
            }
        }
        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }
    }
}
```

Premium Kodlayıcı sorular için e-posta mepd@microsoft.com.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
