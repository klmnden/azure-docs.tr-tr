---
title: "Visual Studio için Data Lake Araçları&quot;nı kullanarak U-SQL betikleri geliştirme | Microsoft Docs"
description: "Visual Studio için Data Lake Araçları&quot;nı nasıl yükleyeceğinizi, U-SQL betiklerini nasıl geliştirip test edeceğinizi öğrenin. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/06/2017
ms.author: edmaca, yanacai
translationtype: Human Translation
ms.sourcegitcommit: 0b53a5ab59779dc16825887b3c970927f1f30821
ms.openlocfilehash: c26ac89bd7ef494331ba309aacf87de03506ac4c
ms.lasthandoff: 04/07/2017


---
# <a name="tutorial-develop-u-sql-scripts-using-data-lake-tools-for-visual-studio"></a>Öğretici: Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri yazın ve test edin.

U-SQL, veri gölü içindeki tüm verilerin ve daha fazlasının hazırlanması, dönüştürülmesi ve analiz edilmesi için kullanılan oldukça ölçeklenebilir, yüksek düzeyde genişletilebilir bir dildir. Daha fazla bilgi edinmek için bkz. [U-SQL Başvurusu](http://go.microsoft.com/fwlink/p/?LinkId=691348).

## <a name="prerequisites"></a>Ön koşullar
* **Visual Studio 2017 (veri depolama ve işleme iş yükü altında), Visual Studio 2015 güncelleştirme 3, Visual Studio 2013 güncelleştirme 4 veya Visual Studio 2012. Enterprise (Ultimate/Premium), Professional, Community sürümleri desteklenir; Express sürümü desteklenmez.**
* **.NET sürüm 2.7.1 veya üzeri için Microsoft Azure SDK**.  [Web platformu yükleyicisini](http://www.microsoft.com/web/downloads/platform.aspx) kullanarak yükleyin.
* **[Visual Studio için Data Lake Araçları](http://aka.ms/adltoolsvs)**.

    Visual Studio için Data Lake Araçları yüklendikten sonra, "Azure" düğümünün altında Sunucu Gezgini içinde "Data Lake Analytics" düğümünü göreceksiniz (Sunucu Gezginini Ctrl+Alt+S tuşlarına basarak açın).

* **Data Lake Analytics hesabı ve örnek veriler** Data Lake Araçları Data Lake Analytics hesabı oluşturmayı desteklemez. Azure portalı, Azure PowerShell, .NET SDK veya Azure CLI kullanarak bir hesap oluşturun.
Size kolaylık sağlamak amacıyla, bir Data Lake Analytics hizmeti oluşturmaya ve kaynak veri dosyasını yüklemeye yönelik bir PowerShell betiği, [Ek A - Öğreticiyi hazırlamaya yönelik PowerShell örneğinde](data-lake-analytics-data-lake-tools-get-started.md#appx-a-powershell-sample-for-preparing-the-tutorial) bulunabilir.

    İsteğe bağlı olarak, hesabınızı oluşturmak ve verileri karşıya el ile yüklemek için [Azure portalını kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md) konu başlığının aşağıdaki iki bölümünü tamamlayabilirsiniz:

    1. [Azure Data Lake Analytics hesabı oluşturma](data-lake-analytics-get-started-portal.md#create-data-lake-analytics-account).
    2. [SearchLog.tsv dosyasını varsayılan Data Lake Storage hesabına yükleme](data-lake-analytics-get-started-portal.md#prepare-source-data).

## <a name="connect-to-azure"></a>Azure'a Bağlanma
**Data Lake Analytics'e bağlanma**

1. Visual Studio'yu açın.
2. **Görünüm** menüsünde, **Sunucu Gezgini**'ne tıklayarak Sunucu Gezgini'ni açın. Veya **[CTRL]+[ALT]+S** tuşlarına basın.
3. **Azure**'a sağ tıklayın, "Microsoft Azure Aboneliğine Bağlan" seçeneğine tıklayın ve ardından yönergeleri izleyin.
4. **Sunucu Gezgini**'nden, **Azure** seçeneğini ve ardından **Data Lake Analytics** seçeneğini genişletin. Varsa Data Lake Analytics hesaplarınızın listesini görürsünüz. Visual Studio'dan Data Lake Analytics hesabı oluşturamazsınız. Hesap oluşturmak için bkz. [Azure portalını kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md) veya [Azure PowerShell'i kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md).

## <a name="upload-source-data-files"></a>Kaynak veri dosyalarını yükleme
Öğreticinin daha önceki **Önkoşul** bölümünde bazı verileri yüklediniz.  

Kendi verilerinizi kullanmak için Data Lake Araçları'ndan veri yüklemeye yönelik aşağıdaki adımları izleyin.

**Bağımlı Azure Data Lake hesabına dosya yükleme**

1. **Sunucu Gezgini**'nden, **Azure** seçeneğini, **Data Lake Analytics** seçeneğini, Data Lake Analytics hesabınızı ve **Storage Hesapları** seçeneğini genişletin. Varsayılan Data Lake Storage hesabını, bağlı Data Lake Storage hesaplarını ve bağlı Azure Storage hesaplarını göreceksiniz. Varsayılan Data Lake hesabı "Varsayılan Storage Hesabı" etiketine sahiptir.
2. Varsayılan Data Lake Store hesabına sağ tıklayın ve ardından **Explorer**'a tıklayın.  Bu, Visual Studio Gezgini bölmesinde Data Lake Araçları'nı açar.  Sol tarafta bir ağaç görünümü gösterilirken, içerik görünümü sağ tarafta bulunur.
3. Dosyaları yüklemek istediğiniz klasöre göz atın,
4. Herhangi bir boş alana sağ tıklayın ve ardından **Yükle**'ye tıklayın.

    ![U-SQL Visual Studio projesi U-SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-upload-files.png)

**Bağlantılı bir Azure Blob depolama hesabına dosya yükleme**

1. **Sunucu Gezgini**'nden, **Azure** seçeneğini, **Data Lake Analytics** seçeneğini, Data Lake Analytics hesabınızı ve **Storage Hesapları** seçeneğini genişletin. Varsayılan Data Lake Storage hesabını, bağlı Data Lake Storage hesaplarını ve bağlı Azure Storage hesaplarını göreceksiniz.
2. Azure Storage Hesabı'nı genişletin.
3. Dosyaları yüklemek istediğiniz kapsayıcıya sağ tıklayın ve ardından **Explorer**'a tıklayın. Bir kapsayıcınız yoksa ilk olarak Azure portalı, Azure PowerShell'i veya diğer araçları kullanarak bir tane oluşturmanız gerekir.
4. Dosyaları yüklemek istediğiniz klasöre göz atın,
5. Herhangi bir boş alana sağ tıklayın ve ardından **Yükle**'ye tıklayın.

## <a name="develop-u-sql-scripts"></a>U-SQL betikleri geliştirme
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

    Kaynak dosyayı farklı bir konuma kopyalamadıysanız bu iki yolu değiştirmeyin.  Data Lake Analytics, mevcut olmaması halinde çıktı klasörünü oluşturur.

    Varsayılan Data Lake hesaplarında depolanan dosyalar için göreli yolların kullanılması daha basittir. Mutlak yol da kullanabilirsiniz.  Örneğin:

        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Mutlak yolları, bağlı Storage hesaplarındaki dosyalara erişmek için kullanmanız gerekir.  Bağlı Azure Storage hesabında depolanan dosyalar için söz dizimi şu şekildedir:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

   > [!NOTE]
   > Ortak blob veya ortak kapsayıcı erişim izinlerine sahip Azure Blob kapsayıcısı şu an için desteklenmemektedir.  
   >
   >

    Aşağıdaki özelliklere dikkat edin:

   * **IntelliSense**

       Ad otomatik olarak tamamlanır ve Satır Kümesi, Sınıflar, Veritabanları, Şemalar ve Kullanıcı Tanımlı Nesneler (UDO'lar) için üyeler gösterilir.

       Katalog varlıkları (Veritabanları, Şemalar, Tablolar, UDO'lar vb.) için IntelliSense, işlem hesabınızla ilgilidir. Geçerli etkin işlem hesabını, veritabanını ve şemayı üst araç çubuğundan denetleyebilir ve bunları açılır listelerden değiştirebilirsiniz.
   * *** sütunlarını genişletme**

       * öğesinin sağına tıklayın; * öğesinin altında mavi bir alt çizgi göreceksiniz. Fare imlecinizi mavi alt çizginin üzerine getirin ve sonra aşağı oka tıklayın.
       ![Data Lake Visual Studio araçlarını genişletme *](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-expand-asterisk.png)

       **Sütunları Genişlet**'e tıklayın; araç, * simgesi yerine sütun adlarını getirecektir.
   * **Otomatik Biçimlendirme**

       Kullanıcılar, Düzenle->Gelişmiş altındaki kod yapısına bağlı olarak U-SQL betiğinin girintisini değiştirebilir:

     * Belgeyi Biçimlendir (Ctrl+E, D): Tüm belgeyi biçimlendirir   
     * Seçimi Biçimlendir (Ctrl+K, Ctrl+F): Seçimi biçimlendirir. Seçim yapılmazsa bu kısayol, imlecin bulunduğu satırı biçimlendirir.  

       Tüm biçimlendirme kuralları, Araçlar->Seçenekler->Metin Düzenleyici->SIP->Biçimlendirme altında yapılandırılabilir.  
   * **Akıllı Girintileme**

       Visual Studio için Data Lake Araçları, siz betik yazarken ifadelere otomatik olarak girinti ekleyebilir. Bu özellik varsayılan olarak devre dışıdır ve kullanıcıların özelliği U-SQL->Seçenekler ve Ayarlar->Anahtarlar-> Akıllı Girintiyi Etkinleştir seçeneğini işaretleyerek etkinleştirmesi gerekir.
   * **Tanıma Gitme ve Tüm Başvuruları Bulma**

       Bir Satır Kümesi/parametre/sütun/UDO vb. öğenin adına sağ tıklayın Tanıma Git (F12) seçeneğine tıklanması, öğenin tanımına gitmenizi sağlar. Tüm Başvuruları Bul (Shift+F12) seçeneğine tıklanması, tüm başvuruları gösterir.
   * **Azure Yolu Ekleme**

       Visual Studio için Data Lake Araçları, Azure dosya yolunu hatırlayıp betik yazarken el ile yazma seçeneğinin yerine kolay bir yol sunar: Düzenleyicinin içine sağ tıklayıp Azure Yolu Ekle'ye tıklayın. Azure Blob Tarayıcısı iletişim kutusundaki dosyaya gidin. **Tamam** düğmesine tıklayın. dosya yolu, kodunuza eklenir.
5. Data Lake Analytics hesabını, Veritabanı'nı ve Şema'yı belirtin. Test amacıyla betiği yerel olarak çalıştırmak için **(yerel)** öğesini seçebilirsiniz. Daha fazla bilgi için bkz. [U-SQL'yi yerel olarak çalıştırma](#run-u-sql-locally).

    ![U-SQL Visual Studio projesini gönderme](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

    Daha fazla bilgi için bkz. [U-SQL kataloğunu kullanma](data-lake-analytics-use-u-sql-catalog.md).
6. **Çözüm Gezgini**'nden, **Script.usql** öğesine sağ tıklayın ve ardından **Betik Oluştur**'a tıklayın. Çıktı bölmesinde sonucu doğrulayın.
7. **Çözüm Gezgini**'nden, **Script.usql** öğesine sağ tıklayın ve ardından **Betiği Gönder**'e tıklayın. İsteğe bağlı olarak, Script.usql bölmesinden **Gönder**'e de tıklayabilirsiniz.  Önceki ekran görüntüsüne bakın.  Gelişmiş seçenekleri kullanarak göndermek için Gönder düğmesinin yanındaki aşağı oka tıklayın:
8. **İş Adı**'nı belirtin, **Analytics Hesabı**'nı doğrulayın ve ardından **Gönder**'e tıklayın. Gönderim tamamlandığında, gönderme işleminin sonuçları ve iş bağlantısı Visual Studio için Data Lake Araçları içinde sunulur.

    ![U-SQL Visual Studio projesini gönderme](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
9. En son iş durumunu görmek ve ekranı yenilemek için Yenile düğmesine tıklamanız gerekir. İş başarılı olduğunda **İş Grafiği**, **Meta Veri İşlemleri**, **Durum Geçmişi**, **Tanılama** bilgilerini size gösterir:

    ![U-SQL Visual Studio Data Lake Analytics iş performans grafiği](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * İş Özeti. Geçerli işe yönelik özet bilgilerini gösterir. Örneğin: Durum, İlerleme Durumu, Yürütme Zamanı, Çalışma Zamanı Adı, Gönderen vb.   
   * İş Ayrıntıları. Betik, kaynak, Köşe Yürütme Görünümü de dahil olmak üzere, bu işe yönelik ayrıntılı bilgiler sağlanır.
   * İş Grafiği. İşe yönelik bilgileri görselleştirmek için dört grafik sağlanır: İlerleme Durumu, Okunan Veriler, Yazılan Veriler, Yürütme Zamanı, Düğüm Başına Ortalama Yürütme Zamanı, Girdi İşleme Birimi, Çıktı İşleme Birimi.
   * Meta Veri İşlemleri. Bu, tüm meta veri işlemlerini gösterir.
   * Durum Geçmişi.
   * Tanılama. Visual Studio için Data Lake Araçları, iş yürütmeyi otomatik olarak tanılar. İşlerinde birtakım hatalar veya performans sorunları olduğunda uyarı alırsınız. Daha fazla bilgi için İş Tanılama (bağlantı henüz belirlenmedi) bölümüne bakın.

**İş durumunu denetlemek için**

1. Sunucu Gezgini'nden, **Azure** seçeneğini, **Data Lake Analytics** seçeneğini ve Data Lake Analytics hesap adını genişletin.
2. İşleri listelemek için **İşler** seçeneğine çift tıklayın.
3. Durumu görmek için işe tıklayın.

**İş çıkışını görmek için**

1. **Sunucu Gezgini**'nden, **Azure** seçeneğini, **Data Lake Analytics** seçeneğini, Data Lake Analytics hesabınızı ve **Depolama Hesapları** seçeneğini genişletin, varsayılan Data Lake Store hesabına sağ tıklayın ve ardından **Explorer**'a tıklayın.
2. Klasörü açmak için **çıktı** seçeneğine çift tıklayın.
3. **SearchLog-From-adltools.csv** dosyasına çift tıklayın.

### <a name="job-playback"></a>İş Kayıttan Yürütme
İş kayıttan yürütme, iş yürütme ilerleme durumunuzu izlemenize ve performans anormalliklerini ve sorunlarını görsel olarak algılamanıza olanak tanır. İş yürütme tamamlanmadan önce (işin etkin şekilde çalıştığı zaman) ve yürütme tamamlandıktan sonra bu özellik kullanılabilir. İş yürütme sırasında kayıttan yürütmenin gerçekleştirilmesi; kullanıcının, ilerleme durumunu güncel zamana kadar kayıttan yürütmesine olanak tanır.

**İş yürütme ilerleme durumunu görüntülemek için**  

1. Sağ üst köşede **Yük Profili** seçeneğine tıklayın. Önceki ekran görüntüsüne bakın.
2. İş yürütme ilerleme durumunu gözden geçirmek için sol alt köşedeki Yürüt düğmesine tıklayın.
3. Kayıttan yürütme sırasında, işlemi durdurmak için **Duraklat**'a tıklayın veya ilerleme çubuğunu doğrudan belirli konumlara sürükleyin.

### <a name="heat-map"></a>Isı Haritası
Visual Studio için Data Lake Araçları, her bir aşamaya yönelik ilerleme durumu, veri G/Ç, yürütme zamanı, G/Ç işleme birimi bilgilerini göstermek üzere iş görünümünde kullanıcı tarafından seçilebilen renk yer paylaşımları sağlar. Bu sayede, kullanıcılar olası sorunları ve iş özelliklerinin dağıtımını doğrudan ve sezgisel olarak anlayabilir. Açılır listeden görüntülemek üzere bir veri kaynağı seçebilirsiniz.  

## <a name="run-u-sql-locally"></a>U-SQL'i yerel olarak çalıştırma

İş istasyonunuzda U-SQL işleri çalıştırmak için, Azure Data Lake hizmetinde yaptığınız gibi Visual Studio için Azure Data Lake Araçları ve Azure Data Lake U-SQL SDK’sını kullanabilirsiniz. Bu iki yerel çalıştırma özelliği, U-SQL işlerinizi test etme ve hata ayıklama işlemlerinde size zaman kazandırır. 

* [Yerel çalıştırma ve Azure Data Lake U-SQL SDK’sını kullanarak U-SQL işlerini test etme ve hatalarını ayıklama](data-lake-analytics-data-lake-tools-local-run.md)


## <a name="see-also"></a>Ayrıca bkz.
Farklı araçlar kullanarak Data Lake Analytics ile çalışmaya başlamak için bkz.

* [Azure portalını kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [Azure PowerShell'i kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
* [.NET SDK'yı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-net-sdk.md)
* [U-SQL işlerinde C# kodu hatalarını ayıklama](data-lake-analytics-debug-u-sql-jobs.md)

Visual Studio Code’a yönelik Data Lake Araçları’nı öğrenmek için bkz. [Visual Studio Code için Azure Data Lake Araçları’nı kullanma](data-lake-analytics-data-lake-tools-for-vscode.md).

Daha fazla geliştirme konu başlığı görmek için:

* [Data Lake Analytics'i kullanarak web günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
* [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md)
* [Data Lake Analytics işleri için U-SQL kullanıcı tanımlı işleçleri geliştirme](data-lake-analytics-u-sql-develop-user-defined-operators.md)

## <a name="appx-a-powershell-sample-for-preparing-the-tutorial"></a>Ek - Öğreticiyi hazırlamaya yönelik bir PowerShell örneği
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

