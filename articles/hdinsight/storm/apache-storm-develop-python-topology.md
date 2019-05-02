---
title: Python bileşenlerini - Azure HDInsight ile Apache Storm
description: Python bileşenlerini kullanan bir Apache Storm topolojisi oluşturmayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
keywords: Apache storm python
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: hrasheed
ms.openlocfilehash: a5cbd54dd07143688b676c063133bb1a73bed01a
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64694400"
---
# <a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Python kullanarak HDInsight üzerinde Apache Storm topolojileri geliştirme

Oluşturmayı bir [Apache Storm](https://storm.apache.org/) Python bileşenlerini kullanan topolojisi. Apache Storm bile, bir topoloji çeşitli dillerde bileşenlerini birleştirmenize olanak sağlayan birden çok dili destekler. [Flux](https://storm.apache.org/releases/current/flux.html) framework (0.10.0 Storm ile sunulan) kolay Python bileşenlerini kullanan çözümleri oluşturmanıza imkan tanır.

> [!IMPORTANT]  
> Bu belgedeki bilgiler, Storm, HDInsight 3.6 üzerinde kullanılarak test edilmiştir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

Bu proje için kod kullanılabilir [ https://github.com/Azure-Samples/hdinsight-python-storm-wordcount ](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).

## <a name="prerequisites"></a>Önkoşullar

* Python 2.7 veya üzeri

* Java JDK 1.8 veya üzeri

* [Apache Maven 3](https://maven.apache.org/download.cgi)

* (İsteğe bağlı) Yerel bir Storm geliştirme ortamı. Yerel bir Storm ortamı, yalnızca topoloji yerel olarak çalıştırmak istiyorsanız gereklidir. Daha fazla bilgi için [geliştirme ortamını ayarlama](https://storm.apache.org/releases/1.1.2/Setting-up-development-environment.html).

## <a name="storm-multi-language-support"></a>Storm çoklu dil desteği

Apache Storm, herhangi bir programlama dili kullanarak yazılır bileşenleri ile çalışacak şekilde tasarlanmıştır. Bileşenleri ile nasıl çalışılacağını anlamalısınız [Storm için Thrift tanımıdır](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift). Python için bir modül kolayca Storm ile arabirim olanak tanıyan Apache Storm projenin bir parçası olarak sağlanır. Bu modülü bulabilirsiniz [ https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py ](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Storm, Java sanal makinesi (JVM) üzerinde çalışan bir Java işlemdir. Diğer dillerde yazılmış bileşenler alt işlemden yürütülür. Storm bu alt işlemden stdin/stdout gönderilen JSON iletileri kullanarak iletişim kurar. Bileşenleri arasındaki iletişim hakkında daha fazla ayrıntı bulunabilir [çoklu dil Protokolü](https://storm.apache.org/documentation/Multilang-protocol.html) belgeleri.

## <a name="python-with-the-flux-framework"></a>Flux framework ile Python

Flux framework Storm topolojilerini ayrı ayrı Bileşenler'den tanımlamanızı sağlar. Flux framework YAML Storm topolojisi tanımlamak için kullanır. Bir Python bileşeni YAML belgede başvurmak nasıl bir örnek aşağıda gösterilmiştir:

```yaml
# Spout definitions
spouts:
  - id: "sentence-spout"
    className: "org.apache.storm.flux.wrappers.spouts.FluxShellSpout"
    constructorArgs:
      # Command line
      - ["python", "sentencespout.py"]
      # Output field(s)
      - ["sentence"]
    # parallelism hint
    parallelism: 1
```

Sınıf `FluxShellSpout` başlatmak için kullanılan `sentencespout.py` spout uygulayan kod.

Flux bekliyor olması için Python betiklerini `/resources` topoloji içeren jar dosyasını içinde dizin. Bu örnek Python betiklerini depolamasının `/multilang/resources` dizin. `pom.xml` Aşağıdaki XML kullanarak bu dosyayı içerir:

```xml
<!-- include the Python components -->
<resource>
    <directory>${basedir}/multilang</directory>
    <filtering>false</filtering>
</resource>
```

Daha önce bahsedildiği gibi bir `storm.py` Storm için Thrift tanımıdır uygulayan dosyası. Flux framework içerir `storm.py` otomatik olarak zaman proje oluşturulduğuna göre dahil olmak üzere hakkında endişelenmeniz gerekmez.

## <a name="build-the-project"></a>Projeyi derleme

Proje kökünden aşağıdaki komutu kullanın:

```bash
mvn clean compile package
```

Bu komut, oluşturur bir `target/WordCount-1.0-SNAPSHOT.jar` derlenmiş topolojisi içeren dosya.

## <a name="run-the-topology-locally"></a>Topoloji yerel olarak çalıştırma

Topoloji yerel olarak çalıştırmak için aşağıdaki komutu kullanın:

```bash
storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -l -R /topology.yaml
```

> [!NOTE]  
> Bu komut, yerel bir Storm geliştirme ortamını gerektirir. Daha fazla bilgi için [geliştirme ortamını ayarlama](https://storm.apache.org/releases/current/Setting-up-development-environment.html)

Bir kez topolojisi başladığında, yerel konsoluna aşağıdaki metne benzer bilgiler gösterir:


    24302 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting the cow jumped over the moon
    24302 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting the
    24302 [Thread-28] INFO  o.a.s.t.ShellBolt - ShellLog pid:2437, name:counter-bolt Emitting years:160
    24302 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=the, count=599}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=seven, count=302}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=dwarfs, count=143}
    24303 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting the cow jumped over the moon
    24303 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting cow
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=four, count=160}


Topolojiyi durdurma için kullanın __Ctrl + C__.

## <a name="run-the-storm-topology-on-hdinsight"></a>HDInsight üzerinde Storm topolojisini çalıştırın

1. Kopyalamak için aşağıdaki komutu kullanın `WordCount-1.0-SNAPSHOT.jar` HDInsight kümesi üzerinde storm'a dosyası:

    ```bash
    scp target\WordCount-1.0-SNAPSHOT.jar sshuser@mycluster-ssh.azurehdinsight.net
    ```

    Değiştirin `sshuser` kümenizin SSH kullanıcısı. Değiştirin `mycluster` küme adı ile. SSH kullanıcı için parola girmeniz istenebilir.

    SSH ve SCP kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Dosya yüklendikten sonra SSH kullanarak kümeye bağlanın:

    ```bash
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. SSH oturumundan, küme üzerinde topoloji başlatmak için aşağıdaki komutu kullanın:

    ```bash
    storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -r -R /topology.yaml
    ```

3. Storm kullanıcı arabirimini, küme üzerinde topolojiyi görüntülemek için kullanabilirsiniz. Storm kullanıcı arabirimini şu konumdadır https://mycluster.azurehdinsight.net/stormui. Değiştirin `mycluster` ile kümenizin adıdır.

> [!NOTE]  
> Başladıktan sonra Storm topolojisini durdurulana kadar çalışır. Topoloji durdurmak için aşağıdaki yöntemlerden birini kullanın:
>
> * `storm kill TOPOLOGYNAME` Komut satırından komutu
> * **KILL** Storm kullanıcı arabirimini düğmesi.


## <a name="next-steps"></a>Sonraki adımlar

HDInsight ile Python kullanılacak diğer yolları için aşağıdaki belgelere bakın:

* [Python kullanıcı tanımlı işlevler (UDF), Apache Pig ve Apache Hive kullanma](../hadoop/python-udf-hdinsight.md)
