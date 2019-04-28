---
title: Azure Media Indexer ile medya dosyalarının dizinini oluşturarak
description: Azure Media Indexer, medya dosyalarınızın içeriklerini aranabilir yapmanıza ve Kapalı Açıklamalı Altyazı veya anahtar sözcükler için bir tam metin dökümü oluşturmak için sağlar. Bu konu, Media Indexer'ı kullanmayı gösterir.
services: media-services
documentationcenter: ''
author: Asolanki
manager: femila
editor: ''
ms.assetid: 827a56b2-58a5-4044-8d5c-3e5356488271
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2019
ms.author: adsolank;juliako;johndeu
ms.openlocfilehash: a11ae0414d6737f1588515ec19524bcf499f0c74
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61215814"
---
# <a name="indexing-media-files-with-azure-media-indexer"></a>Azure Media Indexer ile medya dosyalarının dizinini oluşturarak
Azure Media Indexer, medya dosyalarınızın içeriklerini aranabilir yapmanıza ve Kapalı Açıklamalı Altyazı veya anahtar sözcükler için bir tam metin dökümü oluşturmak için sağlar. Tek bir medya dosyasını işleyebileceğiniz gibi, birden çok medya dosyasını toplu olarak da işleyebilirsiniz.  

> [!IMPORTANT]
> İçerik dizinleme, NET konuşma (olmadan, arka plan müzik, parazit, etkileri veya mikrofon hiss) sahip medya dosyalarını kullanan emin olun. Uygun içerik bazı örnekleri şunlardır: kaydedilen toplantılar, dersler ve sunumlar. Aşağıdaki içerik dizinleme için uygun olmayabilir: filmler, TV programları, sonraki her şey karma ses ve ses efektleri ile kötü içeriği (hiss) arka plan gürültüsü ile kaydedilmiş.
> 
> 

Bir dizin oluşturma işi aşağıdaki çıktıları oluşturabilirsiniz:

* Kapalı açıklamalı alt yazı dosyaları şu biçimde: **LAPONCA**, **TTML**, ve **WebVTT**.
  
    Kapalı açıklamalı alt yazı dosyaları Recognizability, bir dizin oluşturma işi nasıl tanınabilir kaynak videoda konuşma olduğuna bağlı olarak hangi puanları adlı bir etiket ekleyin.  Ekran çıktı dosyalarını kullanılabilirlik Recognizability değerini kullanabilirsiniz. Düşük bir puan ses kalitesi nedeniyle düşük dizin sonuçları anlamına gelir.
