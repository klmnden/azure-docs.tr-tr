---
title: Medya Kodlayıcısı Premium akışıyla kodlama Gelişmiş | Microsoft Docs
description: Medya Kodlayıcısı Premium akışıyla kodlamak öğrenin. Kod örnekleri, C# dilinde yazılmıştır ve .NET için Media Services SDK'sını kullanın.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2017
ms.author: juliako
ms.openlocfilehash: 9b341b244d53993699dfc9096a86305def82cad7
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="advanced-encoding-with-media-encoder-premium-workflow"></a>Medya Kodlayıcısı Premium akışıyla kodlama Gelişmiş
> [!NOTE]
> Bu makalede ele alınan Medya Kodlayıcısı Premium iş akışı medya işlemcisi Çin'de kullanılamaz.
>
>

Premium Kodlayıcı sorular için e-posta mepd@microsoft.com.

## <a name="overview"></a>Genel Bakış
Microsoft Azure Media Services Tanıtımı **Medya Kodlayıcısı Premium iş akışı** medya işlemcisi. Bu işlemci teklifleri Gelişmiş özellikleri, premium isteğe bağlı iş akışları için kodlama.

Aşağıdaki konular ilgili ayrıntıları anahat **Medya Kodlayıcısı Premium iş akışı**:

* [Medya Kodlayıcısı Premium iş akışı tarafından desteklenen biçimler](media-services-premium-workflow-encoder-formats.md) – Discusses dosya biçimlerini ve desteklenen codec bileşenleri tarafından **Medya Kodlayıcısı Premium iş akışı**.
* [Genel bakış ve Azure isteğe bağlı medya kodlayıcılar karşılaştırması](media-services-encode-asset.md) kodlama özelliklerini karşılaştırır **Medya Kodlayıcısı Premium iş akışı** ve **Medya Kodlayıcısı standart**.

Bu makale ile kodlamak gösterilmiştir **Medya Kodlayıcısı Premium iş akışı** .NET kullanarak.

Görevler için kodlama **Medya Kodlayıcısı Premium iş akışı** bir iş akışı dosyası adlı bir ayrı yapılandırma dosyası gerektirir. Bu dosyalar .workflow uzantısına sahiptir ve kullanılarak oluşturulan [iş akışı Tasarımcısı](media-services-workflow-designer.md) aracı.

Varsayılan iş akışı dosyalarını alabilir [burada](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Klasör, bu dosyaların açıklaması da içerir.

Bir varlık olarak Media Services hesabınıza karşıya yüklenecek iş akışı dosyalarını gerekir ve bu varlık kodlama görevi geçirilmesi.

## <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 

## <a name="encoding-example"></a>Kodlama örneği

Aşağıdaki örnek ile kodlamak gösterilmiştir **Medya Kodlayıcısı Premium iş akışı**.

Aşağıdaki adımlar gerçekleştirilir:

1. Bir varlık oluşturun ve bir iş akışı dosyasını karşıya yükleyin.
2. Bir varlık oluşturun ve bir kaynak medya dosyasını karşıya yükleyin.
3. "Medya Kodlayıcısı Premium iş akışı" medya işlemcisi alın.
4. Bir işi ve bir görev oluşturun.

    Çoğu durumda, görev için yapılandırma dizesi boştur (aşağıdaki örnekte ister). (Çalışma zamanı özellikleri dinamik olarak ayarlamak gerekir) bazı Gelişmiş senaryolar vardır; bu durumda kodlama görev bir XML dizesini sağlar. Bu tür senaryoların örnekleri şunlardır: bir katmana, Dikiş, subtitling paralel veya sıralı medya oluşturma.
5. İki giriş varlıklar göreve ekleyin.

    1. 1 – iş akışı varlık.
    2. 2 – varlığı.

    >[!NOTE]
    >İş akışı varlık görev medya varlık önce eklenmesi gerekir.
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
