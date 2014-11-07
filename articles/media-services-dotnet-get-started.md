<properties urlDisplayName="Get Started with Media Services" pageTitle="Medya Hizmetlerini Kullanmaya Başlama - Azure" metaKeywords="Azure medya hizmetleri" description="An introduction to using Media Services with Azure." metaCanonical="" services="media-services" documentationCenter="" title="Get started with Media Services" authors="juliako" solutions="" manager="dwrede" editor="" />

<tags ms.service="media-services" ms.workload="media" ms.tgt_pltfrm="na" ms.devlang="dotnet" ms.topic="article" ms.date="01/01/1900" ms.author="juliako" />





# <a name="getting-started"></a>Medya Hizmetlerini Kullanmaya Başlama

Bu öğretici, Azure Medya Hizmetleri'ni kullanarak yazılım geliştirmeye nasıl başlayacağınızı gösterir. Temel Medya Hizmetleri iş akışını ve Medya Hizmetleri geliştirmeye yönelik en genel programlama nesne ve görevlerini tanıtır. Bu öğreticiyi tamamladığınızda, karşıya yükleyip kodladığınız ve indirdiğiniz bir örnek medya dosyasını yürütebileceksiniz. Ya da kodlanmış varlığa göz atıp onu sunucuda yürütebilirsiniz. 

Bu öğreticideki kodları içeren C# Visual Studio projesini şuradan edinebilirsiniz: [İndirin](http://go.microsoft.com/fwlink/?linkid=253275).

Bu öğretici, şu temel adımlarda sizi eğitir:

