---
title: Yüz API ile gerçek zamanlı video çözümleme | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Yüz API Bilişsel Hizmetleri'nde canlı bir video akıştan geçen çerçeveler yakın gerçek zamanlı analiz gerçekleştirmek için kullanın.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 8675f992ddffe2eedfeac294a6c57560434802c2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352475"
---
# <a name="how-to-analyze-videos-in-real-time"></a>Videolar gerçek zamanlı olarak analiz etme
Bu kılavuz canlı bir video akıştan geçen çerçeveler yakın gerçek zamanlı analiz gerçekleştirme gösterilmektedir. Böyle bir sistemin temel bileşenler şunlardır:
- Bir video kaynağı çerçevelerden Al
- Analiz etmek için hangi çerçevelerini seçin
- Bu çerçeve API gönderme
- Döndürülen her Çözümleme sonucunda tüketen API çağrısından

Bu örnekler C# dilinde yazılmıştır ve kodu Github'da şurada bulunabilir: [ https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis ](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/).

## <a name="the-approach"></a>Yaklaşım
Yakın gerçek zamanlı analiz video akışları üzerinde çalışan sorunu çözmek için birden çok yolu vardır. Biz açıdan çok yönlülük düzeylerini artan üç yaklaşımlar anahat oluşturma tarafından başlatılır.

### <a name="a-simple-approach"></a>Basit bir yaklaşım
En basit tasarım yakın gerçek zamanlı analiz sistemi burada her yinelemede biz bir çerçeve alın, analiz edin ve ardından sonucu kullanmasına sonsuz bir döngüde şöyledir:
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
Basit bir istemci-tarafı algoritması bizim analiz oluşmuştur ise, bu yaklaşım uygun olabilir. Bizim analiz bulutta yapılırken, ancak, söz konusu Gecikmeli bir API çağrısı birkaç saniye sürebilir, hangi sırada biz görüntüleri yakalama değil, ve bizim iş parçacığı temelde hiçbir şey yapmakta olduğu anlamına gelir. Bizim maksimum kare hızı API çağrılarını gecikmesine sınırlıdır.

### <a name="parallelizing-api-calls"></a>Parallelizing API çağrıları
Basit bir istemci-tarafı algoritması için basit bir tek iş parçacıklı döngüsü anlamlı olsa da, bulut API çağrılarında söz konusu gecikme süresi ile iyi uygun değil. Bu sorun için çözüm çerçeve kapmasını ile paralel yürütmek uzun süre çalışan API çağrıları izin vermektir. C# ' ta biz bunu görev tabanlı paralellik, örneğin kullanarak elde:
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
Bu, her analizi yeni çerçeveler kapmasını devam ederken, arka planda çalıştırabilirsiniz ayrı bir görev başlatır. Bu, ana iş parçacığı sağlanan--Basit sürüme dönmek, ancak biz garanti bazıları, kaybetmiş bir API çağrısı için beklenirken birden çok API çağrıları paralel olarak ortaya çıkabilir ve sonuçları yanlış sırayla döndüren engelleme önler. Bu ayrıca hangi işlev iş parçacığı açısından güvenli değilse tehlikeli olabilecek ConsumeResult() işlevi aynı anda girmek birden çok iş parçacığı neden olabilir. Son olarak, bu basit kod oluşturulmasına, görevleri özel durumlar sessizce kaybolur şekilde izlemek değil. Bu nedenle, bize eklemek için son tarifi analiz görevleri izlemek, özel durumlar yükseltmek, uzun süre çalışan görevler sonlandırılır ve sonuçları doğru sırada, bir kerede tüketilen olun bir "tüketici" iş parçacığıdır.

### <a name="a-producer-consumer-design"></a>Üretici-tüketici tasarım
Son "üretici-tüketici" sistemimizde bizim önceki sonsuz bir döngü için çok benzer bir üretici iş parçacığı vardır. Ancak, kullanılabilir duruma geldiğinde çözümleme sonuçlarını kullanma yerine, üretici yalnızca görevleri bunları izlemek için sıraya koyar.
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
Ayrıca bunları tamamlanmasını bekleyen sıranın görevleri sürüyor tüketici iş parçacığı, sonucu görüntülenirken veya oluşturulan özel durum yükseltme sahibiz. Sıranın kullanarak sonuçları tüketilen birer birer, doğru sırada en fazla kare hızı sisteminin sınırlamaksızın almanızı garanti ediyoruz.
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

## <a name="implementing-the-solution"></a>Çözüm uygulama
### <a name="getting-started"></a>Başlarken
Uygulamanızı ve mümkün olan en kısa sürede çalışır duruma için biz, yukarıda açıklanan sistem kullanımı kolay devam ederken pek çok senaryoyu uygulamak için esnek olmasını planlayan uyguladık. Kod erişmek için şu adrese gidin [ https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis ](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis).

