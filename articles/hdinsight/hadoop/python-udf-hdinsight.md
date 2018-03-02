---
title: "Python UDF Apache ile Hive veya Pig - Azure Hdınsight | Microsoft Docs"
description: "Hdınsight'ta Hadoop teknoloji yığınının Azure üzerinde Python kullanıcı tanımlı işlevler (UDF) Hive ve Pig kullanmayı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c44d6606-28cd-429b-b535-235e8f34a664
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/27/2018
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: f98fe82a9637cfdddf7af1dcb6aaf979bffcad6f
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="use-python-user-defined-functions-udf-with-hive-and-pig-in-hdinsight"></a>Hdınsight'ta kullanım Python kullanıcı tanımlı işlevler (UDF) Hive veya Pig ile

Apache Hive veya Pig içinde Azure hdınsight'ta Hadoop ile Python kullanıcı tanımlı işlevler (UDF) kullanmayı öğrenin.

## <a name="python"></a>Hdınsight üzerinde Python

Python2.7 Hdınsight 3.0 ve sonraki sürümlerinde varsayılan olarak yüklenir. Apache Hive Python bu sürümü ile akış işleme için kullanılabilir. Akış işleme, Hive ve UDF arasında veri iletmek için STDOUT ve STDIN kullanır.

Hdınsight, ayrıca Java'da yazılmış bir Python uygulaması Jython içerir. Jython doğrudan Java sanal makinesi üzerinde çalışır ve akış kullanmaz. Jython önerilen Python yorumlayıcı Python ile Pig kullanıldığında.

