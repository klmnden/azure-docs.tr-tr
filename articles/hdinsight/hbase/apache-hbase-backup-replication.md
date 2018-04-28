---
title: HBase ve Phoenix yedekleme ve çoğaltma - Azure Hdınsight ayarlama | Microsoft Docs
description: Yedekleme ve çoğaltma HBase ve Phoenix için ayarlama.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: 575a6db9fd9e5ae2d1fab98192143174df3578a0
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="set-up-backup-and-replication-for-hbase-and-phoenix-on-hdinsight"></a>Yedekleme ve çoğaltma HBase ve hdınsight'ta Phoenix için ayarlama

HBase, veri kaybına karşı kullanılan koruyarak için çeşitli yaklaşımlar destekler:

* Kopya `hbase` klasörü
* Ardından alma dışarı aktarma
* Kopya tabloları
* Anlık Görüntüler
* Çoğaltma

> [!NOTE]
> Apache Phoenix, meta verilerini HBase tabloları depolar HBase Sistem kataloğu tablolarını yedeklediğinizde meta verileri yedeklenir.

Aşağıdaki bölümlerde bu yaklaşım için kullanım senaryosu açıklanmaktadır.

## <a name="copy-the-hbase-folder"></a>Hbase klasörünü kopyalayın

Bu yaklaşımda, bir alt kümesini tablo veya sütun ailesi seçebilir olmadan tüm HBase verileri kopyalayın. Sonraki yaklaşımlar daha fazla denetim sağlar.

Hdınsight'ta HBase, Azure Storage bloblarında ya da Azure Data Lake Store küme oluştururken, seçilen varsayılan depolama kullanır. Her iki durumda da, HBase aşağıdaki yolda bulunan, veri ve meta veri dosyalarını depoladığı:

    /hbase

* Bir Azure depolama hesabındaki `hbase` klasörün bulunduğu blob kapsayıcısını kökünde:

    ```
    wasbs://<containername>@<accountname>.blob.core.windows.net/hbase
    ```

* Azure Data Lake Store'da `hbase` klasörün bulunduğu bir küme hazırlama sırasında belirttiğiniz kök yolu altında. Bu kök yolu genellikle sahip bir `clusters` Hdınsight kümenize sonra adlı bir alt klasörle:

    ```
    /clusters/<clusterName>/hbase
    ```

Her iki durumda da `hbase` klasörü HBase diske tüm verileri içerir, ancak bellek içi veri içermeyebilir. HBase verileri doğru bir temsili bu klasör güvenebilirsiniz önce kümeyi kapatmanız gerekir.

Küme sildikten sonra verileri bırakın veya verileri yeni bir konuma kopyalayın:

* Geçerli depolama konumuna işaret eden yeni bir Hdınsight örneği oluşturun. Yeni örneği var olan tüm veriler ile oluşturulur.

* Kopya `hbase` farklı bir Azure depolama alanına klasörüne blob kapsayıcı veya Data Lake Store konumu ve yeni bir küme bu verilerle başlatın. Azure depolama için kullanan [AzCopy](../../storage/common/storage-use-azcopy.md)ve Data Lake Store kullanıma [AdlCopy](../../data-lake-store/data-lake-store-copy-data-azure-storage-blob.md).

## <a name="export-then-import"></a>Ardından alma dışarı aktarma

Kaynak Hdınsight kümesinde veri kaynağı tablodan bağlı varsayılan depolama birimine dışarı aktarmak için (HBase ile dahil) verme yardımcı programını kullanın. Dışarı aktarılan klasörünü hedef depolama konumuna kopyalayın ve hedef Hdınsight kümesinde alma yardımcı programını çalıştırın.

Baş düğüme kaynak Hdınsight kümenizin ilk SSH bir tabloyu dışarı aktarmak ve ardından aşağıdaki çalıştırın `hbase` komutu:

    hbase org.apache.hadoop.hbase.mapreduce.Export "<tableName>" "/<path>/<to>/<export>"

