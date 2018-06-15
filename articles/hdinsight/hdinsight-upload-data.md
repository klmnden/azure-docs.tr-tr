---
title: Hdınsight'ta Hadoop işleri için veri yükleme | Microsoft Docs
description: Karşıya yükleme ve Azure CLI, Azure Storage Gezgini, Azure PowerShell, Hadoop komut satırı veya Sqoop kullanarak hdınsight'ta Hadoop işleri için veri erişim hakkında bilgi edinin.
keywords: etl hadoop alma verileri hadoop, hadoop veri yükleme
services: hdinsight,storage
documentationcenter: ''
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 56b913ee-0f9a-4e9f-9eaf-c571f8603dd6
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jgao
ms.openlocfilehash: 1734e9f0002ab7f33a8a67e44811352cb5c45fdc
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34202467"
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>HDInsight'ta Hadoop işleri için veri yükleme

Azure Hdınsight, Azure Storage ve Azure Data Lake Store üzerinde tam özellikli Hadoop dağıtılmış dosya sistemi (HDFS) sağlar. Azure depolama ve Data lake Store HDFS uzantı olarak müşterilere sorunsuz bir deneyim sağlamak üzere tasarlanmıştır. Bunlar, doğrudan yönettiği veriler üzerinde çalışmak için Hadoop ekosistemi bileşenlerini kümesini etkinleştirin. Azure depolama ve Data Lake Store depolama verilerinin ve bu verileri hesaplamalar için iyileştirilmiş farklı dosya sistemleridir. Azure depolama kullanmanın yararları hakkında bilgi için [kullanım Azure Storage Hdınsight ile] [ hdinsight-storage] ve [kullanım Data Lake Store Hdınsight ile](hdinsight-hadoop-use-data-lake-store.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki gereksinimleri dikkate alın:

* Bir Azure HDInsight kümesi. Yönergeler için bkz: [Azure Hdınsight kullanmaya başlama] [ hdinsight-get-started] veya [Hdınsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).
* Aşağıdaki iki makaleleri bilgisi:

    - [Hdınsight ile Azure depolama kullanma][hdinsight-storage]
    - [Kullanım Data Lake Store Hdınsight ile](hdinsight-hadoop-use-data-lake-store.md)

## <a name="upload-data-to-azure-storage"></a>Azure depolama alanına veri yükleme

### <a name="command-line-utilities"></a>Komut satırı yardımcı programları
Microsoft Azure Storage ile çalışmak için aşağıdaki yardımcı programlar sağlar:

| Aracı | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure komut satırı arabirimi][azurecli] |✔ |✔ |✔ |
| [Azure PowerShell][azure-powershell] | | |✔ |
| [AzCopy][azure-azcopy] |✔ | |✔ |
| [Hadoop komutu](#commandline) |✔ |✔ |✔ |

> [!NOTE]
> Hadoop komutu, yalnızca Azure CLI, Azure PowerShell ve AzCopy tüm dış Azure'dan kullanılabilir olsa da, Hdınsight kümesinde kullanılabilir. Ve yerel dosya sisteminden Azure depolama alanına veri yükleme komutu yalnızca sağlar.
>
>

#### <a id="xplatcli"></a>Azure CLI
Azure CLI Azure hizmetlerini yönetmenize olanak sağlayan platformlar arası bir araçtır. Verileri Azure depolama alanına yüklemek için aşağıdaki adımları kullanın:

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Yükleme ve Mac, Linux ve Windows için Azure CLI yapılandırma](../cli-install-nodejs.md).
2. Bir komut istemi, bash ya da diğer kabuğunu açın ve Azure aboneliğinize kimliğini doğrulamak için aşağıdakileri kullanın.

    ```cli
    azure login
    ```

    İstendiğinde, aboneliğiniz için kullanıcı adını ve parolasını girin.
3. Aboneliğiniz için depolama hesaplarını listelemek için aşağıdaki komutu girin:

    ```cli
    azure storage account list
    ```