* Anahtar sözcük dosyası (XML).
* Ses BLOB dosyası (AIB) SQL server ile kullanmak için dizin oluşturma.
  
    Daha fazla bilgi için [kullanarak AIB dosyaları Azure Media Indexer'ın ve SQL Server ile](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).

Bu makalede, dizinleme işleri için oluşturma işlemi gösterilmektedir **bir varlık dizin** ve **birden çok dosya dizini**.

En yeni Azure Media Indexer'ın güncelleştirmeler için bkz: [medya Hizmetleri blog](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Görevleri dizin oluşturma için yapılandırması ve bildirim dosyalarını kullanma
Görev yapılandırması kullanarak daha fazla ayrıntı için dizin oluşturma görevlerinizi belirtebilirsiniz. Örneğin, medya dosyanızı kullanmak için hangi meta verileri belirtebilirsiniz. Bu meta veriler dil altyapısı tarafından sözlük genişletmek için kullanılır ve konuşma tanıma doğruluğu büyük ölçüde artırır.  Ayrıca, istenen çıkış dosyaları belirtebilirsiniz.

Bir bildirim dosyası kullanarak birden çok medya dosyasını tek seferde de işleyebilirsiniz.

Daha fazla bilgi için [Azure Media Indexer'ın için görev Önayar](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Bir varlık dizin
Aşağıdaki yöntem bir varlık olarak bir medya dosyasını yükler ve varlık dizin için bir iş oluşturur.

Medya dosyasını, herhangi bir yapılandırma dosyası belirtilmezse, tüm varsayılan ayarlarla dizine alınır.

```csharp
    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload the input media file to storage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

        // Use the following event handler to check job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }  
```

<!-- __ -->
### <a id="output_files"></a>Çıktı dosyaları
Varsayılan olarak, bir dizin oluşturma işi aşağıdaki çıkış dosyalarını oluşturur. Dosyaları ilk çıktı varlık içinde depolanır.

Birden fazla girdi medya dosyası olduğunda, Dizin Oluşturucu 'JobResult.txt' adlı iş çıktıları için bir bildirim dosyası oluşturur. Medya dosyası her giriş için elde edilen AIB, LAPONCA, TTML, WebVTT anahtar sözcüğü dosyaları sıralı olarak numaralı ve "diğer." kullanarak adlı

| Dosya adı | Açıklama |
| --- | --- |
| **InputFileName.aib** |Ses dizini oluşturma blob dosyası. <br/><br/> Ses dizini oluşturma Blob (AIB) dosyası Microsoft SQL server tam metin arama ile aranabilir bir ikili dosyadır.  Alternatifleri için çok daha zengin bir arama deneyimi sağlayan, her bir sözcüğün içerdiği için AIB dosyası basit bir açıklamalı alt yazı dosyaları, daha güçlüdür. <br/> <br/>Dizin Oluşturucu SQL eklentiye bir makine Microsoft SQL server 2008 veya üzeri yüklenmesini gerektiriyor. Arama AIB kullanarak Microsoft SQL server tam metin araması WAMI tarafından oluşturulan kapalı açıklamalı alt yazı dosyaları arama değerinden daha doğru arama sonuçlarını sağlar. AIB kapalı açıklamalı alt yazı dosyaları ses her bir kesim için en yüksek güvenilirlik word içerirken, benzer ses alternatifleri word içerdiğinden budur. Konuşulan kelimeleri için arama upmost önemini ise, Microsoft SQL Server ile AIB içinde birlikte kullanmak için önerilir.<br/><br/> Eklentiyi yüklemek için tıklatın <a href="https://aka.ms/indexersql">Azure Media Indexer SQL eklenti</a>. <br/><br/>Diğer arama motorları gibi Apache Lucene/yeterlidir kapalı açıklamalı alt yazı ve anahtar sözcük XML dosyaları göre video dizini oluşturmak için Solr kullanmak da mümkündür, ancak bu daha az doğru arama sonuçlarında neden olur. |
| **InputFileName.smi**<br/>**InputFileName.ttml**<br/>**InputFileName.vtt** |LAPONCA, TTML ve WebVTT biçimindeki kapalı açıklamalı alt yazı (CC) dosyaları.<br/><br/>Ses ve video dosyaları işitme engelli kişiler için erişilebilir hale getirmek için kullanılabilir.<br/><br/>Kapalı açıklamalı alt yazı dosyaları dahil adlı bir etiket <b>Recognizability</b> nasıl tanınabilir kaynak videoda konuşma olduğuna göre bir dizin oluşturma işi puanlar.  Değerini kullanabilir <b>Recognizability</b> kullanılabilirlik için ekran çıktı dosyaları için. Düşük bir puan ses kalitesi nedeniyle düşük dizin sonuçları anlamına gelir. |
| **InputFileName.kw.xml<br/>InputFileName.info** |Anahtar sözcüğü ve bilgi dosyaları. <br/><br/>Anahtar sözcük dosyası sıklık ve uzaklık bilgilerini konuşma içeriğinden ayıklanan anahtar sözcükleri içeren bir XML dosyasıdır. <br/><br/>Bilgi dosyası tanınan her terim hakkında ayrıntılı bilgi içeren bir düz metin dosyasıdır. İlk satırı özeldir ve Recognizability puanı içerir. Sonraki her satırı aşağıdaki veriler sekmeyle ayrılmış bir listesi verilmiştir: başlangıç zamanı, bitiş zamanı, sözcüğün/tümceciğin, güvenilirlik. Saniyeler içinde bir kez verilir ve güvenle bir sayı olarak 0-1'den verilir. <br/><br/>Örnek satırı: "1.20    1.45    word    0.67" <br/><br/>Bu dosyalar amacıyla, bir dizi için gibi konuşma analizi yapmak için kullanılan veya daha fazla ilgili reklam sunmak için Bing, Google veya Microsoft SharePoint daha bulunabilir, ya da medya dosyalarını yapmak için kullanılan gibi arama motorları için kullanıma sunulan. |
| **JobResult.txt** |Çıkış bildirimi, aşağıdaki bilgileri içeren birden çok dosya yalnızca dizin oluşturulurken mevcut:<br/><br/><table border="1"><tr><th>InputFile</th><th>Diğer ad</th><th>MediaLength</th><th>Hata</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/> |

Aksi takdirde tüm giriş medya dosyalarını başarıyla dizin, dizin oluşturma işi 4000 hata koduyla başarısız oluyor. Daha fazla bilgi için [hata kodları](#error_codes).

## <a name="index-multiple-files"></a>Birden çok dosya dizini
Aşağıdaki yöntemi, bir varlık birden çok medya dosyalarını yükler ve bu dosyaları toplu dizin için bir iş oluşturur.

".Lst" uzantısına sahip bir bildirim dosyası, oluşturulur ve varlık içinde karşıya yükleniyor. Bildirim dosyası, tüm varlık dosyalarının listesini içerir. Daha fazla bilgi için [Azure Media Indexer'ın için görev Önayar](https://msdn.microsoft.com/library/dn783454.aspx).

```csharp
    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload to storage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all the asset file names and upload to storage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

        // Use the following event handler to check job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }
```

### <a name="partially-succeeded-job"></a>Kısmen başarılı işi
Aksi takdirde tüm giriş medya dosyalarını başarıyla dizin, dizin oluşturma işi 4000 hata koduyla başarısız olur. Daha fazla bilgi için [hata kodları](#error_codes).

Aynı çıkışları (olarak başarılı işler için) oluşturulur. Giriş dosyaları, hata sütun değerlerine göre başarısız bulmak için çıkış bildirimi dosyası başvurabilirsiniz. Başarısız oldu, giriş dosyaları için ortaya çıkan AIB, LAPONCA, TTML, WebVTT ve anahtar sözcüğü dosyaları üretilmez.

### <a id="preset"></a> Azure Media Indexer için hazır görev
Azure Media Indexer'ın işlenmesini yanı sıra görev Önayar isteğe bağlı bir görev sağlayarak özelleştirilebilir.  Bu yapılandırma xml biçimini açıklar.

| Ad | Gerektirme | Açıklama |
| --- | --- | --- |
| **Giriş** |false |Dizine eklemek istediğiniz varlık dosyaları.</p><p>Azure Media Indexer, aşağıdaki ortam dosya biçimlerini destekler: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>Dosya adı (s) belirleyebilirsiniz **adı** veya **listesi** özniteliği **giriş** (aşağıda gösterildiği gibi) öğesi. Birincil dosya dizini için hangi varlık dosyası belirtmezseniz çekilir. Hiçbir birincil varlık dosyası olarak ayarlanırsa ilk giriş varlığı dosyasında dizine alınır.</p><p>Varlık dosyası adı açıkça belirtmek için aşağıdakileri yapın:<br/>`<input name="TestFile.wmv">`<br/><br/>Ayrıca, birden çok varlık dosyaları aynı anda (en fazla 10) de dizine ekleyebilir. Bunu yapmak için:<br/><br/><ol class="ordered"><li><p>Bir metin dosyası (bildirim dosyasını) oluşturun ve .lst uzantısı verin. </p></li><li><p>Tüm varlık dosya adlarının bir listesini giriş varlığınız bu bildirim dosyasına ekleyin. </p></li><li><p>Varlık için (yükleme) bildirim dosyası ekleyin.  </p></li><li><p>Bildirim dosyasının adını girdinin listesi özniteliğini belirtin.<br/>`<input list="input.lst">`</li></ol><br/><br/>Not: Bildirim dosyası için 10'dan fazla dosyaları eklerseniz, dizin oluşturma işi 2006 hata kodu ile başarısız olur. |
| **Meta verileri** |false |Sözlük uyarlaması için kullanılan belirli varlık dosyaları için meta veriler.  Dizin Oluşturucu'nun standart sözlük sözcükleri uygun isimleri gibi tanı hazırlamak kullanışlıdır.<br/>`<metadata key="..." value="..."/>` <br/><br/>Verebilirsiniz **değerleri** önceden tanımlanmış **anahtarları**. Şu anda aşağıdaki anahtarları desteklenmektedir:<br/><br/>"title" ve "description" dil ince ayar için Sözlük uyarlaması için kullanılan - işiniz için model ve konuşma tanıma doğruluğunu artırın.  Değerleri, iç sözlük dizin göreviniz süresince artırmak için içeriği kullanma, bağlamsal ilgili metin belgeleri bulmak için Internet arama temel.<br/>`<metadata key="title" value="[Title of the media file]" />`<br/>`<metadata key="description" value="[Description of the media file] />"` |
| **Özellikleri** <br/><br/> Sürüm 1.2 eklendi. Şu anda, yalnızca desteklenen konuşma tanıma ("ASR") özelliğidir. |false |Konuşma tanıma özelliği, aşağıdaki ayarları anahtarlarını sahiptir:<table><tr><th><p>Anahtar</p></th>        <th><p>Açıklama</p></th><th><p>Örnek değer</p></th></tr><tr><td><p>Dil</p></td><td><p>Multimedya dosyasında tanınacak doğal dili.</p></td><td><p>İngilizce, İspanyolca</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>istenen çıkış alt yazı biçimleri (varsa) noktalı virgülle ayrılmış listesi</p></td><td><p>ttml;sami;webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>AIB dosyası (SQL Server ve müşteri dizin oluşturucu IFilter ile kullanın için) gerekli olup olmadığını belirten bir Boole bayrağı.  Daha fazla bilgi için <a href="https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">kullanarak AIB dosyaları Azure Media Indexer'ın ve SQL Server ile</a>.</p></td><td><p>TRUE; False</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Bir anahtar sözcük XML dosyası gerekli olup olmadığını belirten bir Boole bayrağı.</p></td><td><p>TRUE; FALSE. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>(Güven düzeyi) bağımsız olarak tam açıklamalı alt yazılar zorla gerekip gerekmediğini belirten bir Boole bayrağı.  </p><p>Bu durumda sözcük ve tümcecikleri % 50 güvenilirlik düzeyi küçüktür olan son açıklamalı alt yazı çıkışları atlanmış ve elipsler tarafından değiştirildi ("..."), varsayılan false değeridir.  Üç nokta simgesini, açıklamalı alt yazı kalite kontrol ve denetim için kullanışlıdır.</p></td><td><p>TRUE; FALSE. </p></td></tr></table> |

### <a id="error_codes"></a>Hata kodları
Bir hata olması durumunda, Azure Media Indexer'ın bildirmelisiniz aşağıdaki hata kodları birini yedekleyin:

| Kod | Ad | Olası nedenler |
| --- | --- | --- |
| 2000 |Geçersiz yapılandırma |Geçersiz yapılandırma |
| 2001 |Geçersiz giriş varlıklar |Giriş varlıklar veya boş varlık eksik. |
| 2002 |Bildirim geçersiz |Bildirimi boş veya geçersiz bir öğe bildirimi içeriyor. |
| 2003 |Medya dosyası indirilemedi. |Bildirim dosyasında geçersiz URL. |
| 2004 |Desteklenmeyen Protokolü |Ortam URL'si protokolü desteklenmiyor. |
| 2005 |Desteklenmeyen dosya türü |Girdi medya dosya türü desteklenmiyor. |
| 2006 |Çok fazla giriş dosyaları |Giriş bildiriminde 10'dan fazla dosyası vardır. |
| 3000 |Medya dosyası kod çözülemedi |Desteklenmeyen ortam codec bileşeni <br/>or<br/> Bozuk bir medya dosyası <br/>or<br/> Giriş medya ses akışı yok. |
| 4000 |Toplu dizin oluşturma kısmen başarılı oldu |Bazı giriş medya dosyaları dizine getirilir. Daha fazla bilgi için <a href="#output_files">Çıkış dosyalarını</a>. |
| diğer |İç hata |Lütfen Destek ekibiyle iletişime geçin. indexer@microsoft.com |

## <a id="supported_languages"></a>Desteklenen diller
Şu anda İngilizce ve İspanyolca dil desteklenir. Daha fazla bilgi için [v1.2 sürüm blog gönderisini](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Media Services Analizi'ne genel bakış](media-services-analytics-overview.md)

[SQL Server ve Azure Media Indexer ile AIB dosyalarını kullanma](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Azure Media Indexer 2 Önizleme ile medya dosyalarının dizinini oluşturarak](media-services-process-content-with-indexer2.md)

