---
title: Azure Media Indexer ortam dosyalarıyla dizin oluşturma
description: Azure Media Indexer medya dosyalarınızı içeriğini aranabilir yapmanıza ve kapalı açıklamalı alt yazı ve anahtar sözcükler için tam metin dökümü oluşturmak üzere sağlar. Bu konu, Media Indexer kullanmayı gösterir.
services: media-services
documentationcenter: ''
author: Asolanki
manager: cfowler
editor: ''
ms.assetid: 827a56b2-58a5-4044-8d5c-3e5356488271
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/20/2017
ms.author: adsolank;juliako;johndeu
ms.openlocfilehash: 320d8cfa21c36e0f0d751d29d461a0023ab563e7
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788910"
---
# <a name="indexing-media-files-with-azure-media-indexer"></a>Azure Media Indexer ortam dosyalarıyla dizin oluşturma
Azure Media Indexer medya dosyalarınızı içeriğini aranabilir yapmanıza ve kapalı açıklamalı alt yazı ve anahtar sözcükler için tam metin dökümü oluşturmak üzere sağlar. Tek bir medya dosyasını işleyebileceğiniz gibi, birden çok medya dosyasını toplu olarak da işleyebilirsiniz.  

> [!IMPORTANT]
> İçerik dizin oluştururken Temizle konuşma (olmadan, arka plan müzik, parazit, efektleri veya mikrofon hiss) sahip medya dosyalarını kullandığınızdan emin olun. Uygun içeriğin bazı örnekleri şunlardır: kaydedilen toplantılar, dersleri veya sunuları. Aşağıdaki içerik dizin oluşturma işlemi için uygun olmayabilir: filmler, TV programları karma ses ve ses efekti ile herhangi bir şey kötü içeriği arka plan gürültü (hiss) ile kaydedilmiş.
> 
> 

Bir dizin oluşturma işi aşağıdaki çıktıları oluşturabilirsiniz:

* Kapalı açıklamalı alt yazı dosyaları aşağıdaki biçimlerde: **SAMI**, **TTML**, ve **WebVTT**.
  
    Kapalı açıklamalı alt yazı dosyaları Recognizability, bir dizin oluşturma işini nasıl tanınabilir kaynak videoda konuşma açıktır hangi puanları adlı bir etiket içerir.  Kullanılabilirlik için ekran çıktı dosyaları için Recognizability değerini kullanabilirsiniz. Düşük bir puan ses kalitesi nedeniyle kötü dizin sonuçlar anlamına gelir.
* Anahtar dosyası (XML).
* Ses BLOB dosya (AIB) SQL server ile kullanmak için dizin oluşturma.
  
    Daha fazla bilgi için bkz: [kullanarak AIB dosyaları Azure Media Indexer ve SQL Server ile](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).

Bu makalede, dizin oluşturma işleri oluşturmak gösterilmiştir **bir varlık dizin** ve **birden çok dosya dizin**.

