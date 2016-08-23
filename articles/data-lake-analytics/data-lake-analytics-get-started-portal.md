<properties 
   pageTitle="Azure Portal'ı kullanarak Azure Data Lake Analytics ile çalışmaya başlama | Azure" 
   description="Bir Data Lake Analytics hesabı oluşturmak, U-SQL'yi kullanarak Data Lake Analytics işi oluşturmak ve işi göndermek için Azure Portal'ın nasıl kullanılacağını öğrenin. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# Öğretici: Azure Portal'ı kullanarak Azure Data Lake Analytics ile çalışmaya başlama

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Azure Data Lake Analytics hesapları oluşturmak, Data Lake Analytics işlerini [U-SQL](data-lake-analytics-u-sql-get-started.md) içinde tanımlamak ve Data Lake Analytics hesaplarına iş göndermek için Azure Portal'ın nasıl kullanılacağını öğrenin. Data Lake Analytics hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).

Bu öğreticide, bir sekmeyle ayrılmış değerler (TSV) dosyasını okuyan ve bunu virgülle ayrılmış değerler (CSV) dosyasına dönüştüren bir iş geliştireceksiniz. Öğreticiyi desteklenen diğer araçları kullanarak tamamlamak için bu bölümün üst kısmındaki sekmelere tıklayın. İlk işiniz başarılı olduktan sonra, U-SQL ile daha karmaşık veri dönüşümleri yazmaya başlayabilirsiniz.

[AZURE.INCLUDE [basic-process-include](../../includes/data-lake-analytics-basic-process.md)]

##Önkoşullar

Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

- **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

##Data Lake Analytics hesabı oluşturma

Herhangi bir işi çalıştırmadan önce bir Data Lake Analytics hesabına sahip olmanız gerekir.

Her Data Lake Analytics hesabı, bir [Azure Data Lake Store]() hesabı bağımlılığına sahiptir.  Bu hesap, varsayılan Data Lake Store hesabı olarak adlandırılır.  Data Lake Store hesabını önceden veya Data Lake Analytics hesabınızı oluşturduğunuzda oluşturabilirsiniz. Bu öğreticide, Data Lake Store hesabını Data Lake Analytics hesabıyla oluşturacaksınız.

**Data Lake Analytics hesabı oluşturmak için**