* [Projenizi ayarlama](#Step1)
* [Medya Hizmetleri sunucu bağlamını alma](#Step2)
* [Bir varlık oluşturup o varlıkla ilişkili dosyalar Medya Hizmetleri'ne yükleme](#Step3)
* [Bir varlık kodlama ve çıkış varlığını indirme](#Step4)

## Önkoşullar
Bu eğitim ve Azure Medya Hizmetleri SDK'sına dayalı geliştirme işleri için aşağıdaki önkoşullar gereklidir.

- Yeni veya mevcut Azure aboneliğinde bir Medya Hizmetleri hesabı. Ayrıntılar için bkz. [Medya Hizmetleri Hesabı Oluşturma](http://go.microsoft.com/fwlink/?LinkId=256662).
- İşletim Sistemleri: Windows 7, Windows 2008 R2, Windows 8 veya üzeri.
- .NET Framework 4.5 veya .NET Framework 4.
- Visual Studio 2012, Visual Studio 2010 SP1 (Professional, Premium, Ultimate veya Express) ya da üzeri.
- **.NET için Azure SDK'sı**, **.NET için Azure Medya Hizmetleri SDK'sı** ve **OData V3 kitaplıkları için WCF Veri Hizmetleri 5.0** 'ı yükleyin ve [windowsazure.mediaservices Nuget](http://nuget.org/packages/windowsazure.mediaservices) paketini kullanarak projenize başvurular ekleyin. Aşağıdaki bölümde bu başvuruların nasıl yüklenip ekleneceği gösterilmiştir.

>[WACOM.NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınız olmalıdır. Hesabınız yoksa sadece birkaç dakikada ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz. <a href="http://www.windowsazure.com/tr-tr/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure Ücretsiz Deneme Sürümü</a>.

<h2><a id="Step1"></a>Projenizi ayarlama</h2>

1. Visual Studio 2012 veya Visual Studio 2010 SP1'de yeni bir C# Konsol Uygulaması oluşturun. **Ad**, **Konum** ve **Çözüm adı**'nı girin ve Tamam'a tıklayın. 

2. System.Configuration bütünleştirilmiş koduna başvuru ekleyin.

	**Başvuruları Yönet** iletişim kutusunu kullanarak başvurular eklemek için şunları yapın. **Çözüm Gezgini** 'ndeki **Başvurular**  düğümüne sağ tıklayın ve **Başvuru Ekle**'yi seçin. **Başvuruları Yönet**  iletişim kutusunda, uygun bütünleştirilmiş kodları seçin (bu durumda System.Configuration.)

3. Henüz yapmadıysanız <a href="http://nuget.org/packages/windowsazure.mediaservices">windowsazure.mediaservices Nuget</a> paketini kullanarak **.NET için Azure SDK'sı**.(Microsoft.WindowsAzure.StorageClient.dll), **.NET için Azure Medya Hizmetleri SDK'sı** (Microsoft.WindowsAzure.MediaServices.Client.dll) ve **OData V3 için WCF Veri Hizmetleri 5.0**  (Microsoft.Data.OData.dll) kitaplıklarına başvurular ekleyin. 

	Nuget kullanarak başvurular eklemek için şunları yapın. Visual Studio Ana Menü'de, ARAÇLAR -> Kitaplık Paket Yöneticisi -> Paket Yöneticisi Konsolu'nu seçin. Konsol penceresinde type <i>Install-Package [paket adı]</i> yazın ve Enter tuşuna basın (bu durum için şu komutu kullanın: <i>Install-Package windowsazure.mediaservices</i>.)

4. **app.config** dosyasına *appSettings* bölümü ekleyin ve Azure Medya Hizmetleri hesap adınıza ve hesap anahtarınıza ait değerleri ayarlayın. Medya Hizmetleri hesap adını ve hesap anahtarını hesap kurulum işlemi sırasında edinmiştiniz. Bu değerleri, Visual Studio projesindeki app.config dosyasında her ayarın değer özniteliğine ekleyin.

	> [WACOM.NOTE]
	> Visual Studio 2012'de, App.config dosyası varsayılan seçenek olarak eklenir. Visual Studio 2010'da, Uygulama Yapılandırma dosyasını elle eklemeniz gerekir.

	<pre><code>
	<configuration>
  	. . . 
  	<appSettings>
    	<add key="accountName" value="Add-Media-Services-Account-Name" />
    	<add key="accountKey" value="Add-Media-Services-Account-Key" />
  	</appSettings>
	</configuration>
	</code></pre>

5. Yerel makinenizde yeni bir klasör oluşturup supportFiles adını verin (bu örnekte supportFiles dosyası MediaServicesGettingStarted proje dizini altında bulunur.) Bu eğitime eşlik eden <a href="http://go.microsoft.com/fwlink/?linkid=253275">Proje</a> supportFiles dizinini içermektedir. Bu dizinin içeriğini supportFiles klasörünüze kopyalayabilirsiniz.

6. Program.cs dosyasının başındaki deyimleri kullanarak aşağıdaki kodu mevcut olanın üzerine yazın.

		using System;
		using System.Linq;
		using System.Configuration;
		using System.IO;
		using System.Text;
		using System.Threading;
		using System.Threading.Tasks;
		using System.Collections.Generic;
		using Microsoft.WindowsAzure;
		using Microsoft.WindowsAzure.MediaServices.Client;


7. Aşağıdaki sınıf düzeyi yol değişkenlerini ekleyin. **_supportFiles** yolu önceki adımda oluşturduğunuz klasörü göstermelidir. 

		// Base support files path.  Update this field to point to the base path  
		// for the local support files folder that you create. 
		private static readonly string _supportFiles =
		            Path.GetFullPath(@"../..\supportFiles");
		
		// Paths to support files (within the above base path). You can use 
		// the provided sample media files from the "supportFiles" folder, or 
		// provide paths to your own media files below to run these samples.
		private static readonly string _singleInputFilePath =
		    Path.GetFullPath(_supportFiles + @"\multifile\interview2.wmv");
		private static readonly string _outputFilesFolder =
		    Path.GetFullPath(_supportFiles + @"\outputfiles");
		
8. Kimlik doğrulama ve bağlantı ayarlarını almak için aşağıdaki sınıf düzeyi değişkenlerini ekleyin.  Bu ayarlar App.Config dosyasından çekilir ve Medya Hizmetleri'ne bağlanmak, kimlik doğrulamak ve sunucu bağlamına erişebilmek üzere bir belirteç almak için gereklidirler. Projedeki kod, sunucu bağlamının bir örneğini oluşturmak için bu değişkenlere başvurur.
	
		private static readonly string _accountKey = ConfigurationManager.AppSettings["accountKey"];
		private static readonly string _accountName = ConfigurationManager.AppSettings["accountName"];

9. Sunucu bağlamına statik başvuru olarak kullanılan aşağıdaki sınıf düzeyi değişkeni ekleyin.

		// Field for service context.
		private static CloudMediaContext _context = null;
		
<h2><a id="Step2"></a>Medya Hizmetleri Bağlamını Alma</h2>

Medya Hizmetleri bağlam nesnesi Medya Hizmetlerini programlamaya yönelik tüm temel nesneleri ve koleksiyonları içerir. Bu bağlam, işler, varlıklar, dosyalar, erişim ilkeleri, bulucular ve diğer nesneler dahil önemli koleksiyonlara başvurular barındırır. Medya Hizmetleri programlama görevlerinin çoğu için bu sunucu bağlamını almanız gerekir.

Program.cs dosyasında, aşağıdaki kodu **Ana** yönteminizde birinci öğe olarak ekleyin. Bu kod, app.config dosyasından Medya Hizmetleri hesap adınızı ve hesap anahtarınızı kullanarak sunucu bağlamının bir örneğini oluşturur. Bu örnek, sınıf düzeyinde oluşturmuş olduğunuz **_context** değişkenine atanır.

	// Get the service context.
	_context = new CloudMediaContext(_accountName, _accountKey);
	
<h2><a id="Step3"></a>Varlık Oluşturma ve Dosya Yükleme</h2>

Bu bölümdeki kod şunları yapar: 

1. Boş bir Varlık oluşturur</br>
Varlıklar oluştururken onları şifrelemek için üç farklı seçenek belirtebilirsiniz. 

	- **AssetCreationOptions.None**: şifreleme yoktur. Şifrelenmemiş bir varlık oluşturmak istiyorsanız bu seçeneği ayarlamalısınız.
	- **AssetCreationOptions.CommonEncryptionProtected**: Genel Şifre Korumalı (CENC) dosyalar içindir. PlayReady olarak önceden şifrelenmiş dosya kümesi buna bir örnektir. 
	- **AssetCreationOptions.StorageEncrypted**: depolama şifrelemesidir. Azure depolama alanına yüklemeden önce şifresiz bir giriş dosyasını şifreler.

		<div class="dev-callout"> 
	<strong>Not</strong> 
	<p>Medya Hizmetleri, Dijital Hak Yöneticisi'nde(DRM) olduğu gibi iletim şifrelemesi değil disk üzerinde depolama şifrelemesi sağlar.</p> 
	</div>

2. Varlıkla ilişkilendirmek istediğimiz bir AssetFile örneği oluşturur.
3. Varlığın izinlerini ve erişim süresini tanımlayan bir AccessPolicy örneği oluşturur.
4. Varlığa erişim sağlayan bir Bulucu örneği oluşturur.
5. Tek medya dosyasını Medya Hizmetlerine yükler. Varlıkları oluşturma ve yükleme işleminin diğer bir adı da çekmedir.

Aşağıdaki yöntemleri sınıfa ekleyin.

<pre><code>
static private IAsset CreateEmptyAsset(string assetName, AssetCreationOptions assetCreationOptions)
{
    var asset = _context.Assets.Create(assetName, assetCreationOptions);

    Console.WriteLine("Asset name: " + asset.Name);
    Console.WriteLine("Time created: " + asset.Created.Date.ToString());

    return asset;
}

static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
{
    var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
    var asset = CreateEmptyAsset(assetName, assetCreationOptions);

    var fileName = Path.GetFileName(singleFilePath);

    var assetFile = asset.AssetFiles.Create(fileName);

    Console.WriteLine("Created assetFile {0}", assetFile.Name);

    var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(3),
                                                        AccessPermissions.Write | AccessPermissions.List);

    var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

    Console.WriteLine("Upload {0}", assetFile.Name);

    assetFile.Upload(singleFilePath);
    Console.WriteLine("Done uploading of {0} using Upload()", assetFile.Name);

    locator.Delete();
    accessPolicy.Delete();

    return asset;
}

</code></pre>

Ana yönteminizde **\_context = new CloudMediaContext(_accountName, _accountKey);** satırından sonra yönteme bir çağrı ekleyin. 

	IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.None, _singleInputFilePath)

<h2><a id="Step4"></a>Varlığı Sunucuda Kodlama ve Çıkış Varlığını İndirme</h2>

Medya Hizmetleri'nde, medya içeriğini kodlama, şifreleme, biçim dönüşümleri yapma ve benzeri çeşitli yollarla işleyen işler oluşturabilirsiniz. Bir Medya Hizmetleri işi, işleme çalışmasının ayrıntılarını belirten bir veya daha fazla görevi her zaman içerir. Bu bölümde, temel bir kodlama görevi oluşturacak ve sonra Azure Media Encoder'ı kullanarak bunu gerçekleştiren bir işi çalıştıracaksınız. Bu görev, yapılacak kodlama türünü belirtmek için bir ön ayar dizesini kullanır. Mevcut ön ayar kodlama değerlerini görmek için bkz. [Azure Media Encoder İçin Görev Ön Ayar Dizeleri](http://msdn.microsoft.com/tr-tr/library/windowsazure/jj129582.aspx). Medya Hizmetleri, Microsoft Expression Encoder ile aynı medya dosyası giriş ve çıkış biçimlerini destekler. Desteklenen biçimlerin listesi için bkz. [Medya Hizmetleri İçin Desteklenen Dosya Türleri](http://msdn.microsoft.com/tr-tr/library/windowsazure/hh973634.aspx).

<ol>
<li>
Sınıfınıza aşağıdaki <strong>CreateEncodingJob</strong> yöntem tanımını ekleyin. Bu yöntem, bir kodlama işine yönelik çeşitli zorunlu görevlerin nasıl yerine getirileceğini gösterir:
<ul>
<li>
Yeni bir iş tanımlayın.
</li>
<li>
İşi yönetecek bir medya işlemcisi tanımlayın. Medya işlemcisi, kodlama, şifreleme, biçim dönüştürme ve işleme ait diğer ilgili işleri gerçekleştiren bir bileşendir. Mevcut birkaç medya işlemcisi türü vardır (_context.MediaProcessors aracılığıyla bunların tümünü çoğaltabilirsiniz.) Bu eğitimde sonraki bir bölümde gösterilen GetLatestMediaProcessorByName yöntemi, Azure Media Encoder işlemcisini döndürür.
</li>
<li>
Yeni bir görev tanımlayın. Her işte bir veya daha fazla görev vardır. Göreve bir kolay ad, medya işlemcisi örneği, görev yapılandırma dizesi ve görev oluşturma seçenekleri atadığınıza dikkat edin. Yapılandırma dizesi, kodlama ayarlarını belirtir. Bu örnekte <strong>H264 Broadband 720p</strong> ayarı kullanılmıştır. Bu ön ayar, tek bir MP4 dosyası üretir. Bu ve diğer ön ayarlar hakkında daha fazla bilgi için bkz. <a href="http://msdn.microsoft.com/library/windowsazure/jj129582.aspx">Azure Media Encoder İçin Görev Ön Ayar Dizeleri</a>.
</li>
<li>
Göreve giriş varlığı ekleyin. Bu örnekte, giriş varlığı önceki bölümde oluşturduğunuz varlıktır.
</li>
<li>
Göreve çıkış varlığı ekleyin. Çıkış varlığına bir kolay ad, iş tamamlandıktan sonra çıkışın sunucuya kaydedilip kaydedilmeyeceğini gösteren bir Boole değeri ve çıkışın depolama ve aktarım için şifrelenmediğini gösteren bir <strong>AssetCreationOptions.None</strong> değeri atayın. 
</li>
<li>
İşi gönderin.</br>
İşi göndermek, kodlama işini yapmak için gereken son aşamadır.
</li>
</ul>
Bu yöntem aynı zamanda işin ilerleme durumunu izleme ve kodlama işinizin oluşturduğu varlığa erişme gibi diğer kullanışlı (ancak isteğe bağlı) görevlerin nasıl gerçekleştirileceğini de gösterir.
<pre><code>
static IJob CreateEncodingJob(IAsset asset, string inputMediaFilePath, string outputFolder)
{
    // Declare a new job.
    IJob job = _context.Jobs.Create("My encoding job");
    // Get a media processor reference, and pass to it the name of the 
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Azure Media Encoder");

    // Create a task with the encoding details, using a string preset.
    ITask task = job.Tasks.AddNew("My encoding task",
        processor,
        "H264 Broadband 720p",
        Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.ProtectedConfiguration);

    // Specify the input asset to be encoded.
    task.InputAssets.Add(asset);
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

    // Optionally log job details. This displays basic job details
    // to the console and saves them to a JobDetails-{JobId}.txt file 
    // in your output folder.
    LogJobDetails(job.Id);

    // Check job execution and wait for job to finish. 
    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
    progressJobTask.Wait();

    // **********
    // Optional code.  Code after this point is not required for 
    // an encoding job, but shows how to access the assets that 
    // are the output of a job, either by creating URLs to the 
    // asset on the server, or by downloading. 
    // **********

    // Get an updated job reference.
    job = GetJob(job.Id);

    // If job state is Error the event handling 
    // method for job progress should log errors.  Here we check 
    // for error state and exit if needed.
    if (job.State == JobState.Error)
    {
        Console.WriteLine("\nExiting method due to job error.");
        return job;
    }

    // Get a reference to the output asset from the job.
    IAsset outputAsset = job.OutputMediaAssets[0];
    IAccessPolicy policy = null;
    ILocator locator = null;

    // Declare an access policy for permissions on the asset. 
    // You can call an async or sync create method. 
    policy =
        _context.AccessPolicies.Create("My 30 days readonly policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

    // Create a SAS locator to enable direct access to the asset 
    // in blob storage. You can call a sync or async create method.  
    // You can set the optional startTime param as 5 minutes 
    // earlier than Now to compensate for differences in time  
    // between the client and server clocks. 

    locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset,
        policy,
        DateTime.UtcNow.AddMinutes(-5));

    // Build a list of SAS URLs to each file in the asset. 
    List<String> sasUrlList = GetAssetSasUrlList(outputAsset, locator);

    // Write the URL list to a local file. You can use the saved 
    // SAS URLs to browse directly to the files in the asset.
    if (sasUrlList != null)
    {
        string outFilePath = Path.GetFullPath(outputFolder + @"\" + "FileSasUrlList.txt");
        StringBuilder fileList = new StringBuilder();
        foreach (string url in sasUrlList)
        {
            fileList.AppendLine(url);
            fileList.AppendLine();
        }
        WriteToFile(outFilePath, fileList.ToString());

        // Optionally download the output to the local machine.
        DownloadAssetToLocal(job.Id, outputFolder);
    }

    
    return job;
}

</code></pre>
</li>
<li>
<strong>Ana</strong> yönteminizde daha önce eklemiş olduğunuz satırlardan sonra <strong>CreateEncodingJob</strong> yöntemine bir çağrı ekleyin.
<pre><code>
CreateEncodingJob(asset, _singleInputFilePath, _outputFilesFolder);
</code></pre>
</li>
<li>
Aşağıdaki yardım yöntemlerini sınıfa ekleyin. <strong>CreateEncodingJob</strong> yöntemini desteklemek için bunlar gereklidir. Yardım yöntemlerinin özeti aşağıda verilmiştir.
<ul>
<li>
<strong>GetLatestMediaProcessorByName</strong> yöntemi kodlamayı, şifrelemeyi veya ilgili başka bir işlem görevini yönetmek için uygun medya işlemcisini döndürür. Oluşturmak istediğiniz işlemcinin uygun olan dize adını kullanarak medya işlemcisini oluşturursunuz. mediaProcessor parametresi için yönteme geçirilebilen olası dizeler şunlardır: <strong>Azure Media Encoder</strong>, <strong>Windows Azure Media Packager</strong>, <strong>Windows Azure Media Encryptor</strong>, <strong>Storage Decryption</strong>.
<pre><code>
private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
{
    // The possible strings that can be passed into the 
    // method for the mediaProcessor parameter:
    //   Azure Media Encoder
    //   Windows Azure Media Packager
    //   Windows Azure Media Encryptor
    //   Storage Decryption

    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

    if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

    return processor;
}

</code></pre>
</li>
<li>
İşleri çalıştırdığınızda, işin ilerleme durumunu izlemek için çoğunlukla bir yönteme ihtiyacınız olur. Aşağıdaki kod örneği StateChanged olay işleyicisini tanımlar. Bu olay işleyicisi, işin ilerleme durumunu izler ve duruma bağlı olarak güncel durum bilgisini verir. Bu kod LogJobStop yöntemini de tanımlar. Bu yardım yöntemi hata ayrıntılarını günlüğe kaydeder.

<pre><code>
private static void StateChanged(object sender, JobStateChangedEventArgs e)
{
    Console.WriteLine("Job state changed event:");
    Console.WriteLine("  Previous state: " + e.PreviousState);
    Console.WriteLine("  Current state: " + e.CurrentState);

    switch (e.CurrentState)
    {
        case JobState.Finished:
            Console.WriteLine();
            Console.WriteLine("********************");
            Console.WriteLine("Job is finished.");
            Console.WriteLine("Please wait while local tasks or downloads complete...");
            Console.WriteLine("********************");
            Console.WriteLine();
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
            LogJobStop(job.Id);
            break;
        default:
            break;
    }
}

private static void LogJobStop(string jobId)
{
    StringBuilder builder = new StringBuilder();
    IJob job = GetJob(jobId);

    builder.AppendLine("\nThe job stopped due to cancellation or an error.");
    builder.AppendLine("***************************");
    builder.AppendLine("Job ID: " + job.Id);
    builder.AppendLine("Job Name: " + job.Name);
    builder.AppendLine("Job State: " + job.State.ToString());
    builder.AppendLine("Job started (server UTC time): " + job.StartTime.ToString());
    builder.AppendLine("Media Services account name: " + _accountName);
    // Log job errors if they exist.  
    if (job.State == JobState.Error)
    {
        builder.Append("Error Details: \n");
        foreach (ITask task in job.Tasks)
        {
            foreach (ErrorDetail detail in task.ErrorDetails)
            {
                builder.AppendLine("  Task Id: " + task.Id);
                builder.AppendLine("    Error Code: " + detail.Code);
                builder.AppendLine("    Error Message: " + detail.Message + "\n");
            }
        }
    }
    builder.AppendLine("***************************\n");
    // Write the output to a local file and to the console. The template 
    // for an error output file is:  JobStop-{JobId}.txt
    string outputFile = _outputFilesFolder + @"\JobStop-" + JobIdAsFileName(job.Id) + ".txt";
    WriteToFile(outputFile, builder.ToString());
    Console.Write(builder.ToString());
}

private static void LogJobDetails(string jobId)
{
    StringBuilder builder = new StringBuilder();
    IJob job = GetJob(jobId);

    builder.AppendLine("\nJob ID: " + job.Id);
    builder.AppendLine("Job Name: " + job.Name);
    builder.AppendLine("Job submitted (client UTC time): " + DateTime.UtcNow.ToString());
    builder.AppendLine("Media Services account name: " + _accountName);

    // Write the output to a local file and to the console. The template 
    // for an error output file is:  JobDetails-{JobId}.txt
    string outputFile = _outputFilesFolder + @"\JobDetails-" + JobIdAsFileName(job.Id) + ".txt";
    WriteToFile(outputFile, builder.ToString());
    Console.Write(builder.ToString());
}
        
private static string JobIdAsFileName(string jobID)
{
    return jobID.Replace(":", "_");
}
</code></pre>
</li>
<li>
WriteToFile yöntemithe belirtilen çıkış klasöre dosya yazar.
<pre><code>
static void WriteToFile(string outFilePath, string fileContent)
{
    StreamWriter sr = File.CreateText(outFilePath);
    sr.Write(fileContent);
    sr.Close();
}

</code></pre>
</li>
<li>
Medya Hizmetlerinde varlıkları kodladıktan sonra, kodlama işinden kaynaklanan çıkış varlıklarına erişebilirsiniz. Bu eğitim, bir kodlama işinin çıkışına erişmenin iki yolunu gösterir:
<ul>
<li>
Sunucudaki varlığa ait bir SAS URL'si oluşturma. 
</li>
<li>
Çıkış varlığını sunucudan indirme.
</li>
</ul>
GetAssetSasUrlList yöntemi, bir varlıktaki tüm dosyalara ait SAS URL'lerinin listesini oluşturur. 
<pre><code>
static List<String> GetAssetSasUrlList(IAsset asset, ILocator locator)
{
    // Declare a list to contain all the SAS URLs.
    List<String> fileSasUrlList = new List<String>();

    // If the asset has files, build a list of URLs to 
    // each file in the asset and return. 
    foreach (IAssetFile file in asset.AssetFiles)
    {
        string sasUrl = BuildFileSasUrl(file, locator);
        fileSasUrlList.Add(sasUrl);
    }

    // Return the list of SAS URLs.
    return fileSasUrlList;
}

// Create and return a SAS URL to a single file in an asset. 
static string BuildFileSasUrl(IAssetFile file, ILocator locator)
{
    // Take the locator path, add the file name, and build 
    // a full SAS URL to access this file. This is the only 
    // code required to build the full URL.
    var uriBuilder = new UriBuilder(locator.Path);
    uriBuilder.Path += "/" + file.Name;

    // Optional:  print the locator.Path to the asset, and 
    // the full SAS URL to the file
    Console.WriteLine("Locator path: ");
    Console.WriteLine(locator.Path);
    Console.WriteLine();
    Console.WriteLine("Full URL to file: ");
    Console.WriteLine(uriBuilder.Uri.AbsoluteUri);
    Console.WriteLine();


    //Return the SAS URL.
    return uriBuilder.Uri.AbsoluteUri;
}

</code></pre>
</li>
<li>
<strong>DownloadAssetToLocal</strong> yöntemi varlıktaki her dosyayı yerel klasöre indirir. Bu örnekte, varlık tek bir giriş medya dosyasıyla oluşturulduğundan, çıkış varlık dosyaları koleksiyonu iki dosya içerir: kodlanan medya dosyası (bir .mp4 dosyası) ve varlıkla ilgili meta verilerin olduğu bir .xml dosyası. Bu yöntem iki dosyayı da indirir.
<pre><code>
static IAsset DownloadAssetToLocal(string jobId, string outputFolder)
{
    // This method illustrates how to download a single asset. 
    // However, you can iterate through the OutputAssets
    // collection, and download all assets if there are many. 

    // Get a reference to the job. 
    IJob job = GetJob(jobId);
    // Get a reference to the first output asset. If there were multiple 
    // output media assets you could iterate and handle each one.
    IAsset outputAsset = job.OutputMediaAssets[0];

    IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
    ILocator locator = _context.Locators.CreateSasLocator(outputAsset, accessPolicy);
    BlobTransferClient blobTransfer = new BlobTransferClient
    {
        NumberOfConcurrentTransfers = 10,
        ParallelTransferThreadCount = 10
    };

    var downloadTasks = new List<Task>();
    foreach (IAssetFile outputFile in outputAsset.AssetFiles)
    {
        // Use the following event handler to check download progress.
        outputFile.DownloadProgressChanged += DownloadProgress;

        string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);

        Console.WriteLine("File download path:  " + localDownloadPath);

        downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));

        outputFile.DownloadProgressChanged -= DownloadProgress;
    }

    Task.WaitAll(downloadTasks.ToArray());

    return outputAsset;
}

static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
{
    Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
}
</code></pre>
</li>
<li>
GetJob ve GetAsset yardım yöntemleri kimlikleri verilen bir iş nesnesine ve bir varlık nesnesine başvuruyu sorgular ve döndürür. Sunucudaki diğer Medya Hizmetleri nesnelerine olan başvuruları döndürmek için benzer türde bir LINQ sorgusu kullanabilirsiniz.
<pre><code>
static IJob GetJob(string jobId)
{
    // Use a Linq select query to get an updated 
    // reference by Id. 
    var jobInstance =
        from j in _context.Jobs
        where j.Id == jobId
        select j;
    // Return the job reference as an Ijob. 
    IJob job = jobInstance.FirstOrDefault();

    return job;
}
static IAsset GetAsset(string assetId)
{
    // Use a LINQ Select query to get an asset.
    var assetInstance =
        from a in _context.Assets
        where a.Id == assetId
        select a;
    // Reference the asset as an IAsset.
    IAsset asset = assetInstance.FirstOrDefault();

    return asset;
}
</code></pre>
</li>
</ul>
</li>
</ol>

## Kodu Sınama
Programı çalıştırın (F5 tuşuna basın). Konsol, aşağıdakine benzer bir çıkış görüntüler:

<pre><code>
Asset name: UploadSingleFile_11/14/2012 10:09:11 PM
Time created: 11/14/2012 12:00:00 AM
Created assetFile interview2.wmv
Upload interview2.wmv
Done uploading of interview2.wmv using Upload()

Job ID: nb:jid:UUID:ea8d5a66-86b8-9b4d-84bc-6d406259acb8
Job Name: My encoding job
Job submitted (client UTC time): 11/14/2012 10:09:39 PM
Media Services account name: Add-Media-Services-Account-Name
Media Services account location: Add-Media-Services-account-location-name

Job(My encoding job) state: Queued.
Please wait...

Job(My encoding job) state: Processing.
Please wait...

********************
Job(My encoding job) is finished.
Please wait while local tasks or downloads complete...
********************

Locator path:
https://mediasvcd08mtz29tcpws.blob.core.windows-int.net/asset-4f5b42f4-3ade-4c2c
-9d48-44900d4f6b62?st=2012-11-14T22%3A07%3A01Z&se=2012-11-14T23%3A07%3A01Z&sr=c&
si=d07ec40c-02d7-4642-8e54-443b79f3ba3c&sig=XKMo0qJI5w8Fod3NsV%2FBxERnav8Jb6hL7f
xylq3oESc%3D

Full URL to file:
https://mediasvcd08mtz29tcpws.blob.core.windows-int.net/asset-4f5b42f4-3ade-4c2c
-9d48-44900d4f6b62/interview2.mp4?st=2012-11-14T22%3A07%3A01Z&se=2012-11-14T23%3
A07%3A01Z&sr=c&si=d07ec40c-02d7-4642-8e54-443b79f3ba3c&sig=XKMo0qJI5w8Fod3NsV%2F
BxERnav8Jb6hL7fxylq3oESc%3D

Locator path:
https://mediasvcd08mtz29tcpws.blob.core.windows-int.net/asset-4f5b42f4-3ade-4c2c
-9d48-44900d4f6b62?st=2012-11-14T22%3A07%3A01Z&se=2012-11-14T23%3A07%3A01Z&sr=c&
si=d07ec40c-02d7-4642-8e54-443b79f3ba3c&sig=XKMo0qJI5w8Fod3NsV%2FBxERnav8Jb6hL7f
xylq3oESc%3D

Full URL to file:
https://mediasvcd08mtz29tcpws.blob.core.windows-int.net/asset-4f5b42f4-3ade-4c2c
-9d48-44900d4f6b62/interview2_metadata.xml?st=2012-11-14T22%3A07%3A01Z&se=2012-1
1-14T23%3A07%3A01Z&sr=c&si=d07ec40c-02d7-4642-8e54-443b79f3ba3c&sig=XKMo0qJI5w8F
od3NsV%2FBxERnav8Jb6hL7fxylq3oESc%3D

Downloads are in progress, please wait.

File download path:  C:\supportFiles\outputfiles\interview2.mp4
1.70952185308162 % download progress.
3.68508804454907 % download progress.
6.48870388360293 % download progress.
6.83808741232649 % download progress.
. . . 
99.0763740574049 % download progress.
99.1522674787341 % download progress.
100 % download progress.
File download path:  C:\supportFiles\outputfiles\interview2_metadata.xml
100 % download progress.

</code></pre>

1. Bu uygulamayı çalıştırmanın sonucunda şunlar olur:

2. Bir .wmv dosyası Medya Hizmetleri'ne yüklenir. 

3. Bu dosya daha sonra, **Azure Media Encoder**'ın **H264 Broadband 720p** ön ayarı kullanılarak kodlanır.

4. \supportFiles\outputFiles klasöründe FileSasUrlList.txt dosyası oluşturulur. Bu dosya, kodlanan varlığın URL'sini içerir. 

	Medya dosyasını yürütmek için varlığın URL'sini metin dosyasından kopyalayın ve URL'yi tarayıcıya yapıştırın. 

5. .mp4 medya dosyası ve _metadata.xml dosyası outputFiles klasörüne indirilir.

> [WACOM.NOTE]
> Medya Hizmetleri nesne modelinde, bir varlık bir veya birden çok dosyayı temsil eden bir Medya Hizmetleri içerik koleksiyonu nesnesidir. Bulucunun yolu, bu varlığın Azure Depolama'daki temel yolu olan bir Azure blob URL'sini verir. Varlık içindeki belirli dosyalara erişmek için dosya adını temel bulucu yoluna ekleyin.

<h2>Sonraki Adımlar</h2>
Bu eğitimde, basit bir Medya Hizmetleri uygulaması oluşturmak için bir dizi programlama görevi gösterilmiştir. Sunucu bağlamını alma, varlıklar oluşturma, varlıkları kodlama ve sunucudaki varlıkları indirme veya onlara erişme dahil temel Medya Hizmetleri programlama görevlerini öğrendiniz. Sonraki adımlar ve daha gelişmiş geliştirme görevleri için aşağıya bakın:

- <a href="http://azure.microsoft.com/tr-tr/develop/media-services/resources/">Medya Hizmetlerini Kullanma</a>
- <a href="http://msdn.microsoft.com/tr-tr/library/windowsazure/hh973618.aspx">Medya Hizmetleri REST API ile Uygulamalar Oluşturma</a>


<!-- Anchors. -->
[Getting started with Mobile Services]:#getting-started
[Create a new mobile service]:#create-new-service
[Define the mobile service instance]:#define-mobile-service-instance
[Next Steps]:#next-steps