4. Çalışmak istediğiniz blob içeren depolama hesabını seçin ve ardından bu hesap için anahtar almak için aşağıdaki komutu kullanın:

    ```cli
    azure storage account keys list <storage-account-name>
    ```

    Bu komut döndürür **birincil** ve **ikincil** anahtarları. Kopya **birincil** sonraki adımlarda kullanılacak çünkü anahtar değeri.
5. Depolama hesabı blob kapsayıcılara listesini almak için aşağıdaki komutu kullanın:

    ```cli
    azure storage container list -a <storage-account-name> -k <primary-key>
    ```

6. Karşıya yükleme ve blob dosyalarını indirmek için aşağıdaki komutları kullanın:

   * Bir dosyayı karşıya yüklemek için:

        ```cli
        azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
        ```

   * Bir dosyayı indirmek için:

        ```cli
        azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>
        ```
    
> [!NOTE]
> Her zaman aynı depolama hesabı ile çalışıyorsanız, hesabı belirtme yerine aşağıdaki ortam değişkenlerini ayarlama ve her komut için anahtar:
>
> * **AZURE\_depolama\_hesap**: depolama hesabı adı
> * **AZURE\_depolama\_erişim\_anahtar**: depolama hesabı anahtarı
>
>

#### <a id="powershell"></a>Azure PowerShell
Azure PowerShell denetlemek ve dağıtımını ve yönetimini azure'da, iş yüklerini otomatikleştirmek için kullanabileceğiniz bir komut dosyası ortamıdır. Azure PowerShell'i çalıştırmak için iş istasyonunuzu yapılandırma hakkında daha fazla bilgi için bkz: [yükleyin ve Azure PowerShell yapılandırma](/powershell/azure/overview).

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Bir yerel dosyayı Azure depolama alanına yüklemek için**

1. Belirtildiği gibi Azure PowerShell konsolu açın [yükleyin ve Azure PowerShell yapılandırma](/powershell/azure/overview).
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

3. Komut dosyasını çalıştırmak için dosyayı kopyalamak için Azure PowerShell konsolunda yapıştırın.