Bir tablo SSH hedef Hdınsight kümesi baş düğümüne alıp ardından aşağıdaki komutu çalıştırarak `hbase` komutu:

    hbase org.apache.hadoop.hbase.mapreduce.Import "<tableName>" "/<path>/<to>/<export>"

Tam verme yolu varsayılan depolama veya bağlı depolama seçeneklerden birini belirtin. Örneğin, Azure Storage'da:

    wasbs://<containername>@<accountname>.blob.core.windows.net/<path>

Azure Data Lake Store'da sözdizimi aşağıdaki gibidir:

    adl://<accountName>.azuredatalakestore.net:443/<path>

Bu yaklaşım tablo düzeyi ayrıntı düzeyi sunar. Ayrıca, işlem artımlı bir şekilde gerçekleştirmenize olanak tanıyan satırları dahil etmek için bir tarih aralığı belirtebilirsiniz. Her tarihinden bu yana UNIX dönem milisaniye cinsindendir.

    hbase org.apache.hadoop.hbase.mapreduce.Export "<tableName>" "/<path>/<to>/<export>" <numberOfVersions> <startTimeInMS> <endTimeInMS>

Sürümleri vermek için her bir satır sayısını belirtmek gerektiğini unutmayın. Tüm sürümler tarih aralığında içerecek şekilde `<numberOfVersions>` en fazla olası satır sürümlerinizi 100000 gibi daha büyük bir değere.

## <a name="copy-tables"></a>Kopya tabloları

CopyTable yardımcı programı veri kaynağı tablosundan satır temelinde varolan bir hedef tablo kaynağı olarak aynı şema ile kopyalar. Hedef tablo aynı kümede veya farklı bir HBase kümesi olabilir.

CopyTable bir küme içinde SSH kaynak Hdınsight kümesi baş düğümüne kullanın ve bu çalıştırmak için `hbase` komutu:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable --new.name=<destTableName> <srcTableName>

Bir tabloda farklı bir kümeye kopyalamak için CopyTable kullanmak için add `peer` geçiş hedef kümenin adresiyle:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable --new.name=<destTableName> --peer.adr=<destinationAddress> <srcTableName>

Hedef adres aşağıdaki üç bölümden oluşur:

    <destinationAddress> = <ZooKeeperQuorum>:<Port>:<ZnodeParent>

* `<ZooKeeperQuorum>` Örneğin ZooKeeper düğümleri, virgülle ayrılmış bir listesi verilmiştir:

    zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net, zk4 hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net, zk3 hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net

* `<Port>` 2181, Hdınsight varsayılanlara üzerinde ve `<ZnodeParent>` olan `/hbase-unsecure`, bu nedenle tam `<destinationAddress>` olacaktır:

    zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net:2181:/hbase-unsecure