En son Azure Media Indexer güncelleştirmeler için bkz: [Media Services bloglar](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Dizin oluşturma görevlerini yapılandırması ve bildirim dosyalarını kullanma
Görev yapılandırmayı kullanarak, daha fazla ayrıntı için dizin oluşturma görevleri belirtebilirsiniz. Örneğin, ortam dosyası için kullanılacak hangi meta verilerin belirtebilirsiniz. Bu meta veriler dil altyapısı tarafından sözlük genişletmek için kullanılır ve konuşma tanıma doğruluğunu önemli ölçüde artırır.  Ayrıca, istenen çıkış dosyalarını belirtmek üzere kullanabilirsiniz.

Bildirim dosyası kullanarak birden çok medya dosyaları aynı anda aynı zamanda işleyebilir.

Daha fazla bilgi için bkz: [görev Önayar Azure Media Indexer için](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Bir varlık dizin
Aşağıdaki yöntem bir medya dosyası bir varlık olarak yükler ve varlık dizini oluşturmak için bir iş oluşturur.

Medya dosyası, herhangi bir yapılandırma dosyası belirtilmişse, tüm varsayılan ayarlarla dizine alınır.

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
### <a id="output_files"></a>Çıkış dosyaları
Varsayılan olarak, bir dizin oluşturma işi aşağıdaki çıkış dosyalarını oluşturur. Dosyaları ilk çıkış varlığına depolanır.

Birden fazla giriş medya dosyası olduğunda, Dizin Oluşturucu 'JobResult.txt' adlı iş çıktısı için bir bildirim dosyası oluşturur. Her giriş medya dosyası için sonuçta elde edilen AIB, SAMI, TTML, WebVTT anahtar sözcüğü dosyaları sıralı olarak numaralı ve adlandırılmış "Diğer adı." kullanma

| Dosya adı | Açıklama |
| --- | --- |
| **InputFileName.aib** |Ses dizin blob dosyası. <br/><br/> Ses dizin Blob (AIB) dosyası Microsoft SQL server tam metin araması kullanılarak aranabilecek bir ikili dosyadır.  Çok daha zengin bir arama deneyimi sağlayan her sözcüğün alternatifleri içerdiğinden AIB basit açıklamalı alt yazı dosyaları, daha güçlü dosyasıdır. <br/> <br/>Dizin Oluşturucu SQL eklenti makine çalışan Microsoft SQL server 2008 veya sonraki sürümünü üzerinde yüklenmesini gerektirir. Microsoft SQL kullanarak AIB arama server tam metin araması WAMI tarafından oluşturulan kapalı açıklamalı alt yazı dosyaları arama değerinden daha doğru arama sonuçları sağlar. AIB kapalı açıklamalı alt yazı dosyaları her ses parçası için yüksek güvenilirlik word içerirken, benzer ses word alternatifleri içermesidir. Konuşma sözcükler aranıyor upmost önemini ise, Microsoft SQL Server ile AIB içinde birlikte kullanmak için önerilir.<br/><br/> Eklentiyi karşıdan yüklemek için tıklatın <a href="http://aka.ms/indexersql">Azure Media Indexer SQL eklenti</a>. <br/><br/>Diğer arama motorları gibi Apache Lucene/yalnızca kapalı açıklamalı alt yazı ve anahtar sözcüğü XML dosyaları göre video dizine Solr kullanmak da mümkündür, ancak bu daha az doğru arama sonuçlarında neden olur. |
| **InputFileName.smi**<br/>**InputFileName.ttml**<br/>**InputFileName.vtt** |SAMI, TTML ve WebVTT biçimleri kapalı açıklamalı alt yazı (CC) dosyalar.<br/><br/>Ses ve video dosyaları işitme engelli kişiler için erişilebilir hale getirmek için kullanılabilir.<br/><br/>Kapalı açıklamalı alt yazı dosyaları dahil etme adlı bir etiket <b>Recognizability</b> kaynak videoda konuşma nasıl tanınabilir olduğuna bağlı olarak bir dizin oluşturma işi puanlar.  Değerini kullanabilirsiniz <b>Recognizability</b> için kullanılabilirlik ekran çıktı dosyaları. Düşük bir puan ses kalitesi nedeniyle kötü dizin sonuçlar anlamına gelir. |
| **InputFileName.kw.xml<br/>InputFileName.info** |Anahtar sözcüğü ve bilgi dosyaları. <br/><br/>Anahtar sözcük dosyasını sıklığı ve uzaklık bilgileri konuşma içerikten ayıklanan anahtar sözcükler içeren bir XML dosyasıdır. <br/><br/>Bilgi dosyası tanınan her terim hakkında ayrıntılı bilgi içeren bir düz metin dosyasıdır. İlk satırı özeldir ve Recognizability puan içerir. Sonraki her satırı aşağıdaki veriler sekmeyle ayrılmış bir listesi verilmiştir: başlangıç zamanı, bitiş zamanı, sözcük/tümcecik, güven. Saniye cinsinden kez verilir ve güven 0-1'den bir sayı olarak verilir. <br/><br/>Örnek satırı: "1.20 1.45 word 0.67" <br/><br/>Bu dosyalar amaçlar için bir sayı gibi konuşma analiz gerçekleştirmek için kullanılan veya daha çok ilgili reklam sunmak için kullanılan medya dosyalarını daha bulunabilir veya hatta yapmak için Bing, Google veya Microsoft SharePoint gibi arama motorları için kullanıma sunulan. |
| **JobResult.txt** |Çıktı bildirimi, yalnızca aşağıdaki bilgileri içeren birden çok dosya dizin oluştururken mevcut:<br/><br/><table border="1"><tr><th>InputFile</th><th>Diğer ad</th><th>MediaLength</th><th>Hata</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/> |

Aksi takdirde tüm giriş medya dosyalarını başarıyla dizini, dizin oluşturma işi 4000 hata koduyla başarısız. Daha fazla bilgi için bkz: [hata kodları](#error_codes).

## <a name="index-multiple-files"></a>Birden çok dosya dizin
Aşağıdaki yöntem bir varlık birden çok medya dosyalarını yükler ve bu dosyaların toplu dizinini oluşturmak için bir iş oluşturur.

Bir bildirim dosyası ".lst" uzantısı ile oluşturulan ve varlık içine karşıya yükleme. Bildirim dosyası tüm varlık dosyaların listesini içerir. Daha fazla bilgi için bkz: [görev Önayar Azure Media Indexer için](https://msdn.microsoft.com/library/dn783454.aspx).

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

### <a name="partially-succeeded-job"></a>Kısmen başarılı oldu işi
Aksi takdirde tüm giriş medya dosyalarını başarıyla dizini, dizin oluşturma işi 4000 hata koduyla başarısız olur. Daha fazla bilgi için bkz: [hata kodları](#error_codes).

Aynı çıkışları (başarılı işler) olarak üretilir. Giriş dosyaları, hata sütun değerlerini göre başarısız anlamak için çıktı bildirim dosyası başvurabilirsiniz. Başarısız oldu, girdi dosyaları için sonuçta elde edilen AIB, SAMI, TTML, WebVTT ve anahtar sözcüğü dosyaları üretilmez.

### <a id="preset"></a> Azure Media Indexer için görev Önayar
Azure Media Indexer işlenmesini görev Önayar bir isteğe bağlı görev sağlayarak özelleştirilebilir.  Bu yapılandırma xml biçimi açıklar.

| Ad | Gerektirme | Açıklama |
| --- | --- | --- |
| **Giriş** |false |Dizin istediğiniz varlık dosyaları.</p><p>Azure Media Indexer aşağıdaki ortam dosya biçimleri destekler: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>Dosya adı (s) belirleyebilirsiniz **adı** veya **listesi** özniteliği **giriş** (aşağıda gösterildiği gibi) öğesi. Hangi dizin varlık dosyasına belirtmezseniz, birincil dosya çekilir. Herhangi bir birincil varlık dosyası ayarlarsanız, ilk giriş varlık dosyasında dizine alınır.</p><p>Varlık dosya adı açıkça belirtmek için aşağıdakileri yapın:<br/>`<input name="TestFile.wmv">`<br/><br/>Ayrıca, birden çok varlık aynı anda (dosyaları en fazla 10) dizin oluşturabilirsiniz. Bunu yapmak için:<br/><br/><ol class="ordered"><li><p>Bir metin dosyası (bildirim dosyası) oluşturun ve bir .lst uzantısı verin. </p></li><li><p>Tüm varlık dosya adlarının bir listesini giriş Varlığınızı bildirim bu dosyaya ekleyin. </p></li><li><p>(Karşıya yükleme) thanifest dosyasını varlık için ekleyin.  </p></li><li><p>Bildirim dosyasının adı girdinin liste özniteliğinde belirtin.<br/>`<input list="input.lst">`</li></ol><br/><br/>Not: bildirim dosyası 10'dan fazla dosyaları eklerseniz, dizin oluşturma işi 2006 hata koduyla başarısız olur. |
| **Meta veriler** |false |Sözlük uyarlama için kullanılan belirli varlık dosyaları için meta veriler.  Uygun isimleri gibi standart olmayan sözlük sözcükler tanımak için dizin oluşturucu hazırlamak kullanışlıdır.<br/>`<metadata key="..." value="..."/>` <br/><br/>Size sağlayabilir **değerleri** önceden tanımlanmış **anahtarları**. Şu anda aşağıdaki anahtarları desteklenir:<br/><br/>"title" ve "Açıklama" dil ince ayar için Sözlük uyarlama için kullanılan - işiniz için model ve konuşma tanıma doğruluğunu artırmak.  Değerleri, iç sözlük dizin göreviniz süresince büyütmek üzere içeriği kullanarak bağlam ilgili metin belgeleri bulmak için Internet arama Çekirdek değer.<br/>`<metadata key="title" value="[Title of the media file]" />`<br/>`<metadata key="description" value="[Description of the media file] />"` |
| **Özellikleri** <br/><br/> Sürüm 1.2 eklendi. Şu anda, yalnızca desteklenen konuşma tanıma ("ASR") özelliğidir. |false |Konuşma tanıma özelliği şu ayarları anahtarlarını sahiptir:<table><tr><th><p>Anahtar</p></th>        <th><p>Açıklama</p></th><th><p>Örnek değer</p></th></tr><tr><td><p>Dil</p></td><td><p>Multimedya dosyasında tanınması için doğal dil.</p></td><td><p>İngilizce, İspanyolca</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>noktalı virgülle ayrılmış listesini istenen çıkış resim yazısı biçimleri (varsa)</p></td><td><p>ttml;sami;webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Bir AIB dosyası (kullanmak için SQL Server ve müşteri dizin oluşturucu IFilter ile) gerekli olup olmadığını belirten bir Boole bayrağı.  Daha fazla bilgi için bkz: <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">kullanarak AIB dosyaları Azure Media Indexer ve SQL Server ile</a>.</p></td><td><p>TRUE; False</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Bir anahtar sözcük XML dosyasını gerekli olup olmadığını belirten bir Boole bayrağı.</p></td><td><p>TRUE; FALSE. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Tam resim yazıları (bağımsız olarak anlamlılık düzeyi) zorla gerekip gerekmediğini belirten bir Boole bayrağı.  </p><p>Varsayılan; bu durumda sözcükleri ve % 50 anlamlılık düzeyi'den az olan tümcecikleri son resim yazısı çıkışlarından atlanmış ve üç nokta tarafından değiştirildi ("...") false değeridir.  Üç nokta resim yazısı kalite denetimi ve denetim için faydalıdır.</p></td><td><p>TRUE; FALSE. </p></td></tr></table> |

### <a id="error_codes"></a>Hata kodları
Bir hata olması durumunda, Azure Media Indexer bildirmelisiniz aşağıdaki hata kodlarından birini yedekleyin:

| Kod | Ad | Olası nedenler |
| --- | --- | --- |
| 2000 |Geçersiz yapılandırma |Geçersiz yapılandırma |
| 2001 |Geçersiz giriş varlıklar |Giriş varlıklar veya boş varlık eksik. |
| 2002 |Geçersiz bildirim |Bildirim boş veya geçersiz öğeleri bildirimi içerir. |
| 2003 |Medya dosyası karşıdan yüklenemedi |Bildirim dosyasında geçersiz URL. |
| 2004 |Desteklenmeyen protokol |Medya URL'nin protokolü desteklenmiyor. |
| 2005 |Desteklenmeyen dosya türü |Giriş medya dosya türü desteklenmiyor. |
| 2006 |Çok fazla giriş dosyaları |Giriş bildiriminde 10'dan fazla dosyası yok. |
| 3000 |Medya dosyası çözülemedi |Desteklenmeyen medya codec <br/>or<br/> Bozuk medya dosyası <br/>or<br/> Giriş medya ses akış yok. |
| 4000 |Toplu dizin oluşturma kısmen başarılı oldu |Bazı giriş medya dosyaları dizine başarısız. Daha fazla bilgi için bkz: <a href="#output_files">çıkış dosyaları</a>. |
| diğer |İç hata |Lütfen destek ekibi ile iletişime geçin. indexer@microsoft.com |

## <a id="supported_languages"></a>Desteklenen diller
Şu anda İngilizce ve İspanyolca dil desteklenir. Daha fazla bilgi için bkz: [1.2 sürümü yayın blog yayını](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Media Services Analytics a genel bakış](media-services-analytics-overview.md)

[Azure Media Indexer ve SQL Server ile AIB dosyalarını kullanma](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Azure Media Indexer 2 Önizleme ile medya dosyalarını dizin oluşturma](media-services-process-content-with-indexer2.md)

