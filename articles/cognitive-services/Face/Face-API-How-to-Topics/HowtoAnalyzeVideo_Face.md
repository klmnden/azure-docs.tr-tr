---
title: "Örnek: Gerçek zamanlı video analizi - yüz tanıma API'si"
titleSuffix: Azure Cognitive Services
description: Canlı video akışından alınan karelerde gerçek zamanlıya yakın analiz gerçekleştirmek için Yüz Tanıma API’sini kullanın.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 936c516385c88191428a46d22c14b3991885340b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60816176"
---
# <a name="example-how-to-analyze-videos-in-real-time"></a>Örnek: Gerçek Zamanlı Videoları Analiz Etme

Bu kılavuzda, canlı video akışından alınan karelerde nasıl gerçek zamanlıya yakın analiz gerçekleştirileceği gösterilmektedir. Böyle bir sistemdeki temel bileşenler şunlardır:

- Video kaynağından kareleri alma
- Hangi karelerin analiz edileceğini seçme
- Bu kareleri API’ye gönderme
- API çağrısından döndürülen her analiz sonucunu kullanma

Bu örnekler C# dilinde yazılmıştır ve kod, buradaki GitHub’da bulunabilir: [https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/).

## <a name="the-approach"></a>Yaklaşım

Video akışlarında gerçek zamanlıya yakın analiz çalıştırma sorununu çözmenin birçok yolu vardır. Gelişmişlik düzeyini artırma konusunda üç yaklaşımı açıklayarak başlayacağız.

### <a name="a-simple-approach"></a>Basit Bir Yaklaşım

Neredeyse gerçek zamanlı analiz sistemi için en basit tasarım sonsuz döngü burada her yinelemede çerçeve Dallarınızla çözümlediği ve ardından sonucu tüketir şöyledir:

```CSharp
while (true)
{
    Frame f = GrabFrame();
    if (ShouldAnalyze(f))
    {
        AnalysisResult r = await Analyze(f);
        ConsumeResult(r);
    }
}
```

Analizimiz hafif bir istemci algoritmasından oluştuysa bu yaklaşım uygun olacaktır. Ancak, analiz bulutta gerçekleştiğinde, gecikme süresi dahil olan bir API çağrısı birkaç saniye sürebilir anlamına gelir. Bu süre boyunca biz görüntüleri yakalama değil ve müşterilerimizin iş parçacığı aslında hiçbir şey yapıyor. Maksimum kare hızımız, API çağrılarının gecikme süresiyle sınırlıdır.

### <a name="parallelizing-api-calls"></a>API Çağrılarını Paralelleştirme

Basit bir tek iş parçacıklı döngü, hafif istemci tarafı algoritma için mantıklı olsa da, bulut API çağrılarında yer alan gecikme süresine tam uymaz. Bu sorunun çözümü, uzun süre çalıştırılan API çağrılarının kare ele geçirme ile paralel şekilde yürütülmesine izin verilmesidir. C# dilinde, Görev tabanlı paralellik kullanarak bunu elde edebiliriz; örneğin:

```CSharp
while (true)
{
    Frame f = GrabFrame();
    if (ShouldAnalyze(f))
    {
        var t = Task.Run(async () => 
        {
            AnalysisResult r = await Analyze(f);
            ConsumeResult(r);
        }
    }
}
```

Bu kod her bir analizi ayrı bir Görevde başlatır ve biz yeni kareleri ele geçirirken görev arka planda çalıştırılabilir. Bu yöntemle biz döndürmek bir API çağrısı için beklenirken ana iş parçacığı engellemekten kaçınacak ancak biz bazı basit sürümü sağlanan garantileri kaybetmiş. Paralel olarak birden çok API çağrısı oluşabilir ve sonuçları yanlış sırada döndürülen. Bu işlev, iş parçacığı güvenli değilse, tehlikeli olabilecek ConsumeResult() işlevi eşzamanlı olarak girmeyi birden çok iş parçacığı da neden olabilir. Son olarak bu basit kod, oluşturulan Görevleri takip etmez, bu nedenle özel durumlar sessizce kaybolur. Bu nedenle, son adım analiz görevlerini izlemek, yükseltmek özel durumlar, uzun süre çalışan görevleri sonlandırın ve sonuçları doğru sırayla tüketilen emin olun "Müşteri" iş parçacığı eklemektir.