Bkz: [el ile ZooKeeper çekirdek listesi toplama](#manually-collect-the-zookeeper-quorum-list) Hdınsight kümeniz için bu değerleri almak nasıl hakkında ayrıntılı bilgi için bu makaledeki.

CopyTable yardımcı programını Ayrıca satır kopyalamak ve kopyalamak için bir tablodaki sütun ailesi alt belirtmek için zaman aralığı belirtmek üzere parametreler destekler. CopyTable tarafından desteklenen parametreleri tam listesini görmek için hiçbir parametre olmadan CopyTable çalıştırın:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable

CopyTable üzerinden hedef tabloya kopyalanacak tüm kaynak tablo içeriğini tarar. CopyTable çalışırken bu HBase kümenizin performansını düşürebilir.

> [!NOTE]
> Tablolar arasında veri kopyalama otomatik hale getirmek için bkz: `hdi_copy_table.sh` içindeki komut dosyası [Azure HBase yardımcı programları](https://github.com/Azure/hbase-utils/tree/master/replication) github'daki.

### <a name="manually-collect-the-zookeeper-quorum-list"></a>El ile ZooKeeper çekirdek listesi Topla

Her iki Hdınsight kümeleri daha önce açıklandığı gibi aynı sanal ağda olduğunda, iç ana bilgisayar adı çözümlemesi otomatik olarak yapılır. İki ayrı sanal ağ VPN ağ geçidi tarafından bağlı Hdınsight kümelerinde CopyTable kullanmak için ana bilgisayar IP adreslerini çekirdek Zookeeper düğüm sağlamanız gerekir.

Çekirdek ana bilgisayar adlarını almak için aşağıdaki curl komutunu çalıştırın:

    curl -u admin:<password> -X GET -H "X-Requested-By: ambari" "https://<clusterName>.azurehdinsight.net/api/v1/clusters/<clusterName>/configurations?type=hbase-site&tag=TOPOLOGY_RESOLVED" | grep "hbase.zookeeper.quorum"

HBase yapılandırma bilgilerini içeren bir JSON belgesi curl komutunu alır ve grep komutu yalnızca "hbase.zookeeper.quorum" girdisini örneğin döndürür:

    "hbase.zookeeper.quorum" : "zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net"

Çekirdek ana bilgisayar adları değeri, iki nokta üst üste sağındaki tüm dizedir.

Bu ana bilgisayarlar için IP adreslerini almak için önceki listede bulunan her bir konak için aşağıdaki curl komutunu kullanın:

    curl -u admin:<password> -X GET -H "X-Requested-By: ambari" "https://<clusterName>.azurehdinsight.net/api/v1/clusters/<clusterName>/hosts/<zookeeperHostFullName>" | grep "ip"

Bu curl komutta `<zookeeperHostFullName>` örnek gibi ZooKeeper bir ana bilgisayar tam DNS adı `zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net`. Komutunun çıktısını IP adresi için belirtilen ana bilgisayar, örneğin içerir:

    100    "ip" : "10.0.0.9",

Tüm ZooKeeper düğümleri için IP adresleri, çekirdek topladıktan sonra hedef adres yeniden oluşturun:

    <destinationAddress>  = <Host_1_IP>,<Host_2_IP>,<Host_3_IP>:<Port>:<ZnodeParent>

Bizim örneğimizde:

    <destinationAddress> = 10.0.0.9,10.0.0.8,10.0.0.12:2181:/hbase-unsecure

## <a name="snapshots"></a>Anlık Görüntüler

Anlık görüntüler bir nokta zaman içinde HBase datastore verilerin yedeğini almak etkinleştirin. Anlık görüntü en düşük ek yükleri vardır ve bir anlık görüntü işlemi verimli depolama birimindeki tüm dosyaların adlarını bu anlık yakalama meta veri işlemi olduğundan saniye içinde tamamlayın. Bir anlık görüntü zaman gerçek veri kopyalanır. Anlık görüntü HDFS burada güncelleştirmeleri, siler ve eklemeleri tüm yeni verileri olarak temsil edilir, depolanan verileri değişmez yapısını kullanır. Geri yükleyebilirsiniz (*kopya*) bir anlık görüntü aynı küme veya başka bir kümeye bir anlık görüntüyü verebilirsiniz.

Küme ve başlangıç bir anlık görüntüsü, SSH Hdınsight HBase baş düğümüne oluşturmak için `hbase` Kabuk:

    hbase shell

Hbase kabuğunu içinde anlık görüntü komutu ile tablo ve bu anlık görüntü adlarını kullanın:

    snapshot '<tableName>', '<snapshotName>'

İçinde ada göre bir anlık görüntü geri yüklemek için `hbase` Kabuk, ilk tablonun devre dışı sonra anlık görüntü geri yükleme ve tabloyu yeniden etkinleştirin:

    disable '<tableName>'
    restore_snapshot '<snapshotName>'
    enable '<tableName>'

Yeni bir tabloya bir anlık görüntü geri yüklemek için clone_snapshot kullanın:

    clone_snapshot '<snapshotName>', '<newTableName>'

Bir anlık görüntü HDFS için kullanılmak üzere başka bir küme tarafından dışarı aktarmak için daha önce açıklandığı gibi ilk anlık görüntüsünü oluşturmak ve ExportSnapshot yardımcı programını kullanın. Baş düğüm için SSH oturumundan bu yardımcı programını değil içinde çalıştırmak `hbase` Kabuk:

     hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot <snapshotName> -copy-to <hdfsHBaseLocation>

`<hdfsHBaseLocation>` Herhangi bir depolama konumu kaynak kümenize erişilebilir ve hedef küme tarafından kullanılan hbase klasöre işaret etmelidir. Örneğin, kaynak kümeye eklenen ikincil bir Azure depolama hesabı varsa ve bu hesabı hedef kümenin varsayılan depolama tarafından kullanılan kapsayıcı erişim sağlar, bu komutu kullanabilirsiniz:

    hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot 'Snapshot1' -copy-to 'wasbs://secondcluster@myaccount.blob.core.windows.net/hbase'

Anlık görüntü dışarı aktardıktan sonra restore_snapshot kullanılarak anlık görüntü geri yükleme ve hedef küme baş düğümüne içine SSH daha önce açıklandığı gibi komutu.

Anlık görüntüler bir tablonun tam bir yedekleme sırasındaki sağlamak `snapshot` komutu. Anlık görüntü özelliği artımlı anlık görüntüleri zaman windows tarafından ya da anlık eklenecek sütun aileleri kümelerine belirtmek için sağlamaz.

## <a name="replication"></a>Çoğaltma

HBase çoğaltmayı bir kaynak kümesini işlemleri kaynak kümede en az ek yükü ile zaman uyumsuz bir mekanizma kullanarak bir hedef küme için otomatik olarak iter. Hdınsight'ta, kümeler arasında çoğaltmayı ayarlayabilirsiniz burada:

* Kaynak ve hedef kümeler aynı sanal ağda ' dir.
* Kaynak ve hedefleri kümeleri farklı sanal ağlar bir VPN ağ geçidi tarafından bağlı olan, ancak her iki kümeleri aynı coğrafi konumda mevcut.
* Hedefleri kümeleri ve kaynak kümesi farklı sanal ağlar bir VPN ağ geçidi tarafından bağlı olan ve her küme farklı bir coğrafi konumda mevcut.

Çoğaltmayı ayarlama için genel adımlar şunlardır:

1. Kaynak kümede tablolar oluşturmak ve veri doldurun.
2. Hedef kümede boş hedef tablolar kaynak tablonun şeması ile oluşturun.
3. Hedef kümeyi bir eşler arası kaynak kümesi olarak kaydedin.
4. İstenen kaynak tablolarda çoğaltmayı etkinleştirin.
5. Mevcut veri kaynağı tablolardan hedef tablolara kopyalayın.
6. Çoğaltma, kaynak tabloları yeni veri değişikliklerine hedef tablolara otomatik olarak kopyalar.

Hdınsight üzerinde çoğaltmayı etkinleştirmek için çalışan kaynak Hdınsight kümenize bir betik eylemi uygulayın. Çoğaltma kümenizdeki ya da çoğaltma sanal ağları Azure kaynak yönetimi şablonları kullanılarak oluşturulan örnek kümelerde denemeniz için etkinleştirilmesi için bkz [yapılandırma HBase çoğaltmayı](apache-hbase-replication.md). Bu makale, Phoenix meta verilerin etkinleştirme yönergeleri de içerir.

## <a name="next-steps"></a>Sonraki adımlar

* [HBase çoğaltmasını yapılandırma](apache-hbase-replication.md)
