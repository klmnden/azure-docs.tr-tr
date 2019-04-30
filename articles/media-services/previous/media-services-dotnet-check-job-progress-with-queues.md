---
title: .NET ile Media Services iş bildirimlerini izlemek için Azure kuyruk depolama kullanma | Microsoft Docs
description: Azure kuyruk depolama, Media Services iş bildirimlerini izlemek için kullanmayı öğrenin. Kod örneği yazıldığı C# ve .NET için Media Services SDK'sını kullanır.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: f535d0b5-f86c-465f-81c6-177f4f490987
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 811ed6dde4ed98222bdd2589d8d6fdd8e0f64ce8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61465635"
---
# <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications-with-net"></a>.NET ile Media Services iş bildirimlerini izlemek için Azure kuyruk depolama kullanma 

Kodlama işi çalıştırdığınızda, işin ilerleme durumunu izlemek için bir yol genellikle gerektirir. Media Services'ı için bildirimleri göndermeyi yapılandırabilirsiniz [Azure kuyruk depolama](../../storage/storage-dotnet-how-to-use-queues.md). Kuyruk Depolama'yı bildirim alarak, işin ilerleme durumunu izleyebilirsiniz. 

Mesajlar için kuyruk depolama alanından herhangi bir dünyada erişilebilir. Kuyruk depolama Mesajlaşma mimarisi, güvenilir ve yüksek düzeyde ölçeklenebilir. Kuyruk depolama iletiler için yoklama diğer yöntemleri kullanmayı üzerinden önerilir.

Media Services bildirimlerini dinleme sık karşılaşılan bir senaryodur sonra bazı ek görevleri gerçekleştirmek için gereken bir içerik yönetim sistemi geliştiriyorsanız (sonraki adım bir iş akışında veya yayımlamak için tetikleyici için bir kodlama işi tamamlandıktan olduğu içeriği).

Bu makalede, kuyruk Depolama'yı bildirim iletilerinin alınacağı gösterilmektedir.  

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Kuyruk depolama kullanan bir Media Services uygulamaları geliştirirken, aşağıdakileri dikkate alın:

* Kuyruk depolama, ilk-giren ilk çıkar garantili sağlamaz (FIFO) sıralı teslim. Daha fazla bilgi için [Azure kuyrukları ve Azure hizmet veri yolu kuyrukları karşılaştırıldığında ve Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).
* Kuyruk depolama, anında iletme hizmeti değil. Sıra yoklamak sahip.
* İstenen sayıda kuyruk olabilir. Daha fazla bilgi için [kuyruk hizmeti REST API](https://docs.microsoft.com/rest/api/storageservices/Queue-Service-REST-API).
* Kuyruk depolama, bazı sınırlamalar ve dikkat edilmesi gereken özellikleri vardır. Bunlar [Azure kuyrukları ve Azure hizmet veri yolu kuyrukları karşılaştırıldığında ve Contrasted](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted).

## <a name="net-code-example"></a>.NET kodu örneği

Bu bölümdeki kod örneği, şunları yapar:

1. Tanımlar **EncodingJobMessage** bildirim iletisi biçimine eşleyen sınıfı. Kod nesnelerin kuyruktan alınan iletiler seri durumdan çıkarır **EncodingJobMessage** türü.
2. Medya Hizmetleri ve depolama hesabı bilgileri app.config dosyasından yükler. Kod örneği oluşturmak için bu bilgileri kullanır. **CloudMediaContext** ve **CloudQueue** nesneleri.
3. Kodlama işinin hakkında bildirim iletileri alan bir sıra oluşturur.
4. Kuyruğa eşlenen bildirim uç noktası oluşturur.
5. Bildirim uç noktası işe ekler ve kodlama işi gönderir. Birden çok bildirim uç noktaları bir işe bağlı olabilir.
6. Geçişleri **NotificationJobState.FinalStatesOnly** için **AddNew** yöntemi. (Bu örnekte, yalnızca son iş işlemleri durumlarda ilginizi duyuyoruz.)

        job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, _notificationEndPoint);
7. Geçirirseniz **NotificationJobState.All**, aşağıdaki durum değişikliği bildirimleri Al: kuyruğa alınan, zamanlanan, işleme ve tamamlandı. Ancak, daha önce belirtildiği gibi kuyruk depolama sıralı teslim garanti etmez. İletileri sipariş kullanılacağı **zaman damgası** özelliği (tanımlanan **EncodingJobMessage** aşağıdaki örnekte türü). Yinelenen iletileri mümkündür. Yineleme denetimi yapılacak kullanın **ETag özelliği** (tanımlanan **EncodingJobMessage** türü). Bazı durum değişikliği bildirimleri atlandı da mümkündür.
8. İşi kuyruğa her 10 saniyede kontrol ederek tamamlanmış duruma almak bekler. Bunlar işlendikten sonra iletileri siler.
9. Kuyruk ve bildirim bitiş noktasını siler.

