---
title: UDF Apache Hive ve Apache Pig - Azure HDInsight ile Python
description: HDInsight, Apache Hadoop teknoloji yığını azure'da Python kullanıcı tanımlı işlevler (UDF) Apache Hive ve Apache Pig kullanmayı öğrenin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 03/15/2019
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 6f3140f412f9d36ca36cef440bd4e60f1a9197d4
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64702233"
---
# <a name="use-python-user-defined-functions-udf-with-apache-hive-and-apache-pig-in-hdinsight"></a>Apache Hive ve Apache Pig, HDInsight ile kullanmak Python kullanıcı tanımlı işlevler (UDF)

Apache Hive ve Azure HDInsight üzerinde Apache Hadoop, Apache Pig ile Python kullanıcı tanımlı işlevler (UDF) kullanmayı öğrenin.

## <a name="python"></a>HDInsight üzerinde Python

Python2.7 HDInsight 3.0 ve sonraki sürümlerde varsayılan olarak yüklenir. Apache Hive akış işleme için Python'ın bu sürümü ile kullanılabilir. Stream işleme, Hive ve UDF arasında veri iletmek için STDOUT ve STDIN kullanır.

HDInsight, Java dilinde yazılmış bir Python uygulaması Jython de içerir. Jython doğrudan Java sanal makine üzerinde çalışır ve akış kullanmaz. Pig ile Python kullanarak önerilen Python yorumlayıcısı Jython olur.

## <a name="prerequisites"></a>Önkoşullar