> [!WARNING]
> Bu belgede yer alan adımlar aşağıdaki varsayımlar olun: 
>
> * Python betiklerini yerel geliştirme ortamınızı oluşturun.
> * Komut dosyalarını kullanarak Hdınsight'a yükleme `scp` yerel bir Bash oturumu veya sağlanan PowerShell komut dosyası komutu.
>
> Kullanmak istiyorsanız, [Azure bulut Kabuğu (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) gerekir sonra Hdınsight ile çalışmak için önizleme:
>
> * Bulut Kabuk ortamı içindeki komut dosyaları oluşturun.
> * Kullanım `scp` Hdınsight bulut Kabuğu'ndan dosyaları karşıya yüklemek için.
> * Kullanım `ssh` Hdınsight'a bağlanmak ve örnekleri çalıştırmak için bulut Kabuğu'ndan.

## <a name="hivepython"></a>UDF yığını

Python, Hive HiveQL üzerinden gelen bir UDF olarak kullanılabilir `TRANSFORM` deyimi. Örneğin, aşağıdaki HiveQL çağırır `hiveudf.py` kümesi için varsayılan Azure depolama hesabına depolanan dosya.

**Linux tabanlı Hdınsight**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

**Windows tabanlı Hdınsight**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'D:\Python27\python.exe hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

> [!NOTE]
> Windows tabanlı Hdınsight kümelerinde `USING` yan tümcesi Python.exe'yi tam yolunu belirtmeniz gerekir.

Bu örnek yaptığı aşağıda verilmiştir:

1. `add file` Dosyasının başında deyimi ekler `hiveudf.py` dağıtılmış önbellek kümedeki tüm düğümler tarafından erişilebilir olması için dosyaya.
2. `SELECT TRANSFORM ... USING` Deyimi seçer verilerden `hivesampletable`. Ayrıca ClientID, devicemake ve devicemodel değerlere geçirir `hiveudf.py` komut dosyası.
3. `AS` Yan tümcesi açıklar döndürülen alanları `hiveudf.py`.

<a name="streamingpy"></a>

### <a name="create-the-hiveudfpy-file"></a>Hiveudf.py dosyası oluşturma


Geliştirme ortamınızı adlı bir metin dosyası oluşturun `hiveudf.py`. Aşağıdaki kod dosyasının içeriği kullanın:

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

Bu komut dosyası, aşağıdaki eylemleri gerçekleştirir:

1. Veri satırı STDIN okuyun.
2. Sondaki yeni satır karakteri kullanarak kaldırılır `string.strip(line, "\n ")`.
3. Akış işleme yaparken, tek bir satırı her değer arasında bir sekme karakteri ile tüm değerler içeriyor. Bu nedenle `string.split(line, "\t")` yalnızca alanları dönmeden her sekme konumundaki giriş bölmek için kullanılan.
4. İşlem tamamlandığında, çıkış STDOUT sekmesi her alanı arasında tek bir satır olarak yazılması gerekir. Örneğin, `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.
5. `while` Döngü Hayır kadar yinelenir `line` okunur.

Komut dosyası çıkışını bir yapıdır giriş değerlerini `devicemake` ve `devicemodel`ve birleştirilmiş değerinin karması.

Bkz: [örnekleri çalıştırma](#running) nasıl Hdınsight kümenize Bu örneği çalıştırmak.

## <a name="pigpython"></a>Pig UDF

Bir Python komut dosyası Pig gelen bir UDF olarak kullanılabilir `GENERATE` deyimi. Jython veya C Python kullanarak komut dosyasını çalıştırabilirsiniz.

* Jython JVM üzerinde çalışır ve yerel olarak Pig çağrılabilir.
* C Python bir dış işlem olduğundan, Pig JVM üzerinde verilerden bir Python işlemde çalışan komut için gönderilir. Python komut dosyası çıkışını Pig'ya geri gönderilir.

Python yorumlayıcı belirtmek için kullanın `register` Python betiğini başvururken. Aşağıdaki örnekler Pig betikleri kaydolmalı `myfuncs`:

* **Jython kullanmak için**: `register '/path/to/pigudf.py' using jython as myfuncs;`
* **C Python kullanma**: `register '/path/to/pigudf.py' using streaming_python as myfuncs;`

> [!IMPORTANT]
> Jython kullanırken, bir yerel yol veya bir WASB pig_jython dosyasının yolu olabilir: / / yolu. Ancak, C Python kullanırken, Pig işi göndermek için kullanmakta olduğunuz düğümünün yerel dosya sistemindeki bir dosyaya başvurmalıdır.

Bir kez kayıt Bu örnek için Pig Latin her ikisi için de aynıdır:

```pig
LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

Bu örnek yaptığı aşağıda verilmiştir:

1. Örnek veri dosyası, ilk satırı yükler `sample.log` içine `LOGS`. Ayrıca her kayıt olarak tanımlayan bir `chararray`.
2. Sonraki satıra işleminin sonucu depolamak herhangi bir null değeri filtreleyen `LOG`.
3. Ardından, kayıtları üzerinden tekrarlanan `LOG` ve kullandığı `GENERATE` çağrılacak `create_structure` Python/Jython komut dosyasında yer alan yöntemi yüklü olarak `myfuncs`. `LINE` Geçerli kayıt işleve geçirmek için kullanılır.
4. Son olarak, çıkışları STDOUT atılır kullanarak `DUMP` komutu. İşlem tamamlandıktan sonra bu komutun sonuçlarını görüntüler.

### <a name="create-the-pigudfpy-file"></a>Pigudf.py dosyası oluşturma

Geliştirme ortamınızı adlı bir metin dosyası oluşturun `pigudf.py`. Aşağıdaki kod dosyasının içeriği kullanın:

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

Pig Latin örnekte `LINE` girişi için tutarlı bir şemayı olduğundan giriş chararray tanımlanır. Python betiğini veri çıkışı için tutarlı bir şema dönüştürür.

1. `@outputSchema` Deyimi için Pig döndürülen verilerin biçimini tanımlar. Bu durumda olan bir **veri paketi**, Pig veri türü. Paketi chararray (dize) tümü aşağıdaki alanları içerir:

   * tarih - günlük girişinin oluşturulduğu tarih
   * zaman - günlük girişinin oluşturulduğu zaman
   * ClassName - için sınıf adı girişi oluşturuldu
   * Level - günlük düzeyini
   * Ayrıntı - günlük girişi için ayrıntılı ayrıntıları

2. Ardından, `def create_structure(input)` Pig çizgi öğelerine geçirir işlevi tanımlar.

3. Örnek veri `sample.log`, çoğunlukla tarih, saat, classname düzeyi, uyumlu ve ayrıntı şema. Ancak, ile başlayan birkaç satırları içeren `*java.lang.Exception*`. Bu satırlar şemayla eşleşecek şekilde değiştirilmesi gerekir. `if` Deyimi için olanlar denetler ve ardından taşımak için giriş verileri massages `*java.lang.Exception*` veri satır içi beklenen çıkış şemasıyla getiren sonuna dize.

4. Ardından, `split` komutu ilk dört boşluk karakterleri veri bölmek için kullanılır. Çıktı içine atanan `date`, `time`, `classname`, `level`, ve `detail`.

5. Son olarak, değerleri için Pig döndürülür.

Verileri için Pig döndürüldüğünde, tutarlı bir şema tanımlandığı şekilde sahip `@outputSchema` deyimi.

## <a name="running"></a>Karşıya yükleme ve örnekler çalıştırın

> [!IMPORTANT]
> **SSH** adımlar, yalnızca Linux tabanlı Hdınsight kümesi ile çalışır. **PowerShell** adımları bir Linux veya Windows tabanlı Hdınsight kümesi ile çalışır, ancak bir Windows istemci gerektirir.

### <a name="ssh"></a>SSH

SSH kullanma hakkında daha fazla bilgi için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. Kullanım `scp` dosyaları Hdınsight kümenize kopyalamak için. Örneğin, aşağıdaki komut dosyaları adlı bir kümeye kopyalar **mycluster**.

    ```bash
    scp hiveudf.py pigudf.py myuser@mycluster-ssh.azurehdinsight.net:
    ```

2. Kümeye bağlanmak için SSH kullanın.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    Daha fazla bilgi için [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

3. SSH oturumundan WASB depolama küme için daha önce karşıya python dosyalarını ekleyin.

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    hdfs dfs -put pigudf.py /pigudf.py
    ```

Dosyaları karşıya yükleme sonrasında, Hive veya Pig işlerini çalıştırmak için aşağıdaki adımları kullanın.

#### <a name="use-the-hive-udf"></a>UDF Hive kullanma

1. Kovana bağlanmak için aşağıdaki komutu kullanın:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

    Bu komut Beeline istemci başlatır.

2. Aşağıdaki sorgu girin `0: jdbc:hive2://headnodehost:10001/>` istemi:

   ```hive
   add file wasb:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```

3. Son satırı girdikten sonra iş başlamanız gerekir. İş tamamlandığında, çıkış aşağıdaki örneğe benzer şekilde döndürür:

        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

4. Beeline çıkmak için aşağıdaki komutu kullanın:

    ```hive
    !q
    ```

#### <a name="use-the-pig-udf"></a>UDF Pig kullanma

1. Pig için bağlanmak için aşağıdaki komutu kullanın:

    ```bash
    pig
    ```

2. Aşağıdaki deyimler de girin `grunt>` istemi:

   ```pig
   Register wasb:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. Aşağıdaki satırı girdikten sonra iş başlamanız gerekir. İş tamamlandığında, çıkış benzer aşağıdaki veriler döndürür:

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. Kullanmak `quit` Grunt Kabuk çıkın ve ardından yerel dosya sisteminde pigudf.py dosyayı düzenlemek için aşağıdakileri kullanın:

    ```bash
    nano pigudf.py
    ```

5. Bir kez Düzenleyicisi'nde aşağıdaki satırı kaldırarak açıklamadan çıkarın `#` karakter satırını başından itibaren:

    ```bash
    #from pig_util import outputSchema
    ```

    Bu satırı C Python Jython yerine çalışmak için Python betiğini değiştirir. Değişikliği yaptıktan sonra kullanmak **Ctrl + X** düzenleyiciden çıkmak için. Seçin **Y**ve ardından **Enter** değişiklikleri kaydedin.

6. Kullanım `pig` Kabuğu'nu yeniden başlatmak için komutu. Konumundaki olduğunuzda `grunt>` isteminde, aşağıdaki C Python yorumlayıcı kullanarak Python komut dosyasını çalıştırmak için kullanın.

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    Bu iş tamamlandığında, ne zaman önceden Jython kullanarak betiği çalıştırdığınız aynı çıktı görmeniz gerekir.

### <a name="powershell-upload-the-files"></a>PowerShell: dosyaları karşıya yükleme

Hdınsight sunucusuna dosyaları karşıya yükleme için PowerShell kullanın. Python dosyaları karşıya yüklemek için aşağıdaki komut dosyasını kullanın:

> [!IMPORTANT] 
> Bu bölümdeki adımları Azure PowerShell kullanın. Azure PowerShell kullanma hakkında daha fazla bilgi için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=5-41)]

