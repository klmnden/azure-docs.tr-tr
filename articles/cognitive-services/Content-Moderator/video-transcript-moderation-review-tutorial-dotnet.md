---
title: Azure Content Moderator - Orta videoları ve. NET'te dökümleri | Microsoft Docs
description: Videolar ve. NET'te dökümleri denetlemek Content Moderator'ı kullanma
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 1/27/2018
ms.author: sajagtap
ms.openlocfilehash: 0f851c030a05880d79a998ed4b4a941082c057b9
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865480"
---
# <a name="video-and-transcript-moderation-tutorial"></a>Video ve Döküm Denetimi Öğreticisi

İçerik denetleyicinin video API'leri videoları Orta ve video incelemeleri insan tarafından İnceleme aracı oluşturmanıza olanak sağlar. 

Bu öğretici yardımcı olan makine Yardımlı resim denetimi ve İnsan içinde--döngüsü gözden geçirme oluşturma ile eksiksiz bir video ve döküm denetimi çözümü derleme işlemini anlama ayrıntılı.

İndirme [C# konsol uygulaması](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) Bu öğretici için. Konsol uygulaması, aşağıdaki görevleri gerçekleştirmek için SDK ve ilişkili paketleri kullanır:

- Daha hızlı işleme giriş video Sıkıştır
- Görüntüleri ve öngörülerle çerçeveleri almak için video Orta
- Küçük resimleri (görüntüler) oluşturmak için çerçeve zaman damgaları kullanın
- Zaman damgaları ve küçük resim, video incelemeleri oluşturmak için gönderme
- Video Konuşmayı metne (döküm) Media Indexer API ile dönüştürün
- Metin denetimi hizmeti olan döküm Orta
- Yönetilen döküm video gözden geçirici ekleyin

## <a name="sample-program-outputs"></a>Örnek program çıkışı

Daha fazla geçmeden önce aşağıdaki örnek çıkışlara programından göz atalım:

- [Konsol çıktısı](#program-output)
- [Video gözden geçirme](#video-review-default-view)
- [Transkript görüntüle](#video-review-transcript-view)

## <a name="prerequisites"></a>Önkoşullar

1. Kaydolun [Content Moderator İnceleme aracı](https://contentmoderator.cognitive.microsoft.com/) web sitesi ve [özel etiketler oluşturmak](Review-Tool-User-Guide/tags.md) , C# konsol uygulaması içindeki kod gelen atar. Özel etiketler aşağıdaki ekran gösterilir.

  ![Video denetimi özel etiketler](images/video-tutorial-custom-tags.png)

1. Örnek uygulamayı çalıştırmak için bir Azure hesabı ve Azure Media Services hesabı gerekir. Ayrıca, Content Moderator özel Önizleme erişimi gerekir. Son olarak, Azure Active Directory kimlik doğrulama kimlik bilgileri gerekir. Bu bilgiler edinme hakkında daha fazla bilgi için bkz [Video denetimi API'si Hızlı Başlangıç](video-moderation-api.md).

1. Dosyayı düzenlemek `App.config` hizmet uç noktaları, Active Directory Kiracı adı ekleyin ve Abonelik anahtarları tarafından belirtilen `#####`. Aşağıdaki bilgiler gereklidir:

|Anahtar|Açıklama|
|-|-|
|`AzureMediaServiceRestApiEndpoint`|Azure Media Services (AMS) API uç noktası|
|`ClientSecret`|Azure Media Services için abonelik anahtarı|
|`ClientId`|Azure Media Services için istemci kimliği|
|`AzureAdTenantName`|Kuruluşunuz temsil eden active Directory Kiracı adı|
|`ContentModeratorReviewApiSubscriptionKey`|Abonelik anahtarı için Content Moderator API'si gözden geçirin|
|`ContentModeratorApiEndpoint`|Content Moderator API'si için uç nokta|
|`ContentModeratorTeamId`|Content moderator takım kimliği|

## <a name="getting-started"></a>Başlarken

Sınıf `Program` içinde `Program.cs` video denetimi uygulamanın ana giriş noktasıdır.

### <a name="methods-of-class-program"></a>Program sınıfının yöntemlerini

|Yöntem|Açıklama|
|-|-|
|`Main`|Komut satırı ayrıştırır, kullanıcı girişini toplayan ve işlemeye başlıyor.|
|`ProcessVideo`|Sıkıştırır, yükler, moderates ve video incelemeleri oluşturur.|
|`CreateVideoStreamingRequest`|Bir video karşıya yüklemek için bir akış oluşturur|
|`GetUserInputs`|Kullanıcı girişini toplayan; komut satırı seçeneği mevcut olduğunda kullanılır.|
|`Initialize`|Denetleme işlemi için gerekli nesneleri başlatır|

### <a name="the-main-method"></a>Main yöntemi

`Main()` video denetimi işlemini anlama başlangıç noktanız olduğundan yürütme başladığı olduğundan.

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

- MPEG-4 denetimi için gönderilecek video dosyalarını içeren bir dizin yolu. Tüm `*.mp4` dosyalarını bu dizinde ve alt dizinlerinde denetimi için gönderilir.
- Metin dökümleri amacıyla aracılık ses oluşturulması gerekip gerekmediğini belirten isteğe bağlı olarak, bir Boole (true/false) bayrağı.

Hiçbir komut satırı bağımsız değişkenleri yoksa `Main()` çağrıları `GetUserInputs()`. Bu yöntem, tek bir video dosyası yolunu girin ve bir metin dökümü oluşturulması gerekip gerekmediğini belirtmek için kullanıcıya sorar.

> [!NOTE]
> Konsol uygulaması kullanan [Azure Media Indexer API](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) karşıya yüklenen videonun ses parçasından dökümleri oluşturulacak. Sonuçları WebVTT biçiminde sağlanır. Bu biçimi hakkında daha fazla bilgi için bkz. [Web Video metin parçaları biçimi](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API).

### <a name="initialize-and-processvideo-methods"></a>Başlatma ve ProcessVideo yöntemleri

Komut satırından veya etkileşimli kullanıcı girişi, programın seçenekleri olup gelen bağımsız olarak `Main()` sonraki çağrılar `Initialize()` şu örnekleri oluşturmak için:

|Sınıf|Açıklama|
|-|-|
|`AMSComponent`|Video dosyaları için denetleme göndermeden önce sıkıştırır.|
|`AMSconfigurations`|Bulunan uygulama yapılandırma verileri arabirimine `App.config`.|
|`VideoModerator`| Karşıya yükleme, kodlama, şifreleme ve AMS SDK'sını kullanarak denetleme|
|`VideoReviewApi`|Content Moderator hizmetinde video incelemeleri yönetir|

Bu sınıflar (aside gelen `AMSConfigurations`, basit olduğu) Bu öğreticinin sonraki bölümlerde daha ayrıntılı olarak ele alınmaktadır.

Son olarak, video dosyaları çağırarak işlenen birer birer olan `ProcessVideo()` her.

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


`ProcessVideo()` Yöntemi oldukça açıktır. Bu sırada aşağıdaki işlemleri gerçekleştirir:

- Video sıkıştırır
- Video için bir Azure Media Services varlık yükler.
- Video denetlemek bir AMS işi oluşturur
- Content Moderator video bir inceleme oluşturur

Aşağıdaki bölümlerde daha ayrıntılı olarak çağrılan tek tek işlemler bazıları düşünün `ProcessVideo()`. 

## <a name="compressing-the-video"></a>Görüntü sıkıştırma

Ağ trafiğini en aza indirmek için uygulama video dosyalarını H.264 (AVC MPEG-4) biçimine dönüştürür ve bunları bir maksimum 640 piksel genişliğe ölçeklendirir. H.264 codec yüksek verimlilik (sıkıştırma oranı) nedeniyle önerilir. Sıkıştırma yapılır ücretsiz kullanarak `ffmpeg` dahil edilen komut satırı aracı `Lib` Visual Studio Çözüm klasörü. Giriş dosyaları tarafından desteklenen herhangi bir biçimdeki olabilir `ffmpeg`en sık kullanılan video dosyası biçimleri ve codec bileşenleri dahil olmak üzere.

> [!NOTE]
> Komut satırı seçeneklerini kullanarak programı başlattığınızda, denetimi için gönderilecek video dosyaları içeren dizini belirtin. Bu dizine sahip tüm dosyaların `.mp4` dosya adı uzantısı işlenir. Diğer dosya adı uzantılarını işlemek için güncelleştirme `Main()` yönteminde `Program.cs` istenen uzantıların eklenecek.

Tek bir video dosyası sıkıştırır kodu `AmsComponent` sınıfını `AMSComponent.cs`. Bu işlev için sorumlu yöntemdir `CompressVideo()`, burada gösterilen.

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

- Yapılandırmada emin olmak için denetimleri `App.config` gerekli tüm verileri içerir
- Emin olmak için denetimleri `ffmpeg` ikili yok
- Çıktı dosyası adını ekleyerek yapılar `_c.mp4` dosyasının temel adı için (gibi `Example.mp4`  ->  `E>xample_c.mp4`)
- Dönüştürme gerçekleştirmek için komut satırı dizesi oluşturur
- Başlatan bir `ffmpeg` komut satırını kullanarak işlem
- Videonun işlenmesi bekler

> [!NOTE]
> Videolarınızı H.264 kullanarak zaten sıkıştırılmış ve uygun boyutlarda biliyorsanız, yazabilirsiniz `CompressVideo()` sıkıştırma atlanacak.

Bu yöntem, sıkıştırılmış çıkış dosyasının dosya adını döndürür.

## <a name="uploading-and-moderating-the-video"></a>Karşıya yükleme ve video yönetme

İçerik denetleme hizmeti tarafından işlenebilmesi Azure Media Services'da video depolanmalıdır. `Program` Sınıfını `Program.cs` kısa bir yönteme sahip `CreateVideoStreamingRequest()` video karşıya yüklemek için kullanılan akış isteği temsil eden bir nesne döndürür.

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

Ortaya çıkan `UploadVideoStreamRequest` nesne tanımlanmış `UploadVideoStreamRequest.cs` (ve kendi üst `UploadVideoRequest`, `UploadVideoRequest.cs`). Bu sınıfların burada gösterilmez; Bunlar kısa ve yalnızca ilgili bilgileri ve sıkıştırılmış görüntü verileri tutmak için hizmet. Başka bir veri yalnızca sınıf `UploadAssetResult` (`UploadAssetResult.cs`) karşıya yükleme işleminin sonuçlarını tutmak için kullanılır. Şimdi bu satırları anlamak olası `ProcessVideo()`:

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

Bu satırlar, aşağıdaki görevleri gerçekleştirin:

- Oluşturma bir `UploadVideoStreamRequest` sıkıştırılmış video karşıya yüklemek için
- İsteğin ayarlayın `GenerateVTT` kullanıcı metin dökümü istenirse bayrak
- Çağrıları `CreateAzureMediaServicesJobToModerateVideo()` karşıya yükleme gerçekleştirin ve sonuç almak için

## <a name="deep-dive-into-video-moderation"></a>Video denetimi derinlemesine inceleyin

Yöntem `CreateAzureMediaServicesJobToModerateVideo()` bulunduğu `VideoModerator.cs`, toplu Azure Media Services ile etkileşime giren kodu içerir. Yöntemin kaynak kodu aşağıdaki Ayıkla gösterilir.

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

- İşlemenin gerçekleştirilmesi için bir AMS işi oluşturur
- Video dosyasını kodlama, yönetme ve metin transkript oluşturma için görevler ekler
- Başlangıç işleme ve dosyayı karşıya yükleme işi gönderir
- Denetleme sonuçları, metin dökümü (istenirse) ve diğer bilgileri alır.

## <a name="sample-video-moderation-response"></a>Örnek video denetimi yanıt

Video denetimi iş sonucu (bkz [video denetimi hızlı](video-moderation-api.md) olduğunu denetleme sonuçlarını içeren bir JSON veri yapısı. Bu sonuçları parçaları (görüntüleri) dökümünü videodaki içeren her olayları gözden geçirilmek üzere işaretlenmiş anahtar çerçeveler ile (klipleri) içerir. Her anahtar çerçeve yetişkinlere yönelik veya müstehcen içerik içerdiği olasılığına göre puanlanır. Aşağıdaki örnek bir JSON yanıtı gösterilir:

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

Ayrıca bir Videodan ses tanıma, ne zaman üretilen `GenerateVTT` bayrağı ayarlanır.

> [!NOTE]
> Konsol uygulaması kullanan [Azure Media Indexer API](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) karşıya yüklenen videonun ses parçasından dökümleri oluşturulacak. Sonuçları WebVTT biçiminde sağlanır. Bu biçimi hakkında daha fazla bilgi için bkz. [Web Video metin parçaları biçimi](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API).


## <a name="creating-the-human-in-the-loop-review"></a>İnsan içinde--döngüsü İnceleme

Denetleme işlemi, video, ses parçaları bir dökümü ile birlikte gelen anahtar çerçeveler listesini döndürür. Sonraki adım, İnsan Moderatörler için Content Moderator İnceleme aracında bir inceleme oluşturmaktır. Geri giderek `ProcessVideo()` yönteminde `Program.cs`, çağrısına bakın `CreateVideoReviewInContentModerator()` yöntemi. Bu yöntem `videoReviewApi` bulunduğu sınıfı `VideoReviewAPI.cs`ve burada gösterilir.

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
> Konsol uygulaması kullanan [FFmpeg](https://ffmpeg.org/) küçük resimler oluşturma kitaplığı. Bu küçük resimleri (görüntüler) çerçeve veritabanındaki tarih damgası karşılık [video denetimi çıkış](#sample-video-moderation-response).

|Görev|Yöntemler|Dosya|
|-|-|-|
|Anahtar videodan kareler ve bunların küçük resimler oluşturur Ayıkla|`CreateVideoFrames()`<br>`GenerateFrameImages()`|`FrameGeneratorServices.cs`|
|Metin dökümü kullanılabiliyorsa, yetişkinlere yönelik veya müstehcen ses bulmak tarama|`GenerateTextScreenProfanity()`| `VideoReviewAPI.cs`|
|Hazırlama ve insan tarafından İnceleme için bir video gözden geçirme isteği gönderir|`CreateReviewRequestObject()`<br> `ExecuteCreateReviewApi()`<br>`CreateAndPublishReviewInContentModerator()`|`VideoReviewAPI.cs`|

## <a name="video-review-default-view"></a>Video gözden geçirme varsayılan görünüm

Aşağıdaki ekranda, önceki adımın sonuçları gösterilmektedir.

![Video gözden geçirme varsayılan görünüm](images/video-tutorial-default-view.PNG)

## <a name="transcript-generation"></a>Döküm oluşturma

Şimdiye kadar bu öğreticide gösterilen kodun görsel içeriğe bağlı odaklanıyor. Konuşma içeriğini gözden geçirmesini belirtildiği gibi seslerden oluşturulan bir döküm kullanır, ayrı ve isteğe bağlı bir işlemdir. Bu metin dökümleri nasıl oluşturulduğunu ve gözden geçirme işleminde kullanılan göz atalım artık zamanıdır. Transkript oluşturma görevini döner [Azure Media Indexer'ın](https://docs.microsoft.com/azure/media-services/media-services-index-content) hizmeti.

Uygulama, aşağıdaki görevleri gerçekleştirir:

|Görev|Yöntemler|Dosya|
|-|-|-|
|Metin dökümleri oluşturulacak olup olmadığını belirleme|`Main()`<br>`GetUserInputs()`|`Program.cs`|
|Bu durumda, döküm iş denetimi bir parçası olarak gönder|`ConfigureTranscriptTask()`|`VideoModerator.cs`|
|Transkripti yerel bir kopyasını alın|`GenerateTranscript()`|`VideoModerator.cs`|
|Uygunsuz ses içeren videonun karelerini bayrak|`GenerateTextScreenProfanity()`<br>`TextScreen()`|`VideoReviewAPI.cs`|
|Sonuçları gözden geçirici ekleyin|`UploadScreenTextResult()`<br>`ExecuteAddTranscriptSupportFile()`|`VideoReviewAPI.cs`|

### <a name="task-configuration"></a>Görev yapılandırması

Şimdi transkripsiyonu işi gönderiliyor içine sağ atlayın. `CreateAzureMediaServicesJobToModerateVideo()` (zaten açıklandığı gibi) çağrılarını `ConfigureTranscriptTask()`.

    private void ConfigureTranscriptTask(IJob job)
    {
        string mediaProcessorName = _amsConfigurations.MediaIndexer2MediaProcessor;
        IMediaProcessor processor = _mediaContext.MediaProcessors.GetLatestMediaProcessorByName(mediaProcessorName);

        string configuration = File.ReadAllText(_amsConfigurations.MediaIndexerConfigurationJson);
        ITask task = job.Tasks.AddNew("AudioIndexing Task", processor, configuration, TaskOptions.None);
        task.InputAssets.Add(asset);
        task.OutputAssets.AddNew("AudioIndexing Output Asset", AssetCreationOptions.None);
    }

Transkript görev için yapılandırma dosyasından okunur `MediaIndexerConfig.json` çözümün içinde `Lib` klasör. AMS varlıklarını transkripsiyonu işleminin çıktı ve yapılandırma dosyası için oluşturulur. AMS işi çalıştığında, bu görev bir metin dökümü ses video dosyanın parçasından oluşturur.

> [!NOTE]
> Örnek uygulamayı yalnızca konuşma ABD İngilizcesi olarak tanır.

### <a name="transcript-generation"></a>Döküm oluşturma

Transkripti bir AMS varlığı yayımlanır. Transkripti uygunsuz içeriği için taramak için Azure Media Services'den varlık uygulamayı yükler. `CreateAzureMediaServicesJobToModerateVideo()` çağrıları `GenerateTranscript()`gösterilen dosyasını almak için burada.

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

Bazı gerekli AMS Kurulumdan sonra gerçek indirme çağrılarak gerçekleştirilir `DownloadAssetToLocal()`, AMS varlığı yerel bir dosyaya kopyalar genel bir işlev.

## <a name="transcript-moderation"></a>Transkript denetleme

Transkripti ile eldeki kapatın, bu taranan ve incelemeye kullanılır. Gözden geçirme oluşturmak, kapsamında olan `CreateVideoReviewInContentModerator()`, bu çağrılar `GenerateTextScreenProfanity()` işini yapması için. Sırayla bu yöntemi çağırır `TextScreen()`, işlevselliğinin büyük kısmını içeren. 

`TextScreen()` Aşağıdaki görevleri gerçekleştirir:

- Transkripti kez tamps ve kapalı açıklamalı alt yazı ayrıştırma
- Metin denetimi için her bir açıklama Gönder
- Uygunsuz konuşma içeriklere sahip olabilecek herhangi bir kare bayrak

Her bu görevleri daha ayrıntılı inceleyelim:

### <a name="initialize-the-code"></a>Kod Başlat

İlk olarak, tüm değişkenleri ve Koleksiyonlar başlatın.

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
    

### <a name="parse-the-transcript-for-captions"></a>Transkripti alt yazıların ayrıştırılamıyor

Ardından, açıklamalı alt yazılar ve zaman damgaları VTT biçimlendirilmiş transkripti ayrıştırılamıyor. İnceleme aracını bu açıklamalı alt yazılar döküm sekmesinde video İnceleme ekranı görüntüler. Zaman damgaları, resim yazıları karşılık gelen video çerçeveler ile eşitlemek için kullanılır.

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

### <a name="moderate-captions-with-the-text-moderation-service"></a>Metin denetimi hizmeti ile orta açıklamalı alt yazılar

Ardından, ayrıştırılmış metin açıklamalı alt yazılar Content Moderator'ın metin tanıma API'si ile tarayın.

> [!NOTE]
> Content Moderator hizmeti anahtarınızı ikinci (RP'ler) hız sınırı başına bir istek var. Sınırı aşarsanız, SDK'sı 429 hata koduna sahip özel durum oluşturur. 
>
> Ücretsiz katmanı anahtarı bir RPS oranı sınırı vardır.

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

### <a name="breaking-down-the-text-moderation-step"></a>Metin denetimi Adım sonu

`TextScreen()` Haydi bölmek, önemli bir yöntem olduğundan.

1. İlk olarak, yöntem satır döküm dosyasını okur. Satırları içeren ve boş yoksayar bir `NOTE` ile bir güven puanı. Zaman damgaları ve metin öğeleri ayıklar *ipuçları* dosyasında. İpucu metin, ses parçasından temsil eder ve başlangıç ve bitiş zamanlarını içerir. Dize zaman damgası satırla ipucu başlar `-->`. Bir veya daha fazla satırlık metin izler.

1. Örneklerini `CaptionScreentextResult` (tanımlanan `TranscriptProfanity.cs`) her işaret Ayrıştırılan bilgileri tutmak için kullanılır.  Ne zaman yeni bir zaman damgası satır algılandığında veya bir maksimum metin uzunluğu 1024 karakterle ulaşıldığında, yeni bir `CaptionScreentextResult` eklenir `csrList`. 

1. Yöntemi, sonraki metin denetimi API'sine yapılan her işaret gönderir. Her ikisi de çağırır `ContentModeratorClient.TextModeration.DetectLanguageAsync()` ve `ContentModeratorClient.TextModeration.ScreenTextWithHttpMessagesAsync()`, içinde tanımlandığı `Microsoft.Azure.CognitiveServices.ContentModerator` derleme. Sınırlı hızı anda önlemek için yöntemi her işaret göndermeden önce bir saniye için duraklatır.

1. Metin denetimi hizmetinden sonuçları aldıktan sonra yöntemi daha sonra bunları güvenle eşikleri karşılayıp karşılamadığını görmek için analiz eder. Bu değerleri içinde belirlenen `App.config` olarak `OffensiveTextThreshold`, `RacyTextThreshold`, ve `AdultTextThreshold`. Son olarak, uygunsuz koşulları da depolanır. İşaret ait zaman aralığı içinde tüm çerçeveleri, rahatsız edici, müstehcen içeren ve/veya yetişkin metin işaretlenir.

1. `TextScreen()` döndürür bir `TranscriptScreenTextResult` içeren bir bütün olarak video metin denetimi sonuç örneği. Bu nesne bayraklarını içerir ve tüm uygunsuz terimlerin bir listesi ile birlikte uygunsuz içeriği çeşitli türleri puanlar. Arayan `CreateVideoReviewInContentModerator()`, çağrıları `UploadScreenTextResult()` İnsan gözden geçirenlere kullanılabilir olacak şekilde bu bilgiler gözden eklemek için.
 
## <a name="video-review-transcript-view"></a>Video gözden geçirme döküm görüntüle

Aşağıdaki ekranda, oluşturma ve denetleme adımları transkripti sonucunu gösterir.

![Video denetimi döküm görüntüle](images/video-tutorial-transcript-view.PNG)

## <a name="program-output"></a>Program çıktısı

Tamamlandıkça programın komut satırı çıkışı aşağıdaki çeşitli görevler gösterilmektedir. Buna ek olarak, denetimi sonucu (JSON biçiminde) ve konuşma dökümü özgün video dosyaları ile aynı dizinde kullanılabilir durumdadır.

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

[Visual Studio çözümü indirme](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) dosyaları ve gerekli kitaplıkları Bu öğretici için örnek ve tümleştirmenizi üzerinde kullanmaya başlayın.
