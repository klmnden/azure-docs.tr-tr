---
title: "Azure Data Lake araçları: Kullanım Azure Data Lake araçları Visual Studio kodunu | Microsoft Docs"
description: "Visual Studio Code için Azure Data Lake araçları oluşturun, test ve U-SQL betikleri çalıştırmak için nasıl kullanılacağını öğrenin. "
Keywords: VScode,Azure Data Lake Tools,Local run,Local debug,Local Debug,preview file,upload to storage path,download,upload
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/10/2017
ms.author: jejiang
ms.openlocfilehash: 60307b8b16718fdc947bde7616532fa6a0920cf0
ms.sourcegitcommit: 21a58a43ceceaefb4cd46c29180a629429bfcf76
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2017
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Azure Data Lake araçları Visual Studio kodunu kullanın

Visual Studio Code (VS oluşturun, test kodu) için Azure Data Lake araçları öğrenin ve U-SQL betikleri çalıştırın. Bilgiler aşağıdaki videoda da ele alınmıştır:

<a href="https://channel9.msdn.com/Series/AzureDataLake/Azure-Data-Lake-Tools-for-VSCode?term=ADL%20Tools%20for%20VSCode"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Ön koşullar

VSCode için Azure Data Lake araçları, Windows, Linux ve MacOS destekler.  

- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).

