---
title: Azure içerik aracı - Orta videolar ve .NET dökümleri | Microsoft Docs
description: İçerik denetleyici videolar ve .NET dökümleri Orta için nasıl kullanılacağını.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 1/27/2018
ms.author: sajagtap
ms.openlocfilehash: a084b50e44fe26ba2547d0f7b7ed184fb71b190c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "35352523"
---
# <a name="video-and-transcript-moderation-tutorial"></a>Video ve dökümü yönetimini Öğreticisi

İçerik denetleyicinin video API'ler videolar Orta ve İnsan gözden geçirme aracında video incelemeler oluşturmanıza izin verir. 

Bu öğretici yardımcı makine destekli denetleme ve İnsan-içinde--döngü gözden geçirme oluşturma ile tam bir videoyu ve dökümü yönetimi çözümü oluşturmak nasıl anlamak için ayrıntılı.

Karşıdan [C# konsol uygulaması](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) Bu öğretici için. Konsol uygulaması, aşağıdaki görevleri gerçekleştirmek için SDK ve ilişkili paketleri kullanır:

- Daha hızlı işleme için giriş video Sıkıştır
- Görüntüleri ve çerçeveleri ınsights ile almak için video Orta
- Çerçeve zaman damgaları küçük resimleri (görüntüler) oluşturmak için kullanın
- Zaman damgaları ve video incelemeler oluşturmak için küçük resimleri gönderme
- Metin (dökümü) medya dizin oluşturucu API ile video konuşma Dönüştür
- Metin denetleme hizmetiyle dökümü Orta
- Video incelemeye aracılı dökümü Ekle

## <a name="sample-program-outputs"></a>Örnek program çıkarır

Ayrıca, geçmeden önce aşağıdaki örnek çıkış programda bakalım:

- [Konsol çıkışı](#program-output)
- [Video gözden geçirme](#video-review-default-view)
- [Dökümü görünümü](#video-review-transcript-view)

## <a name="prerequisites"></a>Önkoşullar

1. Kaydolun [içerik denetleyici gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) web sitesi ve [özel etiketler oluşturma](Review-Tool-User-Guide/tags.md) , C# konsol uygulaması gelen içindeki kod atar. Aşağıdaki ekran özel etiketler gösterir.

  ![Video yönetimini özel etiketler](images/video-tutorial-custom-tags.png)

1. Örnek uygulamayı çalıştırmak için bir Azure hesabı ve Azure Media Services hesabı gerekir. Ayrıca, içerik denetleyicinin özel Önizleme erişmeniz gerekir. Son olarak, Azure Active Directory kimlik doğrulama kimlik bilgileri gerekir. Bu bilgiler edinme hakkında ayrıntılar için bkz: [Video yönetimini API Hızlı Başlangıç](video-moderation-api.md).

1. Dosyayı düzenlemek `App.config` ve hizmet uç noktaları, Active Directory Kiracı adı ekleyin ve Abonelik anahtarları belirtilen tarafından `#####`. Aşağıdaki bilgiler gereklidir:

|Anahtar|Açıklama|
|-|-|
|`AzureMediaServiceRestApiEndpoint`|Azure Media Services (AMS) API uç noktası|
|`ClientSecret`|Azure Media Services için abonelik anahtarı|
|`ClientId`|Azure Media Services istemci kimliği|
|`AzureAdTenantName`|Kuruluşunuz temsil eden active Directory Kiracı adı|
|`ContentModeratorReviewApiSubscriptionKey`|İçerik denetleyici için abonelik anahtarınızı API gözden geçirin|
|`ContentModeratorApiEndpoint`|Uç noktası için içerik yönetici API'si|
|`ContentModeratorTeamId`|İçerik denetleyici takım kimliği|

## <a name="getting-started"></a>Başlarken

Sınıf `Program` içinde `Program.cs` video yönetimini uygulama için temel giriş noktasıdır.

### <a name="methods-of-class-program"></a>Program sınıfının yöntemleri

|Yöntem|Açıklama|
|-|-|
|`Main`|Komut satırı ayrıştırır, kullanıcı girişi toplar ve işlemeye başlıyor.|
|`ProcessVideo`|Sıkıştırır, karşıya yükleme, moderates ve video incelemeler oluşturur.|
|`CreateVideoStreamingRequest`|Bir video karşıya yüklemek için bir akış oluşturur|
|`GetUserInputs`|Kullanıcı girişi toplar; hiçbir komut satırı seçenekleri olmadığında kullanılır|
|`Initialize`|Denetleme işlemi için gereken nesneleri başlatır|

### <a name="the-main-method"></a>Main yöntemi

`Main()` video denetleme işlemini anlama başlamak için yer olan yürütme başladığı, olduğundan.

    static void Main(string[] args)
    {
        if (args.Length == 0)
        {
            string videoPath = string.Empty;
            GetUserInputs(out videoPath);
            Initialize();
            AmsConfigurations.logFilePath = Path.Combine(Path.GetDirectoryName(videoPath), "log.txt");
            try
            {
                ProcessVideo(videoPath).Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
        else
        {
            DirectoryInfo directoryInfo = new DirectoryInfo(args[0]);
            if (args.Length == 2)
                bool.TryParse(args[1], out generateVtt);
            Initialize();
            AmsConfigurations.logFilePath = Path.Combine(args[0], "log.txt");
            var files = directoryInfo.GetFiles("*.mp4", SearchOption.AllDirectories);
            foreach (var file in files)
            {
                try
                {
                    ProcessVideo(file.FullName).Wait();
                }
                catch (Exception ex)
                {
                    Console.WriteLine(ex.Message);
                }
            }
        }
    }

`Main()` Aşağıdaki komut satırı bağımsız değişkenleri işleme:

- Denetleme için gönderilecek MPEG-4 video dosyaları içeren dizini yolu. Tüm `*.mp4` bu dizin ve alt dizinlerinde dosyaları denetleme için gönderilir.
- Metin dökümleri amacıyla aracılık ses oluşturulan olup olmadığını belirten isteğe bağlı olarak, bir Boole (true/false) bayrak.

Komut satırı bağımsız değişken yoksa `Main()` çağrıları `GetUserInputs()`. Bu yöntem tek bir görüntü dosyasının yolunu girin ve bir metin dökümü oluşturulan olup olmadığını belirtmek için kullanıcıya sorar.

> [!NOTE]
> Konsol uygulaması kullanan [Azure Media Indexer API](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) karşıya yüklenen videonun ses parçasından dökümleri oluşturulacak. Sonuçları WebVTT biçiminde sağlanır. Bu biçimi hakkında daha fazla bilgi için bkz: [Web Video metin parçaları biçimi](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API).

### <a name="initialize-and-processvideo-methods"></a>Başlatma ve ProcessVideo yöntemleri

Komut satırından veya etkileşimli kullanıcı girişi, programın seçenekleri olup gelen bağımsız olarak `Main()` sonraki çağrılar `Initialize()` şu örnekleri oluşturmak için:

|Sınıf|Açıklama|
|-|-|
|`AMSComponent`|Video dosyaları için denetleme göndermeden önce sıkıştırır.|
|`AMSconfigurations`|Bulunan uygulamanın yapılandırma verileri için arabirim `App.config`.|
|`VideoModerator`| Karşıya yükleme, kodlama, şifreleme ve AMS SDK'sını kullanarak denetleme|
|`VideoReviewApi`|Video incelemeler içerik denetleyicinin hizmet yöneten|

Bu sınıfları (aside gelen `AMSConfigurations`, basit olduğu) Bu öğreticinin yaklaşan bölümlerde daha ayrıntılı olarak ele alınmaktadır.

Son olarak, video işlenen birer birer çağırarak dosyalardır `ProcessVideo()` her.

    private static async Task ProcessVideo(string videoPath)
    {
        Stopwatch sw = new Stopwatch();
        sw.Start();
        Console.ForegroundColor = ConsoleColor.White;
        Console.WriteLine("\nVideo compression process started...");

        var compressedVideoPath = amsComponent.CompressVideo(videoPath);
        if (string.IsNullOrWhiteSpace(compressedVideoPath))
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine("Video Compression failed.");
        }

        Console.WriteLine("\nVideo compression process completed...");

        UploadVideoStreamRequest uploadVideoStreamRequest = CreateVideoStreamingRequest(compressedVideoPath);
        UploadAssetResult uploadResult = new UploadAssetResult();

        if (generateVtt)
        {
            uploadResult.GenerateVTT = generateVtt;
        }
        Console.WriteLine("\nVideo moderation process started...");

        if (!videoModerator.CreateAzureMediaServicesJobToModerateVideo(uploadVideoStreamRequest, uploadResult))
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine("Video Review process failed.");
        }

        Console.WriteLine("\nVideo moderation process completed...");
        Console.WriteLine("\nVideo review process started...");
        string reviewId = await videoReviewApi.CreateVideoReviewInContentModerator(uploadResult);
        Console.WriteLine("\nVideo review successfully completed...");
        sw.Stop();
        Console.WriteLine("\nTotal Elapsed Time: {0}", sw.Elapsed);
        using (var stw = new StreamWriter(AmsConfigurations.logFilePath, true))
        {
            stw.WriteLine("Video File Name: " + Path.GetFileName(videoPath));
            stw.WriteLine($"ReviewId: {reviewId}");
            stw.WriteLine("Total Elapsed Time: {0}", sw.Elapsed);
        }
    }


`ProcessVideo()` Yöntemdir oldukça basit. Bu, sırayla aşağıdaki işlemleri gerçekleştirir:

- Video sıkıştırır
- Bir Azure Media Services varlığı video yükler
- Video Orta düzeye bir AMS işi oluşturur
- Video İnceleme içerik Aracı alanında oluşturur

Aşağıdaki bölümlerde daha ayrıntılı olarak bazı tarafından çağrılan tek tek işlemleri göz önünde bulundurun `ProcessVideo()`. 

## <a name="compressing-the-video"></a>Video sıkıştırma

Ağ trafiğini en aza indirmek için uygulama video dosyaları H.264 (MPEG-4 AVC) biçimine dönüştürür ve maksimum 640 piksel genişliğine ölçeklendirir. H.264 codec yüksek verimlilik (sıkıştırma oranı) nedeniyle önerilir. Sıkıştırma yapılır ücretsiz kullanarak `ffmpeg` dahil komut satırı aracı `Lib` Visual Studio çözümü klasörü. Giriş dosyaları tarafından desteklenen herhangi bir biçimdeki olabilir `ffmpeg`en yaygın kullanılan video dosyası biçimlerini ve codec bileşenleri dahil olmak üzere.

> [!NOTE]
> Komut satırı seçeneklerini kullanarak programı başlattığınızda denetleme için gönderilecek video dosyaları içeren dizini belirtin. Bu dizin sahip tüm dosyaları `.mp4` dosya adı uzantısı işlenir. Diğer dosya adı uzantılarını işlemek için güncelleştirme `Main()` yönteminde `Program.cs` istenen uzantıları dahil.

Tek bir video dosyası sıkıştırır kodu `AmsComponent` sınıfını `AMSComponent.cs`. Bu işlevsellik için sorumlu yöntemdir `CompressVideo()`, burada gösterilen.

    public string CompressVideo(string videoPath)
    {
        string ffmpegBlobUrl;
        if (!ValidatePreRequisites())
        {
            Console.WriteLine("Configurations check failed. Please cross check the configurations!");
            throw new Exception();
        }

        if (File.Exists(_configObj.FfmpegExecutablePath))
        {
            ffmpegBlobUrl = this._configObj.FfmpegExecutablePath;
        }
        else
        {
            Console.WriteLine("ffmpeg.exe is missing. Please check the Lib folder");
            throw new Exception();
        }

        string videoFilePathCom = videoPath.Split('.')[0] + "_c.mp4";
        ProcessStartInfo processStartInfo = new ProcessStartInfo();
        processStartInfo.WindowStyle = ProcessWindowStyle.Hidden;
        processStartInfo.FileName = ffmpegBlobUrl;
        processStartInfo.Arguments = "-i \"" + videoPath + "\" -vcodec libx264 -n -crf 32 -preset veryfast -vf scale=640:-1 -c:a aac -aq 1 -ac 2 -threads 0 \"" + videoFilePathCom + "\"";
        var process = Process.Start(processStartInfo);
        process.WaitForExit();
        process.Close();
        return videoFilePathCom;
    }

Kod, aşağıdaki adımları gerçekleştirir:

- Yapılandırmada emin olmak için denetimleri `App.config` tüm gerekli verileri içerir
- Emin olmak için denetimleri `ffmpeg` ikili yok
- Çıktı dosyası adını ekleyerek derlemeler `_c.mp4` dosyasının temel adı için (gibi `Example.mp4`  ->  `E>xample_c.mp4`)
- Dönüştürme gerçekleştirmek için bir komut satırı dizesi oluşturur
- Başlayan bir `ffmpeg` komut satırını kullanarak işlem
- Video işlenecek bekler

> [!NOTE]
> Videolarınızı H.264 kullanılarak önceden sıkıştırılan ve uygun boyutlarda biliyorsanız, yazabilirsiniz `CompressVideo()` sıkıştırma atlanacak.

Bu yöntem sıkıştırılmış çıktı dosyası adını döndürür.

## <a name="uploading-and-moderating-the-video"></a>Karşıya yükleme ve video yönetme

İçerik Yönetimi hizmeti tarafından işlenen önce video Azure Media Services depolanması gerekir. `Program` Sınıfını `Program.cs` kısa bir yönteme sahip `CreateVideoStreamingRequest()` video karşıya yüklemek için kullanılan akış isteği temsil eden bir nesne döndürür.

    private static UploadVideoStreamRequest CreateVideoStreamingRequest(string compressedVideoFilePath)
    {
        return
            new UploadVideoStreamRequest
            {
                VideoStream = File.ReadAllBytes(compressedVideoFilePath),
                VideoName = Path.GetFileName(compressedVideoFilePath),
                EncodingRequest = new EncodingRequest()
                {
                    EncodingBitrate = AmsEncoding.AdaptiveStreaming
                },
                VideoFilePath = compressedVideoFilePath
            };
    }

Elde edilen `UploadVideoStreamRequest` nesne tanımlanmış `UploadVideoStreamRequest.cs` (ve kendi üst `UploadVideoRequest`, `UploadVideoRequest.cs`). Bu sınıfların burada gösterilen değil; Bunlar, kısa ve yalnızca ilgili bilgileri ve sıkıştırılmış video verileri tutmak için hizmet. Başka bir yalnızca veri sınıfı `UploadAssetResult` (`UploadAssetResult.cs`) karşıya yükleme işleminin sonuçlarını tutmak için kullanılır. Bu satırlar anlamak olası şimdi `ProcessVideo()`:

    UploadVideoStreamRequest uploadVideoStreamRequest = CreateVideoStreamingRequest(compressedVideoPath);
    UploadAssetResult uploadResult = new UploadAssetResult();

    if (generateVtt)
    {
        uploadResult.GenerateVTT = generateVtt;
    }
    Console.WriteLine("\nVideo moderation process started...");

    if (!videoModerator.CreateAzureMediaServicesJobToModerateVideo(uploadVideoStreamRequest, uploadResult))
    {
        Console.ForegroundColor = ConsoleColor.Red;
        Console.WriteLine("Video Review process failed.");
    }

Bu satırlar aşağıdaki görevleri gerçekleştirin:

- Oluşturma bir `UploadVideoStreamRequest` sıkıştırılmış video karşıya yüklemek için
- İsteğin ayarlamak `GenerateVTT` kullanıcı metin dökümü istediyseniz bayrak
- Çağrıları `CreateAzureMediaServicesJobToModerateVideo()` karşıya yükleme gerçekleştirmek ve sonucu almak için

## <a name="deep-dive-into-video-moderation"></a>Video yönetimini içine derinlemesine bakış

Yöntem `CreateAzureMediaServicesJobToModerateVideo()` yer `VideoModerator.cs`, Azure Media Services ile etkileşime giren kodu toplu içerir. Yöntemin kaynak kodu aşağıdaki Ayıkla gösterilir.

    public bool CreateAzureMediaServicesJobToModerateVideo(UploadVideoStreamRequest uploadVideoRequest, UploadAssetResult uploadResult)
    {
        asset = CreateAsset(uploadVideoRequest);
        uploadResult.VideoName = uploadVideoRequest.VideoName;
        // Encoding the asset , Moderating the asset, Generating transcript in parallel
        IAsset encodedAsset = null;
        //Creates the job for the tasks.
        IJob job = this._mediaContext.Jobs.Create("AMS Review Job");

        //Adding encoding task to job.
        ConfigureEncodeAssetTask(uploadVideoRequest.EncodingRequest, job);

        ConfigureContentModerationTask(job);

        //adding transcript task to job.
        if (uploadResult.GenerateVTT)
        {
            ConfigureTranscriptTask(job);
        }

        Stopwatch timer = new Stopwatch();
        timer.Start();
        //submit and execute job.
        job.Submit();
        job.GetExecutionProgressTask(new CancellationTokenSource().Token).Wait();
        timer.Stop();
        using (var sw = new StreamWriter(AmsConfigurations.logFilePath, true))
        {
            sw.WriteLine("AMS Job Elapsed Time: {0}", timer.Elapsed);
        }

        if (job.State == JobState.Error)
        {
            throw new Exception("Video moderation has failed due to AMS Job error.");
        }

        UploadAssetResult result = uploadResult;
        encodedAsset = job.OutputMediaAssets[0];
        result.ModeratedJson = GetCmDetail(job.OutputMediaAssets[1]);
        // Check for valid Moderated JSON
        var jsonModerateObject = JsonConvert.DeserializeObject<VideoModerationResult>(result.ModeratedJson);

        if (jsonModerateObject == null)
        {
            return false;
        }
        if (uploadResult.GenerateVTT)
        {
            GenerateTranscript(job.OutputMediaAssets.Last());
        }

        uploadResult.StreamingUrlDetails = PublishAsset(encodedAsset);
        string downloadUrl = GenerateDownloadUrl(asset, uploadVideoRequest.VideoName);
        uploadResult.StreamingUrlDetails.DownloadUri = downloadUrl;
        uploadResult.VideoName = uploadVideoRequest.VideoName;
        uploadResult.VideoFilePath = uploadVideoRequest.VideoFilePath;
        return true;
    }

Bu kod, aşağıdaki görevleri gerçekleştirir:

- Yapılacak işlem için bir AMS işi oluşturur
- Video dosyası kodlama, onu yönetme ve metin dökümü oluşturmak için görevleri ekler
- Başlangıç işlem ve dosya karşıya yükleme işi gönderir
- Denetleme sonuçları, metin dökümü (isteniyorsa) ve diğer bilgileri alır.

## <a name="sample-video-moderation-response"></a>Örnek video yönetimini yanıt

Video denetleme işi sonucu (bkz [video yönetimini quickstart](video-moderation-api.md) olduğunu denetleme sonuçlarını içeren bir JSON veri yapısı. Bu sonuçları (görüntüleri) parçaları dökümünü (klipleri) içeren her olayları gözden geçirilmek üzere işaretlenmiş anahtar çerçeveler ile video içinde içerir. Her anahtar çerçevesi tarafından olasılığını Yetişkin veya saldırganlardan içerik içerdiği puanlanır. Aşağıdaki örnek, bir JSON yanıtı gösterir:

    {
        "version": 2,
        "timescale": 90000,
        "offset": 0,
        "framerate": 50,
        "width": 1280,
        "height": 720,
        "totalDuration": 18696321,
        "fragments": [
        {
            "start": 0,
            "duration": 18000
        },
        {
            "start": 18000,
            "duration": 3600,
            "interval": 3600,
            "events": [
            [
            {
                "reviewRecommended": false,
                "adultScore": 0.00001,
                "racyScore": 0.03077,
                "index": 5,
                "timestamp": 18000,
                "shotIndex": 0
            }
            ]
        ]
        },
        {
            "start": 18386372,
            "duration": 119149,
            "interval": 119149,
            "events": [
            [
            {
                "reviewRecommended": true,
                "adultScore": 0.00000,
                "racyScore": 0.91902,
                "index": 5085,
                "timestamp": 18386372,
                "shotIndex": 62
            }
        ]
        ]
        }
    ]
    }

Transcription, video sesi de olduğu zaman üretilen `GenerateVTT` bayrağı ayarlanır.

> [!NOTE]
> Konsol uygulaması kullanan [Azure Media Indexer API](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) gelen dökümleri oluşturmak için karşıya yüklenen video ses izle. Sonuçları WebVTT biçiminde sağlanır. Bu biçimi hakkında daha fazla bilgi için bkz: [Web Video metin parçaları biçimi](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API).


## <a name="creating-the-human-in-the-loop-review"></a>İnsan-içinde--döngü gözden geçirme oluşturma

Denetleme işlemi video, ses izleri dökümü birlikte anahtar çerçeveler listesini döndürür. Sonraki adım, İnsan denetleyiciler için içerik denetleyici gözden geçirme aracında bir gözden geçirme oluşturmaktır. Geri gidip `ProcessVideo()` yönteminde `Program.cs`, çağrısına bakın `CreateVideoReviewInContentModerator()` yöntemi. Bu yöntem `videoReviewApi` bulunduğu sınıfı `VideoReviewAPI.cs`ve burada gösterilir.

    public async Task<string> CreateVideoReviewInContentModerator(UploadAssetResult uploadAssetResult)
    {
    
        string reviewId = string.Empty;
        List<ProcessedFrameDetails> frameEntityList = framegenerator.CreateVideoFrames(uploadAssetResult);
        string path = uploadAssetResult.GenerateVTT == true ? this._amsConfig.FfmpegFramesOutputPath + Path.GetFileNameWithoutExtension (uploadAssetResult.VideoName) + "_aud_SpReco.vtt" : "";
        TranscriptScreenTextResult screenTextResult = new TranscriptScreenTextResult();
        
    if (File.Exists(path))
        {
            screenTextResult = await GenerateTextScreenProfanity(reviewId, path, frameEntityList);
            uploadAssetResult.Category1TextScore = screenTextResult.Category1Score;
            uploadAssetResult.Category2TextScore = screenTextResult.Category2Score;
            uploadAssetResult.Category3TextScore = screenTextResult.Category3Score;
            uploadAssetResult.Category1TextTag = screenTextResult.Category1Tag;
            uploadAssetResult.Category2TextTag = screenTextResult.Category2Tag;
            uploadAssetResult.Category3TextTag = screenTextResult.Category3Tag;
        }
        
        var reviewVideoRequestJson = CreateReviewRequestObject(uploadAssetResult, frameEntityList);
        if (string.IsNullOrWhiteSpace(reviewVideoRequestJson))
        {
            throw new Exception("Video review process failed in CreateVideoReviewInContentModerator");
        }
        
        reviewId = JsonConvert.DeserializeObject<List<string>>(ExecuteCreateReviewApi(reviewVideoRequestJson).Result).FirstOrDefault();
        frameEntityList = framegenerator.GenerateFrameImages(frameEntityList, uploadAssetResult, reviewId);
        await CreateAndPublishReviewInContentModerator(uploadAssetResult, frameEntityList, reviewId, path, screenTextResult);
        return reviewId;
    
    }

`CreateVideoReviewInContentModerator()` Aşağıdaki görevleri gerçekleştirmek için çeşitli yöntemler çağırır:

> [!NOTE]
> Konsol uygulaması kullanan [FFmpeg](https://ffmpeg.org/) küçük resimler oluşturma kitaplığı. Bu küçük resimleri (görüntüler) çerçeve veritabanındaki tarih damgası karşılık [video yönetimini çıktı](#sample-video-moderation-response).

|Görev|Yöntemler|Dosya|
|-|-|-|
|Anahtar video çerçeveler ve bunları küçük resimlerini oluşturur extract|`CreateVideoFrames()`<br>`GenerateFrameImages()`|`FrameGeneratorServices.cs`|
|Metin dökümü varsa Yetişkin veya saldırganlardan ses bulmak tarama|`GenerateTextScreenProfanity()`| `VideoReviewAPI.cs`|
|Hazırlama ve İnsan İnceleme için video gözden geçirme isteği gönderir|`CreateReviewRequestObject()`<br> `ExecuteCreateReviewApi()`<br>`CreateAndPublishReviewInContentModerator()`|`VideoReviewAPI.cs`|

## <a name="video-review-default-view"></a>Video gözden geçirme varsayılan görünüm

Aşağıdaki ekran önceki adımların sonuçlarını gösterir.

![Video gözden geçirme varsayılan görünüm](images/video-tutorial-default-view.PNG)

## <a name="transcript-generation"></a>Dökümü oluşturma

Şimdiye kadar bu öğreticide sunulan kodu visual içerik odaklı. Konuşma içeriği gözden geçir belirtildiği gibi gelen oluşturulan dökümü kullanır, ayrı ve isteğe bağlı bir işlemdir. Metin dökümleri nasıl oluşturulduğunu ve gözden geçirme işleminde kullanılan bir göz atalım için şimdi zaman yapılır. Dökümü oluşturma görevini döner [Azure Media Indexer](https://docs.microsoft.com/azure/media-services/media-services-index-content) hizmet.

Uygulama, aşağıdaki görevleri gerçekleştirir:

|Görev|Yöntemler|Dosya|
|-|-|-|
|Metin dökümleri oluşturulacak olup olmadığını belirleme|`Main()`<br>`GetUserInputs()`|`Program.cs`|
|Bu durumda, transcription iş yönetimini bir parçası olarak gönder|`ConfigureTranscriptTask()`|`VideoModerator.cs`|
|Dökümü yerel bir kopyasını alma|`GenerateTranscript()`|`VideoModerator.cs`|
|Uygunsuz ses içeren çerçeveleri video bayrak|`GenerateTextScreenProfanity()`<br>`TextScreen()`|`VideoReviewAPI.cs`|
|Sonuçlar incelemeye ekleyin|`UploadScreenTextResult()`<br>`ExecuteAddTranscriptSupportFile()`|`VideoReviewAPI.cs`|

### <a name="task-configuration"></a>Görev yapılandırması

Şimdi sağ transcription işi gönderme uygulamasına geçin. `CreateAzureMediaServicesJobToModerateVideo()` (yukarıda açıklanan) çağrıları `ConfigureTranscriptTask()`.

    private void ConfigureTranscriptTask(IJob job)
    {
        string mediaProcessorName = _amsConfigurations.MediaIndexer2MediaProcessor;
        IMediaProcessor processor = _mediaContext.MediaProcessors.GetLatestMediaProcessorByName(mediaProcessorName);

        string configuration = File.ReadAllText(_amsConfigurations.MediaIndexerConfigurationJson);
        ITask task = job.Tasks.AddNew("AudioIndexing Task", processor, configuration, TaskOptions.None);
        task.InputAssets.Add(asset);
        task.OutputAssets.AddNew("AudioIndexing Output Asset", AssetCreationOptions.None);
    }

Dökümü görev için yapılandırma dosyasından okunur `MediaIndexerConfig.json` çözümün içinde `Lib` klasör. AMS varlıklar transcription işlem çıkışı ve yapılandırma dosyası için oluşturulur. AMS işini çalıştırdığında, bu görev bir metin dökümü video dosyanın ses parçasından oluşturur.

> [!NOTE]
> Örnek uygulamayı yalnızca konuşma ABD İngilizcesi olarak tanır.

### <a name="transcript-generation"></a>Dökümü oluşturma

Dökümü AMS varlık yayımlanır. Uygunsuz içerik dökümü taramak için Azure Media Services'den varlık uygulamayı yükler. `CreateAzureMediaServicesJobToModerateVideo()` çağrıları `GenerateTranscript()`gösterilen dosyasını almak için buraya tıklayın.

    public bool GenerateTranscript(IAsset asset)
    {
        try
        {
            var outputFolder = this._amsConfigurations.FfmpegFramesOutputPath;
            IAsset outputAsset = asset;
            IAccessPolicy policy = null;
            ILocator locator = null;
            policy = _mediaContext.AccessPolicies.Create("My 30 days readonly policy", TimeSpan.FromDays(360), AccessPermissions.Read);
            locator = _mediaContext.Locators.CreateLocator(LocatorType.Sas, outputAsset, policy, DateTime.UtcNow.AddMinutes(-5));
            DownloadAssetToLocal(outputAsset, outputFolder);
            locator.Delete();
            return true;
        }
        catch
        {   //TODO:  Logging
            Console.WriteLine("Exception occured while generating index for video.");
            throw;
        }
    }

Gerekli bazı AMS Kurulumdan sonra çağırarak gerçek indirme gerçekleştirilen `DownloadAssetToLocal()`, yerel bir dosyaya bir AMS varlık kopyalar genel bir işlev.

## <a name="transcript-moderation"></a>Dökümü denetleme

Dökümü ile yakın elinizdeki, taranan olduğundan ve incelemeye kullanılır. Gözden geçirme oluşturma kapsamında olan `CreateVideoReviewInContentModerator()`, o çağrıları `GenerateTextScreenProfanity()` iş yapmak için. Buna karşılık, bu yöntemi çağırır `TextScreen()`, işlevselliğinin büyük kısmını içerir. 

`TextScreen()` Aşağıdaki görevleri gerçekleştirir:

- Zaman dökümü tamps ve başlıkları ayrıştırma
- Metin denetleme için her başlığı gönderme
- Uygunsuz konuşma içeriğe sahip herhangi bir çerçeve bayrak

Her bu görevleri daha ayrıntılı inceleyelim:

### <a name="initialize-the-code"></a>Kod başlatma

İlk olarak, tüm değişkenleri ve koleksiyonları başlatır.

    private async Task<TranscriptScreenTextResult> TextScreen(string filepath, List<ProcessedFrameDetails> frameEntityList)
    {
        List<TranscriptProfanity> profanityList = new List<TranscriptProfanity>();
        string responseContent = string.Empty;
        HttpResponseMessage response;
        bool category1Tag = false;
        bool category2Tag = false;
        bool category3Tag = false;
        double category1Score = 0;
        double category2Score = 0;
        double category3Score = 0;
        List<string> vttLines = File.ReadAllLines(filepath).Where(line => !line.Contains("NOTE Confidence:") && line.Length > 0).ToList();
        StringBuilder sb = new StringBuilder();
        List<CaptionScreentextResult> csrList = new List<CaptionScreentextResult>();
        CaptionScreentextResult captionScreentextResult = new CaptionScreentextResult() { Captions = new List<string>() };

        // Code from the next sections in the tutorial
    

### <a name="parse-the-transcript-for-captions"></a>Resim yazıları için dökümü ayrıştırılamıyor

Ardından, resim yazıları ve zaman damgaları için VTT biçimlendirilmiş dökümü ayrıştırılamıyor. Gözden geçirme Aracı'nı bu resim yazıları video İnceleme ekranı dökümü sekmesinde görüntüler. Zaman damgaları resim yazıları karşılık gelen video çerçeveler ile eşitlemek için kullanılır.

        // Code from the previous section(s) in the tutorial

        //
        // Parse the transcript
        //
        foreach (var line in vttLines.Skip(1))
        {
                if (line.Contains("-->"))
                {
                    if (sb.Length > 0)
                    {
                        captionScreentextResult.Captions.Add(sb.ToString());
                        sb.Clear();
                    }
                    if (captionScreentextResult.Captions.Count > 0)
                    {
                        csrList.Add(captionScreentextResult);
                        captionScreentextResult = new CaptionScreentextResult() { Captions = new List<string>() };
                    }
                    string[] times = line.Split(new string[] { "-->" }, StringSplitOptions.RemoveEmptyEntries);
                    string startTimeString = times[0].Trim();
                    string endTimeString = times[1].Trim();
                    int startTime = (int)TimeSpan.ParseExact(startTimeString, @"hh\:mm\:ss\.fff", CultureInfo.InvariantCulture).TotalMilliseconds;
                    int endTime = (int)TimeSpan.ParseExact(endTimeString, @"hh\:mm\:ss\.fff", CultureInfo.InvariantCulture).TotalMilliseconds;
                    captionScreentextResult.StartTime = startTime;
                    captionScreentextResult.EndTime = endTime;
                }
                else
                {
                    sb.Append(line);
                }
                if (sb.Length + line.Length > 1024)
                {
                    captionScreentextResult.Captions.Add(sb.ToString());
                    sb.Clear();
                }
            }
            if (sb.Length > 0)
            {
                captionScreentextResult.Captions.Add(sb.ToString());
            }
            if (captionScreentextResult.Captions.Count > 0)
            {
                csrList.Add(captionScreentextResult);
            }

            // Code from the following section in the quickstart

### <a name="moderate-captions-with-the-text-moderation-service"></a>Metin denetleme hizmetiyle Orta resim yazıları

Ardından, API içerik denetleyicinin metinle ayrıştırılmış metin başlıklarını tarayın.

> [!NOTE]
> İçerik denetleyici hizmeti anahtarınızı ikinci (RPS) hız sınırı başına bir istek var. Sınırı aşarsanız, SDK 429 hata kodunu içeren bir özel durum oluşturur. 
>
> Ücretsiz katmanı anahtarı bir RPS hızı sınırı vardır.

    //
    // Moderate the captions or cues
    //
    int waitTime = 1000;
    foreach (var csr in csrList)
    {
                bool captionAdultTextTag = false;
                bool captionRacyTextTag = false;
                bool captionOffensiveTextTag = false;
                bool retry = true;

                foreach (var caption in csr.Captions)
                {
                    while (retry)
                    {
                        try
                        {
                            System.Threading.Thread.Sleep(waitTime);
                            var lang = await CMClient.TextModeration.DetectLanguageAsync("text/plain", caption);
                            var oRes = await CMClient.TextModeration.ScreenTextWithHttpMessagesAsync(lang.DetectedLanguageProperty, "text/plain", caption, null, null, null, true);
                            response = oRes.Response;
                            responseContent = await response.Content.ReadAsStringAsync();
                            retry = false;
                        }
                        catch (Exception e)
                        {
                            if (e.Message.Contains("429"))
                            {
                                Console.WriteLine($"Moderation API call failed. Message: {e.Message}");
                                waitTime = (int)(waitTime * 1.5);
                                Console.WriteLine($"wait time: {waitTime}");
                            }
                            else
                            {
                                retry = false;
                                Console.WriteLine($"Moderation API call failed. Message: {e.Message}");
                            }
                        }
                    }
                    var jsonTextScreen = JsonConvert.DeserializeObject<TextScreen>(responseContent);
                    if (jsonTextScreen != null)
                    {
                        TranscriptProfanity transcriptProfanity = new TranscriptProfanity();
                        transcriptProfanity.TimeStamp = "";
                        List<Terms> transcriptTerm = new List<Terms>();
                        if (jsonTextScreen.Terms != null)
                        {
                            foreach (var term in jsonTextScreen.Terms)
                            {
                                var profanityobject = new Terms
                                {
                                    Term = term.Term,
                                    Index = term.Index
                                };
                                transcriptTerm.Add(profanityobject);
                            }
                            transcriptProfanity.Terms = transcriptTerm;
                            profanityList.Add(transcriptProfanity);
                        }
                        if (jsonTextScreen.Classification.Category1.Score > _amsConfig.Category1TextThreshold) captionAdultTextTag = true;
                        if (jsonTextScreen.Classification.Category2.Score > _amsConfig.Category2TextThreshold) captionRacyTextTag = true;
                        if (jsonTextScreen.Classification.Category3.Score > _amsConfig.Category3TextThreshold) captionOffensiveTextTag = true;
                        if (jsonTextScreen.Classification.Category1.Score > _amsConfig.Category1TextThreshold) category1Tag = true;
                        if (jsonTextScreen.Classification.Category2.Score > _amsConfig.Category2TextThreshold) category2Tag = true;
                        if (jsonTextScreen.Classification.Category3.Score > _amsConfig.Category3TextThreshold) category3Tag = true;
                        category1Score = jsonTextScreen.Classification.Category1.Score > category1Score ? jsonTextScreen.Classification.Category1.Score : category1Score;
                        category2Score = jsonTextScreen.Classification.Category2.Score > category2Score ? jsonTextScreen.Classification.Category2.Score : category2Score;
                        category3Score = jsonTextScreen.Classification.Category3.Score > category3Score ? jsonTextScreen.Classification.Category3.Score : category3Score;
                    }
                    foreach (var frame in frameEntityList.Where(x => x.TimeStamp >= csr.StartTime && x.TimeStamp <= csr.EndTime))
                    {
                        frame.IsAdultTextContent = captionAdultTextTag;
                        frame.IsRacyTextContent = captionRacyTextTag;
                        frame.IsOffensiveTextContent = captionOffensiveTextTag;
                    }
                }
            }
            TranscriptScreenTextResult screenTextResult = new TranscriptScreenTextResult()
            {
                TranscriptProfanity = profanityList,
                Category1Tag = category1Tag,
                Category2Tag = category2Tag,
                Category3Tag = category3Tag,
                Category1Score = category1Score,
                Category2Score = category2Score,
                Category3Score = category3Score
            };
            return screenTextResult;
    }

### <a name="breaking-down-the-text-moderation-step"></a>Metin denetleme adım kesiliyor

`TextScreen()` önemli bir yöntem, sağlandığından bölünme, olur.

1. İlk olarak, yöntem satır dökümü dosyasını okur. Boş satırlar ve içeren satırları yoksayar bir `NOTE` güvenirlik puanı. Zaman damgaları ve metin öğeleri ayıklar *ipuçlarını* dosyasında. Bir işaret metin ses parçasından temsil eder ve başlangıç ve bitiş zamanlarını içerir. Bir işaret dize zaman damgası satırıyla ile başlayan `-->`. Bir veya daha fazla satırlık metin tarafından izlenir.

1. Örneklerini `CaptionScreentextResult` (tanımlanan `TranscriptProfanity.cs`) her işaret Ayrıştırılan bilgiyi tutmak için kullanılır.  Ne zaman yeni bir zaman damgası satır algılandığında veya maksimum metin uzunluğu 1024 karakterle ulaşıldığında, yeni bir `CaptionScreentextResult` eklenen `csrList`. 

1. Yöntemi, sonraki her işaret metin denetleme API gönderir. Her ikisi de çağırır `ContentModeratorClient.TextModeration.DetectLanguageAsync()` ve `ContentModeratorClient.TextModeration.ScreenTextWithHttpMessagesAsync()`, içinde tanımlanan `Microsoft.Azure.CognitiveServices.ContentModerator` derleme. Oranı sınırlı olan önlemek için her işaret göndermeden önce bir saniye için yöntemi duraklatır.

1. Sonuçları metin denetleme hizmetinden aldıktan sonra yöntemi sonra bunları güvenirlik eşikleri sağlayıp sağlamadığını görmek için çözümler. Bu değerleri oluşturulmuş `App.config` olarak `OffensiveTextThreshold`, `RacyTextThreshold`, ve `AdultTextThreshold`. Son olarak, uygunsuz şartları da depolanır. İşaret 's zaman aralığı içinde tüm çerçeveleri içeren rahatsız edici, saldırganlardan ve/veya yetişkin metin işaretlenir.

1. `TextScreen()` döndüren bir `TranscriptScreenTextResult` bir bütün olarak video metin denetleme sonucundan içeren örneği. Bu nesne ve tüm uygunsuz terimlerin bir listesi ile birlikte uygunsuz içerik çeşitli türlerde için puanlar bayrakları içerir. Arayan `CreateVideoReviewInContentModerator()`, çağrıları `UploadScreenTextResult()` İnsan gözden geçirenlere kullanılabilir olması için bu bilgileri incelemeye eklemek için.
 
## <a name="video-review-transcript-view"></a>Video gözden geçirme dökümü görünümü

Aşağıdaki ekran oluşturma ve denetleme adımları dökümü sonucunu gösterir.

![Video denetleme dökümü görünümü](images/video-tutorial-transcript-view.PNG)

## <a name="program-output"></a>Program çıktısı

Tamamlandıkça program aşağıdaki komut satırı çıkışı çeşitli görevleri gösterir. Ayrıca, denetleme sonucu (JSON biçiminde) ve konuşma dökümü özgün video dosyaları ile aynı dizinde kullanılabilir.

    Microsoft.ContentModerator.AMSComponentClient
    Enter the fully qualified local path for Uploading the video :
    "Your File Name.MP4"
    Generate Video Transcript? [y/n] : y
    
    Video compression process started...
    Video compression process completed...
    
    Video moderation process started...
    Video moderation process completed...
    
    Video review process started...
    Video Frames Creation inprogress...
    Frames(83) created successfully.
    Review Created Successfully and the review Id 201801va8ec2108d6e043229ba7a9e6373edec5
    Video review successfully completed...
    
    Total Elapsed Time: 00:05:56.8420355


## <a name="next-steps"></a>Sonraki adımlar

[Visual Studio çözümü indirme](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) Bu öğretici için dosyaları ve gerekli kitaplıklar örnek ve tümleştirme üzerinde başlayın.
