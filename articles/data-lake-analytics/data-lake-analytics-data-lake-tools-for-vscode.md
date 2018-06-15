---
title: Azure Data Lake araçları Visual Studio kodunu kullanın
description: Visual Studio Code için Azure Data Lake araçları oluşturun, test ve U-SQL betikleri çalıştırmak için nasıl kullanılacağını öğrenin.
services: data-lake-analytics
author: Jejiang
ms.author: jejiang
manager: kfile
editor: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 02/09/2018
ms.openlocfilehash: 79cd1a04c99891e5146ad20cfd36b8bd4fe4d893
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261493"
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Azure Data Lake araçları Visual Studio kodunu kullanın

Bu makalede, nasıl Azure Data Lake araçları Visual Studio Code (VS Code) için ve için kullanabileceğiniz oluşturmak, sınamak, U-SQL betikleri çalıştırmak öğrenin. Bilgiler aşağıdaki videoda da ele alınmıştır:

<a href="https://channel9.msdn.com/Series/AzureDataLake/Azure-Data-Lake-Tools-for-VSCode?term=ADL%20Tools%20for%20VSCode"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Önkoşullar

VS Code için Azure Data Lake araçları, Windows, Linux ve MacOS destekler.  

- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx)

MacOS ve Linux için:
- [.NET core SDK 2.0](https://www.microsoft.com/net/download/core)
- [Mono 5.2.x](http://www.mono-project.com/download/)

## <a name="install-azure-data-lake-tools"></a>Azure Data Lake Araçları'nı yükleme

Önkoşulları yüklendikten sonra Azure Data Lake araçları için VS Code yükleyebilirsiniz.

**Azure Data Lake araçları yüklemek için**

1. Visual Studio Code'u açın.
2. Seçin **uzantıları** sol bölmede. Girin **Azure Data Lake Araçları** arama kutusuna.
3. Seçin **yükleme** yanına **Azure Data Lake Araçları**. 

   ![Data Lake araçları yükleme seçimleri](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

   Birkaç saniye sonra **yükleme** düğmesi değişiklikler **yeniden**.
4. Seçin **yeniden** etkinleştirmek için **Azure Data Lake Araçları** uzantısı.
5. Seçin **yeniden yükleme penceresi** onaylamak için. Gördüğünüz **Azure Data Lake Araçları** içinde **uzantıları** bölmesi.

 
## <a name="activate-azure-data-lake-tools"></a>Azure Data Lake araçları etkinleştirme
Bir .usql dosyası oluşturun veya uzantıyı etkinleştirmek için mevcut bir .usql dosyasını açın. 


## <a name="work-with-u-sql"></a>U-SQL ile çalışma

U-SQL ile çalışmak için bir U-SQL dosya veya klasör açın.

**Örnek komut dosyasını açmak için**

Komut palet (Ctrl + Shift + P) açın ve girin **ADL: açık örnek komut dosyası**. Bu örnek başka bir örneği açılır. Ayrıca düzenleyebilir, yapılandırmak ve bu örnek bir komut dosyasına göndermek.

**U-SQL projeniz için bir klasör açmak için**

1. Visual Studio koddan seçin **dosya** menüsüne ve ardından **Klasör Aç**.
2. Bir klasör belirtin ve ardından **Klasör Seç**.
3. Seçin **dosya** menüsüne ve ardından **yeni**. Adsız-1 dosyası projeye eklenir.
4. Adsız-1 dosyasında aşağıdaki kodu girin:

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

5. Dosyayı Farklı Kaydet **myUSQL.usql** açılan klasörde.

**U-SQL betiği derlemek için**

1. Komut paletini açmak için Ctrl + Shift + P seçin. 
2. Girin **ADL: derleme betik**. Derleme sonuçları görünür **çıkış** penceresi. Ayrıca bir komut dosyasını sağ tıklatın ve ardından **ADL: derleme betik** bir U-SQL işi derlemek için. Derleme sonucu görünür **çıkış** bölmesi.
 
**U-SQL betiği göndermek için**

1. Komut paletini açmak için Ctrl + Shift + P seçin. 
2. Girin **ADL: işi göndermek**. Ayrıca bir komut dosyasını sağ tıklatın ve ardından **ADL: işi Gönder**. 

U-SQL işi gönderdikten sonra gönderme günlükleri görünür **çıkış** VS Code penceresinde. İş görünümünde sağ bölmede görüntülenir. Gönderim başarılı olursa, iş URL çok görünür. Proje URL'si gerçek zamanlı iş durumunu izlemek için bir web tarayıcısı açabilirsiniz. 

İş görünümün üzerinde **Özet** sekmesi, iş ayrıntılarını görebilirsiniz. Ana işlevler bir komut dosyası yeniden gönderin, bir komut dosyası yinelenen ve portalda açın. İş görünümün üzerinde **veri** sekmesi, giriş dosyaları, çıktı dosyalarını ve kaynak dosyaları başvurabilir. Dosyaları yerel bilgisayara yüklenebilir.

![İş görünümünde Özet sekmesi](./media/data-lake-analytics-data-lake-tools-for-vscode/job-view-summary.png)

![İş görünümünde veri sekmesi](./media/data-lake-analytics-data-lake-tools-for-vscode/job-view-data.png)

**Varsayılan bağlam ayarlamak için**

Varsayılan bağlam parametreleri dosyaları için tek tek ayarlamadıysanız bu ayarı tüm komut dosyalarının uygulamak için ayarlayabilirsiniz.

1. Komut paletini açmak için Ctrl + Shift + P seçin. 
2. Girin **ADL: küme varsayılan bağlamının**. Veya komut dosyası Düzenleyicisi'ni sağ tıklatın ve seçin **ADL: Set Default Context**.
3. Hesap, veritabanı ve istediğiniz şema seçin. Ayar xxx_settings.json yapılandırma dosyasına kaydedilir.

   ![Hesap, veritabanını ve şemayı varsayılan bağlamı olarak ayarlanmış](./media/data-lake-analytics-data-lake-tools-for-vscode/default-context-sequence.png)

**Betik parametrelerini ayarlamak için**

1. Komut paletini açmak için Ctrl + Shift + P seçin. 
2. Girin **ADL: Set betik parametreler**.
3. Xxx_settings.json dosyası aşağıdaki özelliklere sahip açılır:

   - **Hesap**: Azure aboneliğinizdeki derlemek ve U-SQL işleri çalıştırmak için gerekli olan bir Azure Data Lake Analytics hesabı. Derleme ve U-SQL işleri çalıştırma önce bilgisayar hesabını yapılandırmanız.
   - **Veritabanı**: hesabınız altındaki bir veritabanı. Varsayılan değer **ana**.
   - **şema**: bir şema veritabanınızı altında. Varsayılan değer **dbo**.
   - **optionalSettings**:
        - **öncelik**: öncelik 1'den en yüksek öncelikli olarak 1 ile 1000 aralığındadır. Varsayılan değer **1000**.
        - **degreeOfParallelism**: paralellik 1 ila 150 için aralığı. Azure Data Lake Analytics hesabınızı izin verilen maksimum paralellik varsayılan değerdir. 

   ![JSON dosyasının içeriği](./media/data-lake-analytics-data-lake-tools-for-vscode/default-context-setting.png)
      
> [!NOTE] 
> Yapılandırmayı kaydettikten sonra ayarlanmış varsayılan bağlam yoksa hesap, veritabanı ve şema bilgilerini karşılık gelen .usql dosyası sol alt köşesindeki durum çubuğunda görünür.

**Git ayarlamak için Yoksay**

1. Komut paletini açmak için Ctrl + Shift + P seçin. 
2. Girin **ADL: kümesi Git Yoksay**.

   - Yoksa bir **.gitIgnore** , VS Code çalışma klasörü adlı bir dosya dosyasında **.gitIgnore** klasörünüzde oluşturulur. Dört öğe (**usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **.cache**, **obj**) dosyasında varsayılan olarak eklenir. Gerekirse, daha fazla güncelleştirme yapabilirsiniz.
   - Zaten varsa bir **.gitIgnore** VS Code çalışma klasöründe, aracı dosyasını dört öğe ekler (**usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **.cache**, **obj**) içinde **.gitIgnore** dört öğe dosyasında dahil edilmemiş dosyası.

   ![.GitIgnore dosyası öğeleri](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-gitignore.png)


## <a name="work-with-code-behind-files-c-sharp-python-and-r"></a>Arka plan kodu dosyalarıyla çalışma: C Sharp, Python ve R

Azure Data Lake araçları, birden çok özel kodlarını destekler. Yönergeler için bkz: [geliştirmek U-SQL ile Python, R ve C Sharp VS code'da Azure Data Lake Analytics için](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md).

## <a name="work-with-assemblies"></a>Derlemeler ile çalışma

Derlemeler geliştirme hakkında daha fazla bilgi için bkz: [Azure Data Lake Analytics işleri için U-SQL geliştirmek derlemeleri](data-lake-analytics-u-sql-develop-assemblies.md).

Data Lake araçları, Data Lake Analytics Kataloğu'nda özel kod derlemeleri kaydetmek için kullanabilirsiniz.

**Bir derlemeyi kaydetmek için**

Derleme aracılığıyla kaydedebilirsiniz **ADL: kaydetmek derleme** veya **ADL: kaydetmek derleme (Gelişmiş)** komutu.

**ADL kaydetmek için: kayıt derleme komutu**
1.  Komut paletini açmak için Ctrl + Shift + P seçin.
2.  Girin **ADL: kaydetmek derleme**. 
3.  Yerel derleme yolu belirtin. 
4.  Bir Data Lake Analytics hesabı seçin.
5.  Bir veritabanı seçin.

Portal bir tarayıcıda açıldığından ve derleme kayıt işlemi görüntüler.  

Tetiklemek için daha kolay bir yol **ADL: kaydetmek derleme** komuttur dosya Gezgini'nde .dll dosyasını sağ tıklatın. 

**ADL kaydetmek için: kayıt derleme (Gelişmiş) komutu**
1.  Komut paletini açmak için Ctrl + Shift + P seçin.
2.  Girin **ADL: kaydetmek (Gelişmiş) derleme**. 
3.  Yerel derleme yolu belirtin. 
4.  JSON dosyası görüntülenir. Gözden geçirin ve kaynak parametreleri ve derleme bağımlılıklar gerekiyorsa düzenleyin. Yönergeler görüntülenir **çıkış** penceresi. Derleme kayıt aşamasına ilerlemek için (Ctrl + S) JSON dosyasını kaydedin.

    ![JSON dosyası derleme bağımlılıkları ve kaynak parametreleri](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
    
>[!NOTE]
>- Azure Data Lake araçları autodetects DLL derleme bağımlılıkları sahip olup olmadığını belirler. Algılanan sonra bağımlılıkları JSON dosyasında görüntülenir. 
>- Derleme kaydı bir parçası olarak, DLL kaynakları (örneğin, .txt, .png ve .csv) karşıya yükleyebilir. 

Tetiklemek için başka bir yolu **ADL: kaydetmek derleme (Gelişmiş)** komuttur dosya Gezgini'nde .dll dosyasını sağ tıklatın. 

Aşağıdaki U-SQL kodu, bütünleştirilmiş çağırmayı gösterir. Örnekte, derleme adı: *test*.


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


## <a name="use-u-sql-local-run-and-local-debug-for-windows-users"></a>U-SQL yerel çalıştırma ve yerel hata ayıklama Windows kullanıcıları için kullanın
U-SQL yerel çalıştırma yerel verilerinizi sınar ve kodunuzu Data Lake Analytics yayımlanmadan önce komut yerel olarak doğrular. Kodunuz için Data Lake Analytics gönderilmeden önce aşağıdaki görevleri tamamlamak için yerel hata ayıklama özelliğini kullanabilirsiniz: 
- C# arka plan kod hata ayıklaması. 
- Kod üzerinden adım. 
- Komut dosyası yerel olarak doğrulayın.

Yerel çalıştırma ve yerel hata ayıklama hakkında yönergeler için bkz: [U-SQL yerel çalıştırma ve Visual Studio Code ile yerel hata ayıklama](data-lake-tools-for-vscode-local-run-and-debug.md).


## <a name="connect-to-azure"></a>Azure'a Bağlanma

Derleme ve Data Lake Analytics U-SQL betikleri çalıştırmak için önce Azure hesabınızda bağlanmanız gerekir.

<b id="sign-in-by-command">Bir komut kullanarak Azure'a bağlanmak için</b>

1.  Komut paletini açmak için Ctrl + Shift + P seçin. 
2.  Girin **ADL: oturum açma**. Oturum açma bilgileri üzerinde sağ alt köşesinde görüntülenir.

    ![Oturum açma komut girme](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)

    ![Bildirim oturum açma ve kimlik doğrulama hakkında](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)

3.  Seçin **kopyalama & Aç** açmak için [oturum açma Web sayfası](https://aka.ms/devicelogin). Kod kutusuna yapıştırın ve ardından **devam**.

    ![Oturum açma Web sayfası](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png)  
     
4.  Web sayfasından oturum açmak için yönergeleri izleyin. Bağlandığınızda, Azure hesap adınızı VS Code penceresinin sol alt köşesinde durum çubuğunda görünür. 

> [!NOTE] 
>- Oturumu yok, data Lake araçları, otomatik olarak sonraki süreyi imzalar.
>- İki etmen etkin hesabınız varsa, PIN kullanmak yerine telefon kimlik doğrulaması kullanmanızı öneririz.


Oturumu kapatmak için aşağıdaki komutu girin **ADL: oturum kapatma**.

**Gezgini'nden Azure'a bağlanmak için**

Genişletme **AZURE DATALAKE**seçin **Azure'da oturum aç**, adım 3 ve 4. adımını izleyin [bir komut kullanarak Azure'a bağlanmak için](#sign-in-by-command).

!["Azure'da oturum açın" seçim Explorer'da](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-sign-in-from-explorer.png )  

Gezgini'nden oturumu olamaz. Oturumu kapatmak için bkz: [bir komut kullanarak Azure'a bağlanmak için](#sign-in-by-command).


## <a name="create-an-extraction-script"></a>Bir ayıklama komut dosyası oluşturma 
.Csv, .tsv ve .txt dosyaları için bir çıkarma betik komutunu kullanarak oluşturabileceğiniz **ADL: Ayıkla komut dosyası oluştur** veya Azure Data Lake Gezgini'nden.

**Bir komut kullanarak bir ayıklama betiği oluşturmak için**

1. Komut palet açmak ve girmek için Ctrl + Shift + P seçin **ADL: Ayıkla komut dosyası oluştur**.
2. Bir Azure depolama dosyasının tam yolu belirtin ve Enter tuşuna seçin.
3. Bir hesap seçin.
4. .Txt dosyası için dosya ayıklamak için bir sınırlayıcı seçin. 

![Bir ayıklama komut dosyası oluşturma işlemi](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-process.png)

Ayıklama komut dosyası girişlerini temel alarak oluşturulur. Sütunları algılayamaz bir betik için iki seçenekten birini seçin. Aksi durumda, yalnızca bir komut dosyası oluşturulur.

![Sonuç bir ayıklama komut dosyası oluşturma](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-result.png)

**Gezgini'nden bir ayıklama betiği oluşturmak için**

Ayıklama betiği oluşturmak için başka bir .csv, .tsv veya .txt dosyası Azure Data Lake Store veya Azure Blob depolama alanındaki sağ tıklatma (kısayol) menüsünde aracılığıyla yoludur.

![Kısayol menüsünden "EXTRACT komut dosyası oluşturma" komutu](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-from-context-menu.png)

## <a name="integrate-with-azure-data-lake-analytics-through-a-command"></a>Bir komutu aracılığıyla Azure Data Lake Analytics ile tümleştirme

Hesapları listeleme, erişim meta verileri ve analytics işleri görüntüle Azure Data Lake Analytics kaynaklara erişebilir. 

**Azure aboneliğinizdeki Azure Data Lake Analytics hesaplarını listelemek için**

1. Komut paletini açmak için Ctrl + Shift + P seçin.
2. Girin **ADL: liste hesapları**. Hesapları görünür **çıkış** bölmesi.

**Azure Data Lake Analytics meta verilerine erişmek için**

1.  Ctrl + Shift + P seçin ve ardından girin **ADL: liste tabloları**.
2.  Data Lake Analytics hesaplardan birini seçin.
3.  Data Lake Analytics veritabanlarından birini seçin.
4.  Şemalar birini seçin. Tabloların listesini görebilirsiniz.

**Azure Data Lake Analytics işleri görüntülemek için**
1.  Komut palet (Ctrl + Shift + P) açın ve seçin **ADL: Göster işleri**. 
2.  Bir Data Lake Analytics veya yerel bir hesap seçin. 
3.  Hesap için görüntülenecek iş listesi bekleyin.
4.  Bir iş iş listesinden seçin. Data Lake araçları sağ bölmede iş görünümünde açılır ve VS Code çıktısında bazı bilgiler görüntüler.

    ![İş listesi](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="integrate-with-azure-data-lake-store-through-a-command"></a>Bir komutu aracılığıyla Azure Data Lake Store ile tümleştirme

Azure Data Lake Store ile ilgili komutları kullanabilirsiniz:
 - [Azure Data Lake Store kaynaklara göz atın](#list-the-storage-path) 
 - [Azure Data Lake Store Dosya Önizleme](#preview-the-storage-file) 
 - [VS code'da Azure Data Lake Store için doğrudan dosyasını karşıya yükleyin](#upload-file-or-folder)
 - [Dosyayı doğrudan Azure Data Lake Store VS code'da indirin](#download-file)

### <a name="list-the-storage-path"></a>Depolama alanı yolu listesi 

**Komut palet depolama yolundan listelemek için**

1. Kod Düzenleyicisi'ni sağ tıklatın ve seçin **ADL: listesi yolu**.
2. Klasörü listeden seçin ya da seçin **bir yol girin** veya **kök yolundan Gözat**. (Kullanmakta olduğunuz **bir yol girin** bir örnek olarak.) 
3. Data Lake Analytics hesabınızı seçin.
4. Gözatın veya depolama klasör yolu (örneğin, / çıkış /) girin.  

Komut palet girişlerini temel yol bilgileri listeler.

![Depolama birimi yolu sonuçları](./media/data-lake-analytics-data-lake-tools-for-vscode/list-storage-path.png)

Kısayol menüsü göreli yolu listelemek için daha kullanışlı bir yoludur.

**Kısayol menüsü üzerinden depolama yolu listelemek için**

Yol dizesi sağ tıklatıp **listesi yolu**.

!["Yol kısayol menüsünde listesinde"](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)


### <a name="preview-the-storage-file"></a>Depolama dosya önizleme

1. Kod Düzenleyicisi'ni sağ tıklatın ve seçin **ADL: Önizleme dosya**.
2. Data Lake Analytics hesabınızı seçin. 
3. Bir Azure depolama dosya yolu (örneğin, /output/SearchLog.txt) girin. 

VS Code'da dosyayı açar.

![Adımlar ve depolama dosya önizleme için sonuç](./media/data-lake-analytics-data-lake-tools-for-vscode/preview-storage-file.png)

Dosyanın önizlemesini görmek için başka bir dosyanın tam yolunu veya dosyanın göreli yolu komut dosyası Düzenleyicisi'nde kısayol menüsünde aracılığıyla yoludur. 

### <a name="upload-a-file-or-folder"></a>Bir dosya veya klasör karşıya yükle

1. Kod Düzenleyicisi'ni sağ tıklatın ve seçin **dosyasını karşıya yükle** veya **karşıya yükleme klasörü**.
2. Seçtiyseniz, bir dosya veya birden çok dosya seçin **dosyasını karşıya yükle**, veya seçtiyseniz, tüm klasörünü seçin **karşıya yükleme klasörü**. Ardından **karşıya**. 
3. Depolama klasörü listeden seçin ya da seçin **bir yol girin** veya **kök yolundan Gözat**. (Kullanmakta olduğunuz **bir yol girin** bir örnek olarak.) 
4. Data Lake Analytics hesabınızı seçin. 
5. Gözatın veya depolama klasör yolu (örneğin, / çıkış /) girin. 
6. Seçin **geçerli klasör seç** karşıya yükleme Hedefinizi belirtmek için.

![Adımlar ve bir dosya veya klasör yüklemeyle sonucu](./media/data-lake-analytics-data-lake-tools-for-vscode/upload-file.png)    

Dosyaları depolama alanına yüklemek için başka bir dosyanın tam yolunu veya dosyanın göreli yolu komut dosyası Düzenleyicisi'nde kısayol menüsünde aracılığıyla yoludur.

Yapabilecekleriniz [yükleme durumunu izlemek](#check-storage-tasks-status).


### <a name="download-a-file"></a>Dosya indirme 
Komutunu kullanarak bir dosyayı indirebilirsiniz **ADL: karşıdan dosya** veya **ADL: karşıdan dosya (Gelişmiş)**.

**ADL aracılığıyla bir dosyayı indirmek için: dosya yükle (Gelişmiş) komutu**
1. Kod Düzenleyicisi'ni sağ tıklatın ve ardından **karşıdan dosya (Gelişmiş)**.
2. VS Code'da JSON dosyasını görüntüler. Dosya yolları girin ve aynı anda birden çok dosyalarını yükleyin. Yönergeler görüntülenir **çıkış** penceresi. Dosya veya dosyalar yüklemeye devam etmek için (Ctrl + S) JSON dosyasını kaydedin.

    ![Yollar JSON dosyasını karşıdan dosya yükleme için](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-files.png)

**Çıkış** penceresinde dosya indirme durumunu görüntüler.

![Karşıdan yükleme durumu ile Çıktı penceresi](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-file-result.png)     

Yapabilecekleriniz [yükleme durumunu izlemek](#check-storage-tasks-status).

**ADL aracılığıyla bir dosyayı indirmek için: dosya indirme komutu**

1. Komut Dosyası Düzenleyicisi'ni sağ tıklayın, **karşıdan dosya**ve ardından hedef klasöründen **Klasör Seç** iletişim kutusu.
2. Klasörü listeden seçin ya da seçin **bir yol girin** veya **kök yolundan Gözat**. (Kullanmakta olduğunuz **bir yol girin** bir örnek olarak.) 
3. Data Lake Analytics hesabınızı seçin. 
4. Göz atın veya depolama klasör yolu (örneğin, / çıkış /) girin ve ardından indirmek için bir dosya seçin.

![Adımlar ve sonucu bir dosya indirme](./media/data-lake-analytics-data-lake-tools-for-vscode/download-file.png) 

Depolama dosyalarını indirmek için başka bir dosyanın tam yolunu veya dosyanın göreli yolu komut dosyası Düzenleyicisi'nde kısayol menüsünde aracılığıyla yoludur.

Yapabilecekleriniz [yükleme durumunu izlemek](#check-storage-tasks-status).

### <a name="check-storage-tasks-status"></a>Depolama görevlerin durumunu denetle
Karşıya yükleme ve indirme durumu durum çubuğunda görüntülenir. Durum Çubuğu'nu seçin ve üzerinde durumu görünür **çıkış** sekmesi.

![Durum çubuğu ve çıkış ayrıntıları](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-status.png)


## <a name="integrate-with-azure-data-lake-analytics-from-the-explorer"></a>Gezgini'nden Azure Data Lake Analytics ile tümleştirme

Oturum açtıktan sonra Azure hesabınız için tüm abonelikleri sol bölmede altında listelenen **AZURE DATALAKE**. 

![Data Lake Gezgini](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer.png)

### <a name="data-lake-analytics-metadata-navigation"></a>Data Lake Analytics meta veri gezinme

Azure aboneliğiniz genişletin. Altında **U-SQL veritabanları** düğümü, U-SQL veritabanınızı göz atabilir ve görünüm klasörleri ister **şemaları**, **kimlik bilgileri**, **derlemeleri**, **Tabloları**, ve **dizin**.

### <a name="data-lake-analytics-metadata-entity-management"></a>Data Lake Analytics meta veri varlık yönetimi

Genişletme **U-SQL veritabanları**. Karşılık gelen düğümünü sağ tıklayarak ve ardından seçerek bir veritabanı, şema, tablo, tablo türü, dizini veya istatistiği oluşturabilirsiniz **komut dosyası oluşturma** kısayol menüsünde. Açık kod sayfasında, gereksinimlerinize göre betiğini düzenleyin. Ardından, sağ tıklayıp seçerek işi göndermek **ADL: işi Gönder**. 

Öğe oluşturma işlemini tamamladıktan sonra düğümünü sağ tıklatın ve ardından **yenileme** öğesini göstermek için. Öğeyi sağ tıklatıp sonra seçerek silebilirsiniz **silmek**.

![Data Lake Explorer'da kısayol menüsünde "Komut dosyası oluşturma" komutu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create.png)

![Yeni öğe için komut dosyası sayfası](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create-snippet.png)

### <a name="data-lake-analytics-assembly-registration"></a>Data Lake Analytics derleme kaydı

Sağ tıklayarak bir derlemeyi karşılık gelen veritabanında kaydettirebilirsiniz **derlemeleri** düğümünü ve ardından seçerek **kaydetmek derleme**.

![Derlemeleri düğümünün kısayol menüsünde "derlemesini kaydetme" komutu](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer-register-assembly.png)

## <a name="integrate-with-azure-data-lake-store-from-the-explorer"></a>Gezgini'nden Azure Data Lake Store ile tümleştirme

Gözat **Data Lake Store**:

- Klasörü düğümüne sağ tıklayın ve ardından **yenileme**, **silmek**, **karşıya**, **karşıya yükleme klasörü**, **Kopyala Göreli yol**, ve **tam yol Kopyala** kısayol menüsünde komutları.

   ![Data Lake Gezgininde klasör düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-folder-menu.png)

- Dosya düğümüne sağ tıklayın ve ardından **Önizleme**, **karşıdan**, **silmek**, **Ayıkla komut dosyası oluştur** (yalnızca CSV için kullanılabilir TSV ve TXT dosyaları), **göreli yol Kopyala**, ve **tam yol Kopyala** kısayol menüsünde komutları.

   ![Data Lake Gezgininde dosya düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-extract.png)

## <a name="integrate-with-azure-blob-storage-from-the-explorer"></a>Azure Blob storage Gezgini ile tümleştirme

BLOB depolama alanına göz atın:

- Blob kapsayıcı düğümünü sağ tıklatın ve ardından **yenileme**, **Blob kapsayıcısını silmek**, ve **karşıya Blob** kısayol menüsünde komutları.

   ![Blob Depolama blob kapsayıcısını düğümünde için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-blob-container-node.png)

- Klasörü düğümüne sağ tıklayın ve ardından **yenileme** ve **karşıya Blob** kısayol menüsünde komutları.

   ![Blob Depolama altında bir klasör düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-folder-node.png)

- Dosya düğümüne sağ tıklayın ve ardından **Önizleme/Düzenle**, **karşıdan**, **silmek**, **Ayıkla komut dosyası oluştur** (kullanılabilir yalnızca CSV, TSV ve TXT dosyaları için) **göreli yol Kopyala**, ve **tam yol Kopyala** kısayol menüsünde komutları.

    ![Blob Depolama Birimi dosya düğümünde için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-from-context-menu-2.png)

## <a name="open-the-data-lake-explorer-in-the-portal"></a>Data Lake explorer'ı portalda açın
1. Komut paletini açmak için Ctrl + Shift + P seçin.
2. Girin **açık Web Azure Storage Gezgini** veya göreli bir yol veya tam yolunu komut dosyası Düzenleyicisi'nde sağ tıklayın ve ardından **açık Web Azure Storage Gezgini**.
3. Bir Data Lake Analytics hesabı seçin.

Data Lake araçları Azure portalında Azure Storage yol açar. FIND yol ve dosya Web'den Önizleme.

## <a name="additional-features"></a>Ek Özellikler

VS Code'da için Data Lake araçları aşağıdaki özellikleri destekler:

-   **IntelliSense otomatik tamamlama**: önerileri anahtar sözcükler, yöntemleri ve değişkenler gibi öğeleri geçici açılır pencere görünür. Farklı simgeler farklı tip nesneyi temsil eder:

    - Scala veri türü
    - karmaşık veri türü
    - Yerleşik atama
    - .NET koleksiyonu ve sınıfları
    - C# ifadeleri
    - Yerleşik C# UDF'ler, Udo'lar ve UDAAGs 
    - U-SQL işlevleri
    - U-SQL Pencereleme işlevleri
 
    ![IntelliSense nesne türleri](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   **Data Lake Analytics meta veriler üzerinde IntelliSense otomatik tamamlama**: Data Lake araçları, Data Lake Analytics meta veri bilgileri yerel olarak indirir. IntelliSense özelliği, Data Lake Analytics meta veri nesneleri otomatik olarak doldurur. Bu nesneler, veritabanı, şema, tablo, görünüm, Tablo Değerli Fonksiyon, yordamlar ve C# derlemeleri içerir.
 
    ![IntelliSense meta verileri](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   **IntelliSense hata işaret**: Data Lake araçları U-SQL ve C# için düzenleme hataları altını çizer. 
-   **Sözdizimi vurgular**: Data Lake araçları, değişkenleri, anahtar sözcükler, veri türleri ve işlevleri gibi öğeleri ayırt etmek için renk kullanır. 

    ![Çeşitli renklerle sözdizimi](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

> [!NOTE]
> Azure Data Lake araçları Visual Studio sürüm 2.3.3000.4 yükseltmenizi öneririz veya sonraki bir sürümü. Önceki sürümleri artık indirme için kullanılabilir ve artık kullanım dışı bırakılmıştır.  
   
## <a name="next-steps"></a>Sonraki adımlar
- [U-SQL Python, R ve C ile geliştirmek için Azure Data Lake Analytics VS code'da Sharp](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
- [U-SQL yerel çalıştırma ve Visual Studio Code ile yerel hata ayıklama](data-lake-tools-for-vscode-local-run-and-debug.md)
- [Öğretici: Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
- [Öğretici: Visual Studio için Data Lake araçları kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
