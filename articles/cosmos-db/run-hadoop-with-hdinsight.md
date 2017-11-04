---
title: "Azure Cosmos DB ile Hdınsight Hadoop işini çalıştır | Microsoft Docs"
description: "Azure Cosmos DB ve Azure Hdınsight ile basit bir Hive, Pig ve MapReduce işi çalıştıran öğrenin."
services: cosmos-db
author: dennyglee
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 06f0ea9d-07cb-4593-a9c5-ab912b62ac42
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 06/08/2017
ms.author: denlee
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3c8789f08a37466862120dda88a0bce7da3e9a91
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="Azure Cosmos DB-HDInsight"></a>Azure Cosmos DB ve Hdınsight kullanarak bir Apache Hive, Pig veya Hadoop işini çalıştır
Bu öğretici nasıl çalıştırılacağını gösterir [Apache Hive][apache-hive], [Apache Pig][apache-pig], ve [Apache Hadoop] [ apache-hadoop] Cosmos veritabanı Hadoop Bağlayıcısı ile MapReduce işleri Azure hdınsight'ta. Cosmos veritabanı Hadoop Bağlayıcısı Cosmos hem kaynak hem de Hive, Pig ve MapReduce işleri için havuz olarak davranacak şekilde DB sağlar. Bu öğretici Cosmos DB Hadoop işleri veri kaynağı ve hedef kullanır.

Bu öğreticiyi tamamladıktan sonra aşağıdaki soruları yanıtlayın mümkün olacaktır:

* Cosmos Hive, Pig ve MapReduce işi kullanılarak DB'den nasıl veri yükleme?
* Cosmos Hive, Pig ve MapReduce işi kullanılarak DB'de nasıl veri depoluyor?

Biz Cosmos DB Hdınsight ile Hive işi çalıştırdığı aşağıdaki videoyu izleyerek çalışmaya başlamanızı öneririz.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Use-Azure-DocumentDB-Hadoop-Connector-with-Azure-HDInsight/player]
>
>

Ardından, bu makalede, burada nasıl Cosmos DB verilerinizde analytics işlerine çalıştırabilirsiniz tam Ayrıntılar alırsınız döndür.

> [!TIP]
> Bu öğretici, Apache Hadoop, Hive ve/veya Pig kullanma konusunda deneyim sahibi olduğunuzu varsayar. Apache Hadoop, Hive ve Pig yeniyseniz, ziyaret öneririz [Apache Hadoop belgeleri][apache-hadoop-doc]. Bu öğreticinin ayrıca Cosmos DB ile konusunda deneyim sahibi ve bir Cosmos DB hesabına sahip olduğunu varsayar. Cosmos DB yeni veya Cosmos DB hesap yok, lütfen kullanıma bizim [Başlarken] [ getting-started] sayfası.
>
>

Yoksa öğreticiyi tamamlamak için saat ve Hive, Pig ve MapReduce için tam örnek PowerShell komut dosyalarını almak istediğiniz? Bir sorun bunları getirmek [burada][hdinsight-samples]. İndirme Ayrıca bu örnekleri için hql, pig ve java dosyaları içerir.

## <a name="NewestVersion"></a>En yeni sürümü
<table border='1'>
    <tr><th>Hadoop Bağlayıcısı sürüm</th>
        <td>1.2.0</td></tr>
    <tr><th>Betik URI'si</th>
        <td>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-installer-v04.ps1</td></tr>
    <tr><th>Değiştirilme tarihi</th>
        <td>04/26/2016</td></tr>
    <tr><th>Desteklenen Hdınsight sürümleri</th>
        <td>3.1, 3.2</td></tr>
    <tr><th>Değişiklik günlüğü</th>
        <td>Güncelleştirilmiş Azure Cosmos DB Java SDK'sı 1.6.0 için</br>
            Bir kaynak ve havuz olarak bölümlenmiş koleksiyonlar için destek eklendi</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Önkoşullar
Bu öğreticide yönergeleri izlemeden önce aşağıdakileri yapın:

* Cosmos DB hesabı, bir veritabanı ve belgeleri içinde ile bir koleksiyon. Daha fazla bilgi için bkz: [Cosmos DB ile çalışmaya başlama][getting-started]. Cosmos DB hesabınızla örnek veri aktarmak [Cosmos DB içeri aktarma aracını][import-data].
* Üretilen iş. Okuma ve yazma hdınsight'tan sayılır, Koleksiyonlarınız için ayrılan isteği birimlerinizi bulunun.
* Ek bir saklı yordam içinde her kapasiteyi koleksiyonu çıktı. Saklı yordamlar, sonuçta elde edilen belgeler aktarmak için kullanılır.
* Hive, Pig ve MapReduce işleri elde edilen belgelerden kapasitesi.
* [*İsteğe bağlı*] kapasite için ek bir koleksiyonu.

> [!WARNING]
> İşleri hiçbirini sırasında yeni bir koleksiyon oluşturulmasını önlemek için stdout sonuçları yazdırma, WASB kapsayıcıya çıkış kaydedin ve zaten mevcut bir koleksiyonu belirtin. Mevcut bir koleksiyonu belirtme söz konusu olduğunda, yeni belgeler koleksiyon içinde oluşturulur ve zaten var olan belgeler, yalnızca bir çakışma varsa etkilenir *kimlikleri*. **Bağlayıcı kimliği çakışıyor varolan belgeleri otomatik olarak kılacak**. Upsert seçeneği false olarak ayarlayarak bu özelliği devre dışı bırakabilir. Upsert false ise ve bir çakışma oluşur, Hadoop işi başarısız olur; bir kimliği çakışma hata raporlama.
>
>

## <a name="ProvisionHDInsight"></a>1. adım: yeni bir Hdınsight kümesi oluşturma
Bu öğretici Azure Portal'dan betik eylemi Hdınsight kümenize özelleştirmek için kullanır. Bu öğreticide, Hdınsight kümesi oluşturmak için Azure Portalı'nı kullanacağız. PowerShell cmdlet'lerini veya Hdınsight .NET SDK'sını kullanma hakkında daha fazla yönerge için kullanıma [özelleştirme Hdınsight kümeleri betik eylemi kullanarak] [ hdinsight-custom-provision] makalesi.

1. Oturum [Azure Portal][azure-portal].
2. Tıklatın **+ yeni** sol gezinti üst kısmındaki arama **Hdınsight** yeni bir dikey pencere en üstteki arama çubuğunda.
3. **Hdınsight** tarafından yayımlanan **Microsoft** sonuçları üstünde görünür. Tıklayın ve ardından **oluşturma**.
4. Yeni Hdınsight kümesinde oluştur dikey penceresi, girin, **küme adı** seçip **abonelik** altında bu kaynak sağlamak istiyorsunuz.

    <table border='1'>
        <tr><td>Küme adı</td><td>Küme adı.<br/>
DNS adı olmalıdır başlangıç ve bitiş bir alfasayısal karakter ile ve kısa çizgi içerebilir.<br/>
Alan 3 ile 63 karakter arasında bir dize olmalıdır.</td></tr>
        <tr><td>Abonelik adı</td>
            <td>Birden fazla Azure aboneliğiniz varsa, Hdınsight kümenize barındıracak aboneliği seçin. </td></tr>
    </table>
5.Tıklatın **küme türü seçin** ve aşağıdaki özellikleri belirtilen değerlere ayarlayın.

    <table border='1'>
        <tr><td>Küme türü</td><td><strong>Hadoop</strong></td></tr>
        <tr><td>Küme katmanı</td><td><strong>Standart</strong></td></tr>
        <tr><td>İşletim Sistemi</td><td><strong>Windows</strong></td></tr>
        <tr><td>Sürüm</td><td>En son sürümü</td></tr>
    </table>

    Şimdi, **seçin**.

    ![Hadoop Hdınsight ilk küme ayrıntılarını sağlayın][image-customprovision-page1]
6. Tıklayın **kimlik bilgileri** oturum açma ve Uzaktan erişim kimlik bilgilerini ayarlamak için. Seçin, **küme oturum açma kullanıcı** ve **küme oturum açma parolası**.

    Kümenizi uzaktan istiyorsanız seçin *Evet* dikey pencerenin altındaki bir kullanıcı adı ve parolasını girin.
7. Tıklayın **veri kaynağı** birincil konumunuz veri erişimi için ayarlanacak. Seçin **seçim yöntemini** ve zaten varolan bir depolama hesabını belirtin veya yeni bir tane oluşturun.
8. Aynı dikey penceresinde belirtin bir **varsayılan kapsayıcı** ve **konumu**. Ve tıklayın **seçin**.

   > [!NOTE]
   > Cosmos DB hesap bölgeniz daha iyi performans için yakın bir konum seçin
   >
   >