> [!IMPORTANT]
> Değişiklik `C:\path\to` geliştirme ortamınızı dosyalarda yoluna değeri.

Bu komut dosyası Hdınsight kümenizle ilgili bilgileri alır sonra varsayılan depolama hesabı için anahtar ve hesap ayıklar ve kapsayıcı kök dizinine dosyaları yükler.

> [!NOTE]
> Dosyaları karşıya yükleme ile ilgili daha fazla bilgi için bkz: [hdınsight'ta Hadoop işleri için verileri karşıya yükleme](../hdinsight-upload-data.md) belge.

#### <a name="powershell-use-the-hive-udf"></a>PowerShell: UDF Hive kullanma

PowerShell uzaktan Hive sorguları çalıştırmak için de kullanılabilir. Kullanan bir Hive sorgusu çalıştırmak için aşağıdaki PowerShell betiğini kullanın **hiveudf.py** komut dosyası:

> [!IMPORTANT]
> Çalıştırmadan önce komut dosyası, Hdınsight kümeniz için HTTPs/Admin hesap bilgilerini ister.

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=45-94)]

Çıkış için **Hive** işi aşağıdaki örneğe benzer görünmelidir:

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig-jython"></a>Pig (Jython)

PowerShell, Pig Latin işlerini çalıştırmak için de kullanılabilir. Kullanan bir Pig Latin işini çalıştırmak için **pigudf.py** komut dosyası, aşağıdaki PowerShell betiğini kullanın:

> [!NOTE]
> Uzaktan PowerShell kullanarak işi gönderirken C Python yorumlayıcı kullanmak mümkün değil.

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=98-144)]

Çıkış için **Pig** işi aşağıdaki veri benzer görünmelidir:

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <a name="troubleshooting"></a>Sorun giderme

### <a name="errors-when-running-jobs"></a>İşlerini çalışırken hataları

Hive işi çalıştırırken, aşağıdakine benzer bir hata karşılaşabilirsiniz:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.

Bu sorunun nedeni Python dosyasındaki satır sonları. CRLF satır bitiş ancak Linux uygulamaları olarak genellikle birçok Windows düzenleyicileri varsayılan olarak LF bekler.

Hdınsight için dosyayı karşıya yüklemeden önce CR karakterleri kaldırmak için aşağıdaki PowerShell ifadeleri kullanabilirsiniz:

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=148-150)]

### <a name="powershell-scripts"></a>PowerShell komut dosyaları

Örnekleri çalıştırmak için kullanılan örnek PowerShell komut dosyalarının her ikisi de işi için hata çıktısını görüntüler açıklamalı bir satır içerir. İş için beklenen çıktı görmüyorsanız, aşağıdaki satırı açıklamadan çıkarın ve hata bilgilerini ilgili bir sorunu gösterir, bkz.

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=135-139)]

Ayrıca hata bilgilerini (STDERR) ve (STDOUT) iş sonucunu Hdınsight depolama alanınızın günlüğe kaydedilir.

| Bu iş için... | Bu dosyalar blob kapsayıcısında bakın |
| --- | --- |
| Hive |/HivePython/stderr<p>/HivePython/stdout |
| Pig |/PigPython/stderr<p>/PigPython/stdout |

## <a name="next"></a>Sonraki adımlar

Varsayılan olarak sağlanmayan Python modüllerini yüklemek gerekirse bkz [bir modül Azure Hdınsight dağıtma](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).

Diğer yolları için Pig, Hive ve MapReduce kullanma hakkında bilgi edinmek için aşağıdaki belgelere bakın:

* [HDInsight ile Hive kullanma](hdinsight-use-hive.md)
* [HDInsight ile Pig kullanma](hdinsight-use-pig.md)
* [Hdınsight ile MapReduce kullanma](hdinsight-use-mapreduce.md)