* **HDInsight Hadoop kümesinde**. Bkz: [Linux'ta HDInsight kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md).
* **Bir SSH istemcisi**. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md).
* [URI şeması](../hdinsight-hadoop-linux-information.md#URI-and-scheme) kümeleri birincil depolama alanı için. Bu wasb olacaktır: / / abfs olan Azure depolama için: / / Azure Data Lake depolama Gen2'ye veya adl: / / Azure Data Lake depolama Gen1. Güvenli aktarım, Azure Depolama'da veya Data Lake depolama Gen2 için etkinse, URI wasbs olacaktır: / / ya da abfss: / /, sırasıyla ayrıca bakın [güvenli aktarım](../../storage/common/storage-require-secure-transfer.md).
* **Depolama yapılandırması için olası bir değişiklik.**  Bkz: [depolama yapılandırması](#storage-configuration) depolama hesabı türü kullanılıyorsa `BlobStorage`.
* İsteğe bağlı.  PowerShell kullanmayı planlıyorsanız ihtiyacınız olacak [AZ modül](https://docs.microsoft.com/powershell/azure/new-azureps-module-az) yüklü.

> [!NOTE]  
> Bu makalede kullanılan depolama hesabı Azure Storage ile olan [güvenli aktarım](../../storage/common/storage-require-secure-transfer.md) etkin ve bu nedenle `wasbs` makale boyunca kullanılır.

## <a name="storage-configuration"></a>Depolama yapılandırması
Kullanılan depolama hesabı türü ise Eylem gerekmiyor `Storage (general purpose v1)` veya `StorageV2 (general purpose v2)`.  Bu makaledeki işlemi çıkış için en az üretecektir `/tezstaging`.  Varsayılan hadoop yapılandırma içerecek `/tezstaging` içinde `fs.azure.page.blob.dir` yapılandırma değişkeni `core-site.xml` hizmeti `HDFS`.  Bu yapılandırma, çıkış dizini, depolama hesabı türü için desteklenmeyen sayfa blobları için neden olacak `BlobStorage`.  Kullanılacak `BlobStorage` kaldırmak için bu makalede, `/tezstaging` gelen `fs.azure.page.blob.dir` yapılandırma değişkeni.  Yapılandırma erişilebilir [Ambari UI](../hdinsight-hadoop-manage-ambari.md).  Aksi takdirde hata iletisi alırsınız: `Page blob is not supported for this account type.`

> [!WARNING]  
> Bu belgedeki adımlarda aşağıdaki varsayımlar:  
>
> * Yerel geliştirme ortamınızda Python betikleri oluşturun.
> * Komut dosyalarını kullanarak HDInsight için karşıya yüklediğiniz `scp` komut veya PowerShell betiğini.
>
> Kullanmak istiyorsanız [Azure Cloud Shell'i (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) HDInsight ile çalışmak için daha sonra yapmanız gerekir:
>
> * Cloud shell ortam içindeki betikleri oluşturun.
> * Kullanım `scp` HDInsight cloud shell'den dosyaları karşıya yüklemek için.
> * Kullanım `ssh` için HDInsight bağlanıp örnekleri çalıştırmak için cloud shell'den.

## <a name="hivepython"></a>Apache Hive UDF

Python, Hive aracılığıyla HiveQL UDF'yi olarak kullanılabilir `TRANSFORM` deyimi. Örneğin, aşağıdaki HiveQL çağırır `hiveudf.py` dosya kümesi için varsayılan Azure depolama hesabında depolanır.

```hiveql
add file wasbs:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLabel string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

Bu örnekte yaptığı aşağıda verilmiştir:

1. `add file` Dosyasının başında deyimi ekler `hiveudf.py` dağıtılmış önbellek kümedeki tüm düğümler tarafından erişilebilir, bu nedenle, dosyaya.
2. `SELECT TRANSFORM ... USING` Deyimi, verileri seçer `hivesampletable`. Bu işlem ayrıca ClientID, devicemake ve devicemodel değerlere geçirir `hiveudf.py` betiği.
3. `AS` Yan tümcesi, döndürülen alanları açıklar `hiveudf.py`.

<a name="streamingpy"></a>

### <a name="create-file"></a>Dosya oluştur

Geliştirme ortamınızı adlı bir metin dosyası oluşturun `hiveudf.py`. Dosyanın içeriğini aşağıdaki kodu kullanın:

```python
#!/usr/bin/env python
import sys
import string
import hashlib

while True:
    line = sys.stdin.readline()
    if not line:
        break

    line = string.strip(line, "\n ")
    clientid, devicemake, devicemodel = string.split(line, "\t")
    phone_label = devicemake + ' ' + devicemodel
    print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])
```

Bu betik, aşağıdaki eylemleri gerçekleştirir:

1. Veri satırı STDIN okur.
2. Sondaki yeni satır karakterini kullanarak kaldırılır `string.strip(line, "\n ")`.
3. Akış işleme gerçekleştirirken, her değer arasında bir sekme karakteri ile tüm değerleri tek bir satır içerir. Bu nedenle `string.split(line, "\t")` döndüren alanları her sekme, konumundaki giriş bölmek için kullanılabilir.
4. İşleme tamamlandığında, çıkışı her alanı arasında bir sekme ile tek bir satır olarak STDOUT için yazılmış olmalıdır. Örneğin, `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.
5. `while` Döngü Hayır kadar yinelenir `line` okunur.

Betik çıktısı için giriş değeri bir bitiştirmedir `devicemake` ve `devicemodel`ve birleştirilmiş değer karması.

### <a name="upload-file-shell"></a>(Kabuğu) dosyasını karşıya yükleyin
Aşağıdaki komutların yerini `sshuser` gerçek kullanıcı farklı olması durumunda.  Değiştirin `mycluster` gerçek bir küme adı ile.  Dosyasının bulunduğu çalışma dizininizin olduğundan emin olun.

1. Kullanım `scp` dosyaları HDInsight kümenize kopyalamak için. Düzenleyin ve aşağıdaki komutu girin:

    ```cmd
    scp hiveudf.py sshuser@mycluster-ssh.azurehdinsight.net:
    ```

2. Kümeye bağlanmak için SSH kullanın.  Düzenleyin ve aşağıdaki komutu girin:

    ```cmd
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. SSH oturumundan, küme için depolama önceden yüklenmiş python dosyalarını ekleyin.

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    ```

### <a name="use-hive-udf-shell"></a>Hive UDF (kabuğu) kullanın

1. Hive için bağlanmak için açık, SSH oturumunda aşağıdaki komutu kullanın:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

    Bu komut, Beeline istemci başlatır.

2. Aşağıdaki sorgu girin `0: jdbc:hive2://headnodehost:10001/>` istemi:

   ```hive
   add file wasbs:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```

3. Son satırı girdikten sonra iş başlamanız gerekir. İş tamamlandıktan sonra çıktı aşağıdaki örneğe benzer döndürür:

        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

4. Beeline'ndan çıkmak için aşağıdaki komutu girin:

    ```hive
    !q
    ```

### <a name="upload-file-powershell"></a>(PowerShell) dosyasını karşıya yükleyin

> [!IMPORTANT]  
> Bu PowerShell betikleri çalışmaz [güvenli aktarım](../../storage/common/storage-require-secure-transfer.md) etkinleştirilir.  Kabuk komutları kullanabilir veya güvenli aktarım devre dışı bırakın.

PowerShell uzaktan Hive sorguları çalıştırmak için de kullanılabilir. Çalışma dizininizin nerede olduğundan emin olun `hiveudf.py` bulunur.  Aşağıdaki PowerShell komut dosyası kullanan bir Hive sorgusu çalıştırmak amacıyla kullanmak `hiveudf.py` betiği:

```PowerShell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# Revise file path as needed
$pathToStreamingFile = ".\hiveudf.py"

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$clusterInfo = Get-AzHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
$container=$clusterInfo.DefaultStorageContainer
$storageAccountKey=(Get-AzStorageAccountKey `
   -ResourceGroupName $resourceGroup `
   -Name $storageAccountName)[0].Value

# Create an Azure Storage context
$context = New-AzStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

# Upload local files to an Azure Storage blob
Set-AzStorageBlobContent `
    -File $pathToStreamingFile `
    -Blob "hiveudf.py" `
    -Container $container `
    -Context $context
```

> [!NOTE]  
> Karşıya dosya yükleme ile ilgili daha fazla bilgi için bkz: [HDInsight Apache Hadoop işleri için verileri karşıya yükleme](../hdinsight-upload-data.md) belge.


#### <a name="use-hive-udf"></a>Kullanım Hive UDF


```PowerShell
# Script should stop on failures
$ErrorActionPreference = "Stop"

# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -UserName "admin" -Message "Enter the login for the cluster"

$HiveQuery = "add file wasbs:///hiveudf.py;" +
                "SELECT TRANSFORM (clientid, devicemake, devicemodel) " +
                "USING 'python hiveudf.py' AS " +
                "(clientid string, phoneLabel string, phoneHash string) " +
                "FROM hivesampletable " +
                "ORDER BY clientid LIMIT 50;"

# Create Hive job object
$jobDefinition = New-AzHDInsightHiveJobDefinition `
    -Query $HiveQuery

# For status bar updates
$activity="Hive query"

# Progress bar (optional)
Write-Progress -Activity $activity -Status "Starting query..."

# Start defined Azure HDInsight job on specified cluster.
$job = Start-AzHDInsightJob `
    -ClusterName $clusterName `
    -JobDefinition $jobDefinition `
    -HttpCredential $creds

# Progress bar (optional)
Write-Progress -Activity $activity -Status "Waiting on query to complete..."

# Wait for completion or failure of specified job
Wait-AzHDInsightJob `
    -JobId $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds

# Uncomment the following to see stderr output
<#
Get-AzHDInsightJobOutput `
   -Clustername $clusterName `
   -JobId $job.JobId `
   -HttpCredential $creds `
   -DisplayOutputType StandardError
#>

# Progress bar (optional)
Write-Progress -Activity $activity -Status "Retrieving output..."

# Gets the log output
Get-AzHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

Çıkış için **Hive** işi aşağıdaki örneğe benzer görünmelidir:

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9


## <a name="pigpython"></a>Apache Pig UDF

Bir Python betiği Pig UDF'yi olarak kullanılabilir `GENERATE` deyimi. Jython ya da C Python kullanarak betiği çalıştırabilirsiniz.

* Jython JVM üzerinde çalışır ve Pig yerel olarak çağrılabilir.
* C Python dış bir işlem olduğundan, Pig JVM şirket verilerini bir Python işlemde çalışan betik gönderilir. Python betiğinin çıktısı, Pig'ya geri gönderilir.

Python yorumlayıcısı belirtmek için kullanın `register` Python betiğini başvururken. Aşağıdaki örnekler Pig betikleri kaydolmalı `myfuncs`:

* **Jython kullanılacak**: `register '/path/to/pigudf.py' using jython as myfuncs;`
* **C Python kullanılacak**: `register '/path/to/pigudf.py' using streaming_python as myfuncs;`

> [!IMPORTANT]  
> Jython kullanırken pig_jython dosyasının yolunu yerel bir yol ya da bir WASBS olabilir: / / yolu. Ancak, C Python kullanırken, Pig işi göndermek için kullanmakta olduğunuz düğümünün yerel dosya sisteminde bir dosyasına başvurmalıdır.

Bir kez kaydı, bu örnek için Pig Latin her ikisi için de aynıdır:

```pig
LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

Bu örnekte yaptığı aşağıda verilmiştir:

1. Örnek veri dosyası birinci satır yükler `sample.log` içine `LOGS`. Ayrıca her bir kayıt olarak tanımlayan bir `chararray`.
2. Sonraki satır filtreleri işleminin sonucu depolamak herhangi bir boş değerleri `LOG`.
3. Ardından, kayıtları üzerinden yinelenir `LOG` ve kullandığı `GENERATE` çağrılacak `create_structure` Python/Jython betiğinde yer yöntemi yüklü olarak `myfuncs`. `LINE` Geçerli kayıt işlevine geçirmek için kullanılır.
4. Son olarak, çıkışları STDOUT atılır kullanarak `DUMP` komutu. Bu komut, işlemi tamamlandıktan sonra sonuçları görüntüler.

### <a name="create-file"></a>Dosya oluştur

Geliştirme ortamınızı adlı bir metin dosyası oluşturun `pigudf.py`. Dosyanın içeriğini aşağıdaki kodu kullanın:

<a name="streamingpy"></a>

```python
# Uncomment the following if using C Python
#from pig_util import outputSchema

@outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail
```

Pig Latin'i örnekte `LINE` giriş için tutarlı bir şeması yok olduğundan, Giriş bir chararray tanımlanır. Python betiğini veri çıkışı için tutarlı bir şema dönüştürür.

1. `@outputSchema` Deyimi için Pig döndürülen verilerin biçimini tanımlar. Bu durumda olan bir **veri paketi**, Pig veri türü. Paketi her biri chararray (dize), aşağıdaki alanları içerir:

   * tarih - günlük girişinin oluşturulduğu tarih
   * saati - günlük girişinin oluşturulduğu zaman
   * sınıf adı girişi oluşturulmuştur ClassName-
   * Düzey - günlük düzeyini
   * Ayrıntı - günlük girişi için ayrıntılı ayrıntıları

2. Ardından, `def create_structure(input)` satır öğeleri için Pig geçirir işlevi tanımlar.

3. Örnek veri `sample.log`, çoğunlukla uyan tarih, saat, classname düzeyini ve ayrıntı şema. İle başlayan birkaç satır içeriyor ancak `*java.lang.Exception*`. Bu satırlar şemasıyla eşleşmesi için değiştirilmesi gerekir. `if` Deyimi için denetler ve ardından taşımak üzere giriş verilerini massages `*java.lang.Exception*` veri satır içi beklenen çıkış şemasıyla birlikte getirme bitiş dizesi.

4. Ardından, `split` komutu ilk dört alanı karakter verileri bölmek için kullanılır. Çıktı olarak atanan `date`, `time`, `classname`, `level`, ve `detail`.

5. Son olarak, değerleri için Pig döndürülür.

Pig için döndürülen veriler, tutarlı bir şema sınıfında tanımlandığı gibi erişiminizde `@outputSchema` deyimi.



### <a name="upload-file-shell"></a>(Kabuğu) dosyasını karşıya yükleyin

Aşağıdaki komutların yerini `sshuser` gerçek kullanıcı farklı olması durumunda.  Değiştirin `mycluster` gerçek bir küme adı ile.  Dosyasının bulunduğu çalışma dizininizin olduğundan emin olun.

1. Kullanım `scp` dosyaları HDInsight kümenize kopyalamak için. Düzenleyin ve aşağıdaki komutu girin:

    ```cmd
    scp pigudf.py sshuser@mycluster-ssh.azurehdinsight.net:
    ```

2. Kümeye bağlanmak için SSH kullanın.  Düzenleyin ve aşağıdaki komutu girin:

    ```cmd
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. SSH oturumundan, küme için depolama önceden yüklenmiş python dosyalarını ekleyin.

    ```bash
    hdfs dfs -put pigudf.py /pigudf.py
    ```


### <a name="use-pig-udf-shell"></a>Pig UDF (kabuğu) kullanın

1. Pig için bağlanmak için açık, SSH oturumunda aşağıdaki komutu kullanın:

    ```bash
    pig
    ```

2. Aşağıdaki deyimlerini girin `grunt>` istemi:

   ```pig
   Register wasbs:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. Aşağıdaki satırı girdikten sonra iş başlamanız gerekir. İş tamamlandığında, çıkış aşağıdaki verilere benzer döndürür:

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. Kullanma `quit` Grunt kabuktan çıkış yapma ve ardından yerel dosya sisteminde pigudf.py dosyayı düzenlemek için aşağıdakileri kullanın:

    ```bash
    nano pigudf.py
    ```

5. Bir kez Düzenleyicisi'nde aşağıdaki satırı kaldırarak metindeki açıklamayı silin `#` satırını başından itibaren karakter:

    ```bash
    #from pig_util import outputSchema
    ```

    Bu satırı Jython yerine C Python ile çalışmak için Python betiğini değiştirir. Değişikliği yaptıktan sonra kullanın **Ctrl + X** düzenleyiciden çıkmak için. Seçin **Y**, ardından **Enter** değişiklikleri kaydedin.

6. Kullanım `pig` komut kabuğu'nu yeniden başlatmak için. Adresindeki olduğunuzda `grunt>` isteminde, aşağıdaki C Python yorumlayıcısı kullanarak Python betiğini çalıştırmak için kullanın.

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    Bu iş tamamlandığında, daha önce Jython kullanarak betiği çalıştırdığınızda aynı bir çıktı görmeniz gerekir.


### <a name="upload-file-powershell"></a>(PowerShell) dosyasını karşıya yükleyin

> [!IMPORTANT]  
> Bu PowerShell betikleri çalışmaz [güvenli aktarım](../../storage/common/storage-require-secure-transfer.md) etkinleştirilir.  Kabuk komutları kullanabilir veya güvenli aktarım devre dışı bırakın.

PowerShell uzaktan Hive sorguları çalıştırmak için de kullanılabilir. Çalışma dizininizin nerede olduğundan emin olun `pigudf.py` bulunur.  Aşağıdaki PowerShell komut dosyası kullanan bir Hive sorgusu çalıştırmak amacıyla kullanmak `pigudf.py` betiği:

```PowerShell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# Revise file path as needed
$pathToJythonFile = ".\pigudf.py"


# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$clusterInfo = Get-AzHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
$container=$clusterInfo.DefaultStorageContainer
$storageAccountKey=(Get-AzStorageAccountKey `
   -ResourceGroupName $resourceGroup `
   -Name $storageAccountName)[0].Value

# Create an Azure Storage context
$context = New-AzStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

# Upload local files to an Azure Storage blob
Set-AzStorageBlobContent `
    -File $pathToJythonFile `
    -Blob "pigudf.py" `
    -Container $container `
    -Context $context
```

### <a name="use-pig-udf-powershell"></a>Pig UDF (PowerShell) kullanma

> [!NOTE]  
> Uzaktan PowerShell kullanarak bir iş gönderirken C Python yorumlayıcısı olarak kullanmak mümkün değildir.

PowerShell, Pig Latin işlerini çalıştırmak için de kullanılabilir. Kullanan bir Pig Latin işini çalıştırmak için `pigudf.py` betik, aşağıdaki PowerShell betiğini kullanın:

```PowerShell
# Script should stop on failures
$ErrorActionPreference = "Stop"

# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -UserName "admin" -Message "Enter the login for the cluster"


$PigQuery = "Register wasbs:///pigudf.py using jython as myfuncs;" +
            "LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);" +
            "LOG = FILTER LOGS by LINE is not null;" +
            "DETAILS = foreach LOG generate myfuncs.create_structure(LINE);" +
            "DUMP DETAILS;"

# Create Pig job object
$jobDefinition = New-AzHDInsightPigJobDefinition -Query $PigQuery

# For status bar updates
$activity="Pig job"

# Progress bar (optional)
Write-Progress -Activity $activity -Status "Starting job..."

# Start defined Azure HDInsight job on specified cluster.
$job = Start-AzHDInsightJob `
    -ClusterName $clusterName `
    -JobDefinition $jobDefinition `
    -HttpCredential $creds

# Progress bar (optional)
Write-Progress -Activity $activity -Status "Waiting for the Pig job to complete..."

# Wait for completion or failure of specified job
Wait-AzHDInsightJob `
    -Job $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds

# Uncomment the following to see stderr output
<#
Get-AzHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds `
    -DisplayOutputType StandardError
#>

# Progress bar (optional)
Write-Progress -Activity $activity "Retrieving output..."

# Gets the log output
Get-AzHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

Çıkış için **Pig** işi aşağıdaki verilere benzer görünmelidir:

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <a name="troubleshooting"></a>Sorun giderme

### <a name="errors-when-running-jobs"></a>İşleri çalıştırırken hata

Hive işi çalıştırılırken aşağıdaki metne benzer bir hatayla karşılaşabilirsiniz:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.

Bu sorunun nedeni Python dosyasındaki satır sonlarını. CRLF sondaysa satır ancak Linux uygulamaları genellikle çok sayıda Windows düzenleyicileri varsayılan LF bekler.

HDInsight için dosyayı karşıya yüklemeden önce CR karakter kaldırmak için aşağıdaki PowerShell ifadeleri kullanabilirsiniz:

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=148-150)]