1. Yeni [Klasik Azure Portalı](https://portal.azure.com)'nda oturum açın.
2. **Yeni** öğesine, **Veri + Analiz** öğesine ve ardından **Data Lake Analytics**'e tıklayın.
6. Aşağıdakileri yazın veya seçin:

    ![Azure Data Lake Analytics portalı dikey penceresi](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **Ad**: Analytics hesabına bir ad verin.
    - **Data Lake Store**: Her Data Lake Analytics hesabı, bağımlı bir Data Lake Store hesabına sahiptir. Data Lake Analytics hesabı ve bağımlı Data Lake Store hesabı aynı Azure veri merkezinde bulunmalıdır. Yeni bir Data Lake Store hesabı oluşturmaya yönelik yönergeyi uygulayın veya var olan bir hesabı seçin.
    - **Abonelik**: Analytics hesabı için kullanılan Azure aboneliğini seçin.
    - **Kaynak Grubu**. Var olan bir Azure Kaynak Grubu'nu seçin veya yeni bir grup oluşturun. Azure Resource Manager (ARM), uygulamanızdaki kaynaklarla bir grup olarak çalışmanıza olanak sağlar. Daha fazla bilgi için bkz. [Azure Resource Manager'a Genel Bakış](resource-group-overview.md). 
    - **Konum**. Data Lake Analytics hesabı için bir Azure veri merkezi seçin. 
7. **Başlangıç Panosuna Sabitle** seçeneğini belirleyin. Bu işlem, bu öğreticinin uygulanması için gereklidir.
8. **Oluştur**'a tıklayın. Bu işlem sizi Başlangıç Panosu'na götürür. Başlangıç Panosu'na, "Azure Data Lake Analytics'i dağıtma" etiketine sahip yeni bir kutucuk eklenir. Data Lake Analytics hesabının oluşturulması çok kısa süren bir işlemdir. Hesap oluşturulduğunda portal, hesabı portal üzerinde yeni bir dikey pencerede açar.

    ![Azure Data Lake Analytics portalı dikey penceresi](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)


Data Lake Analytics hesabı oluşturulduktan sonra, ek Data Lake Store hesapları ve Azure Storage hesapları ekleyebilirsiniz. Yönergeler için bkz. [Data Lake Analytics hesabı veri kaynaklarını yönetme](data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

##Kaynak verileri hazırlama

Bu öğreticide, bazı arama günlüklerini işleyeceksiniz.  Arama günlüğü, Data Lake Store veya Azure Blob depolama alanında depolanabilir. 

Azure Portal, bir arama günlüğü dosyası içeren bazı örnek veri dosyalarını varsayılan Data Lake hesabına kopyalamak için bir kullanıcı arabirimi sağlar.

**Örnek veri dosyalarını kopyalamak için**

1. Azure Portal'dan, sol üst köşedeki **Microsoft Azure**'a tıklayın.
2. Data Lake Analytics hesap adınızı içeren kutucuğa tıklayın.  Bu kutucuk, hesap oluşturulduğunda buraya sabitlenmiştir.
Hesap buraya sabitlenmemişse hesabı açmak bkz. [Portaldan Data Lake Analytics hesabı açma](data-lake-analytics-manage-use-portal.md#access-adla-account).
3. **Temel Bileşenler** bölmesini genişletin ve ardından **Örnek işleri keşfedin** seçeneğine tıklayın. Bu işlem sonucunda, **Örnek İşler** adlı başka bir dikey pencere açılır.
4. **Örnek Verileri Kopyala** seçeneğine ve ardından onaylamak için **Tamam**'a tıklayın.
5. Zil şeklinde bir simge olan **Bildirim** öğesine tıklayın. **Örnek verileri güncelleştirme işlemi tamamlandı** ifadesini içeren bir günlük göreceksiniz. Bildirim bölmesini kapatmak için, bölmenin dışındaki herhangi bir konuma tıklayın.
7. Data Lake Analytics hesabı dikey penceresinden, üstteki **Veri Gezgini**'ne tıklayın. 

    ![Azure Data Lake Analytics veri gezgini penceresi](./media/data-lake-analytics-get-started-portal/data-lake-analytics-data-explorer-button.png)

    Bu işlem sonucunda iki dikey pencere açılır. Bunlardan biri **Veri Gezgini**, diğeri ise varsayılan Data Lake Store hesabıdır.
8. Varsayılan Data Lake Store hesabı dikey penceresinde, klasörü genişletmek için **Örnekler** seçeneğine, klasörü genişletmek için de **Veri** seçeneğine tıklayın. Aşağıdaki dosyaları ve klasörleri göreceksiniz:

    - AmbulanceData/
    - AdsLog.tsv
    - SearchLog.tsv
    - version.txt
    - WebLog.log
    
    Bu öğreticide, SearchLog.tsv dosyasını kullanacaksınız.

Uygulamada, uygulamalarınızı bağlı depolama hesaplarına veri yazmak veya veri yüklemek üzere programlayacaksınız. Dosya yükleme için bkz. [Data Lake Store'a veri yükleme](data-lake-analytics-manage-use-portal.md#upload-data-to-adls) veya [Blob depolama alanına veri yükleme](data-lake-analytics-manage-use-portal.md#upload-data-to-wasb).

##Data Lake Analytics işleri oluşturma ve gönderme

Veri kaynağını hazırladıktan sonra, U-SQL betiği geliştirmeye başlayabilirsiniz.  

**İşi göndermek için**

1. Portaldaki Data Lake Analytics hesabı dikey penceresinden **Yeni İş**'e tıklayın. 

    ![Azure Data Lake Analytics yeni iş düğmesi](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job-button.png)

    Dikey pencereyi görmüyorsanız bkz. [Portaldan Data Lake Analytics hesabı açma](data-lake-analytics-manage-use-portal.md#access-adla-account).
4. **İş Adı**'nı ve şu U-SQL betiğini girin:

    ![Azure Data Lake Analytics U-SQL işleri oluşturma](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job.png)

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
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Bu U-SQL betiği, **Extractors.Tsv()** öğesini kullanarak kaynak veri dosyasını okur ve ardından **Outputters.Csv()** öğesini kullanarak bir csv dosyası oluşturur. 
    
    Kaynak dosyayı farklı bir konuma kopyalamadıkça bu iki yolu değiştirmeyin.  Data Lake Analytics, mevcut olmaması halinde çıkış klasörünü oluşturur.  Bu durumda biz basit, göreli yolları kullanıyoruz.  
    
    Varsayılan Data Lake hesaplarında depolanan dosyalar için göreli yolların kullanılması daha basittir. Mutlak yol da kullanabilirsiniz.  Örneğin: 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
      

    U-SQL hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md) ve [U-SQL dili başvurusu](http://go.microsoft.com/fwlink/?LinkId=691348).
     
5. Üstteki **İşi Gönder** seçeneğine tıklayın. Yeni bir İş Ayrıntıları bölmesi açılır. Başlık çubuğunda iş durumu gösterilir.   
6. İş durumunun **Başarılı** olarak değiştirilmesini bekleyin. İş tamamlandığında, portal yeni bir dikey pencerede iş ayrıntılarını açar:

    ![Azure Data Lake Analytics işi ayrıntıları](./media/data-lake-analytics-get-started-portal/data-lake-analytics-job-completed.png)

    Önceki ekran görüntüsünde, işin Gönderildi durumundan Sona Erdi durumuna geçerek tamamlanmasının yaklaşık 1,5 dakika sürdüğünü görebilirsiniz.
    
    İş başarısız olduysa bkz. [Data Lake Analytics işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorials.md).

7. **İş Ayrıntısı** dikey penceresinin alt kısmında, **SearchLog-from-Data-Lake.csv** içindeki iş adına tıklayın. Çıkış dosyasını indirebilir, yeniden adlandırabilir ve silebilirsiniz.

    ![Azure Data Lake Analytics işi çıkış dosyası özellikleri](./media/data-lake-analytics-get-started-portal/data-lake-analytics-output-file-properties.png)
8. Çıkış dosyasını görmek için **Önizleme**'ye tıklayın.

    ![Azure Data Lake Analytics işi çıkış dosyası önizlemesi](./media/data-lake-analytics-get-started-portal/data-lake-analytics-job-output-preview.png)

##Ayrıca bkz.

- Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.
- U-SQL uygulamalarını geliştirmeye başlamak için bkz. [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
- Yönetim görevleri için bkz. [Azure Portal'ı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).
- Data Lake Analytics'e yönelik bir genel bakış için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).
- Aynı öğreticiyi diğer araçları kullanarak görmek için sayfanın üst kısmındaki sekme seçicilerine tıklayın.



<!--HONumber=Aug16_HO1-->