### <a name="a-producer-consumer-design"></a>Üretici-Tüketici Tasarımı

Son “üretici-tüketici” sistemimizde, önceki sonsuz döngümüze benzer görünen bir üretici iş parçacığımız vardır. Ancak üretici, kullanılabilir olduğu anda analiz sonuçlarını kullanmak yerine görevleri takip etmek için bir kuyruğa koyar.

```CSharp
// Queue that will contain the API call tasks. 
var taskQueue = new BlockingCollection<Task<ResultWrapper>>();
     
// Producer thread. 
while (true)
{
    // Grab a frame. 
    Frame f = GrabFrame();
 
    // Decide whether to analyze the frame. 
    if (ShouldAnalyze(f))
    {
        // Start a task that will run in parallel with this thread. 
        var analysisTask = Task.Run(async () => 
        {
            // Put the frame, and the result/exception into a wrapper object.
            var output = new ResultWrapper(f);
            try
            {
                output.Analysis = await Analyze(f);
            }
            catch (Exception e)
            {
                output.Exception = e;
            }
            return output;
        }
        
        // Push the task onto the queue. 
        taskQueue.Add(analysisTask);
    }
}
```

Kuyruk görevleri alır, bunları tamamlanması ve görüntüler için sonucu bekler ya da oluşan özel durum harekete bir tüketicinin iş parçacığıyla de sahibiz. Kuyruğu kullanarak, sistemin maksimum kare hızını sınırlamadan sonuçların birer birer doğru sırayla kullanılmasını garanti edebiliriz.

```CSharp
// Consumer thread. 
while (true)
{
    // Get the oldest task. 
    Task<ResultWrapper> analysisTask = taskQueue.Take();
 
    // Await until the task is completed. 
    var output = await analysisTask;
     
    // Consume the exception or result. 
    if (output.Exception != null)
    {
        throw output.Exception;
    }
    else
    {
        ConsumeResult(output.Analysis);
    }
}
```

## <a name="implementing-the-solution"></a>Çözümü uygulama

### <a name="getting-started"></a>Başlarken

Uygulamanızı ve mümkün olan en kısa sürede çalışır duruma için yukarıda açıklanan sistem esnek bir uyarlamasını kullanır. Koda erişmek için [https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis) sayfasına gidin.

Kitaplık uygulayan bir Web kamerası video çerçevelerden işlemek için yukarıda açıklanan üretici-tüketici sistem FrameGrabber sınıfı içerir. Sınıfı ne zaman yeni bir çerçeve elde veya yeni bir analiz sonucu kullanılabilir çağıran kod izin vermek için olayları kullanır ve kullanıcının tam API çağrısının biçimi belirtebilirsiniz.

Bazı olasılıkları göstermek için, kitaplığı kullanan iki örnek uygulama vardır. İlk basit bir konsol uygulamasıdır ve Basitleştirilmiş bir sürümünü aşağıda çoğaltılamaz. Varsayılan web kamerasından kareleri ele geçirir ve yüz algılama için Yüz Tanıma API’sine bunları gönderir.

```CSharp
using System;
using VideoFrameAnalyzer;
using Microsoft.ProjectOxford.Face;
using Microsoft.ProjectOxford.Face.Contract;
     
namespace VideoFrameConsoleApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create grabber, with analysis type Face[]. 
            FrameGrabber<Face[]> grabber = new FrameGrabber<Face[]>();
            
            // Create Face API Client. Insert your Face API key here.
            FaceServiceClient faceClient = new FaceServiceClient("<Subscription Key>");

            // Set up our Face API call.
            grabber.AnalysisFunction = async frame => return await faceClient.DetectAsync(frame.Image.ToMemoryStream(".jpg"));

            // Set up a listener for when we receive a new result from an API call. 
            grabber.NewResultAvailable += (s, e) =>
            {
                if (e.Analysis != null)
                    Console.WriteLine("New result received for frame acquired at {0}. {1} faces detected", e.Frame.Metadata.Timestamp, e.Analysis.Length);
            };
            
            // Tell grabber to call the Face API every 3 seconds.
            grabber.TriggerAnalysisOnInterval(TimeSpan.FromMilliseconds(3000));

            // Start running.
            grabber.StartProcessingCameraAsync().Wait();

            // Wait for keypress to stop
            Console.WriteLine("Press any key to stop...");
            Console.ReadKey();
            
            // Stop, blocking until done.
            grabber.StopProcessingAsync().Wait();
        }
    }
}
```