Örneğin bkz: Hdınsight ile çalışmak için oluşturulan PowerShell komut dosyalarını [Hdınsight Araçları](https://github.com/blackmist/hdinsight-tools).

#### <a id="azcopy"></a>AzCopy
AzCopy içine ve dışına bir Azure Storage hesabı veri aktarma görevini kolaylaştırmak için tasarlanmış bir komut satırı aracıdır. Ayrı bir araç olarak kullanabilir veya varolan bir uygulama bu aracı içerecek. [AzCopy karşıdan][azure-azcopy-download].

AzCopy sözdizimi aşağıdaki gibidir:

```command
AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]
```

Daha fazla bilgi için bkz: [dosyaları Azure BLOB'ları için karşıya yükleme/indirme AzCopy -][azure-azcopy].

Azcopy Linux Önizleme üzerinde kullanılabilir.  Bkz: [AzCopy Linux preview'daki tanışın](https://blogs.msdn.microsoft.com/windowsazurestorage/2017/05/16/announcing-azcopy-on-linux-preview/).

#### <a id="commandline"></a>Hadoop komut satırı
Hadoop komut satırı, yalnızca veri kümesi baş düğümünde zaten geldiğinde, verileri Azure depolama blob'a depolamak için yararlıdır.

Hadoop komutu kullanmak için aşağıdaki yöntemlerden birini kullanarak headnode bağlanın:

* **Windows tabanlı Hdınsight**: [Uzak Masaüstü kullanarak bağlan](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)
* **Linux tabanlı Hdınsight**: bağlanırken [SSH veya PuTTY](hdinsight-hadoop-linux-use-ssh-unix.md).

Bağlantı kurulduktan sonra bir dosya depolama alanına yüklemek için aşağıdaki söz dizimini kullanabilirsiniz.

```bash
hadoop -copyFromLocal <localFilePath> <storageFilePath>
```

Örneğin, `hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Hdınsight için varsayılan dosya sistemi Azure Storage'da olduğundan, /example/data.txt gerçekten Azure Storage'da değildir. Dosyasına olarak başvurabilir:

    wasb:///example/data/data.txt

veya

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Dosyalarla diğer Hadoop komutların listesi için bkz: [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]
> HBase kümelerinde varsayılan bloğu boyutunu veri yazma 256 KB olduğunda kullanılır. Bu HBase API'lerini veya REST API'leri kullanırken düzgün çalışır, ancak kullanarak `hadoop` veya `hdfs dfs` ~ 12 GB'den büyük veri hatayla sonuçlanır yazmak için komutları. Daha fazla bilgi için bkz: [blob yazma için depolama özel durumu](#storageexception) bu makalenin bölümünde.
>
>

### <a name="graphical-clients"></a>Grafik istemcileri
Azure Storage ile çalışmak için bir grafik arabirim sağlayan birkaç uygulamalar vardır. Aşağıdaki tabloda, bu uygulamaları birkaç listesidir:

| İstemci | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Hdınsight için Microsoft Visual Studio Araçları](hadoop/apache-hadoop-visual-studio-tools-get-started.md#explore-linked-resources) |✔ |✔ |✔ |
| [Azure Depolama Gezgini](http://storageexplorer.com/) |✔ |✔ |✔ |
| [Bulut depolama Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Azure Gezgini](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |

#### <a name="visual-studio-tools-for-hdinsight"></a>Hdınsight için Visual Studio Araçları
Daha fazla bilgi için bkz: [bağlı kaynaklara gitme](hadoop/apache-hadoop-visual-studio-tools-get-started.md#explore-linked-resources).

#### <a id="storageexplorer"></a>Azure Storage Gezgini
*Azure Storage Gezgini* inceleme ve BLOB verileri değiştirme için yararlı bir araçtır. Bu, yüklenebilir bir ücretsiz, açık kaynak aracıdır [ http://storageexplorer.com/ ](http://storageexplorer.com/). Kaynak kodu, bu bağlantıdan kullanılabilir.

Aracı'nı kullanmadan önce Azure depolama hesabı adı ve hesap anahtarınızın bilmeniz gerekir. Bu bilgi alma hakkında yönergeler için bkz: "nasıl yapılır: görüntüleme, kopyalama ve erişim anahtarları yeniden oluşturma depolama" bölümünü [oluşturun, yönetmek veya bir depolama hesabını silmek][azure-create-storage-account].

1. Azure Storage Gezgini çalıştırın. İlk kez kullanıyorsanız Depolama Gezgini'ni çalıştırmak, sizden istenir **_vm hesap adı** ve **depolama hesabı anahtarı**. Daha önce çalıştırdıysanız kullanmak **Ekle** bir yeni depolama hesabı adı ve anahtar eklemek için düğmeyi.

    Bir ad girin ve Hdınsight küme tarafından kullanılan depolama hesabı için anahtarını ve ardından **Aç & Kaydet**.

    ![HDI.AzureStorageExplorer][image-azure-storage-explorer]
2. Arabirim solundaki kapsayıcıları listesinde, Hdınsight kümenizle ilişkilendirilmiş kapsayıcının adını tıklatın. Varsayılan olarak, Hdınsight kümesi adıdır, ancak küme oluştururken, belirli bir ad girdiyseniz farklı olabilir.
3. Araç Çubuğu'ndan karşıya yükleme simgesini seçin.

    ![Araç çubuğu vurgulanmış karşıya yükleme simgesi](./media/hdinsight-upload-data/toolbar.png)
4. Dosyayı karşıya yükleyin ve ardından belirtin **açık**. İstendiğinde, seçin **karşıya** depolama kapsayıcısı köküne dosya karşıya. Belirli bir yola dosyayı karşıya yüklemek istediğiniz yolu girin **hedef** alan ve ardından **karşıya**.

    ![Dosya yükleme iletişim kutusu](./media/hdinsight-upload-data/fileupload.png)

    Dosyayı karşıya yüklemeyi tamamladığında, Hdınsight kümesinde işleri kullanabilirsiniz.

### <a name="mount-azure-storage-as-local-drive"></a>Azure depolama yerel sürücü olarak bağlama
Bkz: [bağlama Azure Storage yerel sürücü olarak](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

### <a name="upload-using-services"></a>Hizmetleri kullanarak karşıya yükle
#### <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory hizmetinin kolaylaştırılmış, ölçeklenebilir ve güvenilir veri üretim ardışık düzenlerle veri depolama, veri işleme ve veri taşıma hizmetleri oluşturmak için tam olarak yönetilen bir hizmettir.

Azure Data Factory, verileri Azure depolama alanına taşıyabilir veya doğrudan Hdınsight özelliklerine Hive veya Pig gibi veri ardışık düzen oluşturmak için kullanılabilir.

Daha fazla bilgi için bkz: [Azure Data Factory belgelerine](https://azure.microsoft.com/documentation/services/data-factory/).

#### <a id="sqoop"></a>Apache Sqoop
Sqoop, Hadoop ve ilişkisel veritabanları arasında veri aktarmak için tasarlanmış bir araçtır. SQL Server, MySQL ve Oracle Hadoop dağıtılmış dosya sistemi (HDFS) içine Hadoop ile MapReduce veya Hive verilerde dönüştürme ve bir RDBMS verileri dışarı aktarma gibi bir ilişkisel veritabanı yönetim sistemine (RDBMS), veri aktarmak için kullanabilirsiniz.

Daha fazla bilgi için bkz: [Hdınsight ile kullanım Sqoop][hdinsight-use-sqoop].

### <a name="development-sdks"></a>Geliştirme SDK'ları
Azure depolama aşağıdaki programlama dillerini bir Azure SDK kullanılarak da erişilebilir:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Azure SDK'ları yükleme hakkında daha fazla bilgi için bkz: [Azure indirir](https://azure.microsoft.com/downloads/)

### <a name="troubleshooting"></a>Sorun giderme
#### <a id="storageexception"></a>Blob yazma için depolama özel durumu
**Belirtiler**: kullanırken `hadoop` veya `hdfs dfs` ~ 12 GB olan dosyaları yazmak için komutları veya daha büyük bir HBase kümesi üzerinde aşağıdaki hatalardan biriyle karşılaşabilirsiniz:

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

**Neden**: hdınsight'ta HBase Azure depolama alanına yazılırken bu varsayılan bir blok boyutu 256 KB kümeleri. HBase API'lerini veya REST API'leri için çalışırken, bu hatayla kullanırken sonuçlanır `hadoop` veya `hdfs dfs` komut satırı yardımcı programları.

**Çözümleme**: kullanım `fs.azure.write.request.size` daha büyük bir blok boyutu belirtmek için. Bir kullanım başına temelinde kullanarak bunu yapabilirsiniz `-D` parametresi. Aşağıdaki komutu kullanarak bu parametre ile bir örnektir `hadoop` komutu:

```bash
hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data
```

Değerini de artırabilirsiniz `fs.azure.write.request.size` Ambari kullanarak genel. Aşağıdaki adımlar, Ambari Web kullanıcı arabirimini değerini değiştirmek için kullanılabilir:

1. Tarayıcınızda, kümeniz için Ambari Web kullanıcı arabirimini gidin. Bu https://CLUSTERNAME.azurehdinsight.net, burada **CLUSTERNAME** kümenizin adıdır.

    İstendiğinde, küme için Yönetici adını ve parolasını girin.
2. Ekranın sol taraftan seçin **HDFS**ve ardından **yapılandırmalar** sekmesi.
3. İçinde **filtre...**  alanına, `fs.azure.write.request.size`. Bu alanı ve sayfa ortasında geçerli değeri görüntüler.
4. Değer 262144 (256 KB) yeni değerini değiştirin. Örneğin, 4194304 (4MB).

![Ambari Web kullanıcı Arabirimi aracılığıyla değeri değiştirme resmi](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Ambari kullanarak daha fazla bilgi için bkz: [Ambari Web kullanıcı arabirimini kullanarak Hdınsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Sonraki adımlar
Verileri Hdınsight'a alma nasıl anladığınıza göre Analiz gerçekleştirme hakkında bilgi edinmek için aşağıdaki makalelere okuyun:

* [Azure HDInsight'ı Kullanmaya Başlama][hdinsight-get-started]
* [Hadoop işlerini programlı olarak gönderme][hdinsight-submit-jobs]
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
