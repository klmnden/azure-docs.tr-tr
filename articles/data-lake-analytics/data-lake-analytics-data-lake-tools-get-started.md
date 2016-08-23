<properties
   pageTitle="Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme | Azure"
   description="Visual Studio için Data Lake Araçları'nı nasıl yükleyeceğinizi, U-SQL betiklerini nasıl geliştirip test edeceğinizi öğrenin. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# Öğretici: Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Visual Studio için Data Lake Araçları'nı nasıl yükleyeceğinizi ve U-SQL betiklerini yazıp test etmek üzere Visual Studio için Data Lake Araçları'nı nasıl kullanacağınızı öğrenin.

U-SQL, veri gölü içindeki tüm verilerin ve daha fazlasının hazırlanması, dönüştürülmesi ve analiz edilmesi için kullanılan oldukça ölçeklenebilir, yüksek düzeyde genişletilebilir bir dildir. Daha fazla bilgi için bkz. [U-SQL Başvurusu] (http://go.microsoft.com/fwlink/p/?LinkId=691348).


###Önkoşullar

- **Visual Studio 2015, Visual Studio 2013 güncelleştirme 4 veya Visual Studio 2012. Enterprise (Ultimate/Premium), Professional, Community sürümleri desteklenir; Express sürümü desteklenmez. Visual Studio "15" şu an için desteklenmemekle birlikte bu konudaki çalışmalarımız devam etmektedir.**
- **.NET sürüm 2.7.1 veya üzeri için Microsoft Azure SDK**.  [Web platformu yükleyicisini](http://www.microsoft.com/web/downloads/platform.aspx) kullanarak yükleyin.
- **[Visual Studio için Data Lake Araçları](http://aka.ms/adltoolsvs)**.

    Visual Studio için Data Lake Araçları yüklendikten sonra, "Azure" düğümünün altında Sunucu Gezgini içinde "Data Lake Analytics" düğümünü göreceksiniz (Sunucu gezginini Ctrl+Alt+S tuşlarına basarak açabilirsiniz).

- **[Azure Portal'ı kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)** konusunun içerdiği aşağıdaki iki bölümü tamamlayın.

    - [Azure Data Lake Analytics hesabı oluşturma](data-lake-analytics-get-started-portal.md#create_adl_analytics_account).
    - [SearchLog.tsv dosyasını varsayılan Data Lake Storage hesabına yükleme](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account).

    Size kolaylık sağlamak amacıyla, bir Data Lake Analytic hizmeti oluşturmaya ve kaynak veri dosyasını yüklemeye yönelik bir PowerShell örnek betiği, [Ek A - Öğreticiyi hazırlamaya yönelik PowerShell örneğinde](data-lake-analytics-data-lake-tools-get-started.md#appx-a-powershell-sample-for-preparing-the-tutorial) bulunabilir.

    Data Lake Araçları, Data Lake Analytics hesaplarının oluşturulmasını desteklemez. O nedenle bunu Azure Portal, Azure PowerShell, .NET SDK veya Azure CLI kullanarak oluşturmanız gerekir. Bir Data Lake Analytics işini çalıştırmak için bazı verilere ihtiyaç duyarsınız. Data Lake araçları, veri yüklemeyi desteklese de bu öğreticinin takibini kolaylaştırmak amacıyla örnek verileri yüklemek için portalı kullanacaksınız.

## Azure'a Bağlanma

**Data Lake Analytics'e bağlanmak için**

1. Visual Studio'yu açın.
2. **Görünüm** menüsünde, **Sunucu Gezgini**'ne tıklayarak Sunucu Gezgini'ni açın. Veya **[CTRL]+[ALT]+S** tuşlarına basın.
3. **Azure**'a sağ tıklayın, "Microsoft Azure Aboneliğine Bağlan" seçeneğine tıklayın ve ardından yönergeleri izleyin.
4. **Sunucu Gezgini**'nden, **Azure** seçeneğini ve ardından **Data Lake Analytics** seçeneğini genişletin. Varsa Data Lake Analytics hesaplarnızın listesini görürsünüz. Visual Studio'dan Data Lake Analytics hesabı oluşturamazsınız. Hesap oluşturmak için bkz. [Azure Portal'ı kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md) veya [Azure PowerShell'i kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md).

## Kaynak veri dosyalarını yükleme

Öğreticinin daha önceki **Önkoşul** bölümünde bazı verileri yüklediniz.  

Kendi verilerinizi kullanmak istiyorsanız Data Lake Araçları'ndan veri yüklemeye yönelik yordamlar burada sağlanmıştır.

**Bağımlı Azure Data Lake hesabına dosya yükleme**

1. **Sunucu Gezgini**'nden, **Azure** seçeneğini, **Data Lake Analytics** seçeneğini, Data Lake Analytics hesabınızı ve **Depolama Hesapları** seçeneğini genişletin. Varsayılan Data Lake Storage hesabını, bağlı Data Lake Storage hesaplarını ve bağlı Azure Storage hesaplarını göreceksiniz. Varsayılan Data Lake hesabı "Varsayılan Storage Hesabı" etiketine sahiptir.
2. Varsayılan Data Lake Store hesabına sağ tıklayın ve ardından **Explorer**'a tıklayın.  Bu, Visual Studio Gezgini bölmesinde Data Lake Araçları'nı açar.  Sol tarafta bir ağaç görünümü gösterilirken, içerik görünümü sağ tarafta bulunur.
3. Dosyaları yüklemek istediğiniz klasöre göz atın,
4. Herhangi bir boş alana sağ tıklayın ve ardından **Yükle**'ye tıklayın.

    ![U-SQL Visual Studio projesi U-SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-upload-files.png)

**Bağlı bir Azure Blob depolama hesabına dosya yüklemek için**

1. **Sunucu Gezgini**'nden, **Azure** seçeneğini, **Data Lake Analytics** seçeneğini, Data Lake Analytics hesabınızı ve **Storage Hesapları** seçeneğini genişletin. Varsayılan Data Lake Storage hesabını, bağlı Data Lake Storage hesaplarını ve bağlı Azure Storage hesaplarını göreceksiniz.
2. Azure Storage Hesabı'nı genişletin.
3. Dosyaları yüklemek istediğiniz kapsayıcıya sağ tıklayın ve ardından **Explorer**'a tıklayın. Bir kapsayıcınız yoksa ilk olarak Azure portalı, Azure PowerShell'i veya diğer araçları kullanarak bir tane oluşturmanız gerekir.
4. Dosyaları yüklemek istediğiniz klasöre göz atın,
5. Herhangi bir boş alana sağ tıklayın ve ardından **Yükle**'ye tıklayın.

## U-SQL betikleri geliştirme

Data Lake Analytics işleri, U-SQL dilinde yazılır. U-SQL hakkında daha fazla bilgi için bkz. [U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md) ve [U-SQL dili başvurusu](http://go.microsoft.com/fwlink/?LinkId=691348).

**Data Lake Analytics işi oluşturma ve gönderme**

1. **Dosya** menüsünde **Yeni**'ye ve ardından **Proje**'ye tıklayın.
2. **U-SQL Projesi** türünü seçin.

    ![yeni U-SQL Visual Studio projesi](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. **OK (Tamam)** düğmesine tıklayın. Visual Studio bir **Script.usql** dosyası ile çözüm oluşturur.
4. Aşağıdaki betiği **Script.usql** dosyasına girin:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();

        @res =
            SELECT *
            FROM @searchlog;        

        OUTPUT @res   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Bu U-SQL betiği, **Extractors.Tsv()** öğesini kullanarak kaynak veri dosyasını okur ve ardından **Outputters.Csv()** öğesini kullanarak bir csv dosyası oluşturur.

    Kaynak dosyayı farklı bir konuma kopyalamadıysanız bu iki yolu değiştirmeyin.  Data Lake Analytics, mevcut olmaması halinde çıkış klasörünü oluşturur.

    Varsayılan Data Lake hesaplarında depolanan dosyalar için göreli yolların kullanılması daha basittir. Mutlak yol da kullanabilirsiniz.  Örneğin:

        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Mutlak yolları, bağlı Storage hesaplarındaki dosyalara erişmek için kullanmanız gerekir.  Bağlı Azure Storage hesabında depolanan dosyalar için söz dizimi şu şekildedir:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Ortak blob veya ortak kapsayıcı erişim izinlerine sahip Azure Blob kapsayıcısı şu an için desteklenmemektedir.  

    Aşağıdaki özelliklere dikkat edin:

    - **IntelliSense**

        Ad otomatik olarak tamamlanır ve Satır Kümesi, Sınıflar, Veritabanları, Şemalar ve Kullanıcı Tanımlı Nesneler (UDO'lar) için üyeler gösterilir.

        Katalog varlıkları (Veritabanları, Şemalar, Tablolar, UDO'lar vb.) için IntelliSense, işlem hesabınızla ilgilidir. Geçerli etkin işlem hesabını, veritabanını ve şemayı üst araç çubuğundan denetleyebilir ve bunları açılır listelerden değiştirebilirsiniz.

    - *** sütunlarını genişletme**

        * öğesinin sağına tıklayın; * öğesinin altında mavi bir alt çizgi göreceksiniz. Fare imlecinizi mavi alt çizginin üzerine getirin ve sonra aşağı oka tıklayın.
        ![Data Lake visual studio araçlarını genişletme *](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-expand-asterisk.png)

        **Sütunları Genişlet**'e tıklayın; araç, * simgesi yerine sütun adlarını getirecektir.

    - **Otomatik Biçimlendirme**

        Kullanıcılar, Düzenle->Gelişmiş altındaki kod yapısına bağlı olarak Kapsam betiğinin girintisini değiştirebilir:

        - Belgeyi Biçimlendir (Ctrl+E, D): Tüm belgeyi biçimlendirir   
        - Seçimi Biçimlendir (Ctrl+K, Ctrl+F): Seçimi biçimlendirir. Seçim yapılmazsa bu kısayol, imlecin bulunduğu satırı biçimlendirir.  

        Tüm biçimlendirme kuralları, Araçlar->Seçenekler->Metin Düzenleyici->SIP->Biçimlendirme altında yapılandırılabilir.  
    - **Akıllı Girinti**

        Visual Studio için Data Lake Araçları, siz betik yazarken ifadelere otomatik olarak girinti ekleyebilir. Bu özellik varsayılan olarak devre dışıdır ve kullanıcıların özelliği U-SQL->Seçenekler ve Ayarlar->Anahtarlar-> Akıllı Girintiyi Etkinleştir seçeneğini işaretleyerek etkinleştirmesi gerekir.

    - **Tanıma Git ve Tüm Başvuruları Bul**

        Bir Satır Kümesi/parametre/sütun/UDO vb. öğenin adına sağ tıklayın Tanıma Git (F12) seçeneğine tıklanması, öğenin tanımına gitmenizi sağlar. Tüm Başvuruları Bul (Shift+F12) seçeneğine tıklanması, tüm başvuruları gösterir.

    - **Azure Yolu Ekle**

        Visual Studio için Data Lake Araçları, Azure dosya yolunu hatırlayıp betik yazarken el ile yazma seçeneğinin yerine kolay bir yol sunar: Düzenleyicinin içine sağ tıklayıp Azure Yolu Ekle'ye tıklayın. Azure Blob Tarayıcısı iletişim kutusundaki dosyaya gidin. **Tamam** düğmesine tıklayın. dosya yolu, kodunuza eklenir.

5. Data Lake Analytics hesabını, Veritabanı'nı ve Şema'yı belirtin. Test amacıyla betiği yerel olarak çalıştırmak için **(yerel)** öğesini seçebilirsiniz. Daha fazla bilgi için bkz. [U-SQL'yi yerel olarak çalıştırma](#run-u-sql-locally).

    ![U-SQL Visual Studio projesini gönderme](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

    Daha fazla bilgi için bkz. [U-SQL kataloğunu kullanma](data-lake-analytics-use-u-sql-catalog.md).

5. **Çözüm Gezgini**'nden, **Script.usql** öğesine sağ tıklayın ve ardından **Betik Oluştur**'a tıklayın. Çıkış bölmesinde sonucu doğrulayın.
6. **Çözüm Gezgini**'nden, **Script.usql** öğesine sağ tıklayın ve ardından **Betiği Gönder**'e tıklayın. İsteğe bağlı olarak, Script.usql bölmesinden **Gönder**'e de tıklayabilirsiniz.  Önceki ekran görüntüsüne bakın.  Gelişmiş seçenekleri kullanarak göndermek için Gönder düğmesinin yanındaki aşağı oka tıklayın:
7. **İş Adı**'nı belirtin, **Analytics Hesabı**'nı doğrulayın ve ardından **Gönder**'e tıklayın. Gönderim tamamlandığında, gönderme işleminin sonuçları ve iş bağlantısı Visual Studio için Data Lake Araçları içinde sunulur.

    ![U-SQL Visual Studio projesini gönderme](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)

8. En son iş durumunu görmek ve ekranı yenilemek için Yenile düğmesine tıklamanız gerekir. İş başarılı olduğunda **İş Grafiği**, **Meta Veri İşlemleri**, **Durum Geçmişi**, **Tanılama** bilgilerini size gösterir:

    ![U-SQL Visual Studio Data Lake Analytics iş performans grafiği](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

    * İş Özeti. Geçerli işe yönelik özet bilgilerini gösterir. Örneğin: Durum, İlerleme Durumu, Yürütme Zamanı, Çalışma Zamanı Adı, Gönderen vb.   
    * İş Ayrıntıları. Betik, kaynak, Köşe Yürütme Görünümü de dahil olmak üzere, bu işe yönelik ayrıntılı bilgiler sağlanır.
    * İş Grafiği. İşe yönelik bilgileri görselleştirmek için dört grafik sağlanır: İlerleme Durumu, Okunan Veriler, Yazılan Veriler, Yürütme Zamanı, Düğüm Başına Ortalama Yürütme Zamanı, Giriş İşleme Birimi, Çıkış İşleme Birimi.
    * Meta Veri İşlemleri. Bu, tüm meta veri işlemlerini gösterir.
    * Durum Geçmişi.
    * Tanılama. Visual Studio için Data Lake Araçları, iş yürütmeyi otomatik olarak tanılar. İşlerinde birtakım hatalar veya performans sorunları olduğunda uyarı alırsınız. Daha fazla bilgi için İş Tanılama (bağlantı henüz belirlenmedi) bölümüne bakın.

**İş durumu denetlemek için**

1. Sunucu Gezgini'nden, **Azure** seçeneğini, **Data Lake Analytics** seçeneğini ve Data Lake Analytics hesap adını genişletin.
2. İşleri listelemek için **İşler** seçeneğine çift tıklayın.
2. Durumu görmek için işe tıklayın.

**İş çıkışını görmek için**

1. **Sunucu Gezgini**'nden, **Azure** seçeneğini, **Data Lake Analytics** seçeneğini, Data Lake Analytics hesabınızı ve **Depolama Hesapları** seçeneğini genişletin, varsayılan Data Lake Store hesabına sağ tıklayın ve ardından **Explorer**'a tıklayın.
2.  Klasörü açmak için **çıkış** seçeneğine çift tıklayın
3.  **SearchLog-From-adltools.csv** dosyasına çift tıklayın.


###İş Kayıttan Yürütme

İş kayıttan yürütme, iş yürütme ilerleme durumunuzu izlemenize ve performans anormalliklerini ve sorunlarını görsel olarak algılamanıza olanak tanır. İş yürütme tamamlanmadan önce (işin etkin şekilde çalıştığı zaman) ve yürütme tamamlandıktan sonra bu özellik kullanılabilir. İş yürütme sırasında kayıttan yürütmenin gerçekleştirilmesi; kullanıcının, ilerleme durumunu güncel zamana kadar kayıttan yürütmesine olanak tanır.

**İş yürütme ilerleme durumunu görüntülemek için**  

1. Sağ üst köşede **Yük Profili** seçeneğine tıklayın. Önceki ekran görüntüsüne bakın.
2. İş yürütme ilerleme durumunu gözden geçirmek için sol alt köşedeki Yürüt düğmesine tıklayın.
3. Kayıttan yürütme sırasında, işlemi durdurmak için **Duraklat**'a tıklayın veya ilerleme çubuğunu doğrudan belirli konumlara sürükleyin.


###Isı Haritası

Visual Studio için Data Lake Araçları, her bir aşamaya yönelik ilerleme durumu, veri G/Ç, yürütme zamanı, G/Ç işleme birimi bilgilerini göstermek üzere iş görünümünde kullanıcı tarafından seçilebilen renk yer paylaşımları sağlar. Bu sayede, kullanıcılar olası sorunları ve iş özelliklerinin dağıtımını doğrudan ve sezgisel olarak anlayabilir. Açılır listeden görüntülemek üzere bir veri kaynağı seçebilirsiniz.  

## U-SQL'i yerel olarak çalıştırma

Visual Studio'da U-SQL yerel çalıştırma deneyimini kullanarak şunları yapabilirsiniz:

- C# Derlemeleri'nin yanı sıra, u-SQL betiklerini yerel olarak çalıştırma.
- C# derlemeleri üzerinde yerel olarak hata ayıklama.
- Sunucu Gezgini'nde yerel veritabanlarını, derlemeleri, şemaları ve tabloları Azure Data Lake Analytics hizmetinde olduğu gibi oluşturma/silme/görüntüleme.

Visual Studio'da bir *Yerel* hesap görürsünüz ve yükleyici, *C:\LocalRunRoot* konumunda bir *DataRoot* klasörü oluşturur. DataRoot klasörü şu amaçlarla kullanılır:

- Tablo, veritabanı ve TVF gibi öğeler de dahil olmak üzere, meta verileri depolama.
- Belirli bir betik için: Giriş/çıkış yollarında göreli bir yola başvurulmuşsa DataRoot aranır (giriş olması durumunda betiğin yolu için de geçerlidir)
- Bir derlemeyi kaydetmeye çalışıyor olmanız ve göreli yol kullanmanız halinde DataRoot klasörüne başvurulmaz (daha ayrıntılı bilgi için "Yerel çalıştırma gerçekleştirirken derlemeleri kullanma" kısmına bakın)

Aşağıdaki videoda U-SQL yerel çalıştırma özelliği gösterilmektedir:

>[AZURE.VIDEO usql-localrun]

### Bilinen sorunlar ve sınırlamalar

- U-SQL Yerel Çalıştırması, dosya kümelerinin yerel olarak sorgulanmasını desteklemiyor. Bkz. [U-SQL dosya kümeleri](https://msdn.microsoft.com/library/azure/mt621294.aspx). Bu sorun ilerleyen zamanlarda çözülecektir.
- İş planları seri olarak tek bir işlemde çalıştırıldığı için düşük paralellikten kaynaklanan yavaş performans.
- Yerel çalıştırma, iş grafiklerini Visual Studio'da gösteremiyor. Bu, ilerleyen zamanlarda ele alınacaktır.
- Yerel hesap için Sunucu Gezgini'nde tablo/veritabanı vb. oluşturulamıyor.
- Göreli yola başvurulduğunda:

    - Betik girişinde (EXTRACT * FROM "/yol/abc"): hem DataRoot yolu hem de betik yolu aranır.
    - Betik çıkışında (OUTPUT TO "yol/abc"): DataRoot yolu, çıkış klasörü olarak kullanılır.
    - Derleme kaydında (CREATE ASSEMBLY xyz FROM “/path/abc”): betik yolu aranır, ancak DataRoot aranmaz.
    - Kayıtlı TVF/Görünüm veya diğer meta veri varlıklarında: DataRoot Yolu aranır ancak betik yolu aranmaz.

    Data Lake hizmeti üzerinde çalıştırılan betikler için, varsayılan depolama hesabı, kök klasör olarak kullanılır ve uygun şekilde aranır.

### U-SQL betiklerini yerel olarak test etme
U-SQL betikleri geliştirmeye yönelik yönergeler için bkz. [U-SQL betikleri geliştirme](#develop-and-test-u-sql-scripts). U-SQL betiklerini yerel olarak oluşturup çalıştırmak için küme açılır listesinde **(Yerel)** öğesini seçin ve ardından **Gönder**'e tıklayın. Lütfen doğru verilere başvurulduğundan emin olun; mutlak yola başvurun veya verileri DataRoot klasörünün altına yerleştirin.

![U-SQL Visual Studio projesini yerel olarak gönderme](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-local-run.png)

Ayrıca, bir betiğe sağ tıklayıp ardından bağlam menüsünde **Yerel Planı Çalıştır**'a tıklayabilir veya yerel çalıştırmayı tetiklemek için **CTRL+F5** tuşlarına basabilirsiniz.

### Yerel çalıştırmada derlemeleri kullanma

Özelleştirilmiş C# dosyalarını çalıştırmak için iki yol mevcuttur:

- Derlemeler dosyanın arkasındaki kodda yazın; böylece derlemeler, betiğin tamamlanmasının ardından otomatik olarak kaydedilir ve bırakılır.
- Bir C# derleme projesi oluşturun ve çıkış dll'sini aşağıdaki gibi bir betik ile yerel hesaba kaydedin. Lütfen yolun DataRoot klasörüne değil, betiğe göreli olduğunu unutmayın.

![U-SQL çalıştırmasında derlemeleri kullanma](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-local-run-assembly.png)

### Betikler ve C# derlemeleri üzerinde yerel olarak hata ayıklama

C# derlemeleri üzerinde hata ayıklama işlemini, Azure Data Lake Analytics Hizmeti'ne göndererek ve kaydederek gerçekleştirebilirsiniz. Hem dosyanın arkasındaki kodda hem de başvuruda bulunulan bir C# projesinde kesme noktaları ayarlayabilirsiniz.

**Dosyanın arkasındaki kodda yerel kod hatalarını ayıklamak için**
1.  Dosyanın arkasındaki kodda kesme noktaları ayarlayın.
2.  Betik üzerinde yerel olarak hata ayıklama gerçekleştirmek için **F5**'e basın.

Aşağıdaki yordam yalnızca Visual Studio 2015'te çalışır. Daha eski Visual Studio sürümlerinde, pdb dosyalarını kendiniz eklemeniz gerekebilir.

**Başvuruda bulunulan bir C# projesinde yerel kod hatalarını ayıklamak için**
1.  Bir C# Derleme projesi oluşturun ve projeyi çıkış dll'sini üretmek üzere oluşturun.
2.  U-SQL deyimi kullanarak dll'yi kaydetme:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
3.  C# kodunda kesme noktalarını ayarlayın.
4.  C# dll'sine yerel olarak başvuruda bulunarak koddaki hataları ayıklamak için **F5**'e basın.  

##Ayrıca bkz.

Farklı araçlar kullanarak Data Lake Analytics ile çalışmaya başlamak için bkz.

- [Azure Portal'ı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
- [Azure PowerShell'i kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
- [.NET SDK'yı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-net-sdk.md)

Daha fazla geliştirme konu başlığı görmek için:

- [Data Lake Analytics'i kullanarak web günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md)
- [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
- [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md)
- [Data Lake Analytics işleri için U-SQL kullanıcı tanımlı işleçleri geliştirme](data-lake-analytics-u-sql-develop-user-defined-operators.md)

##Ek - Öğreticiyi hazırlamaya yönelik bir PowerShell örneği

Aşağıdaki PowerShell betiği, sizin için bir Azure Data Lake Analytics hesabı ve veri kaynağı hazırlar; bu nedenle [U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md#develop-u-sql-scripts) bölümüne geçebilirsiniz.

    #region - used for creating Azure service names
    $nameToken = "<Enter an alias>"
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion

    #region - service names
    $resourceGroupName = $namePrefix + "rg"
    $dataLakeStoreName = $namePrefix + "adas"
    $dataLakeAnalyticsName = $namePrefix + "adla"
    $location = "East US 2"
    #endregion


    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - Create an Azure Data Lake Analytics service account
    Write-Host "Create a resource group ..." -ForegroundColor Green
    New-AzureRmResourceGroup `
        -Name  $resourceGroupName `
        -Location $location

    Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
    New-AzureRmDataLakeStoreAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeStoreName `
        -Location $location

    Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
    New-AzureRmDataLakeAnalyticsAccount `
        -Name $dataLakeAnalyticsName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultDataLake $dataLakeStoreName

    Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
    Get-AzureRmDataLakeAnalyticsAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeAnalyticsName  
    #endregion

    #region - prepare the source data
    Write-Host "Import the source data ..."  -ForegroundColor Green
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file.
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

    Write-Host "List the source data ..."  -ForegroundColor Green
    Get-AzureRmDataLakeStoreChildItem -Account $dataLakeStoreName -Path  "/Samples/Data/"
    #endregion



<!--HONumber=Aug16_HO1-->