9. Tıklayın **fiyatlandırma** sayısını ve düğümlerinin türünü seçin. Varsayılan yapılandırmayı korumak ve daha sonra çalışan düğüm sayısı ölçeklendirin.
10. Tıklatın **isteğe bağlı yapılandırma**, ardından **betik eylemleri** isteğe bağlı yapılandırma dikey penceresinde.

     Betik eylemleri Hdınsight kümenize özelleştirmek için aşağıdaki bilgileri girin.

     <table border='1'>
         <tr><th>Özellik</th><th>Değer</th></tr>
         <tr><td>Ad</td>
             <td>Betik eylemi için bir ad belirtin.</td></tr>
         <tr><td>Betik URI'si</td>
             <td>Küme özelleştirmek için çağrılan betik URI'si belirtin.</br></br>
Lütfen girin: </br> <strong>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</td></tr>
         <tr><td>Baş</td>
             <td>Baş düğüm PowerShell betiğini çalıştırmak için onay kutusuna tıklayın.</br></br>
             <strong>Bu onay kutusunu işaretleyin</strong>.</td></tr>
         <tr><td>Çalışan</td>
             <td>Çalışan düğümüne PowerShell betiğini çalıştırmak için onay kutusuna tıklayın.</br></br>
             <strong>Bu onay kutusunu işaretleyin</strong>.</td></tr>
         <tr><td>Zookeeper</td>
             <td>Zookeeper PowerShell betiğini çalıştırmak için onay kutusuna tıklayın.</br></br>
             <strong>Gerekli</strong>.
             </td></tr>
         <tr><td>Parametreler</td>
             <td>Komut dosyası tarafından gerekli parametreleri belirtin.</br></br>
             <strong>Gerekli parametre</strong>.</td></tr>
     </table>
11.Ya da oluşturma yeni bir **kaynak grubu** veya varolan bir kaynak grubunu Azure aboneliğinizdeki kullanın.
12. Şimdi denetleyin **panoya Sabitle** kendi dağıtımını izlemeye tıklatıp **oluşturma**!

## <a name="InstallCmdlets"></a>Azure PowerShell'i 2. adım: Yükleme ve yapılandırma
1. Azure PowerShell'i yükleyin. Yönergeler için bkz [burada][powershell-install-configure].

   > [!NOTE]
   > Alternatif olarak, Hive sorguları olduğu için Hdınsight'ın çevrimiçi Hive Düzenleyicisi'ni kullanabilirsiniz. Bunu yapmak için oturumu [Azure Portal][azure-portal], tıklatın **Hdınsight** Hdınsight kümelerinizi listesini görüntülemek için sol bölmede. Hive sorgularını çalıştırmak ve ardından istediğiniz kümeyi tıklatın **sorgu konsol**.
   >
   >
2. Azure PowerShell Tümleşik komut dosyası ortamı açın:

   * Windows 8 veya Windows Server 2012 veya sonraki sürümlerini çalıştıran bir bilgisayarda, yerleşik aramayı kullanabilirsiniz. Başlangıç ekranından yazın **powershell ISE** tıklatıp **Enter**.
   * Windows 8 veya Windows Server 2012'den önceki bir sürümünü çalıştıran bir bilgisayarda Başlat menüsünü kullanın. Başlat menüsünden yazın **komut istemi** arama kutusuna ardından sonuçlar listesinde tıklatın **komut istemi**. Komut istemine yazın **powershell_ise** tıklatıp **Enter**.
3. Azure hesabınızda ekleyin.

   1. Konsol bölmesinde yazın **Add-AzureAccount** tıklatıp **Enter**.
   2. Azure aboneliğinizle ilişkili e-posta adresini yazın ve tıklatın **devam**.
   3. Azure aboneliğiniz için parolayı yazın.
   4. Tıklatın **oturum**.
4. Aşağıdaki diyagramda, Azure PowerShell komut dosyası ortamı önemli bölümleri tanımlar.

    ![Azure PowerShell diyagramı][azure-powershell-diagram]