> [!NOTE]
> Aşağıdaki örnekte gösterildiği gibi bir işin durumunu izlemek için önerilen yöntem, bildirim iletileri dinleyerek verilmiştir:
>
> Alternatif olarak, kullanarak bir işin durumuna iade edilemedi **IJob.State** özelliği.  Bir işin tamamlanması hakkında bir bildirim iletisi üzerinde duruma gelebilir **IJob** ayarlanır **tamamlandı**. **IJob.State** özelliği kısa bir gecikme ile doğru durumunu yansıtır.
>
>

### <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

1. Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 
2. Yeni bir klasör oluşturun (klasör herhangi bir yerde olabilir yerel sürücünüzde) ve kodlama ve akışını sağlamak veya aşamalı indirmek istediğiniz bir .mp4 dosyasını buraya kopyalayın. Bu örnekte "C:\Media" yol kullanılır.
3. Bir başvuru ekleyin **System.Runtime.Serialization** kitaplığı.

### <a name="code"></a>Kod

```csharp
using System;
using System.Linq;
using System.Configuration;
using System.IO;
using System.Threading;
using System.Collections.Generic;
using Microsoft.WindowsAzure.MediaServices.Client;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Queue;
using System.Runtime.Serialization.Json;

namespace JobNotification
{
    public class EncodingJobMessage
    {
        // MessageVersion is used for version control.
        public String MessageVersion { get; set; }

        // Type of the event. Valid values are
        // JobStateChange and NotificationEndpointRegistration.
        public String EventType { get; set; }

        // ETag is used to help the customer detect if
        // the message is a duplicate of another message previously sent.
        public String ETag { get; set; }

        // Time of occurrence of the event.
        public String TimeStamp { get; set; }

        // Collection of values specific to the event.

        // For the JobStateChange event the values are:
        //     JobId - Id of the Job that triggered the notification.
        //     NewState- The new state of the Job. Valid values are:
        //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
        //     OldState- The old state of the Job. Valid values are:
        //          Scheduled, Processing, Canceling, Cancelled, Error, Finished

        // For the NotificationEndpointRegistration event the values are:
        //     NotificationEndpointId- Id of the NotificationEndpoint
        //          that triggered the notification.
        //     State- The state of the Endpoint.
        //          Valid values are: Registered and Unregistered.

        public IDictionary<string, object> Properties { get; set; }
    }

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

        private static readonly string _StorageConnectionString = 
            ConfigurationManager.AppSettings["StorageConnectionString"];

        private static CloudMediaContext _context = null;
        private static CloudQueue _queue = null;
        private static INotificationEndPoint _notificationEndPoint = null;

        private static readonly string _singleInputMp4Path =
            Path.GetFullPath(@"C:\Media\BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            string endPointAddress = Guid.NewGuid().ToString();

            // Create the context.
            AzureAdTokenCredentials tokenCredentials = 
                new AzureAdTokenCredentials(_AADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
            
            // Create the queue that will be receiving the notification messages.
            _queue = CreateQueue(_StorageConnectionString, endPointAddress);

            // Create the notification point that is mapped to the queue.
            _notificationEndPoint =
                    _context.NotificationEndPoints.Create(
                    Guid.NewGuid().ToString(), NotificationEndPointType.AzureQueue, endPointAddress);


            if (_notificationEndPoint != null)
            {
                IJob job = SubmitEncodingJobWithNotificationEndPoint(_singleInputMp4Path);
                WaitForJobToReachedFinishedState(job.Id);
            }

            // Clean up.
            _queue.Delete();
            _notificationEndPoint.Delete();
        }


        static public CloudQueue CreateQueue(string storageAccountConnectionString, string endPointAddress)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageAccountConnectionString);

            // Create the queue client
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

            // Retrieve a reference to a queue
            CloudQueue queue = queueClient.GetQueueReference(endPointAddress);

            // Create the queue if it doesn't already exist
            queue.CreateIfNotExists();

            return queue;
        }


        public static IJob SubmitEncodingJobWithNotificationEndPoint(string inputMediaFilePath)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("My MP4 to Smooth Streaming encoding job");

            //Create an encrypted asset and upload the mp4.
            IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.StorageEncrypted,
                inputMediaFilePath);

            // Get a media processor reference, and pass to it the name of the
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task with the conversion details, using a configuration file.
            ITask task = job.Tasks.AddNew("My encoding Task",
                processor,
                "Adaptive Streaming",
                Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.None);

            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);

            // Add a notification point to the job. You can add multiple notification points.  
            job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly,
                _notificationEndPoint);

            job.Submit();

            return job;
        }

        public static void WaitForJobToReachedFinishedState(string jobId)
        {
            int expectedState = (int)JobState.Finished;
            int timeOutInSeconds = 600;

            bool jobReachedExpectedState = false;
            DateTime startTime = DateTime.Now;
            int jobState = -1;

            while (!jobReachedExpectedState)
            {
                // Specify how often you want to get messages from the queue.
                Thread.Sleep(TimeSpan.FromSeconds(10));

                foreach (var message in _queue.GetMessages(10))
                {
                    using (Stream stream = new MemoryStream(message.AsBytes))
                    {
                        DataContractJsonSerializerSettings settings = new DataContractJsonSerializerSettings();
                        settings.UseSimpleDictionaryFormat = true;
                        DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(EncodingJobMessage), settings);
                        EncodingJobMessage encodingJobMsg = (EncodingJobMessage)ser.ReadObject(stream);

                        Console.WriteLine();

                        // Display the message information.
                        Console.WriteLine("EventType: {0}", encodingJobMsg.EventType);
                        Console.WriteLine("MessageVersion: {0}", encodingJobMsg.MessageVersion);
                        Console.WriteLine("ETag: {0}", encodingJobMsg.ETag);
                        Console.WriteLine("TimeStamp: {0}", encodingJobMsg.TimeStamp);
                        foreach (var property in encodingJobMsg.Properties)
                        {
                            Console.WriteLine("    {0}: {1}", property.Key, property.Value);
                        }

                        // We are only interested in messages
                        // where EventType is "JobStateChange".
                        if (encodingJobMsg.EventType == "JobStateChange")
                        {
                            string JobId = (String)encodingJobMsg.Properties.Where(j => j.Key == "JobId").FirstOrDefault().Value;
                            if (JobId == jobId)
                            {
                                string oldJobStateStr = (String)encodingJobMsg.Properties.
                                                            Where(j => j.Key == "OldState").FirstOrDefault().Value;
                                string newJobStateStr = (String)encodingJobMsg.Properties.
                                                            Where(j => j.Key == "NewState").FirstOrDefault().Value;

                                JobState oldJobState = (JobState)Enum.Parse(typeof(JobState), oldJobStateStr);
                                JobState newJobState = (JobState)Enum.Parse(typeof(JobState), newJobStateStr);

                                if (newJobState == (JobState)expectedState)
                                {
                                    Console.WriteLine("job with Id: {0} reached expected state: {1}",
                                        jobId, newJobState);
                                    jobReachedExpectedState = true;
                                    break;
                                }
                            }
                        }
                    }
                    // Delete the message after we've read it.
                    _queue.DeleteMessage(message);
                }

                // Wait until timeout
                TimeSpan timeDiff = DateTime.Now - startTime;
                bool timedOut = (timeDiff.TotalSeconds > timeOutInSeconds);
                if (timedOut)
                {
                    Console.WriteLine(@"Timeout for checking job notification messages,
                                        latest found state ='{0}', wait time = {1} secs",
                        jobState,
                        timeDiff.TotalSeconds);

                    throw new TimeoutException();
                }
            }
        }

        static private IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            var asset = _context.Assets.Create("UploadSingleFile_" + DateTime.UtcNow.ToString(),
                assetCreationOptions);

            var fileName = Path.GetFileName(singleFilePath);

            var assetFile = asset.AssetFiles.Create(fileName);

            Console.WriteLine("Created assetFile {0}", assetFile.Name);
            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading of {0}", assetFile.Name);

            return asset;
        }

        static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
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

Yukarıdaki örnek aşağıdaki çıktıyı üretilen: Değerlerinize göre değişir.

    Created assetFile BigBuckBunny.mp4
    Upload BigBuckBunny.mp4
    Done uploading of BigBuckBunny.mp4

    EventType: NotificationEndPointRegistration
    MessageVersion: 1.0
    ETag: e0238957a9b25bdf3351a88e57978d6a81a84527fad03bc23861dbe28ab293f6
    TimeStamp: 2013-05-14T20:22:37
        NotificationEndPointId: nb:nepid:UUID:d6af9412-2488-45b2-ba1f-6e0ade6dbc27
        State: Registered
        Name: dde957b2-006e-41f2-9869-a978870ac620
        Created: 2013-05-14T20:22:35

    EventType: JobStateChange
    MessageVersion: 1.0
    ETag: 4e381f37c2d844bde06ace650310284d6928b1e50101d82d1b56220cfcb6076c
    TimeStamp: 2013-05-14T20:24:40
        JobId: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54
        JobName: My MP4 to Smooth Streaming encoding job
        NewState: Finished
        OldState: Processing
        AccountName: westeuropewamsaccount
    job with Id: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54 reached expected
    State: Finished


## <a name="next-step"></a>Sonraki adım
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
