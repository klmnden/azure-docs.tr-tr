<properties
    pageTitle=".NET SDK kullanarak isteğe bağlı içerik göndermeye başlama"
    description="Bu öğreticide, .NET’i kullanarak Azure Media Services ile isteğe bağlı bir içerik teslim uygulaması gerçekleştirilmesinin adımları açıklanmaktadır."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="04/18/2016"
    ms.author="juliako"/>


# .NET SDK kullanarak isteğe bağlı içerik göndermeye başlama


[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


>[AZURE.NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](/pricing/free-trial/?WT.mc_id=A261C142F). 
 
##Genel Bakış 

Bu öğreticide, .NET için Azure Media Services (AMS) SDK’sını kullanarak bir İsteğe Bağlı Video (VoD) içerik teslim uygulaması gerçekleştirilmesinin adımları açıklanmaktadır.


Öğretici, temel Media Services iş akışını ve Media Services geliştirmek için gereken en genel programlama nesnelerini ve görevleri tanıtır. Öğreticiyi tamamladığınızda, yükleyip kodlayarak ardından indirdiğiniz örnek bir medya dosyasını akışla aktarabilecek veya aşamalı olarak indirebileceksiniz.

## Öğrenecekleriniz

Öğretici aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir:

1.  Media Services hesabı oluşturun (Azure Klasik Portalı'nı kullanarak).
2.  Akış uç noktasını yapılandırın (portalı kullanarak).
3.  Visual Studio projesi oluşturun ve yapılandırın.
5.  Media Services hesabına bağlanın.
6.  Yeni bir varlık oluşturun ve video dosyası yükleyin.
7.  Kaynak dosyayı uyarlamalı bit hızlı bir MP4 dosyaları grubuna kodlayın.
8.  Varlığı yayımlayın, akış ve aşamalı indirme URL’lerini alın.
9.  İçeriğiniz oynatarak test edin.

## Ön koşullar

Öğreticiyi tamamlamak için aşağıdakiler gereklidir.

- Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. 
    
    Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](/pricing/free-trial/?WT.mc_id=A261C142F). Ücretli Azure hizmetlerini denemek için kullanabileceğiniz krediler alırsınız. Krediler bitmiş olsa bile hesabı sürdürebilir ve Azure App Service’deki Web Apps özelliği gibi ücretsiz Azure hizmetlerinden faydalanabilirsiniz.
- İşletim sistemleri: Windows 8 veya sonraki sürümü, Windows 2008 R2, Windows 7.
- .NET framework 4.0 veya sonraki sürümü
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate veya Express) veya sonraki sürümleri.


##Örnek indirme

[Buradan](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/) bir örnek alarak çalıştırın.

##Portalı kullanarak bir Media Services hesabı oluşturun

1. Klasik Azure Portalı’nda **Yeni**’ye, sonra **Medya Hizmeti**’ne ve **Hızlı Oluştur**’a tıklayın.

    ![Media Services Hızlı Oluştur](./media/media-services-dotnet-get-started/wams-QuickCreate.png)

2. **AD** alanına yeni hesabın adını girin. Media Services hesabı adı, boşluk olmadan tamamen küçük harf ve sayılardan oluşmalı ve 3-24 karakter uzunluğunda olmalıdır.

3. **BÖLGE** alanında Media Services hesabınıza ait meta veri kayıtlarını depolamak üzere kullanılacak coğrafi bölgeyi seçin. Açılan listede yalnızca kullanılabilir Media Services bölgeleri görüntülenir.

4. **DEPOLAMA HESABI** alanında, Media Services hesabınızdan gelen medya içeriğine blob depolama sağlamak üzere bir depolama hesabı seçin. Media Services hesabınızla aynı coğrafi bölgede bulunan mevcut bir depolama hesabını seçebilir ya da yeni bir depolama hesabı oluşturabilirsiniz. Aynı bölgede yeni bir depolama hesabı oluşturulur.