## <a name="RunHive"></a>3. adım: Cosmos DB Hdınsight ile Hive işi çalıştırma
> [!IMPORTANT]
> Yapılandırma ayarlarınızı kullanarak < > tarafından belirtilen tüm değişkenleri doldurulması gerekir.
>
>

1. PowerShell betik bölmesinde aşağıdaki değişkenleri ayarlayın.

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"
2. <p>Sorgu dizesi oluşturma başlayalım. Biz tüm belgeleri sistem tarafından oluşturulan zaman damgaları (_ts) ve bir Azure Cosmos DB koleksiyonundan benzersiz kimlikler (_rid) alır, tüm belgeleri dakikaya göre hesaplar ve ardından sonuçları geri yeni bir Azure Cosmos DB koleksiyona depolayan bir Hive sorgusu yazacaksınız.</p>

    <p>İlk olarak, bir Hive tablosu bizim Azure Cosmos DB koleksiyonundan oluşturalım. Aşağıdaki kod parçacığını PowerShell betik bölmesine eklemek <strong>sonra</strong> kod parçacığı # 1. Bizim belgelere yalnızca _ts isteğe bağlı DocumentDB.query t parametresi kırpma eklediğinizden emin olun ve _rid.</p>

   > [!NOTE]
   > **DocumentDB.inputCollections adlandırma bir hata oldu.** Evet, bir giriş olarak birden çok koleksiyon ekleme ver: </br>
   >
   >

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> The collection names are separated without spaces, using only a single comma.

        # Create a Hive table using data from DocumentDB. Pass DocumentDB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

1. Ardından, çıktı koleksiyon için bir Hive tablosu oluşturalım. Çıktı belge özellikleri, ay, gün, saat, dakika ve yineleme toplam sayısı olacaktır.

   > [!NOTE]
   > **Henüz bir yeniden adlandırma DocumentDB.outputCollections bir hata oldu.** Evet, çıkış olarak birden çok koleksiyon ekleme ver: </br>
   > '*DocumentDB.outputCollections*'='*\<DocumentDB çıkış koleksiyon adı 1\>*,*\<DocumentDB çıkış koleksiyon adı 2\>* ' </br> Koleksiyon adları, boşluk, yalnızca tek bir virgül kullanarak ayrılır. </br></br>
   > Belgeler arasında birden çok koleksiyon dağıtılmış hepsini olacaktır. Belgeleri toplu bir koleksiyonda depolanan ve ardından belgeleri ikinci toplu sonraki toplanması ve benzeri içinde depolanır.
   >
   >

       # Create a Hive table for the output data to DocumentDB.
       $queryStringPart2 = "drop table DocumentDB_analytics; " +
                             "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                             "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                             "tblproperties ( " +
                                 "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                 "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                 "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                 "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "
2. Son olarak, şirketinizdeki belgeleri ay, gün, saat ve dakika kaydetmesini ve sonuçları çıktısına dönüştüren eklemek Hive tablosu.

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "
3. Önceki sorgudan bir Hive işi tanımı oluşturmak için aşağıdaki kod parçacığını ekleyin.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    Aynı zamanda HDFS üzerinde HiveQL komut dosyasını belirtmek için dosyası anahtarı.
4. Başlangıç saati kaydetmek ve Hive işi göndermek için aşağıdaki kod parçacığını ekleyin.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition
5. Hive işi tamamlanmasını beklemek için aşağıdakileri ekleyin.

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600
6. Standart çıktı ve başlangıç ve bitiş zamanlarını yazdırmak için aşağıdakileri ekleyin.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
7. **Çalıştırma** yeni kodunuzu! **Tıklatın** yeşil YÜRÜT düğmesine.
8. Sonuçları denetleyin. Oturum [Azure Portal][azure-portal].

   1. Tıklatın <strong>Gözat</strong> sol taraftaki paneldeki. </br>
   2. Tıklatın <strong>her şeyi</strong> Gözat bölmenin sağ üst adresindeki. </br>
   3. Bulun ve tıklatın <strong>Azure Cosmos DB hesapları</strong>. </br>
   4. Ardından, bulma, <strong>Azure Cosmos DB hesabı</strong>, ardından <strong>Azure Cosmos DB veritabanı</strong> ve <strong>Azure Cosmos DB koleksiyonu</strong> belirtilen çıkış derlemesiyle ilişkilendirilmiş, Hive sorgusu.</br>
   5. Son olarak, tıklatın <strong>belge Gezgini</strong> altında <strong>Geliştirici Araçları</strong>.</br></p>

   Hive Sorgunuzun sonuçlarını görürsünüz.

   ![Hive sorgusu sonuçları][image-hive-query-results]

