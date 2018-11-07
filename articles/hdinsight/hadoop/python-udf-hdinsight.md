---
title: Python UDF ile Apache Hive ve Pig - Azure HDInsight
description: HDInsight, Hadoop teknoloji yığını azure'da Python kullanıcı tanımlı işlevler (UDF) Hive ve Pig kullanmayı öğrenin.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 02/27/2018
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 6227b9517da3dacb18b4f9653a7012ef9ab5a4a7
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51232282"
---
# <a name="use-python-user-defined-functions-udf-with-hive-and-pig-in-hdinsight"></a>HDInsight içinde kullanım Python kullanıcı tanımlı işlevler (UDF) Hive ve Pig ile

Apache Hive ve Pig, hadoop'ta Azure HDInsight üzerinde Python kullanıcı tanımlı işlevler (UDF) kullanmayı öğrenin.

## <a name="python"></a>HDInsight üzerinde Python

Python2.7 HDInsight 3.0 ve sonraki sürümlerde varsayılan olarak yüklenir. Apache Hive akış işleme için Python'ın bu sürümü ile kullanılabilir. Stream işleme, Hive ve UDF arasında veri iletmek için STDOUT ve STDIN kullanır.

HDInsight, Java dilinde yazılmış bir Python uygulaması Jython de içerir. Jython doğrudan Java sanal makine üzerinde çalışır ve akış kullanmaz. Pig ile Python kullanarak önerilen Python yorumlayıcısı Jython olur.

