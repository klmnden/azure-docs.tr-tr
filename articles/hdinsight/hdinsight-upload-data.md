---
title: HDInsight'ta Hadoop işleri için veri yükleme
description: Karşıya yükleme ve Azure Klasik CLI, Azure Depolama Gezgini, Azure PowerShell, Hadoop komut satırı veya Sqoop kullanarak bir HDInsight Hadoop işleri için veri erişim hakkında bilgi edinin.
keywords: etl hadoop, hadoop, hadoop veri yükleme ile veri alma
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.author: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/14/2018
ms.openlocfilehash: 44aaccee436011bd7d27bec87515fde0e898732e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46985988"
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>HDInsight'ta Hadoop işleri için veri yükleme

Azure HDInsight, Azure depolama ve Azure Data Lake Store üzerinde tam özellikli Hadoop dağıtılmış dosya sistemi (HDFS) sağlar. Azure depolama ve Data lake Store HDFS uzantı olarak müşterilere sorunsuz bir deneyim sağlamak için tasarlanmıştır. Bunlar Hadoop ekosistemindeki doğrudan yönettiği veriler üzerinde çalışılacak bileşenler kümesinin etkinleştirin. Azure depolama ve Data Lake Store depolama verilerinin ve bu verileri hesaplamaları için optimize edilmiş farklı dosya sistemleridir. Azure depolama kullanmanın avantajları hakkında bilgi için [HDInsight ile Azure depolama kullanma] [ hdinsight-storage] ve [kullanım Data Lake Store ile HDInsight](hdinsight-hadoop-use-data-lake-store.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki gereksinimleri dikkate alın:

* Bir Azure HDInsight kümesi. Yönergeler için [Azure HDInsight ile çalışmaya başlama] [ hdinsight-get-started] veya [oluşturma HDInsight kümeleri](hdinsight-hadoop-provision-linux-clusters.md).
* Aşağıdaki iki makalelerdeki bilgi:

    - [HDInsight ile Azure Depolama'yı kullanma][hdinsight-storage]
    - [Kullanım Data Lake Store ile HDInsight](hdinsight-hadoop-use-data-lake-store.md)

## <a name="upload-data-to-azure-storage"></a>Verileri Azure Depolama'ya yükleme

### <a name="command-line-utilities"></a>Komut satırı yardımcı programları
Microsoft Azure depolama ile çalışmak için aşağıdaki yardımcı programlarını sağlar:

| Araç | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Klasik Azure CLI][azurecli] |✔ |✔ |✔ |
| [Azure PowerShell][azure-powershell] | | |✔ |
| [AzCopy][azure-azcopy] |✔ | |✔ |
| [Hadoop komutu](#commandline) |✔ |✔ |✔ |

> [!NOTE]
> Hadoop komutu, yalnızca klasik Azure CLI, Azure PowerShell ve AzCopy tüm Azure dışından kullanılabilse de HDInsight kümesinde kullanılabilir. Ve verileri yerel dosya sisteminden Azure Depolama'ya yükleme komutu yalnızca izin verir.
>
>

#### <a id="xplatcli"></a>Klasik Azure CLI
Klasik Azure CLI, Azure hizmetlerini yönetmenize olanak tanıyan bir platformlar arası araçtır. Azure Depolama'ya veri yüklemek için aşağıdaki adımları kullanın:

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

1. [Yükleme ve yapılandırma Mac, Linux ve Windows için Azure Klasik CLI](../cli-install-nodejs.md).
2. Bir komut istemi, bash ya da diğer kabuğunu açın ve Azure aboneliğinize kimliğini doğrulamak için aşağıdakileri kullanın.

    ```cli
    azure login
    ```

    İstendiğinde, aboneliğiniz için kullanıcı adını ve parolasını girin.
3. Aboneliğiniz için depolama hesaplarını listelemek için aşağıdaki komutu girin:

    ```cli
    azure storage account list
    ```

4. Birlikte çalışmak istediğiniz blob içeren depolama hesabını seçin ve ardından bu hesabı için anahtarı almak için aşağıdaki komutu kullanın:

    ```cli
    azure storage account keys list <storage-account-name>
    ```

    Bu komut döndürür **birincil** ve **ikincil** anahtarları. Kopyalama **birincil** anahtar değerini sonraki adımda kullanılacağından.
5. Depolama hesabında blob kapsayıcıları listesini almak için aşağıdaki komutu kullanın:

    ```cli
    azure storage container list -a <storage-account-name> -k <primary-key>
    ```

6. Karşıya yüklemek ve dosyaları için blob indirmek için aşağıdaki komutları kullanın:

   * Bir dosyayı karşıya yüklemek için:

        ```cli
        azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
        ```

   * Bir dosyayı indirmek için:

        ```cli
        azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>
        ```
    
> [!NOTE]
> Her zaman aynı depolama hesabı ile çalışıyorsanız, hesap belirtmek yerine aşağıdaki ortam değişkenlerini ayarlayın ve her komut için anahtar:
>
> * **AZURE\_depolama\_hesabı**: depolama hesabı adı
> * **AZURE\_depolama\_erişim\_anahtar**: depolama hesabı anahtarı
>
>

#### <a id="powershell"></a>Azure PowerShell
Azure PowerShell, denetlemek ve dağıtım ve iş yüklerinizi azure'da yönetimini otomatikleştirmek için kullanabileceğiniz bir komut dosyası ortamıdır. İş istasyonunuzdan Azure PowerShell'i çalıştırmak üzere yapılandırma hakkında daha fazla bilgi için bkz: [yüklemek ve Azure PowerShell yapılandırma](/powershell/azure/overview).

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Azure Depolama'ya bir yerel dosya yüklemek için**

1. Belirtildiği gibi Azure PowerShell konsolu açın [yüklemek ve Azure PowerShell yapılandırma](/powershell/azure/overview).
2. Aşağıdaki komut dosyasında ilk beş değişkenlerin değerleri ayarlayın:

    ```powershell
    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<StorageAccountName>"
    $containerName = "<ContainerName>"

    $fileName ="<LocalFileName>"
    $blobName = "<BlobName>"

    # Get the storage account key
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    # Create the storage context object
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

    # Copy the file from local workstation to the Blob container
    Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
    ```

3. Betiği çalıştırmak için bu dosyayı kopyalamak için Azure PowerShell konsolunda yapıştırın.

Örneğin, HDInsight ile çalışmak için oluşturulmuş PowerShell betikleri bakın [HDInsight Araçları](https://github.com/blackmist/hdinsight-tools).

#### <a id="azcopy"></a>AzCopy
AzCopy içine ve dışına bir Azure depolama hesabını veri aktarma görevlerini basitleştirmek için tasarlanmış bir komut satırı aracıdır. Bağımsız bir araç olarak kullanmak ya da mevcut bir uygulamayı Bu araçta dahil edilip derecelendirilir. [AzCopy indirme][azure-azcopy-download].

AzCopy sözdizimi aşağıdaki gibidir:

```command
AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]
```

Daha fazla bilgi için [dosyaları Azure BLOB'ları için karşıya yükleme/indirme AzCopy -][azure-azcopy].

Azcopy üzerinde Linux Önizleme kullanılabilir.  Bkz: [AzCopy üzerinde Linux Önizleme Duyurusu](https://blogs.msdn.microsoft.com/windowsazurestorage/2017/05/16/announcing-azcopy-on-linux-preview/).

#### <a id="commandline"></a>Hadoop komut satırı
Hadoop komut satırı, yalnızca veri kümesi baş düğümünde mevcut olduğunda, verileri Azure depolama blobu depolamak için yararlıdır.

Hadoop komutu kullanmak için aşağıdaki yöntemlerden birini kullanarak bir baş düğüm için önce bağlanmanız gerekir:

* **Windows tabanlı HDInsight**: [Uzak Masaüstü kullanarak bağlan](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)
* **Linux tabanlı HDInsight**: bağlanırken [SSH veya PuTTY](hdinsight-hadoop-linux-use-ssh-unix.md).

Bağlantı kurulduktan sonra dosya depolama alanına yüklemek için aşağıdaki söz dizimini kullanabilirsiniz.

```bash
hadoop -copyFromLocal <localFilePath> <storageFilePath>
```

Örneğin, `hadoop fs -copyFromLocal data.txt /example/data/data.txt`

HDInsight için varsayılan dosya sistemi Azure Storage'da /example/data.txt gerçekte Azure Depolama'da olduğu. Dosyayı farklı de başvurabilirsiniz:

    wasb:///example/data/data.txt

or

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Dosyalarla diğer Hadoop komutlarını bir listesi için bkz. [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]
> HBase kümelerinde veri yazma 256 KB olduğunda kullanılan boyutu varsayılan engelleyin. Bu HBase API'lerini veya REST API'leri kullanırken düzgün çalışır durumdayken `hadoop` veya `hdfs dfs` ~ 12 GB'tan büyük veri hatayla sonuçlanır yazmak için komutları. Daha fazla bilgi için bkz. [blob yazma için depolama özel durumu](#storageexception) bu makaledeki bir bölüm.
>
>

### <a name="graphical-clients"></a>Grafik istemciler
Azure depolama ile çalışmak için bir grafik arabirim sağlayan çeşitli uygulamalar vardır. Aşağıdaki tabloda, birkaç bu uygulamaların bir listesi verilmiştir:

| İstemci | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [HDInsight için Microsoft Visual Studio Araçları](hadoop/apache-hadoop-visual-studio-tools-get-started.md#explore-linked-resources) |✔ |✔ |✔ |
| [Azure Depolama Gezgini](http://storageexplorer.com/) |✔ |✔ |✔ |
| [Bulut depolama Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Azure Gezgini](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |

#### <a name="visual-studio-tools-for-hdinsight"></a>HDInsight için Visual Studio Araçları
Daha fazla bilgi için [bağlı kaynaklara gitme](hadoop/apache-hadoop-visual-studio-tools-get-started.md#explore-linked-resources).

#### <a id="storageexplorer"></a>Azure Depolama Gezgini
*Azure Depolama Gezgini* incelemek ve blob'larda veri değiştirme için kullanışlı bir araçtır. Yüklenebilir ücretsiz, açık kaynaklı bir araç [ http://storageexplorer.com/ ](http://storageexplorer.com/). Kaynak kodu, bu bağlantıdan kullanılabilir.

Aracı'nı kullanmadan önce Azure depolama hesabı adını ve hesap anahtarınızı bilmeniz gerekir. Bu bilgi alma hakkında yönergeler için bkz. "nasıl yapılır: görüntüleme, kopyalama ve yeniden oluşturma depolama erişim anahtarlarını" bölümünü [oluşturun, yönetin veya bir depolama hesabını silmek][azure-create-storage-account].

1. Azure Storage Explorer'ı çalıştırın. İlk kez ise, Depolama Gezgini çalıştırdığınızda, sizden istenir **_depolama hesabı adı** ve **depolama hesabı anahtarı**. Daha önce çalıştırdıysanız, kullanın **Ekle** düğmesini yeni depolama hesabı adı ve anahtarı ekleyin.

    HDInsight kümeniz tarafından kullanılan depolama hesabı için anahtar adını girin ve ardından **Kaydet ve Aç**.

    ![HDI.AzureStorageExplorer][image-azure-storage-explorer]
2. Kapsayıcı arabirimi solundaki listesinde, HDInsight kümenizle ilişkili kapsayıcının adına tıklayın. Varsayılan olarak, HDInsight kümesinin adıdır ancak kümeyi oluştururken belirli bir adı girerseniz farklı olabilir.
3. Araç çubuğundan karşıya yükleme simgesini seçin.

    ![Karşıya simge yükle vurgulanmış olan araç çubuğu](./media/hdinsight-upload-data/toolbar.png)
4. Dosyayı karşıya yükleyin ve ardından belirtin **açık**. Sorulduğunda, **karşıya** depolama kapsayıcısının kök dosyayı karşıya yüklemek için. Belirli bir yol dosyayı karşıya yüklemek istediğiniz yolu girin **hedef** alan ve ardından **karşıya**.

    ![Dosya karşıya yükleme iletişim kutusu](./media/hdinsight-upload-data/fileupload.png)

    Dosyayı karşıya yükleme tamamlandıktan sonra HDInsight kümesinde işleri kullanabilirsiniz.

### <a name="mount-azure-storage-as-local-drive"></a>Azure depolama yerel sürücü olarak takmak
Bkz: [yerel sürücü olarak bağlama Azure Depolama'ya](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

### <a name="upload-using-services"></a>Hizmetleri kullanarak karşıya yükle
#### <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory hizmeti, kolaylaştırılmış, ölçeklenebilir ve güvenilir veri üretim komut zincirlerinde veri depolama, veri işleme ve veri taşıma hizmetleri oluşturmak için tam olarak yönetilen bir hizmettir.

Azure Data Factory, verileri Azure Depolama'ya taşıma veya Hive ve Pig gibi HDInsight özellikleri doğrudan kullanan bir veri işlem hatları oluşturmak için kullanılabilir.

Daha fazla bilgi için [Azure Data Factory belgeleri](https://azure.microsoft.com/documentation/services/data-factory/).

#### <a id="sqoop"></a>Apache Sqoop
Sqoop, Hadoop ve ilişkisel veritabanları arasında veri aktarmak için tasarlanmış bir araçtır. SQL Server, MySQL veya Oracle Hadoop dağıtılmış dosya sistemi (HDFS) ile Hadoop MapReduce veya Hive ile verileri dönüştürün ve ardından bir RDBMS'de geri verileri dışarı aktarma gibi bir ilişkisel veritabanı yönetim sistemi (RDBMS), verileri içeri aktarmak için kullanabilirsiniz.

Daha fazla bilgi için [HDInsight ile Sqoop kullanma][hdinsight-use-sqoop].

### <a name="development-sdks"></a>Geliştirme SDK'ları
Azure depolama, bir Azure SDK'sından aşağıdaki programlama dilleri kullanılarak da erişilebilir:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Azure SDK'ları yükleme hakkında daha fazla bilgi için bkz. [Azure indirmeleri](https://azure.microsoft.com/downloads/)

### <a name="troubleshooting"></a>Sorun giderme
#### <a id="storageexception"></a>Blob yazma için depolama özel durumu
**Belirtiler**: kullanırken `hadoop` veya `hdfs dfs` ~ 12 GB olan dosyaları yazmak için komutları daha büyük bir HBase kümesi, aşağıdaki hatalardan biriyle karşılaşabilirsiniz:

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

**Neden**: HDInsight üzerinde HBase Azure depolama alanına yazarken bu varsayılan 256 KB blok boyutunu için kümeleri. HBase API'lerini veya REST API'leri için çalışırken, bir hata kullanırken sonuç `hadoop` veya `hdfs dfs` komut satırı yardımcı programları.

**Çözüm**: kullanım `fs.azure.write.request.size` daha büyük bir blok boyutunu belirtin. Kullanım başına temelinde kullanarak bunu yapabilirsiniz `-D` parametresi. Aşağıdaki komutu kullanarak bu parametre ile bir örnektir `hadoop` komutu:

```bash
hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data
```

Ayrıca değerini artırabilirsiniz `fs.azure.write.request.size` Ambari kullanarak Global olarak. Aşağıdaki adımlar, Ambari Web kullanıcı arabiriminde değeri değiştirmek için kullanılabilir:

1. Tarayıcınızda, kümeniz için Ambari Web kullanıcı arabirimini gidin. Bu https://CLUSTERNAME.azurehdinsight.netburada **CLUSTERNAME** kümenizin adıdır.

    İstendiğinde, küme için yönetim adını ve parolasını girin.
2. Ekranın sol tarafından seçin **HDFS**ve ardından **yapılandırmaları** sekmesi.
3. İçinde **Filtresi...**  alanına `fs.azure.write.request.size`. Bu alanı ve sayfanın ortasındaki geçerli değeri görüntüler.
4. Değer 262144 (256 KB) yeni değerle değiştirin. Örneğin, 4194304 (4MB).

![Ambari Web kullanıcı Arabirimi aracılığıyla değerini değiştirme görüntüsü](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Ambari kullanarak daha fazla bilgi için bkz: [yönetme HDInsight kümeleri Ambari Web kullanıcı arabirimini kullanarak](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Sonraki adımlar
HDInsight ile verileri alma anladığınıza göre Analiz gerçekleştirme hakkında bilgi edinmek için bu makaleleri okuyun:

* [Azure HDInsight'ı Kullanmaya Başlama][hdinsight-get-started]
* [Program aracılığıyla Hadoop işlerini gönderme][hdinsight-submit-jobs]
* [HDInsight ile Hive kullanma][hdinsight-use-hive]
* [HDInsight ile Pig kullanma][hdinsight-use-pig]

[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]:../storage/common/storage-create-storage-account.md
[azure-azcopy-download]:../storage/common/storage-use-azcopy.md
[azure-azcopy]:../storage/common/storage-use-azcopy.md

[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]:hadoop/submit-apache-hadoop-jobs-programmatically.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[azurecli]: ../cli-install-nodejs.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