## <a name="RunPig"></a>Adım 4: Cosmos DB Hdınsight ile Pig işi çalıştırma
> [!IMPORTANT]
> Yapılandırma ayarlarınızı kullanarak < > tarafından belirtilen tüm değişkenleri doldurulması gerekir.
>
>

1. PowerShell betik bölmesinde aşağıdaki değişkenleri ayarlayın.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"
2. <p>Sorgu dizesi oluşturma başlayalım. Biz tüm belgeleri sistem tarafından oluşturulan zaman damgaları (_ts) ve bir Azure Cosmos DB koleksiyonundan benzersiz kimlikler (_rid) alır, tüm belgeleri dakikaya göre hesaplar ve ardından sonuçları geri yeni bir Azure Cosmos DB koleksiyona depolayan bir Pig sorgu yazacaksınız.</p>
    <p>İlk olarak, belge Hdınsight'a Cosmos DB'den yükleme. Aşağıdaki kod parçacığını PowerShell betik bölmesine eklemek <strong>sonra</strong> kod parçacığı # 1. Bizim belgelere yalnızca _ts kırpma için isteğe bağlı DocumentDB sorgu parametresi için bir DocumentDB sorgu eklediğinizden emin olun ve _rid.</p>

   > [!NOTE]
   > Evet, bir giriş olarak birden çok koleksiyon ekleme ver: </br>
   > '*\<DocumentDB giriş koleksiyon adı 1\>*,*\<DocumentDB giriş koleksiyon adı 2\>*'</br> Koleksiyon adları, boşluk, yalnızca tek bir virgül kullanarak ayrılır. </b>
   >
   >

    Belgeler arasında birden çok koleksiyon dağıtılmış hepsini olacaktır. Belgeleri toplu bir koleksiyonda depolanan ve ardından belgeleri ikinci toplu sonraki toplanması ve benzeri içinde depolanır.

        # Load data from Cosmos DB. Pass DocumentDB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "
3. Ardından, ay, gün, saat, dakika ve yineleme toplam sayısı tarafından belgeleri şimdi kaydetmesini.

       # GROUP BY minute and COUNT entries for each.
       $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                           "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                           "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "
4. Son olarak, şirketinizdeki sonuçları bizim yeni çıkış koleksiyona depolar.

   > [!NOTE]
   > Evet, çıkış olarak birden çok koleksiyon ekleme ver: </br>
   > '\<DocumentDB çıkış koleksiyon adı 1\>,\<DocumentDB çıkış koleksiyon adı 2\>'</br> Koleksiyon adları, boşluk, yalnızca tek bir virgül kullanarak ayrılır.</br>
   > Belgeler arasında birden çok koleksiyon dağıtılmış hepsini olacaktır. Belgeleri toplu bir koleksiyonda depolanan ve ardından belgeleri ikinci toplu sonraki toplanması ve benzeri içinde depolanır.
   >
   >

        # Store output data to Cosmos DB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "
5. Önceki sorgudan bir Pig proje tanımı oluşturmak için aşağıdaki kod parçacığını ekleyin.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    Aynı zamanda HDFS üzerinde Pig betiği belirtmek için dosyası anahtarı.
6. Başlangıç saati kaydetmek ve Pig işi göndermek için aşağıdaki kod parçacığını ekleyin.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition
7. Pig işi tamamlamak beklenecek aşağıdakileri ekleyin.

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600
8. Standart çıktı ve başlangıç ve bitiş zamanlarını yazdırmak için aşağıdakileri ekleyin.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
9. **Çalıştırma** yeni kodunuzu! **Tıklatın** yeşil YÜRÜT düğmesine.
10. Sonuçları denetleyin. Oturum [Azure Portal][azure-portal].

    1. Tıklatın <strong>Gözat</strong> sol taraftaki paneldeki. </br>
    2. Tıklatın <strong>her şeyi</strong> Gözat bölmenin sağ üst adresindeki. </br>
    3. Bulun ve tıklatın <strong>Azure Cosmos DB hesapları</strong>. </br>
    4. Ardından, bulma, <strong>Azure Cosmos DB hesabı</strong>, ardından <strong>Azure Cosmos DB veritabanı</strong> ve <strong>Azure Cosmos DB koleksiyonu</strong> belirtilen çıkış derlemesiyle ilişkilendirilmiş, Pig sorgu.</br>
    5. Son olarak, tıklatın <strong>belge Gezgini</strong> altında <strong>Geliştirici Araçları</strong>.</br></p>

    Pig Sorgunuzun sonuçlarını görürsünüz.

    ![Pig sorgu sonuçları][image-pig-query-results]

