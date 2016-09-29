Bu öğreticide, bir Azure HDInsight (Hadoop) kümesinde Hive betiği çalıştırarak veri işleyen bir veri işlem hattı ile ilk Azure veri fabrikanızı oluşturacaksınız. 

Bu makale, Azure Data Factory‘ye kavramsal bir genel bakış sağlamamaktadır. Hizmete kavramsal bir genel bakış için [Azure Data Factory'ye giriş](../articles/data-factory/data-factory-introduction.md) makalesini okuyun.

## Bu öğreticide açıklananlar nelerdir? 
**Azure Data Factory**, veri **taşıma** ve veri **işleme** görevlerini veriye dayalı iş akışları (veri işlem hatları adı da verilir) olarak oluşturmanıza olanak tanır. Veri işleme (ya da veri dönüştürme) görevine sahip ilk veri işlem hattınızı oluşturmayı öğreneceksiniz. Bu görev, bir Azure HDInsight kümesi kullanarak web günlüklerini dönüştürüp çözümler.  

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

1.  **Veri fabrikasını** oluşturun. Veri fabrikası, verileri taşıyıp işleyen bir veya daha fazla veri işlem hattı içerebilir. 
2.  **Bağlı hizmetleri** oluşturun. Bir veri deposunu veya işlem hizmetini, veri fabrikasına bağlamak için bir bağlı hizmet oluşturursunuz. Azure Depolama gibi bir veri deposu, veri işlem hattındaki etkinliklerin giriş/çıkış verilerini tutar. Azure HDInsight gibi bir işlem hizmeti, verileri işler/dönüştürür.    
3.  Girdi ve çıktı **veri kümeleri** oluşturun. Giriş veri kümesi, veri işlem hattındaki bir etkinlik için girişi ve çıktı veri kümesi, etkinliğin çıktısını temsil eder.
3.  **İşlem hattını** oluşturun. İşlem hattında bir veya daha fazla etkinlik bulunabilir (Örnek: Kopya Etkinliği, HDInsight Hive Etkinliği). Bu örnekte, bir Hive betiği çalıştıran HDInsight Hive etkinliği kullanılmaktadır. Betik ilk olarak Azure blob depolamada depolanan ham web günlüğü verilerine başvuran bir dış tablo oluşturur ve ardından ham verileri yıl ve aya göre bölümlere ayırır.
 
![Data Factory öğreticisinde diyagram görünümü](./media/data-factory-tutorial-prerequisites/data-factory-tutorial-diagram-view.png)


Bu öğreticide, adfgetstarted (kapsayıcı) = > inputdata (klasör), input.log adlı bir dosya içermektedir. Bu günlük dosyasında üç aya ait girişler vardır: Ocak, Şubat ve Mart 2014. Girdi dosyasındaki, her aya ait örnek satırlar şunlardır. 

    2014-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
    2014-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
    2014-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871

Dosya, işlem hattı tarafından HDInsight Hive etkinliği ile işlendiğinde etkinlik, giriş verilerini yıl ve aya göre bölümlere ayıran HDInsight kümesi üzerinde bir Hive betiği çalıştırır. Betik, her bir aya ait girişlerin bulunduğu bir dosya içeren üç çıktı klasörü oluşturur.  

    adfgetstarted/partitioneddata/year=2014/month=1/000000_0
    adfgetstarted/partitioneddata/year=2014/month=2/000000_0
    adfgetstarted/partitioneddata/year=2014/month=3/000000_0

Yukarıda gösterilen örnek satırlardan ilki (2014-01-01 içeren), month=1 klasöründe 000000_0 dosyasına yazılır. Benzer şekilde, ikinci satır month=2 klasöründeki dosyaya ve üçüncü satır month=3 klasöründeki dosyaya yazılır.  


## Ön koşullar
Bu öğreticiye başlamadan önce aşağıdaki önkoşullara sahip olmanız gerekir:

1.  **Azure aboneliği**: Aboneliğiniz yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Nasıl ücretsiz bir deneme hesabı edinebileceğinizi öğrenmek için [Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/) makalesine bakın.

2.  **Azure Depolama**: Bu öğreticide, verileri depolamak için bir Azure depolama hesabı kullanılmaktadır. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../articles/storage/storage-create-storage-account.md#create-a-storage-account) makalesine bakın. Depolama hesabını oluşturduktan sonra, depolamaya erişmek için kullanılan hesap anahtarını edinmeniz gerekir. Bkz. [Depolama erişim tuşlarını görüntüleme, kopyalama ve yeniden oluşturma](../articles/storage/storage-create-storage-account.md#view-and-copy-storage-access-keys).

## Öğretici için Azure Depolama’ya dosya yükleme
Öğreticiye başlamadan önce, öğretici için gerekli dosyaları kullanarak Azure depolamayı hazırlamanız gerekir.

Bu bölümde şunları gerçekleştireceksiniz:

2. Hive sorgu dosyasını (HQL) **adfgetstarted** kapsayıcısının **script** klasörüne yükleyin.
3. Giriş dosyasını **adfgetstarted** kapsayıcısının **inputdata** klasörüne yükleyin. 

### HQL betik dosyası oluşturma 

1. **Not Defteri**’ni başlatın ve aşağıdaki HQL betiğini yapıştırın. Bu Hive betiği iki tablo oluşturur: **WebLogsRaw** ve **WebLogsPartitioned**. Menüde **Dosya**’ya tıklayıp **Farklı Kaydet**’i seçin. Sabit diskinizde **C:\adfgetstarted** klasörüne geçin. **Kayıt türü** alanı için **Tüm Dosyalar (*.*)** seçeneğini belirleyin. **Dosya adı** olarak **partitionweblogs.hql** girin. İletişim kutusunun alt kısmındaki **Kodlama** alanının **ANSI** olarak ayarlandığını doğrulayın. Öyle değilse, **ANSI** olarak ayarlayın.  

        set hive.exec.dynamic.partition.mode=nonstrict;
        
        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        tblproperties ("skip.header.line.count"="2");
        
        LOAD DATA INPATH '${hiveconf:inputtable}' OVERWRITE INTO TABLE WebLogsRaw;
        
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';
        
        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw

Çalışma zamanında Data Factory işlem hattındaki Hive Etkinliği, inputtable ve partitionedtable parametreleri için aşağıdaki parçacıkta gösterildiği gibi değerler geçirir. Burada storageaccountname, Azure depolama hesabınızın adıdır. 

        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
 
### Örnek giriş dosyası oluşturma
Not Defteri’ni kullanarak **c:\adfgetstarted** klasöründe aşağıdaki içeriğe sahip **input.log** adlı bir dosya oluşturun: 

    #Software: Microsoft Internet Information Services 8.0
    #Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step5.png X-ARR-LOG-ID=9b0c14b1-434d-495a-9b0d-46775194257b 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 37931 871 32
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step6.png X-ARR-LOG-ID=f99cff81-ec38-4a13-b2fe-21b10adca996 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 27146 871 47
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step1.png X-ARR-LOG-ID=af94d3b4-8e05-4871-82c4-638f866d4e83 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 66259 871 140
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47

### Giriş dosyası ve HQL dosyasını Azure Blob Depolama biriminize yükleme

Bu bölümde, Azure Blob Depolama’ya dosya kopyalamak için **AzCopy** aracını kullanmaya ilişkin yönergeler sunulmaktadır. Bu görevi gerçekleştirmek için, istediğiniz herhangi bir aracı kullanabilirsiniz (örnek: [Microsoft Azure Depolama Gezgini](http://storageexplorer.com/), [ClumsyLeaf Software ürünü olan CloudXPlorer](http://clumsyleaf.com/products/cloudxplorer)).   
     
2. Azure depolamayı öğreticiye hazırlamak için:
    1. [En son **AzCopy** sürümünü](http://aka.ms/downloadazcopy) veya [en son önizleme sürümünü](http://aka.ms/downloadazcopypr) indirin. Yardımcı programı kullanma yönergeleri için [AzCopy’yi kullanma](../articles/storage/storage-use-azcopy.md) makalesine bakın.
    2. Komut isteminde aşağıdaki komutu çalıştırarak AzCopy konumunu sistem yoluna ekleyin. 
    
            set path=%path%;C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy

    3. C:\adfgetstarted klasörüne gidin ve aşağıdaki komutu çalıştırın. Bu komut, **input.log** dosyasını depolama hesabına yükler (**adfgetstarted** kapsayıcısı ve **inputdata** klasörü). **StorageAccountName** yerine depolama hesabınızın adını ve **Storage Key** yerine de depolama hesabı anahtarını yazın.

            AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/inputdata /DestKey:<storagekey>  /Pattern:input.log

        > [AZURE.NOTE] Bu komut, Azure Blob depolama biriminizde **adfgetstarted** adında bir kapsayıcı oluşturur ve **input.log** dosyasını yerel sürücünüzden kapsayıcıdaki **inputdata** klasörüne kopyalar. 
    
    5. Dosya başarıyla yüklendikten sonra AzCopy’den aşağıdakine benzer bir çıktı görürsünüz.
    
            Finished 1 of total 1 file(s).
            [2015/12/16 23:07:33] Transfer summary:
            -----------------
            Total files transferred: 1
            Transfer successfully:   1
            Transfer skipped:        0
            Transfer failed:         0
            Elapsed time:            00.00:00:01
    1. Aşağıdaki komutu çalıştırarak, **partitionweblogs.hql** dosyasını **adfgetstarted** kapsayıcısının **script** klasörüne yükleyin. Komut aşağıdaki gibidir: 
    
            AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/script /DestKey:<storagekey>  /Pattern:partitionweblogs.hql




<!--HONumber=sep16_HO2-->