MacOS ve Linux için:
- [.NET core SDK 2.0](https://www.microsoft.com/net/download/core). 
- [Mono 5.2.x](http://www.mono-project.com/download/).

## <a name="install-data-lake-tools"></a>Data Lake araçları yükleme

Önkoşulları yüklendikten sonra VS Code için Data Lake araçları yükleyebilirsiniz.

**Data Lake araçları yüklemek için**

1. Visual Studio Code'da açın.
2. Tıklatın **uzantıları** sol bölmede. Girin **Azure Data Lake** arama kutusuna.
3. Tıklatın **yükleme** yanına **Azure Data Lake Araçları**. Birkaç saniye sonra **yükleme** düğmesi değiştirilecek **yeniden**.
4. Tıklatın **yeniden** etkinleştirmek için **Azure Data Lake Araçları** uzantısı.
5. Tıklatın **yeniden yükleme penceresi** onaylamak için. Gördüğünüz **Azure Data Lake Araçları** uzantıları bölmesinde.

    ![Visual Studio kod uzantılarını bölmesi için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

 
## <a name="activate-azure-data-lake-tools"></a>Azure Data Lake araçları etkinleştirme
Yeni bir .usql dosyası oluşturun veya uzantıyı etkinleştirmek için mevcut bir .usql dosyasını açın. 

## <a name="open-the-sample-script"></a>Örnek komut dosyasını açın
Komut palet (Ctrl + Shift + P) açın ve girin **ADL: açık örnek komut dosyası**. Bu örnek başka bir örneği açılır. Ayrıca düzenleyebilir, yapılandırmak ve bu örnek komut gönderme.

## <a name="work-with-u-sql"></a>U-SQL ile çalışma

U-SQL dosya veya U-SQL ile çalışmak için bir klasör açın.

**U-SQL projeniz için bir klasör açmak için**

1. Visual Studio koddan seçin **dosya** menüsüne ve ardından **Klasör Aç**.
2. Bir klasör belirtin ve ardından **Klasör Seç**.
3. Seçin **dosya** menüsüne ve ardından **yeni**. Adsız-1 dosyası projeye eklenir.
4. Adsız-1 dosyasına aşağıdaki kodu girin:

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO "/Output/departments.csv"
        USING Outputters.Csv();

    Komut dosyası/Output klasöründe bulunan bazı veriler içeren bir departments.csv dosyası oluşturur.

5. Dosyayı Farklı Kaydet **myUSQL.usql** açılan klasörde. Bir xxx_settings.json yapılandırma dosyası, klasöre de eklenir.
6. Açın ve aşağıdaki özelliklere sahip xxx_settings.json yapılandırın:

    - : Bir Data Lake Analytics hesabı Azure aboneliğinizdeki derlemek ve U-SQL işleri, derleme ve U-SQL işleri çalıştırma önce bilgisayar hesabını yapılandırmanız çalıştırmak için gereklidir.
    - Veritabanı: Hesabınız altındaki bir veritabanı. Varsayılan değer **ana**.
    - Şema: Veritabanınızı altında bir şema. Varsayılan değer **dbo**.
    - İsteğe bağlı ayarlar:
        - Öncelik: Öncelik 1 ile-1 ile 1000 en yüksek öncelikli olarak aralıktır. Varsayılan değer **1000**.
        - Paralellik: Paralellik 1 ila 150 için aralıktır. Azure Data Lake Analytics hesabınızı izin verilen maksimum paralellik varsayılan değerdir. 
        
        ![Visual Studio Code yapılandırma dosyası için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-configuration-file.png)
      
        > [!NOTE] 
        > Yapılandırma kaydedildikten sonra hesap, veritabanı ve şema bilgilerini karşılık gelen .usql dosyası sol alt köşesindeki durum çubuğunda görünür.

**U-SQL betiği derlemek için**

1. Komut paletini açmak için Ctrl + Shift + P seçin. 
2. Girin **ADL: derleme betik**. Derleme sonuçları görünür **çıkış** penceresi. Ayrıca bir komut dosyasını sağ tıklatın ve ardından **ADL: derleme betik** bir U-SQL işi derlemek için. Derleme sonucu görünür **çıkış** bölmesi.
 

**U-SQL betiği göndermek için**

1. Komut paletini açmak için Ctrl + Shift + P seçin. 
2. Girin **ADL: işi göndermek**.  Ayrıca bir komut dosyasını sağ tıklatın ve ardından **ADL: işi Gönder**. 

U-SQL işi gönderdikten sonra gönderme günlükleri görünür **çıkış** VS Code penceresinde. Gönderim başarılı olursa, proje URL'si de görüntülenir. Proje URL'si gerçek zamanlı iş durumunu izlemek için bir web tarayıcısı açabilirsiniz.

İş ayrıntılarını çıktısını etkinleştirmek için ayarlanmış **jobInformationOutputPath** içinde **vs code için u sql_settings.json** dosya.
 
## <a name="use-python-r-and-csharp-code-behind-file"></a>Python, R ve CSharp arka plan kod dosyası kullanın
Azure Data Lake aracı destekleyen birden çok özel kod yönergelere bakın [geliştirmek U-SQL ile Python, R ve Azure Data Lake Analytics VSCode içinde CSharp](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md).

## <a name="use-assemblies"></a>Derlemeleri kullanma

Derlemeler geliştirme hakkında daha fazla bilgi için bkz: [Azure Data Lake Analytics işleri için U-SQL geliştirmek derlemeleri](data-lake-analytics-u-sql-develop-assemblies.md).

Data Lake araçları, Data Lake Analytics Kataloğu'nda özel kod derlemeleri kaydetmek için kullanabilirsiniz.

**Bir derlemeyi kaydetmek için**

Derleme aracılığıyla kaydedebilirsiniz **ADL: kaydetmek derleme** veya **ADL: kaydetmek derleme yapılandırma yoluyla** komutları.

**ADL kaydetmek için: kayıt derleme komutu**
1.  Komut paletini açmak için Ctrl + Shift + P seçin.
2.  Girin **ADL: kaydetmek derleme**. 
3.  Yerel derleme yolu belirtin. 
4.  Bir Data Lake Analytics hesabı seçin.
5.  Bir veritabanı seçin.

Sonuçları: Portal bir tarayıcıda açıldığından ve derleme kayıt işlemi görüntüler.  

Tetiklemek için kullanışlı bir başka yolu **ADL: kaydetmek derleme** komuttur dosya Gezgini'nde .dll dosyasını sağ tıklatın. 

**ADL yine de kaydetmek için: kayıt derleme yapılandırma komutu**
1.  Komut paletini açmak için Ctrl + Shift + P seçin.
2.  Girin **ADL: kaydetmek derleme yapılandırma yoluyla**. 
3.  Yerel derleme yolu belirtin. 
4.  JSON dosyası görüntülenir. Gözden geçirin ve kaynak parametreleri ve derleme bağımlılıklar gerekiyorsa düzenleyin. Yönergeler görüntülenir **çıkış** penceresi. Derleme kayıt aşamasına ilerlemek için (Ctrl + S) JSON dosyasını kaydedin.

![Visual Studio Code arka plan kod için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
>[!NOTE]
>- Derleme bağımlılıkları: Azure Data Lake araçları autodetects DLL bağımlılıkları sahip olup olmadığını belirler. Algılandıkları sonra bağımlılıkları JSON dosyasında görüntülenir. 
>- Kaynaklar: Derleme kaydı bir parçası olarak, DLL kaynakları (örneğin, .txt, .png ve .csv) karşıya yükleyebilir. 

Tetiklemek için başka bir yolu **ADL: kaydetmek derleme yapılandırma yoluyla** komuttur dosya Gezgini'nde .dll dosyasını sağ tıklatın. 

Aşağıdaki U-SQL kodu, bütünleştirilmiş çağırmayı gösterir. Örnekte, derleme adı: *test*.

```
REFERENCE ASSEMBLY [test];

@a = 
    EXTRACT 
        Iid int,
    Starts DateTime,
    Region string,
    Query string,
    DwellTime int,
    Results string,
    ClickedUrls string 
    FROM @"Sample/SearchLog.txt" 
    USING Extractors.Tsv();

@d =
    SELECT DISTINCT Region 
    FROM @a;

@d1 = 
    PROCESS @d
    PRODUCE 
        Region string,
    Mkt string
    USING new USQLApplication_codebehind.MyProcessor();

OUTPUT @d1 
    TO @"Sample/SearchLogtest.txt" 
    USING Outputters.Tsv();
```

## <a name="connect-to-azure"></a>Azure'a Bağlanma

Derleme ve Data Lake Analytics U-SQL betikleri çalıştırmak için önce Azure hesabınızda bağlanmanız gerekir.

**Azure'a bağlanmak için**

1.  Komut paletini açmak için Ctrl + Shift + P seçin. 
2.  Girin **ADL: oturum açma**. Oturum açma bilgileri görünür **çıkış** bölmesi.

    ![Data Lake araçları Visual Studio Code komutu paletini](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
    ![Visual Studio Code cihaz oturum açma bilgileri için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)
3. Seçin CTRL + üzerinde oturum açma URL'Sİ'ı tıklatın: oturum açma Web sayfası açmak için https://aka.ms/devicelogin. Kodu girin **G567LX42V** metin kutusuna yazın ve ardından **devam**.

   ![Visual Studio Code oturum açma için Data Lake araçları kodu yapıştırın](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png )   
4.  Web sayfasından oturum açmak için yönergeleri izleyin. Bağlandığınızda, Azure hesap adınızı sol alt köşesindeki durum çubuğunda görünür **VS Code** penceresi. 

    > [!NOTE] 
    > İki etmen etkin hesabınız varsa, PIN kullanmak yerine telefon kimlik doğrulaması kullanmanızı öneririz.

Oturumu kapatmak için aşağıdaki komutu girin **ADL: oturum kapatma**.

## <a name="list-your-data-lake-analytics-accounts"></a>Data Lake Analytics hesaplarını listeler

Bağlantıyı sınamak için Data Lake Analytics hesaplarının bir listesini alın.

**Azure aboneliğinizin Data Lake Analytics hesaplarla listelemek için**

1. Komut paletini açmak için Ctrl + Shift + P seçin.
2. Girin **ADL: liste hesapları**. Hesapları görünür **çıkış** bölmesi.


## <a name="access-the-data-lake-analytics-catalog"></a>Data Lake Analytics Kataloğu'na erişme

Azure'a bağlandıktan sonra U-SQL Kataloğu'na erişmek için aşağıdaki adımları kullanabilirsiniz.

**Azure Data Lake Analytics meta verilerine erişmek için**

1.  Ctrl + Shift + P seçin ve ardından girin **ADL: liste tabloları**.
2.  Data Lake Analytics hesaplardan birini seçin.
3.  Data Lake Analytics veritabanlarından birini seçin.
4.  Şemalar birini seçin. Tabloların listesini görebilirsiniz.

## <a name="view-data-lake-analytics-jobs"></a>Görünüm Data Lake Analytics işleri

**Data Lake Analytics işleri görüntülemek için**
1.  Komut palet (Ctrl + Shift + P) açın ve seçin **ADL: Göster iş**. 
2.  Bir Data Lake Analytics veya yerel bir hesap seçin. 
3.  İşleri listesini görünmesi hesabı için bekleyin.
4.  İş listesinden bir iş seçin, Data Lake araçları Azure Portalı'nda iş ayrıntılarını açar ve VS Code'da iş tanımı dosyası gösterir.

    ![Visual Studio kod IntelliSense nesne türleri için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="azure-data-lake-storage-integration"></a>Azure Data Lake Storage tümleştirme

Azure Data Lake Store ile ilgili komutları kullanabilirsiniz:
 - Azure Data Lake Storage kaynaklara göz atın. [Depolama alanı yolu listesinde](#list-the-storage-path). 
 - Azure Data Lake Storage dosyanın önizlemesini görüntüleyin. [Depolama dosya önizleme](#preview-the-storage-file). 
 - VS code'da Azure Data Lake Storage için doğrudan dosyası yükleyin. [Dosya veya klasör karşıya](#upload-file-or-folder).
 - Dosyayı doğrudan Azure Data Lake Storage VS code'da indirir. [Dosya indirme](#download-file).

## <a name="list-the-storage-path"></a>Depolama alanı yolu listesi 

**Komut palet depolama yolundan listelemek için**

Kod Düzenleyicisi'ni sağ tıklatın ve seçin **ADL: listesi yolu**.

Listede klasörü seçin veya tıklatın **yolu girin** veya **kökünden Gözat** (kullanan bir yolu girin örnek olarak). Select ->, **ADLA hesap**. Depolama klasör yolu girin veya Bul -> (örneğin: / çıkış /). Komut palet listeleri -> girişlerini temel yol bilgileri.

![Data Lake araçları Visual Studio Code için depolama birimi yolu sonuçları listesi](./media/data-lake-analytics-data-lake-tools-for-vscode/list-storage-path.png)

Sağ bağlam menüsü göreli yolu listelemek için daha kullanışlı bir yoludur.

**Sağ depolama yolundan listelemek için**

Seçmek için yol dizesi sağ **listesi yolu** devam etmek için.

![Visual Studio Code için Data Lake Araçları bağlam menüsü sağ tıklayın](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)


## <a name="preview-the-storage-file"></a>Depolama dosya önizleme

Kod Düzenleyicisi'ni sağ tıklatın ve seçin **ADL: Önizleme dosya**.

Seçin, **ADLA hesap**. Bir Azure depolama dosya yolu (örneğin, /output/SearchLog.txt) ENTER ->. Result ->: dosya içinde VSCode açılır.

   ![Visual Studio Code için Data Lake araçları dosya sonucu Önizleme](./media/data-lake-analytics-data-lake-tools-for-vscode/preview-storage-file.png)

Başka bir dosya önizlemesi için dosyanın tam yolunu veya dosyanın göreli yolu komut dosyası Düzenleyicisi'nde sağ tıklatma menüsünden aracılığıyla yoludur. 

## <a name="upload-file-or-folder"></a>Dosya veya klasör karşıya yükle

1. Kod Düzenleyicisi'ni sağ tıklatın ve seçin **dosyasını karşıya yükle** veya **karşıya yükleme klasörü**.

2. Bir dosya veya birden çok dosya seçin yüklerseniz dosya ya da seçin klasörü sonra yüklerseniz tüm klasörü tıklatın seçin seçin **karşıya**. Listesinden depolama klasörü Seç ->'yi tıklatın **yolu girin** veya **kökünden Gözat** (kullanan bir yolu girin örnek olarak). Select ->, **ADLA hesap**. Depolama klasör yolu girin veya Bul -> (örneğin: / çıkış /). Click -> **geçerli klasör seç** karşıya yükleme Hedefinizi belirtmek için.

   ![Visual Studio Code için Data Lake araçları karşıya yükleme durumu](./media/data-lake-analytics-data-lake-tools-for-vscode/upload-file.png)    


   Başka bir karşıya yükleme dosyalarının depolama için dosyanın tam yolunu veya dosyanın göreli yolu komut dosyası Düzenleyicisi'nde sağ tıklatma menüsünden aracılığıyla yoludur.

Aynı zamanda, izlediğiniz [karşıya yükleme durumu](#check-storage-tasks-status).


## <a name="download-file"></a>Dosya indirme 
Komutları girerek dosyalarını indirebilirsiniz **ADL: karşıdan dosya** veya **ADL: karşıdan dosya (Gelişmiş)**.

**Ancak ADL indirmek için dosyaları: karşıdan dosya (Gelişmiş)**
1. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **karşıdan dosya (Gelişmiş)**.
2. VS Code'da JSON dosyasını görüntüler. Dosya yolları girin ve aynı anda birden çok dosyalarını yükleyin. Yönergeler görüntülenir **çıkış** penceresi. Dosyayı yüklemeye devam etmek için (Ctrl + S) JSON dosyasını kaydedin.

    ![Yapılandırma dosyaları Visual Studio kodu indirme için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-files.png)

3.  Sonuçları: **çıkış** penceresi dosya karşıya yükleme durumunu görüntüler.

    ![Data Lake Visual Studio kodu indirmek için birden çok dosya sonuçları araçları](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-file-result.png)     

Aynı zamanda, izlediğiniz [karşıdan yükleme durumu](#check-storage-tasks-status).

**Dosyaları yine de ADL indirmek için: dosya indirme**

1. Komut Dosyası Düzenleyicisi'ni sağ tıklayın, **karşıdan dosya**ve ardından hedef klasöründen **Klasör Seç** iletişim.

2. Listede klasörü seçin veya tıklatın **yolu girin** veya **kökünden Gözat** (kullanan bir yolu girin örnek olarak). Select ->, **ADLA hesap**. Depolama klasör yolu girin veya Bul -> (örneğin: / çıkış /) -> indirmek için bir dosya seçin.

   ![Data Lake araçları Visual Studio Code için karşıdan yükleme durumu](./media/data-lake-analytics-data-lake-tools-for-vscode/download-file.png) 

   
   Depolama dosyaları indirme başka bir dosyanın tam yolunu veya dosyanın göreli yolu komut dosyası Düzenleyicisi'nde sağ tıklatma menüsünden aracılığıyla yoludur.

Aynı zamanda, izlediğiniz [karşıdan yükleme durumu](#check-storage-tasks-status).

## <a name="check-storage-tasks-status"></a>Depolama görevlerin durumunu denetle
İndirme ve yükleme tamamlandığında durum çubuğunun alta durumunu görüntüler.
1. Aşağıdaki durum çubuğu'nu tıklatın ve ardından indirme ve karşıya yükleme durumu göster **çıkış** paneli.

   ![Visual Studio kod denetleyin depolama durumu için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-status.png)

## <a name="vscode-explorer-integration-with-azure-data-lake"></a>Azure Data Lake ile VSCode Explorer Tümleştirme
1. Oturum açtıktan sonra sol panelinde listelenen tüm Azure hesaplarını göreceksiniz **DataLake Explorer**. Bir veritabanını genişletin, görüntüleyebileceğiniz **şemaları**, **tabloları**, **derlemeleri** düğümünün altında ve benzeri.

   ![DataLake Gezgini](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer.png)

2. Komut gerçekleştirebilirsiniz **kaydetmek derleme** sağ tıklayarak **derlemeleri** düğümü.

    ![DataLake Gezgini](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer-register-assembly.png)

3. Gidin **depolama hesabı**, karşıya yükleme veya klasöre veya dosyaya sağ tıklayarak dosyasını indirin. Ve ayrıca **Önizleme** bir dosya **karşıdan**, **göreli yol Kopyala**, **tam yol Kopyala** bağlam menüsü tarafından.

   ![DataLake Gezgini](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-download-preview-file.png)

## <a name="open-adl-storage-explorer-in-portal"></a>Portalı'nda Aç ADL Depolama Gezgini
1. Komut paletini açmak için Ctrl + Shift + P seçin.
2. Girin **açık Web Azure Storage Gezgini** veya göreli bir yol veya tam yolunu komut dosyası Düzenleyicisi'nde sağ tıklayın ve ardından **açık Web Azure Storage Gezgini**.
3. Bir Data Lake Analytics hesabı seçin.

Data Lake araçları Azure portalında Azure depolama yol açar. FIND yol ve dosya Web'den Önizleme.

## <a name="local-run-and-local-debug-for-windows-users"></a>Yerel çalıştırma ve yerel kullanıcılar için Windows hata ayıklama
U-SQL yerel çalıştırma yerel verilerinizi sınar ve kodunuzu Data Lake Analytics yayımlanmadan önce komut yerel olarak doğrular. Yerel hata ayıklama özelliği kodunuzu Data Lake Analytics gönderilmeden önce şu görevleri tamamlamanıza sağlar: 
- C# arka plan kod hata ayıklaması. 
- Kod üzerinden adım. 
- Komut dosyası yerel olarak doğrulayın.

Yerel çalıştırma ve yerel hata ayıklama hakkında yönergeler için bkz: [U-SQL yerel çalıştırma ve Visual Studio Code ile yerel hata ayıklama](data-lake-tools-for-vscode-local-run-and-debug.md).

## <a name="additional-features"></a>Ek Özellikler

VS Code'da için Data Lake araçları aşağıdaki özellikleri destekler:

-   IntelliSense otomatik tamamlama: önerileri anahtar sözcükler, yöntemleri ve değişkenler gibi öğelerin etrafında açılır pencere görünür. Farklı simgeler farklı tip nesneyi temsil eder:

    - Scala veri türü
    - karmaşık veri türü
    - Yerleşik atama
    - .NET koleksiyonu ve sınıfları
    - C# ifadeleri
    - Yerleşik C# UDF'ler, Udo'lar ve UDAAGs 
    - U-SQL işlevleri
    - U-SQL Pencereleme işlevi
 
    ![Visual Studio kod IntelliSense nesne türleri için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   IntelliSense otomatik tamamlama Data Lake Analytics meta: Data Lake araçları, Data Lake Analytics meta veri bilgileri yerel olarak indirir. IntelliSense özelliği, veritabanı, şema, tablo, görünüm, tablo değerli işlevi, yordamlar ve C# derlemeleri, Data Lake Analytics meta veriler dahil olmak üzere nesneleri, otomatik olarak doldurur.
 
    ![Visual Studio kod IntelliSense meta verileri için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   IntelliSense hata işaret: Data Lake araçları U-SQL ve C# için düzenleme hataları altını çizer. 
-   Sözdizimi vurgular: Data Lake araçları, değişkenleri, anahtar sözcükler, veri türü ve işlevleri gibi öğeleri ayırt etmek için farklı bir renk kullanır. 

    ![Visual Studio Code sözdizimi için Data Lake araçları vurgular](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Sonraki adımlar
- [U-SQL Python, R ve Azure Data Lake Analytics VSCode içinde CSharp ile geliştirme](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
- [U-SQL yerel çalıştırma ve Visual Studio Code ile yerel hata ayıklama](data-lake-tools-for-vscode-local-run-and-debug.md)
- [Öğretici: Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
- [Öğretici: Visual Studio için Data Lake araçları kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
- [Azure Data Lake Analytics işleri için U-SQL derlemeleri geliştirin](data-lake-analytics-u-sql-develop-assemblies.md)