## <a name="RunMapReduce"></a>5. adım: Azure Cosmos DB Hdınsight ile MapReduce işi çalıştırma
1. PowerShell betik bölmesinde aşağıdaki değişkenleri ayarlayın.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name
2. Biz, Azure Cosmos DB koleksiyondaki her bir belge özellik için yineleme sayısı hesaplayan bir MapReduce işi yürüteceksiniz. Bu kod parçacığını ekleyin **sonra** Yukarıdaki kod parçacığında.

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

   > [!NOTE]
   > TallyProperties v01.jar Cosmos DB Hadoop bağlayıcı özel yüklenmesiyle birlikte gelir.
   >
   >
3. MapReduce işi göndermek için aşağıdaki komutu ekleyin.

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    MapReduce işi tanım yanı sıra, aynı zamanda MapReduce işi ve kimlik bilgileri çalıştırmak istediğiniz Hdınsight küme adı sağlayın. Başlangıç AzureHDInsightJob zaman bir çağrıdır. İş tamamlandığında denetlemek için kullanın *bekleme AzureHDInsightJob* cmdlet'i.
4. MapReduce işi çalıştıran ile hataları denetlemek için aşağıdaki komutu ekleyin.

        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
5. **Çalıştırma** yeni kodunuzu! **Tıklatın** yeşil YÜRÜT düğmesine.
6. Sonuçları denetleyin. Oturum [Azure Portal][azure-portal].

   1. Tıklatın <strong>Gözat</strong> sol taraftaki paneldeki.
   2. Tıklatın <strong>her şeyi</strong> Gözat bölmenin sağ üst adresindeki.
   3. Bulun ve tıklatın <strong>Azure Cosmos DB hesapları</strong>.
   4. Ardından, bulma, <strong>Azure Cosmos DB hesabı</strong>, ardından <strong>Azure Cosmos DB veritabanı</strong> ve <strong>Azure Cosmos DB koleksiyonu</strong> belirtilen çıkış derlemesiyle ilişkilendirilmiş, MapReduce işi.
   5. Son olarak, tıklatın <strong>belge Gezgini</strong> altında <strong>Geliştirici Araçları</strong>.

      MapReduce işinizin sonuçları görürsünüz.

      ![MapReduce sorgu sonuçları][image-mapreduce-query-results]

## <a name="NextSteps"></a>Sonraki Adımlar
Tebrikler! Yalnızca Azure Cosmos DB ve Hdınsight kullanarak, ilk Hive, Pig ve MapReduce işleri çalıştırdığınız.

Bizim açık kaynaklıdır bizim Hadoop bağlayıcı. İlginizi çekiyorsa üzerinde katkıda bulunabilirsiniz [GitHub][github].

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Documentdb ile bir Java uygulaması geliştirme][documentdb-java-application]
* [Hdınsight'ta Hadoop için Java MapReduce programlar geliştirmek][hdinsight-develop-deploy-java-mapreduce]
* [Hadoop ile hdınsight'ta Hive mobil ahize kullanımını çözümleme için kullanmaya başlama][hdinsight-get-started]
* [Hdınsight ile MapReduce kullanma][hdinsight-use-mapreduce]
* [HDInsight ile Hive kullanma][hdinsight-use-hive]
* [HDInsight ile Pig kullanma][hdinsight-use-pig]
* [Betik eylemi kullanarak Hdınsight kümelerini özelleştirme][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[import-data]: import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md
[hdinsight-develop-deploy-java-mapreduce]:../hdinsight/hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]:../hdinsight/hadoop/hdinsight-use-hive.md
[hdinsight-use-mapreduce]:../hdinsight/hadoop/hdinsight-use-mapreduce.md
[hdinsight-use-pig]:../hdinsight/hadoop/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0
