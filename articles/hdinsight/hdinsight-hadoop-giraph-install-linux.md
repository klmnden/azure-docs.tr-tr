---
title: Yükleme ve Hdınsight (Hadoop) - Azure Giraph kullanma | Microsoft Docs
description: Betik eylemleri kullanarak Linux tabanlı Hdınsight kümelerinde Giraph yüklemeyi öğrenin. Betik eylemleri küme oluşturma sırasında küme yapılandırmasını değiştirme veya hizmetleri ve yardımcı programları yükleme özelleştirmenizi sağlar.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9fcac906-8f06-4002-9fe8-473e42f8fd0f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: larryfr
ms.openlocfilehash: dcfd37a40ce16a1574c21e3a6e9520cb2e773166
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a>Giraph Hdınsight Hadoop kümelerine yükleme ve büyük ölçekli grafikleri işlemek için Giraph kullanma

Apache Giraph bir Hdınsight kümesine yüklemek öğrenin. Hdınsight betik eylemi özelliğidir bash komut dosyasını çalıştırarak kümenizi özelleştirmenizi sağlar. Komut dosyaları, küme sırasında ve Küme oluşturulduktan sonra özelleştirmek için kullanılabilir.

> [!IMPORTANT]
> Bu belgede yer alan adımlar Linux kullanan bir Hdınsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="whatis"></a>Giraph nedir

