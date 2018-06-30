---
title: Depolama/Data Lake Store - Azure Hdınsight'ta Apache Storm yazma | Microsoft Docs
description: Apache Storm için Hdınsight HDFS uyumlu depolama alanına yazmak için nasıl kullanılacağını öğrenin. Azure Storage veya Azure Data Lake Store için Hdınsight HDFS comptabile depolama sağlar. Bu belge ve ilişkili örnek HdfsBolt bileşen Hdınsight kümesinde bir Storm varsayılan depolama alanına yazmak için nasıl kullanılabileceğini gösterir.
services: hdinsight
documentationcenter: na
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 1df98653-a6c8-4662-a8c6-5d288fc4f3a6
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: larryfr
ms.openlocfilehash: 149f91f3091f08da2e54458d708a17da928c1972
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131744"
---
# <a name="write-to-hdfs-from-apache-storm-on-hdinsight"></a>Hdınsight üzerinde Apache Storm HDFS için yazma

Storm hdınsight'ta Apache Storm tarafından kullanılan HDFS uyumlu depolama verileri yazmak için nasıl kullanılacağını öğrenin. Hdınsight kullanma her ikisi de Azure depolama ve Azure Data Lake depolama HDFS uyumlu depolama. Storm sağlayan bir [HdfsBolt](http://storm.apache.org/releases/current/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) HDFS için verileri yazar bileşeni. Bu belge, her iki tür depolama alanını HdfsBolt yazma konusunda bilgi sağlar. 

> [!IMPORTANT]
> Bu belgede kullanılan örnek topoloji Hdınsight üzerinde Storm ile dahil olan bileşenleri kullanır. Diğer Apache Storm kümeleri ile kullanıldığında Azure Data Lake Store ile çalışmak için değişiklik gerektirebilir.

## <a name="get-the-code"></a>Kodu alma

Bu topoloji içeren projeyi yükleme yoluyla kullanılabilir [ https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).

Bu projeyi derlemek için geliştirme ortamınız için aşağıdaki yapılandırma gerekir:

* [Java JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya üstü. HDInsight 3.5 veya üstü için Java 8 gerekir.

* [Maven 3.x](https://maven.apache.org/download.cgi)

Dağıtım iş istasyonunuza Java ve JDK yüklerken aşağıdaki ortam değişkenleri ayarlanabilir. Ancak, bunların mevcut olup olmadığını ve sisteminiz için doğru değerleri içerip içermediğini denetlemeniz gerekir.

* `JAVA_HOME` - JDK’nın yüklendiği dizine işaret etmelidir.
* `PATH` - aşağıdaki yolları içermelidir:
  
    * `JAVA_HOME` (veya eşdeğer yol).
    * `JAVA_HOME\bin` (veya eşdeğer yol).
    * Maven'ın yüklendiği dizin.

## <a name="how-to-use-the-hdfsbolt-with-hdinsight"></a>Hdınsight ile HdfsBolt kullanma

> [!IMPORTANT]
> Hdınsight üzerinde Storm ile HdfsBolt kullanmadan önce önce bir betik eylemi içine gerekli jar dosyalarını kopyalamak için kullanmanız gerekir `extpath` Storm için. Daha fazla bilgi için bkz: [küme yapılandırma](#configure) bölümü.

HdfsBolt HDFS için yazma öğrenmek için sağladığınız dosya düzenini kullanır. Hdınsight ile aşağıdaki düzenleri birini kullanın:

* `wasb://`: Bir Azure depolama hesabıyla kullanılır.
* `adl://`: Azure Data Lake Store ile kullanılır.

Aşağıdaki tabloda farklı senaryolar için dosya düzeni kullanma örnekleri sağlar:

| Düzeni | Notlar |
| ----- | ----- |
| `wasb:///` | Bir Azure depolama hesabı blob kapsayıcısında varsayılan depolama hesabıdır |
| `adl:///` | Varsayılan depolama hesabı, Azure Data Lake Store dizinindedir. Küme oluşturma sırasında Data Lake Store'da kümenin HDFS kök dizini belirtin. Örneğin, `/clusters/myclustername/` dizin. |
| `wasb://CONTAINER@ACCOUNT.blob.core.windows.net/` | Kümeyle ilişkili bir varsayılan olmayan (ek) Azure depolama hesabı. |
| `adl://STORENAME/` | Küme tarafından kullanılan veri Gölü deposu kök dizini. Bu düzen küme dosya sistemi içeriyor dizininde dışında veri erişim sağlar. |

Daha fazla bilgi için bkz: [HdfsBolt](http://storm.apache.org/releases/current/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) Apache.org başvuru.

### <a name="example-configuration"></a>Örnek yapılandırma

Aşağıdaki YAML yapılan bir alıntı olan `resources/writetohdfs.yaml` örnekte bulunan dosyası. Bu dosya, Storm topolojisini kullanarak tanımlar [Flux](https://storm.apache.org/releases/1.1.0/flux.html) Apache Storm için çerçevesi.

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

* `syncPolicy`: Dosyaları synched/Temizlenen Dosya sistemine olduğunda tanımlar. Bu örnekte, her 1000 diziler.
* `fileNameFormat`: Dosyaları yazarken kullanmak için yolu ve dosya adı deseni tanımlar. Bu örnekte, yolun bir filtre kullanarak çalışma zamanında sağlanır ve dosya uzantısının `.txt`.
* `recordFormat`: Yazılmış dosyaları iç biçimini tanımlar. Bu örnekte, alanlar virgülle ayrılan `|` karakter.
* `rotationPolicy`: Dosyalarını döndürmek ne zaman tanımlar. Bu örnekte, hiçbir döndürme gerçekleştirilir.
* `hdfs-bolt`: Önceki bileşenleri için yapılandırma parametreleri kullanan `HdfsBolt` sınıfı.

Flux framework hakkında daha fazla bilgi için bkz: [ https://storm.apache.org/releases/1.1.0/flux.html ](https://storm.apache.org/releases/1.1.0/flux.html).

## <a name="configure-the-cluster"></a>Kümeyi yapılandırma

Varsayılan olarak, Hdınsight üzerinde Storm HdfsBolt Storm'ın sınıf yolunda Azure Storage veya Data Lake Store ile iletişim kurmak için kullandığı bileşenlerinin içermez. Bu bileşenler eklemek için aşağıdaki betik eylemi kullanın `extlib` kümenizdeki Storm için dizin:

* Betik URI: `https://hdiconfigactions.blob.core.windows.net/linuxstormextlibv01/stormextlib.sh`
* Uygulamak için düğümleri: Nimbus, yönetici
* Parametreler: yok

Bu komut dosyası kullanmayı hakkında daha fazla bilgi için bkz: [özelleştirme Hdınsight kümeleri betik eylemleri kullanılarak](./../hdinsight-hadoop-customize-cluster-linux.md) belge.

## <a name="build-and-package-the-topology"></a>Derleme ve topoloji paket

1. Örnek projesinden indirmeniz [ https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) geliştirme ortamınız için.

2. Bir komut istemi, terminal veya kabuk oturumundan, indirilen proje kökündeki dizinlere Değiştir. Derleme ve topoloji paket için aşağıdaki komutu kullanın:
   
        mvn compile package
   
    Paketleme ve yapı tamamlandıktan sonra adlı yeni bir dizin yok `target`, adlı bir dosyayı içeren `StormToHdfs-1.0-SNAPSHOT.jar`. Bu dosya derlenmiş topoloji içerir.

## <a name="deploy-and-run-the-topology"></a>Dağıtma ve topoloji çalıştırma

1. Topoloji Hdınsight kümesine kopyalamak için aşağıdaki komutu kullanın. Değiştir **kullanıcı** küme oluştururken kullandığınız SSH kullanıcı adı. **CLUSTERNAME** değerini kümenin adıyla değiştirin.
   
        scp target\StormToHdfs-1.0-SNAPSHOT.jar USER@CLUSTERNAME-ssh.azurehdinsight.net:StormToHdfs-1.0-SNAPSHOT.jar
   
    İstendiğinde, küme için SSH kullanıcısı oluştururken kullanılan parolayı girin. Parola yerine bir ortak anahtar kullandıysanız kullanmanız gerekebilir `-i` parametresi eşleşen özel anahtara yolunu belirtin.
   
   > [!NOTE]
   > HDInsight ile `scp` kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Karşıya yükleme işlemi tamamlandıktan sonra SSH kullanarak Hdınsight kümesine bağlanmak için aşağıdakileri kullanın. Değiştir **kullanıcı** küme oluştururken kullandığınız SSH kullanıcı adı. **CLUSTERNAME** değerini kümenin adıyla değiştirin.
   
        ssh USER@CLUSTERNAME-ssh.azurehdinsight.net
   
    İstendiğinde, küme için SSH kullanıcısı oluştururken kullanılan parolayı girin. Parola yerine bir ortak anahtar kullandıysanız kullanmanız gerekebilir `-i` parametresi eşleşen özel anahtara yolunu belirtin.
   
   Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

3. Bağlantı kurulduktan sonra adında bir dosya oluşturmak için aşağıdaki komutu kullanın `dev.properties`:

        nano dev.properties

4. Aşağıdaki metni içeriğini kullanmak `dev.properties` dosyası:

        hdfs.write.dir: /stormdata/
        hdfs.url: wasb:///

    > [!IMPORTANT]
    > Bu örnek, kümenizi varsayılan depolama alanı olarak Azure Storage hesabı kullandığını varsayar. Kümenizi Azure Data Lake Store kullanıyorsa kullanın `hdfs.url: adl:///` yerine.
    
    Dosyayı kaydetmek için kullanın __Ctrl + X__, ardından __Y__ve son olarak __Enter__. Data Lake deposu URL'si ve veri yazılır dizin adı bu dosyadaki değerlerini ayarlayın.

3. Topoloji başlatmak için aşağıdaki komutu kullanın:
   
        storm jar StormToHdfs-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writetohdfs.yaml --filter dev.properties

    Bu komut kümenin Nimbus düğümü göndererek Flux framework kullanarak topolojisi başlatır. Topoloji tarafından tanımlanan `writetohdfs.yaml` jar bulunan dosyası. `dev.properties` Dosya filtre olarak geçirilir ve dosyada bulunan değerleri topolojisi tarafından salt okunurdur.

## <a name="view-output-data"></a>Çıktı verilerini görüntüleme

Verileri görüntülemek için aşağıdaki komutu kullanın:

    hdfs dfs -ls /stormdata/

Bu topoloji tarafından oluşturulan dosyaların listesi görüntülenir.

Aşağıdaki listede, önceki komutları tarafından döndürülen veri örneğidir:

    Found 30 items
    -rw-r-----+  1 sshuser sshuser       488000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-0-1488568403092.txt
    -rw-r-----+  1 sshuser sshuser       444000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-1-1488568404567.txt
    -rw-r-----+  1 sshuser sshuser       502000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-10-1488568408678.txt
    -rw-r-----+  1 sshuser sshuser       582000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-11-1488568411636.txt
    -rw-r-----+  1 sshuser sshuser       464000 2017-03-03 19:13 /stormdata/hdfs-bolt-3-12-1488568411884.txt

## <a name="stop-the-topology"></a>Topolojiyi durdurma

Durdurulana kadar Storm topolojileri çalıştırmak veya küme silinir. Topoloji durdurmak için aşağıdaki komutu kullanın:

    storm kill hdfswriter

## <a name="delete-your-cluster"></a>Kümenizi Sil

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure Storage ve Azure Data Lake Store yazma için Storm kullanmayı öğrendiniz, diğer bulma [örnekler için Hdınsight Storm](apache-storm-example-topology.md).

