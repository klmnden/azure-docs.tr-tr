---
title: Visual Studio Code için Azure Data Lake Araçları'nı kullanma
description: Oluşturun, test etme ve U-SQL betikleri çalıştırmak için Visual Studio Code için Azure Data Lake Araçları'nı kullanmayı öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: Jejiang
ms.author: jejiang
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.topic: conceptual
ms.date: 02/09/2018
ms.openlocfilehash: 5042d89f1cb5e928444e4b3c9a23db7bb1d66585
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60509349"
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Visual Studio Code için Azure Data Lake Araçları'nı kullanma

Bu makalede, nasıl Azure Data Lake araçları Visual Studio Code için (VS Code) oluşturmak, test için kullanın ve U-SQL betiklerini çalıştırma öğrenin. Bilgiler aşağıdaki videoda da ele alınmıştır:

<a href="https://channel9.msdn.com/Series/AzureDataLake/Azure-Data-Lake-Tools-for-VSCode?term=ADL%20Tools%20for%20VSCode"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Önkoşullar

VS Code için Azure Data Lake araçları, Windows, Linux ve Macos'ta destekler. U-SQL yerel çalıştırma ve yerel hata ayıklama yalnızca Windows içinde çalışır.

- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx)

MacOS ve Linux için:
- [.NET Core SDK 2.0](https://www.microsoft.com/net/download/core)
- [Mono 5.2.x](https://www.mono-project.com/download/)

## <a name="install-azure-data-lake-tools"></a>Azure Data Lake Araçları'nı yükleme

Önkoşullar yüklendikten sonra VS Code için Azure Data Lake araçları yükleyebilirsiniz.

**Azure Data Lake Araçları'nı yüklemek için**

1. Visual Studio Code'u açın.
2. Seçin **uzantıları** sol bölmesinde. Girin **Azure Data Lake Araçları** arama kutusuna.
3. Seçin **yükleme** yanındaki **Azure Data Lake Araçları**. 

   ![Data Lake Araçları'nı yükleme seçimleri](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

   Birkaç saniye sonra **yükleme** düğmesi değişiklikleri **yeniden**.
4. Seçin **yeniden** etkinleştirmek için **Azure Data Lake Araçları** uzantısı.
5. Seçin **Reload Window** onaylamak için. Gördüğünüz **Azure Data Lake Araçları** içinde **uzantıları** bölmesi.

 
## <a name="activate-azure-data-lake-tools"></a>Azure Data Lake Araçları'nı etkinleştir
.Usql dosyasını oluşturun veya uzantıyı etkinleştirmek için mevcut bir .usql dosyasını açın. 


## <a name="work-with-u-sql"></a>U-SQL ile çalışma

U-SQL ile çalışmak için U-SQL dosyası veya klasörü açmak.

**Örnek komut dosyasını açmak için**

(Ctrl + Shift + P) komut paletini açın ve girin **ADL: Açık örnek betik**. Bu örnek başka bir örneğini açar. Düzen, yapılandırma ve bu örneği üzerinde bir betiği Gönder.

**U-SQL projeniz için bir klasörü açmak için**

1. Visual Studio Code'dan seçin **dosya** menüsüne ve ardından **Klasör Aç**.
2. Bir klasör belirtin ve ardından **Klasör Seç**.
3. Seçin **dosya** menüsüne ve ardından **yeni**. Adsız 1 dosya projeye eklenir.
4. Adsız 1 dosyasında aşağıdaki kodu girin:

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO "/Output/departments.csv"
        USING Outputters.Csv();

    Betik/Output klasöründe bulunan bazı verilerle departments.csv dosyası oluşturur.

5. Dosyayı Farklı Kaydet **myUSQL.usql** açılan klasördeki.

**U-SQL betiği derlemek için**

1. Komut paletini açmak için Ctrl + Shift + P'ı seçin. 
2. Girin **ADL: Derleme betiği**. Derleme sonuçları şurada görüntülenir **çıkış** penceresi. Ayrıca bir komut dosyasına sağ tıklayın ve ardından **ADL: Derleme betiği** bir U-SQL işi derlemek için. Derleme sonucu görünür **çıkış** bölmesi.
 
**U-SQL betiği göndermek için**

1. Komut paletini açmak için Ctrl + Shift + P'ı seçin. 
2. Girin **ADL: İşi Gönder**. Ayrıca bir komut dosyasına sağ tıklayın ve ardından **ADL: İşi Gönder**. 

Bir U-SQL işi gönderdikten sonra gönderme günlükleri görünür **çıkış** VS code'da penceresi. İş görünümü sağ bölmede görünür. İş URL'si çok gönderim başarılı olması durumunda görüntülenir. İş URL'si gerçek zamanlı iş durumunu izlemek için bir web tarayıcısı açabilirsiniz. 

İş Görünüm'ün üzerinde **özeti** sekmesi, iş ayrıntılarını görebilirsiniz. Ana işlev içeren bir betiği yeniden gönderin, yinelenen bir betik ve portalda açın. İş Görünüm'ün üzerinde **veri** sekmesi, giriş dosyaları, Çıkış dosyalarını ve kaynak dosyalarını başvurabilir. Dosyaları yerel bilgisayara yüklenebilir.

![İş görünümü Özet sekmesi](./media/data-lake-analytics-data-lake-tools-for-vscode/job-view-summary.png)

![İş görünümünde veri sekmesi](./media/data-lake-analytics-data-lake-tools-for-vscode/job-view-data.png)

**Varsayılan bağlamı ayarlamak için**

Parametre dosyaları için tek tek ayarlamadıysanız tüm komut dosyaları için bu ayarı uygulamak için varsayılan bağlamı ayarlayabilirsiniz.

1. Komut paletini açmak için Ctrl + Shift + P'ı seçin. 
2. Girin **ADL: Ayarlanmış varsayılan bağlamı**. Kod Düzenleyicisi'ni sağ tıklatın ve seçin **ADL: Ayarlanmış varsayılan bağlamı**.
3. Hesabı, veritabanı ve istediğiniz şema seçin. Ayar xxx_settings.json yapılandırma dosyasına kaydedilir.

   ![Hesabı, veritabanı ve şema varsayılan bağlamı olarak ayarlanmış](./media/data-lake-analytics-data-lake-tools-for-vscode/default-context-sequence.png)

**Betik parametrelerini ayarlamak için**

1. Komut paletini açmak için Ctrl + Shift + P'ı seçin. 
2. Girin **ADL: Betik parametreleri ayarlayın**.
3. Aşağıdaki özelliklerle xxx_settings.json dosya açılır:

   - **Hesap**: Bir Azure Data Lake Analytics hesabı derlemek ve U-SQL işleri çalıştırmak için gerekli olan, bir Azure aboneliği altında. Derleme ve U-SQL işleri çalıştırma önce bilgisayar hesabını yapılandırmanız.
   - **Veritabanı**: Bir veritabanı hesabınızın altında. Varsayılan değer **ana**.
   - **Şema**: Şema veritabanınızı altında. Varsayılan değer **dbo**.
   - **optionalSettings**:
        - **Öncelik**: Öncelik 1'den en yüksek öncelikli olarak 1 ile 1000 aralığındadır. Varsayılan değer **1000**.
        - **degreeOfParallelism**: Paralellik 1 ila 150 için aralığındadır. Azure Data Lake Analytics hesabınızda izin verilen en büyük paralellik varsayılan değerdir. 

   ![JSON dosyasının içeriği](./media/data-lake-analytics-data-lake-tools-for-vscode/default-context-setting.png)
      
> [!NOTE] 
> Yapılandırmayı kaydettikten sonra ayarladığınız bir varsayılan bağlam yoksa, hesabını, veritabanını ve şema bilgilerini karşılık gelen .usql dosyanın sol alt köşesindeki durum çubuğunda görünür.

**Git ayarlanacak yoksay**

1. Komut paletini açmak için Ctrl + Shift + P'ı seçin. 
2. Girin **ADL: Kümesi Git yoksayma**.

   - Yoksa bir **.gitIgnore** adlı bir dosya VS Code kullanarak çalışma klasörünüzde dosyasında **.gitIgnore** klasörünüzde oluşturulur. Dört öğe (**usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **.cache**, **obj**) dosyasında varsayılan olarak eklenir. Gerekirse, diğer güncelleştirmeleri yapabilirsiniz.
   - Zaten bir **.gitIgnore** VS Code çalışma klasörünüzde, araç dosyayı dört öğe ekler (**usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **.cache**, **obj**) içinde **.gitIgnore** dört öğe dosyasında bulunmayan dosyası.

   ![.GitIgnore dosyası öğeleri](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-gitignore.png)


## <a name="work-with-code-behind-files-c-sharp-python-and-r"></a>Arka plan kod dosyaları ile çalışma: C Sharp, Python ve R

Azure Data Lake araçları, birden çok özel kodları destekler. Yönergeler için [geliştirme ile U-SQL Python, R ve VS code'da Azure Data Lake Analytics için C Sharp](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md).

## <a name="work-with-assemblies"></a>Derlemeleri ile çalışma

Derlemeleri geliştirme hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics işleri için U-SQL geliştirme derlemeleri](data-lake-analytics-u-sql-develop-assemblies.md).

Data Lake araçları Data Lake Analytics Kataloğu'nda özel kod derlemeleri kaydetmek için kullanabilirsiniz.

**Bir derlemeyi kaydetmek için**

Aracılığıyla derlemeyi kayıt edebilirsiniz **ADL: Bütünleştirilmiş kodu Kaydet** veya **ADL: Derleme (Gelişmiş) kaydetme** komutu.

**ADL ile kaydetmek için: Kayıt derleme komutu**
1.  Komut paletini açmak için Ctrl + Shift + P'ı seçin.
2.  Girin **ADL: Bütünleştirilmiş kodu Kaydet**. 
3.  Yerel derleme yolunu belirtin. 
4.  Bir Data Lake Analytics hesabı seçin.
5.  Bir veritabanı seçin.

Portal, bir tarayıcıda açılır ve derleme kayıt işlemini gösterir.  

Tetiklemek için daha kullanışlı bir yol **ADL: Bütünleştirilmiş kodu Kaydet** komuttur .dll dosyasını dosya Gezgini'ndeki sağ tıklayın. 

**ADL ile kaydetmek için: Derleme (Gelişmiş) komut kaydetme**
1.  Komut paletini açmak için Ctrl + Shift + P'ı seçin.
2.  Girin **ADL: (Gelişmiş) bütünleştirilmiş kodunu kaydetme**. 
3.  Yerel derleme yolunu belirtin. 
4.  JSON dosyası görüntülenir. Gözden geçirin ve gerekirse derleme bağımlılıklarını ve kaynak parametrelerini düzenleyin. Yönergeler görüntülenir **çıkış** penceresi. Derleme kayıt aşamasına ilerlemek için JSON dosyasını (Ctrl + S) kaydedin.

    ![Derleme bağımlılıkları ve kaynak parametreleri içeren JSON dosyası](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
    
>[!NOTE]
>- Azure Data Lake araçları autodetects DLL derleme bağımlılıkları sahip olup olmadığını belirler. Bunlar saptadıktan sonra bağımlılık JSON dosyasında görüntülenir. 
>- Derleme kaydı bir parçası olarak, DLL kaynaklarını (örneğin, .txt, .png ve .csv) karşıya yükleyebilir. 

Başka bir tetikleme yolu **ADL: Derleme (Gelişmiş) kaydetme** komuttur .dll dosyasını dosya Gezgini'ndeki sağ tıklayın. 

Aşağıdaki U-SQL kodu bir bütünleştirilmiş kod çağırmak nasıl gösterir. Aşağıdaki örnekte, derleme adı: *test*.


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


## <a name="use-u-sql-local-run-and-local-debug-for-windows-users"></a>U-SQL yerel çalıştırma ve yerel hata ayıklama Windows kullanıcıları için kullanın.
U-SQL yerel çalıştırma, verilerinizi yerel test eder ve kodunuzu Data Lake Analytics'e yayımlanmadan önce betiği yerel olarak doğrular. Data Lake Analytics için kodunuzu gönderilmeden önce aşağıdaki görevleri tamamlamak için yerel hata ayıklama özelliğini kullanabilirsiniz: 
- C# arka plan kod hatalarını ayıklayın. 
- Kodunuz içinde adım adım. 
- Kodunuzu yerel olarak doğrulayın.

Yerel çalıştırma ve yerel hata ayıklama özellik yalnızca Windows ortamlarda çalışır ve macOS ve Linux tabanlı işletim sistemlerinde desteklenmez.

Yerel çalıştırma ve yerel hata ayıklama hakkında yönergeler için bkz: [U-SQL yerel çalıştırma ve Visual Studio Code ile yerel hata ayıklama](data-lake-tools-for-vscode-local-run-and-debug.md).


## <a name="connect-to-azure"></a>Azure'a Bağlanma

Derleme ve Data Lake Analytics'te U-SQL betiklerini çalıştırma önce Azure hesabınıza bağlanmanız gerekir.

<b id="sign-in-by-command">Bir komut kullanarak Azure'a bağlanmak için</b>

1.  Komut paletini açmak için Ctrl + Shift + P'ı seçin. 
2.  Girin **ADL: Oturum açma**. Oturum açma bilgilerini alt sağ tarafta görüntülenir.

    ![Oturum açma komutunu girerek](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)

    ![Oturum açma ve kimlik doğrulaması hakkında bildirim](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)

3.  Seçin **açın & Kopyala** açmak için [oturum açma Web sayfasına](https://aka.ms/devicelogin). Kod kutuya yapıştırın ve ardından **devam**.

    ![Oturum açma Web sayfası](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png)  
     
4.  Web sayfasında başlatabilirler oturum açmak için yönergeleri izleyin. Bağlandığınızda, VS Code penceresinin sol alt köşesindeki durum çubuğunda Azure hesap adınız görüntülenir. 

> [!NOTE] 
>- Oturumunuzu yoksa, data Lake araçları, otomatik olarak sonraki süreyi imzalar.
>- Hesabınızı etkin iki faktör varsa, PIN kullanma yerine telefon kimlik doğrulaması kullanmanızı öneririz.


Oturumu kapatmak için komutu girin **ADL: Oturum kapatma**.

**Gezgini'nden Azure'a bağlanmak için**

Genişletin **AZURE DATALAKE**seçin **Azure'da oturum aç**ve ardından adım 3 ve 4. adımını izleyin [bir komut kullanarak Azure'a bağlanmak için](#sign-in-by-command).

!["Azure'da oturum açın" seçimi Gezgini'nde](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-sign-in-from-explorer.png )  

Gezgini'nden imzalanamıyor. Oturumu kapatmak için bkz: [bir komut kullanarak Azure'a bağlanmak için](#sign-in-by-command).


## <a name="create-an-extraction-script"></a>Bir ayıklama betiği oluşturma 
.Csv .tsv ve .txt dosyaları için bir çıkarma betik komutunu kullanarak oluşturabileceğiniz **ADL: EXTRACT betiği Oluştur** veya Azure Data Lake Explorer.

**Bir komut kullanarak ayıklama betiği oluşturmak için**

1. Komut paletini açın ve girmek için Ctrl + Shift + P seçin **ADL: EXTRACT betiği Oluştur**.
2. Bir Azure depolama dosyasının tam yolunu belirtin ve Enter tuşunu seçin.
3. Bir hesabı seçin.
4. Bir .txt dosyasına için dosyasını ayıklamak için bir sınırlayıcı seçin. 

![Bir ayıklama betik oluşturma işlemi](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-process.png)

Ayıklama betiği, girişlere göre oluşturulur. Sütunları algılayamaz bir betik için iki seçenekten birini seçin. Aksi durumda, yalnızca bir komut dosyası oluşturulur.

![Sonuç ayıklama betiği oluşturma](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-result.png)

**Gezgini'nden bir ayıklama betiği oluşturmak için**

Ayıklama betiği oluşturmak için başka bir .csv, .tsv ya da Azure Data Lake Store veya Azure Blob Depolama .txt dosyasında (kısayol) sağ tıklama menüsü aracılığıyla yoludur.

![Kısayol menüsünden "EXTRACT betiği oluştur" komutu](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-from-context-menu.png)

## <a name="integrate-with-azure-data-lake-analytics-through-a-command"></a>Bir komutu aracılığıyla Azure Data Lake Analytics ile tümleştirme

Azure Data Lake Analytics hesapları listeleme, erişim meta veri ve analiz işleri görüntüle kaynaklarına erişebilirsiniz. 

**Azure aboneliğiniz kapsamındaki Azure Data Lake Analytics hesaplarını listelemek için**

1. Komut paletini açmak için Ctrl + Shift + P'ı seçin.
2. Girin **ADL: Hesapları Listele**. Hesaplar görüntülenir **çıkış** bölmesi.

**Azure Data Lake Analytics meta verilerine erişim**

1.  Ctrl + Shift + P seçin ve enter **ADL: Tablolar listesinde**.
2.  Data Lake Analytics hesapları birini seçin.
3.  Data Lake Analytics veritabanlarından birini seçin.
4.  Şemaları birini seçin. Tabloların listesini görebilirsiniz.

**Azure Data Lake Analytics işlerini görüntülemek için**
1.  (Ctrl + Shift + P) komut paletini açın ve seçin **ADL: İşleri göster**. 
2.  Bir Data Lake Analytics veya yerel hesap seçin. 
3.  Hesap için görüntülenecek iş listesi bekleyin.
4.  Bir işi iş listesinden seçin. Data Lake araçları, sağ bölmede iş görünümü açılır ve VS Code çıktısında bazı bilgiler görüntüler.

    ![İş listesi](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="integrate-with-azure-data-lake-store-through-a-command"></a>Bir komutu aracılığıyla Azure Data Lake Store ile tümleştirme

Azure Data Lake Store ile ilgili komutları kullanabilirsiniz:
 - [Azure Data Lake Store kaynaklarına göz atın](#list-the-storage-path) 
 - [Azure Data Lake Store Dosya Önizleme](#preview-the-storage-file) 
 - Azure Data Lake Store VS code'da doğrudan dosyasını yükleyin
 - Azure Data Lake Store VS code'da dosya doğrudan indirin

### <a name="list-the-storage-path"></a>Depolama yolu listesi 

**Komut paletini üzerinden depolama yolu listelemek için**

1. Kod Düzenleyicisi'ni sağ tıklayıp **ADL: Liste yolu**.
2. Klasörü listeden seçin ya da seçin **bir yol girin** veya **kök yolu Gözat**. (Kullanıyoruz **bir yol girin** örnek olarak.) 
3. Data Lake Analytics hesabınızı seçin.
4. Göz atın veya depolama klasör yolu (örneğin, / çıkış /) girin.  

Komut paletini, girişlere göre yol bilgilerini listeler.

![Depolama yolu sonuçları](./media/data-lake-analytics-data-lake-tools-for-vscode/list-storage-path.png)

Kısayol menüsü üzerinden göreli yol listelemek için daha kullanışlı bir yoldur.

**Kısayol menüsü aracılığıyla depolama yolu listelemek için**

Yol dizesi sağ tıklayıp **listesi yolu**.

!["Yol kısayol menüsünde list"](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)


### <a name="preview-the-storage-file"></a>Depolama dosya önizleme

1. Kod Düzenleyicisi'ni sağ tıklayıp **ADL: Dosya Önizleme**.
2. Data Lake Analytics hesabınızı seçin. 
3. Bir Azure depolama dosya yolunu (örneğin, /output/SearchLog.txt) girin. 

Dosya, VS Code'da açılır.

![Adımlar ve depolama dosya önizleme için sonuç](./media/data-lake-analytics-data-lake-tools-for-vscode/preview-storage-file.png)

Dosyanın önizlemesini görmek için başka bir dosyanın tam yolunu veya dosyanın göreli yol Kod Düzenleyicisi'nde kısayol menüsünde aracılığıyla yoludur. 

### <a name="upload-a-file-or-folder"></a>Bir dosya veya klasörü karşıya yükle

1. Kod Düzenleyicisi'ni sağ tıklayıp **dosyasını karşıya yükle** veya **klasörü karşıya yükle**.
2. Seçtiyseniz, bir veya birden çok dosya seçin **dosyasını karşıya yükle**, veya seçtiyseniz, tüm klasörü seçin **klasörü karşıya yükle**. Ardından **karşıya**. 
3. Depolama klasörü listeden seçin ya da seçin **bir yol girin** veya **kök yolu Gözat**. (Kullanıyoruz **bir yol girin** örnek olarak.) 
4. Data Lake Analytics hesabınızı seçin. 
5. Göz atın veya depolama klasör yolu (örneğin, / çıkış /) girin. 
6. Seçin **geçerli klasör seç** karşıya yükleme Hedefinizi belirtmek için.

![Adımlar ve sonucu bir dosya veya klasörü karşıya yükleme](./media/data-lake-analytics-data-lake-tools-for-vscode/upload-file.png)    

Dosya depolama alanına yüklemek için başka bir dosyanın tam yolunu veya dosyanın göreli yol Kod Düzenleyicisi'nde kısayol menüsünde aracılığıyla yoludur.

Yapabilecekleriniz [yükleme durumunu izlemek](#check-storage-tasks-status).


### <a name="download-a-file"></a>Dosya indirme 
Komutunu kullanarak bir dosyayı indirebilirsiniz **ADL: Dosya indirme** veya **ADL: (Gelişmiş) dosyasını indirin**.

**ADL aracılığıyla bir dosyayı indirmek için: Komut dosyası (Gelişmiş) indirin**
1. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **karşıdan dosya (Gelişmiş)**.
2. VS Code bir JSON dosyası gösterir. Dosya yolu girin ve aynı anda birden çok dosya indirin. Yönergeler görüntülenir **çıkış** penceresi. Dosya veya dosyalar yüklemeye devam etmek için (Ctrl + S) JSON dosyası olarak kaydedin.

    ![Dosya indirme için yollar ile JSON dosyası](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-files.png)

**Çıkış** penceresinde dosya indirme durumu gösterilir.

![İndirme durumu ile Çıktı penceresi](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-file-result.png)     

Yapabilecekleriniz [yükleme durumunu izleme](#check-storage-tasks-status).

**ADL aracılığıyla bir dosyayı indirmek için: Komut dosyası indir**

1. Kod Düzenleyicisi'ni sağ tıklayın, **karşıdan dosya**ve hedef klasördeki ardından **klasörü seçin** iletişim kutusu.
2. Klasörü listeden seçin ya da seçin **bir yol girin** veya **kök yolu Gözat**. (Kullanıyoruz **bir yol girin** örnek olarak.) 
3. Data Lake Analytics hesabınızı seçin. 
4. Göz atın veya depolama klasör yolu (örneğin, / çıkış /) girin ve ardından yüklenecek bir dosya seçin.

![Adımlar ve dosya indirme için sonuç](./media/data-lake-analytics-data-lake-tools-for-vscode/download-file.png) 

Depolama dosyalarını indirmek için başka bir dosyanın tam yolunu veya dosyanın göreli yol Kod Düzenleyicisi'nde kısayol menüsünde aracılığıyla yoludur.

Yapabilecekleriniz [yükleme durumunu izleme](#check-storage-tasks-status).

### <a name="check-storage-tasks-status"></a>Depolama görevlerin durumunu denetle
Karşıya yükleme ve indirme durumu, durum çubuğunda görüntülenir. Durum çubuğunu seçin ve ardından hakkında durum görüntülenir **çıkış** sekmesi.

![Durum çubuğu ve çıkış ayrıntıları](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-status.png)


## <a name="integrate-with-azure-data-lake-analytics-from-the-explorer"></a>Azure Data Lake Analytics ile Gezgini'nde tümleştirin

Oturum açtıktan sonra Azure hesabınız için tüm abonelikleri sol bölmede altında listelenen **AZURE DATALAKE**. 

![Data Lake Gezgini](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer.png)

### <a name="data-lake-analytics-metadata-navigation"></a>Data Lake Analytics meta veri gezinme

Azure aboneliğinizi genişletin. Altında **U-SQL veritabanlarını** düğümü, U-SQL veritabanınızı göz atabilir ve görüntüleme klasörleri ister **şemaları**, **kimlik bilgilerini**, **derlemeleri**, **Tabloları**, ve **dizin**.

### <a name="data-lake-analytics-metadata-entity-management"></a>Data Lake Analytics meta verileri varlık yönetimi

Genişletin **U-SQL veritabanları**. İlgili düğüm sağ tıklayıp ardından bir veritabanı, şemayı, tablo, tablo türü, dizini veya istatistiği oluşturabilirsiniz **betik oluşturma** kısayol menüsünde. Açık kod sayfasında, ihtiyaçlarınıza göre betiği düzenleyin. Ardından, sağ ve seçerek işi göndermek **ADL: İşi Gönder**. 

Öğe oluşturma işlemini tamamladıktan sonra düğümüne sağ tıklayın ve ardından **Yenile** öğesini göstermek için. Öğeyi sağ tıklatıp ardından seçerek silebilirsiniz **Sil**.

![Data Lake Explorer kısayol menüsündeki "Betik oluşturma" komutu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create.png)

![Yeni öğe için kod sayfası](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create-snippet.png)

### <a name="data-lake-analytics-assembly-registration"></a>Data Lake Analytics bütünleştirilmiş kod kaydı

Sağ tıklayarak bir derlemeye karşılık gelen veritabanında kaydedebilirsiniz **derlemeleri** düğümünü seçip ardından **kaydetme derleme**.

![Derlemeleri düğümü için kısayol menüsündeki "bütünleştirilmiş kodunu kaydetme" komutu](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer-register-assembly.png)

## <a name="integrate-with-azure-data-lake-store-from-the-explorer"></a>Azure Data Lake Store ile Gezgini'nde tümleştirin

Gözat **Data Lake Store**:

- Klasörü düğümüne sağ tıklayın ve ardından **Yenile**, **Sil**, **karşıya**, **klasörü karşıya yükle**, **kopyalama Göreli yol**, ve **tam yol Kopyala** komutları kısayol menüsünde.

   ![Bir Data Lake Gezgini klasör düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-folder-menu.png)

- Dosya düğümüne sağ tıklayın ve ardından **Önizleme**, **indirme**, **Sil**, **EXTRACT betiği Oluştur** (yalnızca CSV için kullanılabilir TSV ve TXT dosyaları), **göreli yolu Kopyala**, ve **tam yol Kopyala** komutları kısayol menüsünde.

   ![Data Lake Gezgini'ndeki bir dosya düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-extract.png)

## <a name="integrate-with-azure-blob-storage-from-the-explorer"></a>Azure Blob storage Explorer ile tümleştirin

BLOB depolama alanına göz atın:

- Blob kapsayıcı düğümüne sağ tıklayın ve ardından **Yenile**, **Blob kapsayıcısını Sil**, ve **Blob karşıya** komutları kısayol menüsünde.

   ![Blob Depolama altında bir blob kapsayıcı düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-blob-container-node.png)

- Klasörü düğümüne sağ tıklayın ve ardından **Yenile** ve **Blob karşıya** komutları kısayol menüsünde.

   ![Blob Depolama altında bir klasör düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-folder-node.png)

- Dosya düğümüne sağ tıklayın ve ardından **Önizleme/Düzenle**, **indirme**, **Sil**, **EXTRACT betiği Oluştur** (kullanılabilir yalnızca CSV, TSV ve TXT dosyaları için) **göreli yolu Kopyala**, ve **tam yol Kopyala** komutları kısayol menüsünde.

    ![Blob Depolama altında bir dosya düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-from-context-menu-2.png)

## <a name="open-the-data-lake-explorer-in-the-portal"></a>Data Lake explorer'ı portalda açın
1. Komut paletini açmak için Ctrl + Shift + P'ı seçin.
2. Girin **Web Azure Depolama Gezgini'ni Aç** veya göreli bir yol veya tam yolunu Kod Düzenleyicisi'nde sağ tıklayın ve ardından **Web Azure Depolama Gezgini'ni Aç**.
3. Bir Data Lake Analytics hesabı seçin.

Data Lake araçları, Azure portalında Azure depolama yolu açılır. Yolun bulabilir ve Web dosyası önizlenemedi.

## <a name="additional-features"></a>Ek özellikler

VS Code için Data Lake araçları, aşağıdaki özellikleri destekler:

-   **IntelliSense otomatik tamamlama**: Anahtar sözcükler, yöntemler ve değişkenler gibi öğeleri geçici açılır pencereleri öneriler görüntülenir. Farklı simgeler farklı türde nesneyi temsil eder:

    - Scala veri türü
    - Karmaşık veri türü
    - Yerleşik Udt'ler
    - .NET koleksiyonu ve sınıfları
    - C# ifadeleri
    - Yerleşik C# UDF'ler, Udo'lar vb. UDAAGs 
    - U-SQL işlevleri
    - U-SQL Pencereleme işlevleri
 
    ![IntelliSense nesne türleri](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   **Data Lake Analytics meta veri çubuğunda IntelliSense otomatik tamamlama**: Data Lake araçları Data Lake Analytics meta veri bilgilerini yerel olarak indirir. IntelliSense özelliği, Data Lake Analytics meta verilerden nesneleri otomatik olarak doldurur. Bu nesneler, veritabanı, şema, tablo, görünüm, tablo değerli işlev, yordamları ve C# derlemeleri içerir.
 
    ![IntelliSense meta verileri](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   **IntelliSense hata işaret**: Data Lake araçları, U-SQL için düzenleme hataları çizmeyeceğini ve C#. 
-   **Söz dizimi vurgular**: Değişkenleri, anahtar sözcükler, veri türleri ve işlevleri gibi öğeleri ayırt etmek için data Lake araçları renk kullanır. 

    ![Çeşitli renklerle söz dizimi](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

> [!NOTE]
> Azure Data Lake Araçları'nı Visual Studio sürümü 2.3.3000.4 yükseltmenizi öneririz veya üzeri. Önceki sürümler artık indirilemiyor ve kullanım dışı.  
   
## <a name="next-steps"></a>Sonraki adımlar
- [U-SQL Python, R ve C ile geliştirme için Azure Data Lake Analytics'i VS code'da Sharp](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
- [U-SQL yerel çalıştırma ve Visual Studio Code ile yerel hata ayıklama](data-lake-tools-for-vscode-local-run-and-debug.md)
- [Öğretici: Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
- [Öğretici: Visual Studio için Data Lake araçları kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
