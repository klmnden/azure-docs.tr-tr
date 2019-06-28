---
title: Öğretici - depolama/Data Lake Storage - Azure HDInsight yazmak için kullanım Apache Storm
description: Öğretici - Azure HDInsight için HDFS uyumlu depolama alanına yazmak için Apache Storm kullanmayı öğrenin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 06/24/2019
ms.openlocfilehash: 5c1376c7d1afe9c9702cfb43a146ac1cd17d6e58
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67428357"
---
# <a name="tutorial-write-to-apache-hadoop-hdfs-from-apache-storm-on-azure-hdinsight"></a>Öğretici: Apache Hadoop HDFS'ye Azure HDInsight üzerinde Apache Storm yazma

Bu öğreticide, HDInsight üzerinde Apache Storm tarafından kullanılan HDFS uyumlu depolama verileri yazmak amacıyla Apache Storm'u kullanma gösterilmektedir. HDInsight, Azure depolama hem de Azure Data Lake Storage HDFS uyumlu depolama kullanabilirsiniz. Storm sağlayan bir [HdfsBolt](https://storm.apache.org/releases/current/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) verileri HDFS'ye Yazar bileşeni. Bu belge, HdfsBolt ya da depolama türüne yazmaya bilgi sağlar.

Bu belgede kullanılan örnek topoloji HDInsight üzerinde Storm ile birlikte gelen bileşenleri kullanır. Bu, diğer Apache Storm kümeleri ile kullanıldığında Azure Data Lake Store ile çalışmak için değişiklik gerektirebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Küme ile betik eylemi yapılandırın
> * Derlemek ve paketlemek topolojisi
> * Dağıtıp topolojisine çalıştırın
> * Çıkış verileri görüntüle
> * Topolojiyi durdurma

## <a name="prerequisites"></a>Önkoşullar

