---
title: "Apache Storm Python bileşenleri - Azure Hdınsight ile | Microsoft Docs"
description: "Python bileşenleri kullanan bir Apache Storm topolojisini oluşturmayı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
keywords: Apache storm python
ms.assetid: edd0ec4f-664d-4266-910c-6ecc94172ad8
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/22/2018
ms.author: larryfr
ms.openlocfilehash: 1da38ebbe3354bbb36f68d1243b30bf2f4c5633f
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Python kullanarak Hdınsight üzerinde Apache Storm topolojileri geliştirme

Python bileşenleri kullanan bir Apache Storm topolojisini oluşturmayı öğrenin. Apache Storm, bile, bir topoloji çeşitli dillerde bileşenlerini birleştirmenize izin vererek birden çok dili destekler. (0.10.0 Storm ile sunulan) Flux framework Python bileşenleri kullanan çözümler kolayca oluşturmanıza olanak sağlar.

> [!IMPORTANT]
> Bu belgedeki bilgiler Hdınsight 3.6 üzerinde Storm kullanılarak test edilmiştir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

Bu proje için kod şu adresten edinilebilir [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).

## <a name="prerequisites"></a>Önkoşullar

* Python 2.7 ya da daha yüksek

* Java JDK 1.8 veya üzeri

* Maven 3

* (İsteğe bağlı) Yerel bir Storm geliştirme ortamı. Yerel bir Storm ortam yalnızca topoloji yerel olarak çalıştırmak istiyorsanız gereklidir. Daha fazla bilgi için bkz: [bir geliştirme ortamını ayarlama](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html).

## <a name="storm-multi-language-support"></a>Storm çoklu dil desteği

Apache Storm, herhangi bir programlama dili kullanılarak yazılmış bileşenleriyle çalışmak üzere tasarlanmıştır. Bileşenleri ile nasıl çalışılacağını anlamalısınız [Thrift tanımıdır Storm için](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift). Python için bir modül Storm ile kolayca arabirim sayesinde Apache Storm projenin bir parçası olarak sağlanır. Bu modülü bulabilirsiniz [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Storm Java sanal makine (JVM) üzerinde çalışan Java bir işlemdir. Diğer dillerde yazılmış bileşenleri alt yürütülür. Storm stdin/stdout gönderilen JSON iletileri kullanarak bu alt iletişim kurar. Bileşenleri arasındaki iletişim hakkında daha fazla ayrıntı bulunabilir [çoklu dil Protokolü](https://storm.apache.org/documentation/Multilang-protocol.html) belgeleri.

## <a name="python-with-the-flux-framework"></a>Python Flux framework ile

Flux framework Storm topolojilerini Bileşenler'den ayrı olarak tanımlamanızı sağlar. Flux framework YAML Storm topolojisini tanımlamak için kullanır. Aşağıdaki metni YAML belge Python bileşeninde başvurmak nasıl örneğidir:

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

Sınıf `FluxShellSpout` başlatmak için kullanılan `sentencespout.py` spout uygulayan komut dosyası.

Flux bekliyor olması için Python komut dosyaları `/resources` topoloji içeren jar dosyasını içinde dizin. Bu örnek Python komut dosyalarında depolamasının `/multilang/resources` dizin. `pom.xml` Bu dosyayı aşağıdaki XML kullanarak içerir:

```xml
<!-- include the Python components -->
<resource>
    <directory>${basedir}/multilang</directory>
    <filtering>false</filtering>
</resource>
```

Daha önce belirtildiği gibi bir `storm.py` Storm Thrift tanımıdır uygulayan dosyası. Flux framework içerir `storm.py` otomatik olarak zaman projesi yapılandırıldığında, böylece dahil hakkında endişelenmeniz gerekmez.

## <a name="build-the-project"></a>Projeyi derleme

Proje kökünden aşağıdaki komutu kullanın:

```bash
mvn clean compile package
```

Bu komut oluşturur bir `target/WordCount-1.0-SNAPSHOT.jar` derlenmiş topoloji içeren dosya.

## <a name="run-the-topology-locally"></a>Topoloji yerel olarak çalıştırma

Topoloji yerel olarak çalıştırmak için aşağıdaki komutu kullanın:

```bash
storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -l -R /topology.yaml
```

> [!NOTE]
> Bu komut, bir yerel Storm geliştirme ortamını gerektirir. Daha fazla bilgi için bkz: [bir geliştirme ortamını ayarlama](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html)

Bir kez topolojisini başlatır aşağıdakine benzer yerel konsol bilgileri gösterir:


    24302 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting the cow jumped over the moon
    24302 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting the
    24302 [Thread-28] INFO  o.a.s.t.ShellBolt - ShellLog pid:2437, name:counter-bolt Emitting years:160
    24302 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=the, count=599}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=seven, count=302}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=dwarfs, count=143}
    24303 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting the cow jumped over the moon
    24303 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting cow
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=four, count=160}


Topoloji durdurmak için kullanma __Ctrl + C__.

## <a name="run-the-storm-topology-on-hdinsight"></a>Hdınsight üzerinde Storm topolojisini çalıştırın

1. Kopyalamak için aşağıdaki komutu kullanın `WordCount-1.0-SNAPSHOT.jar` Hdınsight kümesi üzerinde storm'a dosyası:

    ```bash
    scp target\WordCount-1.0-SNAPSHOT.jar sshuser@mycluster-ssh.azurehdinsight.net
    ```

    Değiştir `sshuser` kümeniz için SSH kullanıcıyla. Değiştir `mycluster` küme adı ile. SSH kullanıcı için parola girin istenebilir.

    SSH ve SCP kullanma hakkında daha fazla bilgi için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Dosya karşıya sonra SSH kullanarak kümeye bağlanın:

    ```bash
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. SSH oturumundan kümede topoloji başlatmak için aşağıdaki komutu kullanın:

    ```bash
    storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -r -R /topology.yaml
    ```

3. Kümede topolojisini görüntülemek için Storm kullanıcı Arabirimi kullanabilirsiniz. Storm kullanıcı Arabirimi https://mycluster.azurehdinsight.net/stormui bulunur. Değiştir `mycluster` küme adıyla.

> [!NOTE]
> Başladıktan sonra Storm topolojisini durdurulana kadar çalışır. Topoloji durdurmak için aşağıdaki yöntemlerden birini kullanın:
>
> * `storm kill TOPOLOGYNAME` Komut satırından komutu
> * **KILL** düğmesini Storm kullanıcı Arabirimi.


## <a name="next-steps"></a>Sonraki adımlar

Diğer yolları Python Hdınsight ile kullanmak için aşağıdaki belgelere bakın:

* [MapReduce işleri akış için Python kullanma](../hadoop/apache-hadoop-streaming-python.md)
* [Python kullanıcı tanımlı işlevler (UDF), Pig ve Hive kullanma](../hadoop/python-udf-hdinsight.md)