İkinci örnek uygulama biraz daha ilginçtir ve video karelerinde hangi API’yi çağıracağınızı seçmenize olanak sağlar. Sol kısımda uygulama, canlı videonun bir önizlemesini gösterir, sağ kısımdaysa ilgili karenin üzerine yerleştirilmiş şekilde en son API sonucunu gösterir.

Çoğu modda, soldaki canlı video ile sağdaki görselleştirilmiş analiz arasında görünür bir gecikme olacaktır. Bu gecikme, API çağrısı yapmak için geçen süredir. Bir özel durum yüz algılama OpenCV, Bilişsel hizmetler için herhangi bir görüntü göndermeden önce kullanarak istemci bilgisayarda yerel olarak gerçekleştirir. "EmotionsWithClientFaceDetect" modudur. Bu şekilde, biz algılanan yüz anında görselleştirin ve API çağrısı döndüğünde duyguları güncelleştirin. Bu, burada istemci bazı basit işlemler gerçekleştirebilir ve daha gelişmiş analiz ile gerekli olduğunda bu Bilişsel hizmetler API'leri genişletebilirsiniz "karma" yaklaşım, örneğidir.

![Video analiz etme](../../Video/Images/FramebyFrame.jpg)

### <a name="integrating-into-your-codebase"></a>Kod temelinizle tümleştirme

Bu örneği kullanmaya başlamak için şu adımları izleyin:

1. [Abonelikler](https://azure.microsoft.com/try/cognitive-services/)’den Görüntü İşleme API’leri için API anahtarlarını alın. Video karesi analizi için geçerli API’ler şunlardır:
    - [Görüntü İşleme API’si](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
    - [Duygu Tanıma API'si](https://docs.microsoft.com/azure/cognitive-services/emotion/home)
    - [Yüz Tanıma API’si](https://docs.microsoft.com/azure/cognitive-services/face/overview)

2. [Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/) GitHub deposunu kopyalayın

3. Visual Studio 2015'te örneği açın ve örnek uygulamalar oluşturup çalıştırın:
    - Doğrudan sabit kodlanmış BasicConsoleSample için yüz tanıma API'si anahtarı [BasicConsoleSample/Program.cs](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/blob/master/Windows/BasicConsoleSample/Program.cs).
    - LiveCameraSample için anahtarlar, uygulamanın Ayarlar bölmesine girilmelidir. Oturumlarda kullanıcı verileri olarak kalıcı duruma getirilir.
        

Tümleştirmek hazır olduğunuzda **kendi projelerden VideoFrameAnalyzer Kitaplığı Başvurusu.** 

## <a name="summary"></a>Özet

Bu kılavuzda, yüz tanıma, görüntü işleme ve duygu tanıma API'leri kullanarak canlı video akışları neredeyse gerçek zamanlı analiz gerçekleştirme ve kullanmaya başlamak için örnek kodumuz kullanmayı öğrendiniz. Uygulamanızı ücretsiz API anahtarları, yapı başlayabilirsiniz [Azure Bilişsel hizmetler kayıt sayfasına](https://azure.microsoft.com/try/cognitive-services/). 

Geri bildirim ve öneri iletmekten çekinmeyin [GitHub deposu](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/) veya daha geniş API görüş üzerinde bizim [UserVoice sitesinde](https://cognitive.uservoice.com/).

## <a name="related-topics"></a>İlgili Konular
- [Görüntüdeki Yüzleri Belirleme](HowtoIdentifyFacesinImage.md)
- [Görüntüdeki Yüzleri Algılama](HowtoDetectFacesinImage.md)