> [!WARNING]
> Bu belgedeki adımlarda aşağıdaki varsayımlar: 
>
> * Yerel geliştirme ortamınızda Python betikleri oluşturun.
> * Komut dosyalarını kullanarak HDInsight için karşıya yüklediğiniz `scp` yerel bir Bash oturumu veya PowerShell betiğini komutu.
>
> Kullanmak istiyorsanız [Azure Cloud Shell'i (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) Önizleme gerekir sonra HDInsight ile çalışmak için:
>
> * Cloud shell ortam içindeki betikleri oluşturun.
> * Kullanım `scp` HDInsight cloud shell'den dosyaları karşıya yüklemek için.
> * Kullanım `ssh` için HDInsight bağlanıp örnekleri çalıştırmak için cloud shell'den.

## <a name="hivepython"></a>UDF hive

Python, Hive aracılığıyla HiveQL UDF'yi olarak kullanılabilir `TRANSFORM` deyimi. Örneğin, aşağıdaki HiveQL çağırır `hiveudf.py` dosya kümesi için varsayılan Azure depolama hesabında depolanır.

**Linux tabanlı HDInsight**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLabel string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

**Windows tabanlı HDInsight**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'D:\Python27\python.exe hiveudf.py' AS
    (clientid string, phoneLabel string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

> [!NOTE]
> Windows tabanlı HDInsight kümelerinde `USING` yan tümcesi python.exe tam yolunu belirtmeniz gerekir.

Bu örnekte yaptığı aşağıda verilmiştir:

1. `add file` Dosyasının başında deyimi ekler `hiveudf.py` dağıtılmış önbellek kümedeki tüm düğümler tarafından erişilebilir, bu nedenle, dosyaya.
2. `SELECT TRANSFORM ... USING` Deyimi, verileri seçer `hivesampletable`. Bu işlem ayrıca ClientID, devicemake ve devicemodel değerlere geçirir `hiveudf.py` betiği.
3. `AS` Yan tümcesi, döndürülen alanları açıklar `hiveudf.py`.

<a name="streamingpy"></a>

### <a name="create-the-hiveudfpy-file"></a>Hiveudf.py dosyası oluşturma


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

1. Veri satırı STDIN okuyun.
2. Sondaki yeni satır karakterini kullanarak kaldırılır `string.strip(line, "\n ")`.
3. Akış işleme gerçekleştirirken, her değer arasında bir sekme karakteri ile tüm değerleri tek bir satır içerir. Bu nedenle `string.split(line, "\t")` döndüren alanları her sekme, konumundaki giriş bölmek için kullanılabilir.
4. İşleme tamamlandığında, çıkışı her alanı arasında bir sekme ile tek bir satır olarak STDOUT için yazılmış olmalıdır. Örneğin, `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.
5. `while` Döngü Hayır kadar yinelenir `line` okunur.

Betik çıktısı için giriş değeri bir bitiştirmedir `devicemake` ve `devicemodel`ve birleştirilmiş değer karması.

Bkz: [örnekleri çalıştırma](#running) Bu örnekte, HDInsight kümesinde çalıştırma için.

## <a name="pigpython"></a>Pig UDF

Bir Python betiği Pig UDF'yi olarak kullanılabilir `GENERATE` deyimi. Jython ya da C Python kullanarak betiği çalıştırabilirsiniz.

* Jython JVM üzerinde çalışır ve Pig yerel olarak çağrılabilir.
* C Python dış bir işlem olduğundan, Pig JVM şirket verilerini bir Python işlemde çalışan betik gönderilir. Python betiğinin çıktısı, Pig'ya geri gönderilir.

Python yorumlayıcısı belirtmek için kullanın `register` Python betiğini başvururken. Aşağıdaki örnekler Pig betikleri kaydolmalı `myfuncs`:

* **Jython kullanılacak**: `register '/path/to/pigudf.py' using jython as myfuncs;`
* **C Python kullanılacak**: `register '/path/to/pigudf.py' using streaming_python as myfuncs;`

> [!IMPORTANT]
> Jython kullanırken pig_jython dosyasının yolunu yerel bir yol ya da bir WASB olabilir: / / yolu. Ancak, C Python kullanırken, Pig işi göndermek için kullanmakta olduğunuz düğümünün yerel dosya sisteminde bir dosyasına başvurmalıdır.

Bir kez kaydı, bu örnek için Pig Latin her ikisi için de aynıdır:

```pig
LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

Bu örnekte yaptığı aşağıda verilmiştir:

1. Örnek veri dosyası birinci satır yükler `sample.log` içine `LOGS`. Ayrıca her bir kayıt olarak tanımlayan bir `chararray`.
2. Sonraki satır filtreleri işleminin sonucu depolamak herhangi bir boş değerleri `LOG`.
3. Ardından, kayıtları üzerinden yinelenir `LOG` ve kullandığı `GENERATE` çağrılacak `create_structure` Python/Jython betiğinde yer yöntemi yüklü olarak `myfuncs`. `LINE` Geçerli kayıt işlevine geçirmek için kullanılır.
4. Son olarak, çıkışları STDOUT atılır kullanarak `DUMP` komutu. Bu komut, işlemi tamamlandıktan sonra sonuçları görüntüler.

### <a name="create-the-pigudfpy-file"></a>Pigudf.py dosyası oluşturma

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

## <a name="running"></a>Karşıya yükleme ve örnekleri çalıştırmak

> [!IMPORTANT]
> **SSH** adımlar, yalnızca Linux tabanlı HDInsight kümesi ile çalışır. **PowerShell** adımları için bir Linux veya Windows tabanlı HDInsight kümesi ile çalışır, ancak bu bir Windows istemci gerektirir.

### <a name="ssh"></a>SSH

SSH kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. Kullanım `scp` dosyaları HDInsight kümenize kopyalamak için. Örneğin, aşağıdaki komut dosyaları adlı bir küme kopyalar **mycluster**.

    ```bash
    scp hiveudf.py pigudf.py myuser@mycluster-ssh.azurehdinsight.net:
    ```

2. Kümeye bağlanmak için SSH kullanın.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    Daha fazla bilgi için [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

3. SSH oturumundan WASB depolama kümesi için daha önce yüklenmiş python dosyalarını ekleyin.

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    hdfs dfs -put pigudf.py /pigudf.py
    ```

Dosyaları karşıya yükledikten sonra Hive ve Pig işleri çalıştırmak için aşağıdaki adımları kullanın.

#### <a name="use-the-hive-udf"></a>Hive UDF kullanma

1. Hive için bağlanmak için aşağıdaki komutu kullanın:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

    Bu komut, Beeline istemci başlatır.

2. Aşağıdaki sorgu girin `0: jdbc:hive2://headnodehost:10001/>` istemi:

   ```hive
   add file wasb:///hiveudf.py;
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

4. Beeline'ndan çıkmak için aşağıdaki komutu kullanın:

    ```hive
    !q
    ```

#### <a name="use-the-pig-udf"></a>UDF Pig kullanma

1. Pig için bağlanmak için aşağıdaki komutu kullanın:

    ```bash
    pig
    ```

2. Aşağıdaki deyimlerini girin `grunt>` istemi:

   ```pig
   Register wasb:///pigudf.py using jython as myfuncs;
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
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    Bu iş tamamlandığında, daha önce Jython kullanarak betiği çalıştırdığınızda aynı bir çıktı görmeniz gerekir.

### <a name="powershell-upload-the-files"></a>PowerShell: dosyaları karşıya yükleme

HDInsight sunucuya dosya yüklemek için PowerShell kullanabilirsiniz. Python dosyaları karşıya yüklemek için aşağıdaki betiği kullanın:

> [!IMPORTANT] 
> Bu bölümdeki adımlarda, Azure PowerShell kullanırsınız. Azure PowerShell kullanma hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=5-41)]

> [!IMPORTANT]
> Değişiklik `C:\path\to` üzerinde geliştirme ortamınızı dosyalarının yolunu değeri.

Bu betik, HDInsight kümenizle ilgili bilgileri alır sonra hesabı ve varsayılan depolama hesabı için anahtarı ayıklar ve kapsayıcı köküne dosyaları yükler.

> [!NOTE]
> Karşıya dosya yükleme ile ilgili daha fazla bilgi için bkz: [HDInsight Hadoop işleri için verileri karşıya yükleme](../hdinsight-upload-data.md) belge.

#### <a name="powershell-use-the-hive-udf"></a>PowerShell: ' % s'Hive UDF kullanma

PowerShell uzaktan Hive sorguları çalıştırmak için de kullanılabilir. Aşağıdaki PowerShell komut dosyası kullanan bir Hive sorgusu çalıştırmak amacıyla kullanmak **hiveudf.py** betiği:

> [!IMPORTANT]
> Çalıştırmadan önce betiği HDInsight kümeniz için HTTPs/yönetim hesabı bilgileri ister.

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=45-94)]