### <a name="powershell-scripts"></a>PowerShell betikleri

Örnekleri çalıştırmak için kullanılan örnek PowerShell komut dosyaları hem de işin hata çıktısı görüntüler açıklamalı bir satır içerir. İş için beklenen çıkış görmediğinizden, aşağıdaki satırı açıklamadan çıkarın ve hata bilgilerini bir sorun olduğunu gösterir, bkz.

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=135-139)]

Hata (STDERR) bilgilerini ve (STDOUT) işin sonucunu da HDInsight depolama günlüğe kaydedilir.

| Bu iş için... | Bu dosyalar bir blob kapsayıcısında bakın |
| --- | --- |
| Hive |/HivePython/stderr<p>/ HivePython/stdout |
| Pig |/PigPython/stderr<p>/ PigPython/stdout |

## <a name="next"></a>Sonraki adımlar

Varsayılan olarak sağlanmayan Python modüllerini yüklemek ihtiyacınız varsa bkz [Azure HDInsight için bir modül dağıtma](https://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).

Kullanılacak diğer yolları için Pig, Hive ve MapReduce kullanma hakkında bilgi edinmek için aşağıdaki belgelere bakın:

* [Apache Hive, HDInsight ile kullanma](hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](hdinsight-use-pig.md)
* [HDInsight ile MapReduce kullanma](hdinsight-use-mapreduce.md)
