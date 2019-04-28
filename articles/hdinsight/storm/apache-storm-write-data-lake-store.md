---
title: Depolama/Data Lake Storage - Azure HDInsight için Apache Storm yazma
description: Apache Storm, HDInsight için HDFS uyumlu depolama alanına yazılacak kullanmayı öğrenin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.openlocfilehash: 4ba0c861674eb2308cf1f96c33d0792f3e1a0f94
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62109472"
---
# <a name="write-to-apache-hadoop-hdfs-from-apache-storm-on-hdinsight"></a>Apache Hadoop HDFS'ye HDInsight üzerinde Apache Storm yazma

Nasıl kullanacağınızı öğrenin [Apache Storm](https://storm.apache.org/) HDInsight üzerinde Apache Storm tarafından kullanılan HDFS uyumlu depolama verileri yazmak amacıyla. HDInsight, Azure depolama hem de Azure Data Lake Storage HDFS uyumlu depolama kullanabilirsiniz. Storm sağlayan bir [HdfsBolt](https://storm.apache.org/releases/current/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) verileri HDFS'ye Yazar bileşeni. Bu belge, HdfsBolt ya da depolama türüne yazmaya bilgi sağlar. 

> [!IMPORTANT]  
> Bu belgede kullanılan örnek topoloji HDInsight üzerinde Storm ile birlikte gelen bileşenleri kullanır. Bu, diğer Apache Storm kümeleri ile kullanıldığında Azure Data Lake Store ile çalışmak için değişiklik gerektirebilir.

## <a name="get-the-code"></a>Kodu alma

Bu topoloji içeren proje yükleme yoluyla kullanılabilir [ https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).

Bu projeyi derlemek için aşağıdaki yapılandırma için geliştirme ortamınızı gerekir:

* [Java JDK 1.8](https://aka.ms/azure-jdks) veya üstü. HDInsight 3.5 veya üstü için Java 8 gerekir.

* [Maven 3.x](https://maven.apache.org/download.cgi)

Dağıtım iş istasyonunuza Java ve JDK yüklerken aşağıdaki ortam değişkenleri ayarlanabilir. Ancak, bunların mevcut olup olmadığını ve sisteminiz için doğru değerleri içerip içermediğini denetlemeniz gerekir.

* `JAVA_HOME` - JDK’nın yüklendiği dizine işaret etmelidir.
* `PATH` - aşağıdaki yolları içermelidir:
  
    * `JAVA_HOME` (veya eşdeğer yol).
    * `JAVA_HOME\bin` (veya eşdeğer yol).
    * Maven'ın yüklendiği dizin.

## <a name="how-to-use-the-hdfsbolt-with-hdinsight"></a>HDInsight ile HdfsBolt kullanma

> [!IMPORTANT]  
> HDInsight üzerinde Storm ile HdfsBolt kullanmadan önce ilk betik eylemi içine gerekli jar dosyalarını kopyalamak için kullanmalısınız `extpath` Storm için. Daha fazla bilgi için yapılandırma kümesi bölümüne bakın.

HdfsBolt HDFS'ye yazma anlamak için sağladığınız dosya şeması kullanır. HDInsight ile aşağıdaki düzenlerden birini kullanın:

* `wasb://`: Bir Azure depolama hesabı ile kullanılır.
* `abfs://`: Bir Azure Data Lake depolama Gen2 ile kullanılır.
* `adl://`: Azure Data Lake depolama Gen1 ile kullanılır.

Aşağıdaki tabloda farklı senaryolar için dosya düzeni kullanma örnekleri verilmiştir:

| Düzeni | Notlar |
| ----- | ----- |
| `wasb:///` | Bir Azure depolama hesabındaki bir blob kapsayıcısını varsayılan depolama hesabıdır |
| `abfs:///` | Varsayılan depolama hesabı bir Azure Data Lake depolama Gen2 hesabınız dizinindedir. |
| `adl:///` | Varsayılan depolama hesabı, Azure Data Lake depolama Gen1 dizinindedir. Küme oluşturma sırasında Data Lake Storage kümenin HDFS kök dizini belirtin. Örneğin, `/clusters/myclustername/` dizin. |
| `wasb://CONTAINER@ACCOUNT.blob.core.windows.net/` | Kümeyle ilişkilendirilmiş varsayılan olmayan (ek) Azure depolama hesabı. |
| `abfs://CONTAINER@ACCOUNT.dfs.core.windows.net/` | Kümeyle ilişkilendirilmiş varsayılan olmayan (ek) Azure depolama hesabı. |
| `adl://STORENAME/` | Data Lake Storage küme tarafından kullanılan kök dizini. Bu düzen, küme dosya sistemini içeren dizininin dışında bulunan verilere erişmesini sağlar. |

Daha fazla bilgi için [HdfsBolt](https://storm.apache.org/releases/current/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) Apache.org başvuru.

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

Varsayılan olarak, HDInsight üzerinde Storm HdfsBolt Storm'ın sınıf Azure Depolama'da veya Data Lake depolama ile iletişim kurmak için kullandığı bileşenlerinin içermez. Bu bileşenleri eklemek için aşağıdaki betik eylemi kullanın `extlib` kümenizdeki Storm için dizin:

* Betik URI'si: `https://hdiconfigactions.blob.core.windows.net/linuxstormextlibv01/stormextlib.sh`
* Uygulamak için düğümleri: Nimbus, gözetmen
* Parametreler: None

Bu betik, kümesi ile kullanma hakkında daha fazla bilgi için bkz: [özelleştirme HDInsight kümelerini betik eylemlerini kullanarak](./../hdinsight-hadoop-customize-cluster-linux.md) belge.

## <a name="build-and-package-the-topology"></a>Derlemek ve paketlemek topolojisi

1. Adresinden örnek projesini indirin [ https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) geliştirme ortamınıza.

2. Bir komut istemi, terminal veya kabuk oturumundan, indirilen projedeki kök dizin değiştirin. Derleme ve topoloji paketlemek için aşağıdaki komutu kullanın:
   
        mvn compile package
   
    Derleme ve paketleme işlemi tamamlandıktan sonra adlı yeni bir dizin yok `target`, adlı dosyayı içeren `StormToHdfs-1.0-SNAPSHOT.jar`. Bu dosya, derlenmiş topolojiyi içerir.

## <a name="deploy-and-run-the-topology"></a>Dağıtıp topolojisine çalıştırın

1. HDInsight küme topolojisini kopyalamak için aşağıdaki komutu kullanın. Değiştirin **kullanıcı** ile kümeyi oluştururken SSH kullanıcı adı. **CLUSTERNAME** değerini kümenin adıyla değiştirin.
   
        scp target\StormToHdfs-1.0-SNAPSHOT.jar USER@CLUSTERNAME-ssh.azurehdinsight.net:StormToHdfs-1.0-SNAPSHOT.jar
   
    İstendiğinde, küme için SSH kullanıcısı oluştururken kullandığınız parolayı girin. Parola yerine bir ortak anahtar kullandıysanız, kullanmanız gerekebilir `-i` parametresini kullanarak eşleşen özel anahtarı için yolu belirtin.
   
   > [!NOTE]  
   > HDInsight ile `scp` kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Karşıya yükleme işlemi tamamlandıktan sonra SSH kullanarak HDInsight kümesine bağlanmak için aşağıdakileri kullanın. Değiştirin **kullanıcı** ile kümeyi oluştururken SSH kullanıcı adı. **CLUSTERNAME** değerini kümenin adıyla değiştirin.
   
        ssh USER@CLUSTERNAME-ssh.azurehdinsight.net
   
    İstendiğinde, küme için SSH kullanıcısı oluştururken kullandığınız parolayı girin. Parola yerine bir ortak anahtar kullandıysanız, kullanmanız gerekebilir `-i` parametresini kullanarak eşleşen özel anahtarı için yolu belirtin.
   
   Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

3. Bağlantı kurulduktan sonra adlı bir dosya oluşturmak için aşağıdaki komutu kullanın `dev.properties`:

        nano dev.properties

4. Aşağıdaki metni içeriğini kullanın `dev.properties` dosyası:

        hdfs.write.dir: /stormdata/
        hdfs.url: wasb:///

    > [!IMPORTANT]  
    > Bu örnek, kümenizi varsayılan depolama alanı olarak Azure depolama hesabı kullandığını varsayar. Kümenizi Azure Data Lake depolama Gen2 kullanıyorsa, kullanın `hdfs.url: abfs:///` yerine. Kümenizi Azure Data Lake depolama Gen1 kullanıyorsa, kullanın `hdfs.url: adl:///` yerine.
    
    Dosyayı kaydetmek için kullanın __Ctrl + X__, ardından __Y__ve son olarak __Enter__. Bu dosyadaki değerleri, Data Lake depolama URL'si ve veri yazılır dizin adını ayarlayın.

3. Topoloji başlatmak için aşağıdaki komutu kullanın:
   
        storm jar StormToHdfs-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writetohdfs.yaml --filter dev.properties

    Bu komut kümenin Nimbus düğümüne göndererek Flux framework kullanarak topolojisini başlatır. Topoloji tarafından tanımlanan `writetohdfs.yaml` içinde jar dosyası. `dev.properties` Dosya, filtre olarak geçirilir ve dosyada bulunan değerleri topoloji tarafından okunur.

## <a name="view-output-data"></a>Çıkış verileri görüntüle

Verileri görüntülemek için aşağıdaki komutu kullanın:

    hdfs dfs -ls /stormdata/

Bu topoloji tarafından oluşturulan dosyaların bir listesi görüntülenir.

Aşağıdaki listede, önceki komut tarafından döndürülen veriler bir örneğidir:

    Found 30 items
    -rw-r-----+  1 sshuser sshuser       488000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-0-1488568403092.txt
    -rw-r-----+  1 sshuser sshuser       444000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-1-1488568404567.txt
    -rw-r-----+  1 sshuser sshuser       502000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-10-1488568408678.txt
    -rw-r-----+  1 sshuser sshuser       582000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-11-1488568411636.txt
    -rw-r-----+  1 sshuser sshuser       464000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-12-1488568411884.txt

## <a name="stop-the-topology"></a>Topolojiyi durdurma

Storm topolojileri durdurulana kadar çalıştırın veya küme silinir. Topoloji durdurmak için aşağıdaki komutu kullanın:

    storm kill hdfswriter

## <a name="delete-your-cluster"></a>Kümenizi Sil

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Sonraki adımlar

Apache Storm, Azure depolama ve Azure Data Lake depolamaya yazma kullanacağınızı öğrendiniz, diğer bulma [HDInsight için Apache Storm örnekleri](apache-storm-example-topology.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
