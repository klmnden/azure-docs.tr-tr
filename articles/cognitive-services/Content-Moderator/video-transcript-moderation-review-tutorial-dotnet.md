---
title: "Öğretici: Orta videoları ve. NET'te - Content Moderator dökümleri"
titlesuffix: Azure Cognitive Services
description: Bu öğreticide makine Yardımlı resim denetimi ve İnsan içinde--döngüsü gözden geçirme oluşturma ile tam bir video ve döküm denetimi çözümü oluşturmak nasıl anlamanıza yardımcı olur.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: tutorial
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 6f5a6c6ac4bd125fd7aa6358fe92f9453a0314b1
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266783"
---
# <a name="tutorial-video-and-transcript-moderation"></a>Öğretici: Video ve döküm denetleme

Content Moderator'ın video API'leri videoları denetlemenizi ve kullanıcı tarafından çalıştırılan gözden geçirme aracında video incelemeleri oluşturmanızı sağlar. 

Bu öğreticide makine Yardımlı resim denetimi ve İnsan içinde--döngüsü gözden geçirme oluşturma ile tam bir video ve döküm denetimi çözümü oluşturmak nasıl anlamanıza yardımcı olur.

Bu öğretici için [C# konsol uygulamasını](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) indirin. Konsol uygulaması SDK'yı ve ilgili paketleri kullanarak aşağıdaki görevleri yerine getirir:

- Daha hızlı işlenmesi için giriş videolarını sıkıştırma
- İçgörüler içeren çekimleri ve kareleri almak için videoyu denetleme
- Küçük resimler (görüntüler) oluşturmak için kare zaman damgalarını kullanma
- Video incelemeleri oluşturmak için zaman damgalarını ve küçük resimleri gönderme
- Media Indexer API'siyle video konuşmasını metne (transkript) dönüştürme
- Metin denetimi hizmetiyle transkripti denetleme
- Denetlenen transkripti video incelemesine ekleme

## <a name="sample-program-outputs"></a>Örnek program çıkışları

Daha fazla geçmeden önce aşağıdaki örnek çıkışlara programından göz atalım:

- [Konsol çıkışı](#program-output)
- [Video incelemesi](#video-review-default-view)
- [Transkript görünümü](#video-review-transcript-view)

## <a name="prerequisites"></a>Önkoşullar

1. [Content Moderator inceleme aracı](https://contentmoderator.cognitive.microsoft.com/) web sitesine kaydolun ve C# konsol uygulamasının kodun içinden atadığı [özel etiketler oluşturun](Review-Tool-User-Guide/tags.md). Aşağıdaki ekranda özel etiketler gösterilir.

  ![Video denetimi özel etiketleri](images/video-tutorial-custom-tags.png)

1. Örnek uygulamayı çalıştırmak için, bir Azure hesabına ve Azure Media Services hesabına ihtiyacınız vardır. Buna ek olarak, Content Moderator özel önizlemesine de erişiminiz olmalıdır. Son olarak, Azure Active Directory kimlik doğrulama bilgileri gerekir. Bu bilgileri alma konusundaki ayrıntılar için bkz. [Video Denetim API'si hızlı başlangıcı](video-moderation-api.md).

1. `App.config` dosyasını düzenleyin ve Active Directory kiracı adını, sunucu uç noktalarını ve `#####` tarafından belirtilen abonelik anahtarlarını ekleyin. Aşağıdaki bilgiler gerekir:

|Anahtar|Açıklama|
|-|-|
|`AzureMediaServiceRestApiEndpoint`|Azure Media Services (AMS) API'si için uç nokta|
|`ClientSecret`|Azure Media Services için abonelik anahtarı|
|`ClientId`|Azure Media Services için istemci kimliği|
|`AzureAdTenantName`|Kuruluşunuzu temsil eden Active Directory kiracı adı|
|`ContentModeratorReviewApiSubscriptionKey`|Content Moderator inceleme API'si için abonelik anahtarı|
|`ContentModeratorApiEndpoint`|Content Moderator API’si için uç nokta|
|`ContentModeratorTeamId`|Content Moderator ekip kimliği|

## <a name="getting-started"></a>Başlarken

`Program.cs` içindeki `Program` sınıfı, video denetim uygulamasına ana giriş noktasıdır.

### <a name="methods-of-class-program"></a>Program sınıfının yöntemleri

|Yöntem|Açıklama|
|-|-|
|`Main`|Komut satırını ayrıştırır, kullanıcı girişlerini toplar ve işlemeyi başlatır.|
|`ProcessVideo`|Video incelemelerini sıkıştırır, karşıya yükler, denetler ve oluşturur.|
|`CreateVideoStreamingRequest`|Videoyu karşıya yüklemek için bir akış oluşturur|
|`GetUserInputs`|Kullanıcı girişlerini toplar; hiç komut satırı seçeneği olmadığında kullanılır|
|`Initialize`|Denetim işlemi için gereken nesneleri başlatır|

### <a name="the-main-method"></a>Main yöntemi

`Main()` yürütmenin başlatıldığı yerdir, dolayısıyla video denetleme işlemini anlamaya buradan başlanır.

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

`Main()` aşağıdaki komut satırı bağımsız değişkenlerini işler:

- Denetleme için gönderilecek MPEG-4 video dosyalarını içeren dizinin yolu. Bu dizindeki ve onun alt dizinlerindeki tüm `*.mp4` dosyaları denetleme için gönderilir.
- İsteğe bağlı olarak, sesi denetlemek amacıyla metin transkriptlerinin oluşturulup oluşturulmayacağını gösteren bir Boole (doğru/yanlış) bayrağı.

Hiçbir komut satırı bağımsız değişkeni yoksa, `Main()` `GetUserInputs()` çağrısı yapar. Bu yöntem kullanıcıdan tek bir video dosyasının yolunu girmesini ve metin transkriptinin oluşturulup oluşturulmayacağını belirtmesini ister.

> [!NOTE]
> Konsol uygulaması karşıya yüklenen videonun ses parçasından transkriptleri oluşturmak için [Azure Media Indexer API'sini](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) kullanır. Sonuçlar WebVTT biçiminde sağlanır. Bu biçimle ilgili daha fazla bilgi için bkz. [Web Video Metin Parçaları Biçimi](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API).

### <a name="initialize-and-processvideo-methods"></a>Initialize ve ProcessVideo yöntemleri

Programın seçenekleri ister komut satırından ister etkileşimli kullanıcı girişinden gelsin, bundan sonra `Main()` aşağıdaki örnekleri oluşturmak için `Initialize()` yöntemini çağırır:

|Sınıf|Açıklama|
|-|-|
|`AMSComponent`|Video dosyalarını denetleme için göndermeden önce sıkıştırır.|
|`AMSconfigurations`|Uygulamanın `App.config` dosyasında bulunan yapılandırma verilerine arabirim oluşturur.|
|`VideoModerator`| AMS SDK'sı kullanılarak karşıya yükleme, kodlama, şifreleme ve denetleme|
|`VideoReviewApi`|Content Moderator hizmetindeki video incelemelerini yönetir|

Bu sınıflar (gayet anlaşılır olan `AMSConfigurations` dışında), bu öğreticinin gelecek bölümlerinde daha ayrıntılı olarak ele alınacaktır.

Son olarak, video dosyalarının her biri için `ProcessVideo()` yöntemi çağrılarak bu dosyalar birer birer işlenir.

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


`ProcessVideo()` yöntemi gayet kolay anlaşılır. Sırasıyla aşağıdaki işlemleri gerçekleştirir:

- Videoyu sıkıştırır
- Videoyu Azure Media Services varlığına yükler
- Videoyu denetlemek için bir AMS işi oluşturur
- Content Moderator'da bir video incelemesi oluşturur

Aşağıdaki bölümlerde `ProcessVideo()` tarafından çağrılan tek tek işlemler biraz daha ayrıntılı incelenecektir. 

## <a name="compressing-the-video"></a>Videoyu sıkıştırma

Ağ trafiğini en aza indirmek için, uygulama video dosyalarını H.264 (MPEG-4 AVC) biçimine dönüştürür ve en çok 640 piksel genişliğe ölçeklendirir. Yüksek verimliliği (sıkıştırma oranı) nedeniyle H.264 codec bileşeni önerilir. Sıkıştırma, Visual Studio çözümünün `Lib` klasörüne eklenmiş olan ücretsiz `ffmpeg` komut satırı aracı kullanılarak yapılır. Giriş dosyaları, en yaygın kullanılan video dosyası biçimleri ve codec bileşenleri de dahil olmak üzere `ffmpeg` tarafından desteklenen herhangi bir biçimde olabilir.

> [!NOTE]
> Komut satırı seçeneklerini kullanarak programı başlattığınızda, denetim için gönderilecek vide dosyalarını içeren dizini belirtirsiniz. Bu dizinde yer alan ve `.mp4` dosya adı uzantısına sahip olan dosyaların tümü işlenir. Diğer dosya adı uzantılarını işlemek için, `Program.cs` dosyasında `Main()` yöntemini istenen uzantıları içerecek şekilde güncelleştirin.

Tek video dosyasını sıkıştıran kod `AMSComponent.cs` içinde `AmsComponent` sınıfıdır. Bu işlevden sorumlu olan yöntem, burada gösterilen `CompressVideo()` yöntemidir.

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

Kod aşağıdaki adımları gerçekleştirir:

- `App.config` dosyasındaki yapılandırmayı denetleyerek tüm gerekli verileri içerdiğinden emin olur
- `ffmpeg` ikilisini denetleyerek var olduğundan emin olur
- `_c.mp4` öğesini temel dosya adına ekleyerek çıkış dosya adını oluşturur (örneğin, `Example.mp4` -> `E>xample_c.mp4`)
- Dönüştürmeyi gerçekleştirmek için bir komut satırı dizesi oluşturur
- Komut satırını kullanarak bir `ffmpeg` işlemi başlatır
- Videonun işlenmesini bekler

> [!NOTE]
> Videolarınızın zaten H.264 kullanılarak sıkıştırıldığını ve uygun boyutlara sahip olduğunu biliyorsanız, sıkıştırmayı atlamak için `CompressVideo()` yöntemini yeniden yazabilirsiniz.

Bu yöntem sıkıştırılmış çıkış dosyasının adını döndürür.

## <a name="uploading-and-moderating-the-video"></a>Videoyu karşıya yükleme ve denetleme

Videonun Content Moderation hizmeti tarafından işlenebilmesi için önce Azure Media Services'de depolanması gerekir. `Program.cs` içindeki `Program` sınıfının, videoyu karşıya yüklemek için kullanılan akış isteğini temsil eden bir nesnenin döndürüldüğü kısa bir `CreateVideoStreamingRequest()` yöntemi vardır.

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

Sonuçta elde edilen `UploadVideoStreamRequest` nesnesi `UploadVideoStreamRequest.cs` içinde tanımlanır (ve üst öğesi olan `UploadVideoRequest` de `UploadVideoRequest.cs` içinde). Bu sınıflar burada gösterilmiyor; bunlar kısadır ve yalnızca sıkıştırılmış video verilerini ve onlar hakkındaki bilgileri barındırma işine yarar. Bir diğer salt veri sınıfı olan `UploadAssetResult` ise (`UploadAssetResult.cs`) karşıya yükleme işleminin sonuçlarını barındırır. Artık, `ProcessVideo()` içindeki şu satırları anlamak mümkün olacaktır:

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

Bu satırlar aşağıdaki görevleri gerçekleştirir:

- Sıkıştırılmış videoyu karşıya yüklemek için bir `UploadVideoStreamRequest` oluşturma
- Kullanıcı metin transkripti istediyse isteğin `GenerateVTT` bayrağını ayarlama
- Karşıya yüklemeyi gerçekleştirmek ve sonucu almak için `CreateAzureMediaServicesJobToModerateVideo()` çağrısı yapma

## <a name="deep-dive-into-video-moderation"></a>Video denetimine yakından bakma

`CreateAzureMediaServicesJobToModerateVideo()` yöntemi, Azure Media Services ile etkileşim kuran kodun büyük bölümünün yer aldığı `VideoModerator.cs` içindedir. Yöntemin kaynak kodu aşağıdaki alıntıda gösterilir.

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

Bu kod aşağıdaki görevleri gerçekleştirir:

- İşlemenin yapılması için bir AMS işi oluşturur
- Video dosyasını kodlama, denetleme ve metin transkripti oluşturma için görevler ekler
- Dosyayı karşıya yükleyerek ve işlemeye başlayarak işi gönderir
- Denetleme sonuçlarını, metin transkriptini (istendiyse) ve diğer bilgileri alır

## <a name="sample-video-moderation-response"></a>Örnek video denetimi yanıtı

Video denetimi işinin sonucu (Bkz. [video denetimine hızlı başlangıç](video-moderation-api.md)), denetim sonuçlarını içeren bir JSON veri yapısıdır. Bu sonuçlarda, inceleme için bayrak eklenmiş anahtar karelerle olayları (klipleri) içeren video içindeki parçaların (çekimlerin) dökümü yer alır. Her anahtar kare, yetişkinlere yönelik veya müstehcen içerik bulundurma olasılığına göre puanlanır. Aşağıdaki örnekte JSON yanıtı gösterilir:

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

`GenerateVTT` bayrağı ayarlandığında videodan ses transkripsiyonu da oluşturulur.

> [!NOTE]
> Konsol uygulaması karşıya yüklenen videonun ses parçasından transkriptleri oluşturmak için [Azure Media Indexer API'sini](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) kullanır. Sonuçlar WebVTT biçiminde sağlanır. Bu biçimle ilgili daha fazla bilgi için bkz. [Web Video Metin Parçaları Biçimi](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API).


## <a name="creating-the-human-in-the-loop-review"></a>Döngüye insanı da ekleyen bir inceleme oluşturma

Denetleme işlemi videodaki anahtar karelerin listesini ve ses parçalarının transkriptini döndürür. Sonraki adım, insan denetleyiciler için Content Moderator inceleme aracında bir inceleme oluşturmaktır. `Program.cs` dosyasında `ProcessVideo()` yöntemine döndüğünüzde, `CreateVideoReviewInContentModerator()` yöntemine yapılan çağrıyı görürsünüz. Bu yöntem `VideoReviewAPI.cs` içinde yer alan `videoReviewApi` sınıfındadır ve aşağıda gösterilmiştir.

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

`CreateVideoReviewInContentModerator()` aşağıdaki görevleri gerçekleştirmek için başka bazı yöntemleri çağırır:

> [!NOTE]
> Konsol uygulaması küçük resimleri oluşturmak için [FFmpeg](https://ffmpeg.org/) kitaplığını kullanır. Bu küçük resimler (görüntüler), [video denetimi çıkışındaki](#sample-video-moderation-response) kare zaman damgalarına karşılık gelir.

|Görev|Yöntemler|Dosya|
|-|-|-|
|Videodan anahtar kareleri ayıklama ve bunların küçük resim görüntülerini oluşturma|`CreateVideoFrames()`<br>`GenerateFrameImages()`|`FrameGeneratorServices.cs`|
|Yetişkinlere yönelik ve müstehcen sesi (varsa) bulmak için metin transkriptini tarama|`GenerateTextScreenProfanity()`| `VideoReviewAPI.cs`|
|İnsan incelemesi için video inceleme isteğini hazırlama ve gönderme|`CreateReviewRequestObject()`<br> `ExecuteCreateReviewApi()`<br>`CreateAndPublishReviewInContentModerator()`|`VideoReviewAPI.cs`|

## <a name="video-review-default-view"></a>Video incelemesi varsayılan görünümü

Aşağıdaki ekranda önceki adımların sonuçları gösterilir.

![Video incelemesi varsayılan görünümü](images/video-tutorial-default-view.PNG)

## <a name="transcript-generation"></a>Transkript oluşturma

Şimdiye kadar, bu öğreticide tanıtılan kod görsel içeriğe odaklanmıştı. Konuşma içeriğinin incelenmesi ayrı ve isteğe bağlı bir işlemdir ve daha önce belirtildiği gibi sesten oluşturulan transkripti kullanır. Artık metin transkriptlerinin nasıl oluşturulduğunu ve inceleme işleminde nasıl kullanıldığını gözden geçirmenin zamanı geldi. Transkript oluşturma görevi, [Azure Media Indexer](https://docs.microsoft.com/azure/media-services/media-services-index-content) hizmetinin kapsamına girer.

Uygulama aşağıdaki görevleri gerçekleştirir:

|Görev|Yöntemler|Dosya|
|-|-|-|
|Metin transkriptlerinin oluşturulup oluşturulmayacağını belirleme|`Main()`<br>`GetUserInputs()`|`Program.cs`|
|Oluşturulacaksa, denetimin bir parçası olarak transkripsiyon işini gönderme|`ConfigureTranscriptTask()`|`VideoModerator.cs`|
|Transkriptin yerel kopyasını alma|`GenerateTranscript()`|`VideoModerator.cs`|
|Uygunsuz ses içeren video karelerine bayrak ekleme|`GenerateTextScreenProfanity()`<br>`TextScreen()`|`VideoReviewAPI.cs`|
|Sonuçları incelemeye ekleme|`UploadScreenTextResult()`<br>`ExecuteAddTranscriptSupportFile()`|`VideoReviewAPI.cs`|

### <a name="task-configuration"></a>Görev yapılandırması

Şimdi doğrudan transkripsiyon işini gönderme işlemine geçelim. `CreateAzureMediaServicesJobToModerateVideo()` (daha önce açıklanmıştı) `ConfigureTranscriptTask()` yöntemini çağırır.

    private void ConfigureTranscriptTask(IJob job)
    {
        string mediaProcessorName = _amsConfigurations.MediaIndexer2MediaProcessor;
        IMediaProcessor processor = _mediaContext.MediaProcessors.GetLatestMediaProcessorByName(mediaProcessorName);

        string configuration = File.ReadAllText(_amsConfigurations.MediaIndexerConfigurationJson);
        ITask task = job.Tasks.AddNew("AudioIndexing Task", processor, configuration, TaskOptions.None);
        task.InputAssets.Add(asset);
        task.OutputAssets.AddNew("AudioIndexing Output Asset", AssetCreationOptions.None);
    }

Transkript görevinin yapılandırması, çözümün `Lib` klasöründe yer alan `MediaIndexerConfig.json` dosyasından okunur. Yapılandırma dosyası için ve transkripsiyon işleminin çıkışı için AMS varlıkları oluşturulur. AMS işi çalıştırıldığında, bu görev video dosyasının ses parçasından metin transkriptini oluşturur.

> [!NOTE]
> Örnek uygulama yalnızca ABD İngilizcesi konuşmaları tanır.

### <a name="transcript-generation"></a>Transkript oluşturma

Transkript bir AMS varlığı olarak yayımlanır. Transkripti uygunsuz içerik için taramak amacıyla, uygulama varlığı Azure Media Services'den indirir. Dosyayı almak için `CreateAzureMediaServicesJobToModerateVideo()` burada gösterilen `GenerateTranscript()` yöntemini çağırır.

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
            Console.WriteLine("Exception occurred while generating index for video.");
            throw;
        }
    }

Bazı gerekli AMS kurulum işlemlerinden sonra, indirme işlemi AMS varlığını yerel dosyaya kopyalayan genel `DownloadAssetToLocal()` işlevi çağrılarak gerçekleştirilir.

## <a name="transcript-moderation"></a>Transkripti denetleme

Transkript elinizin altında olduğundan, taranır ve incelemede kullanılır. İncelemeyi oluşturmak `CreateVideoReviewInContentModerator()` yönteminin konusudur ve bu işi yapmak için `GenerateTextScreenProfanity()` yöntemini çağırır. Bu yöntem de işlevlerin çoğunu içeren `TextScreen()` yöntemini çağırır. 

`TextScreen()` aşağıdaki görevleri gerçekleştirir:

- Zaman damgaları ve açıklamalı alt yazılar için transkripti ayrıştırma
- Her açıklamalı alt yazıyı metin denetimi için gönderme
- Uygunsuz konuşma içeriği bulunuyor olabilecek tüm karelere bayrak ekleme

Şimdi bu görevleri daha ayrıntılı inceleyelim:

### <a name="initialize-the-code"></a>Kodu başlatma

İlk olarak tüm değişkenleri ve koleksiyonları başlatın.

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
    

### <a name="parse-the-transcript-for-captions"></a>Açıklamalı alt yazılar için transkripti ayrıştırma

Ardından, VTT biçimindeki transkripti açıklamalı alt yazılar ve zaman damgaları için ayrıştırın. İnceleme aracı, video inceleme ekranının Transkript Sekmesinde bu açıklamalı alt yazıları görüntüler. Zaman damgaları açıklamalı alt yazıları ilgili video kareleriyle eşleştirmekte kullanılır.

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

### <a name="moderate-captions-with-the-text-moderation-service"></a>Metin denetimi hizmetiyle açıklamalı alt yazıları denetleme

Ardından, ayrıştırılmış metin açıklamalı alt yazıları Content Moderator'ın metin API'siyle tararız.

> [!NOTE]
> Content Moderator hizmet anahtarınızın saniyede istek sayısı (RPS) hız sınırı vardır. Sınırı aşarsanız, SDK 429 hata koduyla bir özel durum oluşturulur. 
>
> Ücretsiz katmanı anahtarı bir RPS’lik hız sınırına sahiptir.

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

### <a name="breaking-down-the-text-moderation-step"></a>Metin denetimi adımını bölümlere ayırma

`TextScreen()` önemli bir yöntemdir, bu nedenle şimdi onu bölümlerine ayıralım.

1. İlk olarak, yöntem transkript dosyasını satır satır okur. Boş satırları ve güvenilirlik puanı ile `NOTE` içeren satırları yoksayar. Dosyadaki *ipuçlarından* zaman damgalarını ve metin öğelerini ayıklar. İpucu, ses parçasındaki metinleri temsi eder ve başlangıç ile bitiş saatlerini içerir. İpucu, `-->` dizesini içeren zaman damgası satırıyla başlar. Bunu bir veya birden çok metin satırı izler.

1. Her ipucundan ayıklanan bilgileri tutmak için `CaptionScreentextResult` örnekleri (`TranscriptProfanity.cs` içinde tanımlanır) kullanılır.  Yeni bir zaman damgası satırı algılandığında veya metin uzunluğu üst sınırı olan 1024 karaktere ulaşıldığında, `csrList` içine yeni bir `CaptionScreentextResult` eklenir. 

1. Yöntem bundan sonra her ipucunu Metin Denetimi API'sine gönderir `Microsoft.Azure.CognitiveServices.ContentModerator` derlemesinde tanımlanan hem `ContentModeratorClient.TextModeration.DetectLanguageAsync()` hem de `ContentModeratorClient.TextModeration.ScreenTextWithHttpMessagesAsync()` yöntemini çağırır. Hız sınırlamasıyla karşılaşmayı önlemek için, yöntem her ipucunu göndermeden önce bir saniye duraklar.

1. Metin Denetimi hizmetinden sonuçları aldıktan sonra, yöntem güvenilirlik eşiklerine uyum uymadığını görmek için bunları analiz eder. Bu değerler `App.config` dosyasında `OffensiveTextThreshold`, `RacyTextThreshold` ve `AdultTextThreshold` olarak oluşturulur. Son olarak, uygunsuz terimler de depolanır. İpucunun zaman aralığı içinde kalan tüm kareler saldırgan, müstehcen ve/veya yetişkinlere yönelik metin içeriyor bayrağı ekler.

1. `TextScreen()`, bir bütün olarak videodan gelen metin denetimi sonucunu içeren bir `TranscriptScreenTextResult` örneği döndürür. Bu nesne çeşitli türlerdeki uygun içerik için bayraklar ve puanlar, ayrıca tüm uygunsuz terimlerin listesini içerir. Çağrıyı yapan (`CreateVideoReviewInContentModerator()`) incelemeyi yapan insanların kullanabilmesi için `UploadScreenTextResult()` çağrısı yaparak bu bilgiyi incelemeye ekler.
 
## <a name="video-review-transcript-view"></a>Video inceleme transkript görünümü

Aşağıdaki ekranda transkript oluşturma ve denetim adımlarının sonucu gösterilir.

![Video denetimi transkript görünümü](images/video-tutorial-transcript-view.PNG)

## <a name="program-output"></a>Program çıktısı

Programın aşağıdaki komut satırı çıkışı tamamlanan çeşitli görevleri gösterir. Buna ek olarak, özgün video dosyalarıyla aynı dizinde denetim sonucu (JSON biçiminde) ve konuşma transkripti de sağlanır.

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

Bu öğretici için [Visual Studio çözümünü indirin](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp). Ayrıca örnek dosyalarıyla gerekli kitaplıkları da indirin ve tümleştirmenizi başlatın.
