---
title: Hive tabloları oluşturma ve Azure Blob depolama alanından veri yükleme | Microsoft Docs
description: Hive tabloları oluşturma ve tabloları hive blob veri yükleme
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: jhubbard
editor: cgronlun
ms.assetid: cff9280d-18ce-4b66-a54f-19f358d1ad90
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: 474eb7122de59d12c69b7c1021cfdff8548c5a25
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34837965"
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a>Hive tabloları oluşturma ve Azure Blob depolama alanından veri yükleme
Bu konu, Hive tabloları oluşturma ve Azure blob depolama alanından veri yükleme genel Hive sorguları gösterir. Hive tablolarını bölümlendirme ve en iyi duruma getirilmiş satır sütunlu (sorgu performansını artırmak için biçimlendirme ORC) kullanarak bu bazı yönergeler de sağlanır.

Bu **menü** burada veri depolanabilir ve takım veri bilimi işlem (TDSP) sırasında işlenen hedef ortamlara veri alma nasıl açıklayan konulara bağlantılar.

[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, sahip olduğunuz varsayılmaktadır:

* Bir Azure depolama hesabı oluşturuldu. Yönergeler gerekiyorsa bkz [Azure storage hesapları hakkında](../../storage/common/storage-create-storage-account.md).
* Özelleştirilmiş bir Hadoop kümesine Hdınsight hizmetiyle sağlandı.  Yönergeler gerekiyorsa bkz [özelleştirme Azure Hdınsight Hadoop kümeleri için Gelişmiş analiz](customize-hadoop-cluster.md).
* Küme için etkin uzaktan erişim, oturum açmış ve Hadoop komut satırı konsolu açılır. Yönergeler gerekiyorsa bkz [Hadoop küme baş düğümü erişim](customize-hadoop-cluster.md).

## <a name="upload-data-to-azure-blob-storage"></a>Azure blob depolama alanına veri yükleme
Sağlanan yönergeleri izleyerek bir Azure sanal makinesi oluşturduysanız [Gelişmiş analiz için Azure sanal makinesi ayarlama](../data-science-virtual-machine/setup-virtual-machine.md), bu komut dosyası indirilmiş oldukları *C:\\kullanıcılar \\ \<kullanıcı adı\>\\belgeleri\\veri bilimi betikleri* sanal makinede dizin. Bu Hive sorguları yalnızca kendi veri şeması ve Azure blob depolama yapılandırması gönderimi için hazır olması için uygun alanları takın gerektirir.

Hive tablolarını verilerini de olduğunu varsayıyoruz bir **sıkıştırılmamış** tablo biçiminde ve verileri varsayılan (veya ek bir) yüklendiğini Hadoop küme tarafından kullanılan depolama hesabının kapsayıcı.

Uygulama için istiyorsanız **NYC ücreti seyahat veri**, gerekir:

* **karşıdan** 24 [NYC ücreti seyahat veri](http://www.andresmh.com/nyctaxitrips) (12 seyahat dosyalar ve 12 ücreti dosyaları)
* **Unzip** .csv dosyalarına tüm dosyaları ve ardından
* **karşıya yükleme** özetlenen yordamı tarafından oluşturulan Azure depolama hesabı varsayılan (veya uygun bir kapsayıcı) onları [özelleştirme Azure Hdınsight Hadoop kümeleri Advanced Analytics işlemi ve teknolojiiçin](customize-hadoop-cluster.md)konu. İşlem depolama hesabındaki varsayılan kapsayıcı .csv dosyalarını yüklemek için bu bilgisayarda bulunabilir [sayfa](hive-walkthrough.md#upload).

## <a name="submit"></a>Hive sorguları gönderme
Hive sorgularını kullanarak gönderilebilir:

1. [Hadoop kümesi headnode Hive sorguları Hadoop komut satırı aracılığıyla gönderme](#headnode)
2. [Hive düzenleyicisinde Hive sorguları gönderme](#hive-editor)
3. [Azure PowerShell komutlarıyla Hive sorguları gönderme](#ps)

Hive sorguları SQL benzeri. SQL ile hakkında bilginiz varsa, bulabilirsiniz [Hive SQL kullanıcılar kopya sayfası](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) yararlıdır.

Bir Hive sorgusu gönderirken, ayrıca Hive sorguları çıktısını hedef, ekranda veya baş düğüm yerel bir dosyaya veya bir Azure blob'a olması olup olmadığını kontrol edebilirsiniz.

### <a name="headnode"></a> 1. Hadoop kümesi headnode Hive sorguları Hadoop komut satırı aracılığıyla gönderme
Hive sorgusu karmaşıksa, doğrudan Hadoop baş düğümünde gönderme küme genellikle daha hızlı Hive Düzenleyicisi'ni veya Azure PowerShell komut dosyalarıyla gönderme daha kapatma neden olmaktadır.

Hadoop küme baş düğümüne oturum açın, masaüstünde baş düğüm, Hadoop komut satırı açın ve komutu girin `cd %hive_home%\bin`.

Hadoop komut satırı Hive sorguları göndermek için üç yol vardır:

* doğrudan
* .hql dosyalarını kullanma
* Hive komutu konsoluyla

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Hive sorguları doğrudan içinde Hadoop komut satırı gönderin.
Komutu gibi çalıştırabilirsiniz `hive -e "<your hive query>;` basit Hive sorguları doğrudan içinde Hadoop komut satırı göndermek için. Burada, burada kırmızı kutu Hive sorgusu gönderdiğinde komutu özetler ve Hive sorgusu çıktısını yeşil kutunun özetlenmektedir bir örnek verilmiştir.

![Çalışma alanı oluştur](./media/move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Hive sorguları .hql dosyalarında gönderme
Komut satırı ya da Hive komut konsolundan sorguları düzenleme, Hive sorgusu daha karmaşıktır ve birden çok satıra sahip olduğunda, pratik değildir. Bir metin düzenleyicisi Hadoop kümesi baş düğümünde bir yerel dizinin baş düğümü .hql dosyasında Hive sorguları kaydetmek için kullanılacak bir alternatiftir. Hive sorgusu .hql dosyasında kullanarak gönderilebilir sonra `-f` şekilde bağımsız değişkeni:

    hive -f "<path to the .hql file>"

![Çalışma alanı oluştur](./media/move-hive-tables/run-hive-queries-3.png)

**İlerleme durumu ekranı yazdırma Hive sorgularının gösterme**

Hive sorgusu Hadoop komutu gönderildikten sonra varsayılan olarak, harita/azaltın işinin ilerleme durumunu ekranda yazdırılır. Harita/azaltın iş ilerleme ekranı yazdırma gizlemek için bağımsız değişken kullanabilirsiniz `-S` (büyük harflerle "S") komut satırı aşağıdaki gibi:

    hive -S -f "<path to the .hql file>"
.    -S -e hive "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Hive komut konsolunda Hive sorguları göndermek.
Komutunu çalıştırarak da ilk Hive komut konsolundan girebilirsiniz `hive` Hadoop komut satırı ve Hive komut konsolunda Hive sorguları göndermek. Bir örnek verilmiştir. Bu örnekte, iki kırmızı kutu Hive komut konsolundan girmek için kullanılan komutlar ve Hive komut konsolunda sırasıyla gönderilen Hive sorgusu vurgulayın. Yeşil kutunun Hive sorgusu çıktısını vurgular.

![Çalışma alanı oluştur](./media/move-hive-tables/run-hive-queries-2.png)

Önceki örneklerde doğrudan Hive sorgu sonuçları ekranda çıktı. Yerel bir dosyaya baş düğüm veya bir Azure blob çıkış yazabilirsiniz. Ardından, daha fazla Hive sorguları çıktısını analiz etmek için diğer araçları kullanabilirsiniz.

**Yerel bir dosyaya Hive sorgu sonuçları çıktı.**
Baş düğüm yerel bir dizine Hive sorgusu sonuçlarını çıkarmak için Hive sorgusu Hadoop komut satırında şu şekilde göndermek için gerekenler:

    hive -e "<hive query>" > <local path in the head node>

Aşağıdaki örnekte, Hive sorgusu çıktısını bir dosyaya yazılır `hivequeryoutput.txt` dizininde `C:\apps\temp`.

![Çalışma alanı oluştur](./media/move-hive-tables/output-hive-results-1.png)

**Bir Azure blob çıkış Hive sorgu sonuçları**

Hadoop kümesi varsayılan kapsayıcı içinde bir Azure blob Hive sorgu sonuçlarını da çıkarabilirsiniz. Bu Hive sorgusu aşağıdaki gibidir:

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

Aşağıdaki örnekte, bir blob dizinine Hive sorgusu çıktısını yazılır `queryoutputdir` Hadoop kümesi varsayılan kapsayıcı içinde. Burada, yalnızca blob adı olmadan dizin adı sağlamanız gerekir. Dizin ve blob adları gibi sağlarsanız, bir hata atılır `wasb:///queryoutputdir/queryoutput.txt`.

![Çalışma alanı oluştur](./media/move-hive-tables/output-hive-results-2.png)

Azure Depolama Gezgini'ni kullanarak Hadoop kümenin varsayılan kapsayıcı açarsanız, aşağıdaki çizimde gösterildiği gibi Hive sorgusu çıktısını görebilirsiniz. Yalnızca blob adlarındaki belirtilen harf ile almak için (kırmızı kutu ile vurgulanan) filtre uygulayabilirsiniz.

![Çalışma alanı oluştur](./media/move-hive-tables/output-hive-results-3.png)

### <a name="hive-editor"></a> 2. Hive düzenleyicisinde Hive sorguları gönderme
Bir URL biçiminde girerek (Düzenleyicisi Hive) sorgu konsolunu da kullanabilirsiniz *https://<Hadoop cluster name>.azurehdinsight.net/Home/HiveEditor* bir web tarayıcısı içine. Olması gerekir bu konsolu bakın oturum ve gereken şekilde, Hadoop küme kimlik.

### <a name="ps"></a> 3. Azure PowerShell komutlarıyla Hive sorguları gönderme
Hive sorguları göndermek için PowerShell de kullanabilirsiniz. Yönergeler için bkz: [gönderme Hive işleri PowerShell kullanarak](../../hdinsight/hadoop/apache-hadoop-use-hive-powershell.md).

## <a name="create-tables"></a>Hive veritabanı ve tablo oluşturma
Hive sorgularını paylaşılan [GitHub deposunu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) ve buradan yüklenebilir.

Bir Hive tablosu oluşturur Hive sorgusu aşağıdadır.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Takılır gereken alanlarının açıklamaları ve diğer yapılandırmalar şunlardır:

* **<database name>**: oluşturmak istediğiniz veritabanının adı. Varsayılan veritabanı sorgu kullanmak istiyorsanız, *veritabanı oluştur...*  atlanabilir.
* **<table name>**: Belirtilen veritabanı içinde oluşturmak istediğiniz tablonun adı. Varsayılan veritabanı kullanmak istiyorsanız, tablonun doğrudan göre başvurulabilen *<table name>* olmadan <database name>.
* **<field separator>**: Hive tablosu karşıya yüklenecek veri dosyasındaki alanlar sınırlandıran ayırıcı.
* **<line separator>**: veri dosyasındaki satır sınırlandıran ayırıcı.
* **<storage location>**: Hive tablolarını verileri kaydetmek için Azure depolama konumu. Belirtmezseniz, *konumu <storage location>* , veritabanını ve tabloları depolanır *hive/ambarı/* varsayılan olarak Hive kümenin varsayılan kapsayıcısında dizin. Depolama konumu belirtmek istiyorsanız, depolama konumu tablo ve veritabanı için varsayılan kapsayıcı içinde olması gerekir. Bu konum biçimi kümede varsayılan kapsayıcı göreli konumunu olarak adlandırılan gerekiyor *' wasb: / / / < 1 dizini > /'* veya *' wasb: / / / < 1 dizini > / < 2 dizini > /'*, vb. Sorgu yürütüldükten sonra göreli dizinleri varsayılan kapsayıcı içinde oluşturulur.
* **TBLPROPERTIES("Skip.header.Line.Count"="1")**: veri dosyası bir başlık satırı varsa, bu özellik eklemek zorunda **sonunda** , *tablo oluşturma* sorgu. Aksi takdirde, başlık satırı tablosuna bir kayıt olarak yüklenir. Veri dosyasındaki bir başlık satırı yok, bu yapılandırma Sorguda atlanabilir.

## <a name="load-data"></a>Hive tablolarını veri yükleme
Burada, Hive tabloya veri yükler Hive sorgusu verilmiştir.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

* **<path to blob data>**: Hive tablosu karşıya yüklenecek blob Hdınsight Hadoop kümesi varsayılan kapsayıcısında dosyasıysa *<path to blob data>* biçiminde olmalıdır *' wasb: / / /<directory in this container> / <blob file name>'*. Blob dosya de Hdınsight Hadoop kümesi ek bir kapsayıcı olabilir. Bu durumda, *<path to blob data>* biçiminde olmalıdır *' wasb: / /<container name><storage account name>.blob.core.windows.net/<blob file name>'*.

  > [!NOTE]
  > Hive tablosu yüklenebilmesi için blob verilerini varsayılan veya ek kapsayıcısına Hadoop kümesi için depolama hesabına ait olması gerekir. Aksi takdirde, *veri yükleme* sorgu başarısız şikayetçi verilere erişemez.
  >
  >

## <a name="partition-orc"></a>Gelişmiş konular: Tablo ve Depolama yığını verileri ORC biçiminde bölümlenmiş
Veri büyükse, tablo bölümleme yalnızca birkaç bölümlerini tablo taraması gereken sorguları için faydalıdır. Örneğin, bir web sitesinin günlük verilerini göre tarih bölümlemek uygun olur.

Hive tablolarını bölümleme yanı sıra en iyi duruma getirilmiş satır sütunlu (ORC) biçiminde Hive verileri depolamak faydalı olacaktır. ORC biçimlendirme daha fazla bilgi için bkz: <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">kullanarak ORC dosyaları Hive okuma, yazma ve verileri işlerken performansını artırır</a>.

### <a name="partitioned-table"></a>Bölümlenmiş bir tablo
Bölümlenmiş bir tablo oluşturur ve veri içine yükler Hive sorgusu aşağıdadır.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Bölümlenmiş tablolar sorgulanırken bölüm koşulu eklemek için önerilir **başına** , `where` yan tümcesi bu olarak önemli ölçüde arama sürecinin artırır.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Hive verileri ORC biçiminde depolayan
ORC biçiminde depolanan Hive tablolara blob depolama alanından verileri doğrudan yüklenemiyor. , Yüklemek için uygulamanız gereken adımlar şunlardır verileri Azure BLOB'ların ORC biçiminde depolanan Hive tablolara.

Bir dış tablo oluşturma **DEPOLANAN AS TEXTFILE** ve veri yükleme blob depolama alanından tabloya.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

Adım 1, aynı alan sınırlayıcı ile dış tabloda aynı şemasıyla bir iç tablosu oluşturun ve Hive verileri ORC biçiminde depolayan.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

1. adımda dış tablodan veri seçin ve ORC tablosuna Ekle

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

> [!NOTE]
> Varsa TEXTFILE tablo  *<database name>.<external textfile table name>* bölümler, adım 3'te içeren `SELECT * FROM <database name>.<external textfile table name>` komutu döndürülen veri kümesindeki bir alan olarak bölüm değişkeni seçer. İçine ekleme  *<database name>.<ORC table name>* bu yana başarısız  *<database name>.<ORC table name>* Bölüm değişkeni, tablo şemasında bir alan olarak yok. Bu durumda, özellikle için eklenecek alanları seçmeniz gerekir  *<database name>.<ORC table name>* aşağıdaki gibi:
>
>

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Bırakma güvenlidir *<external textfile table name>* olduğunda tüm veri sonra aşağıdaki sorguyu kullanarak ekledi içine *<database name>.<ORC table name>*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Bu yordamı tamamladıktan sonra kullanıma hazır ORC biçimindeki verileri içeren bir tablo olması gerekir.  