Çıkış için **Hive** işi aşağıdaki örneğe benzer görünmelidir:

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig-jython"></a>Pig (Jython)

PowerShell, Pig Latin işlerini çalıştırmak için de kullanılabilir. Kullanan bir Pig Latin işini çalıştırmak için **pigudf.py** betik, aşağıdaki PowerShell betiğini kullanın:

> [!NOTE]
> Uzaktan PowerShell kullanarak bir iş gönderirken C Python yorumlayıcısı olarak kullanmak mümkün değildir.

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=98-144)]

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
| Hive |/ HivePython/stderr<p>/ HivePython/stdout |
| Pig |/PigPython/stderr<p>/ PigPython/stdout |

## <a name="next"></a>Sonraki adımlar

Varsayılan olarak sağlanmayan Python modüllerini yüklemek ihtiyacınız varsa bkz [Azure HDInsight için bir modül dağıtma](https://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).

Kullanılacak diğer yolları için Pig, Hive ve MapReduce kullanma hakkında bilgi edinmek için aşağıdaki belgelere bakın:

* [HDInsight ile Hive kullanma](hdinsight-use-hive.md)
* [HDInsight ile Pig kullanma](hdinsight-use-pig.md)
* [HDInsight ile MapReduce kullanma](hdinsight-use-mapreduce.md)