[Apache Giraph](http://giraph.apache.org/) Hadoop kullanarak işleme grafik işlemleri yapmanıza olanak tanır ve Azure Hdınsight ile kullanılabilir. Grafik nesneleri arasındaki ilişkiler model. Örneğin, büyük bir ağ yönlendiricileri arasındaki bağlantıları Internet ya da sosyal ağlar üzerindeki kişiler arasındaki ilişkileri gibi. Grafik işleme nedeni bir grafik nesneleri arasındaki ilişkiler hakkında gibi sağlar:

* Geçerli ilişkilere dayanan olası arkadaş tanımlama.

* Bir ağda iki bilgisayar arasındaki en kısa yolu tanımlama.

* Web sayfalarındaki sayfa derecesini hesaplama.

> [!WARNING]
> Hdınsight kümesi ile sağlanan bileşenler tam olarak desteklenen - yalıtmak ve bu bileşenleri ilgili sorunları gidermek için Microsoft Support yardımcı olur.
>
> Giraph gibi özel bileşenleri daha fazla sorunu gidermeye yardımcı olmak üzere ticari koşulların elverdiği oranda makul desteği alabilirsiniz. Microsoft Support sorunu çözmek için mümkün olabilir. Aksi durumda, bu teknoloji derin uzmanlık bulunduğu açık kaynak toplulukları başvurmanız gerekir. Örneğin, olduğu gibi kullanılabilecek birçok topluluk siteleri vardır: [Hdınsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Apache projeleri proje siteleri de [ http://apache.org ](http://apache.org), örneğin: [Hadoop](http://hadoop.apache.org/).


## <a name="what-the-script-does"></a>Betiğin yaptığı

Bu komut dosyası, aşağıdaki eylemleri gerçekleştirir:

* Giraph için yükler `/usr/hdp/current/giraph`

* Kopya `giraph-examples.jar` varsayılan storage (WASB) dosyasına kümeniz için: `/example/jars/giraph-examples.jar`

## <a name="install"></a>Betik eylemleri kullanılarak Giraph yükleyin

Giraph bir Hdınsight kümesine yüklemek için örnek komut dosyası şu konumda kullanılabilir:

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

Bu bölümde Azure Portalı'nı kullanarak küme oluşturulurken örnek komut dosyası kullanma hakkında yönergeler sağlar.

> [!NOTE]
> Aşağıdaki yöntemlerden birini kullanarak bir komut dosyası eylemi uygulanabilir:
> * Azure PowerShell
> * Azure CLI
> * Hdınsight .NET SDK'sı
> * Azure Resource Manager şablonları
> 
> Zaten çalışan kümeler için betik eylemleri de uygulayabilirsiniz. Daha fazla bilgi için bkz: [özelleştirme Hdınsight betik eylemleri ile kümeleri](hdinsight-hadoop-customize-cluster-linux.md).

1. ' Ndaki adımları kullanarak bir küme oluşturmaya başlamak [oluşturma Linux tabanlı Hdınsight kümeleri](hdinsight-hadoop-create-linux-clusters-portal.md), ancak oluşturma tamamlamayın.

2. İçinde **isteğe bağlı yapılandırma** bölümünde, select **betik eylemleri**ve aşağıdaki bilgileri sağlayın:

   * **AD**: betik eylemi için kolay bir ad girin.

   * **BETİK URI'Sİ**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

   * **HEAD**: Bu girişi denetleyin

   * **ÇALIŞAN**: Bu girdi işaretlemeden bırakın

   * **ZOOKEEPER**: Bu girdi işaretlemeden bırakın

   * **PARAMETRELERİ**: Bu alanı boş bırakın

3. Ekranın alt kısmındaki **betik eylemleri**, kullanın **seçin** yapılandırmayı kaydetmek için düğmesi. Son olarak, **seçin** alt kısmındaki düğmesi **isteğe bağlı yapılandırma** isteğe bağlı yapılandırma bilgilerini kaydetmek için bölümü.

4. Bölümünde açıklandığı gibi küme oluşturmaya devam [oluşturma Linux tabanlı Hdınsight kümeleri](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="usegiraph"></a>Hdınsight'ta nasıl Giraph kullanıyor?

Küme oluşturulduktan sonra Giraph ile dahil SimpleShortestPathsComputation örneği çalıştırmak için aşağıdaki adımları kullanın. Bu örnek temel kullanır [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) bir grafik nesneler arasındaki en kısa yolu bulmak için uygulama.

1. SSH kullanarak HDInsight kümesine bağlanma:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Adlı bir dosya oluşturmak için aşağıdaki komutu kullanın **tiny_graph.txt**:

    ```bash
    nano tiny_graph.txt
    ```

    Aşağıdaki metni bu dosyanın içeriğini kullanın:

    ```text
    [0,0,[[1,1],[3,3]]]
    [1,0,[[0,1],[2,2],[3,1]]]
    [2,0,[[1,2],[4,4]]]
    [3,0,[[0,3],[1,1],[4,4]]]
    [4,0,[[3,4],[2,4]]]
    ```

    Bu veri biçimi kullanarak bir yönlendirilmiş grafik nesneler arasındaki ilişkiyi tanımlayan `[source_id, source_value,[[dest_id], [edge_value],...]]`. Her satır arasındaki bir ilişkiyi temsil eden bir `source_id` nesne ve bir veya daha fazla `dest_id` nesneleri. `edge_value` , Gücü veya arasındaki bağlantıyı uzaklığını olarak düşünülebilir `source_id` ve `dest\_id`.

    Çıkış çizilmiş ve nesneleri arasındaki uzaklığı olarak değeri (veya ağırlık) kullanarak verileri Aşağıdaki diyagramda gibi görünebilir:

    ![Daireler arasında değişen uzaklığı satır olarak çizilen tiny_graph.txt](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

3. Dosyayı kaydetmek için kullanın **Ctrl + X**, ardından **Y**ve son olarak **Enter** dosya adını kabul etmek için.

4. Hdınsight kümeniz için birincil depolama alanına verileri depolamak için aşağıdakileri kullanın:

    ```bash
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. Aşağıdaki komutu kullanarak SimpleShortestPathsComputation örnek çalıştırın:

    ```bash
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```

    Bu komutla birlikte kullanılan parametreler aşağıdaki tabloda açıklanmıştır:

   | Parametre | Ne yapar? |
   | --- | --- |
   | `jar` |Örnekler içeren jar dosyasını. |
   | `org.apache.giraph.GiraphRunner` |Örnekler başlatmak için kullanılan bir sınıftır. |
   | `org.apache.giraph.examples.SimpleShortestPathsCoputation` |Kullanılan örnek. Bu örnekte, kimliği 1 ve grafikteki tüm diğer kimlikleri arasında en kısa yolu hesaplar. |
   | `-ca mapred.job.tracker` |Küme için headnode. |
   | `-vif` |Girdi verileri kullanmak için giriş biçimi. |
   | `-vip` |Giriş veri dosyasını. |
   | `-vof` |Çıktı biçimi. Bu örnek, kimlik ve düz metin olarak değeri. |
   | `-op` |Çıktı konumu. |
   | `-w 2` |Kullanmak için çalışan sayısı. Bu örnekte, 2. |

    Bu ve Giraph örnekleri ile kullanılan diğer parametreleri hakkında daha fazla bilgi için bkz: [Giraph quickstart](http://giraph.apache.org/quick_start.html).

6. İş tamamlandıktan sonra sonuçları depolanmış **/example/out/shotestpaths** dizini. Çıktı dosyası adları ile başlayan **bölümü-m -** ve birinci, ikinci, vb. dosya belirten bir sayı ile bitmelidir. Çıktı görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -text /example/output/shortestpaths/*
    ```

    Çıktı aşağıdakine benzer görünmelidir:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    Örnek başlamak kodlanmış sabit SimpleShortestPathComputation kimliği 1 nesne ve diğer nesnelere en kısa yolu bulunamadı. Çıktı biçiminde olduğunu `destination_id` ve `distance`. `distance` Nesne kimliği 1 ve hedef kimliği arasında seyahat kenarları değeri (veya ağırlık)

    Bu veri görselleştirme, kısa yolları kimliği 1 ile tüm diğer nesnelerin arasında seyahat sonuçları doğrulayabilirsiniz. Kimliği 1 ve kimliği 4 arasındaki en kısa yolu 5'tir. Bu değer arasındaki toplam uzaklığı: <span style="color:orange">kimliği 1 ve 3</span>ve ardından <span style="color:red">kimliği 3 ve 4</span>.

    ![Nesnelerin arasında çizilmiş kısa yolları daireler olarak çizme](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Yükleme ve Hdınsight kümelerinde ton kullanma](hdinsight-hadoop-hue-linux.md).

* [Hdınsight kümelerinde Solr yükleme](hdinsight-hadoop-solr-install-linux.md).