* [Java Developer Kit (JDK) 8 sürümü](https://aka.ms/azure-jdks)

* [Apache Maven](https://maven.apache.org/download.cgi) düzgün [yüklü](https://maven.apache.org/install.html) Apache göre.  Maven derleme sistemi Java projeleri için proje olur.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

* [URI şeması](../hdinsight-hadoop-linux-information.md#URI-and-scheme) kümeleri birincil depolama alanı için. Bu `wasb://` Azure depolama için `abfs://` için Azure Data Lake depolama Gen2'ye veya `adl://` Azure Data Lake depolama Gen1 için. Güvenli aktarım, Azure Depolama'da veya Data Lake depolama Gen2 için etkinse, URI olacaktır `wasbs://` veya `abfss://`sırasıyla ayrıca bakın [güvenli aktarım](../../storage/common/storage-require-secure-transfer.md).

### <a name="example-configuration"></a>Örnek yapılandırma

Aşağıdaki YAML kitabından olan `resources/writetohdfs.yaml` örnek dosya. Bu dosya, Storm topolojisi kullanan tanımlar [Flux](https://storm.apache.org/releases/1.1.2/flux.html) Apache Storm için bir çerçeve.

```yaml
components:
  - id: "syncPolicy"
    className: "org.apache.storm.hdfs.bolt.sync.CountSyncPolicy"
    constructorArgs:
      - 1000

  # Rotate files when they hit 5 MB
  - id: "rotationPolicy"
    className: "org.apache.storm.hdfs.bolt.rotation.FileSizeRotationPolicy"
    constructorArgs:
      - 5
      - "MB"

  - id: "fileNameFormat"
    className: "org.apache.storm.hdfs.bolt.format.DefaultFileNameFormat"
    configMethods:
      - name: "withPath"
        args: ["${hdfs.write.dir}"]
      - name: "withExtension"
        args: [".txt"]

  - id: "recordFormat"
    className: "org.apache.storm.hdfs.bolt.format.DelimitedRecordFormat"
    configMethods:
      - name: "withFieldDelimiter"
        args: ["|"]

# spout definitions
spouts:
  - id: "tick-spout"
    className: "com.microsoft.example.TickSpout"
    parallelism: 1


# bolt definitions
bolts:
  - id: "hdfs-bolt"
    className: "org.apache.storm.hdfs.bolt.HdfsBolt"
    configMethods:
      - name: "withConfigKey"
        args: ["hdfs.config"]
      - name: "withFsUrl"
        args: ["${hdfs.url}"]
      - name: "withFileNameFormat"
        args: [ref: "fileNameFormat"]
      - name: "withRecordFormat"
        args: [ref: "recordFormat"]
      - name: "withRotationPolicy"
        args: [ref: "rotationPolicy"]
      - name: "withSyncPolicy"
        args: [ref: "syncPolicy"]
```

Aşağıdaki öğeler bu YAML tanımlar:

* `syncPolicy`: Eşitlenmiş/Temizlenen Dosya sistemine dosya olduğunda tanımlar. Bu örnekte, her 1000 tanımlama grubu.
* `fileNameFormat`: Dosyaları yazma sırasında kullanılacak yol ve dosya adı deseni tanımlar. Bu örnekte, bir filtre kullanarak çalışma zamanında sağlanan yol ve dosya uzantısı `.txt`.
* `recordFormat`: Yazılmış olan dosyalar iç biçimini tanımlar. Bu örnekte, alanlar tarafından sınırlandırılmıştır `|` karakter.
* `rotationPolicy`: Dosyaları döndürmek ne zaman tanımlar. Bu örnekte, hiçbir dönüş gerçekleştirilir.
* `hdfs-bolt`: Önceki bileşenleri için yapılandırma parametreleri olarak kullandığı `HdfsBolt` sınıfı.

Flux çerçevesi hakkında daha fazla bilgi için bkz. [ https://storm.apache.org/releases/current/flux.html ](https://storm.apache.org/releases/current/flux.html).

## <a name="configure-the-cluster"></a>Kümeyi yapılandırma

Varsayılan olarak, HDInsight üzerinde Storm bileşenlerini içermez, `HdfsBolt` Storm'ın sınıf Azure Depolama'da veya Data Lake depolama ile iletişim kurmak için kullanır. Bu bileşenleri eklemek için aşağıdaki betik eylemi kullanın `extlib` kümenizdeki Storm için dizin:

| Özellik | Değer |
|---|---|
|Betik türü |-Özel|
|Bash betiği URI'si |`https://hdiconfigactions.blob.core.windows.net/linuxstormextlibv01/stormextlib.sh`|
|Düğüm türleri |Nimbus, gözetmen|
|Parametreler |None|

Bu betik, kümesi ile kullanma hakkında daha fazla bilgi için bkz: [özelleştirme HDInsight kümelerini betik eylemlerini kullanarak](./../hdinsight-hadoop-customize-cluster-linux.md) belge.

## <a name="build-and-package-the-topology"></a>Derlemek ve paketlemek topolojisi

1. Adresinden örnek projesini indirin [ https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) geliştirme ortamınıza.

2. Bir komut istemi, terminal veya kabuk oturumundan, indirilen projedeki kök dizin değiştirin. Derleme ve topoloji paketlemek için aşağıdaki komutu kullanın:

    ```cmd
    mvn compile package
    ```

    Derleme ve paketleme işlemi tamamlandıktan sonra adlı yeni bir dizin yok `target`, adlı dosyayı içeren `StormToHdfs-1.0-SNAPSHOT.jar`. Bu dosya, derlenmiş topolojiyi içerir.

## <a name="deploy-and-run-the-topology"></a>Dağıtıp topolojisine çalıştırın

1. HDInsight küme topolojisini kopyalamak için aşağıdaki komutu kullanın. Değiştirin `CLUSTERNAME` ile kümesinin adı.

    ```cmd
    scp target\StormToHdfs-1.0-SNAPSHOT.jar sshuser@CLUSTERNAME-ssh.azurehdinsight.net:StormToHdfs-1.0-SNAPSHOT.jar
    ```

1. Karşıya yükleme işlemi tamamlandıktan sonra SSH kullanarak HDInsight kümesine bağlanmak için aşağıdakileri kullanın. Değiştirin `CLUSTERNAME` ile kümesinin adı.

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Bağlantı kurulduktan sonra adlı bir dosya oluşturmak için aşağıdaki komutu kullanın `dev.properties`:

    ```bash
    nano dev.properties
    ```

1. Aşağıdaki metni içeriğini kullanın `dev.properties` dosya. Gerektiği şekilde gözden geçirme temel alarak, [URI şeması](../hdinsight-hadoop-linux-information.md#URI-and-scheme).

    ```
    hdfs.write.dir: /stormdata/
    hdfs.url: wasbs:///
    ```

    Dosyayı kaydetmek için kullanın __Ctrl + X__, ardından __Y__ve son olarak __Enter__. Bu dosyadaki değerleri, depolama URL'si ve veri yazılır dizin adını ayarlayın.

1. Topoloji başlatmak için aşağıdaki komutu kullanın:

    ```bash
    storm jar StormToHdfs-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writetohdfs.yaml --filter dev.properties
    ```

    Bu komut kümenin Nimbus düğümüne göndererek Flux framework kullanarak topolojisini başlatır. Topoloji tarafından tanımlanan `writetohdfs.yaml` içinde jar dosyası. `dev.properties` Dosya, filtre olarak geçirilir ve dosyada bulunan değerleri topoloji tarafından okunur.

## <a name="view-output-data"></a>Çıkış verileri görüntüle

Verileri görüntülemek için aşağıdaki komutu kullanın:

  ```bash
  hdfs dfs -ls /stormdata/
  ```

Bu topoloji tarafından oluşturulan dosyaların bir listesi görüntülenir. Aşağıdaki listede, önceki komut tarafından döndürülen veriler bir örneğidir:

```output
Found 23 items
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-0-1561407909895.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-1-1561407915577.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-10-1561407943327.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-11-1561407946312.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-12-1561407949320.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-13-1561407952662.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-14-1561407955502.txt
```

## <a name="stop-the-topology"></a>Topolojiyi durdurma

Storm topolojileri durdurulana kadar çalıştırın veya küme silinir. Topoloji durdurmak için aşağıdaki komutu kullanın:

```bash
storm kill hdfswriter
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici ile oluşturulan kaynakları temizlemek için kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, ilişkili HDInsight kümesini ve kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

Azure portalını kullanarak kaynak grubunu kaldırmak için:

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve sonra __Kaynak Grupları__'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynak grubunu bulun ve sonra listenin sağ tarafındaki __Daha fazla__ düğmesine (...) sağ tıklayın.
3. __Kaynak grubunu sil__'i seçip onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, HDInsight üzerinde Apache Storm tarafından kullanılan HDFS uyumlu depolama verileri yazmak amacıyla Apache Storm kullanmayı öğrendiniz.

> [!div class="nextstepaction"]
> Diğer bulma [HDInsight için Apache Storm örnekleri](apache-storm-example-topology.md)