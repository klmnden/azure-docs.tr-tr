---
title: HDInsight, Apache Hadoop işleri için veri yükleme
description: Klasik Azure CLI, Azure Depolama Gezgini, Azure PowerShell, Hadoop komut satırı veya Sqoop kullanarak HDInsight, Apache Hadoop işleri için erişim verileri ve nasıl yükleneceğini öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdiseo17may2017
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 0dbd5a886e2369d29a568eca47dda5558f43c8cd
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479145"
---
# <a name="upload-data-for-apache-hadoop-jobs-in-hdinsight"></a>HDInsight, Apache Hadoop işleri için veri yükleme

Azure HDInsight, Azure depolama ve Azure Data Lake Storage (Gen1 ve 2. nesil) üzerinden bir tam özellikli Hadoop dağıtılmış dosya sistemi (HDFS) sağlar. Azure depolama ve Data Lake depolama Gen1 ve 2. nesil HDFS uzantıları müşterilere sorunsuz bir deneyim sağlamak için tasarlanmıştır. Bunlar Hadoop ekosistemindeki doğrudan yönettiği veriler üzerinde çalışılacak bileşenler kümesinin etkinleştirin. Azure depolama, Data Lake depolama Gen1 ve 2. nesil depolama verilerinin ve bu verileri hesaplamaları için optimize edilmiş farklı dosya sistemleridir. Azure depolama kullanmanın avantajları hakkında bilgi için [HDInsight ile Azure depolama kullanma](hdinsight-hadoop-use-blob-storage.md), [kullanım Data Lake depolama Gen1 HDInsight ile](hdinsight-hadoop-use-data-lake-store.md), ve [kullanım Data Lake depolama 2. nesil ile HDInsight](hdinsight-hadoop-use-data-lake-storage-gen2.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki gereksinimleri dikkate alın:

* Bir Azure HDInsight kümesi. Yönergeler için [Azure HDInsight ile çalışmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md) veya [oluşturma HDInsight kümeleri](hdinsight-hadoop-provision-linux-clusters.md).
* Aşağıdaki makalelerdeki bilgi:

    - [HDInsight ile Azure Depolama'yı kullanma](hdinsight-hadoop-use-blob-storage.md)
    - [Data Lake depolama Gen1 HDInsight ile kullanma](hdinsight-hadoop-use-data-lake-store.md)
    - [Data Lake depolama Gen2 HDInsight ile kullanma](hdinsight-hadoop-use-data-lake-storage-gen2.md)  

## <a name="upload-data-to-azure-storage"></a>Verileri Azure Depolama'ya yükleme

## <a name="utilities"></a>Altyapı Hizmetleri
Microsoft Azure depolama ile çalışmak için aşağıdaki yardımcı programlarını sağlar:

| Aracı | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure portal](../storage/blobs/storage-quickstart-blobs-portal.md) |✔ |✔ |✔ |
| [Azure CLI](../storage/blobs/storage-quickstart-blobs-cli.md) |✔ |✔ |✔ |
| [Azure PowerShell](../storage/blobs/storage-quickstart-blobs-powershell.md) | | |✔ |
| [AzCopy](../storage/common/storage-use-azcopy-v10.md) |✔ | |✔ |
| [Hadoop komutu](#commandline) |✔ |✔ |✔ |


> [!NOTE]  
> Hadoop komutu yalnızca HDInsight kümesi üzerinde kullanılabilir. Komutu, yalnızca verileri yerel dosya sisteminden Azure Depolama'ya yükleme sağlar.  


## <a id="commandline"></a>Hadoop komut satırı
Hadoop komut satırı, yalnızca veri kümesi baş düğümünde mevcut olduğunda, verileri Azure depolama blobu depolamak için yararlıdır.

Hadoop komutu kullanabilmek için önce baş kullanarak bağlanmalısınız [SSH veya PuTTY](hdinsight-hadoop-linux-use-ssh-unix.md).

Bağlantı kurulduktan sonra dosya depolama alanına yüklemek için aşağıdaki söz dizimini kullanabilirsiniz.

```bash
hadoop -copyFromLocal <localFilePath> <storageFilePath>
```

Örneğin, `hadoop fs -copyFromLocal data.txt /example/data/data.txt`

HDInsight için varsayılan dosya sistemi Azure Storage'da /example/data.txt gerçekte Azure Depolama'da olduğu. Dosyayı farklı de başvurabilirsiniz:

    wasbs:///example/data/data.txt

or

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Dosyalarla diğer Hadoop komutlarını bir listesi için bkz. [https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]  
> Apache HBase kümelerinde veri yazma 256 KB olduğunda kullanılan boyutu varsayılan engelleyin. Bu HBase API'lerini veya REST API'leri kullanırken düzgün çalışır durumdayken `hadoop` veya `hdfs dfs` ~ 12 GB'tan büyük veri hatayla sonuçlanır yazmak için komutları. Daha fazla bilgi için bkz. [blob yazma için depolama özel durumu](#storageexception) bu makaledeki bir bölüm.

## <a name="graphical-clients"></a>Grafik istemciler
Azure depolama ile çalışmak için bir grafik arabirim sağlayan çeşitli uygulamalar vardır. Aşağıdaki tabloda, birkaç bu uygulamaların bir listesi verilmiştir:

| İstemci | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [HDInsight için Microsoft Visual Studio Araçları](hadoop/apache-hadoop-visual-studio-tools-get-started.md#explore-linked-resources) |✔ |✔ |✔ |
| [Azure Depolama Gezgini](../storage/blobs/storage-quickstart-blobs-storage-explorer.md) |✔ |✔ |✔ |
| [Cerulea](https://www.cerebrata.com/products/cerulean/features/azure-storage) | | |✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Microsoft Azure için cloudBerry Gezgini](https://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |


## <a name="mount-azure-storage-as-local-drive"></a>Azure depolama yerel sürücü olarak takmak
Bkz: [yerel sürücü olarak bağlama Azure Depolama'ya](https://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

## <a name="upload-using-services"></a>Hizmetleri kullanarak karşıya yükle
### <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory hizmeti, kolaylaştırılmış, ölçeklenebilir ve güvenilir veri üretim komut zincirlerinde veri depolama, veri işleme ve veri taşıma hizmetleri oluşturmak için tam olarak yönetilen bir hizmettir.

|Depolama türü|Belgeler|
|----|----|
|Azure Blob depolama|[Azure Data Factory kullanarak veya Azure Blob depolamadan/depolamaya veri kopyalayın](../data-factory/connector-azure-blob-storage.md)|
|Azure Data Lake Storage Gen1|[Azure Data Factory kullanarak Azure Data Lake depolama Gen1 gelen veya veri kopyalama](../data-factory/connector-azure-data-lake-store.md)|
|Azure Data Lake Storage Gen2 |[Azure Data Lake depolama Gen2 Azure Data Factory ile veri yükleme](../data-factory/load-azure-data-lake-storage-gen2.md)|

### <a id="sqoop"></a>Apache Sqoop
Sqoop, Hadoop ve ilişkisel veritabanları arasında veri aktarmak için tasarlanmış bir araçtır. SQL Server, MySQL veya Oracle Hadoop dağıtılmış dosya sistemi (HDFS) ile Hadoop MapReduce veya Hive ile verileri dönüştürün ve ardından bir RDBMS'de geri verileri dışarı aktarma gibi bir ilişkisel veritabanı yönetim sistemi (RDBMS), verileri içeri aktarmak için kullanabilirsiniz.

Daha fazla bilgi için [HDInsight ile Sqoop kullanma](hadoop/hdinsight-use-sqoop.md).

### <a name="development-sdks"></a>Geliştirme SDK'ları
Azure depolama, bir Azure SDK'sından aşağıdaki programlama dilleri kullanılarak da erişilebilir:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Azure SDK'ları yükleme hakkında daha fazla bilgi için bkz. [Azure indirmeleri](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Sorun giderme
### <a id="storageexception"></a>Blob yazma için depolama özel durumu
**Belirtiler**: Kullanırken `hadoop` veya `hdfs dfs` ~ 12 GB olan dosyaları yazmak için komutları daha büyük bir HBase kümesi, aşağıdaki hatalardan biriyle karşılaşabilirsiniz:

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

**Neden**: Bir blok boyutu 256 KB Azure depolama alanına yazarken varsayılan HDInsight üzerinde HBase kümeleri. HBase API'lerini veya REST API'leri için çalışırken, bir hata kullanırken sonuç `hadoop` veya `hdfs dfs` komut satırı yardımcı programları.

**Çözüm**: Kullanım `fs.azure.write.request.size` daha büyük bir blok boyutunu belirtin. Kullanım başına temelinde kullanarak bunu yapabilirsiniz `-D` parametresi. Aşağıdaki komutu kullanarak bu parametre ile bir örnektir `hadoop` komutu:

```bash
hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data
```

Ayrıca değerini artırabilirsiniz `fs.azure.write.request.size` Apache Ambari kullanarak Global olarak. Aşağıdaki adımlar, Ambari Web kullanıcı arabiriminde değeri değiştirmek için kullanılabilir:

1. Tarayıcınızda, kümeniz için Ambari Web kullanıcı arabirimini gidin. Bu `https://CLUSTERNAME.azurehdinsight.net`burada `CLUSTERNAME` kümenizin adıdır.

    İstendiğinde, küme için yönetim adını ve parolasını girin.
2. Ekranın sol tarafından seçin **HDFS**ve ardından **yapılandırmaları** sekmesi.
3. İçinde **Filtresi...**  alanına `fs.azure.write.request.size`. Bu alanı ve sayfanın ortasındaki geçerli değeri görüntüler.
4. Değer 262144 (256 KB) yeni değerle değiştirin. Örneğin, 4194304 (4 MB).

    ![Ambari Web kullanıcı Arabirimi aracılığıyla değerini değiştirme görüntüsü](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Ambari kullanarak daha fazla bilgi için bkz: [yönetme HDInsight kümeleri Apache Ambari Web kullanıcı arabirimini kullanarak](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Sonraki adımlar
HDInsight ile verileri alma anladığınıza göre Analiz gerçekleştirme hakkında bilgi edinmek için bu makaleleri okuyun:

* [Azure HDInsight ile çalışmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Program aracılığıyla Apache Hadoop işlerini gönderme](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [Apache Hive, HDInsight ile kullanma](hadoop/hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](hadoop/hdinsight-use-pig.md)