Kitaplık bir Web kamerası video çerçevelerden işlemek için yukarıda açıklanan üretici-tüketici sistem uygulayan FrameGrabber sınıfı içerir. Kullanıcı API çağrısı tam biçimini belirtebilir ve ne zaman yeni bir çerçeve aldığınız veya yeni bir Çözümleme sonucunda kullanılabilir bilmeniz çağıran kodu izin vermek için olayları sınıfı kullanır.

Bazı özellikleri göstermek için kitaplığını kullanan iki örnek uygulamaları vardır. İlk basit konsol uygulamasıdır ve bu basitleştirilmiş bir sürümünü aşağıda çoğaltılamaz. Varsayılan Web kamerası çerçevelerden alan ve bunları yüz algılama yüz API gönderir.
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
İkinci örnek uygulama biraz daha ilginç ve video çerçeve çağırmak için hangi API seçmenize olanak sağlar. Sol tarafta uygulama sağ tarafındaki canlı video önizlemesi, karşılık gelen çerçevesinde yayılan en son API'si sonucu gösterir.

Çoğu modlarında olacaktır görünür bir gecikme canlı video sol ve sağ taraftaki görselleştirilmiş çözümleme arasında. Bu gecikme API çağrısı yapmak için harcanan süre ' dir. Bu yüz algılama Bilişsel hizmetler için tüm görüntüleri göndermeden önce OpenCV, kullanarak istemci bilgisayarda yerel olarak gerçekleştirir. "EmotionsWithClientFaceDetect" modunda istisnadır. Bunu yaparak, biz algılanan yüz hemen görselleştirmek ve API çağrısı döndüğünde duygular daha sonra güncelleştirin. Burada bazı basit işlem istemcide gerçekleştirilebilir ve bu daha gelişmiş analiz ile gerektiğinde büyütmek için Bilişsel hizmetler API'leri sonra kullanılabilir bir "karma" yaklaşım olasılığını gösterir.

![HowToAnalyzeVideo](../../Video/Images/FramebyFrame.jpg)

### <a name="integrating-into-your-codebase"></a>Temelinizde tümleştirme
Bu örnek kullanmaya başlamak için aşağıdaki adımları izleyin:

1. Görme API'lerden için API anahtarlarını almak [abonelikleri](https://azure.microsoft.com/try/cognitive-services/). Video çerçeve analizi için uygulanabilir API'leri şunlardır:
    - [Görüntü İşleme API’si](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
    - [Duygu tanıma API'si](https://docs.microsoft.com/azure/cognitive-services/emotion/home)
    - [Yüz Tanıma API’si](https://docs.microsoft.com/azure/cognitive-services/face/overview)
2. Kopya [Cognitive örnekleri VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/) GitHub depo

3. Visual Studio 2015'te örneği açın, yapı ve örnek uygulamaları çalıştırma:
    - Doğrudan sabit kodlanmış yüz API anahtarını BasicConsoleSample için [BasicConsoleSample/Program.cs](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/blob/master/Windows/BasicConsoleSample/Program.cs).
    - LiveCameraSample için anahtarları uygulama ayarlarını bölmesine girilmesi gerekir. Kullanıcı verileri olarak oturumlar arasında kalıcıdır.
        

Tümleştirmek hazır olduğunuzda **yalnızca kendi projelerden VideoFrameAnalyzer Kitaplığı Başvurusu.** 



## <a name="developer-code-of-conduct"></a>Geliştirici Kullanım Kuralları
Bilişsel hizmetler tüm ile bizim API'leri ve örnekleri ile geliştirme geliştiriciler izlemek için gerekli olan "[Geliştirici kullanım kuralları Microsoft Cognitive Hizmetleri için](https://azure.microsoft.com/support/legal/developer-code-of-conduct/)." 


Görüntü, ses, video veya metin VideoFrameAnalyzer yeteneklerini anlama Microsoft Bilişsel Hizmetleri kullanır. Microsoft, görüntüleri, ses, video ve diğer verileri (Bu uygulama) karşıya yükleyin ve bunları hizmeti geliştirme amaçlarıyla kullanabilir alırsınız. Verileri Microsoft Bilişsel hizmetler için uygulamanızı gönderir kişiler korumanın, Yardım isteyin. 


## <a name="summary"></a>Özet
Bu kılavuzda, yüz, bilgisayar görme ve duygu tanıma API'leri kullanarak canlı video akışları yakın gerçek zamanlı analiz gerçekleştirme ve nasıl başlayacağınızı bizim örnek kodu kullanabilirsiniz öğrendiniz.  Ücretsiz API anahtarlarda ile uygulamanızı oluşturma başlayabiliriz [Microsoft Bilişsel hizmetler kayıt sayfasına](https://azure.microsoft.com/try/cognitive-services/). 

Lütfen görüş ve öneriler de sağlar çekinmeyin [GitHub deposunu](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/), veya daha fazla geniş API geri bildirim, üzerinde bizim [UserVoice sitesinde](https://cognitive.uservoice.com/).



## <a name="related"></a> İlgili Konular
- [Görüntüde yüzeyleri belirleme](HowtoIdentifyFacesinImage.md)
- [Görüntüde nasıl algılanacağını yüzler](HowtoDetectFacesinImage.md)
