---
title: Yükleme ve HDInsight (Hadoop) - Azure Giraph kullanma
description: Betik eylemlerini kullanarak Linux tabanlı HDInsight kümelerinde Giraph yüklemeyi öğrenin. Betik eylemleri, küme oluşturma sırasında küme yapılandırmasını değiştirme veya hizmetleri ve yardımcı programları'nı yükleme özelleştirmenize olanak sağlar.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: jasonh
ms.openlocfilehash: f5d7a5587d47f7601f8dc3f65318a6b7d486f58e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953302"
---
# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a>HDInsight Hadoop kümelerinde Giraph'ı yükleyin ve büyük ölçekli grafikleri işlemek için Giraph kullanma

Bir HDInsight kümesi üzerinde Apache giraph'ı yüklemeyi öğrenin. HDInsight betik eylemi özelliği, bir bash betiğini çalıştırarak kümeniz özelleştirmenizi sağlar. Betikler, sırasında ve Küme oluşturulduktan sonra kümeleri özelleştirmek için kullanılabilir.

> [!IMPORTANT]
> Bu belgedeki adımlar, Linux kullanan bir HDInsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="whatis"></a>Giraph nedir

[Apache giraph'ı](http://giraph.apache.org/) grafik Hadoop kullanarak işleme yapmanıza olanak tanır ve Azure HDInsight ile kullanılabilir. Grafik nesneler arasındaki ilişkileri modellemek. Örneğin, büyük bir ağda bulunan yönlendiriciler arasındaki bağlantıları Internet veya sosyal ağlarda kişiler arasındaki ilişkileri gibi. Grafik işleme nedeni hakkında bir grafta nesneleri arasındaki ilişkileri gibi sağlar:

* Geçerli ilişkilerinizi tabanlı olası arkadaş tanımlayıcı.

* Bir ağda iki bilgisayar arasındaki en kısa yolu tanımlama.

* Web sayfalarının sayfası boyut hesaplanıyor.

> [!WARNING]
> HDInsight kümesi ile sağlanan bileşenler tam olarak desteklenir: Microsoft Support yalıtmak ve bu bileşenler için ilgili sorunları gidermek için yardımcı olur.
>
> Giraph gibi özel bileşenler daha fazla sorun giderme konusunda yardımcı olması için ticari açıdan makul destek alırsınız. Microsoft Support ve sorunu çözmek için mümkün olabilir. Aksi durumda, bu teknoloji için derin bir uzmanlık bulunduğu açık kaynaklı topluluklar başvurmalısınız. Örneğin, gibi kullanılan birçok topluluk siteleri vardır: [HDInsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Apache projeleri proje siteleri de [ http://apache.org ](http://apache.org), örneğin: [Hadoop](http://hadoop.apache.org/).


## <a name="what-the-script-does"></a>Betik yapar

Bu betik, aşağıdaki eylemleri gerçekleştirir:

* Giraph için yükler. `/usr/hdp/current/giraph`

* Kopya `giraph-examples.jar` kümeniz için varsayılan depolama (WASB) dosyası: `/example/jars/giraph-examples.jar`

## <a name="install"></a>Betik eylemleri kullanılarak giraph'ı yükleme

Bir HDInsight kümesi üzerinde Giraph'ı yüklemek için örnek betik, aşağıdaki konumda kullanılabilir:

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

Bu bölümde, örnek betik Azure portalını kullanarak kümeyi oluştururken kullanmak hakkında yönergeler açıklanmaktadır.

> [!NOTE]
> Betik eylemi, aşağıdaki yöntemlerden birini kullanarak uygulanabilir:
> * Azure PowerShell
> * Klasik Azure CLI
> * HDInsight .NET SDK'sı
> * Azure Resource Manager şablonları
> 
> Betik eylemleri zaten kümelerini çalıştırmak için de uygulayabilirsiniz. Daha fazla bilgi için [özelleştirme HDInsight kümeleri ile betik eylemleri](hdinsight-hadoop-customize-cluster-linux.md).

1. Adımları kullanarak bir küme oluşturmaya başlayın [oluşturma Linux tabanlı HDInsight kümeleri](hdinsight-hadoop-create-linux-clusters-portal.md), ancak oluşturma tamamlamayın.

2. İçinde **isteğe bağlı yapılandırma** bölümünden **betik eylemleri**ve aşağıdaki bilgileri sağlayın:

   * **AD**: betik eylemi için bir kolay ad girin.

   * **BETİK URI'Sİ**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

   * **HEAD**: Bu girdi denetleyin

   * **ÇALIŞAN**: Bu girdi işaretlemeden bırakın

   * **ZOOKEEPER**: Bu girdi işaretlemeden bırakın

   * **PARAMETRELERİ**: Bu alanı boş bırakın

3. Sayfanın alt kısmında **betik eylemleri**, kullanın **seçin** yapılandırmayı kaydetmek için düğme. Son olarak, **seçin** düğme alttaki **isteğe bağlı yapılandırma** isteğe bağlı yapılandırma bilgilerini kaydetmek için bölümü.

4. Açıklandığı gibi kümeyi oluştururken devam [oluşturma Linux tabanlı HDInsight kümeleri](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="usegiraph"></a>Giraph HDInsight içinde nasıl kullanabilirim?

Küme oluşturulduktan sonra Giraph dahil SimpleShortestPathsComputation örneği çalıştırmak için aşağıdaki adımları kullanın. Bu örnekte temel [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) grafikteki nesneler arasındaki en kısa yolu bulmak için uygulama.

1. SSH kullanarak HDInsight kümesine bağlanma:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Adlı bir dosya oluşturmak için aşağıdaki komutu kullanın **tiny_graph.txt**:

    ```bash
    nano tiny_graph.txt
    ```

    Bu dosyanın içeriği olarak aşağıdaki metni kullanın:

    ```text
    [0,0,[[1,1],[3,3]]]
    [1,0,[[0,1],[2,2],[3,1]]]
    [2,0,[[1,2],[4,4]]]
    [3,0,[[0,3],[1,1],[4,4]]]
    [4,0,[[3,4],[2,4]]]
    ```

    Bu veri biçimi kullanarak bir yönlü graf nesneler arasındaki ilişkiyi tanımlayan `[source_id, source_value,[[dest_id], [edge_value],...]]`. Her satırı arasında bir ilişki temsil eden bir `source_id` nesnesi ve bir veya daha fazla `dest_id` nesneleri. `edge_value` , Güç veya bağlantı uzaklığı olarak düşünülebilir `source_id` ve `dest\_id`.

    Çıkış çizilmiş ve nesneler arasındaki uzaklık değeri (veya ağırlık) kullanarak, verileri Aşağıdaki diyagramda gibi görünebilir:

    ![tiny_graph.txt arasında değişen mesafenin satırlarla bir daire olarak çizilir.](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

3. Dosyayı kaydetmek için kullanın **Ctrl + X**, ardından **Y**ve son olarak **Enter** dosya adını kabul etmek için.

4. Birincil depolama, HDInsight kümeniz içine verileri depolamak için aşağıdakileri kullanın:

    ```bash
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. Aşağıdaki komutu kullanarak SimpleShortestPathsComputation örneği çalıştırın:

    ```bash
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```

    Bu komutla birlikte kullanılan parametreler aşağıdaki tabloda açıklanmıştır:

   | Parametre | Ne yapar? |
   | --- | --- |
   | `jar` |Örnekler içeren jar dosyasını. |
   | `org.apache.giraph.GiraphRunner` |Örnekleri başlatmak için kullanılan sınıf. |
   | `org.apache.giraph.examples.SimpleShortestPathsCoputation` |Kullanılan örnek. Bu örnekte, grafikteki tüm kimlikleri kimliği 1 ila en kısa yolu hesaplar. |
   | `-ca mapred.job.tracker` |Kümenin baş düğümüne. |
   | `-vif` |Girdi verilerini kullanmaya giriş biçimi. |
   | `-vip` |Giriş veri dosyasını. |
   | `-vof` |Çıkış biçimi. Bu örnek, kodu ve düz metin olarak değeri. |
   | `-op` |Çıkış konumu. |
   | `-w 2` |Kullanmak için çalışan sayısı. Bu örnekte, 2. |

    Bu ve Giraph örnekleri ile kullanılan diğer parametreleri hakkında daha fazla bilgi için bkz. [Giraph hızlı](http://giraph.apache.org/quick_start.html).

6. İş tamamlandıktan sonra sonuçları depolanan **/example/out/shotestpaths** dizin. Çıkış dosyası adı şununla **bölümü-m -** ve birinci, ikinci, vb. dosya belirten bir sayı ile bitmelidir. Çıkışı görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -text /example/output/shortestpaths/*
    ```

    Çıktı aşağıdaki metne benzer görünür:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    Örnek sabit kodlanmış başlamak SimpleShortestPathComputation nesne kimliği 1 ve diğer nesnelerin en kısa yolu bulmak. Çıkış biçiminde olan `destination_id` ve `distance`. `distance` Nesne kimliği 1 ile hedef kimliğini arasında imkansız kenarlarına değer (veya ağırlık)

    Bu verileri görselleştirme, kısa yolları kimliği 1 ile tüm diğer nesneler arasında seyahat sonuçları doğrulayabilirsiniz. En kısa yolu kimliği 4 kimliği 1 ila 5'tir. Bu değer arasındaki toplam uzaklıktır <span style="color:orange">kimliği 1 ile 3</span>, ardından <span style="color:red">kimliği 3 ve 4</span>.

    ![Nesneler arasında çizilmiş kısa yolları daireler olarak çizme](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Yükleme ve HDInsight kümelerinde Hue kullanma](hdinsight-hadoop-hue-linux.md).

* [HDInsight kümelerinde Solr yükleme](hdinsight-hadoop-solr-install-linux.md).