5. Yeni bir depolama hesabı oluşturduysanız **YENİ DEPOLAMA HESABI ADI** alanına depolama hesabı için bir ad girin. Depolama hesabı adları için kurallar Media Services hesapları ile aynıdır.

6. Formun altındaki **Hızlı Oluştur**’a tıklayın.

İşlemin durumunu pencerenin altındaki ileti alanında izleyebilirsiniz.

Hesabınız başarıyla oluşturulduğunda durum **Etkin** olarak değişir.

Sayfanın altında **ANAHTARLARI YÖNET** düğmesi görünür. Bu düğmeye tıkladığınızda Media Services hesap adını, birincil ve ikincil anahtarları içeren bir iletişim kutusu görüntülenir. Media Services hesabına programlı olarak erişmek için hesap adına ve birincil anahtar bilgilerine ihtiyacınız vardır.

![Media Services Sayfası](./media/media-services-dotnet-get-started/wams-mediaservices-page.png)

Hesap adına çift tıkladığınızda, varsayılan olarak **Hızlı Başlangıç** sayfası görüntülenir. Bu sayfada, portalın diğer sayfalarında da bulunan bazı yönetim görevleri gerçekleştirilebilir. Örneğin, bir video dosyasını bu sayfadan veya İÇERİK sayfasından yükleyebilirsiniz.

##Portalı kullanarak akış uç noktasını yapılandırma

Azure Media Services ile çalışırken en sık karşılaşılan senaryolardan biri, istemcilerinize bit hızı uyarlamalı akış iletmektir. Bit hızı uyarlamalı akış ile video geçerli ağ bant genişliği, CPU kullanımı ve diğer etkenlere bağlı olarak görüntülendiğinden istemci, daha yüksek veya daha düşük bit hızlı bir akışa geçebilir. Media Services şu bit hızı uyarlamalı akış teknolojilerini destekler: HTTP Canlı Akışı (HLS), Kesintisiz Akış, MPEG DASH ve HDS (yalnızca Adobe PrimeTime/Erişim lisans sahipleri için).

Media Services, bit hızı uyarlamalı MP4 veya Kesintisiz Akış kodlanmış içeriğinizi Media Services tarafından desteklenen akış biçimlerinde (MPEG DASH, HLS, Kesintisiz Akış, HDS), bu akış biçimlerine yeniden paketlemeden iletmenize olanak tanıyan dinamik paketleme sağlar.

Dinamik paketlemeden yararlanmak için aşağıdakileri yapmanız gerekir:

- Ara (kaynak) dosyanızı bit hızı uyarlamalı bir MP4 veya Kesintisiz Akış dosyaları kümesine kodlayın veya kodlamasını dönüştürün (kodlama adımları bu öğreticinin ilerleyen bölümlerinde gösterilmektedir),
- Kendisinden içeriğinizi iletmek istediğiniz **akış uç noktası** için en az bir akış birimi alın.

Dinamik paketleme ile dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Akışa ayrılan birim sayısını değiştirmek için aşağıdakileri yapın:

1. [Portalda](https://manage.windowsazure.com/) **Media Services**’e tıklayın. Ardından, medya hizmetinin adına tıklayın.

2. AKIŞ UÇ NOKTALARI sayfasını seçin. Değiştirmek istediğiniz akış uç noktasına tıklayın.

3. Akış birim sayısını belirtmek için, ÖLÇEK sekmesine tıklayın ve **ayrılan kapasite** kaydırıcısını hareket ettirin.

    ![Ölçek sayfası](./media/media-services-dotnet-get-started/media-services-origin-scale.png)

4. **TAMAM**’a tıklayarak değişikliklerinizi kaydedin.

Yeni birimlerin ayrılması yaklaşık 20 dakikada tamamlanır.

>[AZURE.NOTE] Şu anda, herhangi bir pozitif sayıda akış birimi bulunan bir durumdan hiçbir akış birimi bulunmamasına dönmek, akışı bir saate varan bir süre boyunca devre dışı bırakabilir.
>
> Maliyet hesaplamasında, 24 saatlik dönem için belirtilen en yüksek birim sayısı kullanılır. Fiyatlandırma ayrıntıları hakkında daha fazla bilgi için bkz. [Media Services fiyatlandırma ayrıntıları](http://go.microsoft.com/fwlink/?LinkId=275107).



##Visual Studio projesi oluşturup yapılandırma

1. Visual Studio 2013, Visual Studio 2012 veya Visual Studio 2010 SP1’de yeni bir C# Konsol Uygulaması oluşturun. **Ad**, **Konum** ve **Çözüm adı** değerlerini girip **Tamam**’a tıklayın.

2. [Windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions) Nuget paketini kullanarak **Azure Media Services .NET SDK Uzantıları**’nı yükleyin.  Media Services .NET SDK Uzantıları, kodunuzu basitleştirerek Media Services ile geliştirme yapmayı kolaylaştıran bir dizi genişletme yöntemi ve yardımcı işlevdir. Bu paketin yüklenmesiyle **Media Services .NET SDK** da yüklenir ve diğer tüm gerekli bağımlılıklar eklenir.

3. System.Configuration bütünleştirilmiş koduna bir başvuru ekleyin. Bu bütünleştirilmiş kod, App.config gibi yapılandırma dosyalarına erişmek için kullanılan **System.Configuration.ConfigurationManager** sınıfını içerir.

4. App.config dosyasını açın (varsayılan olarak eklenmemişse dosyayı projenize ekleyin) ve dosyaya bir *appSettings* bölümü ekleyin. Aşağıdaki örnekte gösterildiği gibi Azure Media Services hesap adınız ve hesap anahtarınızın değerlerini ayarlayın. Hesap adı ve anahtarı bilgilerini elde etmek için, Klasik Azure Portalı'nı açıp medya hizmetleri hesabınızı seçin ve **ANAHTARLARI YÖNET** düğmesine tıklayın.

        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>
          
        </configuration>

5. Aşağıdaki kodu, Program.cs dosyasının başındaki mevcut **using** deyimlerinin üzerine yazın.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        

6. Proje dizini altında yeni bir klasör oluşturun ve kodlayıp akışla aktarmak veya aşamalı indirmek istediğiniz bir .mp4 veya .wmv dosyasını buraya kopyalayın. Bu örnekte "C:\VideoFiles" yolu kullanılmaktadır.

##Media Services hesabına bağlanma

.NET ile Media Services’i kullanırken, Media Services programlama görevlerinin çoğu için **CloudMediaContext** sınıfını kullanmalısınız: Media Services hesabına bağlanma; şu nesneleri oluşturma, güncelleştirme, silme ve bunlara erişme: varlıklar, varlık dosyaları, işler, erişim ilkeleri, bulucular vb.

Varsayılan Program sınıfının üzerine aşağıdaki kodu yazın. Kod, bağlantı değerlerinin App.config dosyasından nasıl okunacağını ve Media Services’e bağlanmak amacıyla **CloudMediaContext** nesnesinin nasıl oluşturulacağını gösterir. Media Services’e bağlanma hakkında daha fazla bilgi için bkz. [.NET için Media Services SDK’sı ile Media Services’e bağlanma](http://msdn.microsoft.com/library/azure/jj129571.aspx).

 **Ana** işlev, bu bölümün sonraki kısımlarında açıklanacak olan yöntemleri çağırır.

    class Program
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];

        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;

        static void Main(string[] args)
        {
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }

##Yeni varlık oluşturma ve video dosyası yükleme

Media Services’de, dijital dosyalar bir varlığa yüklenir (veya alınır). **Varlık** varlığı video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları (ve bu dosyalar hakkındaki meta veriler) içerebilir.  Dosyalar yüklendiğinde, içeriğiniz sonraki işleme ve akışla aktarma faaliyetleri için güvenli bir şekilde bulutta depolanmış olur. Varlık içindeki dosyalara **Varlık Dosyaları** adı verilir.

Aşağıda tanımlanan **UploadFile** yöntemi, **CreateFromFile** yöntemini (.NET SDK Uzantılarında tanımlanmıştır) çağırır. **CreateFromFile**, belirtilen kaynak dosyasının yüklendiği yeni bir varlık oluşturur.

**CreateFromFile** yöntemi, aşağıdaki varlık oluşturma seçeneklerden birini belirtmenize olanak sağlayan **AssetCreationOptions**’ı alır:

- **Hiçbiri**: Şifreleme kullanılmaz. Varsayılan değer budur. Bu seçeneği kullandığınızda, içeriğinizin aktarım sırasında ve depolama alanında beklerken korunmadığını unutmayın.
Aşamalı indirme kullanarak bir MP4 iletmeyi planlıyorsanız bu seçeneği kullanın.
- **StorageEncrypted**: Temiz içeriğinizi yerel olarak Gelişmiş Şifreleme Standardı (AES) 256 bit şifreleme kullanarak şifrelemek için bu seçeneği kullanın. Şifrelenen içerik ardından Azure Storage’a yüklenerek bekleme durumunda depolanır. Depolama Şifrelemesi ile korunan varlıklar, kodlamadan önce otomatik olarak şifrelenerek şifrelenmiş bir dosya sistemine yerleştirilir ve yeni bir çıkış varlığı şeklinde geri yüklenmeden önce isteğe bağlı olarak yeniden şifrelenir. Depolama Şifrelemesinin birincil kullanım nedeni, yüksek kaliteli giriş medya dosyalarınızın güvenliğini güçlü şifrelemeyle diskte bekleyen konumda sağlamak istediğiniz durumdur.
- **CommonEncryptionProtected**: Zaten Ortak Şifreleme veya PlayReady DRM ile şifrelenip korunan bir içerik (örneğin, PlayReady DRM ile korunan Kesintisiz Akış) yüklüyorsanız bu seçeneği kullanın.
- **EnvelopeEncryptionProtected**: AES ile şifrelenmiş HLS yüklüyorsanız bu seçeneği kullanın. Dosyaların Transform Manager tarafından kodlanmış ve şifrelenmiş olması gerektiğini unutmayın.

**CreateFromFile** yöntemi, dosyanın yükleme durumunu bildirmek üzere bir geri çağırma belirtmenize de olanak sağlar.

Aşağıdaki örnekte, varlık seçeneği olarak **Hiçbiri** belirliyoruz.

Program sınıfına aşağıdaki yöntemi ekleyin.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


##Kaynak dosyayı uyarlamalı bit hızlı bir MP4 dosyaları grubuna kodlama

Varlıklar Media Services’e alındıktan sonra medyaya, istemcilere teslim edilmeden önce kodlama, biçimini değiştirme, filigran ekleme ve benzeri işlemler uygulanabilir. Bu etkinlikler, yüksek performans ve kullanılabilirlik sağlamak için birden fazla arka plan rol örneğinde zamanlanır ve çalıştırılır. Bu etkinliklere İşler adı verilir ve her bir İş, Varlık dosyası üzerinde asıl işi yapan atomik Görevlerden oluşur.

Daha önce belirtildiği gibi, Azure Media Services ile çalışırken en sık karşılaşılan senaryolardan biri, istemcilerinize bit hızı uyarlamalı akış iletmektir. Media Services, bit hızı uyarlamalı bir MP4 dosyaları kümesini dinamik olarak şu biçimlerden birine paketleyebilir: HTTP Canlı Akışı (HLS), Kesintisiz Akış, MPEG DASH ve HDS (yalnızca Adobe PrimeTime/Erişim lisans sahipleri için).

Dinamik paketlemeden yararlanmak için aşağıdakileri yapmanız gerekir:

- Ara (kaynak) dosyanızı bit hızı uyarlamalı bir MP4 veya Kesintisiz Akış dosyaları kümesine kodlayın veya kodlamasını dönüştürün.  
- Kendisinden içeriğinizi iletmek istediğiniz akış uç noktası için en az bir akış birimi alın.

Aşağıdaki kod, bir kodlama işinin nasıl gönderileceğini gösterir. İş, ara dosyayı **Medya Kodlayıcı Standart**’ını kullanarak bir grup bit hızı uyarlamalı MP4’e kodlamasını dönüştüren bir görev içerir. Kod işi gönderir ve tamamlanana kadar bekler.

İş tamamlandığında varlığınızı akışla aktarabilir veya kodlama dönüştürmenin sonucu olarak oluşturulan MP4 dosyalarını aşamalı indirebilirsiniz.
MP4 dosyalarını aşamalı indirmek için 0'dan fazla akış birimine ihtiyacınız olmadığını unutmayın.

Program sınıfına aşağıdaki yöntemi ekleyin.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {
    
        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.
    
        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);
    
        Console.WriteLine("Submitting transcoding job...");
    
    
        // Submit the job and wait until it is completed.
        job.Submit();
    
        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;
    
        Console.WriteLine("Transcoding job finished.");
    
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        return outputAsset;
    }

##Varlığı yayımlayıp akış ve aşamalı indirme URL’lerini alma

Bir varlığı akışla aktarmak veya indirmek için söz konusu varlığı önce bir bulucu oluşturarak “yayımlamak” gerekir. Bulucular varlıkta bulunan dosyalara erişim imkanı sağlar. Media Services, iki tür bulucuyu destekler: Medyayı akışla aktarmak (örneğin MPEG DASH, HLS veya Kesintisiz Akış) için kullanılan OnDemandOrigin bulucuları ve medya dosyalarını indirmek için kullanılan Erişim İmzası (SAS) bulucuları.

Bulucuları oluşturduktan sonra, dosyalarınızı akışla aktarmak veya indirmek için kullanılan URL'leri oluşturabilirsiniz.


Kesintisiz Akışa ilişkin bir akış URL'si aşağıdaki biçime sahiptir:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS’ye ilişkin bir akış URL'si aşağıdaki biçime sahiptir:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH’e ilişkin bir akış URL'si aşağıdaki biçime sahiptir:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Dosya indirmek için kullanılan bir SAS URL'si aşağıdaki biçime sahiptir:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Media Services .NET SDK uzantıları, yayımlanan varlık için biçimlendirilmiş URL'ler döndüren kullanışlı yardımcı yöntemler sağlar.

Aşağıdaki kod, .NET SDK Uzantıları kullanarak bulucu oluşturur, akış ve aşamalı indirme URL'leri alır. Kod ayrıca yerel bir klasöre nasıl dosya indirileceğini gösterir.

Program sınıfına aşağıdaki yöntemi ekleyin.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

##İçeriğiniz oynatarak test etme  

Önceki bölümde tanımlanan programı çalıştırdığınızda konsol penceresinde aşağıdakine benzer URL'ler görüntülenir.

Uyarlamalı akış URL'leri:

Kesintisiz Akış

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG DASH

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

Aşamalı indirme URL'leri (ses ve video).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


Videonuzu akışla aktarmak için [Azure Media Services Oynatıcısı](http://amsplayer.azurewebsites.net/azuremediaplayer.html)’nı kullanın.

Aşamalı indirmeyi test etmek için bir tarayıcıya (örneğin Internet Explorer, Chrome veya Safari) bir URL yapıştırın.


##Sonraki Adımlar: Media Services’i öğrenme yolları

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Geri bildirimde bulunma

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


### Başka bir şey mi arıyorsunuz?

Beklediklerinizi bu konuda bulamadıysanız, eksik bir şeyler varsa veya herhangi bir nedenle gereksinimleriniz karşılanmadıysa, lütfen aşağıdaki Disqus yazışmasını kullanarak bize geri bildirimde bulunun.


<!-- Anchors. -->


<!-- URLs. -->
  [Web Platformu Yükleyicisi]: http://go.microsoft.com/fwlink/?linkid=255386
  [Portal]: http://manage.windowsazure.com/



<!----HONumber=Jun16_HO2-->


